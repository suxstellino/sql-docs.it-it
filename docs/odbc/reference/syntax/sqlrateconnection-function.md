---
description: Funzione SQLRateConnection
title: Funzione SQLRateConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLRateConnection function [ODBC]
ms.assetid: e8da2ffb-d6ef-4ca7-824f-57afd29585d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40aec3459e43d2684bd80f0ced55171314551d92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192544"
---
# <a name="sqlrateconnection-function"></a>Funzione SQLRateConnection
**Conformità**  
 Versione introdotta: ODBC 3,81 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLRateConnection** determina se un driver può riutilizzare una connessione esistente nel pool di connessioni.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp
  
SQLRETURN  SQLRateConnection(  
                SQLHDBC_INFO_TOKEN   hRequest,  
                SQLHDBC              hCandidateConnection,  
                BOOL                 fRequiredTransactionEnlistment,  
                TRANSID              transId,  
                DWORD *              pRating );  
```  
  
## <a name="arguments"></a>Argomenti  
 *hRequest*  
 Input Handle di token che rappresenta la nuova richiesta di connessione dell'applicazione.  
  
 *hCandidateConnection*  
 Input Connessione esistente nel pool di connessioni. Lo stato della connessione deve essere opened.  
  
 *fRequiredTransactionEnlistment*  
 Input Se TRUE, il riutilizzo del *hCandidateConnection* della connessione esistente per la nuova richiesta di connessione (*hRequest*) richiede un'integrazione aggiuntiva.  
  
 *transId*  
 Input Se *fRequiredTransactionEnlistment* è true, *transId* rappresenta la transazione DTC che verrà integrata dalla richiesta. Se *fRequiredTransactionEnlistment* è false, *transId* verrà ignorato.  
  
 *Spettegolare*  
 Output classificazione di riutilizzo di *hCandidateConnection* per *hRequest*. Questa valutazione sarà compresa tra 0 e 100 (inclusi).  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Gestione driver non elaborerà le informazioni di diagnostica restituite da questa funzione.  
  
## <a name="remarks"></a>Commenti  
 **SQLRateConnection** produce un punteggio compreso tra 0 e 100 (inclusi) che indica il modo in cui una connessione esistente corrisponde alla richiesta.  
  
|Punteggio|Significato (quando viene restituito SQL_SUCCESS)|  
|-----------|-----------------------------------------------|  
|0|*hCandidateConnection* non deve essere riutilizzato per il *hRequest*.|  
|Qualsiasi valore compreso tra 1 e 98 (inclusi)|Maggiore è il punteggio, più vicino a *hCandidateConnection* corrisponde a *hRequest*.|  
|99|Negli attributi non significativi sono presenti solo mancate corrispondenze.  Gestione driver deve arrestare il ciclo di valutazione.|  
|100|Corrispondenza perfetta.  Gestione driver deve arrestare il ciclo di valutazione.|  
|Qualsiasi altro valore maggiore di 100|*hCandidateConnection* è contrassegnato come Dead e non verrà riutilizzato anche in una richiesta di connessione futura.|  
  
 Gestione driver contrassegna una connessione come inattiva se il codice restituito è diverso da SQL_SUCCESS (incluso SQL_SUCCESS_WITH_INFO) o la classificazione è maggiore di 100. Tale connessione non verrà riutilizzata (anche nelle future richieste di connessione) e si verificherà un timeout dopo il passaggio di CPTimeout. Gestione driver continuerà a trovare un'altra connessione dal pool al tasso.  
  
 Se Gestione driver ha riutilizzato una connessione il cui punteggio è rigorosamente inferiore a 100 (incluso 99), gestione driver chiamerà SQLSetConnectAttr (SQL_ATTR_DBC_INFO_TOKEN) per reimpostare la connessione nello stato richiesto dall'applicazione. Il driver non deve reimpostare la connessione in questa chiamata di funzione.  
  
 Se *fRequiredTransactionEnlistment* è true, per riutilizzare *hCandidateConnection* è necessario un elenco aggiuntivo (*transId* ! = null) o unintegration (*transId* = = null). Ciò indica il costo di riutilizzo di una connessione e il fatto che il driver debba integrare/rimuovere la connessione in caso di riutilizzo della connessione. Se *fRequireTransactionEnlistment* è false, il driver deve ignorare il valore di *transId*.  
  
 Gestione driver garantisce che l'handle HENV padre di *hRequest* e *hCandidateConnection* siano uguali. Gestione driver garantisce che l'ID del pool associato a *hRequest* e *hCandidateConnection* sia lo stesso.  
  
 Le applicazioni non devono chiamare direttamente questa funzione. Un driver ODBC che supporta il pool di connessioni compatibile con il driver deve implementare questa funzione.  
  
 Includere sqlspi. h per lo sviluppo di driver ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di un driver ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pool di connessioni compatibile con il driver](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Sviluppo del rilevamento di pool di connessioni in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
