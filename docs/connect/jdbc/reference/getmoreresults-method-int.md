---
description: Metodo getMoreResults (int)
title: Metodo getMoreResults (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getMoreResults (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6419e5a8-8b3a-4d5b-8226-95865c52c723
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e29acc97b946253c30455ac4877e50de51449acd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162726"
---
# <a name="getmoreresults-method-int"></a>Metodo getMoreResults (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Passa al risultato successivo di questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) e gestisce qualsiasi oggetto del set di risultati attualmente aperto in base alle istruzioni indicate dalla modalità specificata.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final boolean getMoreResults(int mode)  
```  
  
#### <a name="parameters"></a>Parametri  
 *mode*  
  
 Valore **int** che indica come gestire oggetti del set di risultati attualmente aperti. Deve essere una delle costanti seguenti:  
  
 CLOSE_CURRENT_RESULT  
  
 KEEP_CURRENT_RESULT (non supportata dal driver JDBC)  
  
 CLOSE_ALL_RESULTS  
  
## <a name="return-value"></a>Valore restituito  
 **true** se il risultato restituito è un set di risultati. In caso contrario, **false**.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getMoreResults viene specificato dal metodo getMoreResults nell'interfaccia java.sql.Statement.  
  
 Se il metodo getMoreResults viene chiamato prima che vengano recuperati i risultati, si comporta come specificato dall'argomento *mode* e passa al risultato successivo.  
  
> [!NOTE]  
>  Il driver JDBC non supporta l'utilizzo della costante KEEP_CURRENT_RESULT. Se utilizzata, verrà generata un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getMoreResults &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)   
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
