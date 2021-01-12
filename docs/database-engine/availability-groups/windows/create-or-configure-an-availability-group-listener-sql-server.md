---
title: Configurare il listener del gruppo di disponibilità
description: 'Descrive la procedura per configurare un listener per un gruppo di disponibilità Always On usando PowerShell o SQL Server Management Studio. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.newaglistener.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], client connectivity
ms.assetid: 2bc294f6-2312-4b6b-9478-2fb8a656e645
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 6e6b362f9e121f10515753d46ee098387348f272
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091738"
---
# <a name="configure-a-listener-for-an-always-on-availability-group"></a>Configurare un listener per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come creare o configurare un singolo *listener del gruppo di disponibilità* per un gruppo di disponibilità AlwaysOn usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
> [!IMPORTANT]  
>  Per creare il primo listener di un gruppo di disponibilità, è consigliabile usare [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Evitare di creare un listener direttamente nel cluster WSFC se non necessario, ad esempio per creare un listener aggiuntivo.  
  
 
##  <a name="does-a-listener-exist-for-this-availability-group-already"></a><a name="DoesListenerExist"></a> Esiste già un listener per questo gruppo di disponibilità?  

 **Per determinare se un listener già esiste per il gruppo di disponibilità**  
  
-   [Visualizzare le proprietà del listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
> [!NOTE]  
>  Se è già presente un listener e si vuole creare un listener aggiuntivo, vedere [Per creare un listener aggiuntivo per un gruppo di disponibilità (facoltativo)](#CreateAdditionalListener), più avanti in questo argomento.  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   È possibile creare un solo listener per gruppo di disponibilità tramite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ogni gruppo di disponibilità richiede in genere un solo listener. Tuttavia, alcuni scenari del cliente richiedono più listener per un gruppo di disponibilità.   Dopo avere creato un listener tramite SQL Server, è possibile utilizzare Windows PowerShell per i cluster di failover o Gestione cluster di failover WSFC per creare listener aggiuntivi. Per altre informazioni, vedere [Per creare un listener aggiuntivo per un gruppo di disponibilità (facoltativo)](#CreateAdditionalListener), più avanti in questo argomento.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
 L'utilizzo di un indirizzo IP statico è consigliato, sebbene non obbligatorio, per più configurazioni di subnet.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
  
-   Se si imposta un listener del gruppo di disponibilità su più subnet e si pensa di utilizzare gli indirizzi IP statici, è necessario ottenere gli indirizzi IP statici di ogni subnet che ospita una replica di disponibilità per il gruppo di disponibilità per il quale si crea il listener. In genere, è necessario richiedere gli indirizzi IP statici agli amministratori di rete.  
  
> [!IMPORTANT]  
>  Prima di creare il primo listener, è consigliabile leggere l'agomento [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="requirements-for-the-dns-name-of-an-availability-group-listener"></a><a name="DNSnameReqs"></a> Requisiti per il nome DNS di un listener del gruppo di disponibilità  
 Ogni listener del gruppo di disponibilità richiede un nome host DNS univoco nel dominio e in NetBIOS. Il nome DNS è un valore stringa. Può contenere solo caratteri alfanumerici, trattini/segni meno (-) e caratteri di sottolineatura (_), in qualsiasi ordine. Per i nomi host DNS non viene fatta distinzione tra maiuscole e minuscole. La lunghezza massima è di 63 caratteri, tuttavia in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]è possibile specificare una lunghezza massima di 15 caratteri.  
  
 È consigliabile specificare una stringa significativa. Ad esempio, per un gruppo di disponibilità denominato `AG1`un nome host DNS significativo potrebbe essere `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS riconosce solo i primi 15 caratteri di dns_name. Se si dispone di due cluster WSFC controllati dallo stesso dominio Active Directory e si tenta di creare listener del gruppo di disponibilità in entrambi i cluster usando nomi con più di 15 caratteri e un prefisso a 15 caratteri identico, verrà restituito un errore in cui si segnala che non è possibile portare online la risorsa del nome di rete virtuale. Per informazioni sulle regole di denominazione dei prefissi per i nomi DNS, vedere [Assegnare nomi ai domini](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
##  <a name="windows-permissions"></a><a name="WinPermissions"></a> Autorizzazioni di Windows  
  
|Autorizzazioni|Collegamento|  
|-----------------|----------|  
|Il nome dell'oggetto cluster WSFC che ospita il gruppo di disponibilità deve avere l'autorizzazione per la **creazione degli oggetti computer** .<br /><br /> Per impostazione predefinita, in Active Directory un nome di oggetto cluster non ha l'autorizzazione per la **creazione degli oggetti computer** assegnata in modo esplicito e può creare 10 oggetti computer virtuali. Dopo aver creato 10 oggetti computer virtuali, la creazione di ulteriori oggetti di questo tipo avrà esito negativo. È possibile evitare questo problema concedendo in modo esplicito l'autorizzazione al nome dell'oggetto cluster WSFC. Si noti che gli oggetti computer virtuali per i gruppi di disponibilità eliminati non vengono rimossi automaticamente da Active Directory e continuano a essere conteggiati ai fini del limite predefinito di 10 oggetti a meno che non vengano eliminati manualmente.<br /><br /> Nota: in alcune organizzazioni i criteri di sicurezza non permettono di concedere l'autorizzazione per la **creazione di oggetti computer** a singoli account utente.|*Passaggi per la configurazione dell'account per chi installa il cluster* nella [Guida dettagliata al cluster di failover: Configurazione di account in Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_installer)<br /><br /> *Passaggi per la configurazione pre-installazione dell'account del nome cluster* nella [Guida dettagliata al cluster di failover: Configurazione di account in Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating)|  
|Se l'organizzazione richiede la configurazione pre-installazione dell'account del computer per un nome di rete virtuale del listener, sarà necessaria l'appartenenza al gruppo **Account Operator** o l'assistenza dell'amministratore di dominio.|*Passaggi per la configurazione pre-installazione di un account per un servizio o un'applicazione cluster* nella [Guida dettagliata al cluster di failover: Configurazione di account in Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating2).|  
  
> [!TIP]  
>  In genere, è più semplice non effettuare la configurazione pre-installazione dell'account del computer per un nome di rete virtuale del listener. Se è possibile, procedere con la creazione e la configurazione automatica dell'account durante l'esecuzione della procedura guidata Disponibilità elevata WSFC.  
  
##  <a name="sql-server-permissions"></a><a name="SqlPermissions"></a> Autorizzazioni di SQL Server  
  
|Attività|Autorizzazioni|  
|----------|-----------------|  
|Per creare un listener del gruppo di disponibilità|Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.|  
|Per modificare un listener del gruppo di disponibilità esistente|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
> [!TIP]  
>  In [Creazione guidata Gruppo di disponibilità](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md) è supportata la creazione del listener per un nuovo gruppo di disponibilità.  
  
 **Per creare o configurare un listener del gruppo di disponibilità**  
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria del gruppo di disponibilità, quindi fare clic sul nome del server per espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic sul gruppo di disponibilità di cui si desidera configurare il listener e scegliere una delle alternative seguenti:  
  
    -   Per creare un listener, fare clic con il pulsante destro del mouse sul nodo **Listener del gruppo di disponibilità** e selezionare il comando **Nuovo listener** . Verrà aperta la finestra di dialogo **Nuovo listener gruppo di disponibilità** . Per ulteriori informazioni, vedere [Aggiungi listener del gruppo di disponibilità (finestra di dialogo)](#AddAgListenerDialog), più avanti in questo argomento.  
  
    -   Per modificare il numero di porta di un listener esistente, espandere il nodo **Listener del gruppo di disponibilità** , fare clic con il pulsante destro del mouse sul listener e selezionare il comando **Proprietà** . Immettere il nuovo numero di porta nel campo **Porta** e fare clic su **OK**.  
  
###  <a name="new-availability-group-listener-dialog-box"></a><a name="AddAgListenerDialog"></a> Nuovo listener gruppo di disponibilità (finestra di dialogo)  
 **Nome DNS del listener**  
 Specifica il nome host DNS del listener del gruppo di disponibilità. È inoltre necessario che il nome DNS sia una stringa univoca nel dominio e in NetBIOS. Può contenere solo caratteri alfanumerici, trattini (-) e caratteri di sottolineatura (_), in qualsiasi ordine. Per i nomi host DNS non viene fatta distinzione tra maiuscole e minuscole. La lunghezza massima è di 15 caratteri.  
  
 Per ulteriori informazioni, vedere [Requisiti per il nome DNS di un listener del gruppo di disponibilità](#DNSnameReqs), più indietro in questo argomento.  
  
 **Porta**  
 Porta TCP usata dal listener.  
  
 **Modalità di rete**  
 Indica il protocollo TCP utilizzato dal listener. È possibile scegliere uno dei seguenti valori:  
  
 **DHCP**  
 Il listener utilizzerà un indirizzo IP dinamico assegnato da un server in cui viene eseguito il protocollo DHCP (Dynamic Host Configuration Protocol). DHCP è limitato a una sola subnet.  
  
> [!IMPORTANT]  
>  Non è consigliabile utilizzare DHCP negli ambienti di produzione. Se si verifica un periodo di inattività e il lease IP DHCP scade, è necessario del tempo aggiuntivo per registrare il nuovo indirizzo IP della rete DHCP che è associato al nome DNS del listener e influisce sulla connettività client. DHCP può essere tranquillamente usato per la configurazione dell'ambiente di sviluppo e test per verificare le funzioni di base di gruppi di disponibilità e per l'integrazione con le applicazioni.  
  
 **Indirizzo IP statico**  
 Il listener utilizzerà uno o più indirizzi IP statici. Gli indirizzi IP aggiuntivi sono facoltativi. Per creare un listener del gruppo di disponibilità tra più subnet, è necessario specificare un indirizzo IP statico per ogni subnet nella configurazione del listener. Contattare l'amministratore della rete per ottenere questi indirizzi IP statici.  
  
 Se si seleziona **Indirizzo IP statico** , verrà visualizzata una griglia della subnet sotto il campo **Modalità di rete** . In questa griglia sono visualizzate informazioni su ogni subnet alla quale è possibile accedere da questo listener del gruppo di disponibilità. La griglia è vuota finché non si aggiunge un indirizzo IP statico facendo clic su **Aggiungi**.  
  
 Le colonne sono le seguenti:  
  
 **Subnet**  
 Viene visualizzato l'identificatore di ogni subnet aggiunta al listener del gruppo di disponibilità.  
  
 **Indirizzo IP**  
 Viene visualizzato l'indirizzo IP di una determinata subnet.  Per una determinata subnet, l'indirizzo IP è un indirizzo IPv4 o IPv6.  
  
 **Aggiungere**  
 Fare clic per aggiungere un indirizzo IP statico a una subnet selezionata o a un'altra subnet per questo listener. Verrà aperta la finestra di dialogo **Aggiungi indirizzo IP** . Per altre informazioni, vedere l'argomento della Guida [Finestra di dialogo Aggiungi indirizzo IP &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Rimuovi**  
 Fare clic per rimuovere la subnet selezionata dal listener.  
  
 **OK**  
 Fare clic per creare il listener del gruppo di disponibilità specificato.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 **Per creare o configurare un listener del gruppo di disponibilità**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Utilizzare l'opzione LISTENER dell'istruzione [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) o l'opzione ADD LISTENER dell'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) .  
  
     Nell'esempio seguente viene aggiunta un listener del gruppo di disponibilità a un gruppo di disponibilità denominato `MyAg2`. e un nome DNS univoco, `MyAg2ListenerIvP6`, per questo listener. Le due repliche si trovano in subnet diverse, pertanto, come consigliato, per il listener devono essere utilizzati indirizzi IP statici. Per ognuna delle due repliche di disponibilità, tramite la clausola WITH IP si specifica un indirizzo IP statico, `2001:4898:f0:f00f::cf3c and 2001:4898:e0:f213::4ce2`, per il quale è utilizzato il formato IPv6. In questo esempio si specifica anche utilizza l'argomento PORT facoltativo per indicare la porta `60173` come porta del listener.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAg2   
          ADD LISTENER 'MyAg2ListenerIvP6' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173 );   
    GO  
  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
 **Per creare o configurare un listener del gruppo di disponibilità**  
  
1.  Cambiare la directory (**cd**) impostandola sull'istanza del server che ospita la replica primaria.  
  
2.  Per creare o modificare un listener del gruppo di disponibilità, utilizzare uno dei cmdlet seguenti:  
  
     **New-SqlAvailabilityGroupListener**  
     Crea un nuovo listener del gruppo di disponibilità e lo collega a un gruppo di disponibilità esistente.  
  
     Ad esempio, il comando **New-SqlAvailabilityGroupListener** seguente consente di creare un listener del gruppo di disponibilità denominato `MyListener` per il gruppo di disponibilità `MyAg`. Il listener userà l'indirizzo IPv4 passato al parametro **-StaticIp** come indirizzo IP virtuale.  
  
    ```  
    New-SqlAvailabilityGroupListener -Name MyListener `   
    -StaticIp '192.168.3.1/255.255.252.0' `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg  
  
    ```  
  
     **Set-SqlAvailabilityGroupListener**  
     Modifica l'impostazione della porta su un listener del gruppo di disponibilità esistente.  
  
     Ad esempio, il comando **Set-SqlAvailabilityGroupListener** consente di impostare il numero di porta per il listener del gruppo di disponibilità denominato `MyListener` su `1535`. Questa porta viene utilizzata per restare in ascolto delle connessioni al listener.  
  
    ```  
    Set-SqlAvailabilityGroupListener -Port 1535 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\MyListener  
  
    ```  
  
     **Add-SqlAGListenerstaticIp**  
     Aggiunge un indirizzo IP statico a una configurazione del listener del gruppo di disponibilità esistente. L'indirizzo IP può essere un indirizzo IPv4 con subnet o un indirizzo IPv6.  
  
     Ad esempio, il comando **Add-SqlAGListenerstaticIp** seguente consente di aggiungere un indirizzo IPv4 statico al listener del gruppo di disponibilità `MyListener` nel gruppo di disponibilità `MyAg`. Questo indirizzo IPv6 viene utilizzato come indirizzo IP virtuale del listener nella subnet `255.255.252.0`. Se il gruppo di disponibilità viene esteso a più subnet, è consigliabile aggiungere un indirizzo IP statico per ogni subnet al listener.  
  
    ```  
    $path = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\ MyListener" `   
    Add-SqlAGListenerstaticIp -Path $path `   
    -StaticIp "2001:0db8:85a3:0000:0000:8a2e:0370:7334"  
    ```  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help**  nell'ambiente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi  
  
###  <a name="failure-to-create-an-availability-group-listener-because-of-active-directory-quotas"></a><a name="ADQuotas"></a> Impossibile creare un listener del gruppo di disponibilità a causa di quote di Active Directory  
 La creazione di un nuovo listener del gruppo di disponibilità può avere esito negativo perché è stata raggiunta una quota di Active Directory per l'account del computer del nodo del cluster interessato.  Per altre informazioni, vedere gli articoli seguenti:  
  
-   [Risoluzione dei problemi dell'account del Servizio cluster in relazione alla modifica di oggetti computer](https://support.microsoft.com/kb/307532)  
  
-   [Quote di Active Directory](https://technet.microsoft.com/library/cc904295\(WS.10\).aspx)  
  
##  <a name="follow-up-after-creating-an-availability-group-listener"></a><a name="FollowUp"></a> Completamento: Dopo la creazione di un listener del gruppo di disponibilità  
  
###  <a name="multisubnetfailover-keyword-and-associated-features"></a><a name="MultiSubnetFailover"></a> Parola chiave MultiSubnetFailover e funzionalità associate  
 **MultiSubnetFailover** è una nuova parola chiave della stringa di connessione usata per accelerare il failover con i gruppi di disponibilità AlwaysOn e le istanze del cluster di failover AlwaysOn in SQL Server 2012. Le tre seguenti funzionalità secondarie vengono abilitate quando `MultiSubnetFailover=True` è impostato nella stringa di connessione:  
  
-   Failover multisubnet più rapido su un listener su più subnet per un gruppo di disponibilità AlwaysOn o istanze del cluster di failover.  
  
-   Failover a singola subnet più rapido su un listener su una sola subnet per un gruppo di disponibilità AlwaysOn o istanze del cluster di failover.  
  
    -   Questa funzionalità viene usata per la connessione a un listener in cui è disponibile un singolo indirizzo IP in una sola subnet. Vengono effettuati tentativi di connessione TCP più aggressivi per accelerare i failover su una sola subnet.  
  
-   Risoluzione dell'istanza denominata su un'istanza del cluster di failover AlwaysOn su più subnet.  
  
    -   Questo consente di aggiungere il supporto della risoluzione dell'istanza denominata per istanze del cluster di failover AlwaysOn con endpoint su più subnet.  
  
 **MultiSubnetFailover=True non supportato da .NET Framework 3.5 o OLEDB**  
  
 **Problema:** se nel gruppo di disponibilità o nell'istanza del cluster di failover è disponibile un nome di listener (noto come nome di rete o punto di accesso client in Gestione cluster WSFC) dipendente da più indirizzi IP di subnet diverse e si sta usando ADO.NET con .NET Framework 3.5SP1 o SQL Native Client 11.0 OLEDB, potenzialmente il 50% delle richieste di connessione client al listener del gruppo di disponibilità riscontrerà un timeout di connessione.  
  
 **Soluzioni alternative:** è consigliabile eseguire una delle attività seguenti.  
  
-   Se non si dispone dell'autorizzazione per usare le risorse cluster, modificare il timeout di connessione in 30 secondi (questo valore corrisponde a un periodo di timeout TCP di 20 secondi più un buffer di 10).  
  
     **Vantaggi**: se si verifica un failover tra subnet, il tempo di recupero del client è breve.  
  
     **Svantaggi**: la metà delle connessioni client impiegherà più di 20 secondi  
  
-   Se si dispone dell'autorizzazione per utilizzare le risorse cluster, l'approccio migliore consiste nell'impostare il nome di rete del listener del gruppo di disponibilità su `RegisterAllProvidersIP=0`. Per altre informazioni, vedere "Impostazione RegisterAllProvidersIP" più avanti in questa sezione.  
  
     **Vantaggi:** non è necessario aumentare il valore di timeout della connessione client.  
  
     **Svantaggi:** se si verifica un failover tra subnet, il tempo di recupero del client potrebbe essere di almeno 15 minuti, a seconda dell'impostazione **HostRecordTTL** e della pianificazione della replica DNS/AD tra siti.  
  
###  <a name="registerallprovidersip-setting"></a><a name="RegisterAllProvidersIP"></a> Impostazione RegisterAllProvidersIP  
 Se si usa [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell per creare un listener del gruppo di disponibilità, il punto di accesso client viene creato in WSFC con la proprietà **RegisterAllProvidersIP** impostata su 1 (true). L'effetto del valore di questa proprietà dipende dalla stringa di connessione client, come indicato di seguito:  
  
-   Stringhe di connessione che impostano **MultiSubnetFailover** su true  
  
     [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] imposta la proprietà  **RegisterAllProvidersIP** su 1 per ridurre il tempo di riconnessione in seguito a un failover per i client le cui stringhe di connessione specificano `MultiSubnetFailover = True`, come consigliato. Per sfruttare la funzionalità di più subnet del listener, i client possono richiedere un provider di dati che supporta la parola chiave **MultiSubnetFailover** . Per informazioni sul supporto per la connettività client, vedere [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
     Per informazioni sul clustering su più subnet, vedere [Clustering su più subnet di SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
    > [!TIP]  
    >  Quando `RegisterAllProvidersIP = 1`, se si esegue la Convalida guidata configurazione di WSFC nel cluster WSFC, verrà generato il seguente messaggio di avviso:  
    >   
    >  "La proprietà RegisterAllProviderIP per il nome di rete 'Nome:<nome_rete>' è impostata su 1. Per la configurazione corrente del cluster tale valore dovrebbe essere impostato su 0."  
    >   
    >  Ignorare tale messaggio.  
  
-   Stringhe di connessione che non impostano **MultiSubnetFailover** su true  
  
     Se `RegisterAllProvidersIP = 1`, in tutti i client le cui stringhe di connessione non utilizzano `MultiSubnetFailover = True`si verificheranno connessioni ad alta latenza. Questa situazione si verifica in quanto questi client tentano di effettuare connessioni a tutti gli indirizzi IP in sequenza. Al contrario, se **RegisterAllProvidersIP** viene impostato su 0, l'indirizzo IP attivo viene registrato nel punto di accesso client del cluster WSFC, riducendo la latenza per i client legacy. Se quindi alcuni client legacy devono connettersi a un listener del gruppo di disponibilità e non è possibile usare la proprietà **MultiSubnetFailover**, è consigliabile impostare **RegisterAllProvidersIP** su 0.  
  
    > [!IMPORTANT]  
    >  Durante la creazione di un listener del gruppo di disponibilità con il cluster WSFC (interfaccia utente grafica di Gestione cluster di failover), il valore di **RegisterAllProvidersIP** sarà 0 (false) per impostazione predefinita.  
  
###  <a name="hostrecordttl-setting"></a><a name="HostRecordTTL"></a> Impostazione HostRecordTTL  
 Per impostazione predefinita, tramite i client vengono memorizzati nella cache record DNS del cluster per 20 minuti.  Riducendo il valore di **HostRecordTTL**, la durata (TTL), per i record memorizzati nella cache, i client legacy possono riconnettersi più rapidamente.  La riduzione del valore dell'impostazione **HostRecordTTL** può però comportare anche un aumento del traffico verso i server DNS.  
  
###  <a name="sample-powershell-script-to-disable-registerallprovidersip-and-reduce-ttl"></a><a name="SampleScript"></a> Script PowerShell di esempio per disabilitare RegisterAllProvidersIP e ridurre TTL  
 L'esempio di PowerShell seguente illustra come configurare entrambi i parametri cluster **RegisterAllProvidersIP** e **HostRecordTTL** per la risorsa listener.  Il record DNS verrà memorizzato nella cache per 5 minuti, anziché i 20 minuti predefiniti.  Modificando entrambi i parametri del cluster è possibile ridurre il tempo di connessione all'indirizzo IP corretto dopo un failover per i client legacy che non possono usare il parametro **MultiSubnetFailover** .  Sostituire `yourListenerName` con il nome del listener che si sta modificando.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName | Set-ClusterParameter RegisterAllProvidersIP 0   
Get-ClusterResource yourListenerName | Set-ClusterParameter HostRecordTTL 300  
Stop-ClusterResource yourListenerName  
Start-ClusterResource yourListenerName  
Start-Clustergroup yourListenerGroupName
```  
  
 Per altre informazioni sui tempi di recupero durante i failover, vedere [Client Recovery Latency During Failover](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md#DNS).  
  
###  <a name="follow-up-recommendations"></a><a name="FollowUpRecommendations"></a> Indicazioni sul completamento  
 Dopo aver creato un listener del gruppo di disponibilità:  
  
-   Chiedere all'amministratore di rete di riservare l'indirizzo IP del listener per un uso esclusivo.  
  
-   Fornire il nome host DNS del listener agli sviluppatori dell'applicazione in modo da essere usato nelle stringhe di connessione per la richiesta di connessioni client al gruppo di disponibilità.  
  
-   Incoraggiare gli sviluppatori ad aggiornare le stringhe di connessione client in modo da specificare `MultiSubnetFailover = True`, se possibile. Per informazioni sul supporto per la connettività client, vedere [Connettività client AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="create-an-additional-listener-for-an-availability-group-optional"></a><a name="CreateAdditionalListener"></a> Creare un listener aggiuntivo per un gruppo di disponibilità (facoltativo)  
 Dopo avere creato un listener tramite SQL Server, è possibile aggiungere un altro listener come indicato di seguito:  
  
1.  Creare il listener utilizzando uno degli strumenti seguenti:  
  
    -   **Gestione cluster di failover WSFC:**  
  
        1.  Aggiungere un punto di accesso client e configurare l'indirizzo IP.  
  
        2.  Portare il listener online.  
  
        3.  Aggiungere una dipendenza alla risorsa del gruppo di disponibilità WSFC.  
  
         Per informazioni sulle finestre di dialogo e le schede di Gestione cluster di failover, vedere [Interfaccia utente: snap-in Gestione cluster di failover](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772502(v=ws.11)).  
  
    -   **Utilizzo di Windows PowerShell per i cluster di failover:**  
  
        1.  Usare [Add-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee460983(v=technet.10)) per creare un nome di rete e le risorse dell'indirizzo IP.  
  
        2.  Usare [Start-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461056(v=technet.10)) per avviare la risorsa del nome di rete.  
  
        3.  Usare [Add-ClusterResourceDependency](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461014(v=technet.10)) per impostare la dipendenza tra la risorsa del nome di rete e la risorsa del gruppo di disponibilità di SQL Server.  
  
         Per informazioni sull'utilizzo di Windows PowerShell per i cluster di failover, vedere [Panoramica dei comandi di Server Manager](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732757(v=ws.11)#BKMK_wps).  
  
2.  Avviare l'attesa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sul nuovo listener. Dopo avere creato il listener aggiuntivo, connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui è ospitata la replica primaria del gruppo di disponibilità e utilizzare [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell per modificare la porta del listener.  
  
 Per ulteriori informazioni, vedere [Come creare più listener per lo stesso gruppo di disponibilità](/archive/blogs/sqlalwayson/how-to-create-multiple-listeners-for-same-availability-group-goden-yao) (blog del team di SQL Server AlwaysOn).  
  
 
  
## <a name="next-steps"></a>Passaggi successivi

Dopo aver creato il listener, configurare l'applicazione per [connettersi al listener](listeners-client-connectivity-application-failover.md). È anche possibile esaminare diverse [strategie di monitoraggio dei gruppi di disponibilità](monitoring-of-availability-groups-sql-server.md) per garantire l'integrità del gruppo di disponibilità.

È anche possibile [visualizzare le proprietà di un listener](view-availability-group-listener-properties-sql-server.md) o [rimuovere il listener](remove-an-availability-group-listener-sql-server.md), se necessario.