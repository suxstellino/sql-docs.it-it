---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 308e11275807ff11e3700bfcb443238c8c6ffeef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098669"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabella **MSdbms_datatype_mapping** contiene i mapping dei tipi di dati consentiti dal tipo di dati nel sistema di gestione di database di origine (DBMS) a uno o più tipi di dati specifici nel sistema DBMS di destinazione. Questa tabella è archiviata nel database **msdb** e viene utilizzata per la replica di database eterogenei.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|Identifica ogni mapping univoco di tipi di dati.|  
|**map_id**|**int**|Identifica il tipo di dati di origine.|  
|**dest_datatype_id**|**int**|Identifica il tipo di dati di destinazione.|  
|**dest_precision**|**bigint**|Definisce la precisione del tipo di dati di destinazione, dove il valore NULL indica che la precisione non viene utilizzata e il valore **-1** indica che viene utilizzata la precisione del tipo di dati di origine.|  
|**dest_scale**|**int**|Definisce la scala del tipo di dati di destinazione, dove il valore NULL indica che la scala non viene utilizzata e il valore **-1** indica che viene utilizzata la scala del tipo di dati di origine.|  
|**dest_length**|**bigint**|Definisce la lunghezza del tipo di dati di destinazione, dove il valore NULL indica che la lunghezza non viene utilizzata e il valore **-1** indica che viene utilizzata la lunghezza del tipo di dati di origine.|  
|**dest_nullable**|**bit**|Specifica se la colonna di destinazione nel mapping ammette valori NULL. Il valore NULL indica che questa definizione non è necessaria.|  
|**dest_createparams**|**int**|Mappa di bit che descrive quale combinazione di lunghezza, precisione e scala è applicabile a ogni tipo di dati. Sono disponibili i valori seguenti:<br /><br /> **0x1** = Precision.<br /><br /> **0x2** = scala.<br /><br /> **0x4** = length.|  
  
## <a name="see-also"></a>Vedere anche  
 [Replica di database eterogenei](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Specificare i mapping dei tipi di dati per un server di pubblicazione Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Viste della replica &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
