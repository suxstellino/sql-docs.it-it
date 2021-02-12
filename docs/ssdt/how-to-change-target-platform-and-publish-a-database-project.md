---
title: Modificare la piattaforma di destinazione e pubblicare un progetto di database
description: Informazioni su come modificare la piattaforma per un progetto di database di SQL Server Data Tools in un'istanza supportata di SQL Server. Scoprire come pubblicare un progetto di database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.publish.dialog
- sql.data.tools.publishdacproject
ms.assetid: 6012e120-5f72-4f4f-ae6e-f9a57ae1dea7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8a5a9cec35620f408182af3df75702eb85ce4e27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018461"
---
# <a name="how-to-change-target-platform-and-publish-a-database-project"></a>Procedura: Modificare la piattaforma di destinazione e pubblicare un progetto di database

È possibile impostare la versione di destinazione di SQL Server per il progetto di database SQL Server Data Tools (SSDT) su qualsiasi istanza supportata di SQL Server (SQL Server 2005, 2008, 2008 R2, Microsoft SQL Server 2012 o SQL Azure). In questo modo, è possibile centralizzare lo sviluppo del database in un unico progetto, ma la pubblicazione viene eseguita in più versioni di istanze di SQL Server in base alle esigenze.  
  
In SSDT, l'esecuzione di questa attività risulta semplice poiché viene presa in considerazione la piattaforma di destinazione e vengono rilevati automaticamente eventuali errori nel codice, ad esempio se si utilizzano funzionalità non supportate per un progetto che verrà pubblicato in SQL Azure.  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-change-a-projects-target-platform"></a>Per modificare la piattaforma di destinazione di un progetto  
  
1.  Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Proprietà**. Fare clic sulla scheda **Impostazioni progetto** posizionata a sinistra per accedere alla relativa pagina **delle proprietà**.  
  
2.  Nell'elenco a discesa **Piattaforma di destinazione** sono incluse tutte le piattaforme SQL Server supportate in cui è possibile pubblicare un progetto di database. Per questa procedura, selezionare **SQL Azure**.  
  
### <a name="to-use-platform-validation-when-editing-scripts"></a>Per utilizzare la convalida della piattaforma in caso di modifica di script  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella **Products** in Esplora soluzioni e selezionare **Visualizza codice** per aprirla nell'Editor Transact\-SQL.  
  
2.  Aggiungere `ON [PRIMARY]` alla fine dell'istruzione `CREATE TABLE` .  
  
3.  Si noti che nel riquadro **Elenco errori** viene visualizzato l'errore seguente: SQL70015: 'Schema di partizionamento e riferimento a filegroup' non supportato in SQL Azure.  
  
    SSDT consente di convalidare automaticamente lo script in base alla piattaforma di destinazione. In questo caso, poiché il filegroup non è supportato in SQL Azure, viene restituito un errore da SSDT. Per un elenco di istruzioni Transact\-SQL non supportate in SQL Azure, vedere [Istruzioni Transact-SQL parzialmente supportate (database SQL di Microsoft Azure)](/previous-versions/azure/ee336267(v=azure.100)).  
  
4.  Rimuovere la clausola `ON` . Si noti che l'errore scompare immediatamente dall' **Elenco errori**.  
  
### <a name="to-publish-a-database-project"></a>Per pubblicare un progetto di database  
  
1.  Se si dispone dell'accesso a un'istanza di SQL Azure, è possibile passare al passaggio successivo. In caso contrario, fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Proprietà** per accedere alla pagina delle proprietà **Impostazioni progetto**. Usare l'elenco a discesa **Piattaforma di destinazione** per selezionare la piattaforma SQL Server in cui si desidera pubblicare il progetto.  
  
2.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** in **Esplora soluzioni** e selezionare **Pubblica**. In SSDT verrà avviata la compilazione del progetto. Se non si verifica alcun errore di compilazione, verrà visualizzata la finestra di dialogo **Database di pubblicazione** .  
  
3.  Nella finestra di dialogo **Database di pubblicazione** fare clic su **Modifica** per modificare la connessione al database di destinazione.  
  
4.  Nella finestra di dialogo **Proprietà connessione** immettere il nome dell'istanza di SQL Server e le credenziali per l'autenticazione. In **Connessione al database** immettere **NewTrade**. In questo modo si tenterà di pubblicare il progetto di database in uno nuovo database. È inoltre possibile scegliere un database esistente in cui eseguire la pubblicazione. Se si sceglie il database esistente **TradeDev**, ad esempio, tutte le modifiche apportate agli oggetti (come script) nel progetto **TradeDev** offline verranno propagate nel database **TradeDev** attivo.  
  
    Se si dispone dell'autorizzazione per apportare tutte le modifiche al database in cui si desidera pubblicare, premere il pulsante **Pubblica** . Se tuttavia non di dispone dell'accesso in scrittura a un database di produzione, è possibile fare clic sul pulsante **Genera script** per produrre uno script di pubblicazione Transact\-SQL, che può essere passato a un amministratore di database. L'amministratore di database può quindi eseguire lo script per aggiornare il server di produzione in modo che lo schema sia sincronizzato con il progetto di database.  
  
5.  Nella finestra **Operazioni degli strumenti dati**  verrà visualizzato lo stato delle operazioni di pubblicazione e verranno notificati eventuali errori. Se lo si desidera, in questa nuova finestra è possibile anche scegliere di visualizzare l'anteprima della distribuzione, lo script generato o i risultati completi della pubblicazione.  
  
6.  Inoltre, è possibile salvare le impostazioni di pubblicazione in un profilo affinché sia possibile riutilizzare le stesse impostazioni per operazioni di pubblicazione future. A tale scopo, fare clic sul pulsante **Salva profilo con nome** nella finestra di dialogo **Database di pubblicazione** . In futuro, è possibile fare clic sul pulsante **Carica profilo** quando si desidera ricaricare le impostazioni esistenti.  
  
7.  Si notino i messaggi nella finestra **Operazioni degli strumenti dati** . Fare clic sul collegamento "Visualizza anteprima" a destra di **Creazione anteprima di pubblicazione...** In tal modo verrà aperta l'anteprima report della distribuzione. Se la piattaforma di destinazione del progetto non è identica al server di database in cui viene pubblicato il progetto, SSDT genererà un avviso in questo report.  Se, ad esempio, la piattaforma di destinazione del progetto è Microsoft SQL Server 2012 e si prova a pubblicare il progetto in un'istanza del server SQL Server 2008 R2, verrà visualizzato il seguente avviso nella finestra di **output**:  
  
**Un progetto in cui si specifica Microsoft SQL Server 2012 come piattaforma di destinazione può riscontrare problemi di compatibilità con SQL Server 2008**    Se in tale progetto sono incluse entità (ad esempio, un oggetto Sequence) introdotte in Microsoft SQL Server 2012, l'operazione di pubblicazione non verrà completata.  
  
La distribuzione avrà esito negativo se un predicato di oggetto usa **CONTAINS** o **FREETEXT** su un indice full-text appena creato e se vengono usati script transazionali. Se durante la distribuzione l'opzione per includere script transazionali è abilitata, le procedure e le viste vengono definite in una transazione, mentre un indice full-text viene definito all'esterno di una transazione al termine dello script di distribuzione. A causa di questi ordinamenti nello script, le procedure o le viste che utilizzano CONTAINS o FREETEXT non verranno risolte rispetto all'indice full-text e si verificherà un errore nella distribuzione.  
