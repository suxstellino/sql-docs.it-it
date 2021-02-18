---
title: Supporto del driver OLE DB per SQL Server per disponibilità elevata e ripristino di emergenza
description: Informazioni sul supporto di OLE DB Driver per SQL Server di disponibilità elevata e ripristino di emergenza durante la connessione ai database con tali funzionalità configurate.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5adce5bcda14dd275791f055359654bc120e79d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347901"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>Supporto del driver OLE DB per SQL Server per disponibilità elevata e ripristino di emergenza
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questo articolo descrive il supporto di *OLE DB Driver per SQL Server* per [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Per altre informazioni su [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vedere [Listener del gruppo di disponibilità, connettività client e failover dell’applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clustering di failover e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) e [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 È possibile specificare il listener di un determinato gruppo di disponibilità nella stringa di connessione. Se un driver OLE DB per un'applicazione SQL Server è connesso a un database in un gruppo di disponibilità, la connessione originale viene interrotta e deve esserne aperta una nuova per l'applicazione affinché quest'ultima possa continuare a funzionare dopo il failover.  
  
 Se non si sta eseguendo la connessione a un listener del gruppo di disponibilità e se più indirizzi IP sono associati a un nome host, il driver OLE DB per SQL Server eseguirà l'iterazione di tutti gli indirizzi IP associati alla voce DNS in modo sequenziale. Questa operazione può richiedere tempi lunghi se il primo indirizzo IP restituito dal server DNS non è associato ad alcuna scheda di interfaccia di rete. Durante la connessione al listener di un gruppo di disponibilità, il driver OLE DB per SQL Server tenta di connettersi a tutti gli indirizzi IP in parallelo. Se un tentativo riesce, tutti gli altri tentativi di connessione in sospeso vengono ignorati.  
  
> [!NOTE]  
> L'aumento del timeout di connessione e l'implementazione della logica di riesecuzione per le connessioni aumentano le probabilità che un'applicazione si connetta a un gruppo di disponibilità. Inoltre, poiché potrebbe non essere possibile stabilire una connessione a causa del failover di un gruppo di disponibilità, è opportuno implementare la logica di riesecuzione delle connessioni, finché non si ottiene la riconnessione.  
  
## <a name="connecting-with-multisubnetfailover"></a>Connessione con MultiSubnetFailover  
 Specificare sempre **MultiSubnetFailover=Yes** in caso di connessione a un listener del gruppo di disponibilità AlwaysOn di SQL Server o a un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover** consente un failover più veloce per tutti i gruppi di disponibilità AlwaysOn, abilita le istanze del cluster di failover in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e riduce in modo significativo la durata del failover per le topologie AlwaysOn a subnet singola e a più subnet. Durante un failover su più subnet, verranno tentate connessioni in parallelo da parte del client. Durante un failover della subnet, OLE DB Driver per SQL Server tenterà di nuovo di stabilire la connessione TCP.  
  
 La proprietà di connessione **MultiSubnetFailover** indica che l'applicazione viene distribuita in un gruppo di disponibilità o nell'istanza del cluster di failover e che il driver OLE DB per SQL Server tenta di connettersi al database nell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] primaria tramite la connessione a tutti gli indirizzi IP. Quando si specifica **MultiSubnetFailover=Yes** per una connessione, i ripetuti tentativi di connessione TCP del client vengono eseguiti più rapidamente rispetto agli intervalli di ritrasmissione TCP predefiniti del sistema operativo. In tal modo si abilita in modo più veloce la riconnessione a seguito di failover di un gruppo di disponibilità AlwaysOn o di un'istanza del cluster di failover ed è applicabile a istanze del cluster di failover o a gruppi di disponibilità su una singola subnet o su più subnet.  
  
 Per informazioni sulle parole chiave della stringa di connessione, vedere [Uso delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 La specifica di **MultiSubnetFailover=Yes** durante la connessione a un oggetto diverso da un listener del gruppo di disponibilità o dall'istanza del cluster di failover non è supportata poiché potrebbe determinare un impatto negativo sulle prestazioni.  
  
 Utilizzare le linee guida seguenti per connettersi a un server in un gruppo di disponibilità o nell'istanza del cluster di failover:  
  
-   Usare la proprietà di connessione **MultiSubnetFailover** in caso di connessione a una singola subnet o a più subnet, in modo da migliorare le prestazioni.  
  
-   Per eseguire la connessione a un gruppo di disponibilità, specificare il listener del gruppo di disponibilità come server nella stringa di connessione.  
  
-   La connessione a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] configurata con più di 64 indirizzi IP determinerà un errore di connessione.  
  
-   Il comportamento di un'applicazione in cui viene usata la proprietà di connessione **MultiSubnetFailover** non è influenzato dal tipo di autenticazione, cioè dall'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], dall'autenticazione Kerberos, o dall’autenticazione di Windows.  
  
-   È possibile aumentare il valore di **loginTimeout** per adattarlo alla durata del failover e ridurre il numero di nuovi tentativi di connessione dell'applicazione.  
  
-   Le transazioni distribuite non sono supportate.  
  
Se il routing di sola lettura non è attivo, non è possibile stabilire una connessione a un percorso di replica secondaria in un gruppo di disponibilità nelle situazioni seguenti:  
  
1.  Se il percorso di replica secondaria non è configurato per accettare le connessioni.  
  
2.  Se un'applicazione usa **ApplicationIntent=ReadWrite** (vedere di seguito) e il percorso di replica secondaria è configurato per l'accesso in sola lettura.  
  
Una connessione non riesce se una replica primaria è configurata per rifiutare i carichi di lavoro in sola lettura e la stringa di connessione contiene **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Aggiornamento per l'utilizzo di cluster su più subnet dal mirroring del database  
Si verificherà un errore di connessione se nella stringa di connessione sono presenti le parole chiave di connessione **MultiSubnetFailover** e **Failover_Partner**. Si verificherà un errore anche se viene usato **MultiSubnetFailover** e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] restituisce una risposta del partner di failover in cui viene indicato che fa parte di una coppia del mirroring del database.  
  
