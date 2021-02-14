---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ab9739d17fbe36bb8a703dd06ee85a32e93e7662
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349034"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|15401|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|SEC_INVALIDLOGINORGROUP|
|Testo del messaggio|Impossibile trovare l'utente o il gruppo di Windows NT '%s'. Controllare di nuovo il nome.|
||

## <a name="explanation"></a>Spiegazione

Questo errore si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di creare un account di accesso basato su un'entità di Windows, ad esempio un utente di dominio o un gruppo di domini Windows. Viene visualizzato all'utente un messaggio di errore simile al seguente

> Errore 15401: Impossibile trovare l'utente o il gruppo di Windows NT '%s'. Controllare di nuovo il nome.

## <a name="cause"></a>Causa

Le possibili cause di questo errore sono le seguenti:

- L'account di accesso non esiste in Active Directory.
- Il controller di dominio non è disponibile.
- Non si usa BUILTIN come nome di dominio quando si aggiunge un account locale.
- Problemi di risoluzione del nome.

## <a name="user-action"></a>Azione utente

Esaminare le sezioni seguenti per le azioni che è possibile eseguire per ognuna delle diverse cause indicate in precedenza.

### <a name="verify-the-login-you-are-trying-to-add"></a>Verificare l'account di accesso che si sta tentando di aggiungere

1. Verificare che l'account di accesso di Windows esista ancora nel dominio. L'amministratore di rete potrebbe avere rimosso l'account di accesso di Windows per motivi specifici e potrebbe non essere possibile concedere all'account l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
1. Verificare di aver digitato correttamente il nome del dominio e dell'account di accesso e di aver usato il formato seguente: `Domain\User`
1. Se l'account di accesso esiste ed è corretto e viene comunque visualizzato l'errore, continuare con le sezioni seguenti di questo articolo.

### <a name="verify-if-the-domain-controller-is-available"></a>Verificare che il controller di dominio sia disponibile

È possibile che venga visualizzato l'errore 15401 quando il controller di dominio per il dominio in cui risiede l'account di accesso (lo stesso dominio o un dominio diverso) non è disponibile per qualche motivo.

Se l'account di accesso si trova in un dominio diverso da quello di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], verificare che esistano i trust corretti tra i domini.

Verificare che il controller di dominio dell'account di accesso sia accessibile tramite il comando ping del computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Controllare sia l'indirizzo IP che il nome del controller di dominio.

### <a name="verify-the-domain-name-for-local-accounts"></a>Verificare il nome di dominio per gli account locali

Gli account locali (non di dominio) richiedono una gestione speciale. Se si sta tentando di aggiungere un account locale dal computer locale che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], assicurarsi di usare BUILTIN come nome di dominio.

### <a name="check-for-name-resolution-issues"></a>Cercare problemi di risoluzione del nome

Se si verificano problemi di risoluzione del nome di un computer usato per l'aggiunta dell'account di accesso o del gruppo, è possibile che venga visualizzato l'errore 15401.

Verificare che il meccanismo di risoluzione dei nomi (ad esempio, WINS, DNS, HOSTS o LMHOSTS) sia configurato correttamente.

## <a name="see-also"></a>Vedi anche

- [Testare un canale tra il computer locale e il relativo dominio](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](../system-stored-procedures/sp-change-users-login-transact-sql.md)