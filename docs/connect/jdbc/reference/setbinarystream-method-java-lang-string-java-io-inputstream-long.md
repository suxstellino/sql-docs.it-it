---
description: Metodo setBinaryStream (java.lang.String, java.io.InputStream, long)
title: Metodo setBinaryStream per il flusso di input - long | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d59c7327-c9dc-4e4f-9dff-19e1a3c62eb2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8669f362907fcfd9c83bd53e4f0873f3fe84a690
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173864"
---
# <a name="setbinarystream-method-javalangstring-javaioinputstream-long"></a>Metodo setBinaryStream (java.lang.String, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sul flusso di input specificato, che conterrà il numero specificato di byte.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public void setBinaryStream(java.lang.String parameterName,  
                            java.io.InputStream x,  
                            long length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterName*  
  
 Una **stringa** che contiene il nome del parametro.  
  
 *x*  
  
 Oggetto InputStream.  
  
 *length*  
  
 Valore **long** che indica la lunghezza espressa in numero di byte.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setBinaryStream viene specificato dal metodo setBinaryStream nell'interfaccia java.sql.CallableStatement.  
  
 Se la lunghezza del flusso è diversa da quanto specificato nel parametro *length*, il driver JDBC genera un'eccezione al momento dell'aggiornamento o dell'inserimento della riga.  
  
 Se la lunghezza del flusso è sconosciuta, il parametro *length* può essere impostato su -1 a indicare che il driver deve accettare il flusso indipendentemente dalla lunghezza. Con sqljdbc4.jar, si consiglia di utilizzare il metodo [setBinaryStream Method (java.lang.String, java.io.InputStream)](../../../connect/jdbc/reference/setbinarystream-method-java-lang-string-java-io-inputstream.md) di JDBC 4.0 se nell'applicazione è richiesto l'aggiornamento della colonna da un flusso la cui lunghezza è sconosciuta.  
  
## <a name="see-also"></a>Vedere anche  
 [setBinaryStream &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)   
 [Membri di SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
