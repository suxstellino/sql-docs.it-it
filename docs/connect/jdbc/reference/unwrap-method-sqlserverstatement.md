---
description: Metodo unwrap (SQLServerStatement)
title: Metodo unwrap (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ce680176-ef04-4e44-bb6c-ec50bd06e7e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 633d24d6d90166e361f21f197da27e1d27c0eee1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160901"
---
# <a name="unwrap-method-sqlserverstatement"></a>Metodo unwrap (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Parametri  
 *iface*  
  
 Classe di tipo **T** che definisce un'interfaccia.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto che implementa l'interfaccia specificata.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Il metodo [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) è definito dall'interfaccia java.sql.Wrapper, introdotta nella specifica JDBC 4.0.  
  
 È possibile che le applicazioni debbano accedere alle estensioni dell'API JDBC specifiche di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Il metodo unwrap supporta l'annullamento del wrapping nelle classi pubbliche estese dall'oggetto, se le classi espongono estensioni del fornitore.  
  
 Quando questo metodo viene chiamato, l'oggetto annulla il wrapping nella classe [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
 Per un esempio di codice, vedere [Esempio di aggiornamento di dati di grandi dimensioni](../../../connect/jdbc/updating-large-data-sample.md) o [Metodo unwrap &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md).  
  
 Per altre informazioni, vedere [Wrapper e interfacce](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo isWrapperFor &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)   
 [Membri di SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
