---
description: sp_addlinkedsrvlogin (Transact-SQL)
title: sp_addlinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedsrvlogin_TSQL
- sp_addlinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedsrvlogin
ms.assetid: eb69f303-1adf-4602-b6ab-f62e028ed9f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bcefbe0acef90deb488a1d95f162a4d94f60aa95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202381"
---
# <a name="sp_addlinkedsrvlogin-transact-sql"></a>sp_addlinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea o aggiorna un mapping tra un account di accesso nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e un account di sicurezza in un server remoto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_addlinkedsrvlogin [ @rmtsrvname = ] 'rmtsrvname'   
     [ , [ @useself = ] { 'TRUE' | 'FALSE' | NULL } ]   
     [ , [ @locallogin = ] 'locallogin' ]   
     [ , [ @rmtuser = ] 'rmtuser' ]   
     [ , [ @rmtpassword = ] 'rmtpassword' ]   
```  
  
## <a name="arguments"></a>Argomenti  
 `[ @rmtsrvname = ] 'rmtsrvname'`  
 Nome di un server collegato a cui viene applicato il mapping dell'account di accesso. *rmtsrvname* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 `[ @useself = ] { 'TRUE' | 'FALSE' | NULL }'`  
 Determina se connettersi a *rmtsrvname* tramite rappresentazione di account di accesso locali o inviando esplicitamente un account di accesso e una password. Il tipo di dati è **varchar (** 8 **)** e il valore predefinito è true.  
  
 Il valore TRUE indica che gli account di accesso utilizzano le proprie credenziali per connettersi a *rmtsrvname*, con gli argomenti *rmtuser* e *rmtpassword* ignorati. FALSE specifica che gli argomenti *rmtuser* e *rmtpassword* vengono usati per connettersi a *rmtsrvname* per il *locallogin* specificato. Se anche *rmtuser* e *rmtpassword* sono impostati su null, per la connessione al server collegato non viene utilizzato alcun account di accesso o password.  
  
 `[ @locallogin = ] 'locallogin'`  
 Account di accesso per il server locale. *locallogin* è di **tipo sysname** e il valore predefinito è null. NULL specifica che questa voce si applica a tutti gli account di accesso locali che si connettono a *rmtsrvname*. Se non è NULL, *locallogin* può essere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso di o un account di accesso di Windows. È necessario che l'account di accesso di Windows disponga dell'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ottenuto tramite concessione diretta o in seguito all'appartenenza a un gruppo di Windows che dispone dell'accesso.  
  
 `[ @rmtuser = ] 'rmtuser'`  
 Account di accesso remoto utilizzato per connettersi a *rmtsrvname* quando @useself è false. Quando il server remoto è un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che non utilizza l'autenticazione di Windows, *rmtuser* è un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso. *rmtuser* è di **tipo sysname** e il valore predefinito è null.  
  
 `[ @rmtpassword = ] 'rmtpassword'`  
 Password associata a *rmtuser*. *rmtpassword* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Quando un utente accede al server locale ed esegue una query distribuita che accede a una tabella del server collegato, il server locale deve accedere al server collegato per parte dell'utente che desidera accedere a tale tabella. Per specificare le credenziali dell'account di accesso utilizzate nel server locale per l'accesso al server collegato, utilizzare la procedura sp_addlinkedsrvlogin.  
  
