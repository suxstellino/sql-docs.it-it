---
description: CREATE SERVICE (Transact-SQL)
title: CREATE SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SERVICE_TSQL
- SERVICE
- CREATE SERVICE
- SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- services [Service Broker], creating
- CREATE SERVICE statement
- contracts [Service Broker], service creation
ms.assetid: fb804fa2-48eb-4878-a12f-4e0d5f4bc9e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f4c3d54c74d21994974ea1cdf47dbea07d8171ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097001"
---
# <a name="create-service-transact-sql"></a>CREATE SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuovo servizio. Un servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un nome per un'attività specifica o per un set di attività. Il nome del servizio viene utilizzato da [!INCLUDE[ssSB](../../includes/sssb-md.md)] per il routing dei messaggi, il recapito dei messaggi alla coda corretta in un database nonché per l'applicazione del contratto per una conversazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE SERVICE service_name  
   [ AUTHORIZATION owner_name ]  
   ON QUEUE [ schema_name. ]queue_name  
   [ ( contract_name | [DEFAULT][ ,...n ] ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *service_name*  
 Nome del servizio da creare. I nuovi servizi vengono creati nel database corrente e sono di proprietà dell'entità specificata nella clausola AUTHORIZATION. Non è possibile specificare i nomi del server, del database e dello schema. *route_name* deve essere un valore **sysname** valido.  
  
> [!NOTE]  
> Non creare un servizio che usa la parola chiave ANY per *service_name*. Quando si specifica `ANY` per un nome del servizio in `CREATE BROKER PRIORITY`, la priorità viene considerata per tutti i servizi e non è limitata a un servizio il cui nome è ANY.  
  
 AUTHORIZATION *owner_name*  
 Imposta come proprietario del servizio l'utente o il ruolo del database specificato. Se l'utente corrente è **dbo** o **sa**, *owner_name* può essere il nome di qualsiasi utente o ruolo valido. In caso contrario, *owner_name* deve corrispondere al nome dell'utente corrente, al nome di un utente per il quale l'utente corrente dispone di autorizzazione IMPERSONATE oppure al nome di un ruolo a cui appartiene l'utente corrente.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Specifica la coda che riceve i messaggi per il servizio. La coda deve esistere nello stesso database del servizio. Se non si specifica *schema_name* viene usato lo schema predefinito per l'utente che esegue l'istruzione.  
  
 *contract_name*  
 Specifica il contratto da utilizzare quando il servizio funge da servizio di destinazione per conversazioni. I programmi del servizio avviano le conversazioni con questo servizio utilizzando i contratti specificati. Se non si specifica alcun contratto, il servizio può solo avviare conversazioni.  
  
 **[** DEFAULT **]**  
 Specifica che il servizio può fungere da servizio di destinazione per le conversazioni che seguono il contratto DEFAULT. Nel contesto di questa clausola DEFAULT non è una parola chiave ed è pertanto necessario delimitarlo come identificatore. Il contratto DEFAULT consente a entrambi i lati della conversazione di inviare messaggi di tipo DEFAULT. Per il tipo di messaggi DEFAULT viene utilizzata la convalida NONE.  
  
## <a name="remarks"></a>Osservazioni  
 Un servizio espone la funzionalità fornita dai contratti a cui è associato, rendendone possibile l'utilizzo da parte di altri servizi. L'istruzione `CREATE SERVICE` specifica i contratti per cui questo servizio funge da destinatario. Un servizio può fungere da destinatario solo per le conversazioni che utilizzano i contratti specificati dal servizio. Un servizio che non specifica alcun contratto non espone alcuna funzionalità per gli altri servizi.  
  
 Le conversazioni avviate da questo servizio possono utilizzare qualsiasi contratto. Creare un servizio senza specificare alcun contratto se il servizio deve solo avviare conversazioni.  
  
 Quando [!INCLUDE[ssSB](../../includes/sssb-md.md)] accetta una nuova conversazione da un servizio remoto, il nome del servizio di destinazione determina la coda in cui Service Broker inserirà i messaggi della conversazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la creazione di un servizio viene assegnata per impostazione predefinita ai membri del ruolo predefinito del database `db_ddladmin` o `db_owner` o del ruolo predefinito del server `sysadmin`. L'utente che esegue l'istruzione `CREATE SERVICE` deve disporre dell'autorizzazione `REFERENCES` per la coda e per tutti i contratti specificati.  
  
 L'autorizzazione `REFERENCES` per un servizio viene assegnata per impostazione predefinita al proprietario del servizio, ai membri del ruolo predefinito del database `db_ddladmin` o `db_owner` o ai membri del ruolo predefinito del server `sysadmin`. Le autorizzazioni `SEND` per un servizio vengono assegnate per impostazione predefinita al proprietario del servizio, ai membri del ruolo predefinito del database `db_owner` o ai membri del ruolo predefinito del server `sysadmin`.  
  
 Un servizio non può essere un oggetto temporaneo. I nomi dei servizi che iniziano con **#** sono consentiti, ma sono oggetti permanenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-service-with-one-contract"></a>R. Creazione di un servizio con un contratto  
 Nell'esempio seguente viene creato il servizio `//Adventure-Works.com/Expenses` per la coda `ExpenseQueue` nello schema `dbo`. I dialoghi per questo servizio devono essere basati sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE [dbo].[ExpenseQueue]  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="b-creating-a-service-with-multiple-contracts"></a>B. Creazione di un servizio con più contratti  
 Nell'esempio seguente viene creato il servizio `//Adventure-Works.com/Expenses` per la coda `ExpenseQueue`. I dialoghi per questo servizio devono essere basati sul contratto `//Adventure-Works.com/Expenses/ExpenseSubmission` oppure sul contratto `//Adventure-Works.com/Expenses/ExpenseProcessing`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission],  
     [//Adventure-Works.com/Expenses/ExpenseProcessing]) ;  
```  
  
### <a name="c-creating-a-service-with-no-contracts"></a>C. Creazione di un servizio senza contratti  
 Nell'esempio seguente viene creata la coda del servizio `//Adventure-Works.com/Expenses on the ExpenseQueue`. Per questo servizio non è stato specificato alcun contratto, quindi il servizio può solo fungere da initiator di un dialogo.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
