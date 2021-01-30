---
description: CREATE ENDPOINT (Transact-SQL)
title: CREATE ENDPOINT (Transact-SQL)
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENDPOINT
- CREATE ENDPOINT
- ENDPOINT_TSQL
- CREATE_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HTTP SOAP support [SQL Server]
- CREATE ENDPOINT statement
- Availability Groups [SQL Server], configuring
- endpoints [SQL Server], creating
- SOAP [SQL Server built-in support], endpoints
- SOAP [SQL Server built-in support], sqlbatch
- DATABASE_MIRRORING option
- HTTP protocol option [SQL Server]
- SOAP [SQL Server built-in support], ad hoc
- TCP protocol option [SQL Server]
- SERVICE_BROKER option
- Availability Groups [SQL Server], endpoint
ms.assetid: 6405e7ec-0b5b-4afd-9792-1bfa5a2491f6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 066159f6f98ad7b7e8c525e329bf753b3aec3500
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186892"
---
# <a name="create-endpoint-transact-sql"></a>CREATE ENDPOINT (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea gli endpoint e ne definisce le proprietà, inclusi i metodi disponibili alle applicazioni client. Per altre informazioni relative alle autorizzazioni, vedere [GRANT - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 La sintassi di CREATE ENDPOINT può essere logicamente suddivisa in due parti:  
  
-   La prima parte inizia con AS e termina prima della clausola FOR.  
  
     In questa parte vengono fornite informazioni specifiche del protocollo di trasporto TCP e viene impostato un numero di porta di attesa per l'endpoint, nonché il metodo di autenticazione dell'endpoint e/o l'elenco degli eventuali indirizzi IP che si desidera escludere dall'accesso all'endpoint.  
  
-   La seconda parte inizia dalla clausola FOR.  
  
     In questa parte viene definito il payload supportato dall'endpoint. I tipi di payload supportati sono [!INCLUDE[tsql](../../includes/tsql-md.md)], SERVICE BROKER e DATABASE MIRRORING. In questa parte è inoltre possibile includere informazioni specifiche della lingua.  
  
> **NOTA** I servizi Web XML nativi (endpoint SOAP/HTTP) sono stati rimossi in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
AS { TCP } (  
   <protocol_specific_arguments>  
        )  
FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_arguments>  
        )  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( xx.xx.xx.xx IPv4 address ) | ( '__:__1' IPv6 address ) ]  
  
)  
  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ [ , ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
   ]  
   [ [ , ] MESSAGE_FORWARDING = { ENABLED | DISABLED } ]  
   [ [ , ] MESSAGE_FORWARD_SIZE = forward_size ]  
)  
  

<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
   [ [ [ , ] ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
  
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *endPointName*  
 Nome assegnato per l'endpoint in fase di creazione. Utilizzare questo nome in caso di aggiornamento o eliminazione dell'endpoint.  
  
 AUTHORIZATION *login*  
 Specifica un account di accesso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Windows valido a cui viene assegnata la proprietà del nuovo oggetto endpoint creato. Se si omette AUTHORIZATION, per impostazione predefinita il chiamante diventerà il proprietario del nuovo oggetto creato.  
  
 Per assegnare la proprietà tramite AUTHORIZATION, il chiamante deve avere l'autorizzazione IMPERSONATE per l'account di accesso *login* specificato.  
  
 Per riassegnare la proprietà, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 STATE **=** { STARTED | **STOPPED** | DISABLED }  
 Stato dell'endpoint al momento della creazione. Se durante la procedura di creazione non viene specificato uno stato, per impostazione predefinita verrà utilizzato STOPPED.  
  
 STARTED  
 L'endpoint viene avviato ed è attivamente in attesa di connessioni.  
  
 DISABLED  
 L'endpoint è disabilitato. In questo stato, tramite il server vengono attese le richieste della porta, ma anche restituiti errori ai client.  
  
 **STOPPED**  
 L'endpoint è arrestato. In questo stato, tramite il server non vengono attese le richieste della porta dell'endpoint né viene fornita una risposta ai tentativi di richiesta di utilizzare l'endpoint.  
  
 Per modificare lo stato, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 AS { TCP }  
 Viene specificato il protocollo di trasporto da utilizzare.  
  
 FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING }  
 Specifica il tipo di payload.  
  
 Attualmente non sono presenti argomenti specifici del linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] da passare nel parametro `<language_specific_arguments>`.  
  
 **Opzione TCP**  
  
 I seguenti argomenti sono validi solo per l'opzione TCP.  
  
 LISTENER_PORT **=** _listenerPort_  
 Specifica il numero della porta della quale il protocollo TCP/IP di Service Broker è in attesa delle connessioni. Per convenzione, viene utilizzato il valore 4022 ma sono validi tutti i numeri compresi tra 1024 e 32767.  
  
 LISTENER_IP **=** ALL | **(** _4-part-ip_ **)**  |  **(** "*ip_address_v6*" **)**  
 Specifica l'indirizzo IP in corrispondenza del quale verrà eseguita l'attesa dell'endpoint. Il valore predefinito è ALL. Ciò significa che il listener accetterà una connessione su qualsiasi indirizzo IP valido.  
  
 Se si configura il mirroring del database con un indirizzo IP anziché con un nome di dominio completo (`ALTER DATABASE SET PARTNER = partner_IP_address` o `ALTER DATABASE SET WITNESS = witness_IP_address`), è necessario specificare `LISTENER_IP =IP_address` anziché `LISTENER_IP=ALL` quando si creano gli endpoint del mirroring.  
  
 **Opzioni SERVICE_BROKER e DATABASE_MIRRORING**  
  
 Gli argomenti AUTHENTICATION e ENCRYPTION di seguito descritti sono comuni alle opzioni SERVICE_BROKER e DATABASE_MIRRORING.  
  
