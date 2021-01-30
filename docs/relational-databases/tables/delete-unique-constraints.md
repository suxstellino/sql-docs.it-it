---
description: Eliminazione di vincoli univoci
title: Eliminare vincoli univoci | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing constraints
- UNIQUE constraints [SQL Server], deleting
- constraints [SQL Server], deleting
- deleting constraints
- constraints [SQL Server], unique
ms.assetid: 71e563fc-f5d7-4c2e-a42f-f0695a831f32
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8cba9e925111bf7939798f80cce5d7eeb442f6d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179756"
---
# <a name="delete-unique-constraints"></a>Eliminazione di vincoli univoci

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  È possibile eliminare un vincolo univoco in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'eliminazione di un vincolo univoco consente di rimuovere il requisito di univocità per i valori immessi nella colonna o nella combinazione di colonne inclusa nell'espressione del vincolo ed elimina l'indice univoco corrispondente.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per eliminare un vincolo univoco:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-delete-a-unique-constraint-using-object-explorer"></a>Per eliminare un vincolo univoco utilizzando Esplora oggetti  
  
1.  In Esplora oggetti, espandere la tabella contenente il vincolo univoco, quindi espandere la cartella **Vincoli**.  
  
2.  Fare clic con il pulsante destro del mouse sulla chiave e scegliere **Elimina**.  
  
3.  Nella finestra di dialogo **Elimina oggetto** verificare che venga specificata la chiave corretta e fare clic su **OK**.  
  
#### <a name="to-delete-a-unique-constraint-using-table-designer"></a>Per eliminare un vincolo univoco utilizzando Progettazione tabelle  
  
1.  In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella con il vincolo UNIQUE e selezionare **Progetta**.  
  
2.  Scegliere **Indici/chiavi** nel menu **Progettazione tabelle**.  
  
3.  Nella finestra di dialogo **Indici/chiavi** selezionare la chiave univoca dall'elenco **Chiave o indice primario/univoco selezionato** .  
  
4.  Fare clic su **Elimina**.  
  
5.  Nel menu **File** fare clic su **Salva** _nome tabella_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-delete-a-unique-constraint"></a>Per eliminare un vincolo univoco  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    -- Return the name of unique constraint.  
    SELECT name  
    FROM sys.objects  
    WHERE type = 'UQ' AND OBJECT_NAME(parent_object_id) = N' DocExc';  
    GO  
    -- Delete the unique constraint.  
    ALTER TABLE dbo.DocExc   
    DROP CONSTRAINT UNQ_ColumnB_DocExc;  
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) e [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
###  <a name="TsqlExample"></a>  
