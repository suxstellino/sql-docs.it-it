---
description: Membri di SQLServerXAResource
title: Membri di SQLServerXAResource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a069bf2c-1b70-4817-b084-a508445de799
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 81a63c16b08b7c1d418614a1e521cc8bede0b0f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181670"
---
# <a name="sqlserverxaresource-members"></a>Membri di SQLServerXAResource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[SSTRANSTIGHTLYCPLD](../../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md)|Utilizzato per consentire le transazioni XA "a regime di controllo stretto ("tightly-coupled")" che dispongono di diversi ID di transazione dei rami XA (XID), ma dello stesso ID di transazione globale (GTRID).|  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|javax.transaction.xa.XAResource|TMENDRSCAN, TMFAIL, TMJOIN, TMNOFLAGS, TMONEPHASE, TMRESUME, TMSTARTRSCAN, TMSUCCESS, TMSUSPEND, XA_OK, XA_RDONLY|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverxaresource.md)|Esegue il commit della transazione globale specificata dall'oggetto Xid fornito.|  
|[end](../../../connect/jdbc/reference/end-method-sqlserverxaresource.md)|Termina il lavoro eseguito per conto di un ramo di transazione.|  
|[forget](../../../connect/jdbc/reference/forget-method-sqlserverxaresource.md)|Indica allo strumento di gestione delle risorse di dimenticare un ramo di transazione completato euristicamente.|  
|[getTransactionTimeout](../../../connect/jdbc/reference/gettransactiontimeout-method-sqlserverxaresource.md)|Ottiene il valore del timeout della transazione corrente impostato per questo oggetto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).|  
|[isSameRM](../../../connect/jdbc/reference/issamerm-method-sqlserverxaresource.md)|Determina se l'istanza dello strumento di gestione delle risorse rappresentata dall'oggetto di destinazione è uguale a quella rappresentata dall'oggetto XAResource specificato.|  
|[prepare](../../../connect/jdbc/reference/prepare-method-sqlserverxaresource.md)|Richiede che lo strumento di gestione delle risorse esegua la preparazione per un commit della transazione specificata dall'oggetto Xid fornito.|  
|[recover](../../../connect/jdbc/reference/recover-method-sqlserverxaresource.md)|Ottiene un elenco di rami di transazioni preparati da uno strumento di gestione delle risorse.|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverxaresource.md)|Richiede che il lavoro di rollback dello strumento di gestione delle risorse venga eseguito per conto di un ramo di transazione.|  
|[setTransactionTimeout](../../../connect/jdbc/reference/settransactiontimeout-method-sqlserverxaresource.md)|Imposta il valore del timeout della transazione corrente per questo oggetto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).|  
|[start](../../../connect/jdbc/reference/start-method-sqlserverxaresource.md)|Avvia il lavoro per conto di un ramo di transazione specificato nell'oggetto Xid.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
