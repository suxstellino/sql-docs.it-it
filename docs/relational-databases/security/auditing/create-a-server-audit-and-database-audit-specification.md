---
title: Creare una specifica di controllo server e di controllo database
description: Informazioni su come creare una specifica di controllo SQL Server e di controllo database usando SQL Server Management Studio o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlaudit.dbaudit.general.f1
helpviewer_keywords:
- audits [SQL Server], creating database specification
- database audit [SQL Server]
ms.assetid: 26ee85de-6e97-4318-b526-900924d96e62
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d015a1adf0bdd5e5f47b49a8dabb6783b8538b7e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250081"
---
# <a name="create-a-server-audit-and-database-audit-specification"></a>Creare una specifica di controllo server e di controllo database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo articolo descrive come creare una specifica di controllo server e di controllo database in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Il controllo di un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o di un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comporta il rilevamento e la registrazione di eventi che si verificano nel sistema. L'oggetto *SQL Server Audit* raccoglie un'unica istanza di azioni a livello di server o di database e gruppi di azioni da monitorare. Il controllo si trova a livello dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per ogni istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è possibile disporre di più controlli. Anche l'oggetto *Database-Level Audit Specification* fa parte di un controllo. È possibile creare una specifica del controllo del database per ogni database di SQL Server e per ogni controllo. Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Le specifiche del controllo del database sono oggetti non a sicurezza diretta che risiedono in un database specifico. Al momento della creazione, una specifica di controllo database è in stato disabilitato.  
  
 Durante la creazione o la modifica di una specifica di controllo database in un database utente, non includere azioni di controllo su oggetti con ambito server, come le viste di sistema. Se si includono oggetti con ambito server, verrà creato il controllo, ma gli oggetti con ambito server non saranno inclusi e non verrà restituito alcun errore. Per eseguire il controllo degli oggetti con ambito server, usare una specifica di controllo database nel database master.  
  
 Le specifiche di controllo database risiedono nel database dove vengono create, ad eccezione del database di sistema **TempDB**.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
-   Gli utenti con l'autorizzazione ALTER ANY DATABASE AUDIT possono creare specifiche di controllo database e associarle a qualsiasi controllo.  
  
-   Dopo la creazione di una specifica di controllo database, le entità con le autorizzazioni CONTROL SERVER o ALTER ANY DATABASE AUDIT possono visualizzarla. Anche l'account sysadmin può visualizzarla.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>Per creare un controllo del server  
  
1.  In Esplora oggetti espandere la cartella **Sicurezza** .  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Controlli** e scegliere **Nuovo controllo**. Per altre informazioni, vedere [Creare un controllo server e una specifica di controllo server](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md).  
  
3.  Dopo aver selezionato le opzioni desiderate selezionare **OK**.  

#### <a name="to-create-a-database-level-audit-specification"></a>Per creare una specifica del controllo a livello di database  
  
1.  In Esplora oggetti espandere il database in cui si vuole creare la specifica di controllo.  
  
2.  Espandere la cartella **Sicurezza** .  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Specifiche controllo database** e scegliere **Nuova specifica controllo database**.  
  
     Nella finestra di dialogo **Crea specifica controllo database** sono disponibili queste opzioni:  
  
     **Nome**  
     Nome della specifica del controllo del database. Un nome viene generato automaticamente quando si crea una nuova specifica di controllo server. Il nome è modificabile.  
  
     **Controllo**  
     Nome di un oggetto controllo server esistente. Digitare il nome del controllo o selezionarlo nell'elenco.  
  
     **Tipo di azione di controllo**  
     Specifica i gruppi di azioni di controllo a livello di database e le azioni di controllo da acquisire. Per un elenco di gruppi di azioni di controllo a livello di database e delle azioni di controllo e una descrizione degli eventi contenuti, vedere [Azioni e gruppi di azioni di controllo di SQL Server](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Schema dell'oggetto**  
     Consente di visualizzare lo schema per il **nome oggetto** specificato.  
  
     **nome oggetto**  
     Nome dell'oggetto da controllare. Questa opzione è disponibile solo per le azioni di controllo. Non si applica ai gruppi di controllo.  
  
     **Puntini di sospensione (...)**  
     Apre la finestra di dialogo **Seleziona oggetti** in modo da poter cercare e selezionare un oggetto disponibile, in base all'opzione **Tipo di azione di controllo** specificata.  
  
     **Nome entità**  
     Account per filtrare il controllo per l'oggetto da controllare.  
  
     **Puntini di sospensione (...)**  
     Apre la finestra di dialogo **Seleziona oggetti** in modo da poter cercare e selezionare un oggetto disponibile, in base all'opzione **Nome oggetto** specificata.  
  
4.  Dopo aver selezionato le opzioni desiderate selezionare **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>Per creare un controllo del server  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard selezionare **Nuova query**.  
  
3.  Incollare l'esempio seguente nella finestra Query e quindi selezionare **Esegui**.  
  
    ```  
    USE master ;  
    GO  
    -- Create the server audit.   
    CREATE SERVER AUDIT Payrole_Security_Audit  
        TO FILE ( FILEPATH =   
    'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;   
    GO  
    -- Enable the server audit.   
    ALTER SERVER AUDIT Payrole_Security_Audit   
    WITH (STATE = ON) ;  
    ```  
  
#### <a name="to-create-a-database-level-audit-specification"></a>Per creare una specifica del controllo a livello di database  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard selezionare **Nuova query**.  
  
3.  Incollare l'esempio seguente nella finestra Query e quindi selezionare **Esegui**. In questo esempio viene creata una specifica di controllo database denominata `Audit_Pay_Tables`. Controlla le istruzioni SELECT e INSERT da parte dell'utente `dbo` per la tabella `HumanResources.EmployeePayHistory`, in base al controllo server definito nella sezione precedente.  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    -- Create the database audit specification.   
    CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
    FOR SERVER AUDIT Payrole_Security_Audit  
    ADD (SELECT , INSERT  
         ON HumanResources.EmployeePayHistory BY dbo )   
    WITH (STATE = ON) ;   
    GO  
  
    ```  
  
 Per altre informazioni, vedere [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) e [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md).  
  
  
