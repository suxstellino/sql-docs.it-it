---
description: Modificare l'ordine delle colonne in una tabella
title: Modificare l'ordine delle colonne in una tabella | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8064366c25925bf5ba6781e0afa0168a8df04fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181516"
---
# <a name="change-column-order-in-a-table"></a>Modificare l'ordine delle colonne in una tabella

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  In Progettazione tabelle è possibile modificare l'ordine delle colonne in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!CAUTION]  
>  La modifica dell'ordine delle colonne potrebbe influire su codice e applicazioni che dipendono da un ordine specifico, incluse query, viste, stored procedure, funzioni definite dall'utente e applicazioni client. è pertanto opportuno valutare attentamente ogni eventuale modifica da apportare all'ordine delle colonne prima di procedere. La procedura consigliata è specificare l'ordine nel quale le colonne vengono restituite all'applicazione e il livello della query. Non è necessario basarsi sull'utilizzo di SELECT * per restituire tutte le colonne nell'ordine previsto basato sull'ordine nel quale sono definiti nella tabella. Nelle query e nelle applicazioni, specificare sempre le colonne per nome nell'ordine nel quale si desidera visualizzarle.  
  
 **Contenuto dell'articolo**  
  
-   **Per modificare l'ordine delle colonne:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>Per modificare l'ordine delle colonne  
  
1.  In **Esplora oggetti** fare clic con il pulsante destro del mouse sulle colonne da riordinare e selezionare **Progetta**.  
  
2.  Selezionare la casella a sinistra del nome della colonna che si desidera spostare.  
  
3.  Trascinare la colonna in una posizione diversa nella tabella.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per modificare l'ordine delle colonne**  
  
 Questa attività non è supportata tramite istruzioni Transact-SQL.  
  
###  <a name="TsqlExample"></a>  
