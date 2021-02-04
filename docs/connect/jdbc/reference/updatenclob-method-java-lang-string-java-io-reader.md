---
description: Metodo updateNClob (java.lang.String, java.io.Reader)
title: Metodo updateNClob (java.lang.String, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 87621ca7-e64a-49e2-b9c2-551390adaa26
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7b2de4f83b789f320a05d028d1a643216fd5ef73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194937"
---
# <a name="updatenclob-method-javalangstring-javaioreader"></a>Metodo updateNClob (java.lang.String, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata tramite l'oggetto Reader specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateNClob(java.lang.String columnLabel,  
                        java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnLabel*  
  
 Valore **String** che indica l'etichetta della colonna.  
  
 *reader*  
  
 Oggetto Reader.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateNClob viene specificato dal metodo updateNClob nell'interfaccia java.sql.ResultSet.  
  
 Questo metodo è supportato solo nelle colonne **nvarchar(max)**, **ntext** e **xml**. L'utilizzo di questo metodo su qualsiasi altro tipo di dati genererà un'eccezione.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateNClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
