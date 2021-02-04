---
description: Metodo getAttributes (SQLServerDatabaseMetaData)
title: Metodo getAttributes (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getAttributes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4dc784ed-4699-4197-9af5-6e03da80d14c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: adb3f8a2872f4e9ecd7ddf66e19031f385ada458
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165788"
---
# <a name="getattributes-method-sqlserverdatabasemetadata"></a>Metodo getAttributes (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una descrizione di un determinato attributo del tipo indicato per un tipo definito dall'utente disponibile nello schema e nel catalogo specificati.  
  
> [!NOTE]  
>  Questo metodo non è attualmente supportato da [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Se viene chiamato, restituirà sempre un set di risultati vuoto.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.ResultSet getAttributes(java.lang.String catalog,  
                                        java.lang.String schemaPattern,  
                                        java.lang.String typeNamePattern,  
                                        java.lang.String attributeNamePattern)  
```  
  
#### <a name="parameters"></a>Parametri  
 *catalog*  
  
 Valore **String** contenente il nome del catalogo.  
  
 *schemaPattern*  
  
 Valore **String** contenente il modello del nome dello schema.  
  
 *typeNamePattern*  
  
 Valore **String** contenente il modello del nome del tipo.  
  
 *attributePattern*  
  
 Valore **String** contenente il modello del nome dell'attributo.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getAttributes viene specificato dal metodo getAttributes nell'interfaccia java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
