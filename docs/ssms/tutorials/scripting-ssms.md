---
title: Oggetti dello script SSMS
description: Generare script Transact-SQL (T-SQL) per vari oggetti presenti in SQL Server Management Studio (SSMS).
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio, Script, Scripting
helpviewer_keywords:
- projects [SQL Server Management Studio], tutorials
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- solutions [SQL Server Management Studio], tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/13/2018
ms.openlocfilehash: 2607b029389d0a0d3f4b59e55e031b768baf4aac
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597247"
---
# <a name="script-objects-in-sql-server-management-studio"></a>Creare script per oggetti in SQL Server Management Studio

In questa esercitazione viene illustrato come generare script Transact-SQL (T-SQL) per vari oggetti presenti in SQL Server Management Studio (SSMS). In questa esercitazione è possibile trovare esempi di come creare script per gli oggetti seguenti:

> [!div class="checklist"]
> * Query, quando si eseguono azioni all'interno della GUI
> * Database in due modalità diverse (Script come e Genera Script)
> * Tabelle
> * Stored procedure
> * Eventi estesi

Per creare lo script per un oggetto in **Esplora oggetti**, fare clic con il pulsante destro del mouse su di esso e scegliere l'opzione **Script Object As** (Crea script per oggetto). Questa esercitazione illustra il processo.

## <a name="prerequisites"></a>Prerequisites

Per completare questa esercitazione, sono necessari SQL Server Management Studio, l'accesso a un server che esegue SQL Server e un database AdventureWorks.

