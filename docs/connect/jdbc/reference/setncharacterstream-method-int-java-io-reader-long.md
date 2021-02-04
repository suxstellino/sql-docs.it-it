---
description: Metodo setNCharacterStream per l'oggetto java.io.Reader - long
title: Metodo setNCharacterStream per l'oggetto java.io.Reader - long | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 36396dc9-f109-4da0-bd64-726704046bbf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce7445a945b8ee655961f458ba39849bc4fe7ce1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178870"
---
# <a name="setncharacterstream-method-int-javaioreader-long"></a>Metodo setNCharacterStream (int, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il parametro designato sull'oggetto Reader specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public final void setNCharacterStream(int parameterIndex,  
                                                  java.io.Reader value,  
                                                                long length)  
```  
  
#### <a name="parameters"></a>Parametri  
 *parameterIndex*  
  
 Valore **int** che specifica l'indice del parametro.  
  
 *value*  
  
 Oggetto Reader contenente il valore del parametro.  
  
 *length*  
  
 Valore **long** che indica il numero di caratteri nel valore del parametro.  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo setNCharacterStream viene specificato dal metodo setNCharacterStream nell'interfaccia java.sql.PreparedStatement.  
  
 Questo metodo deve essere usato per i tipi di dati **NCHAR**, **NVARCHAR**, **NTEXT** e **XML**.  
  
 Se la lunghezza del flusso è diversa da quanto specificato nel parametro *length*, il driver JDBC genera un'eccezione al momento dell'aggiornamento o dell'inserimento della riga.  
  
 Se la lunghezza del flusso è sconosciuta, il parametro *length* può essere impostato su -1 a indicare che il driver deve accettare il flusso indipendentemente dalla lunghezza. Con sqljdbc4.jar, è consigliabile usare il [metodo setNCharacterStream &#40;int, java.io.Reader&#41;](../../../connect/jdbc/reference/setncharacterstream-method-int-java-io-reader.md) di JDBC 40 se nell'applicazione è richiesto l'aggiornamento della colonna da un flusso la cui lunghezza è sconosciuta.  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo setNCharacterStream &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)   
 [Membri di SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
