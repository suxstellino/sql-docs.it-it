---
description: Avviare la procedura guidata Abilitare il database per l'estensione
title: Avviare la procedura guidata Abilitare il database per l'estensione
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: quickstart
f1_keywords:
- sql13.swb.stretchwizard.f1
- sql13.swb.stretchwizard.createdatabasecredentials.f1
- sql13.swb.stretchwizard.selectdatabasetables.f1
- sql13.swb.stretchwizard.validatesqlserver.f1
- sql13.swb.stretchwizard.selectazuredeployment.f1
- sql13.swb.stretchwizard.configureazuredeployment.f1
- sql13.swb.stretchwizard.Summary.f1
- sql13.swb.stretchwizard.Results.f1
- sql13.swb.stretchwizard.introduction.f1
helpviewer_keywords:
- Stretch Database, wizard
- Enable Database for Stretch Wizard
ms.assetid: 855dd9fc-f80c-4dbc-bf46-55a9736bfe15
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 74435746bc2ef72741cf25a610cb37d4ac1893b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080992"
---
# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Avviare la procedura guidata Abilitare il database per l'estensione
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 Per configurare un database per Stretch Database, eseguire la procedura guidata Abilitare il database per la funzionalità Stretch.  Questo articolo illustra le informazioni da inserire e le opzioni disponibili nella procedura guidata.  
  
 Per saperne di più su Stretch Database, vedere l'articolo relativo a [Stretch Database](../../sql-server/stretch-database/stretch-database.md). 
 
 > [!NOTE] 
 > Se in seguito si disabilita Stretch Database, tenere presente che la disabilitazione di Stretch Database per una tabella o un database non elimina l'oggetto remoto. Se si vuole eliminare la tabella remota o il database remoto, è necessario eliminarlo tramite il portale di gestione di Azure. Gli oggetti remoti continuano a generare costi di Azure fino a quando non vengono eliminati manualmente. 
  
## <a name="launch-the-wizard"></a>Avviare la procedura guidata  
  
1.  In Esplora oggetti di SQL Server Management Studio selezionare il database per il quale abilitare l'estensione.  
  
2.  Fare clic con il pulsante destro del mouse e selezionare **Attività**, **Estendi** e quindi **Abilita** per avviare la procedura guidata.  
  
##  <a name="introduction"></a><a name="Intro"></a> Introduzione  
 Esaminare lo scopo della procedura guidata e i prerequisiti.  
 
 I prerequisiti importanti includono i seguenti.
 -   È necessario essere un amministratore per modificare le impostazioni del database.
 -   È necessaria una sottoscrizione di Microsoft Azure.
 -   SQL Server deve poter comunicare con il server remoto di Azure.
  
 ![Pagina Introduzione della procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-1.png "Pagina Introduzione della procedura guidata Stretch Database")  
  
##  <a name="select-tables"></a><a name="Tables"></a> Selezionare le tabelle  
 Selezionare le tabelle di cui si desidera abilitare l'estensione.  
 
Le tabelle con un numero elevato di righe vengono visualizzate nella parte superiore dell'elenco ordinato. La procedura guidata, prima di visualizzare l'elenco di tabelle, le analizza per cercare i tipi di dati attualmente non supportati da Stretch Database. 
  
 ![Pagina Seleziona tabelle della procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-2.png "Pagina Seleziona tabelle della procedura guidata Stretch Database")  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|(nessun titolo)|Selezionare la casella di controllo in questa colonna per abilitare la tabella selezionata per l'estensione.|  
