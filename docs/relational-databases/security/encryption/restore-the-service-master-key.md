---
title: Ripristinare la chiave master del servizio | Microsoft Docs
description: Informazioni su come ripristinare la chiave master del servizio in SQL Server tramite Transact-SQL. La chiave master del servizio è la radice della gerarchia di crittografia di SQL Server.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- service master key [SQL Server], importing
- service master key [SQL Server], restoring
ms.assetid: 14bdbbbe-d384-4692-b670-4243d2466fe1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 8df8f8a8a01b8ade5372147f274d329baf346ffd
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250171"
---
# <a name="restore-the-service-master-key"></a>Ripristino della chiave master del servizio
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come ripristinare una chiave master del servizio in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> [!WARNING]  
> È improbabile che sia mai necessario ripristinare la chiave master del servizio. In tal caso, tuttavia, è consigliabile procedere con estrema cautela. Per altre informazioni, vedere [Back Up the Service Master Key](../../../relational-databases/security/encryption/back-up-the-service-master-key.md).  
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
  
- Quando si ripristina la chiave master del servizio, in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono decrittografati tutti i segreti e tutte le chiavi crittografate con la chiave master del servizio corrente. Tali elementi vengono poi crittografati nuovamente con la chiave master del servizio caricata dal file di backup.  
  
- In caso di esito negativo di una qualsiasi delle operazioni di decrittografia, il ripristino avrà esito negativo. È possibile utilizzare l'opzione FORCE per ignorare eventuali errori, ma in questo caso andranno perduti tutti i dati che non possono essere decrittografati.  
  
- La rigenerazione della gerarchia di crittografia è un'operazione che utilizza molte risorse e pertanto dovrebbe essere pianificata in periodi di carico ridotto.  
  
> [!CAUTION]  
> La chiave master del servizio è l'elemento radice della gerarchia di crittografia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Con la chiave master del servizio vengono protette direttamente o indirettamente tutte le altre chiavi nell'albero. Se non è possibile decrittografare una chiave dipendente durante un ripristino forzato, i dati protetti da tale chiave andranno perduti.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
È richiesta l'autorizzazione CONTROL SERVER per il server.  
  
## <a name="using-transact-sql"></a>Uso di Transact-SQL  
  
### <a name="to-restore-the-service-master-key"></a>Per ripristinare la chiave master del servizio  
  
1. Recuperare una copia della chiave master del servizio da un supporto di backup fisico o da una directory nel file system locale.  
  
2. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
3. Sulla barra Standard fare clic su **Nuova query**.  
  
4. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql
    -- Restores the service master key from a backup file.  
    RESTORE SERVICE MASTER KEY   
        FROM FILE = 'c:\temp_backups\keys\service_master_key'   
        DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
    GO  
    ```  
  
    > [!NOTE]  
    > Il percorso del file della chiave e della password della chiave (se esistente) sarà diverso da quello indicato in precedenza. Assicurarsi che entrambi siano specifici della configurazione della chiave e del server in uso.
