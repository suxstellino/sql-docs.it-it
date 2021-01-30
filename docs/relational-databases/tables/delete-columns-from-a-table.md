---
description: Eliminare le colonne da una tabella
title: Eliminare le colonne da una tabella | Microsoft Docs
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], deleting
- removing columns
- deleting columns
- dropping columns
ms.assetid: 0d8f6e4f-bc71-4fa3-8615-74249c8e072d
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fb537c503ec66c368c6c7e12e4740d9db1224aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160273"
---
# <a name="delete-columns-from-a-table"></a>Eliminare le colonne da una tabella

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene descritta la modalità di eliminazione delle colonne tabella in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

> [!CAUTION]
> Quando si elimina una colonna da una tabella, tale colonna e tutti i dati in essa contenuti vengono eliminati.

 **Contenuto dell'articolo**

- **Prima di iniziare:**

   [Limitazioni e restrizioni](#Restrictions)

   [Sicurezza](#Security)

- **Per eliminare una colonna da una tabella:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

Non è possibile eliminare una colonna che dispone di un vincolo CHECK. È necessario eliminare prima questo vincolo.

Non è possibile eliminare una colonna che dispone di vincoli PRIMARY KEY o FOREIGN KEY o altre dipendenze a parte quando si utilizza Progettazione tabelle. Quando si utilizza Esplora oggetti o [!INCLUDE[tsql](../../includes/tsql-md.md)], è necessario prima rimuovere tutte le dipendenze sulla colonna.

### <a name="security"></a><a name="Security"></a> Sicurezza

#### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

È necessario disporre dell'autorizzazione ALTER per la tabella.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio

### <a name="to-delete-columns-by-using-object-explorer"></a>Per eliminare le colonne utilizzando Esplora oggetti.

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. In **Esplora oggetti** individuare la tabella da cui si vogliono eliminare colonne ed espanderla per esporre i nomi delle colonne.
3. Fare clic con il pulsante destro del mouse sulla colonna da eliminare e scegliere **Elimina**.
4. Nella finestra di dialogo **Elimina oggetto** fare clic su **OK**.

Se la colonna contiene vincoli o altre dipendenze, un messaggio di errore sarà visualizzato nella finestra di dialogo **Elimina oggetto** . Risolvere l'errore eliminando i vincoli a cui si fa riferimento.

### <a name="to-delete-columns-by-using-table-designer"></a>Per eliminare le colonne utilizzando Progettazione tabelle.

1. In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella da cui si vogliono eliminare colonne, quindi scegliere **Progettazione**.
2. Fare clic con il pulsante destro del mouse sulla colonna che si vuole eliminare e scegliere **Elimina colonna** dal menu di scelta rapida.
3. Se la colonna fa parte di una relazione (FOREIGN KEY o PRIMARY KEY), verrà visualizzato un messaggio in cui viene chiesto di confermare l'eliminazione delle colonne selezionate e delle corrispondenti relazioni. Scegliere **Sì**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL

### <a name="to-delete-columns"></a>Per eliminare le colonne

L'esempio seguente mostra come eliminare una colonna.

```sql
ALTER TABLE dbo.doc_exb DROP COLUMN column_b;
```

Se la colonna contiene vincoli o altre dipendenze, verrà restituito un messaggio di errore. Risolvere l'errore eliminando i vincoli a cui si fa riferimento.

Per altri esempi, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).

## <a name="FollowUp"></a>
