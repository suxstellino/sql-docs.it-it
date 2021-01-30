---
title: MSSQLSERVER_18456 | Microsoft Docs
description: Tentativo di connessione rifiutato a causa di un errore con password o nome utente non validi in SQL Server. Vedere la descrizione dell'errore e delle possibili soluzioni.
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18456 (Database Engine error)
ms.assetid: c417631d-be1f-42e0-8844-9f92c77e11f7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c1e2f5aaf5198530b5d7c93ecffb088e7145b9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196522"
---
# <a name="mssqlserver_18456"></a>MSSQLSERVER_18456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|18456|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LOGON_FAILED|  
|Testo del messaggio|Accesso non riuscito per l'utente '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Spiegazione  
Quando un tentativo di connessione viene rifiutato a causa di un errore di autenticazione dovuto a una password o un nome utente errato, al client viene restituito un messaggio simile al seguente:  "Accesso non riuscito per l'utente '<nome_utente>' (Microsoft SQL Server, Errore: 18456)".  
  
Al client vengono restituite informazioni aggiuntive tra cui le seguenti:  
  
"Accesso non riuscito per l'utente '<nome_utente>' (provider di dati SqlClient .Net)"  
  
-----------------------------\-  
  
"Nome server: <nome_computer>"  
  
"Numero errore: 18456"  
  
"Gravità: 14"  
  
"Stato: 1"  
  
"Numero riga: 65536"  
  
Può essere inoltre restituito il messaggio seguente:  
  
"Messaggio 18456, livello 14, stato 1, server <nome_computer>, riga 1"  
  
"Accesso non riuscito per l'utente '<nome_utente>'".  
  
## <a name="additional-error-information"></a>Informazioni aggiuntive sull'errore  
Per aumentare il livello di sicurezza, il messaggio di errore restituito al client nasconde deliberatamente la natura dell'errore di autenticazione. Tuttavia, nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per ogni errore viene indicato lo stato corrispondente tramite cui viene eseguito il mapping alla condizione di errore di autenticazione. Confrontare lo stato di errore restituito con l'elenco seguente per determinare il motivo dell'errore di accesso.  
  
|State|Descrizione|  
|---------|---------------|  
|1|Non sono disponibili informazioni sull'errore. Questo stato in genere indica che non si dispone dell'autorizzazione a ricevere i dettagli dell'errore. Per ulteriori informazioni, contattare l'amministratore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|2|L'ID utente non è valido.|  
|5|L'ID utente non è valido.|  
|6|Si è tentato di utilizzare un account di accesso di Windows con l'autenticazione di SQL Server.|  
|7|L'account di accesso è disabilitato e la password non è corretta.|  
|8|La password non è corretta.|  
|9|La password non è valida.|  
|11|L'account di accesso è valido, ma l'accesso al server ha avuto esito negativo. Una possibile causa di questo errore è quando l'utente di Windows dispone dell'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come un membro del gruppo Administrators locale, ma Windows non fornisce le credenziali di amministratore. Per connettersi, avviare il programma di connessione usando l'opzione **Esegui come amministratore** e quindi aggiungere l'utente di Windows a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come accesso specifico.|  
|12|L'account di accesso è valido, ma l'accesso al server ha avuto esito negativo.|  
|18|È necessario modificare la password.|  
|38, 46|Impossibile trovare il database richiesto dall'utente.|
|58| Quando SQL Server è impostato per usare solo l'autenticazione di Windows e un client prova ad accedere usando l'autenticazione di SQL. Un'altra causa si ha quando gli ID di sicurezza non corrispondono.|
|102 - 111|Errore di AAD.|
|122 - 124|Errore dovuto al campo di nome utente o password vuoto.|
|126|Il database richiesto dall'utente non esiste.|
|132 - 133|Errore di AAD.|
  
Esistono altri stati di errore che indicano un errore di elaborazione interno non previsto.  
  
**Possibile causa insolita aggiuntiva**  
  
Il motivo dell'errore **Tentativo di accesso tramite l'autenticazione di SQL Server non riuscito. Il server è configurato solo per l'autenticazione di Windows.** può essere restituito nelle situazioni seguenti.  
  
-   Quando il server è configurato per l'autenticazione in modalità mista, in una connessione ODBC viene utilizzato il protocollo TCP e tramite la connessione non viene specificato in modo esplicito che in essa deve essere utilizzata una connessione trusted.  
  
-   Quando il server è configurato per l'autenticazione in modalità mista, in una connessione ODBC vengono utilizzate named pipe, le credenziali utilizzate dal client per aprire la named pipe vengono utilizzate per rappresentare automaticamente l'utente e tramite la connessione non viene specificato in modo esplicito che in essa deve essere utilizzata una connessione trusted.  
  
Per risolvere questo problema, includere **TRUSTED_CONNECTION = TRUE** nella stringa di connessione.  
  
## <a name="examples"></a>Esempi  
In questo esempio, lo stato dell'errore di autenticazione è 8 e indica che la password non è corretta.  
  
|Data|Source (Sorgente)|Message|  
|--------|----------|-----------|  
|2007-12-05 20:12:56.34|Accesso|Errore: 18456, Gravità: 14, Stato: 8.|  
|2007-12-05 20:12:56.34|Accesso|Accesso non riuscito per l'utente '<nome_utente>'. [CLIENT: <ip address>]|  
  
> [!NOTE]  
> Se durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si usa la modalità Autenticazione di Windows e successivamente si passa alla modalità Autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e di Windows, l'account di accesso **sa** verrà inizialmente disabilitato. Ciò determina l'errore di stato 7: "Accesso non riuscito per l'utente 'sa'". Per abilitare l'account di accesso **sa**, vedere [Modifica della modalità di autenticazione del server](~/database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="user-action"></a>Azione dell'utente  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], verificare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia configurato in modalità Autenticazione mista. Per altre informazioni sulla modifica della modalità di autenticazione di SQL Server, rivedere [Modifica della modalità di autenticazione del server](../../database-engine/configure-windows/change-server-authentication-mode.md).
  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che l'account di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esista e che non contenga errori di ortografia.  
  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di Windows, verificare di essere connessi al dominio corretto.  
  
Se l'errore indica lo stato 1, contattare l'amministratore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Se si tenta di eseguire la connessione usando le credenziali di amministratore, avviare l'applicazione usando l'opzione **Esegui come amministratore**. Dopo aver stabilito la connessione, aggiungere l'utente di Windows come singolo account di accesso.  
  
Se il [!INCLUDE[ssDE](../../includes/ssde-md.md)] supporta i database indipendenti, verificare che l'account di accesso non sia stato eliminato dopo la migrazione a un utente del database indipendente.  
  
Quando ci si connette in locale a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le connessioni da servizi in esecuzione con **NT AUTHORITY\NETWORK SERVICE** devono essere autenticate usando computer con nomi di dominio completi. Per altre informazioni, vedere [Procedura: Usare l'account del servizio di rete per accedere alle risorse in ASP.NET](/previous-versions/msp-n-p/ff647402(v=pandp.10))