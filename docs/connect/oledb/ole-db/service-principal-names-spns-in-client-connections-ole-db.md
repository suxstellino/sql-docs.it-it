---
title: Nomi dell'entità servizio (SPN) nelle connessioni client (OLE DB) | Microsoft Docs
description: Informazioni sulle proprietà e le funzioni membro di OLE DB Driver per SQL Server che supportano i nomi delle entità servizio nelle applicazioni client.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 259ecea7a9890ae44a9a32477f4712137175f04d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754171"
---
# <a name="service-principal-names-spns-in-client-connections-ole-db-in-sql-server-native-client"></a>Nomi dell'entità servizio (SPN) nelle connessioni client (OLE DB) in SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]


  In questo argomento vengono descritte le funzioni membro e le proprietà OLE DB che supportano i nomi SPN (Service Principal Name) nelle applicazioni client. Per altre informazioni sui nomi SPN nelle applicazioni client, vedere [Supporto per nomi SPN nelle connessioni client](../../oledb/features/service-principal-name-spn-support-in-client-connections.md). Per un esempio, vedere [Autenticazione Kerberos integrata &#40;OLE DB&#41;](../../oledb/ole-db-how-to/integrated-kerberos-authentication-ole-db.md).  
  
## <a name="provider-initialization-string-keywords"></a>Parole chiave della stringa di inizializzazione del provider  
 Le seguenti parole chiave della stringa di inizializzazione del provider supportano i nomi SPN nelle applicazioni OLE DB. Nella tabella seguente i valori nella colonna relativa alla parola chiave vengono usati per la stringa del provider di IDBInitialize::Initialize. I valori nella colonna relativa alla descrizione vengono usati nelle stringhe di inizializzazione quando la connessione viene eseguita con ADO oppure IDataInitialize::GetDataSource.  
  
|Parola chiave|Descrizione|valore|  
|-------------|-----------------|-----------|  
|ServerSPN|SPN server|Nome SPN del server. Il valore predefinito è una stringa vuota, che in OLE DB Driver per SQL Server determina l'uso del nome SPN predefinito generato dal provider.|  
|FailoverPartnerSPN|Nome SPN del partner di failover|Nome SPN del partner di failover. Il valore predefinito è una stringa vuota, che in OLE DB Driver per SQL Server determina l'uso del nome SPN predefinito generato dal provider.|  
  
## <a name="data-source-initialization-properties"></a>Proprietà di inizializzazione dell'origine dati  
 Le seguenti proprietà del set di proprietà **DBPROPSET_SQLSERVERDBINIT** consentono alle applicazioni di specificare nomi SPN.  
  
|Nome|Type|Uso|  
|----------|----------|-----------|  
|SSPROP_INIT_SERVERSPN|VT_BSTR, lettura/scrittura|Specifica il nome SPN per il server. Il valore predefinito è una stringa vuota, che in OLE DB Driver per SQL Server determina l'uso del nome SPN predefinito generato dal provider.|  
|SSPROP_INIT_FAILOVERPARTNERSPN|VT_BSTR, lettura/scrittura|Specifica il nome SPN per il partner di failover. Il valore predefinito è una stringa vuota, che in OLE DB Driver per SQL Server determina l'uso del nome SPN predefinito generato dal provider.|  
  
## <a name="data-source-properties"></a>Proprietà dell'origine dati  
 Le seguenti proprietà del set di proprietà **DBPROPSET_SQLSERVERDATASOURCEINFO** consentono alle applicazioni di trovare il metodo di autenticazione.  
  
|Nome|Type|Uso|  
|----------|----------|-----------|  
|SSPROP_INTEGRATEDAUTHENTICATIONMETHOD|VT_BSTR, sola lettura|Restituisce il metodo di autenticazione utilizzato per la connessione. Il valore restituito all'applicazione è il valore che Windows restituisce a OLE DB Driver per SQL Server. Di seguito sono indicati i valori possibili: <br />"NTLM", restituito quando una connessione viene aperta utilizzando l'autenticazione NTLM.<br />"Kerberos", restituito quando una connessione viene aperta utilizzando l'autenticazione Kerberos.<br /><br /> Se è stata aperta una connessione ed è impossibile determinare il metodo di autenticazione, viene restituito VT_EMPTY.<br /><br /> Questa proprietà può essere letta solo quando è stata inizializzata un'origine dati. Se si tenta di leggere la proprietà prima che sia stata inizializzata un'origine dati, IDBProperties::GetProperies restituirà DB_S_ERRORSOCCURRED o DB_E_ERRORSOCCURRED, a seconda dei casi e verrà impostato DBPROPSTATUS_NOTSUPPORTED in DBPROPSET_PROPERTIESINERROR per questa proprietà. Questo comportamento è conforme alla specifica OLE DB principale.|  
|SSPROP_MUTUALLYAUTHENTICATED|VT_BOOL, sola lettura|Restituisce VARIANT_TRUE se nella connessione è stata eseguita un'autenticazione reciproca dei server. In caso contrario, restituisce VARIANT_FALSE.<br /><br /> Questa proprietà può essere letta solo quando è stata inizializzata un'origine dati. Se si tenta di leggere la proprietà prima che sia stata inizializzata un'origine dati, IDBProperties::GetProperies restituirà DB_S_ERRORSOCCURRED o DB_E_ERRORSOCCURRED, a seconda dei casi e verrà impostato DBPROPSTATUS_NOTSUPPORTED in DBPROPSET_PROPERTIESINERROR per questa proprietà. Questo comportamento è conforme alla specifica OLE DB principale<br /><br /> Se viene eseguita una query su questo attributo per una connessione in cui non è stata utilizzata l'autenticazione di Windows, viene restituito VARIANT_FALSE.|  
  
## <a name="ole-db-api-support-for-spns"></a>Supporto dell'API OLE DB per i nomi SPN  
 Nella tabella seguente vengono descritte le funzioni membro OLE DB che supportano i nomi SPN nelle connessioni client:  
  
|Funzione membro|Descrizione|  
|---------------------|-----------------|  
|IDataInitialize::GetDataSource|*pwszInitializationString* può contenere le nuove parole chiave **ServerSPN** e **FailoverPartnerSPN**.|  
|IDataInitialize::GetInitializationString|Se SSPROP_INIT_SERVERSPN e SSPROP_INIT_FAILOVERPARTNERSPN contengono valori non predefiniti, saranno inclusi nella stringa di inizializzazione attraverso *ppwszInitString* come valori di parola chiave per **ServerSPN** e **FailoverPartnerSPN**. In caso contrario, queste parole chiave non saranno incluse nella stringa di inizializzazione.|  
|IDBInitialize::Initialize|Se la richiesta viene abilitata impostando DBPROP_INIT_PROMPT nelle proprietà di inizializzazione dell'origine dati, verrà visualizzata la finestra di dialogo di accesso OLE DB. Ciò consente di immettere i nomi SPN per il server principale e per il relativo partner di failover.<br /><br /> La stringa del provider in DPPROP_INIT_PROVIDERSTRING, se impostata, riconoscerà le nuove parole chiave **ServerSPN** e **FailoverPartnerSPN** e userà i relativi valori, se presenti, per inizializzare SSPROP_INIT_SERVER_SPN e SSPROP_INIT_FAILOVER_PARTNER_SPN.<br /><br /> È possibile chiamare IDBProperties::SetProperties per impostare le proprietà SSPROP_INIT_SERVER_SPN e SSPROP_INIT_FAILOVER_PARTNER_SPN prima che venga chiamato IDBInitialize::Initialize. Si tratta di un'alternativa all'utilizzo di una stringa del provider.<br /><br /> Se una proprietà viene impostata in più posizioni, un valore impostato a livello di programmazione ha la precedenza su un valore impostato nella stringa del provider. Un valore impostato in una stringa di inizializzazione ha la precedenza su un valore impostato in una finestra di dialogo.<br /><br /> Se la stessa parola chiave viene visualizzata più volte nella stringa del provider, il valore della prima occorrenza avrà la precedenza.|  
|IDBProperties::GetProperties|È possibile chiamare IDBProperties::GetProperties per ottenere i valori delle nuove proprietà di inizializzazione dell'origine dati SSPROP_INIT_SERVERSPN e SSPROP_INIT_FAILOVERPARTNERSPN e delle nuove proprietà dell'origine dati SPROP_AUTHENTICATIONMETHOD e SSPROP_MUTUALLYAUTHENTICATED.|  
|IDBProperties::GetPropertyInfo|IdbProperties::GetPropertyInfo include le nuove proprietà di inizializzazione dell'origine dati SSPROP_INIT_SERVERSPN e SSPROP_INIT_FAILOVERPARTNERSPN oppure le nuove proprietà dell'origine dati SPROP_AUTHENTICATIONMETHOD e SSPROP_MUTUALLYAUTHENTICATED.|  
|IDBProperties::SetProperties|È possibile chiamare IDBProperties::SetProperties per impostare i valori delle nuove proprietà di inizializzazione dell'origine dati SSPROP_INITSERVERSPN e SSPROP_INIT_FAILOVERPARTNERSPN.<br /><br /> È possibile impostare queste proprietà in qualsiasi momento; tuttavia, se l'origine dati è già aperta, verrà restituito il seguente errore: DB_E_ERRORSOCCURRED, "Si sono verificati errori in un'operazione OLE DB composta da più passaggi. Controllare i singoli valori di stato OLE DB, se disponibili. Nessuna operazione eseguita".|  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
