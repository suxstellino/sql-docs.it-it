---
title: Configurare le regole del firewall prima di eseguire il debugger di T-SQL
description: Informazioni su come configurare le regole di Windows Firewall prima di eseguire il debugger Transact-SQL (T-SQL) quando si è connessi a un'istanza di SQL Server in SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.error.sqlde_accessdenied
- vs.debug.error.sqlde_register_failed
- vs.debug.error.sqlde_firewall.remotemachines
helpviewer_keywords:
- Transact-SQL debugger, remote connections
- Windows Firewall [Database Engine], Transact-SQL debugger
- Transact-SQL debugger, Windows Firewall
- Transact-SQL debugger, configuring
- ports [SQL Server], Transact-SQL debugger
- TCP/IP [SQL Server], port numbers
ms.assetid: f50e0b0d-eaf0-4f4a-be83-96f5be63e7ea
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 10/20/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a55de0f9f6a6b8ae80161adaa0252b3d5c4a774
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354697"
---
# <a name="configure-firewall-rules-before-running-the-tsql-debugger"></a>Configurare le regole del firewall prima di eseguire il debugger TSQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

È necessario configurare regole di Windows Firewall per abilitare il debug di [!INCLUDE[tsql](../../includes/tsql-md.md)] durante la connessione a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] eseguita in un computer diverso da Editor di query [!INCLUDE[ssDE](../../includes/ssde-md.md)] .

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="configuring-the-transact-sql-debugger"></a>Configurazione del debugger Transact-SQL

Il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] include componenti sia sul lato server sia sul lato client. I componenti del debugger lato server vengono installati con ogni istanza del motore di database da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) o versione successiva. Sono inclusi i componenti del debugger lato client:

- In caso di installazione degli strumenti lato client da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versione successiva.

- In caso di installazione di Microsoft Visual Studio 2010 o versione successiva.

- In caso di installazione di [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] dal download Web.

Non esistono requisiti di configurazione per eseguire il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] se [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o or [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] viene eseguito nello stesso computer dell'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Per eseguire il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] quando si è connessi a un'istanza remota del [!INCLUDE[ssDE](../../includes/ssde-md.md)], è tuttavia necessario abilitare regole relative a programmi e porte in Windows Firewall in entrambi i computer. Tali regole possono essere create dall'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si verificano errori mentre si tenta di aprire una sessione di debug remoto, assicurarsi che nel computer siano definite le regole firewall indicate di seguito.

Usare l'applicazione **Windows Firewall con protezione avanzata** per gestire le regole firewall. In [!INCLUDE[win7](../../includes/win7-md.md)] e [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]aprire **Pannello di controllo**, fare clic su **Windows Firewall** e scegliere **Impostazioni avanzate**. In [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] è anche possibile aprire **Service Manager**, espandere **Configurazione** nel riquadro sinistro e quindi **Windows Firewall con protezione avanzata**.

> [!CAUTION]
>  È possibile che l'abilitazione di regole in Windows Firewall esponga il computer a rischi per la sicurezza, per bloccare i quali è progettato il firewall. L'abilitazione di regole per il debug remoto determina lo sblocco delle porte e dei programmi elencati in questo argomento.

## <a name="firewall-rules-on-the-server"></a>Regole firewall nel server
Nel computer in cui è in esecuzione l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]usare **Windows Firewall con protezione avanzata** per specificare le seguenti informazioni:

- Aggiungere una regola di programma in entrata per sqlservr.exe. È necessario disporre di una regola per ogni istanza che richiede il supporto di sessioni di debug remoto.

   1. Nel riquadro sinistro di **Windows Firewall con protezione avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.

   2. Nella finestra di dialogo **Tipo di regola** selezionare **Programma**, quindi fare clic su **Avanti**.

   3. Nella finestra di dialogo **Programma** selezionare **Percorso programma** e immettere il percorso completo di sqlservr.exe per questa istanza. Per impostazione predefinita, il programma sqlservr.exe viene installato nel percorso C:\Programmi\Microsoft SQL Server\MSSQL13.*NomeIstanza*\MSSQL\Binn, dove *NomeIstanza* è MSSQLSERVER per l'istanza predefinita e il nome dell'istanza per qualsiasi istanza denominata.

   4. Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.

   5. Nella finestra di dialogo **Profilo** selezionare qualsiasi profilo che descrive l'ambiente di connessione del computer quando si vuole aprire una sessione di debug con l'istanza e scegliere **Avanti**.

   6. Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.

   7. Nell'elenco **Regole in entrata** fare clic con il pulsante destro del mouse sulla regola creata e quindi selezionare **Proprietà** nel riquadro azioni.

   8. Selezionare la scheda **Protocolli e porte** .

   9. Selezionare **TCP** nel riquadro **Tipo di protocollo** , **Porte dinamiche RPC** nel riquadro **Porta locale** , quindi fare clic su **Applica** e **OK**.

- Aggiungere una regola di programma in entrata per svchost.exe per abilitare le comunicazioni DCOM dalle sessioni di debug remoto.

   1. Nel riquadro sinistro di **Windows Firewall con protezione avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.

   2. Nella finestra di dialogo **Tipo di regola** selezionare **Programma**, quindi fare clic su **Avanti**.

   3. Nella finestra di dialogo **Programma** selezionare **Percorso programma** e immettere il percorso completo di svchost.exe. Per impostazione predefinita, svchost.exe è installato in %systemroot%\System32\svchost.exe.

   4. Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.

   5. Nella finestra di dialogo **Profilo** selezionare qualsiasi profilo che descrive l'ambiente di connessione del computer quando si vuole aprire una sessione di debug con l'istanza e scegliere **Avanti**.

   6. Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.

   7. Nell'elenco **Regole in entrata** fare clic con il pulsante destro del mouse sulla regola creata e quindi selezionare **Proprietà** nel riquadro azioni.

   8. Selezionare la scheda **Protocolli e porte** .

   9. Selezionare **TCP** nel riquadro **Tipo di protocollo** , **Agente mapping endpoint RPC** nel riquadro **Porta locale** , quindi fare clic su **Applica** e **OK**.

