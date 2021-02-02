---
title: Eseguire il backup della chiave master di un database | Microsoft Docs
description: Informazioni su come eseguire un backup della chiave master del database in SQL Server usando Transact-SQL. Questa chiave fondamentale crittografa le altre chiavi e i certificati.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], exporting
ms.assetid: 7ad9a0a0-6e4f-4f7b-8801-8c1b9d49c4d8
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 92e8753033e37561b23f2e1d15cb06cef4b662d7
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250685"
---
# <a name="back-up-a-database-master-key"></a>Backup della chiave master di un database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come eseguire il backup di una chiave master del database in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La chiave master viene usata per crittografare altre chiavi e certificati all'interno di un database. Se questa chiave viene eliminata oppure danneggiata, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] potrebbe non essere in grado di decrittografare tali chiavi e i dati crittografati con tali chiavi verranno di fatto persi. Per tale motivo, è consigliabile eseguire il backup della chiave master di un database e archiviare la copia di backup in un altro luogo adeguatamente protetto.  
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
  
- È necessario aprire e pertanto decrittografare la chiave master prima di eseguirne il backup. Se è crittografata con la chiave master del servizio, non è necessario aprire in modo esplicito la chiave master. Se invece la chiave master è crittografata solo con una password dovrà essere aperta in modo esplicito.  
  
- È consigliabile creare una copia di backup della chiave master subito dopo la creazione e archiviare il backup in una posizione esterna sicura.  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni
È richiesta l'autorizzazione CONTROL per il database.  
  
## <a name="using-sql-server-management-studio-with-transact-sql"></a>Utilizzo di SQL Server Management Studio con Transact-SQL  
  
### <a name="to-back-up-the-database-master-key"></a>Per eseguire il backup della chiave master di un database  
  
1. In [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] contenente la chiave master del database di cui si desidera eseguire il backup.  
  
2. Scegliere una password che verrà usata per crittografare la chiave master del database sul supporto di backup. Questa password è soggetta ai controlli di complessità delle password.  
  
3. Procurarsi un supporto di backup rimovibile per l'archiviazione di una copia della chiave di cui viene eseguito il backup.  
  
4. Identificare una directory NTFS in cui creare il backup della chiave. Il file specificato nel passaggio successivo verrà creato in questa directory, che è consigliabile proteggere tramite elenchi di controllo di accesso altamente restrittivi.  
  
5. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
6. Sulla barra Standard fare clic su **Nuova query**.  
  
7. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```sql
    -- Creates a backup of the "AdventureWorks2012" master key. Because this master key is not encrypted by the service master key, a password must be specified when it is opened.  
    USE AdventureWorks2012;   
    GO  
    OPEN MASTER KEY DECRYPTION BY PASSWORD = 'sfj5300osdVdgwdfkli7';   
  
    BACKUP MASTER KEY TO FILE = 'c:\temp\exportedmasterkey'   
        ENCRYPTION BY PASSWORD = 'sd092735kjn$&adsg';   
    GO  
    ```  
  
    > [!NOTE]  
    > Il percorso del file della chiave e della password della chiave (se esistente) sarà diverso da quello indicato in precedenza. Assicurarsi che entrambi siano specifici della configurazione della chiave e del server in uso.  
  
8. Copiare il file nel supporto di backup e verificare la copia.  
  
9. Archiviare il file in una posizione esterna protetta.  
  
 Per altre informazioni, vedere [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-master-key-transact-sql.md) e [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-master-key-transact-sql.md).  
