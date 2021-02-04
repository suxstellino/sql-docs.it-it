---
description: Costruttore SQLServerBlob (SQLServerConnection, byte)
title: Costruttore SQLServerBlob (SQLServerConnection, byte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection, byte[].SQLServerBlob
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fe573e3-30db-4828-abab-e9346493e931
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2ff6f2a7fadaa23df917e07b10b20f2cf3df427
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178407"
---
# <a name="sqlserverblob-constructor-sqlserverconnection-byte"></a>Costruttore SQLServerBlob (SQLServerConnection, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Inizializza una nuova istanza della classe [SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md) se sono specificati un oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) e una matrice di **byte**.  
  
> [!NOTE]  
>  Questo metodo è deprecato nella versione 2.0 del driver JDBC. Usare invece il metodo [createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md) della classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public SQLServerBlob(SQLServerConnection connection,  
                     byte[] data)  
```  
  
#### <a name="parameters"></a>Parametri  
 *connection*  
  
 Oggetto SQLServerConnection.  
  
 *data*  
  
 Matrice di **byte**.  
  
## <a name="see-also"></a>Vedere anche  
 [Costruttori di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-constructors.md)   
 [Membri di SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Classe SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
