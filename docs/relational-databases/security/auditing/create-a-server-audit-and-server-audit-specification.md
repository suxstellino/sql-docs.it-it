---
title: Creare un controllo server e una specifica di controllo server
description: Informazioni su come creare un controllo server e una specifica di controllo server di SQL Server usando SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL)
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.SWB.SQLAUDIT.FILTER.F1
- sql13.swb.sqlaudit.general.f1
- sql13.swb.sqlaudit.srvaudit.general.f1
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
ms.assetid: 6624b1ab-7ec8-44ce-8292-397edf644394
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: 3db484c1279546ab76ebf302582dbb984f991fa4
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250120"
---
# <a name="create-a-server-audit-and-server-audit-specification"></a>Creazione di un controllo del server e di una specifica del controllo del server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come creare un controllo del server e la specifica di un controllo del server in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Il *controllo* di un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o di un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comporta il rilevamento e la registrazione di eventi che si verificano nel sistema. L'oggetto *SQL Server Audit* raccoglie un'unica istanza di azioni a livello di server o di database e gruppi di azioni da monitorare. Il controllo si trova a livello dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile disporre di più controlli. L'oggetto *specifica controllo server* appartiene a un controllo. È possibile creare una specifica del controllo del server per ogni controllo, poiché entrambi vengono creati nell'ambito dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare un controllo del server e una specifica del controllo del server utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   È necessario che un controllo esista prima che sia possibile creare la specifica del controllo del server. Quando viene creata una specifica del controllo del server, il relativo stato è disabilitato.  
  
-   L'istruzione CREATE SERVER AUDIT è nell'ambito di una transazione. L'esecuzione del rollback della transazione comporta il rollback anche per l'istruzione.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
-   Per creare, modificare o eliminare un controllo del server, le entità devono disporre dell'autorizzazione ALTER ANY SERVER AUDIT o CONTROL SERVER.  
  
-   Gli utenti che dispongono dell'autorizzazione ALTER ANY SERVER AUDIT possono creare specifiche del controllo del server e associarle a qualsiasi controllo.  
  
-   Dopo essere stata creata, la specifica del controllo del server può essere visualizzata dalle entità con autorizzazione CONTROL SERVER o ALTER ANY SERVER AUDIT, dell'account sysadmin oppure dalle entità che possono accedere esplicitamente al controllo.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>Per creare un controllo del server  
  
1.  In Esplora oggetti espandere la cartella **Sicurezza** .  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Controlli** e scegliere **Nuovo controllo...** .  
  
     Nella pagina **Generale** della finestra di dialogo **Crea controllo** sono disponibili le opzioni indicate di seguito.  
  
     **Nome controllo**  
     Nome del controllo. Tale nome viene generato automaticamente quando si crea un nuovo controllo, ma è modificabile.  
  
     **Ritardo coda (in millisecondi)**  
     Indica la quantità di tempo in millisecondi che può trascorrere prima che venga forzata l'elaborazione delle azioni di controllo.  Il valore 0 indica un recapito sincrono. Il valore minimo predefinito è **1000** (1 secondo). Il valore massimo è 2.147.483.647 (2.147.483,647 secondi o 24 giorni, 20 ore, 31 minuti e 23,647 secondi).  
  
     **In caso di errore del log di controllo:**  
     **Continua**  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Le operazioni di SQL Server continuano. I record di controllo non vengono mantenuti. Il controllo consente di continuare il tentativo di registrazione di eventi e verrà ripreso se viene risolta la condizione di errore. Selezionando l'opzione **Continua** si potrà consentire un'attività non certificata che potrebbe violare i criteri di sicurezza. Selezionare questa opzione quando il funzionamento del [!INCLUDE[ssDE](../../../includes/ssde-md.md)] è più importante della gestione di un controllo completo. Si tratta della selezione predefinita.  
  
     **Arresta server**  
     Viene forzata la chiusura del server quando risulta impossibile scrivere dati nella destinazione di controllo da parte dell'istanza del server preposta a tale compito. L'account di accesso che esegue questa operazione deve avere l'autorizzazione **SHUTDOWN** . In caso contrario, questa funzione non verrà eseguita e verrà generato un messaggio di errore. Non viene generato alcun evento controllato. Selezionare questa opzione quando un errore a livello di controllo potrebbe compromettere la sicurezza o l'integrità del sistema.  
  
     **Errore operazione**  
     Nei casi in cui non è possibile scrivere nel log di controllo tramite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit, questa opzione può impedire le azioni del database qualora provocassero eventi controllati. Non viene generato alcun evento controllato. Le azioni che non provocano eventi controllati possono continuare. Il controllo consente di continuare il tentativo di registrazione di eventi e verrà ripreso se viene risolta la condizione di errore. Selezionare questa opzione quando la gestione di un controllo completo è più importante dell'accesso completo al [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
    > [!IMPORTANT]  
    >  Quando il controllo è in uno stato di errore, l'esecuzione di eventi controllati può continuare tramite la connessione amministrativa dedicata.  
  
     Elenco **Destinazione controllo**  
     Indica la destinazione per i dati del controllo. Le opzioni disponibili sono un file binario, il registro applicazioni di Windows o il registro di sicurezza di Windows. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Se non si configurano impostazioni aggiuntive in Windows, non sarà possibile scrivere nel registro di sicurezza di Windows. Per altre informazioni, vedere [Scrivere eventi di controllo di SQL Server nel registro di sicurezza](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md).  
  
     **Percorso file**  
     Indica il percorso della cartella in cui vengono scritti i dati del controllo quando in **Destinazione controllo** è specificato un file.  
  
     **Puntini di sospensione (...)**  
     Apre la finestra di dialogo **Trova cartella -** _nome\_server_ per specificare un percorso di file o creare una cartella in cui viene scritto il file di controllo.  
  
     **Limite massimo di file di controllo:**  
     **Numero massimo file di rollover**  
     Specifica che quando viene raggiunto il numero massimo di file di controllo, i file di controllo meno recenti vengono sovrascritti dal nuovo contenuto del file.  
  
     **Numero massimo di file**  
     Specifica che quando viene raggiunto il numero massimo di file di controllo, qualsiasi azione che causa la generazione di eventi di controllo aggiuntivi avrà esito negativo e verrà visualizzato un errore.  
  
     Casella di controllo **Senza limiti**  
     Quando si seleziona la casella di controllo **Senza limiti** in **Numero massimo file di rollover** , non viene applicato alcun limite al numero di file di controllo che verranno creati. La casella di controllo **Senza limiti** viene selezionata per impostazione predefinita e si applica alle selezioni **Numero massimo file di rollover** e **Numero massimo di file** .  
  
     Casella **Numero di file**  
     Specifica il numero di file di controllo da creare, fino a un massimo di 2.147.483.647. Questa opzione è disponibile solo se non è selezionato **Senza limiti** .  
  
     **Dimensioni massime del file**  
     Specifica la dimensione massima per un file di controllo in megabyte (MB), gigabyte (GB) o terabyte (TB). È possibile specificare un numero fino a 2.147.483.647 TB. Se si seleziona la casella di controllo **Senza limiti** non si impono alcun limite alle dimensioni del file. La casella di controllo **Senza limiti** è selezionata per impostazione predefinita.  
  
     Casella di controllo **Riserva spazio su disco**  
     Indica che sul disco viene preallocata una quantità di spazio uguale alle dimensioni massime del file specificate. Questa impostazione può essere utilizzata solo se non è selezionata la casella di controllo **Senza limiti** in **Dimensioni massime file** . Questa casella di controllo non è selezionata per impostazione predefinita.  
  
