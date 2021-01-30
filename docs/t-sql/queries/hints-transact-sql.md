---
description: Hint (Transact-SQL)
title: Hints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query optimizer [SQL Server], hints
- hints [SQL Server], about hints
- SELECT statement [SQL Server], hints
- hints [SQL Server]
- INSERT statement [SQL Server], hints
- UPDATE statement [SQL Server], hints
- DELETE statement [SQL Server], hints
ms.assetid: 99412475-b0df-4264-9938-33a0b302b41a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 0da4ac18eaf7887c772bb7906026aaab9a476ea0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207681"
---
# <a name="hints-transact-sql"></a>Hint (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gli hint sono opzioni o strategie che vengono specificati per imporre un comportamento specifico a Query Processor di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'esecuzione di istruzioni SELECT, INSERT, UPDATE o DELETE. Gli hint consentono di ignorare eventuali piani di esecuzione selezionati da Query Optimizer per una query.  
  
> [!CAUTION]  
>  Poiché Query Optimizer di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seleziona in genere il piano di esecuzione migliore per una query, gli oggetti \<join_hint>, \<query_hint> e \<table_hint> devono essere usati solo se strettamente necessari ed esclusivamente da sviluppatori e amministratori di database esperti.
  
 In questa sezione vengono descritti gli hint seguenti:  
  
-   [Hint di join](../../t-sql/queries/hints-transact-sql-join.md)  
  
-   [Query Hints](../../t-sql/queries/hints-transact-sql-query.md) (Hint per la query)  
  
-   [Hint di tabella](../../t-sql/queries/hints-transact-sql-table.md)  
  
  