* Installare [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* Scaricare i [database di esempio AdventureWorks2016](https://github.com/Microsoft/sql-server-samples/releases).

Le istruzioni per il ripristino dei database in SSMS sono disponibili in [Ripristinare un database](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="script-queries-from-the-gui"></a>Creare script di query dalla GUI

È possibile generare il codice T-SQL associato per un'attività quando si usa la GUI in SSMS per completarla. Negli esempi seguenti viene illustrato come effettuare questa operazione quando si esegue il backup di un database e quando si compatta il log delle transazioni. Questi stessi passaggi possono essere applicati a qualsiasi azione che viene completata tramite la GUI.

### <a name="script-t-sql-when-you-back-up-a-database"></a>Generare script T-SQL quando si esegue il backup di un database

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo di **Database** .

3. Fare clic con il pulsante destro del mouse sul database **Adventureworks2016** > **Attività** > **Backup**:

    ![Eseguire il backup del database](media/scripting-ssms/backupdb.png)

4. Configurare il backup in base alle preferenze. Per questa esercitazione, sono stati lasciati tutti i valori predefiniti. Tutte le modifiche apportate nella finestra si riflettono tuttavia nello script. 

5. Selezionare **Script** > **Genera script azione in nuova finestra Query**:

    ![Generare lo script per il backup del database - Generare lo script dell'azione](media/scripting-ssms/scriptdbbackup.PNG)
6. Esaminare il codice T-SQL compilato nella finestra della query.

    ![Generare lo script per il backup del database - Esaminare T-SQL](media/scripting-ssms/dbbackupscript.PNG)
7. Selezionare **Esegui** per eseguire la query per il backup del database tramite T-SQL. 

### <a name="script-t-sql-when-you-shrink-the-transaction-log"></a>Generare script T-SQL quando si compatta il log delle transazioni

1. Fare clic con il pulsante destro del mouse sul database **AdventureWorks2016** > **Attività** > **Compatta** > **File**:

     ![Compattare file](media/scripting-ssms/shrinkfiles.png)

2. Selezionare **Log** nell'elenco a discesa **Tipo file**:

    ![Compattare il log delle transazioni](media/scripting-ssms/shrinktlog.png)

3. Selezionare **Script** e quindi **Genera script azione negli Appunti**:

    ![Genera script negli Appunti](media/scripting-ssms/scriptactiontoclipboard.png)

4. Aprire una finestra **Nuova query** e incollare. Fare clic con il pulsante destro del mouse nella finestra, quindi scegliere **Incolla**.

    ![Incollare lo script](media/scripting-ssms/paste.png)

5. Selezionare **Esegui** per eseguire la query e compattare il log delle transazioni.

## <a name="script-databases"></a>Generare script di database

Nella sezione seguente viene illustrato come generare uno script per un database usando le opzioni **Script come** e **Genera script**. L'opzione **Script come** ricrea il database e le relative opzioni di configurazione. È possibile generare lo script sia per lo schema che per i dati usando l'opzione **Genera script**. In questa sezione si creano due nuovi database. Si usa l'opzione **Script come** per creare *AdventureWorks2016a*. Si usa l'opzione **Genera script** per creare *AdventureWorks2016b*.

### <a name="script-a-database-by-using-the-script-option"></a>Generare lo script di un database usando l'opzione Script

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo di **Database** .

3. Fare clic con il pulsante destro del mouse sul database **AdventureWorks2016** > **Crea script per database** > **Genera codice per istruzione CREATE in** > **Nuova finestra editor di query**:

    ![Creare lo script per il database](media/scripting-ssms/scriptdb.png)

4. Esaminare la query di creazione del database nella finestra:

    ![Database inserito nello script](media/scripting-ssms/scriptedoutdb.png) Questa opzione inserisce nello script solo le opzioni di configurazione del database.

5. Premere CTRL+F per aprire la finestra di dialogo **Trova**. Selezionare la freccia verso il basso per aprire l'opzione **Sostituzione**. Nella riga in alto della finestra **Trova** digitare AdventureWorks2016 e nella riga inferiore **Sostituzione** digitare AdventureWorks2016a.

6. Selezionare **Replace All** (Sostituisci tutto) per sostituire tutte le istanze di *AdventureWorks2016* con *AdventureWorks2016a*. 

    ![Ricerca e sostituzione](media/scripting-ssms/findandreplace.png)

7. Selezionare **Esegui** per eseguire la query e creare un nuovo database AdventureWorks2016a. 

### <a name="script-a-database-by-using-the-generate-scripts-option"></a>Generare lo script di un database usando l'opzione Genera script

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo di **Database** .

3. Fare clic con il pulsante destro del mouse su **AdventureWorks2016** > **Attività** > **Genera script**:

    ![Generare script per i database](media/scripting-ssms/generatescriptsfordb.png)

4. Si apre la pagina **Introduzione**. Selezionare **Avanti** per aprire la pagina **Seleziona oggetti**. È possibile selezionare l'intero database o oggetti specifici del database. Selezionare **Genera script per l'intero database e tutti gli oggetti di database**.

    ![Generare script per oggetti](media/scripting-ssms/scriptobjects.png)

5. Selezionare **Avanti** per aprire la pagina **Imposta opzioni di generazione script**, in cui è possibile configurare la posizione in cui salvare lo script e alcune altre opzioni avanzate. 

    a. Selezionare **Salva in una nuova finestra Query**.

    b. Selezionare **Avanzate** e verificare che le opzioni seguenti siano impostate come segue:

      * **Script per statistiche** impostato su *Genera script per statistiche*.
      * **Tipi di dati per cui generare lo script** impostato su *Solo schema*.
      * **Script per indici** impostato su *True*.

   ![Oggetti dello script](media/scripting-ssms/advancedscripts.png)

   > [!NOTE]
   > È possibile creare lo script di dati per il database quando si seleziona *Schema e dati* per l'opzione **Tipi di dati per cui generare lo script**. Non è tuttavia l'ideale con database di grandi dimensioni perché può richiedere più memoria di quanta ne possa essere allocata da SSMS. Questa limitazione è accettabile per i database di piccole dimensioni. Per spostare i dati per un database di dimensioni maggiori, usare l[Importazione/Esportazione guidata](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

6. Selezionare **OK**, quindi **Avanti**.

7. Selezionare **Avanti** nel **riepilogo**, quindi selezionare nuovamente **Avanti** per generare lo script in una finestra **Nuova query**.

8. Aprire la finestra di dialogo **Trova** premendo CTRL+F. Selezionare la freccia verso il basso per aprire l'opzione **Sostituzione**. Nella riga superiore **Trova** immettere *AdventureWorks2016*. Nella riga inferiore **Sostituisci** immettere *AdventureWorks2016b*.

9. Selezionare **Replace All** (Sostituisci tutto) per sostituire tutte le istanze di *AdventureWorks2016* con *AdventureWorks2016b*.

    ![AdventureWorks 2016](media/scripting-ssms/adventureworks2016b.png)

10. Selezionare **Esegui** per eseguire la query e creare un nuovo database AdventureWorks2016b.

## <a name="script-tables"></a>Generare script per tabelle

In questa sezione viene illustrato come generare script di tabelle dal database. Usare questa opzione per creare la tabella o eliminare e creare la tabella. È anche possibile usare questa opzione per creare lo script T-SQL associato modificando la tabella. È ad esempio possibile inserirla o aggiornarla al suo interno. In questa sezione viene eliminata e ricreata una tabella. 

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo **Database**.

3. Espandere il nodo del database **AdventureWorks2016**. 

4. Espandere il nodo **Tabelle**.

5. Fare clic con il pulsante destro del mouse su **dbo.ErrorLog** > **Crea script per tabella** > **Genera codice per istruzioni DROP e CREATE in** > **Nuova finestra editor di query**:

    ![Creare script per tabella](media/scripting-ssms/scripttable.png)

6. Selezionare **Esegui** per eseguire la query. Questa azione elimina la tabella *Errorlog* e la ricrea. 

    >[!NOTE]
    > La tabella *Errorlog* è vuota per impostazione predefinita nel database AdventureWorks2016. Eliminando la tabella, non verrà quindi perso alcun dato. Tuttavia, se si segue la procedura con una tabella contenente dati, questi vanno persi.

## <a name="script-stored-procedures"></a>Generare script per stored procedure

In questa sezione viene illustrato come eliminare e creare una stored procedure.  

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo **Database**.

3. Espandere il nodo **Programmabilità**. 

4. Espandere il nodo **Stored procedure**.

5. Fare clic con il pulsante destro del mouse sulla stored procedure **dbo.uspGetBillOfMaterials** > **Crea script per stored procedure** > **Genera codice per istruzioni DROP e CREATE in** > **Nuova finestra editor di query**:

    ![Generare script per stored procedure](media/scripting-ssms/script-stored-procedure.png)

## <a name="script-extended-events"></a>Generare script per eventi estesi

In questa sezione viene illustrato come inserire nello script gli [eventi estesi](../../relational-databases/extended-events/extended-events.md).

1. Connettersi a un server che esegue SQL Server.

2. Espandere il nodo **Gestione**.

3. Espandere il nodo **Eventi estesi**.

4. Espandere il nodo **Sessioni**.

5. Fare clic con il pulsante destro del mouse sulla sessione estesa di interesse in > **Crea script per sessione** > **Genera codice per istruzione CREATE in** > **Nuova finestra editor di query**:

    ![Sessione estesa di Nuova finestra editor di query](media/scripting-ssms/scriptxevents.png)

6. In **Nuova finestra editor di query** modificare il nuovo nome della sessione da *system_health* a *system_health2*. Selezionare **Esegui** per eseguire la query.

7. Fare clic con il pulsante destro del mouse su **Sessioni** in **Esplora oggetti**. Selezionare **Aggiorna** per visualizzare la nuova sessione di eventi estesi. L'icona verde accanto alla sessione indica che la sessione è in esecuzione. L'icona rossa indica che la sessione è stata arrestata.

    ![Nuova sessione di eventi estesi](media/scripting-ssms/newxevent.png)

    >[!NOTE]
    > È possibile avviare la sessione selezionandola con il pulsante destro del mouse e scegliendo **Avvia**. Si tratta tuttavia di una copia della sessione **system_health** già in esecuzione, quindi è possibile ignorare questo passaggio. È possibile eliminare la copia della sessione di eventi estesi: fare clic con il pulsante destro del mouse su di essa e scegliere **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

Il modo migliore per acquisire familiarità con SSMS è la pratica diretta. Questa *esercitazione* e questi articoli di *procedure* sono utili per varie funzionalità disponibili in SSMS. Questi articoli illustrano come gestire i componenti di SSMS e individuare le funzionalità usate regolarmente.

* [Connettersi ed eseguire query su un'istanza](../quickstarts/ssms-connect-query-sql-server.md)
* [Uso di modelli in SSMS](../template/templates-ssms.md)
* [Configurazione di SSMS](ssms-configuration.md)
* [Suggerimenti e consigli per l'uso di SSMS](ssms-tricks.md)