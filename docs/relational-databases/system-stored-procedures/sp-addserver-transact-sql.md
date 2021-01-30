---
description: sp_addserver (Transact-SQL)
title: sp_addserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addserver
- sp_addserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addserver
- renaming servers
- machine names [SQL Server]
- computer names
ms.assetid: 160a6b29-5e80-44ab-80ec-77d4280f627c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fabe72f253eda808131d876fd180ddb2f7a67ad1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207096"
---
# <a name="sp_addserver-transact-sql"></a>sp_addserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Definisce il nome dell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando il computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita viene rinominato, utilizzare **sp_addserver** per informare l'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] del nuovo nome del computer. Questa routine deve essere eseguita in tutte le istanze del [!INCLUDE[ssDE](../../includes/ssde-md.md)] ospitate nel computer. Non è possibile modificare il nome dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Per modificare il nome di un'istanza denominata, installare una nuova istanza con il nome desiderato, scollegare i file di database dall'istanza precedente, collegare i database alla nuova istanza ed eliminare l'istanza precedente. In alternativa, è possibile creare un nome alias del client nel computer client, reindirizzando la connessione a una combinazione diversa di server e nome di istanza o **server:port** senza modificare il nome dell'istanza nel computer server.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```

sp_addserver [ @server = ] 'server' ,
     [ @local = ] 'local' 
     [ , [ @duplicate_ok = ] 'duplicate_OK' ]
```

## <a name="arguments"></a>Argomenti
`[ @server = ] 'server'` Nome del server. I nomi di server devono essere univoci e conformi alle regole per i nomi di computer di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Gli spazi non sono consentiti. *server* è di tipo **sysname** e non prevede alcun valore predefinito.

 Se in un computer sono installate più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un'istanza funziona come se si trovasse in un server distinto. Specificare un'istanza denominata facendo riferimento al *Server* come *nomeserver\nomeistanza*.

`[ @local = ] 'LOCAL'` Specifica che il server viene aggiunto come server locale. **\@ local** è di tipo **varchar (10)** e il valore predefinito è null. Se si specifica **\@ local** come **local** , il **\@ Server** viene definito come nome del server locale e la @SERVERNAME funzione @ restituisce il valore del *Server*.

 Durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa variabile viene impostata sul nome del computer. Per impostazione predefinita, il nome del computer consente agli utenti di connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza ulteriori operazioni di configurazione.

 La definizione locale diventa effettiva solo dopo il riavvio del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . È possibile definire un solo server locale in ogni istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].

`[ @duplicate_ok = ] 'duplicate_OK'` Specifica se è consentito un nome di server duplicato. **\@ duplicate_OK** è di tipo **varchar (13)** e il valore predefinito è null. **\@ duplicate_OK** può avere solo il valore **duplicate_OK** o null. Se viene specificato **duplicate_OK** e il nome del server da aggiungere esiste già, non viene generato alcun errore. Se i parametri denominati non vengono usati, è necessario specificare **\@ local** .

## <a name="return-code-values"></a>Valori del codice restituito
 0 (operazione completata) o 1 (operazione non riuscita)

## <a name="remarks"></a>Commenti
 Per impostare o deselezionare le opzioni del server, utilizzare **sp_serveroption**.

 Impossibile utilizzare **sp_addserver** all'interno di una transazione definita dall'utente.

 L'utilizzo di **sp_addserver** per l'aggiunta di un server remoto non è più disponibile.  Usare [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) in alternativa.

## <a name="permissions"></a>Autorizzazioni
 È richiesta l'appartenenza al ruolo predefinito del server **setupadmin** .

## <a name="examples"></a>Esempio
 Nell'esempio seguente la voce del [!INCLUDE[ssDE](../../includes/ssde-md.md)] per il nome del computer che ospita [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene modificata in `ACCOUNTS`.

```
sp_addserver 'ACCOUNTS', 'local';
```

## <a name="see-also"></a>Vedere anche
 [Rinominare un computer che ospita un'istanza Stand-Alone di SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) [sp_addlinkedserver &#40;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)&#41;SP_DROPSERVER &#40;[Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md) Sp_helpserver &#40;Transact [-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) [stored procedure di sistema &#40;Transact-SQL ](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)&#41;[stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)


