---
title: utilità sqlmaint
description: In SQL Server usare l'utilità sqlmaint per eseguire controlli DBCC, creare un backup di un database e del relativo log delle transazioni, aggiornare le statistiche e ricompilare gli indici.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- database maintenance plans [SQL Server]
- sqlmaint utility
- maintaining databases [SQL Server]
- backups [SQL Server], sqlmaint utility
- command prompt utilities [SQL Server], sqlmaint
- maintenance plans [SQL Server], command prompt
- backing up [SQL Server], sqlmaint utility
ms.assetid: 937a9932-4aed-464b-b97a-a5acfe6a50de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c822bd2acbd92bcfd9b3008c78c3ccef244f3c88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338550"
---
# <a name="sqlmaint-utility"></a>utilità sqlmaint
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L'utilità **sqlmaint** consente di eseguire un set specifico di operazioni di manutenzione in uno o più database. Usare l'utilità **sqlmaint** per eseguire controlli DBCC, backup di un database e del relativo log delle transazioni, aggiornare statistiche e ricompilare indici. Tutte le attività di manutenzione dei database generano un report che può essere inviato a un file di testo, un file HTML o un account di posta elettronica specificato. **sqlmaint** esegue i piani di manutenzione dei database creati con le versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Per eseguire i piani di manutenzione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dal prompt dei comandi, usare l' [utilità dtexec](../integration-services/packages/dtexec-utility.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../includes/ssnotedepnextavoid-md.md)] Usare invece la funzionalità di pianificazione della manutenzione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per altre informazioni sui piani di manutenzione, vedere [Piani di manutenzione](../relational-databases/maintenance-plans/maintenance-plans.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sqlmaint   
[-?] |  
[  
     [-S server_name[\instance_name]]  
     [-U login_ID [-P password]]  
     {  
          [-D database_name | -PlanName name | -PlanID guid ]  
          [-Rpt text_file]  
          [-To operator_name]  
          [-HtmlRpt html_file [-DelHtmlRpt <time_period>] ]  
          [-RmUnusedSpace threshold_percentfree_percent]  
          [-CkDB | -CkDBNoIdx]  
          [-CkAl | -CkAlNoIdx]  
          [-CkCat]  
          [-UpdOptiStats sample_percent]  
          [-RebldIdx free_space]  
          [-SupportComputedColumn]  
          [-WriteHistory]  
          [  
               {-BkUpDB [backup_path] | -BkUpLog [backup_path] }  
               {-BkUpMedia  
                    {DISK [  
                           [-DelBkUps <time_period>]   
                           [-CrBkSubDir ]   
                           [-UseDefDir ]   
                          ]  
                     | TAPE   
                    }  
               }  
               [-BkUpOnlyIfClean]  
               [-VrfyBackup]  
          ]  
     }  
]  
<time_period> ::=  
number[minutes | hours | days | weeks | months]  
```  
  
## <a name="arguments"></a>Argomenti  
 Separare i parametri e i relativi valori con uno spazio. Inserire ad esempio uno spazio tra **-S** e *nome_server*.  
  
 **-?**  
 Specifica che deve essere restituito il diagramma della sintassi di **sqlmaint** . Questo parametro deve essere utilizzato da solo.  
  
 **-S** _server_name_[ **\\** _instance\_name_]  
 Specifica l'istanza di destinazione di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Specificare _server\_name_ per connettersi all'istanza predefinita di [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] su tale server. Specificare _server\_name_ **\\** _instance\_name_ per connettersi a un'istanza denominata di [!INCLUDE[ssDE](../includes/ssde-md.md)] su tale server. Se non si specifica alcun server, **sqlmaint** si connette all'istanza predefinita di [!INCLUDE[ssDE](../includes/ssde-md.md)] nel computer locale.  
  
 **-U** _login_ID_  
 Specifica l'ID di accesso utilizzato per la connessione al server. Se omesso, **sqlmaint** prova a usare l'autenticazione di Windows [!INCLUDE[msCoName](../includes/msconame-md.md)] . Se *login_ID* contiene caratteri speciali, il valore deve essere racchiuso tra virgolette doppie ("). In caso contrario, le virgolette doppie sono facoltative.  
  
> [!IMPORTANT]  
>  Se possibile, usare l'autenticazione di Windows.  
  
 **-P** _password_  
 Specifica la password per l'ID di accesso. È valido solo se viene indicato anche il parametro **-U** . Se *password* contiene caratteri speciali, il valore deve essere racchiuso tra virgolette doppie. In caso contrario, le virgolette doppie sono facoltative.  
  
> [!IMPORTANT]  
>  La password non è nascosta. Se possibile, usare l'autenticazione di Windows.  
  
 **-D** _database_name_  
 Specifica il nome del database nel quale eseguire l'operazione di manutenzione. Se *database_name* contiene caratteri speciali, il valore deve essere racchiuso tra virgolette doppie. In caso contrario, le virgolette doppie sono facoltative.  
  
 **-PlanName** _name_  
 Specifica il nome di un piano di manutenzione del database definito utilizzando Creazione guidata piano di manutenzione database. L'unica informazione del piano usata da **sqlmaint** è l'elenco dei database. Le attività di manutenzione specificate negli altri parametri di **sqlmaint** vengono applicate a questo elenco di database.  
  
 **-PlanID** _guid_  
 Specifica l'identificatore univoco globale (GUID, Globally Unique Identifier) del piano di manutenzione del database definito utilizzando Creazione guidata piano di manutenzione database. L'unica informazione del piano usata da **sqlmaint** è l'elenco dei database. Le attività di manutenzione specificate negli altri parametri di **sqlmaint** vengono applicate a questo elenco di database. Questo valore deve corrispondere al valore plan_id di msdb.dbo.sysdbmaintplans.  
  
 **-Rpt** _text_file_  
 Specifica il percorso completo e il nome del file in cui deve essere generato il report. Il report viene visualizzato anche sullo schermo. Il report gestisce informazioni sulla versione tramite l'aggiunta di una data al nome del file. La data viene inserita alla fine del nome file ma prima del punto, in formato _ *yyyyMMddhhmm*. *yyyy* = anno, *MM* = mese, *dd* = giorno, *hh* = ora, *mm* = minuto.  
  
 Se si esegue l'utilità alle 10.23 del 1° dicembre 1996 e il valore di *text_file* è:  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.rpt  
```  
  
 il nome del file generato è:  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint_199612011023.rpt  
