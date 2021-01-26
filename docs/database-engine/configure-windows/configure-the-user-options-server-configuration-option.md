---
title: Configurare l'opzione di configurazione del server user options | Microsoft Docs
description: Informazioni sull'opzione "user options". Scoprire come vengono modificati i valori predefiniti delle opzioni di elaborazione delle query stabiliti da SQL Server per le sessioni di lavoro dell'utente.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- global default for all users [SQL Server]
- users [SQL Server], global defaults
- user options option [SQL Server]
ms.assetid: cfed8f86-6bcf-4b90-88eb-9656e22d5dc5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 71dcf7e52b1c58c7df868f502bc8de5da9892c6b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783434"
---
# <a name="configure-the-user-options-server-configuration-option"></a>Configurare l'opzione di configurazione del server user options
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento si illustra come configurare l'opzione di configurazione del server **user options** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'opzione **user options** consente di specificare impostazioni globali predefinite per tutti gli utenti. Viene creato un elenco di opzioni predefinite per l'elaborazione delle query, che rimane valido per tutta la durata della sessione di lavoro dell'utente. L'opzione **user options** consente di modificare i valori predefiniti delle opzioni SET, se le impostazioni predefinite del server non risultano appropriate.  
  
 Un utente può ottenere la priorità su tali impostazioni predefinite utilizzando l'istruzione SET. È possibile configurare dinamicamente **user options** per i nuovi account di accesso. Dopo aver modificato l'impostazione di **user options**, le nuove sessioni di accesso utilizzano la nuova impostazione, mentre le sessioni correnti non vengono interessate dalla modifica.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per impostare l'opzione di configurazione user options utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo l'impostazione dell'opzione di configurazione user options](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Nella tabella seguente sono elencati e descritti i valori di configurazione per **user options**. Non tutti i valori di configurazione sono compatibili tra loro. Ad esempio, non è possibile impostare contemporaneamente ANSI_NULL_DFLT_ON e ANSI_NULL_DFLT_OFF.  
  
    |valore|Configurazione|Descrizione|  
    |-----------|-------------------|-----------------|  
    |1|DISABLE_DEF_CNST_CHK|Controlla la verifica dei vincoli posticipata o provvisoria.|  
    |2|IMPLICIT_TRANSACTIONS|Per connessioni alla libreria di rete dblib, determina se una transazione viene avviata in modo implicito al momento dell'esecuzione di un'istruzione. L'impostazione IMPLICIT_TRANSACTIONS non influisce su connessioni ODBC o OLEDB.|  
    |4|CURSOR_CLOSE_ON_COMMIT|Determina il funzionamento dei cursori dopo l'esecuzione di un'operazione di commit.|  
    |8|ANSI_WARNINGS|Controlla i troncamenti e la generazione di avvisi nel caso le funzioni di aggregazione contengano valori Null.|  
    |16|ANSI_PADDING|Controlla i caratteri di riempimento nelle variabili di lunghezza fissa.|  
    |32|ANSI_NULLS|Controlla la gestione dei valori Null con gli operatori di uguaglianza.|  
    |64|ARITHABORT|Interrompe una query quando si verifica un errore di divisione per zero o di overflow durante l'esecuzione della query stessa.|  
    |128|ARITHIGNORE|Restituisce un valore Null quando durante l'esecuzione di una query si verifica un errore di overflow o di divisione per zero.|  
    |256|QUOTED_IDENTIFIER|Riconosce la differenza tra virgolette doppie e singole per la valutazione di un'espressione.|  
    |512|NOCOUNT|Disabilita la restituzione del messaggio che indica il numero di righe interessate al termine di ogni istruzione.|  
    |1024|ANSI_NULL_DFLT_ON|Modifica il funzionamento della sessione in modo che venga utilizzata la compatibilità ANSI per il supporto di valori Null. Nelle nuove colonne definite senza supporto esplicito dei valori Null sarà possibile utilizzare valori Null.|  
    |2048|ANSI_NULL_DFLT_OFF|Modifica il funzionamento della sessione in modo che non venga utilizzata la compatibilità ANSI per il supporto di valori Null. Nelle nuove colonne definite senza supporto esplicito dei valori Null non sarà possibile utilizzare valori Null.|  
    |4096|CONCAT_NULL_YIELDS_NULL|Restituisce NULL in seguito alla concatenazione di un valore Null con una stringa.|  
    |8192|NUMERIC_ROUNDABORT|Genera un errore quando in un'espressione si verifica una perdita di precisione.|  
    |16384|XACT_ABORT|Esegue il rollback di una transazione se un'istruzione Transact-SQL genera un errore di run-time.|  
  
-   Le posizioni dei bit in **user options** sono identiche a quelle in @@OPTIONS. Ogni connessione dispone della funzione @@OPTIONS corrispondente, che rappresenta l'ambiente di configurazione. Per ogni utente che accede a un'istanza di \ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]è disponibile un ambiente predefinito che assegna il valore corrente di **user options** a @@OPTIONS. L'esecuzione di istruzioni SET per **user options** ha effetto sul valore corrispondente nella funzione @@OPTIONS per la sessione. Tutte le connessioni create dopo la modifica di questa impostazione utilizzeranno il nuovo valore.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Per impostare l'opzione di configurazione user options  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Connessioni** .  
  
3.  Nella casella **Opzioni di connessione predefinite** selezionare uno o più attributi per configurare le opzioni predefinite di elaborazione delle query per tutti gli utenti connessi.  
  
     Per impostazione predefinita, non è configurata alcuna opzione utente.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Per impostare l'opzione di configurazione user options  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene illustrato come utilizzare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per configurare `user options` in modo da modificare l'impostazione server ANSI_WARNINGS.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'user options', 8 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-user-options-configuration-option"></a><a name="FollowUp"></a> Completamento: Dopo l'impostazione dell'opzione di configurazione user options  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
