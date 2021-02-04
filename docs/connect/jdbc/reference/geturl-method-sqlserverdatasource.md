---
description: Metodo getURL (SQLServerDataSource)
title: Metodo getURL (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: dd0d5d2c-91fe-4b0f-a162-69d898ba176e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b7a8e5a67d4c99bc525b887df550f70cbd0d8f96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177646"
---
# <a name="geturl-method-sqlserverdatasource"></a>Metodo getURL (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Restituisce l'URL utilizzato per la connessione all'origine dati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.lang.String getURL()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Valore **String** che contiene l'URL.  
  
## <a name="remarks"></a>Osservazioni  
 Per motivi di sicurezza, non includere la password nell'URL fornito al metodo [setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md). poiché nei server applicazioni Java di terze parti sarà molto spesso visualizzato il set di valori per la proprietà URL nella relativa interfaccia utente di configurazione dell'origine dati. Usare invece il metodo [setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md) per impostare il valore della password. Nei server applicazioni Java non sarà visualizzata una password che viene impostata nella relativa origine dati nell'interfaccia utente di configurazione.  
  
> [!NOTE]  
>  Se il metodo setURL non viene chiamato prima del metodo getURL, l'oggetto getURL restituisce il valore predefinito "jdbc:sqlserver://".  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
