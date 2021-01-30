---
description: END CONVERSATION (Transact-SQL)
title: END CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- END DIALOG
- END CONVERSATION
- END_DIALOG_TSQL
- END_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [Service Broker], conversations
- dialogs [Service Broker], ending
- closing conversations
- END CONVERSATION statement
- conversations [Service Broker], ending
- ending conversations [SQL Server]
ms.assetid: 4415a126-cd22-4a5e-b84a-d8c68515c83b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83d0acb45d13442e2c538878d3336688513e6519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183008"
---
# <a name="end-conversation-transact-sql"></a>END CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Termina un lato di una conversazione esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
END CONVERSATION conversation_handle  
   [   [ WITH ERROR = failure_code DESCRIPTION = 'failure_text' ]  
     | [ WITH CLEANUP ]  
    ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *conversation_handle*  
 Handle della conversazione da terminare.  
  
 WITH ERROR =*failure_code*  
 Codice di errore. *failure_code* è di tipo **int**. Il codice di errore è un codice definito dall'utente incluso nel messaggio di errore inviato all'altro lato della conversazione. Il codice di errore deve essere maggiore di 0.  
  
 DESCRIPTION =*failure_text*  
 Messaggio di errore. *failure_text* è di tipo **nvarchar(3000)**. Il testo dell'errore è un messaggio testuale definito dall'utente incluso nel messaggio di errore inviato all'altro lato della conversazione.  
  
 WITH CLEANUP  
 Rimuove tutti i messaggi e le voci della vista del catalogo per un lato di una conversazione che non è possibile completare normalmente. La pulizia non viene notificata all'altro lato della conversazione. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina l'endpoint della conversazione, tutti i messaggi per la conversazione nella coda di trasmissione e tutti i messaggi per la conversazione nella coda del servizio. Gli amministratori possono utilizzare questa opzione per rimuovere le conversazioni che non possono essere completate normalmente. Se, ad esempio, il servizio remoto viene rimosso in modo definitivo, un amministratore può utilizzare WITH CLEANUP per rimuovere le conversazioni associate a tale servizio. Non usare WITH CLEANUP nel codice di un'applicazione [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Se si esegue END CONVERSATION WITH CLEANUP prima che l'endpoint di ricezione confermi la ricezione di un messaggio, l'endpoint di invio invierà di nuovo il messaggio. In questo modo, il dialogo potrebbe essere eseguito di nuovo.  
  
## <a name="remarks"></a>Commenti  
 Quando si termina una conversazione, viene bloccato il gruppo di conversazioni a cui appartiene il valore *conversation_handle* specificato. Al termine di una conversazione, tramite [!INCLUDE[ssSB](../../includes/sssb-md.md)] vengono rimossi dalla coda del servizio tutti i messaggi corrispondenti.  
  
 Dopo aver terminato una conversazione, le applicazioni non possono più inviare o ricevere messaggi per tale conversazione. Entrambi i partecipanti alla conversazione devono chiamare END CONVERSATION per completare la conversazione. Se [!INCLUDE[ssSB](../../includes/sssb-md.md)] non riceve un messaggio di fine dialogo o un messaggio di errore dall'altro partecipante alla conversazione, tale partecipante riceve da [!INCLUDE[ssSB](../../includes/sssb-md.md)] una notifica della fine della conversazione. In questo caso, sebbene l'handle della conversazione non sia più valido, l'endpoint della conversazione rimane attivo fino a quando l'istanza che ospita il servizio remoto non invia un acknowledgement per il messaggio di notifica.  
  
 Se [!INCLUDE[ssSB](../../includes/sssb-md.md)] non ha già elaborato un messaggio di fine dialogo o un messaggio di errore per la conversazione, il lato remoto riceve da [!INCLUDE[ssSB](../../includes/sssb-md.md)] una notifica della fine della conversazione. I messaggi inviati da [!INCLUDE[ssSB](../../includes/sssb-md.md)] al servizio remoto dipendono dalle opzioni specificate:  
  
-   Se la conversazione termina senza errori e la conversazione con il servizio remoto è ancora attiva, [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia un messaggio di tipo `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog` al servizio remoto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] aggiunge questo messaggio alla coda di trasmissione nell'ordine della conversazione. Prima di inviare questo messaggio, [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia tutti i messaggi per questa conversazione che sono attualmente nella coda di trasmissione.  
  
-   Se la conversazione termina con un errore e la conversazione con il servizio remoto è ancora attiva, [!INCLUDE[ssSB](../../includes/sssb-md.md)] invia un messaggio di tipo `https://schemas.microsoft.com/SQL/ServiceBroker/Error` al servizio remoto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] elimina tutti gli altri messaggi per questa conversazione che sono attualmente nella coda di trasmissione.  
  
