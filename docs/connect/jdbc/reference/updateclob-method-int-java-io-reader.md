---
description: Metodo updateClob (int, java.io.Reader)
title: Metodo updateClob (int, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: df60fbf1-44b2-4658-84a5-5cb129ce2dc6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98920219bfde773b9f8297501e16ecc43fcda2f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188220"
---
# <a name="updateclob-method-int-javaioreader"></a>Metodo updateClob (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aggiorna la colonna designata mediante l'oggetto Reader specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void updateClob(int columnIndex,  
                        java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Parametri  
 *columnIndex*  
  
 Valore **int** che indica l'indice di colonna.  
  
 *reader*  
  
 Oggetto Reader.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo updateClob viene specificato dal metodo updateClob nell'interfaccia java.sql.ResultSet.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo updateClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)   
 [Membri di SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
