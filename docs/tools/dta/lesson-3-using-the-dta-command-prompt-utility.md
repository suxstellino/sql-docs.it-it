---
title: Uso dell'utilità del prompt dei comandi dta
description: Informazioni sulle funzionalità offerte dall'utilità del prompt dei comandi dta, oltre a quelle fornite da Ottimizzazione guidata motore di database di SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 30f27f4d-8852-4b12-ba62-57f63e496f1d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 064d7fc2d47cbd4f688d113f60be6080ca90fee1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642600"
---
# <a name="lesson-3-using-the-dta-command-prompt-utility"></a>Lezione 3: Uso dell'utilità del prompt dei comandi dta
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
L'utilità del prompt dei comandi **dta** offre una funzionalità aggiuntiva rispetto allo strumento Ottimizzazione guidata motore di database.  
  
È possibile utilizzare gli strumenti XML preferiti per creare file di input per l'utilità adottando XML Schema di Ottimizzazione guidata motore di database. Questo schema viene installato con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed è disponibile in: C:\Programmi (x86)\Microsoft SQL Server\110\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd.  
  
XML Schema di Ottimizzazione guidata motore di database è inoltre disponibile online nel [sito Web Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
XML Schema dello strumento Ottimizzazione guidata motore di database garantisce una maggiore flessibilità per l'impostazione delle opzioni di ottimizzazione. Ad esempio, questo schema consente di eseguire analisi di simulazione. Nell'analisi di simulazione è necessario specificare un set di strutture di progettazione fisica ipotetica per il database che si desidera ottimizzare e quindi eseguire l'analisi con lo strumento Ottimizzazione guidata motore di database per verificare se questa progettazione fisica ipotetica sarà in grado di migliorare le prestazioni dell'elaborazione delle query. Questo tipo di analisi ha il vantaggio di valutare la nuova configurazione senza l'overhead dovuto all'effettiva implementazione. Se la progettazione fisica ipotetica non determina i miglioramenti nelle prestazioni desiderati, è possibile modificarla e analizzarla nuovamente con facilità fino a quando non sarà stata raggiunta la configurazione che produce i risultati richiesti.  
  
Usando l'XML Schema dello strumento Ottimizzazione guidata motore di database e l'utilità del prompt dei comandi **dta** , è anche possibile integrare la funzionalità dello strumento Ottimizzazione guidata motore di database negli script e usarla con altri strumenti per la progettazione del database.  
  
In questa lezione non verrà illustrata la funzionalità relativa agli input in formato XML dello strumento Ottimizzazione guidata motore di database.  
  
 In questa attività vengono illustrate le procedure per avviare l'utilità **dta**, visualizzarne la Guida e quindi usare l'utilità dal prompt dei comandi per ottimizzare un carico di lavoro. Viene usato il carico di lavoro MyScript.sql creato per l'esercitazione sull'interfaccia utente grafica di Ottimizzazione guidata motore di database [Ottimizzazione di un carico di lavoro](lesson-2-using-database-engine-tuning-advisor.md#tuning-a-workload)  
  
In questa esercitazione viene usato il database di esempio AdventureWorks2017. Per motivi di sicurezza, i database di esempio non vengono installati per impostazione predefinita. Per installare i database di esempio, vedere la pagina [Installazione degli esempi e dei database di esempio di SQL Server](../../samples/adventureworks-install-configure.md).  
  
Le attività seguenti consentono di aprire un prompt dei comandi, avviare l'utilità della riga di comando **dta** , visualizzare la Guida relativa alla sintassi e ottimizzare un carico di lavoro semplice, ovvero MyScript.sql, creato in [Ottimizzazione di un carico di lavoro](./lesson-2-using-database-engine-tuning-advisor.md).  

## <a name="prerequisites"></a>Prerequisiti 

Per completare questa esercitazione, sono necessari SQL Server Management Studio, l'accesso a un server che esegue SQL Server e un database AdventureWorks.

- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare il [database di esempio AdventureWorks2017](../../samples/adventureworks-install-configure.md).


Le istruzioni per il ripristino dei database in SSMS sono disponibili in [Ripristinare un database](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Questa esercitazione è destinata agli utenti che hanno familiarità con l'uso di SQL Server Management Studio e con attività semplici di amministrazione del database. 

## <a name="access-dta-command-prompt-utility-help-menu"></a>Accedere al menu della Guida dell'utilità del prompt dei comandi di ottimizzazione guidata di database
  
  
1.  Fare clic sul pulsante **Start** , scegliere **Tutti i programmi**, **Accessori** e quindi **Prompt dei comandi**.  
  
2.  Al prompt dei comandi digitare quanto segue e premere INVIO:  
  
    ```  
    dta -? | more  
    ```  
  
    La sezione `| more` di questo comando è facoltativa. Tuttavia utilizzandola è possibile scorrere le pagine della Guida dell'utilità relative alla sintassi. Premere INVIO per visualizzare il testo della Guida riga per riga oppure premere la BARRA SPAZIATRICE per scorrere le pagine.  

  ![Uso della Guida con l'utilità cmd di ottimizzazione guidata di database](media/dta-tutorials/dta-cmd-help.png)

## <a name="tune-simple-workload-using-the-dta-command-prompt-utility"></a>Ottimizzare un carico di lavoro semplice utilizzando l'utilità del prompt dei comandi di ottimizzazione guidata di database  


  
1.  Al prompt dei comandi, passare alla directory contenente il file MyScript.sql.  
  
2.  Al prompt dei comandi, digitare quanto segue e premere INVIO per eseguire il comando e avviare la sessione di ottimizzazione (si noti che l'utilità distingue tra maiuscole e minuscole quando viene eseguita l'analisi dei comandi):  
  
    ```  
    dta -S YourServerName\YourSQLServerInstanceName -E -D AdventureWorks2012 -if MyScript.sql -s MySession2 -of MySession2OutputScript.sql -ox MySession2Output.xml -fa IDX_IV -fp NONE -fk NONE  
    ```  
  
    dove `-S` indica il nome del server e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è installato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . L'impostazione `-E` indica l'utilizzo di una connessione trusted all'istanza che risulta appropriata nel caso di una connessione con un account di dominio di Windows. L'impostazione `-D` specifica il database da ottimizzare, `-if` il file del carico di lavoro, `-s` il nome della sessione, `-of` il file sul quale verrà creato lo script delle indicazioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e `-ox` il file sul quale verranno scritte le indicazioni in formato XML. Gli ultimi tre parametri definiscono le opzioni di ottimizzazione nel modo seguente: `-fa IDX_IV` indica che in Ottimizzazione guidata motore di database devono essere aggiunti solo gli indici (cluster e non cluster) e le viste indicizzate; `-fp NONE` indica che durante l'analisi non devono essere considerate le strategie di partizionamento e `-fk NONE` indica che quando vengono generate le indicazioni in Ottimizzazione guidata motore di database non devono essere mantenute nel database le strutture di progettazione fisica esistenti.  

  ![Uso di CMD con l'ottimizzazione guidata di database](media/dta-tutorials/dta-cmd.png)
  
3.  Al termine dell'ottimizzazione del carico di lavoro, in Ottimizzazione guidata motore di database viene visualizzato un messaggio che indica che la sessione di ottimizzazione è stata completata correttamente. È possibile visualizzare i risultati dell'ottimizzazione utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per aprire i file MySession2OutputScript.sql e MySession2Output.xml. In alternativa è possibile aprire la sessione di ottimizzazione MySession2 nell'interfaccia utente grafica di Ottimizzazione guidata motore di database e visualizzare le indicazioni e i report come descritto in [Visualizzazione delle indicazioni di ottimizzazione](./lesson-2-using-database-engine-tuning-advisor.md) e [Visualizzazione dei report di ottimizzazione](./lesson-2-using-database-engine-tuning-advisor.md).  
  
 
## <a name="after-you-finish-this-tutorial"></a>Al termine di questa esercitazione  
Al termine di questa esercitazione, per ulteriori informazioni sullo strumento Ottimizzazione guidata motore di database, è possibile consultare gli argomenti seguenti:  
  
-   [Ottimizzazione guidata motore di database](../../relational-databases/performance/database-engine-tuning-advisor.md) per le descrizioni sull'uso di questo strumento in determinate attività. 
-   [Utilità dta](../../tools/dta/dta-utility.md) per materiale di riferimento sull'utilità del prompt dei comandi e sul file XML facoltativo disponibile per usare l'utilità.  
  
Per tornare all'inizio dell'esercitazione, vedere [Esercitazione: Ottimizzazione guidata motore di database](../../tools/dta/tutorial-database-engine-tuning-advisor.md).  
  
## <a name="see-also"></a>Vedere anche  
[Esercitazioni del motore di database](../../relational-databases/database-engine-tutorials.md)  
