---
description: Modificare un indice
title: Modificare un indice | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 0b653936dd08b5c124fd1249697dec35e9ae7ba9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766311"
---
# <a name="modify-an-index"></a>Modificare un indice
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  In questo argomento viene descritto come modificare un indice in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!IMPORTANT]  
>  Gli indici creati come risultato di un vincolo PRIMARY KEY o UNIQUE non possono essere modificati con questo metodo. È necessario invece modificare il vincolo.  
  
 **Contenuto dell'articolo**  
  
-   **Per modificare un indice usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>Per modificare un indice  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , quindi espandere questa istanza.  
  
2.  Espandere **Database**, espandere il database a cui appartiene la tabella e quindi espandere **Tabelle**.  
  
3.  Espandere la tabella a cui appartiene l'indice e quindi espandere **Indici**.  
  
4.  Fare clic con il pulsante destro del mouse sull'indice da modificare l'indice e scegliere **Proprietà**.  
  
5.  Nella finestra di dialogo **Proprietà indice** apportare le modifiche desiderate. È ad esempio possibile aggiungere o rimuovere una colonna dalla chiave di indice o modificare l'impostazione di un'opzione di indice.  
  
#### <a name="to-modify-index-columns"></a>Per modificare le colonne indice  
  
1.  Per aggiungere, rimuovere o modificare la posizione di una colonna indice, selezionare la pagina **Generale** della finestra di dialogo **Proprietà indice** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-modify-an-index"></a>Per modificare un indice  
  
Nell'esempio seguente viene eliminato e ricreato un indice esistente nella colonna `ProductID` della tabella `Production.WorkOrder` nel database AdventureWorks usando l'opzione `DROP_EXISTING`. Vengono inoltre impostate le opzioni `FILLFACTOR` e `PAD_INDEX` .  
  
[!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
Nell'esempio seguente viene usato ALTER INDEX per impostare diverse opzioni sull'indice `AK_SalesOrderHeader_SalesOrderNumber`.  
  
[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>Per modificare le colonne indice  
  
1.  Per aggiungere, rimuovere o modificare la posizione di una colonna dell'indice, è necessario eliminare e ricreare l'indice.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Impostare le opzioni di indice](../../relational-databases/indexes/set-index-options.md)   
 [Ridenominazione di indici](../../relational-databases/indexes/rename-indexes.md)  
  
  
