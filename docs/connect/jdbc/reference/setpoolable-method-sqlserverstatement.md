---
description: Metodo setPoolable (SQLServerStatement)
title: Metodo setPoolable (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: f0f798c8-cafb-4acc-b85d-2e0059c91d92
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2ae78a9cfeaf4f6391f5c9e74899c90ff50203f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178710"
---
# <a name="setpoolable-method-sqlserverstatement"></a>Metodo setPoolable (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Richiede che un'istruzione sia o meno inserita in un pool.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setPoolable(boolean poolable) throws SQLException  
```  
  
#### <a name="parameters"></a>Parametri  
 *poolable*  
  
 Se **true**, richiede che l'istruzione sia inserita in un pool. Se **false**, richiede che l'istruzione non sia inserita in un pool.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Il valore specificato nel parametro *poolable* è un hint per l'implementazione del pool di istruzioni che indica se l'applicazione vuole che l'istruzione sia inserita in un pool. L'utilizzo dell'hint sarà deciso tramite la gestione del pool di istruzioni.  
  
 Un valore del pool di istruzioni viene applicato sia alle cache interne dell'istruzione implementate dal driver sia a quelle esterne implementate dai server applicazioni e da altre applicazioni.  
  
 Per impostazione predefinita, un oggetto SQLServerStatement non può essere inserito in un pool al momento della creazione. Gli oggetti SQLServerPreparedStatement e SQLServerCallableStatement possono essere inseriti in un pool al momento della creazione.  
  
 L'eccezione [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) viene generata se questo metodo viene chiamato su un'istruzione chiusa.  
  
 L'oggetto [isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md) restituisce un valore che indica se l'oggetto può essere inserito in un pool.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
