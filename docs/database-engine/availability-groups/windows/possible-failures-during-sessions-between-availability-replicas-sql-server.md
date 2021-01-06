---
title: Cause degli errori di connessione tra le repliche di disponibilità
description: Questo argomento descrive diversi motivi per cui può verificarsi un errore di connessione tra le repliche che fanno parte di un gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], HADR
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: e96836a85fb75cc6436530356e8f38ea1a1e1882
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642547"
---
# <a name="determine-possible-reason-for-connectivity-failures-between-availability-replicas"></a>"Determinare le cause possibili degli errori di connessione tra le repliche di disponibilità
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Gli errori in una sessione tra due repliche di disponibilità possono essere causati da problemi di tipo fisico, del sistema operativo o di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Una replica di disponibilità non controlla regolarmente i componenti sui quali Sqlservr.exe si basa per verificare se stiano funzionando correttamente o abbiano generato un errore. In alcuni casi, tuttavia, il componente interessato invia una segnalazione di errore a Sqlservr.exe. Un errore segnalato da un altro componente è denominato *errore hardware*. Per rilevare altri errori che altrimenti non verrebbero rilevati, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implementa un proprio meccanismo di timeout della sessione. Specifica il periodo di timeout della sessione in secondi. Il periodo di timeout indica l'intervallo di attesa massimo rispettato dall'istanza del server per la ricezione di un messaggio PING da un'altra istanza, prima che l'altra istanza venga considerata disconnessa. Quando si verifica un timeout della sessione tra due repliche di disponibilità, le repliche di disponibilità presuppongono che si sia verificato un errore e viene dichiarato un *errore software*.  
  
> [!IMPORTANT]  
>  Non è possibile rilevare gli errori che si verificano nei database diversi da quello primario. È inoltre improbabile rilevare un errore di un disco dati, a meno che il database non venga riavviato a causa di un errore di un disco dati.  
  
 La velocità di rilevamento degli errori, e quindi il tempo di reazione a un errore, varia a seconda che si tratti di un errore hardware o software. Alcuni errori hardware, quali gli errori di rete, vengono segnalati immediatamente. In alcuni casi, invece, i periodi di timeout specifici dei componenti possono ritardare la segnalazione di determinati errori hardware. Per gli errori software, la velocità di rilevamento degli errori dipende dalla durata del periodo di timeout della sessione. L'impostazione predefinita è 10 secondi. Questo è il valore minimo consigliato.  
  
## <a name="failures-due-to-hard-errors"></a>Errori provocati da errori hardware  
 Di seguito vengono riportate alcune possibili cause di errori hardware:  
  
-   Interruzione di una connessione o danneggiamento di un cavo  
  
-   Funzionamento non corretto di una scheda di rete  
  
-   Modifica della configurazione di un router  
  
-   Modifiche al firewall  
  
-   Riconfigurazione dell'endpoint  
  
-   Guasto dell'unità contenente il log delle transazioni  
  
-   Errori del sistema operativo o di processo  
  
 Se ad esempio l'unità dei log sul database primario smette di rispondere e genera un errore, il sistema operativo segnala a Sqlservr.exe che si è verificato un errore grave.  
  
 Alcuni componenti, ad esempio componenti di rete o alcuni sottosistemi IO, dispongono di specifici timeout per la determinazione degli errori. Tali timeout sono indipendenti da [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], che non è a conoscenza della loro esistenza né del loro funzionamento. In questi casi, il ritardo di timeout aumenta il tempo che intercorre tra il verificarsi di un errore e la ricezione del corrispondente errore hardware da parte della replica di disponibilità.  
  
> [!NOTE]  
>  L'unico controllo degli errori attivo eseguito per le repliche di disponibilità si verifica per i casi di errori software. Per ulteriori informazioni, vedere "Errori provocati da errori software" di seguito in questo argomento.  
  
 Per interpretare correttamente le condizioni di errore che si verificano nella rete, richiedere a un tecnico della rete di indicare quali messaggi di errore vengono inviati a una porta quando si verificano gli eventi seguenti in una connessione TCP:  
  
