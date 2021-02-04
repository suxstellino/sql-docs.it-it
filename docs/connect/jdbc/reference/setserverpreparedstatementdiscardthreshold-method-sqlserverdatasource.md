---
description: Metodo setServerPreparedStatementDiscardThreshold (SQLServerDataSource)
title: Metodo setServerPreparedStatementDiscardThreshold (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 570b857b9e589fb71d791e47af90acca1927cfc5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173101"
---
# <a name="setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource"></a>Metodo setServerPreparedStatementDiscardThreshold (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Imposta il valore della proprietà di connessione serverPreparedStatementDiscardThreshold. Questa impostazione consente di controllare il numero di azioni di annullamento di istruzioni preparate (sp_unprepare) in sospeso per ogni connessione prima che venga eseguita una chiamata per pulire gli handle in sospeso nel server. Quando l'impostazione è <= 1, le azioni di annullamento della preparazione vengono eseguite immediatamente alla chiusura dell'istruzione preparata. Se il valore è impostato su > 1, queste chiamate vengono eseguite in batch per evitare il sovraccarico di chiamate troppo frequenti di sp_unprepare.
 
## <a name="syntax"></a>Sintassi  
  
```
public void setServerPreparedStatementDiscardThreshold(int enablePrepareOnFirstPreparedStatementCall);  
```  
  
#### <a name="parameters"></a>Parametri  
 *serverPreparedStatementDiscardThreshold*  
  
 Nuovo valore della proprietà di connessione **serverPreparedStatementDiscardThreshold**.  

## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Osservazioni  
 Questo metodo è disponibile dal driver JDBC versione 6.4 e successive.
 
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
