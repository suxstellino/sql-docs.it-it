---
title: Caricare dati in SQL Server o nel database SQL di Azure con SQL Server Integration Services (SSIS) | Microsoft Docs
description: Questo articolo illustra come creare un pacchetto di SQL Server Integration Services (SSIS) per spostare dati da una vasta gamma di origini dati a SQL Server o al database SQL di Azure.
documentationcenter: NA
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.custom: loading
ms.date: 08/20/2018
ms.author: chugu
author: chugugrace
ms.openlocfilehash: 4bdd992e4cea1f2c869f3669905709f91727c874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074187"
---
# <a name="load-data-into-sql-server-or-azure-sql-database-with-sql-server-integration-services-ssis"></a>Caricare dati in SQL Server o nel database SQL di Azure con SQL Server Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/ssis-appliesto-ssvrpluslinux-asdb-xxxx-xxx.md)]

Creare un pacchetto di SQL Server Integration Services (SSIS) per caricare dati in SQL Server o nel [database SQL di Azure](/azure/sql-database/). È anche possibile ristrutturare, trasformare e pulire i dati durante il passaggio attraverso il flusso di dati SSIS.

Questo articolo illustra come eseguire le operazioni seguenti:

* Creare un nuovo progetto di Integration Services in Visual Studio.
* Progettare un pacchetto SSIS che carica i dati dall'origine nella destinazione.
* Eseguire il pacchetto SSIS per caricare i dati.

## <a name="basic-concepts"></a>Concetti fondamentali

Il pacchetto è l'unità di lavoro di base in SSIS. I pacchetti correlati vengono raggruppati in progetti. I progetti e pacchetti di progettazione in Visual Studio vengono creati con SQL Server Data Tools. Il processo di progettazione è un processo visivo in cui l'utente trascina e rilascia i componenti dalla casella degli strumenti all'area di progettazione, li connette e ne imposta le proprietà. Dopo aver completato il pacchetto, è possibile eseguirlo e distribuirlo facoltativamente in SQL Server o nel database SQL per usufruire di strumenti completi per gestione, monitoraggio e sicurezza.

Un'introduzione dettagliata a SSIS esula dagli scopi di questo articolo. Per altre informazioni, vedere gli articoli seguenti:

- [SQL Server Integration Services](sql-server-integration-services.md)

- [Come creare un pacchetto ETL](ssis-how-to-create-an-etl-package.md)

## <a name="about-the-solution"></a>Informazioni sulla soluzione

La soluzione è un pacchetto tipico che usa un'attività Flusso di dati che contiene un'origine e una destinazione. Questo approccio supporta un'ampia gamma di origini dati, inclusi SQL Server e il database SQL di Azure.

Questa esercitazione usa SQL Server come origine dati. SQL Server può essere eseguito in locale o in una macchina virtuale di Azure.

Per connettersi a SQL Server e al database SQL, è possibile usare una gestione connessione ADO.NET e un'origine e una destinazione oppure una gestione connessione OLE DB e un'origine e una destinazione. Questa esercitazione usa ADO NET perché prevede il minor numero di opzioni di configurazione. OLE DB può offrire prestazioni leggermente migliori rispetto ad ADO NET.

Per velocizzare l'operazione, è possibile usare Importazione/Esportazione guidata SQL Server per creare il pacchetto di base. Quindi, salvare il pacchetto e aprirlo in Visual Studio o SSDT per visualizzarlo e personalizzarlo. Per altre informazioni, vedere [Importare ed esportare dati con l'Importazione/Esportazione guidata SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

## <a name="prerequisites"></a>Prerequisites
Per eseguire questa esercitazione, sono necessari:

