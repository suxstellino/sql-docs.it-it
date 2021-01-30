---
description: EnvelopeCenter (tipo di dati geography)
title: EnvelopeCenter (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EnvelopeCenter
- EnvelopeCenter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeCenter method
ms.assetid: dee9d807-faad-45b8-b3f3-7e8aa7d07147
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 59428c1d87ab1a4c855e4890a113f96318c8c164
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211432"
---
# <a name="envelopecenter-geography-data-type"></a>EnvelopeCenter (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un punto che è possibile usare come centro di un cerchio di delimitazione per l'istanza **geography**.  
  
Ogni punto nell'istanza viene descritto come un vettore. Per determinare il cerchio di delimitazione, il vettore si estende dal centro della terra al punto sulla superficie della terra. Il punto centrale del cerchio di delimitazione viene calcolato come media di tutti i vettori. Per i cicli chiusi, in un'istanza **Polygon** o **LineString** il primo e l'ultimo punto vengono usati solo una volta.  
  
Questo metodo con tipo di dati **geography** supporta le istanze **FullGlobe** o le istanze spaziali con dimensioni maggiori di un emisfero.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
EnvelopeCenter( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito: **geography**  
  
Tipo CLR restituito: **SqlGeography**  
  
## <a name="remarks"></a>Osservazioni  
Questo metodo restituisce un elemento **point**. Se usato con `EnvelopeAngle()`, `EnvelopeCenter()` restituisce un cerchio di delimitazione di un'istanza **geography**.  
  
> [!NOTE]  
>  `EnvelopeCenter()` restituisce un cerchio di delimitazione per un'istanza **geography**, ma i risultati non garantiscono l'ottenimento del cerchio di delimitazione minimo. Al contrario, il metodo `STEnvelope()` con tipo di dati **geometry** garantisce la restituzione del rettangolo di selezione minimo quando viene applicato a un'istanza **geometry**.  
  
In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive, restituisce il centro del cerchio che rappresenta la busta di questa istanza come **point**. Per tutti gli oggetti di grandi dimensioni, come definito da `EnvelopeAngle()` = 180, `EnvelopeCenter()` restituirà (90,0).  
  
Questo metodo non è preciso.  
  
## <a name="examples"></a>Esempi  
  
```  
DECLARE @g geography = 'LINESTRING(-120 45, -120 0, -90 0)';  
SELECT @g.EnvelopeCenter().ToString();  
```  
  
## <a name="see-also"></a>Vedere anche  
[Metodi estesi sulle istanze di geografia](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
[EnvelopeAngle &#40;tipo di dati geography&#41;](../../t-sql/spatial-geography/envelopeangle-geography-data-type.md)  
  
  
