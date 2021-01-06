---
title: Abilitare la proprietà TRUSTWORTHY per un database con mirroring
description: Informazioni su come abilitare la proprietà di database TRUSTWORTHY in un nuovo database con mirroring usando Transact-SQL in SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- TRUSTWORTHY database option
- mirror database [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 6993b076-78ef-453e-b0ea-e341b8e5ee3e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0feb3ff64be67f157aa1f8ec2e9c588507674701
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641995"
---
# <a name="set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql"></a>Impostazione di un database mirror per l'utilizzo della proprietà Trustworthy (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Quando viene eseguito il backup di un database, la proprietà TRUSTWORTHY del database viene impostata su OFF. Di conseguenza, la proprietà TRUSTWORTHY di un nuovo database mirror è sempre impostata su OFF. Se il database deve risultare attendibile dopo un failover, è necessario eseguire passaggi di configurazione aggiuntivi dopo l'avvio del mirroring.  
  
> [!NOTE]  
>  Per informazioni su questa proprietà di database, vedere [Proprietà di database TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md).  
  
## <a name="procedure"></a>Procedura  
  
#### <a name="to-setup-a-mirror-database-to-use-the-trustworthy-property"></a>Per impostare un database mirror per l'utilizzo della proprietà Trustworthy  
  
1.  Sull'istanza del server principale verificare che nel database principale la proprietà Trustworthy sia impostata su ON.  
  
    ```  
    SELECT name, database_id, is_trustworthy_on FROM sys.databases   
    ```  
  
     Per altre informazioni, vedere [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
2.  Dopo l'avvio del mirroring, verificare che il database sia impostato quale database principale, che nella sessione venga utilizzata una modalità operativa sincrona e che la sessione sia già sincronizzata.  
  
    ```  
    SELECT database_id, mirroring_role, mirroring_safety_level_desc, mirroring_state_desc FROM sys.database_mirroring  
    ```  
  
     Per altre informazioni, vedere [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
3.  Dopo la sincronizzazione della sessione di mirroring, eseguire manualmente il failover sul database mirror.  
  
     Questa operazione può essere eseguita in SQL Server Management Studio o mediante Transact-SQL:  
  
    -   [Failover manuale di una sessione di mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-sql-server-management-studio.md)  
  
    -   [Eseguire il failover manuale di una sessione di mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md)  
  
4.  Impostare la proprietà Trustworthy del database su ON utilizzando il comando ALTER DATABASE seguente:  
  
    ```  
    ALTER DATABASE <database_name> SET TRUSTWORTHY ON  
    ```  
  
     Per altre informazioni, vedere[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
5.  Facoltativamente, eseguire di nuovo il failover manuale per tornare al database principale originale.  
  
6.  Facoltativamente, passare in modalità asincrona a prestazioni elevate impostando SAFETY su OFF e verificando che anche WITNESS sia impostato su OFF.  
  
     In Transact-SQL:  
  
    -   [Modificare la sicurezza delle transazioni in una sessione di mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)  
  
    -   [Rimuovere il server di controllo del mirroring da una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
     In SQL Server Management Studio:  
  
    -   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà di database TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Impostazione di un database mirror crittografato](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