```  
  
 Quando si accede a un server remoto con *sqlmaint* , è necessario specificare il nome UNC (Universal Naming Convention) completo del file per **text_file** .  
  
 **-To**  _operator_name_  
 Specifica l'operatore al quale il report generato viene inviato tramite SQL Mail.  
  
 **-HtmlRpt** _html_file_  
 Specifica il percorso completo e il nome del file in cui generare il report HTML. **sqlmaint** aggiunge al nome file una stringa nel formato _ *yyyyMMddhhmm* , in modo analogo a quanto avviene per il parametro **-Rpt** .  
  
 Quando si accede a un server remoto con *sqlmaint* , è necessario specificare il nome UNC completo del file per **html_file** .  
  
 **-DelHtmlRpt** \<*time_period*>  
 Specifica che è necessario eliminare dalla directory dei report tutti i report HTML se l'intervallo di tempo dopo la creazione del file del report supera \<*time_period*>. **-DelHtmlRpt** cerca i file con nome corrispondente al modello generato dal parametro *html_file*. Se *html_file* è c:\Programmi\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.htm, **-DelHtmlRpt** fa sì che **sqlmaint** elimini tutti i file con nome corrispondente al modello C:\Programmi\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint\*.htm e successivi al valore specificato per \<*time_period*>.  
  
 **-RmUnusedSpace** _threshold_percent free_percent_  
 Specifica la rimozione dello spazio inutilizzato dal database indicato in **-D**. Questa opzione è utile solo nel caso di database per i quali è stato impostato l'aumento automatico delle dimensioni. *Threshold_percent* specifica le dimensioni, in megabyte, che il database deve raggiungere prima che **sqlmaint** rimuova lo spazio inutilizzato. Se le dimensioni del database sono inferiori a *threshold_percent*, non viene eseguita alcuna azione. *Free_percent* specifica la quantità di spazio inutilizzato che deve rimanere nel database, in termini di percentuale delle dimensioni finali del database. Se ad esempio un database di 200 MB contiene 100 MB di dati e si specifica 10 per *free_percent* , le dimensioni finali del database saranno di 110 MB. Le dimensioni del database non vengono aumentate se sono inferiori a *free_percent* più la quantità di dati del database. Se ad esempio un database di 108 MB contiene 100 MB di dati e si specifica 10 per *free_percent* , il database non viene esteso a 110 MB, ma resta di 108 MB.  
  
 **-CkDB** |  **-CkDBNoIdx**  
 Specifica l'esecuzione di un'istruzione DBCC CHECKDB o DBCC CHECKDB con l'opzione NOINDEX nel database indicato in **-D**. Per ulteriori informazioni, vedere DBCC CHECKDB.  
  
 Se durante l'esecuzione di *sqlmaint* il database è in uso, viene scritto un avviso in **text_file** .  
  
 **-CkAl** |  **-CkAlNoIdx**  
 Specifica l'esecuzione di un'istruzione DBCC CHECKALLOC con l'opzione NOINDEX nel database indicato in **-D**. Per altre informazioni, vedere [DBCC CHECKALLOC &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md).  
  
 **-CkCat**  
 Specifica l'esecuzione di un'istruzione DBCC CHECKCATALOG (Transact-SQL) nel database indicato in **-D**. Per altre informazioni, vedere [DBCC CHECKCATALOG &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md).  
  
 **-UpdOptiStats** _sample_percent_  
 Specifica l'esecuzione dell'istruzione seguente in tutte le tabelle del database:  
  
```  
UPDATE STATISTICS table WITH SAMPLE sample_percent PERCENT;  
```  
  
 Se le tabelle contengono colonne calcolate e si usa **-UpdOptiStats** , è anche necessario specificare l'argomento **-SupportedComputedColumn**.  
  
 Per altre informazioni, vedere [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md).  
  
 **-RebldIdx** _free_space_  
 Specifica che gli indici delle tabelle nel database di destinazione devono essere ricompilati usando il valore percentuale *free_space* come valore inverso del fattore di riempimento. Ad esempio, se la percentuale di *free_space* è 30, viene usato un fattore di riempimento pari a 70. Se si specifica un valore percentuale *free_space* pari a 100, gli indici vengono ricompilati con il valore del fattore di riempimento originale.  
  
 Se gli indici si trovano in colonne calcolate e si usa **-RebldIdx** , è anche necessario specificare l'argomento **-SupportComputedColumn**.  
  
 **-RebldIdx**  
 Specificare questo argomento per eseguire i comandi di manutenzione DBCC sulle colonne calcolate usando **sqlmaint** .  
  
 **-WriteHistory**  
 Specifica l'aggiunta di una voce in msdb.dbo.sysdbmaintplan_history per ogni azione di manutenzione eseguita da **sqlmaint**. Se si specifica **-PlanName** o **-PlanID** , le voci in sysdbmaintplan_history usano l'ID del piano specificato. Se si specifica **-D** , l'ID del piano delle voci di sysdbmaintplan_history viene rappresentato da zeri.  
  
 **-BkUpDB** [ *backup_path*] |  **-BkUpLog** [ *backup_path* ]  
 Specifica un'azione di backup. **-BkUpDb** esegue il backup di tutto il database. **-BkUpLog** esegue solo il backup del log delle transazioni.  
  
 *backup_path* specifica la directory di backup. *backup_path* non è necessario se si specifica **-UseDefDir** e, se vengono specificati entrambi, **-UseDefDir** è prioritario. È possibile memorizzare il backup in una directory o usare l'indirizzo di un dispositivo nastro, ad esempio \\\\.\TAPE0. Il nome del file per il backup di un database viene generato automaticamente nel formato seguente:  
  
```  
dbname_db_yyyyMMddhhmm.BAK  
  