> [!NOTE]  
>  Per creare piani di query ottimali quando si utilizza una tabella in un server collegato, è necessario che Query Processor ottenga le statistiche di distribuzione dei dati dal server collegato. Gli utenti con autorizzazioni limitate per qualsiasi colonna della tabella potrebbero non disporre delle autorizzazioni sufficienti per ottenere tutte le statistiche utili, nonché ricevere un piano di query meno efficiente e riscontrare un peggioramento delle prestazioni. Se il server collegato è un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], per ottenere tutte le statistiche disponibili, l'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server sysadmin o del ruolo predefinito del database db_owner o db_ddladmin sul server collegato. In SQL Server 2012 SP1 le restrizioni delle autorizzazioni vengono modificate per ottenere le statistiche e gli utenti con l'autorizzazione SELECT possono accedere alle statistiche disponibili tramite DBCC SHOW_STATISTICS. Per ulteriori informazioni, vedere la sezione relativa alle autorizzazioni di [DBCC SHOW_STATISTICS &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
 Un mapping predefinito tra tutti gli account di accesso del server locale e gli account di accesso remoti del server collegato viene creato automaticamente tramite la procedura sp_addlinkedserver. In base al mapping predefinito, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono utilizzate le credenziali dell'account di accesso locale dell'utente durante la connessione al server collegato. Questa operazione equivale a eseguire sp_addlinkedsrvlogin con @useself impostato su **true** per il server collegato, senza specificare un nome utente locale. Utilizzare la procedura sp_addlinkedsrvlogin solo per modificare il mapping predefinito o per aggiungere nuovi mapping per account di accesso locali specifici. Per eliminare il mapping predefinito o qualsiasi altro mapping, utilizzare la procedura sp_droplinkedsrvlogin.  
  
 Anziché utilizzare la procedura sp_addlinkedsrvlogin per creare un mapping predefinito agli account di accesso, per la connessione a un server collegato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può utilizzare automaticamente le credenziali di sicurezza di Windows (nome di accesso e password di Windows) di un utente che invia la query se sussistono le seguenti condizioni:  
  
-   Un utente si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in base all'autenticazione di Windows.  
  
-   È disponibile la delega dell'account di sicurezza nel client e nel server di origine.  
  
-   Il provider supporta l'autenticazione di Windows, ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]eseguito in Windows.  
  
> [!NOTE]  
>  Non è necessario abilitare la delega per scenari a hop singolo ma è necessario abilitarla per gli scenari a più hop.  
  
 Dopo che l'autenticazione è stata eseguita dal server collegato in base ai mapping definiti con la procedura sp_addlinkedsrvlogin eseguita nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le autorizzazioni per singoli oggetti nel database remoto sono determinate dal server collegato, non dal server locale.  
  
 Non è possibile eseguire la procedura sp_addlinkedsrvlogin all'interno di una transazione definita dall'utente.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LOGIN nel server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-connecting-all-local-logins-to-the-linked-server-by-using-their-own-user-credentials"></a>R. Connessione di tutti gli account di accesso locali al server collegato utilizzando le relative credenziali  
 Nell'esempio seguente viene creato un mapping per assicurare che tutti gli account di accesso del server locale si connettano al server collegato `Accounts` utilizzando le proprie credenziali.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts';  
```  
  
 Oppure  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'true';  
```  
  
> [!NOTE]  
>  Se ci sono mapping espliciti creati per account di accesso individuali, essi hanno la precedenza su ogni eventuale mapping globale per quel server collegato.  
  
### <a name="b-connecting-a-specific-login-to-the-linked-server-by-using-different-user-credentials"></a>B. Connessione di un account di accesso specifico al server collegato utilizzando credenziali utente diverse  
 Nell'esempio seguente viene creato un mapping per assicurare che l'utente di Windows `Domain\Mary` si connetta al server collegato `Accounts` tramite l'account `MaryP` e la password `d89q3w4u`.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'false', 'Domain\Mary', 'MaryP', 'd89q3w4u';  
```  
  
> [!IMPORTANT]  
>  In questo esempio non viene utilizzata l'autenticazione di Windows. Le password verranno trasmesse senza essere crittografate. Le password possono essere visibili nelle definizioni delle origini dei dati e negli script salvati su disco, in copie di backup e in file di log. Non utilizzare mai una password di amministratore per questo tipo di connessioni. Per ulteriori informazioni sulla sicurezza specifiche al proprio ambiente, consultare l'amministratore di rete.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo di server collegati &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
