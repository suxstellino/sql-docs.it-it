---
description: ALTER PARTITION SCHEME (Transact-SQL)
title: ALTER PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER PARTITION SCHEME
- ALTER_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PARTITION SCHEME statement
- partition schemes [SQL Server], modifying
- modifying partition schemes
- adding filegroups
- NEXT USED filegroups
ms.assetid: f01d6880-9800-4cfb-8d11-d4be21efc8ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cbf8b1f76fc50438ff403c1b2703067ddb7cee53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204110"
---
# <a name="alter-partition-scheme-transact-sql"></a>ALTER PARTITION SCHEME (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Aggiunge un filegroup a uno schema di partizione oppure modifica la designazione del filegroup NEXT USED per lo schema di partizione. 

>[!NOTE]
>Nel database SQL di Azure sono supportati solo i filegroup primari.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER PARTITION SCHEME partition_scheme_name   
NEXT USED [ filegroup_name ] [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *partition_scheme_name*  
 Nome dello schema di partizione che si desidera modificare.  
  
 *filegroup_name*  
 Specifica il filegroup che si desidera contrassegnare come NEXT USED nello schema di partizione. Questo significa che il filegroup accetterà una nuova partizione creata tramite un'istruzione [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
 In uno schema di partizione è possibile designare come NEXT USED un solo filegroup. È possibile specificare un filegroup non vuoto. Se viene specificato *filegroup_name* e attualmente nessun filegroup è contrassegnato come NEXT USED, il contrassegno NEXT USED verrà applicato a *filegroup_name*. Se viene specificato *filegroup_name* ed esiste già un filegroup con la proprietà NEXT USED, questo contrassegno verrà trasferito dal filegroup esistente a *filegroup_name*.  
  
 Se *filegroup_name* viene omesso ed esiste già un filegroup con la proprietà NEXT USED, questo filegroup perderà lo stato NEXT USED, in modo che non esistano filegroup NEXT USED in *filegroup_name*.  
  
 Se *filegroup_name* viene omesso e non esistono filegroup contrassegnati come NEXT USED, ALTER PARTITION SCHEME restituirà un avviso.  
  
## <a name="remarks"></a>Osservazioni  
 Tutti i filegroup a cui viene applicata l'istruzione ALTER PARTITION SCHEME devono essere online.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire l'istruzione ALTER PARTITION SCHEME, è possibile utilizzare le autorizzazioni seguenti:  
  
-   Autorizzazione ALTER ANY DATASPACE. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
-   Autorizzazione CONTROL o ALTER per il database nel quale viene creato lo schema di partizione.  
  
-   Autorizzazione CONTROL SERVER o ALTER ANY DATABASE per il server del database nel quale è stato creato lo schema di partizione.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente si presuppone che lo schema di partizione `MyRangePS1` e il filegroup `test5fg` esistano già nel database corrente.  
  
```sql  
ALTER PARTITION SCHEME MyRangePS1  
NEXT USED test5fg;  
```  
  
 Il filegroup `test5fg` riceverà tutte le partizioni aggiuntive di una tabella o un indice partizionato create tramite un'istruzione ALTER PARTITION FUNCTION.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