|**Nome**|Specifica il nome della tabella nel database.|  
|(nessun titolo)|Un simbolo in questa colonna può rappresentare un avviso che non impedisce l'abilitazione della tabella selezionata per l'estensione. Può rappresentare anche un problema che impedisce l'abilitazione della tabella selezionata per l'estensione, ad esempio perché la tabella usa un tipo di dati non supportato. Passare il mouse sul simbolo per visualizzare una descrizione comando con altre informazioni. Per altre informazioni, vedere [Limitazioni di Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).|  
|**Estesa**|Indica se la tabella è già abilitata per l'estensione.|  
|**Migrazione**|È possibile eseguire la migrazione di un'intera tabella (**Intera tabella**) oppure specificare un filtro in una colonna esistente nella tabella. Per usare una funzione di filtro diversa per selezionare le righe di cui eseguire la migrazione, eseguire l'istruzione ALTER TABLE per specificare la funzione di filtro dopo aver chiuso la procedura guidata. Per altre informazioni sulla funzione di filtro, vedere [Select rows to migrate by using a filter function (Selezionare le righe di cui eseguire la migrazione usando una funzione di filtro)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Per altre informazioni su come applicare la funzione, vedere [Enable Stretch Database for a table (Abilitare Estensione database per una tabella)](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) o [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).|  
|**prime righe**|Consente di specificare il numero di righe nella tabella.|  
|**Dimensioni (KB)**|Specifica le dimensioni della tabella in KB.|  
  
## <a name="optionally-provide-a-row-filter"></a>Facoltativamente, specificare un filtro di riga  
 Se si vuole specificare una funzione di filtro per selezionare le righe di cui eseguire la migrazione, seguire questa procedura nella pagina **Seleziona tabelle** .  
  
1.  Nell'elenco **Selezionare le tabelle da estendere** fare clic su **Intera tabella** nella riga della tabella. Viene visualizzata la finestra di dialogo **Selezionare le righe da estendere** .  
  
     ![Definire un predicato di filtro basato sulla data](../../sql-server/stretch-database/media/stretch-wizard-2a.png "Definire un predicato di filtro basato sulla data")  
  
2.  Nella finestra di dialogo **Selezionare le righe da estendere** fare clic su **Scegli righe**.  
  
3.  Nel campo **Nome** specificare un nome per la funzione di filtro.  
  
4.  Per la clausola **Where** , scegliere una colonna dalla tabella, selezionare un operatore e specificare un valore.  
  
5.  Fare clic su **Controlla** per testare la funzione. Se la funzione restituisce i risultati della tabella, ossia se sono presenti righe di cui eseguire la migrazione che soddisfano la condizione, il test visualizza **Esito positivo**.  

> [!NOTE] 
> La casella di testo che visualizza la query del filtro è di sola lettura. Non è possibile modificare la query nella casella di testo.
  
6.  Fare clic su Fine per tornare alla pagina **Seleziona tabelle** .  

La funzione di filtro viene creata in SQL Server solo al termine della procedura guidata. Fino ad allora, è possibile tornare alla pagina **Seleziona tabelle** per modificare o rinominare la funzione di filtro.

![Pagina Seleziona tabelle dopo la definizione di un predicato del filtro](../../sql-server/stretch-database/media/stretch-wizard-2b.png "Pagina Seleziona tabelle dopo la definizione di un predicato del filtro")

Se si vuole usare un tipo diverso di funzione di filtro per selezionare le righe di cui eseguire la migrazione, eseguire una di queste operazioni.  
  
-   Uscire dalla procedura guidata ed eseguire l'istruzione ALTER TABLE per abilitare l'estensione per la tabella e specificare una funzione di filtro. Per altre informazioni, vedere [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).  
  
-   Eseguire l'istruzione ALTER TABLE per specificare una funzione di filtro dopo l'uscita dalla procedura guidata. Per i passaggi necessari, vedere [Aggiungere una funzione di filtro dopo l'esecuzione della procedura guidata](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
##  <a name="configure-azure"></a><a name="Configure"></a> Configura Azure  
  
1.  Accedere a Microsoft Azure con un account Microsoft.  
  
     ![Accesso ad Azure - Procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-3.png "Accesso ad Azure - Procedura guidata Stretch Database")  
  
2.  Selezionare la sottoscrizione di Azure esistente da usare per Stretch Database. 

> [!NOTE] 
> Per abilitare l'Estensione in un database, è necessario disporre di diritti di amministratore per la sottoscrizione in uso. La procedura guidata di Estensione database visualizzerà solo le sottoscrizioni in cui l'utente ha i diritti di amministratore.
  
3.  Selezionare l'area di Azure da usare per Stretch Database.
    -   Se si crea un nuovo server, il server viene creato in questa area.  
    -   Se sono presenti server nell'area selezionata, la procedura guidata li elenca quando si sceglie **Server esistente**.
  
     Per ridurre al minimo la latenza, selezionare l'area di Azure in cui si trova SQL Server. Per altre informazioni sulle aree, vedere [Aree di Azure](https://azure.microsoft.com/regions/).  
  
4.  Specificare se si desidera usare un server esistente o creare un nuovo server Azure.  
  
     Se Active Directory in SQL Server è federata con Azure Active Directory, è possibile usare facoltativamente un account del servizio federato per SQL Server per comunicare con il server Azure remoto. Per altre informazioni sui requisiti per questa opzione, vedere [Opzioni di ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
    -   **Creazione di un nuovo server**  
  
        1.  Creare un account di accesso e una password dell'amministratore del server.  
  
        2.  È anche possibile usare un account del servizio federato per SQL Server per comunicare con il server Azure remoto.  
  
         ![Creazione di un nuovo server di Azure - Procedura guidata Stretch Database](../../relational-databases/tables/media/stretch-wizard-4.png "Creazione di un nuovo server di Azure - Procedura guidata Stretch Database")  
  
    -   **Server esistente**  
  
        1.  Selezionare il server di Azure esistente.  
  
        2.  Selezionare il metodo di autenticazione.  
  
            -   Se si seleziona **Autenticazione di SQL Server**, specificare un account di accesso di amministratore e la relativa password.  
  
            -   Selezionare **Autenticazione integrata di Active Directory** per usare un account del servizio federato per SQL Server per comunicare con il server Azure remoto. Se il server selezionato non è integrato con Azure Active Directory, questa opzione non viene visualizzata.
  
         ![Selezione di un server di Azure esistente - Procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-5.png "Selezione di un server di Azure esistente - Procedura guidata Stretch Database")  
  
##  <a name="secure-credentials"></a><a name="Credentials"></a> Credenziali protette  
 È necessario disporre di una chiave master del database per proteggere le credenziali usate da Stretch Database per la connessione al database remoto.  
  
 Se esiste già una chiave master del database, immettere la relativa password.  
  
 ![Screenshot che mostra la pagina Credenziali protette della procedura guidata Stretch Database con la casella di testo Password vuota.](../../sql-server/stretch-database/media/stretch-wizard-6b.PNG "Pagina Credenziali protette della procedura guidata Stretch Database")  
  
 Se il database non contiene già una chiave master, immettere una password complessa per creare una chiave master del database.  
  
 ![Screenshot che mostra la pagina Credenziali protette della procedura guidata Stretch Database con la nuova password e la password di conferma specificate nelle caselle di testo.](../../relational-databases/tables/media/stretch-wizard-6.png "Pagina Credenziali protette della procedura guidata Stretch Database")  
  
 Per altre informazioni sulla chiave master del database, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) e [Creazione della chiave master di un database](../../relational-databases/security/encryption/create-a-database-master-key.md). Per altre informazioni sulle credenziali create dalla procedura guidata, vedere [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
##  <a name="select-ip-address"></a><a name="Network"></a> Selezionare l'indirizzo IP  
 Usare l'intervallo di indirizzi IP pubblici della subnet (consigliato) oppure l'indirizzo IP pubblico di SQL Server per creare una regola del firewall in Azure che consenta a SQL Server di comunicare con il server Azure remoto.  
  
 L'indirizzo o gli indirizzi IP forniti in questa pagina indicano al server Azure di consentire a dati, query e operazioni di gestione in ingresso avviate da SQL Server di passare attraverso il firewall di Azure. La procedura guidata non modifica le impostazioni del firewall in SQL Server.  
  
 ![Pagina Selezionare l'indirizzo IP della procedura guidata Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Pagina Selezionare l'indirizzo IP della procedura guidata Stretch Database")  
  
##  <a name="summary"></a><a name="Summary"></a> Riepilogo  
 Esaminare i valori immessi e le opzioni selezionate nella procedura guidata e i costi previsti in Azure. Selezionare quindi **Fine** per abilitare l'estensione.  
  
 ![Pagina Riepilogo della procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-8.png "Pagina Riepilogo della procedura guidata Stretch Database")  
  
##  <a name="results"></a><a name="Results"></a> Risultati  
 Esaminare i risultati.  
  
 Per monitorare lo stato della migrazione dei dati, vedere [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Monitorare e risolvere i problemi relativi alla migrazione dei dati (Estensione database))](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 ![Pagina Risultati della procedura guidata Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-9.PNG "Pagina Risultati della procedura guidata Stretch Database")  
  
##  <a name="troubleshooting-the-wizard"></a><a name="KnownIssues"></a> Risoluzione dei problemi relativi alla procedura guidata  
 **La procedura guidata Stretch Database non ha esito positivo.**  
 La procedura guidata non viene eseguita correttamente quando Stretch Database non è ancora abilitato a livello di server e non si hanno le necessarie autorizzazioni di amministratore di sistema. Chiedere all'amministratore di sistema di abilitare Estensione database nell'istanza del server locale e quindi eseguire nuovamente la procedura guidata. Per ulteriori informazioni, vedere [Prerequisito: autorizzazioni per abilitare Stretch Database nel server](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).  
  
## <a name="next-steps"></a>Passaggi successivi  
 Abilitare tabelle aggiuntive per Stretch Database. Monitorare la migrazione dei dati e gestire tabelle e database abilitati per l'estensione.  
  
-   [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) per abilitare tabelle aggiuntive.  
  
-   [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Monitorare e risolvere i problemi relativi alla migrazione dei dati (Estensione database))](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) per visualizzare lo stato della migrazione dei dati.  
  
-   [Pause and resume data migration &#40;Stretch Database&#41; (Sospendere e riprendere la migrazione dei dati (Estensione database))](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Gestire e risolvere i problemi di Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Eseguire il backup di database con estensione abilitata](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Ripristinare database con estensione abilitata](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
