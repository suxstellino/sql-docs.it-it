---
description: CREATE BROKER PRIORITY (Transact-SQL)
title: CREATE BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE BROKER PRIORITY
- PRIORITY_TSQL
- CREATE_BROKER_PRIORITY_TSQL
- PRIORITY
- CREATE BROKER
- BROKER_TSQL
- BROKER
- CREATE_BROKER_TSQL
- BROKER PRIORITY
- BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE BROKER PRIORITY statement
ms.assetid: e0bbebfa-b7c3-4825-8169-7281f7e6de98
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b027d1e0472a2154eaff8540e5dcc4b93f7be930
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100215"
---
# <a name="create-broker-priority-transact-sql"></a>CREATE BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Definisce un livello di priorità e il set di criteri da utilizzare per determinare a quali conversazioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] deve essere assegnato il livello di priorità. Il livello di priorità è assegnato a qualsiasi endpoint di conversazione che usa la stessa combinazione di contratti e servizi specificata nella priorità di conversazione. Il valore delle priorità va da 1 (basso) a 10 (alto). Il valore predefinito è 5.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
[ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = {PriorityValue | DEFAULT } ]  
       )  
]  
[;]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *ConversationPriorityName*  
 Specifica il nome per la priorità di conversazione. Il nome deve essere univoco nel database corrente e deve essere conforme alle regole per gli  [identificatori](../../relational-databases/databases/database-identifiers.md) di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 SET  
 Specifica i criteri per determinare se la priorità di conversazione si applica a una conversazione. Se specificato, l'argomento SET deve contenere almeno un criterio: CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME o PRIORITY_LEVEL. Se SET non è specificato, per tutti e tre i criteri vengono utilizzate le impostazioni predefinite.  
  
 CONTRACT_NAME = {*ContractName* | **ANY**}  
 Viene specificato il nome di un contratto da utilizzare come criterio per determinare se la priorità di conversazione si applica a una conversazione. *ContractName* è un identificatore [!INCLUDE[ssDE](../../includes/ssde-md.md)] e deve specificare il nome di un contratto nel database corrente.  
  
 *ContractName*  
 Specifica che la priorità di conversazione può essere applicata solo alle conversazioni dove l'istruzione BEGIN DIALOG che ha avviato la conversazione ha specificato il valore ON CONTRACT *ContractName*.  
  
 ANY  
 Specifica che la priorità di conversazione può essere applicata a qualsiasi conversazione, indipendentemente dal contratto utilizzato.  
  
 Il valore predefinito è ANY.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Specifica il nome di un servizio da utilizzare come criterio per determinare se la priorità di conversazione si applica a un endpoint di conversazione.  
  
 *LocalServiceName* è un identificatore [!INCLUDE[ssDE](../../includes/ssde-md.md)]. e deve specificare il nome di un servizio nel database corrente.  
  
 *LocalServiceName*  
 Specifica che la priorità di conversazione può essere applicata agli elementi seguenti:  
  
-   Qualsiasi endpoint di conversazione dell'initiator il cui nome del servizio Initiator corrisponde a *LocalServiceName*.  
  
-   Qualsiasi endpoint di conversazione di destinazione il cui nome del servizio di destinazione corrisponde a *LocalServiceName*.  
  
 ANY  
 -   Specifica che la priorità di conversazione può essere applicata a qualsiasi endpoint di conversazione, indipendentemente dal nome del servizio locale utilizzato dall'endpoint.  
  
 Il valore predefinito è ANY.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Specifica il nome di un servizio da utilizzare come criterio per determinare se la priorità di conversazione si applica a un endpoint di conversazione.  
  
 *RemoteServiceName* è un valore letterale di tipo **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa un confronto byte per byte per la corrispondenza con la stringa *RemoteServiceName*. Nel confronto viene fatta distinzione tra maiuscole e minuscole e non vengono considerate le regole di confronto correnti. Il servizio di destinazione può trovarsi nell'istanza corrente di [!INCLUDE[ssDE](../../includes/ssde-md.md)] o in un'istanza remota di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 '*RemoteServiceName*'  
 Specifica che la priorità di conversazione può essere applicata agli elementi seguenti:  
  
-   Qualsiasi endpoint di conversazione dell'initiator il cui nome del servizio di destinazione associato corrisponde a *RemoteServiceName*.  
  
