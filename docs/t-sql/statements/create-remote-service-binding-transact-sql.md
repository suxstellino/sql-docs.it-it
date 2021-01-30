---
description: CREATE REMOTE SERVICE BINDING (Transact-SQL)
title: CREATE REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE REMOTE SERVICE BINDING
- SERVICE_BINDING_TSQL
- CREATE REMOTE SERVICE
- REMOTE_TSQL
- CREATE_REMOTE_SERVICE_BINDING_TSQL
- CREATE_REMOTE_SERVICE_TSQL
- BINDING
- SERVICE BINDING
- BINDING_TSQL
- CREATE_REMOTE_TSQL
- REMOTE_SERVICE_TSQL
- CREATE REMOTE
- REMOTE SERVICE
- REMOTE_SERVICE_BINDING_TSQL
- REMOTE
- REMOTE SERVICE BINDING
dev_langs:
- TSQL
helpviewer_keywords:
- binding remote service [Service Broker]
- dialog security [Service Broker]
- end-to-end security [Service Broker]
- security [Service Broker], remote service bindings
- CREATE REMOTE SERVICE BINDING statement
- conversation security [Service Broker]
- remote service bindings [Service Broker], creating
ms.assetid: 4165c404-4d50-4063-9a6e-6e267d309376
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13abf5a16b800bc856d482fbc525a833b9d7e872
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188522"
---
# <a name="create-remote-service-binding-transact-sql"></a>CREATE REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un'associazione che definisce le credenziali di sicurezza da utilizzare per avviare una conversazione con un servizio remoto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE REMOTE SERVICE BINDING binding_name   
   [ AUTHORIZATION owner_name ]   
   TO SERVICE 'service_name'   
   WITH  USER = user_name [ , ANONYMOUS = { ON | OFF } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *binding_name*  
 Nome dell'associazione al servizio remoto da creare. Non è possibile specificare i nomi del server, del database e dello schema. *binding_name* deve essere un valore **sysname** valido.  
  
 AUTHORIZATION *owner_name*  
 Imposta come proprietario dell'associazione l'utente o il ruolo del database specificato. Se l'utente corrente è **dbo** o **sa**, *owner_name* può essere il nome di qualsiasi utente o ruolo valido. In caso contrario, *owner_name* deve essere il nome dell'utente corrente, il nome di un utente per il quale l'utente corrente dispone di autorizzazioni IMPERSONATE oppure il nome di un ruolo a cui appartiene l'utente corrente.  
  
 TO SERVICE '*service_name*'  
 Specifica il servizio remoto da associare all'utente identificato nella clausola WITH USER.  
  
 USER = *user_name*  
 Specifica l'entità database proprietaria del certificato associato al servizio remoto identificato dalla clausola TO SERVICE. Il certificato viene utilizzato per la crittografia e l'autenticazione dei messaggi scambiati con il servizio remoto.  
  
 ANONYMOUS  
 Specifica se viene utilizzato l'accesso anonimo durante la comunicazione con il servizio remoto. Se ANONYMOUS = ON viene usata l'autenticazione anonima e le operazioni nel database remoto vengono eseguite con un account membro del ruolo predefinito del database **public**. Se ANONYMOUS = OFF, le operazioni nel database remoto vengono eseguite con l'account di un utente specifico di tale database. Se questa clausola viene omessa, il valore predefinito è OFF.  
  
## <a name="remarks"></a>Osservazioni  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilizza un'associazione al servizio remoto per individuare il certificato da utilizzare per una nuova conversazione. La chiave pubblica nel certificato associato a *user_name* viene usata per autenticare i messaggi inviati al servizio remoto e crittografare una chiave di sessione che verrà quindi usata per crittografare la conversazione. Il certificato per *user_name* deve corrispondere al certificato per un utente nel database che ospita il servizio remoto.  
  
 Un'associazione al servizio remoto è necessaria solo per l'avvio dei servizi che comunicano con servizi di destinazione che si trovano all'esterno dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un database che ospita un servizio di origine deve contenere associazioni ai servizi remoti per tutti i servizi di destinazione esterni all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non è necessario che un database che ospita un servizio di destinazione contenga associazioni a servizi remoti per i servizi in fase di inizializzazione che comunicano con il servizio di destinazione. Quando i servizi di origine e di destinazione si trovano nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], non è necessaria alcuna associazione al servizio remoto. Se tuttavia è presente un'associazione al servizio remoto dove il valore di *user_name* specificato per TO SERVICE corrisponde al nome del servizio locale, in [!INCLUDE[ssSB](../../includes/sssb-md.md)] viene usata l'associazione.  
  
 Se ANONYMOUS = ON, il servizio in fase di inizializzazione si connette al servizio di destinazione come membro del ruolo predefinito del database **public**. Per impostazione predefinita, i membri di questo ruolo non hanno l'autorizzazione per la connessione a un database. Per poter inviare un messaggio, il database di destinazione deve concedere al ruolo **public** l'autorizzazione CONNECT per il database e l'autorizzazione SEND per il servizio di destinazione.  
  
 Se un utente possiede più di un certificato, [!INCLUDE[ssSB](../../includes/sssb-md.md)] seleziona il certificato con la data di scadenza più recente tra i certificati validi e contrassegnati con AVAILABLE FOR BEGIN_DIALOG.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per la creazione di un'associazione al servizio remoto vengono assegnate per impostazione predefinita all'utente indicato nella clausola USER, ai membri del ruolo predefinito del database **db_owner**, ai membri del ruolo predefinito del database **db_ddladmin** e ai membri del ruolo predefinito del server **sysadmin**.  
  
 L'utente che esegue l'istruzione CREATE REMOTE SERVICE BINDING deve disporre dell'autorizzazione di rappresentazione per l'entità specificata nell'istruzione.  
  
 Un'associazione al servizio remoto non può essere un oggetto temporaneo. I nomi delle associazioni ai servizi remoti che iniziano con **#** sono consentite, ma sono oggetti permanenti.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-remote-service-binding"></a>R. Creazione di un'associazione al servizio remoto  
 Nell'esempio seguente viene creata un'associazione per il servizio `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilizza il certificato di proprietà dell'entità database `APUser` per l'autenticazione per il servizio remoto e per scambiare la chiave di crittografia della sessione con il servizio remoto.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser ;  
```  
  
### <a name="b-creating-a-remote-service-binding-using-anonymous-authentication"></a>B. Creazione di un'associazione al servizio remoto utilizzando l'autenticazione anonima  
 Nell'esempio seguente viene creata un'associazione per il servizio `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilizza il certificato di proprietà dell'entità database `APUser` per scambiare la chiave di crittografia della sessione con il servizio remoto. Service Broker non esegue l'autenticazione per il servizio remoto. Nel database che ospita il servizio remoto i messaggi vengono recapitati come utente **guest**.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser, ANONYMOUS=ON ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
