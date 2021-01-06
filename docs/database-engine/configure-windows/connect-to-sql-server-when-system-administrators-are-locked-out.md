---
title: Connettersi a SQL Server se gli amministratori di sistema sono bloccati | Microsoft Docs
description: Informazioni su come riottenere l'accesso a SQL Server come amministratore di sistema in caso di blocco per errore.
ms.custom: contperf-fy20q4
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b4cf5946fba4eb3c79150b29ef44bd232f9c155
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878948"
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>Connettersi a SQL Server se gli amministratori di sistema sono bloccati 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
Questo articolo descrive come è possibile riottenere l'accesso al [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] come amministratore di sistema in caso di blocco.  Un amministratore di sistema può perdere l'accesso a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per uno dei motivi seguenti:  
  
-   Tutti gli account di accesso membri del ruolo predefinito del server sysadmin sono stati rimossi per errore.  
  
-   Tutti i gruppi di Windows membri del ruolo predefinito del server sysadmin sono stati rimossi per errore.  
  
-   Gli account di accesso membri del ruolo predefinito del server sysadmin sono assegnati a utenti che hanno lasciato la società o che non sono disponibili.  
  
-   L'account sa è disabilitato o la password non è nota ad alcun utente.  
  
## <a name="resolution"></a>Risoluzione

Per risolvere il problema di accesso, è consigliabile avviare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente singolo. Questa modalità impedisce che si verifichino altre connessioni mentre si tenta di riottenere l'accesso. Da questa modalità è possibile connettersi all'istanza di SQL Server e aggiungere il proprio account di accesso al ruolo del server **sysadmin**. La procedura dettagliata per questa soluzione è disponibile nella sezione delle [istruzioni dettagliate](#step-by-step-instructions).


È possibile avviare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente singolo con le opzioni `-m` o `-f` dalla riga di comando. Qualsiasi membro del gruppo Administrators locale del computer può quindi connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come membro del ruolo predefinito del server **sysadmin**.  

Quando si avvia l'istanza in modalità utente singolo, arrestare innanzitutto il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. In caso contrario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent potrebbe connettersi per primo, acquisendo l'unica connessione disponibile al server e impedendo quindi ulteriori accessi.

È anche possibile che un'applicazione client sconosciuta usi l'unica connessione disponibile prima di poter eseguire l'accesso. Per evitare che ciò accada, è possibile usare l'opzione `-m` seguita da un nome di applicazione per limitare le connessioni a una singola connessione dall'applicazione specificata. Ad esempio, l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con `-mSQLCMD` limita le connessioni a una singola connessione che viene identificata come programma client **sqlcmd**. Per connettersi tramite l'editor di query in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], usare `-m"Microsoft SQL Server Management Studio - Query"`.  


> [!IMPORTANT]  
> Non usare `-m` con un nome di applicazione come funzionalità di sicurezza. Le applicazioni client specificano il nome dell'applicazione tramite le impostazioni della stringa di connessione, quindi può essere facilmente sottoposto a spoofing con un nome falso.

La tabella seguente riepiloga i diversi modi per avviare l'istanza in modalità utente singolo dalla riga di comando.

| Opzione | Descrizione | Utilizzo |
|:---|:---|:---|
|`-m` | Limita le connessioni a una singola connessione | Quando non sono presenti altri utenti che tentano di connettersi all'istanza di oppure non si è certi del nome dell'applicazione usato per la connessione all'istanza. |
|`-mSQLCMD`| Limita le connessioni a una singola connessione, che deve identificarsi come programma client **sqlcmd**| Quando si prevede di connettersi all'istanza con **sqlcmd** e si vuole impedire ad altre applicazioni di ottenere l'unica connessione disponibile. |
|`-m"Microsoft SQL Server Management Studio - Query"`| Limita le connessioni a una singola connessione che deve identificarsi come applicazione **Microsoft SQL Server Management Studio - Query**.| Quando si prevede di connettersi all'istanza tramite l'editor di query di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e si vuole impedire ad altre applicazioni di ottenere l'unica connessione disponibile. |
|`-f`| Limita le connessioni a una singola connessione e avvia l'istanza con la configurazione minima | Quando un'altra configurazione impedisce l'avvio. |
| &nbsp; | &nbsp; | &nbsp; |
  
Per istruzioni dettagliate su come avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente singolo, vedere [Avviare SQL Server in modalità utente singolo](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).

## <a name="step-by-step-instructions"></a>Istruzioni dettagliate

Nelle istruzioni dettagliate riportate di seguito viene descritto come concedere le autorizzazioni di amministratore di sistema a un account di accesso SQL Server che non ha più accesso per errore.

Presupposti per le istruzioni:

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in Windows 8 o versione successiva. Quando applicabile vengono indicate lievi modifiche per le versioni precedenti di SQL Server o Windows.