-   Qualsiasi endpoint di conversazione di destinazione il cui nome del servizio dell'initiator associato corrisponde a *RemoteServiceName*.  
  
 ANY  
 Specifica che la priorità di conversazione può essere applicata a qualsiasi endpoint di conversazione, indipendentemente dal nome del servizio remoto associato all'endpoint.  
  
 Il valore predefinito è ANY.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Viene specificata la priorità da assegnare a qualsiasi endpoint di conversazione in cui vengono utilizzati i contratti e i servizi specificati nella priorità di conversazione. *PriorityValue* deve essere un valore letterale intero compreso tra 1 (priorità più bassa) e 10 (priorità più elevata). Il valore predefinito è 5.  
  
## <a name="remarks"></a>Osservazioni  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assegna i livelli di priorità agli endpoint di conversazione. I livelli di priorità controllano la priorità delle operazioni associate all'endpoint. Ciascuna conversazione presenta due endpoint di conversazione:  
  
-   L'endpoint di conversazione dell'initiator associa un lato della conversazione al servizio Initiator e alla coda dell'initiator. L'endpoint di conversazione dell'Initiator viene creato quando l'istruzione BEGIN DIALOG viene eseguita. Le operazioni associate all'endpoint di conversazione dell'initiator includono:  
  
    -   Invio dal servizio Initiator.  
  
    -   Ricezione dalla coda dell'initiator.  
  
    -   Recupero del successivo gruppo di conversazioni dalla coda dell'Initiator.  
  
-   L'endpoint di conversazione di destinazione associa l'altro lato della conversazione al servizio e alla coda di destinazione. L'endpoint di conversazione di destinazione viene creato quando la conversazione viene utilizzata per inviare un messaggio alla coda di destinazione. Le operazioni associate all'endpoint di conversazione di destinazione includono:  
  
    -   Ricezione dalla coda di destinazione.  
  
    -   Invio dal servizio di destinazione.  
  
    -   Recupero del gruppo di conversazioni successivo dalla coda di destinazione.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assegna i livelli di priorità di conversazione quando vengono creati gli endpoint di conversazione. L'endpoint di conversazione conserva il livello di priorità fino alla fine della conversazione. Le nuove priorità o le modifiche alle priorità esistenti non sono applicate alle conversazioni esistenti.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assegna il livello di priorità a un endpoint di conversazione in base alla priorità di conversazione con i criteri di contratto e servizi maggiormente corrispondenti alle proprietà dell'endpoint. Nella tabella seguente viene indicata la precedenza tra le corrispondenze:  
  
|Contratto dell'operazione|Servizio locale dell'operazione|Servizio remoto dell'operazione|  
|------------------------|-----------------------------|------------------------------|  
|*ContractName*|*LocalServiceName*|*RemoteServiceName*|  
|*ContractName*|*LocalServiceName*|ANY|  
|*ContractName*|ANY|*RemoteServiceName*|  
|*ContractName*|ANY|ANY|  
|ANY|*LocalServiceName*|*RemoteServiceName*|  
|ANY|*LocalServiceName*|ANY|  
|ANY|ANY|*RemoteServiceName*|  
|ANY|ANY|ANY|  
  
 Tramite [!INCLUDE[ssSB](../../includes/sssb-md.md)] viene innanzitutto eseguita la ricerca di una priorità i cui contratto, servizio locale e servizio remoto specificati corrispondano a quelli utilizzati dall'operazione. Se tale priorità non viene trovata, tramite [!INCLUDE[ssSB](../../includes/sssb-md.md)] viene eseguita la ricerca di una priorità con un contratto e un servizio locale corrispondenti a quelli utilizzati dall'operazione e per cui il servizio remoto sia stato specificato come ANY. La ricerca procede quindi in questo modo per tutte le variazioni elencate nella tabella delle precedenze. Se non viene trovata alcuna corrispondenza, all'operazione viene assegnata la priorità predefinita di 5.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assegna indipendentemente un livello di priorità a ogni endpoint di conversazione. Affinché [!INCLUDE[ssSB](../../includes/sssb-md.md)] assegni livelli di priorità agli endpoint di conversazione sia dell'initiator che di destinazione, è necessario verificare che per entrambi gli endpoint siano state specificate priorità di conversazione. Se gli endpoint di conversazione dell'initiator e di destinazione sono in database separati, è necessario creare le priorità di conversazione in ogni database. In genere per entrambi gli endpoint di conversazione viene specificato lo stesso livello di priorità, ma è possibile specificare livelli di priorità diversi.  
  
 I livelli di priorità sono applicati sempre a operazioni che ricevono messaggi o identificatori di gruppi di conversazioni da una coda. I livelli di priorità vengono applicati anche durante la trasmissione di messaggi da un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] a un'altra.  
  
 I livelli di priorità non vengono utilizzati quando i messaggi vengono trasferiti:  
  
