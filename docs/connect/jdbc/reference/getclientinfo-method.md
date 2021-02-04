---
description: Metodo getClientInfo ()
title: Metodo getClientInfo () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b06a5ced-b760-4c78-b17e-854ce95a1a5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d29f906bc1c1bc14c9b25d796fe5fb4db10602ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176098"
---
# <a name="getclientinfo-method-"></a>Metodo getClientInfo ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un elenco che contiene il nome e il valore corrente di ogni proprietà delle informazioni client supportata dal driver JDBC.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.util.Properties getClientInfo()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto Properties che contiene il nome e il valore corrente di ogni proprietà delle informazioni client supportata dal driver.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getClientInfo viene specificato dal metodo getClientInfo nell'interfaccia java.sql.Connection.  
  
 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] non supporta alcuna proprietà delle informazioni client. Di conseguenza, questo metodo restituisce un oggetto Properties vuoto.  
  
 Analogamente, le applicazioni possono usare il metodo [getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md) della classe [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) per recuperare un elenco delle proprietà delle informazioni client supportate dal driver. Il metodo [getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md) restituisce un set di risultati vuoto.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo getClientInfo &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)   
 [Membri di SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
