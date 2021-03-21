---
title: Creare la chiave master di un database | Microsoft Docs
description: Creare la chiave master di un database in SQL Server usando Transact-SQL. Assicurarsi di avere le autorizzazioni necessarie.
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], creating
ms.assetid: 8cb24263-e97d-4e4d-9429-6cf494a4d5eb
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b7b6112aa855fdedbd1fd227bdcd2d8f81f09ad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755231"
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
