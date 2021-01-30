---
description: sp_help_spatial_geometry_histogram (Transact-SQL)
title: sp_help_spatial_geometry_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_histogram
- sp_help_spatial_geometry_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_histogram
ms.assetid: 036aaf61-df3e-40f7-aa4e-62983c5a37bd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75609570d3c021b350fc06cbaf35f32dee4bd8ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183603"
---
# <a name="sp_help_spatial_geometry_histogram-transact-sql"></a>sp_help_spatial_geometry_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Semplifica l'impostazione delle chiavi dei parametri di griglia e del rettangolo di selezione per un indice spaziale.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_spatial_geometry_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @xmin = ] 'minx' ]   
     [ , [ @ymin = ] 'miny' ]   
     [ ,.[ @xmax = ] 'maxx' ]  
     [ , [ @ymax = ] 'maxy' ]  
     [ , [ @sample = ] 'sample' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tabname = ] 'tabname'` Nome completo o non qualificato della tabella per la quale è stato specificato l'indice spaziale.  
  
 Le virgolette sono necessarie solo se viene specificata una tabella qualificata. Nel caso di un nome completo, ovvero contenente un nome di database, il nome del database deve corrispondere a quello del database corrente. *TabName* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @colname = ] 'colname'` Nome della colonna spaziale specificata. *colname* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @resolution = ] 'resolution'` Risoluzione del rettangolo di delimitazione. I valori validi sono compresi tra 10 e 5000. la *risoluzione* è di **tinyint** e non prevede alcun valore predefinito.  
  
`[ @xmin = ] 'xmin'` Proprietà del rettangolo di delimitazione X-Minimum. *xmin* è di tipo **float** e non prevede alcun valore predefinito.  
  
`[ @ymin = ] 'ymin'` Proprietà del rettangolo delimitatore Y minimo. *yMin* è di tipo **float** e non prevede alcun valore predefinito.  
  
`[ @xmax = ] 'xmax'` Proprietà del rettangolo di delimitazione X-Maximum. *Xmax* è di tipo **float** e non prevede alcun valore predefinito.  
  
`[ @ymax = ] 'ymax'` Proprietà del valore massimo di Y del rettangolo di delimitazione. *yMax* è di tipo **float** e non prevede alcun valore predefinito.  
  
`[ @sample = ] 'sample'` Percentuale della tabella utilizzata. I valori validi sono compresi tra 0 e 100. *esempio* è un valore **float**. Il valore predefinito è 100.  
  
## <a name="property-valuereturn-value"></a>Valore proprietà/Valore restituito  
 Viene restituito un valore di tabella. Nella griglia seguente viene descritto il contenuto delle colonne della tabella.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Rappresenta l'ID univoco di ciascuna cella. Il conteggio inizia da 1.|  
|**cella**|**geometry**|Poligono rettangolare che rappresenta ciascuna cella. La forma della cella è identica alla forma della cella utilizzata per l'indicizzazione spaziale.|  
|**row_count**|**bigint**|Indica il numero di oggetti spaziali che toccano o contengono la cella.|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere un membro del ruolo **public** . È necessario disporre dell'autorizzazione READ ACCESS per il server e l'oggetto.  
  
## <a name="remarks"></a>Commenti  
 Nella scheda spaziale di SSMS viene illustrata una rappresentazione grafica dei risultati. È possibile eseguire una query sui risultati rispetto alla finestra spaziale per ottenere un numero approssimativo di risultati. Gli oggetti nella tabella potrebbero riguardare più di una cella, pertanto la somma delle celle potrebbe essere maggiore del numero di oggetti effettivi.  
  
 È possibile che una riga aggiuntiva venga aggiunta al set di risultati contenente il numero di oggetti esterni al rettangolo di selezione o che toccano il bordo dello stesso. L'oggetto **Cellid** di questa riga è 0 e la **cella** di questa riga contiene un oggetto **LineString** che rappresenta il rettangolo di delimitazione. Questa riga rappresenta l'intero spazio esterno al rettangolo di selezione.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creata una tabella di esempio, quindi viene chiamato **sp_help_spatial_geometry_histogram** nella tabella.  
  
 `USE AdventureWorksDW2012`  
  
 `GO`  
  
 `-- Set database compatibility for circular arc segments`  
  
 `ALTER DATABASE AdventureWorksDW2012`  
  
 `SET COMPATIBILITY_LEVEL = 110;`  
  
 `GO`  
  
 `-- Create table to execute sp_help_spatial_geometry_histogram on`  
  
 `CREATE TABLE TownSites`  
  
 `(`  
  
 `Location geometry NULL,`  
  
 `SiteName nvarchar(50) NULL`  
  
 `)`  
  
 `GO`  
  
 `-- Insert site data into table`  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('POINT(0 0)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Booth Map';`  
  
 `SET @g = geometry::Parse('POLYGON((1 1, 1 2, 2 2, 2 1, 1 1))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Town Hall';`  
  
 `SET @g = geometry::Parse('CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-1 0, 0 -1, 1 0),(1 0, 1 2, -1 0)))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Park';`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(1 5, 2 2, 5 1)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Road';`  
  
 `-- Call proc to see data within bounding box`  
  
 `EXEC sp_help_spatial_geometry_histogram @tabname = TownSites, @colname = Location, @resolution = 64, @xmin = -2, @ymin = -2, @xmax = 3, @ymax = 3, @sample = 100;`  
  
 `GO`  
  
 `DROP TABLE TownSites;`  
  
 `GO`  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di indice spaziale &#40;&#41;Transact-SQL ](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
