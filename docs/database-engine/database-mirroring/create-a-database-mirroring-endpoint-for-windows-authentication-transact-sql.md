---
title: Creare un endpoint del mirroring del database (Transact-SQL)
description: Usare Transact-SQL per creare un endpoint di mirroring del database usando l'autenticazione di Windows in SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: baf1a4b1-6790-4275-b261-490bca33bdb9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 87df52f30863d21f51a73a0dfe7b6cc1f4481bfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353209"
---
# <a name="create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql"></a>Creare un endpoint del mirroring del database per l'autenticazione Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si illustra come creare un endpoint del mirroring del database in cui si utilizza l'autenticazione di Windows in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per supportare il mirroring del database o [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , per ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è necessario un endpoint del mirroring del database. In un'istanza del server può essere incluso uno solo di questo tipo di endpoint, che a sua volta dispone di una sola porta. Un endpoint del mirroring del database può utilizzare qualsiasi porta disponibile nel sistema locale al momento della creazione dell'endpoint. Tutte le sessioni di mirroring del database in un'istanza del server sono in attesa su quella porta, che viene utilizzata anche per tutte le connessioni in ingresso per il mirroring del database.  
  
> [!IMPORTANT]  
>  Se un endpoint del mirroring del database è presente e già in uso, è consigliabile utilizzare quello. L'eliminazione di un endpoint in uso determina la chiusura delle sessioni esistenti.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  [Sicurezza](#Security)  
  
-   **Per creare un endpoint del mirroring del database usando:**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 I metodi di autenticazione e crittografia dell'istanza del server sono stabiliti dall'amministratore di sistema.  
  
