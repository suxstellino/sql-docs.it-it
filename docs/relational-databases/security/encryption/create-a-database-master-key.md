---
title: Creare la chiave master di un database | Microsoft Docs
description: Creare la chiave master di un database in SQL Server usando Transact-SQL. Assicurarsi di avere le autorizzazioni necessarie.
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], creating
ms.assetid: 8cb24263-e97d-4e4d-9429-6cf494a4d5eb
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f97dbd4425b990cd2ec433afa817081683b38f45
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251416"
---
# <a name="create-a-database-master-key"></a>Creazione della chiave master di un database

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In questo argomento viene descritto come creare una chiave master del database in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)].

## <a name="security"></a>Sicurezza

### <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione CONTROL per il database.

## <a name="using-transact-sql"></a>Uso di Transact-SQL

### <a name="to-create-a-database-master-key"></a>Per creare la chiave master di un database

1. Scegliere una password per la crittografia della copia della chiave master che verrà archiviata nel database.
2. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].
3. Espandere **Database di sistema**, fare clic con il pulsante destro del mouse su `master` e quindi scegliere **Nuova query**.
4. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.

   ```sql
     -- Creates the master key.
     -- The key is encrypted using the password "23987hxJ#KL95234nl0zBe".  
     CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   ```

Per altre informazioni, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).
