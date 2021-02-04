---
description: Costruttore SQLServerException (java.lang.Object, java.lang.String, java.lang.String, StreamError, boolean)
title: Costruttore SQLServerException (java.lang.Object, java.lang.String, java.lang.String, StreamError, boolean) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c2d6594123eb26ce92357e93cebdb8fdb261605d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178206"
---
# <a name="sqlserverexception-constructor-javalangobject-javalangstring-javalangstring-streamerror-boolean"></a>Costruttore SQLServerException (java.lang.Object, java.lang.String, java.lang.String, StreamError, boolean)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Inizializza una nuova istanza della classe [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) quando viene specificato un valore **object**, un oggetto **string**, un oggetto **string**, un oggetto **StreamError** e un valore **boolean**.

## <a name="syntax"></a>Sintassi  
  
```  

public SQLServerException(java.lang.Object obj,
            java.lang.String errText,
            java.lang.String errState,
            StreamError streamError,
            boolean bStack)

            
```  
  
#### <a name="parameters"></a>Parametri  
 *obj*  
  
 Buffer di I/O che ha generato l'eccezione.

 *errText*  
  
 Stringa contenente il testo dell'errore.
  
 *sqlState*  
  
 Oggetto enum che contiene lo stato SQL.
 
 *streamError*  
  
 Oggetto StreamError che contiene i dettagli sull'errore.
 
 *bStack*  
  
 Valore booleano che indica se deve essere generata la traccia dello stack.
  
## <a name="see-also"></a>Vedere anche  
 [Costruttori di SQLServerException](../../../connect/jdbc/reference/sqlserverexception-constructors.md)   
 [Membri di SQLServerException](../../../connect/jdbc/reference/sqlserverexception-members.md)   
 [Classe SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
  