> [!WARNING]  
>  L'algoritmo RC4 è deprecato. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] È consigliabile utilizzare AES.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione `CREATE ENDPOINT` o l'appartenenza al ruolo predefinito del server `sysadmin`. Per altre informazioni, vedere [GRANT - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-database-mirroring-endpoint-that-uses-windows-authentication"></a>Per creare un endpoint del mirroring del database in cui viene utilizzata l'autenticazione di Windows  
  
1.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui si desidera creare un endpoint del mirroring del database.  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Per determinare se esiste già un endpoint del mirroring del database, utilizzare l'istruzione seguente:  
  
    ```sql  
    SELECT name, role_desc, state_desc FROM sys.database_mirroring_endpoints;   
    ```  
  
    > [!IMPORTANT]  
    >  Se per l'istanza del server esiste già un endpoint del mirroring, utilizzarlo per tutte le altre sessioni stabilite nell'istanza del server.  
  
4.  Per usare Transact-SQL per creare un endpoint utilizzabile con l'autenticazione di Windows, usare un'istruzione `CREATE ENDPOINT`. Il formato generale dell'istruzione è il seguente:  
  
     ```syntaxsql
     CREATE ENDPOINT *\<endpointName>*  
  
     STATE=STARTED  
  
     AS TCP ( LISTENER_PORT = *\<listenerPortList>* )  
  
     FOR DATABASE_MIRRORING  
  
     (  
  
     [ AUTHENTICATION = **WINDOWS** [ *\<authorizationMethod>* ]  
  
     ]  
  
     [ [**,**] ENCRYPTION = **REQUIRED**  
  
     [ ALGORITHM { *\<algorithm>* } ]  
  
     ]  
  
     [**,**] ROLE = *\<role>*  
  
     )  
     ```
     
     Dove:  
  
    -   *\<endpointName>* è un nome univoco per l'endpoint del mirroring del database dell'istanza del server.  
  
    -   STARTED indica che l'endpoint deve essere avviato e deve rimanere in attesa delle connessioni. Lo stato di un endpoint del mirroring del database creato è in genere STARTED. In alternativa, è possibile avviare una sessione nello stato STOPPED (impostazione predefinita) o DISABLED.  
  
    -   *\<listenerPortList>* è un singolo numero di porta (*nnn*) su cui si vuole che il server riceva i messaggi di mirroring del database. È consentito solo il protocollo TCP. Tutti gli altri protocolli restituiranno un errore.  
  
         È possibile utilizzare un numero di porta una sola volta per ciascun computer. Un endpoint del mirroring del database può utilizzare qualsiasi porta disponibile nel sistema locale al momento della creazione dell'endpoint. Per identificare le porte attualmente utilizzate dagli endpoint TCP nel sistema, utilizzare l'istruzione Transact-SQL seguente:  
  
        ```sql  
        SELECT name, port FROM sys.tcp_endpoints;  
        ```  
  
        > [!IMPORTANT]  
        >  In ogni istanza del server è necessaria un'unica porta di attesa univoca.  
  
    -   Per l'autenticazione di Windows, l'opzione AUTHENTICATION è facoltativa, a meno che non si desideri che l'endpoint utilizzi solo NTLM o Kerberos per autenticare connessioni. *\<authorizationMethod>* specifica uno dei metodi seguenti usato per autenticare le connessioni: NTLM, KERBEROS o NEGOTIATE. Con il metodo predefinito NEGOTIATE l'endpoint utilizzerà il protocollo di negoziazione di Windows per scegliere tra NTLM e Kerberos. La negoziazione consente di utilizzare le connessioni con o senza autenticazione, a seconda del livello di autenticazione dell'endpoint opposto.  
  
    -   Per impostazione predefinita, ENCRYPTION è impostato su REQUIRED, pertanto verrà utilizzata la crittografia per tutte le connessioni a questo endpoint. È tuttavia possibile disabilitare la crittografia o renderla facoltativa in un endpoint. Sono disponibili le alternative seguenti:  
  
        |valore|Definizione|  
        |-----------|----------------|  
        |DISABLED|Specifica che i dati inviati tramite una connessione non vengano crittografati.|  
        |SUPPORTED|Specifica che i dati vengano crittografati solo se per l'endpoint opposto è stato specificato SUPPORTED o REQUIRED.|  
        |REQUIRED|Specifica che i dati inviati tramite una connessione devono essere crittografati.|  
  
         Se è necessaria la crittografia per un endpoint, ENCRYPTION deve essere impostato su SUPPORTED o REQUIRED nell'altro endpoint.  
  
    -   *\<algorithm>* consente di specificare gli standard di crittografia per l'endpoint. Il valore di *\<algorithm>* può essere uno degli algoritmi o delle combinazioni di algoritmi seguenti: RC4, AES, AES RC4 o RC4 AES.  
  
         AES RC4 indica che questo endpoint negozierà l'algoritmo di crittografia, dando la preferenza a quello AES. RC4 AES indica che questo endpoint negozierà l'algoritmo di crittografia, dando la preferenza all'algoritmo RC4. Se i due endpoint specificano entrambi gli algoritmi, ma con un ordine diverso, l'algoritmo verrà definito dall'endpoint che accetta la connessione. Fornire lo stesso algoritmo in modo esplicito per evitare errori di connessione tra server diversi.
  
        > [!WARNING]  
        >  L'algoritmo RC4 è deprecato. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] È consigliabile utilizzare AES.  
  
    -   *\<role>* definisce i ruoli eseguibili tramite il server. Il valore di ROLE deve essere specificato. Tuttavia, il ruolo dell'endpoint è rilevante solo per il mirroring del database. Per [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], viene ignorato il ruolo dell'endpoint.  
  
         Per consentire a un'istanza del server di utilizzare un ruolo per una sessione di mirroring del database e un altro ruolo per un'altra sessione, specificare ROLE=ALL. Per limitare un'istanza del server in modo che funga da partner o da server di controllo del mirroring, specificare rispettivamente `ROLE = PARTNER` o `ROLE = WITNESS`.  
  
        > [!NOTE]  
        >  Per altre informazioni sulle opzioni del mirroring del database per le diverse edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Per una descrizione completa della sintassi di CREATE ENDPOINT, vedere [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
    > [!NOTE]  
    >  Per modificare un endpoint esistente, usare [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
###  <a name="example-creating-endpoints-to-support-for-database-mirroring-transact-sql"></a><a name="TsqlExample"></a> Esempio: Creazione di endpoint per supportare il mirroring del database (Transact-SQL)  
 Nell'esempio seguente si creano endpoint del mirroring del database per le istanze del server predefinite in tre sistemi di computer distinti:  
  
|Ruolo dell'istanza del server|Nome del computer host|  
|-----------------------------|---------------------------|  
|Partner (inizialmente nel ruolo principale)|`SQLHOST01\.`|  
|Partner (inizialmente nel ruolo mirror)|`SQLHOST02\.`|  
|Controllo|`SQLHOST03\.`|  
  
 Nell'esempio tutti e tre gli endpoint utilizzano la porta numero 7022, anche se qualunque numero di porta disponibile sarebbe utilizzabile. L'opzione AUTHENTICATION non è necessaria, in quanto gli endpoint utilizzano il tipo predefinito, autenticazione di Windows. L'opzione ENCRYPTION è anch'essa superflua, in quanto gli endpoint sono tutti progettati per negoziare il metodo di autenticazione per una connessione, che rappresenta il comportamento predefinito per autenticazione di Windows. Inoltre, tutti gli endpoint richiedono la crittografia, che rappresenta il comportamento predefinito.  
  
 Ogni istanza del server è limitata a fungere da partner o da server di controllo e l'endpoint di ogni server specifica espressamente il ruolo (ROLE=PARTNER o ROLE=WITNESS).  
  
> [!IMPORTANT]  
>  Ogni istanza del server può includere un solo endpoint. Pertanto, se si desidera che un'istanza del server sia partner in alcune sessioni e server di controllo in altre, specificare ROLE=ALL.  
  
```sql  
--Endpoint for initial principal server instance, which  
--is the only server instance running on SQLHOST01.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for initial mirror server instance, which  
--is the only server instance running on SQLHOST02.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for witness server instance, which  
--is the only server instance running on SQLHOST03.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=WITNESS);  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per configurare un endpoint del mirroring del database**  
  
-   [Creare un endpoint del mirroring del database per i gruppi di disponibilità AlwaysOn &#40;SQL Server PowerShell&#41;](../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in uscita &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Impostazione dell'endpoint del mirroring del database per l'utilizzo di certificati per le connessioni in ingresso &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Specifica dell'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Per visualizzare informazioni sull'endpoint del mirroring del database**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Specificare un indirizzo di rete del server &#40;Mirroring del database&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Esempio: Configurazione del mirroring del database tramite l'autenticazione di Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)   
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  

