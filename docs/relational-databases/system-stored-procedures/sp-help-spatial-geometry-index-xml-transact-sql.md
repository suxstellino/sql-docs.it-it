---
description: sp_help_spatial_geometry_index_xml (Transact-SQL)
title: sp_help_spatial_geometry_index_xml (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_index_xml_TSQL
- sp_help_spatial_geometry_index_xml
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index_xml procedure
ms.assetid: 9668ae6d-9ed5-418e-bb9a-9e7b66f7dd16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0630c37d00e6f9d8496d1fd0b0786987c46e96f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185391"
---
# <a name="sp_help_spatial_geometry_index_xml-transact-sql"></a>sp_help_spatial_geometry_index_xml (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce i nomi e i valori per un set specificato di proprietà relative a un indice spaziale di **geometria** . È possibile scegliere di restituire un set principale di proprietà oppure tutte le proprietà dell'indice.  
  
 I risultati vengono restituiti in un frammento XML in cui vengono visualizzati il nome e valore delle proprietà selezionate.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ]'{ 0 | 1 }]   
     [ , [ @query_sample = ] 'query_sample' ]   
     [ ,.[ @xml_output = ] 'xml_output' ]   
```  
  
## <a name="arguments"></a>Argomenti  
 Vedere [argomenti e proprietà delle stored procedure per gli indici spaziali](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="properties"></a>Proprietà  
 Vedere [argomenti e proprietà delle stored procedure per gli indici spaziali](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere un membro del ruolo **public** . È necessario disporre dell'autorizzazione READ ACCESS per il server e l'oggetto.  
  
## <a name="remarks"></a>Commenti  
 Le proprietà che contengono valori NULL non sono incluse nel set XML restituito.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene usato `sp_help_spatial_geometry_index_xml` per esaminare l'indice spaziale **SIndx_SpatialTable_geometry_col2** definito nella tabella **geometry_col** per l'esempio di query specificato in **\@ QS**. L'esempio restituisce le proprietà principali dell'indice specificato in un frammento XML in cui vengono visualizzati il nome e il valore delle proprietà selezionate.  
  
 Viene quindi eseguita una [query XQuery](../../xquery/xquery-basics.md) sul set di risultati, restituendo una proprietà specifica.  
  
```  
DECLARE @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
DECLARE @x xml;  
EXEC sp_help_spatial_geometry_index_xml 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs, @x output;  
SELECT @x.value('(/Primary_Filter_Efficiency/text())[1]', 'float');  
```  
  
 Analogamente a [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md), questo stored procedure fornisce accesso a livello di codice più semplice alle proprietà di un indice spaziale e segnala il set di risultati in formato XML.  
  
## <a name="requirements"></a>Requisiti  
  
## <a name="see-also"></a>Vedere anche  
 [Argomenti e proprietà delle stored procedure dell'indice spaziale](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)   
 [Stored procedure per indici spaziali](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Dati spaziali &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Nozioni fondamentali su XQuery](../../xquery/xquery-basics.md)   
 [Guida di riferimento al linguaggio XQuery](../../xquery/xquery-language-reference-sql-server.md)  
  
