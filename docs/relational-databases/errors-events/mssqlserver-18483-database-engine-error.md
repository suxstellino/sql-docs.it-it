---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 40a75d8ad0bd6237b594f38bbb5cb83be8cca788
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797827"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|18483|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|REMLOGIN_INVALID_USER|
|Testo del messaggio|Impossibile connettersi al server '%.ls' perché '%. ls' non è definito come account di accesso remoto nel server. Verificare che il nome account di accesso specificato sia corretto. %.*ls.|
||

## <a name="explanation"></a>Spiegazione

Questo errore si verifica quando si tenta di configurare un server di distribuzione repliche in un sistema che è stato ripristinato usando l'immagine del disco rigido di un altro computer in cui è stata installata in origine l'istanza di SQL. Viene visualizzato all'utente un messaggio di errore simile al seguente:

> SQL Server Management Studio non è in grado di configurare '\<Server>\<Instance>' come server di distribuzione per '\<Server>\<Instance>' . Errore 18483: Non è stato possibile connettersi al server ‘\<Server>\<Instance>’ perché ‘distributor_admin’ non è definito come account di accesso remoto nel server. Verificare che il nome account di accesso specificato sia corretto. %.*ls.

## <a name="cause"></a>Causa

Quando si distribuisce [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dall’immagine del disco rigido di un altro computer in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il nome di rete del computer con immagine viene mantenuto nella nuova installazione. Un nome di rete non corretto causa l’esito negativo della configurazione del server di distribuzione repliche. Lo stesso problema si verifica se si rinomina il computer dopo l’installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="user-action"></a>Azione utente

Per ovviare a questo problema, sostituire il nome del server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il nome di rete corretto del computer. A questo scopo, attenersi alla procedura seguente:

1. Accedere al computer in cui è stato distribuito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dall'immagine del disco, quindi eseguire la seguente istruzione Transact-SQL in SSMS:

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. Riavviare il computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Per verificare che il nome del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il nome di rete del computer siano uguali, eseguire la seguente istruzione Transact-SQL:

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>Ulteriori informazioni

È possibile usare la variabile globale `@@SERVERNAME` o la funzione `SERVERPROPERTY`(' ServerName') in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per trovare il nome di rete del computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La proprietà ServerName della funzione `SERVERPROPERTY` segnala automaticamente la modifica apportata al nome di rete del computer quando si riavvia il computer e il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La variabile globale `@@SERVERNAME` mantiene il nome originale del computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fino a quando il nome del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene reimpostato manualmente.

### <a name="steps-to-reproduce-the-problem"></a>Passaggi per riprodurre il problema

Nel computer in cui è stato distribuito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un'immagine del disco seguire questa procedura:

1. Avviare [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].
2. In **Esplora oggetti** espandere il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Fare clic con il pulsante destro del mouse sulla cartella **Replica**, selezionare la **configurazione della replica della distribuzione** e quindi la **configurazione di pubblicazione, sottoscrittori e distribuzione**.
4. Nella finestra di dialogo della **configurazione guidata della distribuzione** fare clic su **Avanti**.
5. Nella finestra di dialogo **Server di distribuzione** selezionare '\<**Server**>\<**Instance**>' fungerà da server di distribuzione per se stesso. Verranno creati un database di distribuzione e un pulsante di opzione Log, quindi fare clic su **Avanti**.
6. Nella finestra di dialogo **Avvio SQL Server Agent** fare clic su **Avanti**.
7. Nella finestra di dialogo **Cartella snapshot** fare clic su **Avanti**.

    > [!NOTE]
    > Se viene visualizzato un messaggio che chiede di confermare il percorso della cartella snapshot, fare clic su **Sì**.
8. Nella finestra di dialogo **Database di distribuzione** fare clic su **Avanti**.
9. Nella finestra di dialogo **Server di pubblicazione** fare clic su **Avanti**.
10. Nella finestra di dialogo **Azioni procedura guidata** fare clic su **Avanti**.
11. Nella finestra di dialogo **Completare la procedura guidata** fare clic su **Fine**.

## <a name="see-also"></a>Vedi anche

- [Rinominare un computer che ospita un'istanza autonoma di SQL Server](/sql/database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server)
- [@@SERVERNAME (Transact-SQL)](/sql/t-sql/functions/servername-transact-sql)
- [SERVERPROPERTY (Transact-SQL)](/sql/t-sql/functions/serverproperty-transact-sql)
- [sp_addserver (Transact-SQL)](/sql/relational-databases/system-stored-procedures/sp-addserver-transact-sql)