-   DNS non funziona correttamente.  
  
-   I cavi sono scollegati.  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows blocca una porta specifica.  
  
-   Si è verificato un errore nell'applicazione che esegue il monitoraggio di una porta.  
  
-   Un server Windows è stato rinominato.  
  
-   Un server Windows è stato riavviato.  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)] non fornisce protezione da problemi specifici dei client che accedono ai server. Si consideri, ad esempio, un caso in cui una scheda di rete pubblica gestisce le connessioni client alla replica primaria, mentre una scheda di interfaccia di rete privata gestisce il traffico tra le istanze del server che ospitano le repliche di un gruppo di disponibilità. In questo caso, l'errore della scheda di rete pubblica impedirebbe l'accesso dei client al database.  
  
## <a name="failures-due-to-soft-errors"></a>Errori provocati da errori software  
 Di seguito vengono riportate alcune possibili cause dei timeout della sessione:  
  
-   Errori di rete, ad esempio timeout di collegamenti TCP, pacchetti ignorati, danneggiati o in ordine errato.  
  
-   Sistema operativo, server o database che non risponde.  
  
-   Timeout di un server Windows.  
  
-   Risorse di elaborazione insufficienti, ad esempio overload della CPU o del disco, esaurimento dello spazio del log delle transazioni o esaurimento della memoria o dei thread di sistema. In questi casi è necessario aumentare il periodo di timeout, ridurre il carico di lavoro o cambiare l'hardware per gestire il carico di lavoro.  
  
### <a name="the-session-timeout-mechanism"></a>Meccanismo di timeout della sessione  
 Poiché gli errori software non sono direttamente rilevabili da un'istanza del server, un errore software potrebbe potenzialmente causare che una replica di disponibilità attenda indefinitamente la risposta dell'altra replica di disponibilità in una sessione. Per evitare questo problema, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implementa un meccanismo di timeout della sessione in base alle repliche di disponibilità connesse che inviano un ping a ogni connessione aperta a intervalli fissi. La ricezione di un ping durante il periodo di timeout indica che la connessione è ancora aperta e che le istanze del server comunicano attraverso tale connessione. Quando riceve un ping, una replica reimposta il contatore del timeout su tale connessione. Per informazioni sulla relazione di timeout della modalità di disponibilità e della sessione, vedere [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 Le repliche primarie e secondarie effettuano vicendevolmente il ping per segnalare che sono ancora attive e un limite di timeout della sessione impedisce che la replica attenda indefinitamente la ricezione di un ping dall'altra replica. Il limite di timeout della sessione è una proprietà della replica configurabile dall'utente con un valore predefinito di 10 secondi. La ricezione di un ping durante il periodo di timeout indica che la connessione è ancora aperta e che le istanze del server comunicano attraverso tale connessione. Alla ricezione di un ping, la replica di disponibilità reimposta il contatore del timeout per quella connessione.  
  
 Se non viene ricevuto alcun ping dall'altra replica entro il periodo di timeout della sessione, si verifica il timeout della connessione. La connessione viene chiusa e per la replica scaduta viene impostato lo stato DISCONNECTED. Anche se la replica disconnessa è configurata per la modalità con commit sincrono, le transazioni non attenderanno che quella replica venga riconnessa e risincronizzata.  
  
## <a name="responding-to-an-error"></a>Risposta a un errore  
 Indipendentemente dal tipo di errore, un'istanza del server che rileva un errore esegue l'azione appropriata in base al proprio ruolo, alla modalità di disponibilità della sessione e allo stato delle altre connessioni della sessione. Per informazioni sulle conseguenze della perdita di un partner, vedere [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="related-tasks"></a>Attività correlate  
 **Per modificare il valore di timeout (solo modalità di disponibilità con commit sincrono)**  
  
-   [Modificare il periodo di timeout della sessione per una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Per visualizzare il valore di timeout corrente**  
  
-   Eseguire una query su **session_timeout** in [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
