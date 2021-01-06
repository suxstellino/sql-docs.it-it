---
title: Impostare un database mirror crittografato | Microsoft Docs
description: Informazioni su come abilitare la decrittografia automatica della chiave master del database di un database mirror usando sp_control_dbmasterkey_password per creare una credenziale.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- database master key [SQL Server], database mirroring
- mirror database [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 7329a575-be29-46e0-abc6-1344db37920c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6611528bd3d5b12f01ecebb48e63ee3b01d73215
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641980"
---
# <a name="set-up-an-encrypted-mirror-database"></a>Impostazione di un database mirror crittografato
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Per abilitare la decrittografia automatica della chiave master del database di un database mirror, è necessario fornire la password utilizzata per crittografare tale chiave nell'istanza del server mirror. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive includono meccanismi per trasferire la password. Usare **sp_control_dbmasterkey_password** per creare una credenziale per la chiave master del database prima di avviare il mirroring del database. Questa operazione va ripetuta per ogni database per il quale si desidera eseguire il mirroring. Per altre informazioni, vedere [sp_control_dbmasterkey_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md).  
  
> [!CAUTION]  
>  Non abilitare la decrittografia di failover di un database che deve rimanere inaccessibile a **sa** e altre entità server con privilegi elevati. È possibile configurare un database in modo che la sua gerarchia di chiavi non possa essere decrittografata dalla chiave master del servizio. Questa opzione viene supportata come difesa in profondità per database che contengono informazioni che non vanno rese accessibili a entità server **sa** o altre entità server con privilegi elevati. Abilitando la decrittografia di failover per un database di questo tipo si elimina questa difesa in profondità, consentendo a entità server **sa** e altre entità server con privilegi elevati di decrittografare il database.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_control_dbmasterkey_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Impostazione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
  
  