Se si aggiorna un driver OLE DB per un'applicazione SQL Server che usa il mirroring del database in uno scenario su più subnet, è necessario rimuovere la proprietà di connessione **Failover_Partner** e sostituirla con **MultiSubnetFailover** impostata su **Yes**, nonché sostituire il nome del server nella stringa di connessione con un listener del gruppo di disponibilità. Se in una stringa di connessione vengono usati **Failover_Partner** e **MultiSubnetFailover=Yes**, il driver genererà un errore. Se tuttavia in una stringa di connessione vengono usati **Failover_Partner** e **MultiSubnetFailover=No** (o **ApplicationIntent=ReadWrite**), l'applicazione userà il mirroring del database.  
  
Il driver restituirà un errore se il mirroring del database viene usato nel database primario nel gruppo di disponibilità e se **MultiSubnetFailover=Yes** veine usato nella stringa di connessione a un database primario anziché a un listener del gruppo di disponibilità.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
OLE DB Driver per SQL Server supporta le parole chiave **ApplicationIntent** e **MultiSubnetFailover**.   
  
Sono state aggiunte le due parole chiave della stringa di connessione OLE DB per supportare [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in OLE DB Driver per SQL Server:  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 Per informazioni sulle parole chiave della stringa di connessione nel driver OLE DB per SQL Server, vedere [Uso delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

### <a name="application-intent"></a>Finalità dell'applicazione 

Le proprietà di connessione equivalenti sono:  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
Un'applicazione OLE DB Driver per SQL Server può usare uno dei metodi seguenti per specificare la finalità dell'applicazione:  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** prevede l'uso del set di proprietà precedentemente configurato per inizializzare l'origine dati e creare l'oggetto origine dati. La finalità dell'applicazione viene specificata come proprietà del provider o come parte della stringa di proprietà estesa.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDatasource** accetta una stringa di connessione di input che può contenere la parola chiave **Application Intent**.  
  
 -   **IDBProperties::SetProperties**  
 Per impostare il valore della proprietà **ApplicationIntent**, chiamare **IDBProperties::SetProperties** passando la proprietà **SSPROP_INIT_APPLICATIONINTENT** con un valore "**ReadWrite**" o "**ReadOnly**" o la proprietà **DBPROP_INIT_PROVIDERSTRING** con un valore contenente "**ApplicationIntent=ReadOnly**" o "**ApplicationIntent=ReadWrite**".  
  
È possibile specificare la finalità dell'applicazione nel campo delle proprietà della finalità dell’applicazione della scheda Tutte nella finestra di dialogo **Proprietà di Data Link**.  
  
Quando vengono stabilite connessioni implicite, per la connessione viene utilizzata l'impostazione relativa alla finalità dell'applicazione definita per la connessione padre. Analogamente, più sessioni create dalla stessa origine dati ereditano l'impostazione relativa alla finalità dell'applicazione definita per l'origine dati.  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

Le proprietà di connessione equivalenti sono:  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

Un'applicazione OLE DB Driver per SQL Server può usare uno dei metodi seguenti per impostare l'opzione MultiSubnetFailover:  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** prevede l'uso del set di proprietà precedentemente configurato per inizializzare l'origine dati e creare l'oggetto origine dati. La finalità dell'applicazione viene specificata come proprietà del provider o come parte della stringa di proprietà estesa.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDatasource** accetta una stringa di connessione di input che può contenere la parola chiave **MultiSubnetFailover**.  

-   **IDBProperties::SetProperties**  
Per impostare il valore della proprietà **MultiSubnetFailover**, chiamare **IDBProperties::SetProperties** passando la proprietà **SSPROP_INIT_MULTISUBNETFAILOVER** con valore **VARIANT_TRUE** o **VARIANT_FALSE** o la proprietà **DBPROP_INIT_PROVIDERSTRING** con valore contenente "**MultiSubnetFailover=Yes**" o "**MultiSubnetFailover=No**".

#### <a name="example"></a>Esempio

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Utilizzo delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  