- Se i criteri di dominio richiedono che le comunicazioni di rete vengano eseguite tramite IPSec, è necessario aggiungere anche regole in entrata per l'apertura delle porte UDP 4500 e UDP 500.

## <a name="firewall-rules-on-the-client"></a>Regole firewall nel client

Nel computer in cui è in esecuzione l'editor di query [!INCLUDE[ssDE](../../includes/ssde-md.md)] , con l'installazione di SQL Server o di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] potrebbe essere stato configurato Windows Firewall per consentire il debug remoto.

Se si verificano errori nel tentativo di aprire una sessione di debug remoto, è possibile configurare manualmente le eccezioni relative a programmi e porte usando **Windows Firewall con protezione avanzata** per configurare regole firewall:

- Aggiungere una voce di programma per svchost:

   1. Nel riquadro sinistro di **Windows Firewall con protezione avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.

   2. Nella finestra di dialogo **Tipo di regola** selezionare **Programma**, quindi fare clic su **Avanti**.

   3. Nella finestra di dialogo **Programma** selezionare **Percorso programma** e immettere il percorso completo di svchost.exe. Per impostazione predefinita, svchost.exe è installato in %systemroot%\System32\svchost.exe.

   4. Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.

   5. Nella finestra di dialogo **Profilo** selezionare qualsiasi profilo che descrive l'ambiente di connessione del computer quando si vuole aprire una sessione di debug con l'istanza e scegliere **Avanti**.

   6. Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.

   7. Nell'elenco **Regole in entrata** fare clic con il pulsante destro del mouse sulla regola creata e quindi selezionare **Proprietà** nel riquadro azioni.

   8. Selezionare la scheda **Protocolli e porte** .

   9. Selezionare **TCP** nel riquadro **Tipo di protocollo** , **Agente mapping endpoint RPC** nel riquadro **Porta locale** , quindi fare clic su **Applica** e **OK**.

- Aggiungere una voce di programma per l'applicazione che ospita l'editor di query [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Se è necessario aprire le sessioni di debug remoto da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] nello stesso computer, è necessario aggiungere una regola di programma per entrambi:

   1. Nel riquadro sinistro di **Windows Firewall con protezione avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.

   2. Nella finestra di dialogo **Tipo di regola** selezionare **Programma**, quindi fare clic su **Avanti**.

   3. Nella finestra di dialogo **Programma** selezionare **Percorso programma** e immettere uno di questi tre valori.

       - Per [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]immettere il percorso completo di ssms.exe. Per impostazione predefinita, ssms.exe viene installato nel percorso C:\Programmi (x86)\Microsoft SQL Server\130\Tools\Binn\Management Studio.

       - Per [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] immettere il percorso completo di devenv.exe:

            1. Per impostazione predefinita, devenv.exe per Visual Studio 2010 si trova in C:\Programmi (x86)\Microsoft Visual Studio 10.0\Common7\IDE.

            2. Per impostazione predefinita, devenv.exe per Visual Studio 2012 si trova in C:\Programmi (x86)\Microsoft Visual Studio 11.0\Common7\IDE

            3. È possibile trovare il percorso di ssms.exe dal collegamento utilizzato per avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È possibile trovare il percorso di devenv.exe dal collegamento utilizzato per avviare [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Fare clic con il pulsante destro del mouse sul collegamento, quindi scegliere **Proprietà**. Il file eseguibile e il percorso vengono elencati nel riquadro **Destinazione** .

   4. Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.

   5. Nella finestra di dialogo **Profilo** selezionare qualsiasi profilo che descrive l'ambiente di connessione del computer quando si vuole aprire una sessione di debug con l'istanza e scegliere **Avanti**.

   6. Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.

   7. Nell'elenco **Regole in entrata** fare clic con il pulsante destro del mouse sulla regola creata e quindi selezionare **Proprietà** nel riquadro azioni.

   8. Selezionare la scheda **Protocolli e porte** .

   9. Selezionare **TCP** nel riquadro **Tipo di protocollo** , **Porte dinamiche RPC** nel riquadro **Porta locale** , quindi fare clic su **Applica** e **OK**.

## <a name="requirements-for-starting-the-debugger"></a>Requisiti per l'avvio del debugger

Qualsiasi tentativo di avviare il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] deve soddisfare anche i requisiti seguenti:

- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] deve essere eseguito con un account di Windows membro del ruolo predefinito del server sysadmin.

- La finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve essere connessa tramite un account di accesso con autenticazione di Windows o con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che sia membro del ruolo predefinito del server sysadmin.

- La finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] deve essere connessa a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) o versione successiva. Non è possibile eseguire il debugger quando la finestra dell'editor di query è connessa a un'istanza che è in modalità utente singolo.

- Il server deve ricomunicare con il client tramite RPC. L'account su cui è in esecuzione il servizio SQL Server deve avere le autorizzazioni autenticate per il client.

## <a name="see-also"></a>Vedere anche

- [Debugger Transact-SQL](./transact-sql-debugger.md)
- [Eseguire il debugger Transact-SQL](./run-the-transact-sql-debugger.md)
- [Eseguire istruzione per istruzione il codice Transact-SQL](./step-through-transact-sql-code.md)
- [Informazioni del debugger Transact-SQL](./transact-sql-debugger-information.md)
- [Editor di query del Motore di database &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)