* [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è installato nel computer.  

Eseguire queste istruzioni mentre si è connessi a Windows come membro del gruppo Administrators locale.

1.  Dal menu Start di Windows fare clic con il pulsante destro del mouse sull'icona per Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e scegliere **Esegui come amministratore** per passare le credenziali di amministratore a Gestione configurazione.  
  
2.  Nel riquadro a sinistra di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] selezionare **Servizi di SQL Server**. Nel riquadro a destra individuare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nell'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è incluso **(MSSQLSERVER)** dopo il nome del computer. Le istanze denominate vengono visualizzate in maiuscolo con lo stesso nome presente in Server registrati. Fare clic con il pulsante destro del mouse sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi scegliere **Proprietà**.  
  
3.  Nella casella **Specificare un parametro di avvio** della scheda **Parametri di avvio** digitare `-m` (ovvero un trattino seguito dalla lettera m minuscola) e quindi fare clic su **Aggiungi**. .  
  
    > [!NOTE]  
    >  In alcune versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è presente alcuna scheda **Parametri di avvio** . In questo caso, nella scheda **Avanzate** fare doppio clic su **Parametri di avvio**. I parametri vengono visualizzati in una finestra molto piccola. Fare attenzione a non modificare nessuno dei parametri esistenti. Al termine, aggiungere un nuovo parametro `;-m` (ovvero un punto e virgola seguito da un trattino e una lettera m minuscola) e quindi fare clic su **OK**. .  
  
4.  Fare clic su **OK** e, dopo il messaggio di riavvio, fare clic con il pulsante destro del mouse sul nome del server e quindi scegliere **Riavvia**.  
  
5.  Dopo il riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il server sarà in modalità utente singolo. Accertarsi che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non sia in esecuzione, altrimenti l'unica connessione presente non sarà più disponibile a causa del relativo utilizzo da parte di questo servizio.  
  
6.  Dal menu Start di Windows fare clic con il pulsante destro del mouse sull'icona per [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e scegliere **Esegui come amministratore**. Le credenziali di amministratore verranno passate a SSMS.
  
    > [!NOTE]  
    >  Per le versioni precedenti di Windows, l'opzione **Esegui come amministratore** viene visualizzata come sottomenu.  
  
     In alcune configurazioni, tramite SSMS si tenterà di stabilire diverse connessioni. Non sarà possibile stabilire più connessioni poiché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in modalità utente singolo. In base allo specifico scenario, eseguire una delle azioni seguenti.  
  
    1.  Effettuare la connessione con Esplora oggetti mediante l'autenticazione di Windows, che include le credenziali di amministratore. Espandere **Sicurezza**, **Account di accesso** e fare doppio clic sul proprio account di accesso. Nella pagina **Ruoli server** selezionare **sysadmin** quindi fare clic su **OK**.  
  
    2.  Anziché effettuare la connessione con Esplora oggetti, utilizzare una finestra Query tramite l'autenticazione di Windows (in cui sono incluse le credenziali di amministratore) (si tratta dell'unica modalità di connessione possibile se non è stato utilizzato Esplora oggetti). Eseguire codice come il seguente per aggiungere un nuovo account di accesso con autenticazione di Windows che fa parte del ruolo predefinito del server **sysadmin**. Nell'esempio seguente viene aggiunto un utente di dominio denominato `CONTOSO\PatK`.  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione nella modalità di autenticazione mista, effettuare la connessione con una finestra Query utilizzando l'autenticazione di Windows (in cui sono incluse le credenziali di amministratore). Eseguire codice come il seguente per creare un nuovo account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che fa parte del ruolo predefinito del server **sysadmin**.  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  Sostituire ************ con una password complessa.  
  
    4.  Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione nella modalità di autenticazione mista e si vuole reimpostare la password dell'account **sa** , eseguire la connessione con una finestra Query usando l'autenticazione di Windows, che include le credenziali di amministratore. Modificare la password dell'account **sa** con la sintassi seguente.  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  Sostituire ************ con una password complessa.  

7. Chiudere [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
8. Questi passaggi successivi ripristinano la modalità multiutente per SQL Server. Nel riquadro a sinistra di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] selezionare **Servizi di SQL Server**.

9. Nel riquadro a destra fare clic con il pulsante destro del mouse sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]e quindi scegliere **Proprietà**.  
  
10. Nella casella **Parametri esistenti** della scheda **Parametri di avvio** selezionare `-m` e quindi fare clic su **Rimuovi**.  
  
    > [!NOTE]  
    >  In alcune versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è presente alcuna scheda **Parametri di avvio** . In questo caso, nella scheda **Avanzate** fare doppio clic su **Parametri di avvio**. I parametri vengono visualizzati in una finestra molto piccola. Rimuovere i caratteri `;-m` aggiunti in precedenza e fare clic su **OK**.  
  
11. Fare clic con il pulsante destro del mouse sul nome del server e quindi scegliere **Riavvia**. Assicurarsi di avviare di nuovo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent se è stato arrestato prima di avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modalità utente singolo.
  
A questo punto è possibile connettersi normalmente con uno degli account che fa parte del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  

* [Configurare le opzioni di avvio del server](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
* [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
