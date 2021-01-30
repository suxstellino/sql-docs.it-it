---
description: ALTER BROKER PRIORITY (Transact-SQL)
title: ALTER BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_BROKER_TSQL
- ALTER BROKER PRIORITY
- ALTER BROKER
- ALTER_BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER BROKER PRIORITY statement
- ssbdiagnose
ms.assetid: 15fda1b2-e4dd-4f9d-935a-2e38926075b2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8d9fa209e3f2c9fbfceeeec4a2a58a0fe633ba7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191072"
---
# <a name="alter-broker-priority-transact-sql"></a>ALTER BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà di una priorità di conversazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
ALTER BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
{ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = { PriorityValue | DEFAULT } ]  
              )  
}  
[;]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *ConversationPriorityName*  
 Specifica il nome della priorità di conversazione da modificare. Il nome deve fare riferimento a una priorità di conversazione nel database corrente.  
  
 SET  
 Specifica i criteri per determinare se la priorità di conversazione si applica a una conversazione. L'argomento SET è obbligatorio e deve contenere almeno un criterio: CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME o PRIORITY_LEVEL.  
  
 CONTRACT_NAME = {*ContractName* | **ANY**}  
 Viene specificato il nome di un contratto da utilizzare come criterio per determinare se la priorità di conversazione si applica a una conversazione. *ContractName* è un identificatore [!INCLUDE[ssDE](../../includes/ssde-md.md)] e deve specificare il nome di un contratto nel database corrente.  
  
 *ContractName*  
 Specifica che la priorità di conversazione può essere applicata solo alle conversazioni dove l'istruzione BEGIN DIALOG che ha avviato la conversazione ha specificato il valore ON CONTRACT *ContractName*.  
  
 ANY  
 Specifica che la priorità di conversazione può essere applicata a qualsiasi conversazione, indipendentemente dal contratto utilizzato.  
  
 Se CONTRACT_NAME non è specificato, la proprietà del contratto relativa alla priorità di conversazione non viene modificata.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Specifica il nome di un servizio da utilizzare come criterio per determinare se la priorità di conversazione si applica a un endpoint di conversazione.  
  
 *LocalServiceName* è un identificatore [!INCLUDE[ssDE](../../includes/ssde-md.md)] e deve specificare il nome di un servizio nel database corrente.  
  
 *LocalServiceName*  
 Specifica che la priorità di conversazione può essere applicata agli elementi seguenti:  
  
-   Qualsiasi endpoint di conversazione dell'initiator il cui nome del servizio Initiator corrisponde a *LocalServiceName*.  
  
-   Qualsiasi endpoint di conversazione di destinazione il cui nome del servizio di destinazione corrisponde a *LocalServiceName*.  
  
 ANY  
 -   Specifica che la priorità di conversazione può essere applicata a qualsiasi endpoint di conversazione, indipendentemente dal nome del servizio locale utilizzato dall'endpoint.  
  
 Se LOCAL_SERVICE_NAME non è specificato, la proprietà del servizio locale relativa alla priorità di conversazione non viene modificata.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Specifica il nome di un servizio da utilizzare come criterio per determinare se la priorità di conversazione si applica a un endpoint di conversazione.  
  
 *RemoteServiceName* è un valore letterale di tipo **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa un confronto byte per byte per la corrispondenza con la stringa *RemoteServiceName*. Nel confronto viene fatta distinzione tra maiuscole e minuscole e non vengono considerate le regole di confronto correnti. Il servizio di destinazione può trovarsi nell'istanza corrente di [!INCLUDE[ssDE](../../includes/ssde-md.md)] o in un'istanza remota di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 '*RemoteServiceName*'  
 Specifica la priorità di conversazione da assegnare agli elementi seguenti:  
  
-   Qualsiasi endpoint di conversazione dell'initiator il cui nome del servizio di destinazione associato corrisponde a *RemoteServiceName*.  
  
-   Qualsiasi endpoint di conversazione di destinazione il cui nome del servizio dell'initiator associato corrisponde a *RemoteServiceName*.  
  
 ANY  
 Specifica che la priorità di conversazione si applica a qualsiasi endpoint di conversazione, indipendentemente dal nome del servizio remoto associato all'endpoint.  
  
 Se REMOTE_SERVICE_NAME non è specificato, la proprietà del servizio remoto relativa alla priorità di conversazione non viene modificata.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Specifica il livello di priorità da assegnare a qualsiasi endpoint di conversazione che utilizza i contratti e servizi specificati nella priorità di conversazione. *PriorityValue* deve essere un valore letterale intero compreso tra 1 (priorità più bassa) e 10 (priorità più elevata).  
  
 Se PRIORITY_LEVEL non è specificato, la proprietà del livello di priorità relativa alla priorità di conversazione non viene modificata.  
  
## <a name="remarks"></a>Commenti  
 Nessuna delle proprietà modificate da ALTER BROKER PRIORITY viene applicata alle conversazioni esistenti. Le conversazioni esistenti continuano con la priorità assegnata al momento dell'avvio.  
  
 Per altre informazioni, vedere [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la creazione di una priorità di conversazione viene assegnata per impostazione predefinita ai membri del ruolo predefinito del database **db_ddladmin** o **db_owner** e al ruolo predefinito del server **sysadmin**. È richiesta l'autorizzazione ALTER per il database.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-only-the-priority-level-of-an-existing-conversation-priority"></a>R. Modifica esclusivamente del livello di priorità di una priorità di conversazione esistente.  
 Modifica il livello di priorità, ma non modifica le proprietà del contratto, del servizio locale o del servizio remoto.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-changing-all-of-the-properties-of-an-existing-conversation-priority"></a>B. Modifica di tutte le proprietà di una priorità di conversazione esistente.  
 Modifica il livello di priorità e le proprietà del contratto, del servizio locale e del servizio remoto.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContractB,  
         LOCAL_SERVICE_NAME = TargetServiceB,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceB',  
         PRIORITY_LEVEL = 8);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