1. **SQL Server Integration Services (SSIS)** . SSIS è un componente di SQL Server e richiede una versione con licenza o la versione per sviluppatori o la versione di valutazione di SQL Server. Per ottenere una versione di valutazione di SQL Server, vedere [Evaluation Center per SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm).
2. **Visual Studio** (facoltativo). Per ottenere l'edizione gratuita di Visual Studio Community, vedere [Visual Studio Community][Visual Studio Community]. Se non si vuole installare Visual Studio, è possibile installare solo SQL Server Data Tools (SSDT). SSDT installa una versione di Visual Studio con funzionalità limitate.
3. **SQL Server Data Tools per Visual Studio (SSDT)** . Per ottenere SQL Server Data Tools per Visual Studio, vedere [Scaricare SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. In questa esercitazione ci si connette a un'istanza di SQL Server o del database SQL e si caricano i dati. È necessario avere le autorizzazioni per connettersi, creare una tabella e caricare i dati nelle seguenti destinazioni:
   - **Un database SQL di Azure**. Per altre informazioni, vedere [Database SQL di Azure](/azure/sql-database/).  
      o
   - **Un'istanza di SQL Server**. SQL Server può essere eseguito in locale o in una macchina virtuale di Azure. Per scaricare una versione di valutazione gratuita o un'edizione per sviluppatori di SQL Server, vedere [Download di SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

5. **Dati di esempio**. Questa esercitazione usa come dati di origine i dati di esempio archiviati in SQL Server, nel database di esempio AdventureWorks. Per ottenere il database di esempio AdventureWorks, vedere [Database di esempio AdventureWorks][AdventureWorks 2014 Sample Databases].
6. **Una regola del firewall** se si caricano dati nel database SQL. È necessario creare una regola del firewall nel database SQL con l'indirizzo IP del computer locale prima di poter caricare dati nel database SQL.

## <a name="create-a-new-integration-services-project"></a>Creazione di un nuovo progetto di Integration Services
1. Avviare Visual Studio.
2. Scegliere **Nuovo progetto** dal menu **File**.
3. Andare ai tipi di progetto **Installati | Progetti | Business Intelligence | Integration Services**.
4. Selezionare **Progetto di Integration Services**. Specificare i valori per **Nome** e **Percorso** e quindi selezionare **OK**.

Viene aperto Visual Studio e viene creato un nuovo progetto di Integration Services (SSIS). Visual Studio apre quindi la finestra di progettazione per il singolo nuovo pacchetto SSIS (package.dtsx) nel progetto. Vengono visualizzate le aree della schermata seguenti:

* A sinistra, la **Casella degli strumenti** dei componenti SSIS.
* Al centro, l'area di progettazione con più schede. In genere si usano almeno le schede **Flusso di controllo** e il **Flusso di dati**.
* A destra, i riquadri **Esplora soluzioni** e **Proprietà**.
  
    ![Screenshot di Visual Studio che mostra il riquadro Casella degli strumenti, il riquadro Progettazione, il riquadro Esplora soluzioni e il riquadro Proprietà.][01]

## <a name="create-the-basic-data-flow"></a>Creare il flusso di dati di base
1. Trascinare un'attività Flusso di dati dalla casella degli strumenti al centro dell'area di progettazione (nella scheda **Flusso di controllo**).
   
    ![Screenshot di Visual Studio che mostra un'attività Flusso di dati trascinata nella scheda Flusso di controllo del riquadro Progettazione.][02]
2. Fare doppio clic sull'attività Flusso di dati per passare alla scheda Flusso di dati.
3. Dall'elenco Altre origini nella casella degli strumenti trascinare un'origine ADO.NET nell'area di progettazione. Con l'adattatore di origine ancora selezionato, modificare il nome su **Origine SQL Server** nel riquadro **Proprietà**.
4. Dall'elenco Altre destinazioni nella casella degli strumenti,trascinare una destinazione ADO.NET all'area di progettazione sotto l'origine ADO.NET. Con l'adattatore di destinazione ancora selezionato, modificare il nome in **Destinazione SQL** nel riquadro **Proprietà**.
   
    ![Screenshot di un adattatore di destinazione trascinato in una posizione direttamente sotto l'adattatore di origine.][09]

## <a name="configure-the-source-adapter"></a>Configurare l'adattatore di origine
1. Fare doppio clic sull'adattatore di origine per aprire l'**Editor origine ADO.NET**.
   
    ![Screenshot dell'Editor origine ADO.NET. La scheda Gestione connessioni è visibile e sono disponibili controlli per la configurazione delle proprietà del flusso di dati.][03]
2. Nella scheda **Gestione connessione** della finestra **Editor origine ADO.NET** fare clic sul pulsante **Nuovo** accanto all'elenco **Gestione connessione ADO.NET** per aprire la finestra di dialogo **Configura gestione connessione ADO.NET** e creare le impostazioni di connessione per il database di SQL Server da cui questa esercitazione carica i dati.
   
    ![Screenshot della finestra di dialogo Configura gestione connessione ADO.NET. Sono disponibili controlli per l'installazione e la configurazione dei gestori connessioni.][04]
3. Nella finestra di dialogo **Configura gestione connessione ADO.NET** fare clic sul pulsante **Nuovo** per aprire la finestra di dialogo **Gestione connessione** e creare una nuova connessione dati.
   
    ![Screenshot della finestra di dialogo Gestione connessione. Sono disponibili controlli per la configurazione di una connessione dati.][05]
4. Nella finestra di dialogo **Gestione connessione** eseguire le operazioni seguenti.
   
   1. Per **Provider** selezionare il provider di dati SqlClient.
   2. Per **Nome server** immettere il nome di SQL Server.
   3. Nella sezione **Accesso al server** selezionare o immettere le informazioni di autenticazione.
   4. Nella sezione **Connessione a un database** selezionare il database di esempio AdventureWorks.
   5. Fare clic su **Test connessione**.
      
       ![Screenshot di una finestra di dialogo in cui sono visualizzati un pulsante OK e un testo che indica che la connessione di test è riuscita.][06]
   6. Nella finestra di dialogo che indica i risultati del test di connessione fare clic su **OK** per tornare alla finestra di dialogo **Gestione connessione**.
   7. Nella finestra di dialogo **Gestione connessione** fare clic su **OK** per ritornare alla finestra di dialogo **Configura gestione connessione ADO.NET**.
5. Nella finestra di dialogo **Configura gestione connessione ADO.NET** fare clic su **OK** per ritornare all'**Editor origine ADO.NET**.
6. Nell'**Editor origine ADO.NET** selezionare la tabella **Sales.SalesOrderDetail** nell'elenco **Nome tabella o vista**.
   
    ![Screenshot dell'Editor origine ADO.NET. Nell'elenco Nome tabella o vista è selezionata la tabella Sales.SalesOrderDetail.][07]
7. Fare clic su **Anteprima** per visualizzare le prime 200 righe di dati nella tabella di origine nella finestra di dialogo **Anteprima risultati query**.
   
    ![Screenshot della finestra di dialogo Anteprima risultati query. Sono visibili diverse righe di dati di vendita della tabella di origine.][08]
8. Nella finestra di dialogo **Anteprima risultati query** fare clic su **Chiudi** per ritornare all'**Editor origine ADO.NET**.
9. Nell'**Editor origine ADO.NET** fare clic su **OK** per completare la configurazione dell'origine dati.

## <a name="connect-the-source-adapter-to-the-destination-adapter"></a>Connettere l'adattatore di origine all'adattatore di destinazione
1. Selezionare l'adattatore di origine nell'area di progettazione.
2. Selezionare la freccia blu che si estende dall'adattatore di origine e trascinarla nell'editor di destinazione fino al completo inserimento.
   
    ![Screenshot che mostra gli adattatori di origine e di destinazione. Una freccia blu collega l'adattatore di origine all'adattatore di destinazione.][10]
   
    In un pacchetto SSIS tipico è possibile usare diversi altri componenti dalla casella degli strumenti SSIS tra l'origine e la destinazione per ristrutturare, trasformare e pulire i dati mentre attraversano il flusso di dati SSIS. Per mantenere questo esempio il più semplice possibile, viene eseguita la connessione dell'origine direttamente alla destinazione.

## <a name="configure-the-destination-adapter"></a>Configurare l'adattatore di destinazione
1. Fare doppio clic sull'adattatore di destinazione per aprire l'**Editor destinazione ADO.NET**.
   
    ![Screenshot dell'Editor destinazione ADO.NET. La scheda Gestione connessione è visibile e contiene controlli per la configurazione delle proprietà del flusso di dati.][11]
2. Nella scheda **Gestione connessione** della finestra **Editor destinazione ADO.NET** fare clic sul pulsante **Nuovo** accanto all'elenco **Gestione connessione** per aprire la finestra di dialogo **Configura gestione connessione ADO.NET** e creare le impostazioni di connessione per il database in cui questa esercitazione carica i dati.
3. Nella finestra di dialogo **Configura gestione connessione ADO.NET** fare clic sul pulsante **Nuovo** per aprire la finestra di dialogo **Gestione connessione** e creare una nuova connessione dati.
4. Nella finestra di dialogo **Gestione connessione** eseguire le operazioni seguenti.
   1. Per **Provider** selezionare il provider di dati SqlClient.
   2. In **Nome server** immettere il nome del server SQL Server o del server di database SQL.
   3. Nella sezione **Accesso al server** selezionare **Usa autenticazione di SQL Server** e immettere le informazioni di autenticazione.
   4. Nella sezione **Connessione al database** selezionare un database SQL esistente.
    a. Fare clic su **Test connessione**.
    b. Nella finestra di dialogo che indica i risultati del test di connessione fare clic su **OK** per tornare alla finestra di dialogo **Gestione connessione**.
    c. Nella finestra di dialogo **Gestione connessione** fare clic su **OK** per ritornare alla finestra di dialogo **Configura gestione connessione ADO.NET**.
5. Nella finestra di dialogo **Configura gestione connessione ADO.NET** fare clic su **OK** per ritornare all'**Editor destinazione ADO.NET**.
6. Nell'**Editor destinazione ADO.NET** fare clic su **Nuovo** accanto all'elenco **Tabella o vista** per visualizzare la finestra di dialogo **Crea tabella** per creare una nuova tabella di destinazione con un elenco di colonne che corrisponde alla tabella di origine.
   
    ![Screenshot della finestra di dialogo Crea tabella. È visibile il codice SQL per la creazione di una tabella di destinazione.][12a]
7. Nella finestra di dialogo **Crea tabella** eseguire le operazioni seguenti.
   
   1. Modificare il nome della tabella di destinazione su **SalesOrderDetail**.
      
       ![Screenshot della finestra di dialogo Crea tabella. È visibile il codice SQL per la creazione di una tabella denominata SalesOrderDetail.][12b]

   2. Fare clic su **OK** per creare la tabella e ritornare all'**Editor destinazione ADO.NET**.
8. Nell'**Editor destinazione ADO.NET** selezionare la scheda **Mapping** per visualizzare come le colonne nell'origine vengono mappate alle colonne nella destinazione.
   
    ![Screenshot della scheda Mapping dell'Editor destinazione ADO.NET. Le linee collegano le colonne con nomi identici nelle tabelle di origine e di destinazione.][13]
9. Fare clic su **OK** per completare la configurazione della destinazione.

## <a name="run-the-package-to-load-the-data"></a>Eseguire il pacchetto per caricare i dati
Eseguire il pacchetto facendo clic sul pulsante **Avvia** della barra degli strumenti o selezionando una delle opzioni **Esegui** nel menu **Debug**.

I paragrafi seguenti descrivono i risultati visualizzati se si crea il pacchetto con la seconda opzione descritta in questo articolo, vale a dire con un flusso di dati che contiene un'origine e una destinazione.

Quando inizia l'esecuzione del pacchetto si possono osservare delle ruote gialle in rotazione che indicano l'attività e il numero di righe elaborate fino a ora.

![Screenshot che mostra gli adattatori di origine e di destinazione. Sopra ogni adattatore si trovano ruote gialle in rotazione e tra di essi è presente il testo "89748 righe".][14]

Quando l'esecuzione del pacchetto è terminata vengono visualizzati dei segni di spunta verdi che indicano l'esito positivo e il numero totale di righe di dati caricati dall'origine alla destinazione.

![Screenshot che mostra gli adattatori di origine e di destinazione. Sopra ogni adattatore si trovano segni di spunta verdi e tra di essi è presente il testo "121317 righe".][15]

Congratulazioni! SQL Server Integration Services è stato usato per caricare dati nel database SQL Server o nel database SQL di Azure.

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come eseguire il debug e risolvere i problemi relativi ai pacchetti direttamente nell'ambiente di progettazione. Iniziare da: [Strumenti per la risoluzione dei problemi relativi allo sviluppo dei pacchetti][Troubleshooting Tools for Package Development].

- Informazioni su come distribuire i progetti SSIS e i pacchetti nel server Integration Services o in un'altra posizione di archiviazione. Iniziare da: [Distribuzione di progetti e pacchetti][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/load-data-to-sql-database-with-ssis/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-database-with-ssis/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-database-with-ssis/test-connection-06.png
[07]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-database-with-ssis/preview-data-08.png
[09]:  ./media/load-data-to-sql-database-with-ssis/source-destination-09.png
[10]:  ./media/load-data-to-sql-database-with-ssis/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-database-with-ssis/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-database-with-ssis/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-database-with-ssis/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-database-with-ssis/column-mapping-13.png
[14]:  ./media/load-data-to-sql-database-with-ssis/package-running-14.png
[15]:  ./media/load-data-to-sql-database-with-ssis/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]: https://www.microsoft.com/download/details.aspx?id=54798
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2017
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
