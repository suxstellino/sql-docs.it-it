---
description: Campo SSTRANSTIGHTLYCPLD (SQLServerXAResource)
title: Campo SSTRANSTIGHTLYCPLD (SQLServerXAResource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SSTRANSTIGHTLYCPLD Field (SQLServerXAResource)
apilocation:
- SSTRANSTIGHTLYCPLD Field (SQLServerXAResource)
apitype: Assembly
ms.assetid: 379857c3-9de1-4964-8782-32df317cbfbb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 70ceb436d02015e4a38d38ce6c6687eae674efeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158274"
---
# <a name="sstranstightlycpld-field-sqlserverxaresource"></a>Campo SSTRANSTIGHTLYCPLD (SQLServerXAResource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Utilizzato per consentire le transazioni XA "a regime di controllo stretto ("tightly-coupled")" che dispongono di diversi ID di transazione dei rami XA (XID), ma dello stesso ID di transazione globale (GTRID).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public static final int SSTRANSTIGHTLYCPLD  
```  
  
## <a name="field-value"></a>Valore del campo  
 Valore **int** 32768.  
  
## <a name="remarks"></a>Osservazioni  
 Ogni transazione viene identificata da un ID di transazione dei rami XA (XID) e da un ID di transazione globale (GTRID). Per consentire alle applicazioni di usare transazioni XA strettamente associate con XID differenti ma con stesso GTRID, è necessario impostare [SSTRANSTIGHTLYCPLD](../../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) sul parametro flags del metodo XAResource.start. Per altre informazioni sull'uso di questo flag, vedere [Informazioni sulle transazioni XA](../../../connect/jdbc/understanding-xa-transactions.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Campi SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-fields.md)   
 [Membri di SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [Classe SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
