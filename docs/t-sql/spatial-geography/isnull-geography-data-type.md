---
description: IsNull (tipo di dati geography)
title: IsNull (tipo di dati geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsNull (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull method
ms.assetid: c031074f-bfda-4584-a3bf-4e7c324f237f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5cf656f1f05188a6b765aa9eef4c36131fda56b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205412"
---
# <a name="isnull-geography-data-type"></a>IsNull (tipo di dati geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Proprietà che specifica se l'istanza **geography** è Null. Restituisce 'TRUE' se l'istanza è Null. In caso contrario, restituisce 0.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 Tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Osservazioni  
 `IsNull` può essere usato per verificare se un'istanza **geography** è Null. Questa verifica potrebbe produrre risultati piuttosto confusi, poiché restituisce 0 se l'istanza è diversa da Null, ma restituisce Null se l'istanza è Null.  
  
 Questo metodo viene usato principalmente dall'infrastruttura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È consigliabile usare il predicato T-SQL IS NULL per verificare se un'istanza **geography** è Null. Per altre informazioni sul predicato T-SQL IS NULL, vedere [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi estesi sulle istanze geografiche](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
