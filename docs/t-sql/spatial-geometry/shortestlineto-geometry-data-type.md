---
description: ShortestLineTo (tipo di dati geometry)
title: ShortestLineTo (tipo di dati geometry) | Microsoft Docs
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
- ShortestLineTo method (geometry)
ms.assetid: 39a2d0e4-4f93-4e94-a27e-6ad9537cfe74
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 937a631ca32ab7d63dece2603d5cf1357fdd1567
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200410"
---
# <a name="shortestlineto-geometry-data-type"></a>ShortestLineTo (tipo di dati geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un'istanza **LineString** con due punti che rappresentano la distanza più breve tra le due istanze **geometry**. La lunghezza dell'istanza **LineString** restituita è la distanza tra le due istanze **geometry**.
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.ShortestLineTo ( geometry_other )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *geometry_other*  
 Seconda istanza **geometry** da cui l'istanza **geometry** chiamante prova a determinare la distanza più breve.  
  
## <a name="return-types"></a>Tipi restituiti  
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geometry**  
  
 Tipo CLR restituito: **SqlGeometry**  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo restituisce un'istanza **LineString** con endpoint che si trovano sui bordi delle due istanze **geometry** non intersecate messe a confronto. La lunghezza dell'istanza **LineString** restituita corrisponde alla distanza minore tra le due istanze **geometry**. Viene restituita un'istanza **LineString** vuota quando le due istanze **geometry** si intersecano.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calling-shortestlineto-on-non-intersecting-instances"></a>R. Chiamata di ShortestLineTo() in istanze non intersecate  
 In questo esempio viene individuata la distanza più breve tra un'istanza `CircularString` e un'istanza `LineString` e viene restituita l'istanza `LineString` che collega i due punti:  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(-4 7, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
### <a name="b-calling-shortestlineto-on-intersecting-instances"></a>B. Chiamata di ShortestLineTo() in istanze intersecate  
 In questo esempio viene restituita un'istanza `LineString` vuota perché l'istanza `LineString` interseca l'istanza `CircularString`:  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(0 5, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
## <a name="see-also"></a>Vedere anche  
 [ShortestLineTo &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/shortestlineto-geography-data-type.md)  
  
  

