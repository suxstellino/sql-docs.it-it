---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ff7d1cf9be26bd9301d16ae21dcbd6af96f57bc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345551"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|18482|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|REMLOGIN_INVALID_SITE|
|Testo del messaggio|Impossibile connettersi al server '%.ls' perché '%. ls' non è definito come server remoto. Verificare che il nome specificato per il server sia corretto. %.*ls|
||

## <a name="explanation"></a>Spiegazione

Questo errore si verifica quando si tenta di eseguire una chiamata di procedura remota (RPC) da un server a un altro, ad esempio eseguendo una stored procedure in un computer remoto con un'istruzione di tipo `EXEC SERV_REMOTE.pubs..byroyalty`. Viene visualizzato all'utente un messaggio di errore simile al seguente

> Errore 18482: Impossibile connettersi al server \<SERV_REMOTE> perché \<SERV_REMOTE> non è definito come server remoto. Verificare che il nome specificato per il server sia corretto.

## <a name="cause"></a>Causa

Questo errore si verifica quando SQL Server non è in grado di eseguire una chiamata di procedura remota. Il problema può essere causato da un server locale configurato in modo non corretto. Per eseguire una chiamata di procedura remota, SQL Server per prima cosa determina l’identità del server locale cercando il nome del server con srvid = 0 in sysservers. Se in sysservers non viene trovata una voce con srvid = 0 o se il nome del server con srvid = 0 appartiene a un nome di server diverso dal nome del computer Windows locale, verrà visualizzato l'errore.

## <a name="user-action"></a>Azione utente

Per determinare se il server locale è configurato correttamente, esaminare la colonna `srvstatus` in master...sysservers. Questo valore deve essere 0 per il server locale.

Si supponga, ad esempio, che il nome del server locale sia SERV_LOCAL, che il nome del server remoto sia *SERV_REMOTE* e che sysservers contenga le informazioni seguenti:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

Nell'output precedente SERV_LOCAL è il server locale, ma il valore di srvid è 1 e dovrebbe essere 0. Per correggere l’errore, seguire questa procedura:

1. Eseguire `sp_dropserver` local_server_name, droplogins (in questo esempio, si eseguirà `sp_dropserver SERV_LOCAL, droplogins`).
1. Eseguire `sp_addserver` local_server_name, LOCAL (in questo esempio, si eseguirà `sp_addserver SERV_LOCAL, LOCAL`).
1. Arrestare e riavviare SQL Server.

Dopo aver eseguito questi passaggi, la tabella sysservers dovrebbe essere simile alla seguente:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> L’ID server (srvid) deve essere 0 per il server locale.

In alcuni casi è possibile che le voci della tabella sysservers sembrino corrette, ma quando si esegue `select @@servername` viene restituito NULL. In queste situazioni è comunque necessario eseguire i passaggi da 1 a 3 elencati sopra per risolvere il problema.

## <a name="more-information"></a>Ulteriori informazioni

Questo messaggio di errore può essere visualizzato quando si installa la replica perché il processo di installazione effettua chiamate di procedure remote tra i server interessati dalla replica.