3.  Facoltativamente, nella pagina **Filtro** , immettere un predicato o clausola `WHERE` per il controllo del server per specificare opzioni aggiuntive non disponibili nella pagina **Generale** . Racchiudere il predicato tra le parentesi, ad esempio `(object_name = 'EmployeesTable')`.  
  
4.  Una volta selezionate le opzioni, fare clic su **OK**.  
  
#### <a name="to-create-a-server-audit-specification"></a>Per creare una specifica del controllo del server  
  
1.  In Esplora oggetti fare clic sul segno più per espandere la cartella **Sicurezza** .  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Specifiche controllo server** e selezionare **Nuova specifica controllo server...** .  
  
     Nella finestra di dialogo **Crea specifica controllo server** sono disponibili le opzioni indicate di seguito.  
  
     **Nome**  
     Nome della specifica del controllo del server. Tale nome viene generato automaticamente quando si crea una nuova specifica del controllo del server, ma è modificabile.  
  
     **Controllo**  
     Nome di un controllo server esistente. Digitare il nome del controllo o selezionarlo nell'elenco.  
  
     **Tipo di azione di controllo**  
     Specifica i gruppi di azioni di controllo a livello di server e le azioni di controllo da acquisire. Per l'elenco di gruppi di azioni di controllo a livello di server e di azioni di controllo e una descrizione degli eventi contenuti, vedere [Azioni e gruppi di azioni di SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Schema dell'oggetto**  
     Consente di visualizzare lo schema per il **nome oggetto** specificato.  
  
     **nome oggetto**  
     Nome dell'oggetto da controllare. Tale opzione è disponibile solo per le azioni di controllo e non si applica ai gruppi di controllo.  
  
     **Puntini di sospensione (...)**  
     Consente di visualizzare la finestra di dialogo **Seleziona oggetti** per eseguire la ricerca e la selezione di un oggetto disponibile, in base all'opzione **Tipo di azione di controllo**.  
  
     **Nome entità**  
     Account per filtrare il controllo per l'oggetto da controllare.  
  
     **Puntini di sospensione (...)**  
     Viene visualizzata la finestra di dialogo **Seleziona oggetti** per eseguire la ricerca e selezionare un oggetto disponibile, in base all'opzione **Nome oggetto** specificata.  
  
3.  Al termine, fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>Per creare un controllo del server  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. 
  
    ```  
    -- Creates a server audit called "HIPAA_Audit" with a binary file as the target and no options.  
    CREATE SERVER AUDIT HIPAA_Audit  
        TO FILE ( FILEPATH ='E:\SQLAudit\' );  
    ```  
> [!NOTE]
> Anche se è possibile usare un percorso UNC come destinazione per il file di controllo, procedere con attenzione. Se è presente una latenza di rete per quella condivisione file, può verificarsi una riduzione del livello delle prestazioni in SQL Server, perché i thread attendono il completamento di una scrittura di controllo prima di procedere. Nel log degli errori di SQL Server possono apparire vari messaggi di errore, ad esempio 17894:
>
>   2020-02-07 12:21:35.100 Server Dispatcher (0x7954) from dispatcher pool 'XE Engine main dispatcher pool' Worker 0x00000058E7300000  appears to be non-yielding on Node 0.


#### <a name="to-create-a-server-audit-specification"></a>Per creare una specifica del controllo del server  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    /*Creates a server audit specification called "HIPAA_Audit_Specification" that audits failed logins for the SQL Server audit "HIPAA_Audit" created above.  
    */  
  
    CREATE SERVER AUDIT SPECIFICATION HIPAA_Audit_Specification  
    FOR SERVER AUDIT HIPAA_Audit  
        ADD (FAILED_LOGIN_GROUP);  
    GO  
    -- Enables the audit.   
  
    ALTER SERVER AUDIT HIPAA_Audit  
    WITH (STATE = ON);  
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) e [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md).  
  
  
