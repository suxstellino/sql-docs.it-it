---
title: Configurare un computer multihomed per l'accesso
description: Informazioni su come configurare SQL Server e Windows Firewall per fornire le connessioni di rete a un'istanza di SQL Server in un ambiente multihomed.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multi-homed computer
- multi-homed computer [SQL Server] configuring ports
- firewall systems [Database Engine], multi-homed computer
ms.assetid: ba369e5b-7d1f-4544-b7f1-9b098a1e75bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a105d59c32308da01f9d875e69cb417dad31b10
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336790"
---
# <a name="configure-a-multi-homed-computer-for-sql-server-access"></a>Configurare un computer multihomed per l'accesso a SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In uno scenario tipico in cui un server deve fornire una connessione a due o più reti o subnet di rete viene utilizzato un computer multihomed, che si trova in genere in una rete perimetrale. Questo articolo descrive come configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Windows Firewall con sicurezza avanzata per fornire le connessioni di rete a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un ambiente multihomed.  
  
> [!NOTE]  
>  Un computer multihomed dispone di più schede di rete oppure è stato configurato per utilizzare più indirizzi IP per una singola scheda di rete, mentre un computer dualhomed dispone di due schede di rete oppure è stato configurato per utilizzare due indirizzi IP per una singola scheda di rete.  
  
 Prima di continuare con questo articolo, è necessario conoscere le informazioni disponibili nell'articolo [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md). Questo articolo contiene informazioni di base sul funzionamento dei componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il firewall.  
  
 **Presupposti per l'esecuzione di questo esempio:**  
  
-   Nel computer sono installate due schede di rete, di cui una o più possono essere wireless. È possibile simulare la presenza di due schede di rete tramite l'indirizzo IP di una scheda di rete e utilizzando l'indirizzo IP di loopback (127.0.0.1) come seconda scheda di rete.  
  
-   Sebbene per semplicità in questo esempio vengano utilizzati indirizzi IPv4, è possibile eseguire le stesse procedure tramite indirizzi IPv6.  
  
    > [!NOTE]  
    >  Gli indirizzi IPv4 sono costituiti da una serie di quattro numeri nota come ottetto. Ogni numero è minore di 255 ed è separato da un punto, ad esempio 127.0.0.1. Gli indirizzi IPv6 sono costituiti da una serie di otto numeri esadecimali separati da due punti, ad esempio fe80:4898:23:3:49a6:f5c1:2452:b994.  
  
-   Le regole del firewall possono consentire l'accesso tramite una porta specifica, ad esempio la porta 1433, oppure tramite il programma [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] (sqlservr.exe). Nessuno dei due metodi è migliore dell'altro. Poiché un server in una rete perimetrale è più vulnerabile agli attacchi rispetto ai server presenti in una rete Intranet, in questo articolo si presuppone che l'utente voglia esercitare un controllo più accurato e che selezioni personalmente le porte da aprire. Per questo motivo, si presuppone che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà configurato in modo da essere in ascolto su una porta fissa. Per altre informazioni sulle porte usate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
-   In questo esempio l'accesso al [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene configurato tramite la porta TCP 1433. È possibile configurare le altre porte utilizzate dai componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diversi tramite gli stessi passaggi generali.  
  
 **Di seguito vengono elencati i passaggi generali dell'esempio:**  
  
-   Determinare gli indirizzi IP nel computer.  
  
-   Configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per restare in attesa su una porta TCP specifica.  
  
-   Configurare Windows Firewall con sicurezza avanzata.  
  
## <a name="optional-procedures"></a>Procedure facoltative  
 Se gli indirizzi IP disponibili nel computer e utilizzati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sono noti, è possibile ignorare queste procedure.  
  
#### <a name="to-determine-the-ip-addresses-available-on-the-computer"></a>Per determinare gli indirizzi IP disponibili nel computer  
  