-   Da un database in cui l'opzione HONOR_BROKER_PRIORITY è impostata su OFF. Per altre informazioni, vedere [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
-   Tra servizi nella stessa istanza del Motore di database.  
  
-   Se non sono state create priorità di conversazione nel database, a tutte le operazioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] in un database viene assegnata la priorità predefinita 5.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la creazione di una priorità di conversazione viene assegnata per impostazione predefinita ai membri del ruolo predefinito del database db_ddladmin o db_owner e al ruolo predefinito del server sysadmin. È richiesta l'autorizzazione ALTER per il database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-assigning-a-priority-level-to-both-directions-of-a-conversation"></a>R. Assegnazione di un livello di priorità a entrambe le direzioni di una conversazione.  
 Queste due priorità di conversazione assicurano che a tutte le operazioni che utilizzano `SimpleContract` tra `TargetService` e `InitiatorAService` venga assegnato il livello di priorità 3.  
  
```sql  
CREATE BROKER PRIORITY InitiatorAToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = InitiatorServiceA,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 3);  
CREATE BROKER PRIORITY TargetToInitiatorAPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceA',  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-setting-the-priority-level-for-all-conversations-that-use-a-contract"></a>B. Impostazione del livello di priorità per tutte le conversazioni in cui viene utilizzato un contratto  
 Viene assegnato un livello di priorità `7` a tutte le operazioni che utilizzano un contratto denominato `SimpleContract`. Questo presuppone che non vi siano altre priorità che specificano `SimpleContract` e un servizio locale o remoto.  
  
```sql 
CREATE BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 7);  
```  
  
### <a name="c-setting-a-base-priority-level-for-a-database"></a>C. Impostazione di un livello di priorità di base per un database.  
 Vengono definite le priorità di conversazione per due servizi specifici e quindi una priorità di conversazione per tutti gli altri endpoint di conversazione. La priorità predefinita non viene sostituita, e rimane sempre 5, ma viene ridotto il numero di elementi a cui è assegnato il valore predefinito.  
  
```sql 
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ClaimPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 9);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ApprovalPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/BasePriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="d-creating-three-priority-levels-for-a-target-service-by-using-services"></a>D. Creazione di tre livelli di priorità per un servizio di destinazione utilizzando i servizi  
 L'esempio seguente supporta un sistema che fornisce tre livelli di prestazione: Gold (più elevato), Silver (medio) e Bronze (più basso). È presente un contratto, ma ogni livello prevede un servizio Initiator separato. Tutti i servizi Initiator comunicano con un servizio di destinazione centrale.  
  
```sql  
CREATE BROKER PRIORITY GoldInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = GoldInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY GoldTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'GoldInitiatorService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = SilverInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY SilverTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'SilverInitiatorService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzeInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = BronzeInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 2);  
CREATE BROKER PRIORITY BronzeTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'BronzeInitiatorService',  
         PRIORITY_LEVEL = 2);  
```  
  
### <a name="e-creating-three-priority-levels-for-multiple-services-using-contracts"></a>E. Creazione di tre livelli di priorità per più servizi utilizzando i contratti  
 L'esempio seguente supporta un sistema che fornisce tre livelli di prestazione: Gold (più elevato), Silver (medio) e Bronze (più basso). Per ogni livello è presente un contratto separato. Queste priorità si applicano a qualsiasi servizio a cui fanno riferimento le conversazioni che utilizzano i contratti.  
  
```sql  
CREATE BROKER PRIORITY GoldPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = GoldContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SilverContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzePriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = BronzeContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 2);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