```  
  
 dove  
  
-   *dbname* è il nome del database del quale eseguire il backup.  
  
-   *yyyyMMddhhmm* rappresenta la data e l'ora dell'operazione di backup in cui *yyyy* = anno, *MM* = mese, *dd* = giorno, *hh* = ora e *mm* = minuto.  
  
 Il nome del file per il backup del log delle transazioni viene generato automaticamente nel formato seguente:  
  
```  
dbname_log_yyyymmddhhmm.BAK  
  
```  
  
 Se si usa il parametro **-BkUpDB** è anche necessario specificare il supporto con il parametro **-BkUpMedia** .  
  
 **-BkUpMedia**  
 Specifica il tipo di supporto per il backup, ovvero DISK o TAPE.  
  
 **DISK**  
 Indica che il supporto di backup è un disco.  
  
 **-DelBkUps**< *time_period* >  
 Per i backup su disco, specifica che è necessario eliminare dalla directory di backup tutti i file di backup se l'intervallo di tempo dopo la creazione del backup supera \<*time_period*>.  
  
 **-CrBkSubDir**  
 Per i backup su disco, specifica la creazione di una sottodirectory nella directory [*backup_path*] o nella directory di backup predefinita, se si usa anche **-UseDefDir** . Il nome della sottodirectory viene generato in base al nome del database specificato in **-D**. **-CrBkSubDir** consente di memorizzare in modo semplice tutti i backup di database diversi in sottodirectory separate senza dover modificare il parametro *backup_path* .  
  
 **-UseDefDir**  
 Per i backup su disco, specifica la creazione del file di backup nella directory di backup predefinita. Se sono specificati entrambi,**UseDefDir** è prioritario rispetto a *backup_path* . Nel caso di un'installazione predefinita di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la directory di backup predefinita è C:\Programmi\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\Backup.  
  
 **TAPE**  
 Indica che il supporto di backup è un nastro.  
  
 **-BkUpOnlyIfClean**  
 Specifica che il backup viene eseguito solo se i controlli **-Ck** specificati non rilevano problemi con i dati. Le azioni di manutenzione vengono eseguite nella stessa sequenza in cui sono indicate nel prompt dei comandi. Specificare i parametri **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** o **-CkCat** prima dei parametri **-BkUpDB**/ **-BkUpLog** se si intende specificare anche **- BkUpOnlyIfClean**, oppure il backup verrà eseguito a prescindere dal fatto che il controllo abbia segnalato problemi.  
  
 **-VrfyBackup**  
 Specifica l'esecuzione dell'istruzione RESTORE VERIFYONLY al termine del backup.  
  
 *number*[**minutes**\| **hours**\| **day**\| **weeks**\| **months**]  
 Specifica l'intervallo di tempo trascorso il quale un report o un file di backup può essere eliminato. *number* è un numero intero seguito (senza spazi) da un'unità di tempo, ad esempio:  
  
-   **12weeks**  
  
-   **3months**  
  
-   **15days**  
  
 Se si specifica solo *number* , l'unità di tempo predefinita è **weeks**.  
  
## <a name="remarks"></a>Osservazioni  
 L'utilità **sqlmaint** consente di eseguire operazioni di manutenzione in uno o più database. Se è specificato **-D** , le operazioni indicate nelle altre opzioni vengono eseguite solo nel database specificato. Se si specifica **-PlanName** o **-PlanID** , **sqlmaint** recupera dal piano di manutenzione solo l'elenco dei database. Tutte le operazioni specificate negli altri parametri di **sqlmaint** vengono eseguite in tutti i database dell'elenco ottenuto dal piano. L'utilità **sqlmaint** di per sé non esegue nessuna delle attività di manutenzione definite nel piano.  
  
 Se l'esecuzione riesce, l'utilità **sqlmaint** restituisce 0, in caso contrario 1. L'esito negativo viene segnalato se:  
  
-   Una delle azioni di manutenzione ha esito negativo.  
  
-   **-CkDB**, **-CkDBNoIdx**, **-CkAl**, **-CkAlNoIdx**, **-CkTxtAl** o **-CkCat** rileva problemi con i dati.  
  
-   Si verifica un errore generale.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utilità **sqlmaint** può essere eseguita da qualsiasi utente di Windows che abbia l'autorizzazione **Lettura/esecuzione** su `sqlmaint.exe`, archiviato nella cartella `x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER1\MSSQL\Binn` per impostazione predefinita. L'account di accesso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] specificato con **-login_ID** deve avere anche le autorizzazioni di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] necessarie per eseguire l'operazione specificata. Se la connessione a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizza l'autenticazione di Windows, l'account di accesso di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sul quale viene eseguito il mapping all'utente di Windows autenticato deve disporre delle autorizzazioni [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] necessarie per eseguire l'operazione specificata.  
  
 Ad esempio, per usare **-BkUpDB** è necessaria l'autorizzazione per eseguire l'istruzione BACKUP, mentre l'uso dell'argomento **-UpdOptiStats** richiede l'autorizzazione per eseguire l'istruzione UPDATE STATISTICS. Per ulteriori informazioni, vedere le sezioni dedicate alle autorizzazioni nei relativi argomenti della documentazione online.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-performing-dbcc-checks-on-a-database"></a>R. Esecuzione di controlli DBCC in un database  
  
```  
sqlmaint -S MyServer -D AdventureWorks2012 -CkDB -CkAl -CkCat -Rpt C:\MyReports\AdvWks_chk.rpt  
```  
  
### <a name="b-updating-statistics-using-a-15-sample-in-all-databases-in-a-plan-also-shrink-any-of-the-database-that-have-reached-110-mb-to-having-only-10-free-space"></a>B. Aggiornamento delle statistiche utilizzando un campione del 15% per tutti i database di un piano e compattazione dei database che hanno raggiunto dimensioni di 110 MB in modo che lo spazio libero sia pari al 10%  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -UpdOptiStats 15 -RmUnusedSpace 110 10  
```  
  
### <a name="c-backing-up-all-the-databases-in-a-plan-to-their-individual-subdirectories-in-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory-also-delete-any-backups-older-than-2-weeks"></a>C. Backup di tutti i database di un piano nelle singole sottodirectory della directory predefinita x:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup directory. ed eliminazione delle copie di backup risalenti a più di due settimane prima.  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -BkUpDB -BkUpMedia DISK -UseDefDir -CrBkSubDir -DelBkUps 2weeks  
```  
  
### <a name="d-backing-up-a-database-to-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory"></a>D. Backup di un database nella directory predefinita x:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup directory.  
  
```  
sqlmaint -S MyServer -BkUpDB -BkUpMedia DISK -UseDefDir  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../t-sql/statements/update-statistics-transact-sql.md)  
  
  