> [!NOTE]  
>  Per le opzioni specifiche per SERVICE_BROKER, vedere "Opzioni di SERVICE_BROKER" di seguito in questo argomento. Per le opzioni specifiche per DATABASE_MIRRORING, vedere "Opzioni di DATABASE_MIRRORING" di seguito in questo argomento.  
  
 AUTHENTICATION **=** \<authentication_options> Specifica i requisiti di autenticazione TCP/IP per le connessioni relative a questo endpoint. Il valore predefinito è WINDOWS.  
  
 I metodi di autenticazione supportati includono NTLM o Kerberos oppure entrambi.  
  
> [!IMPORTANT]  
>  Tutte le connessioni per il mirroring in un'istanza del server utilizzano un singolo endpoint del mirroring del database. Qualsiasi tentativo di creare un endpoint del mirroring del database aggiuntivo avrà esito negativo.  
  
 **\<authentication_options> ::=**  
  
 **WINDOWS** [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Specifica che l'endpoint deve connettersi utilizzando il protocollo di autenticazione di Windows per autenticare gli endpoint Questa è la modalità predefinita.  
  
 Se si specifica un metodo di autenticazione (NTLM o KERBEROS), il metodo specificato viene utilizzato sempre come protocollo di autenticazione. Il valore predefinito NEGOTIATE imposta l'utilizzo da parte dell'endpoint di uno dei protocolli di negoziazione di Windows (NTLM o Kerberos).  
  
 CERTIFICATE *certificate_name*  
 Specifica che l'endpoint deve autenticare la connessione tramite il certificato specificato da *certificate_name* per definire l'identità per l'autorizzazione. L'endpoint sull'altro lato della connessione deve disporre di un certificato con chiave pubblica corrispondente alla chiave privata del certificato specificato.  
  
 WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ] CERTIFICATE *certificate_name*  
 Specifica che l'endpoint deve tentare di connettersi utilizzando l'autenticazione di Windows e, se il tentativo ha esito negativo, di provare a utilizzare il certificato specificato.  
  
 CERTIFICATE *certificate_name* WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Specifica che l'endpoint deve tentare di connettersi utilizzando il certificato specificato e, se il tentativo ha esito negativo, di provare a utilizzare l'autenticazione di Windows.  
  
 ENCRYPTION = { DISABLED \| SUPPORTED \| **REQUIRED** } [ALGORITHM { **AES** \| RC4 \| AES RC4 \| RC4 AES } ]  
 Specifica se nel processo viene utilizzata la crittografia. Il valore predefinito è REQUIRED.  
  
 DISABLED  
 Specifica che i dati inviati tramite una connessione non vengano crittografati.  
  
 SUPPORTED  
 Specifica che i dati vengano crittografati solo se per l'endpoint opposto è stato specificato SUPPORTED o REQUIRED.  
  
 REQUIRED  
 Specifica che le connessioni con questo endpoint devono utilizzare la crittografia. Per connettersi a questo endpoint, è necessario impostare l'argomento ENCRYPTION su SUPPORTED o REQUIRED per l'altro endpoint.  
  
 Facoltativamente, è possibile utilizzare l'argomento ALGORITHM per specificare la forma di crittografia utilizzata dall'endpoint, come descritto di seguito.  
  
 **AES**  
 Specifica che l'endpoint deve utilizzare l'algoritmo AES. Questo è l'impostazione predefinita in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.  
  
 RC4  
 Specifica che l'endpoint deve utilizzare l'algoritmo RC4. Questa è l'impostazione predefinita fino a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
