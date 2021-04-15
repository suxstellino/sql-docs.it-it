---
title: Driver ODBC in Linux e macOS - Disponibilità elevata e ripristino di emergenza
description: Informazioni sul supporto di Gruppi di disponibilità AlwaysOn con il driver ODBC Microsoft per Linux e macOS.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: fa656c5b-a935-40bf-bc20-e517ca5cd0ba
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8183d7d99bd3ec9ffa344f3b813d17eca0dac85b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "107490069"
---
# <a name="odbc-driver-on-linux-and-macos-support-for-high-availability-and-disaster-recovery"></a>Supporto del driver ODBC in Linux e macOS per disponibilità elevata e ripristino di emergenza
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

I driver ODBC per Linux e macOS supportano [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]. Per altre informazioni su [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)], vedere:  
  
-   [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
-   [Creazione e configurazione di gruppi di disponibilità (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Clustering di failover e gruppi di disponibilità AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Repliche secondarie attive: Repliche secondarie leggibili (Gruppi di disponibilità AlwaysOn)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
È possibile specificare il listener di un determinato gruppo di disponibilità nella stringa di connessione. Se un'applicazione ODBC in Linux o macOS è connessa a un database in un gruppo di disponibilità, la connessione originale viene interrotta e deve esserne aperta una nuova per l'applicazione affinché quest'ultima possa continuare a funzionare dopo il failover.

I driver ODBC in Linux e macOS eseguono l'iterazione in modo sequenziale di tutti gli indirizzi IP associati a un nome host DNS se non si sta eseguendo la connessione a un listener del gruppo di disponibilità e se più indirizzi IP sono associati a un nome host,.

Queste iterazioni possono richiedere tempi lunghi se il primo indirizzo IP restituito dal server DNS non è collegabile. Quando ci si connette al listener di un gruppo di disponibilità, il driver tenta di stabilire connessioni a tutti gli indirizzi IP in parallelo. Se un tentativo di connessione ha esito positivo, il driver ignora tutti i tentativi di connessione in sospeso.

> [!NOTE]  
> Poiché il failover di un gruppo di disponibilità potrebbe causare l'esito negativo di una connessione, è anche opportuno implementare una logica di ripetizione dei tentativi per le connessioni finché non si ottiene la riconnessione. L'aumento del timeout di connessione e l'implementazione di una logica di ripetizione dei tentativi per le connessioni aumentano le probabilità di connessione a un gruppo di disponibilità.

## <a name="connecting-with-multisubnetfailover"></a>Connessione con MultiSubnetFailover

Specificare sempre **MultiSubnetFailover=Yes** in caso di connessione al listener di un gruppo di disponibilità di [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] o a un'istanza del cluster di failover di [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **MultiSubnetFailover** garantisce un failover più veloce per tutti i gruppi di disponibilità e per tutte le istanze del cluster di failover in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **MultiSubnetFailover** riduce anche notevolmente il tempo di failover per le topologie AlwaysOn a subnet singola e a più subnet. Durante un failover su più subnet, vengono tentate connessioni in parallelo da parte del client. Durante un failover su una subnet, il driver ritenta in modo insistente la connessione TCP.

La proprietà di connessione **MultiSubnetFailover** indica che l'applicazione è in corso di distribuzione in un gruppo di disponibilità o in un'istanza del cluster di failover. Il driver tenta di connettersi al database nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] primaria tentando di connettersi a tutti gli indirizzi IP. Se si connette con **MultiSubnetFailover=Yes**, il client riesegue i tentativi di connessione TCP più velocemente rispetto agli intervalli di ritrasmissione TCP predefinita del sistema operativo. Dopo il failover,**MultiSubnetFailover = Yes** consente una riconnessione più veloce di un gruppo di disponibilità AlwaysOn o di un'istanza del cluster di failover AlwaysOn. **MultiSubnetFailover = Yes** si applica a gruppi di disponibilità e a cluster di failover sia a subnet singola che a più subnet.  

Usare **MultiSubnetFailover=Yes** in caso di connessione al listener di un gruppo di disponibilità o a un'istanza del cluster di failover. In caso contrario, le prestazioni dell'applicazione potrebbero essere influenzate negativamente.

Si notino le linee guida seguenti per connettersi a un server in un gruppo di disponibilità o nell'istanza del cluster di failover:
  
-   Specificare **multiSubnetFailover=Yes** per migliorare le prestazioni in caso di connessione a singola subnet o a un gruppo di disponibilità su più subnet.

-   Specificare il listener del gruppo di disponibilità come server nella stringa di connessione.
  
-   Non è possibile connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurata con più di 64 indirizzi IP.

-   È possibile usare sia l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sia l'autenticazione Kerberos con **MultiSubnetFailover=Yes** senza influenzare il comportamento dell'applicazione.

-   È possibile aumentare il valore di **loginTimeout** per adattarlo alla durata del failover e ridurre il numero di nuovi tentativi di connessione dell'applicazione.

-   Le transazioni distribuite non sono supportate.  
  
Se il routing di sola lettura non è attivo, non è possibile stabilire una connessione a un percorso di replica secondaria in un gruppo di disponibilità nelle situazioni seguenti:  
  
1.  Se il percorso di replica secondaria non è configurato per accettare le connessioni.  
  
2.  Se un'applicazione usa **ApplicationIntent=ReadWrite** e il percorso di replica secondaria è configurato per l'accesso in sola lettura.  
  
Una connessione non riesce se una replica primaria è configurata per rifiutare i carichi di lavoro di sola lettura e la stringa di connessione contiene **ApplicationIntent=ReadOnly**.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="odbc-syntax"></a>Sintassi ODBC

Due parole chiave di stringa di connessione ODBC supportano [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]:  
  
-   **ApplicationIntent**  
  
-   **MultiSubnetFailover**  
  
Per altre informazioni sulle parole chiave di stringa di connessione ODBC, vedere [Uso delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
Gli attributi delle proprietà di connessione equivalenti sono:
  
-   **SQL_COPT_SS_APPLICATION_INTENT**  
  
-   **SQL_COPT_SS_MULTISUBNET_FAILOVER**  
  
Per altre informazioni sugli attributi di connessione ODBC, vedere [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
Un'applicazione ODBC che usa [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)] può eseguire la connessione tramite una delle due funzioni seguenti:  
  
|Funzione|Descrizione|  
|------------|---------------|  
|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|**SQLConnect** supporta sia **ApplicationIntent** che **MultiSubnetFailover** tramite un nome origine dati (DSN, Data Source Name) o un attributo di connessione.|  
|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|**SQLDriverConnect** supporta **ApplicationIntent** e **MultiSubnetFailover** tramite DSN, parola chiave di stringa di connessione o attributo di connessione.|
  
## <a name="see-also"></a>Vedere anche  

[Parole chiave delle stringhe di connessione e nomi delle origini dati (DSN)](../../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)

[Linee guida per la programmazione](../../../connect/odbc/linux-mac/programming-guidelines.md)

[Note sulla versione](../../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)  
