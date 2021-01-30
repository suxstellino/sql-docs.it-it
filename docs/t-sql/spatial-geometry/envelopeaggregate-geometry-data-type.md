---
description: EnvelopeAggregate (tipo di dati geometry)
title: EnvelopeAggregate (tipo di dati geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeAggregate method (geometry)
ms.assetid: c4c15abe-0fe9-441d-9d42-6572e264869c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5a19b29088f5680ba6c9fc8db8b3813a973ad672
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193677"
---
# <a name="envelopeaggregate-geometry-data-type"></a>EnvelopeAggregate (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un rettangolo di selezione per un set specificato di oggetti **geometry**.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
EnvelopeAggregate ( geometry_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *geometry_operand*  
 Colonna della tabella di tipo **geometry** che rappresenta un set di oggetti **geometry**.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
## <a name="exceptions"></a>Eccezioni  
 Genera un'eccezione `FormatException` in presenza di valori di input non validi. Vedere [STIsValid &#40;tipo di dati geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo restituisce **Null** quando l'input è vuoto o dispone di SRID diversi. Vedere [Identificatori SRID &#40;Spatial Reference Identifier&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)  
  
 Il metodo ignora gli input **Null**.  
  
> [!NOTE]  
>  Il metodo restituisce **Null** se tutti i valori immessi sono **Null**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituito un rettangolo di selezione per un set di oggetti in una colonna delle variabili di tabella.  
  
 ```
 -- Setup table variable for EnvelopeAggregate example 
DECLARE @Geom TABLE 
( 
shape geometry, 
shapeType nvarchar(50) 
) 
INSERT INTO @Geom(shape,shapeType) VALUES('CURVEPOLYGON(CIRCULARSTRING(2 3, 4 1, 6 3, 4 5, 2 3))', 'Circle'), 
('POLYGON((1 1, 4 1, 4 5, 1 5, 1 1))', 'Rectangle'); 
-- Perform EnvelopeAggregate on @Geom.shape column 
SELECT geometry::EnvelopeAggregate(shape).ToString() 
FROM @Geom;
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di geometria statici estesi](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
  