> [!NOTE]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
  
 AES RC4  
 Specifica che due endpoint eseguiranno la negoziazione di un algoritmo di crittografia con l'endpoint corrente, dando la priorità all'algoritmo AES.  
  
 RC4 AES  
 Specifica che due endpoint eseguiranno la negoziazione di un algoritmo di crittografia con l'endpoint corrente, dando la priorità all'algoritmo RC4.  
  
> [!NOTE]  
>  L'algoritmo RC4 è deprecato. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] È consigliabile utilizzare AES.  
  
 Se i due endpoint specificano entrambi gli algoritmi, ma con un ordine diverso, l'algoritmo verrà definito dall'endpoint che accetta la connessione.  
  
 **Opzioni di SERVICE_BROKER**  
  
 Gli argomenti seguenti sono specifici dell'opzione SERVICE_BROKER.  
  
 MESSAGE_FORWARDING **=** { ENABLED | **DISABLED** }  
 Determina se i messaggi ricevuti da questo endpoint e destinati a servizi ubicati altrove verranno inoltrati.  
  
 ENABLED  
 Inoltra i messaggi se è disponibile un indirizzo di inoltro.  
  
 DISABLED  
 Cancella i messaggi per i servizi ubicati altrove. Questa è la modalità predefinita.  
  
 MESSAGE_FORWARD_SIZE **=** _forward_size_  
 Specifica lo spazio di archiviazione massimo espresso in MB da allocare per l'endpoint durante l'archiviazione dei messaggi da inoltrare.  
  
 **Opzioni di DATABASE_MIRRORING**  
  
 L'argomento seguente è specifico dell'opzione DATABASE_MIRRORING.  
  
 ROLE **=** { WITNESS | PARTNER | ALL }  
 Specifica il ruolo o i ruoli di mirroring del database supportati dall'endpoint.  
  
 WITNESS  
 Consente all'endpoint di assumere un ruolo di controllo del mirroring durante il processo di mirroring.  
  
> [!NOTE]  
>  Per [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] WITNESS è l'unica opzione disponibile.  
  
 PARTNER  
 Consente all'endpoint di assumere un ruolo di partner del mirroring durante il processo di mirroring.  
  
 ALL  
 Consente all'endpoint di assumere i ruoli di controllo e partner del mirroring durante il processo di mirroring.  
  
 Per altre informazioni su questi ruoli, vedere[Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!NOTE]  
>  Non esiste alcuna porta predefinita per DATABASE_MIRRORING.  
  
## <a name="remarks"></a>Osservazioni  
 Non è possibile eseguire le istruzioni ENDPOINT DDL all'interno di una transazione utente. Le istruzioni ENDPOINT DDL non avranno esito negativo anche in presenza di una transazione attiva a livello di isolamento dello snapshot che utilizza l'endpoint in fase di modifica.  
  
 Le richieste possono essere eseguite su ENDPOINT da:  
  
-   Membri del ruolo predefinito del server **sysadmin**  
  
-   Il proprietario dell'endpoint  
  
-   Utenti o gruppi a cui è stata concessa l'autorizzazione CONNECT per l'endpoint.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE ENDPOINT o l'appartenenza al ruolo predefinito del server **sysadmin** . Per altre informazioni, vedere [GRANT - autorizzazioni per endpoint &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="example"></a>Esempio  
  
### <a name="creating-a-database-mirroring-endpoint"></a>Creazione di un endpoint del mirroring del database  
 Nell'esempio seguente viene creato un endpoint del mirroring del database. L'endpoint utilizza il numero di porta `7022`, anche se qualsiasi numero di porta funzionerebbe correttamente. L'endpoint è configurato in modo da utilizzare solo Kerberos come modalità di autenticazione Windows. L'opzione `ENCRYPTION` è configurata sul valore non predefinito `SUPPORTED` per garantire il supporto di dati crittografati e non crittografati. L'endpoint verrà configurato in modo da supportare entrambi i ruoli partner e di controllo del mirroring.  
  
```sql  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (  
       AUTHENTICATION = WINDOWS KERBEROS,  
       ENCRYPTION = SUPPORTED,  
       ROLE=ALL);  
GO  
```  

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv4-address-and-port"></a>Creare un nuovo endpoint che punti a un indirizzo IPv4 e a una porta specifici

```sql
CREATE ENDPOINT ipv4_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = (10.0.75.1)
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public; -- Keep existing public permission on default endpoint for demo purpose
GRANT CONNECT ON ENDPOINT::ipv4_endpoint_special
TO login_name;
```

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv6-address-and-port"></a>Creare un nuovo endpoint che punti a un indirizzo IPv6 e a una porta specifici

```sql
CREATE ENDPOINT ipv6_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = ('::1')
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public;
GRANT CONNECT ON ENDPOINT::ipv6_endpoint_special

```
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