-   La clausola WITH CLEANUP consente agli amministratori di database di rimuovere le conversazioni che non possono essere completate normalmente. Questa opzione rimuove tutti i messaggi e le voci della vista del catalogo per la conversazione. Si noti che, in questo caso, il lato remoto della conversazione non riceve notifica della fine della conversazione e potrebbe non ricevere i messaggi inviati da un'applicazione ma non ancora trasmessi in rete. È consigliabile utilizzare questa opzione solo se la conversazione non può essere completata normalmente.  
  
 Dopo la fine di una conversazione, l'esecuzione di un'istruzione SEND [!INCLUDE[tsql](../../includes/tsql-md.md)] che specifica l'handle della conversazione causa un errore [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se vengono ricevuti messaggi per questa conversazione provenienti dall'altro lato della conversazione, tali messaggi vengono eliminati da [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 Se una conversazione termina mentre nel servizio remoto sono ancora presenti messaggi non inviati per la conversazione, il servizio remoto elimina i messaggi non inviati. Ciò non viene considerato un errore e pertanto il servizio remoto non riceve alcuna notifica dell'eliminazione dei messaggi.  
  
 I codici di errore specificati nella clausola WITH ERROR devono essere numeri positivi. I numeri negativi sono riservati ai messaggi di errore di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 END CONVERSATION non è un'istruzione valida in una funzione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per terminare una conversazione attiva, l'utente corrente deve essere il proprietario della conversazione, un membro del ruolo predefinito del server sysadmin o un membro del ruolo predefinito del database db_owner.  
  
 I membri del ruolo predefinito del server sysadmin o del ruolo predefinito del database db_owner possono utilizzare WITH CLEANUP per rimuovere i metadati di una conversazione già completata.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-ending-a-conversation"></a>R. Fine di una conversazione  
 Nell'esempio seguente viene terminato il dialogo specificato da `@dialog_handle`.  
  
```sql 
END CONVERSATION @dialog_handle ;  
```  
  
### <a name="b-ending-a-conversation-with-an-error"></a>B. Fine di una conversazione con un errore  
 Nell'esempio seguente viene terminato il dialogo specificato da `@dialog_handle` con un errore, se l'istruzione di elaborazione restituisce un errore. Si noti che la gestione dell'errore illustrata nell'esempio è semplicistica e potrebbe non essere appropriata per alcune applicazioni.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ErrorSave INT,  
        @ErrorDesc NVARCHAR(100) ;  
BEGIN TRANSACTION ;  
  
<receive and process message>  
  
SET @ErrorSave = @@ERROR ;  
  
IF (@ErrorSave <> 0)  
  BEGIN  
      ROLLBACK TRANSACTION ;  
      SET @ErrorDesc = N'An error has occurred.' ;  
      END CONVERSATION @dialog_handle   
      WITH ERROR = @ErrorSave DESCRIPTION = @ErrorDesc ;  
  END  
ELSE  
  
COMMIT TRANSACTION ;  
```  
  
### <a name="c-cleaning-up-a-conversation-that-cannot-complete-normally"></a>C. Pulizia di una conversazione che non può essere completata normalmente  
 Nell'esempio seguente viene terminato il dialogo specificato da `@dialog_handle`. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono rimossi immediatamente tutti i messaggi dalla coda del servizio e dalla coda di trasmissione, senza inviare notifica al servizio remoto. Poiché la fine di un dialogo con pulizia non prevede notifiche al servizio remoto, è consigliabile usare questa operazione solo nei casi in cui il servizio remoto non è disponibile per la ricezione di un messaggio **EndDialog** or **Error**.  
  
```sql  
END CONVERSATION @dialog_handle WITH CLEANUP ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
