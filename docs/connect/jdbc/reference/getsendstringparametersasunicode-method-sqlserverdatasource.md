---
description: Metodo getSendStringParametersAsUnicode (SQLServerDataSource)
title: Metodo getSendStringParametersAsUnicode (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getSendStringParametersAsUnicode
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3836d0ab-c3fb-41ff-bb89-10389594ae51
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2bcac762b3874fd95fd03286d1a52a5654d31228
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162304"
---
# <a name="getsendstringparametersasunicode-method-sqlserverdatasource"></a>Metodo getSendStringParametersAsUnicode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce un valore **booleano** che indica se è abilitato l'invio di parametri di stringa al server in formato UNICODE.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public boolean getSendStringParametersAsUnicode()  
```  
  
## <a name="return-value"></a>Valore restituito  
 **true** se i parametri di stringa vengono inviati al server in formato UNICODE. In caso contrario, **false**.  
  
## <a name="remarks"></a>Osservazioni  
 Se la proprietà sendStringParametersAsUnicode è impostata su **true**, che è il valore predefinito, i parametri di stringa vengono inviati al server in formato UNICODE. Se sendStringParametersAsUnicode è impostato su **false**, i parametri di stringa vengono inviati al server in formato ASCII/MBCS, non in Unicode. Se la proprietà sendStringParametersAsUnicode non è impostata, getSendStringParametersAsUnicode restituisce il valore predefinito **true**.  
  
 Per altre informazioni sulla proprietà di connessione sendStringParametersAsUnicode, vedere [Impostazione delle proprietà di connessione](../../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