1.  Nel computer in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fare clic sul pulsante **Start**, scegliere **Esegui**, digitare **cmd** , quindi [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
2.  Nella finestra del prompt dei comandi digitare **ipconfig** , quindi premere INVIO per elencare gli indirizzi IP disponibili nel computer.  
  
    > [!NOTE]  
    >  Quando si usa il comando **ipconfig** , talvolta vengono elencate molte possibili connessioni, incluse quelle disattivate. Il comando **ipconfig** consente di elencare indirizzi sia IPv4 che IPv6.  
  
3.  Annotare gli indirizzi IPv4 e gli indirizzi IPv6 utilizzati. Le altre informazioni presenti nell'elenco, ad esempio gli indirizzi temporanei, le subnet mask e i gateway predefiniti, rappresentano informazioni importanti per la configurazione di una rete TCP/IP, ma non vengono utilizzate in questo esempio.  
  
#### <a name="to-determine-the-ip-addresses-and-ports-used-by-ssnoversion"></a>Per determinare le porte e gli indirizzi IP usati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Fare clic sul pulsante **Start**, scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, quindi fare clic su **Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
2.  In **Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nel riquadro della console espandere **Configurazione di rete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , espandere **Protocolli per \<instance name>** e quindi fare doppio clic su **TCP/IP**.  
  
3.  Nella scheda **Indirizzi TCP/IP** della finestra di dialogo **Proprietà TCP/IP** vengono visualizzati vari indirizzi IP nel formato **IP1**, **IP2** e **IPAll**. Uno di tali indirizzi corrisponde all'indirizzo IP della scheda loopback, ovvero 127.0.0.1. Ulteriori indirizzi IP vengono visualizzati per ogni indirizzo IP configurato nel computer.  
  
4.  Per ogni indirizzo IP, la presenza del valore **0** nella finestra di dialogo **Porte dinamiche TCP** indica che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è in attesa su porte dinamiche. Poiché in questo esempio vengono utilizzate porte fisse e non porte dinamiche, che potrebbero subire modifiche in caso di riavvio, se nella finestra di dialogo **Porte dinamiche TCP** è contenuto il valore **0**, eliminare lo 0.  
  
5.  Annotare la porta TCP elencata per ogni indirizzo IP da configurare. Ai fini di questo esempio, si presuppone che entrambi gli indirizzi IP siano in attesa sulla porta predefinita 1433.  
  
6.  Se si preferisce che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non usi alcune porte disponibili, nella scheda **Protocollo** impostare il valore di **Attesa su tutti** su **No**, quindi nella scheda **Indirizzi IP** impostare il valore di **Attivo** su **No** per gli indirizzi IP da non usare.  
  
## <a name="configuring-windows-firewall-with-advanced-security"></a>Configurazione di Windows Firewall con sicurezza avanzata  
 Dopo avere stabilito gli indirizzi IP utilizzati dal computer e le porte utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile creare regole del firewall, quindi configurarle per indirizzi IP specifici.  
  
#### <a name="to-create-a-firewall-rule"></a>Per creare una regola del firewall  
  
1.  Nel computer in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accedere come amministratore.  
  
2.  Fare clic sul pulsante **Start**, scegliere **Esegui**, digitare **wf.msc**, quindi fare clic su **OK**.  
  
3.  Nella finestra di dialogo **Controllo account utente** fare clic su **Continua** per usare le credenziali di amministratore per aprire lo snap-in Windows Firewall con sicurezza avanzata.  
  
4.  Nella pagina **Panoramica** confermare che Windows Firewall è abilitato.  
  
5.  Nel riquadro sinistro fare clic su **Regole connessioni in entrata**.  
  
6.  Fare clic con il pulsante destro del mouse su **Regole connessioni in entrata**, quindi scegliere **Nuova regola** per aprire **Creazione guidata nuova regola connessioni in entrata**.  
  
7.  Sebbene sia possibile creare una regola per il programma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , poiché in questo esempio viene usata una porta fissa selezionare **Porta**, quindi fare clic su **Avanti**.  
  
8.  Nella pagina **Protocolli e porte** selezionare **TCP**.  
  
9. Selezionare **Porte locali specifiche**. Digitare i numeri di porta separati da virgole, quindi fare clic su **Avanti**. Poiché in questo esempio verrà configurata la porta predefinita, immettere **1433**.  
  
10. Nella pagina **Azione** esaminare le opzioni. Poiché in questo esempio non si utilizza il firewall per forzare connessioni protette, fare clic su **Consenti la connessione**, quindi su **Avanti**.  
  
    > [!NOTE]  
    >  Nell'ambiente potrebbe essere necessario utilizzare connessioni protette. Se si seleziona una delle opzioni relative alle connessioni protette, potrebbe essere necessario configurare un certificato e l'opzione **Forza crittografia** . Per altre informazioni sulle connessioni protette, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md) e [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
11. Nella pagina **Profilo** selezionare uno o più profili per la regola. Se non si ha familiarità con i profili del firewall, fare clic sul collegamento **Ulteriori informazioni sui profili** nel programma firewall.  
  
    -   Se il computer è un server ed è disponibile solo quando è connesso a un dominio, selezionare **Dominio**, quindi fare clic su **Avanti**.  
  
    -   Se il computer è un portatile, è probabile che vengano utilizzati più profili nel caso in cui si connetta a reti diverse. In un computer di questo tipo è possibile configurare funzionalità di accesso diverse per profili distinti. È possibile ad esempio consentire l'accesso quando il computer utilizza il profilo Dominio, ma non consentirlo quando utilizza il profilo Pubblico.  
  
12. Nella pagina **Nome** specificare un nome e una descrizione per la regola, quindi fare clic su **Fine**.  
  
13. Ripetere questa procedura per creare un'altra regola per ogni indirizzo IP utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Dopo avere creato uno o più regole, per configurare ogni indirizzo IP nel computer in modo che utilizzi una regola effettuare le operazioni seguenti.  
  
#### <a name="to-configure-the-firewall-rule-for-a-specific-ip-addresses"></a>Per configurare la regola del firewall per un indirizzo IP specifico  
  
1.  Nella pagina **Regole connessioni in entrata** di **Windows Firewall con sicurezza avanzata** fare clic con il pulsante destro del mouse sulla regola creata, quindi scegliere **Proprietà**.  
  
2.  Nella finestra di dialogo **Proprietà regola** selezionare la scheda **Ambito** .  
  
3.  Nell'area **Indirizzo IP locale** selezionare **Questi indirizzi IP**, quindi fare clic su **Aggiungi**.  
  
4.  Nella finestra di dialogo **Indirizzo IP** selezionare **Subnet o indirizzo IP** quindi digitare uno degli indirizzi IP da configurare.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Nell'area **Indirizzo IP remoto** selezionare **Questi indirizzi IP**, quindi fare clic su **Aggiungi**.  
  
7.  Usare la finestra di dialogo **Indirizzo IP** per configurare la connettività per l'indirizzo IP selezionato nel computer. È possibile abilitare connessioni da indirizzi IP, intervalli di indirizzi IP e subnet intere specificati o da computer specifici. Per configurare correttamente questa opzione, è necessario conoscere in modo approfondito la rete. Per informazioni sulla rete, contattare il relativo amministratore.  
  
8.  Per chiudere la finestra di dialogo **Indirizzo IP** , fare clic su **OK**, quindi fare clic nuovamente su **OK** per chiudere la finestra di dialogo **Proprietà regola** .  
  
9. Per configurare gli altri indirizzi IP in un computer multihomed, ripetere questa procedura utilizzando un altro indirizzo IP e un'altra regola.  
  
## <a name="see-also"></a>Vedere anche  
 [Servizio SQL Server Browser &#40;motore di database e SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)   
 [Connessione a SQL Server tramite un server proxy &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager.md)  
  
  
