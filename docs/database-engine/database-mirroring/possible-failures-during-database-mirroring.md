---
title: Possibili errori durante il mirroring del database | Microsoft Docs
description: Informazioni sugli errori nelle sessioni di mirroring del database causati da problemi fisici, del sistema operativo o di SQL Server e su come rispondere agli errori.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- time-out period [SQL Server database mirroring]
- soft errors [SQL Server]
- database mirroring [SQL Server], troubleshooting
- timeout errors [SQL Server]
- troubleshooting [SQL Server], database mirroring
- hard errors
- failed database mirroring sessions [SQL Server]
ms.assetid: d7031f58-5f49-4e6d-9a62-9b420f2bb17e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2cd8b70c401a0cfc3b12f76d289b395f3d9874ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343745"
---
# <a name="possible-failures-during-database-mirroring"></a>Possibili errori durante il mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Gli errori in una sessione di mirroring del database possono essere causati da problemi di tipo fisico, del sistema operativo o di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il mirroring del database non controlla regolarmente i componenti sui quali Sqlservr.exe si basa per verificare se stiano funzionando correttamente o abbiano generato un errore. In alcuni casi, tuttavia, il componente interessato invia una segnalazione di errore a Sqlservr.exe. Un errore segnalato da un altro componente è denominato *errore hardware*. Per rilevare altri errori che altrimenti non verrebbero rilevati, il mirroring del database implementa un proprio meccanismo di timeout. Quando si verifica un timeout di mirroring, il mirroring del database presuppone che si sia verificato un errore e dichiara un *errore software*. Tuttavia, alcuni errori che si verificano a livello dell'istanza di SQL Server non provocano il timeout del mirroring a timeout e possono non essere rilevati.  
  
> [!IMPORTANT]  
>  In una sessione di mirroring del database non è possibile rilevare gli errori che si verificano nei database diversi da quello con mirroring. È inoltre improbabile rilevare un errore di un disco dati, a meno che il database non venga riavviato a causa di un errore di un disco dati.  
  
 La velocità di rilevamento degli errori, e quindi il tempo di reazione della sessione di mirroring a un errore, varia a seconda che si tratti di un errore hardware o software. Alcuni errori hardware, quali gli errori di rete, vengono segnalati immediatamente. In alcuni casi, invece, i periodi di timeout specifici dei componenti possono ritardare la segnalazione di determinati errori hardware. Per gli errori software, la velocità di rilevamento degli errori dipende dalla durata del periodo di timeout per il mirroring. L'impostazione predefinita è 10 secondi. Questo è il valore minimo consigliato.  
  
## <a name="failures-due-to-hard-errors"></a>Errori provocati da errori hardware  
 Di seguito vengono riportate alcune possibili cause di errori hardware:  
  
-   Interruzione di una connessione o danneggiamento di un cavo  
  
-   Funzionamento non corretto di una scheda di rete  
  
-   Modifica della configurazione di un router  
  
-   Modifiche al firewall  
  
-   Riconfigurazione dell'endpoint  
  
-   Guasto dell'unità contenente il log delle transazioni  
  
-   Errori del sistema operativo o di processo  
  
 Se ad esempio l'unità dei log sul database principale smette di rispondere e genera un errore, il sistema operativo segnala a Sqlservr.exe che si è verificato un errore grave.  
  
 Alcuni componenti, ad esempio componenti di rete o alcuni sottosistemi IO, dispongono di specifici timeout per la determinazione degli errori. Tali timeout sono indipendenti dal mirroring del database, che non è a conoscenza della loro esistenza né del loro funzionamento. In questi casi, il ritardo di timeout aumenta il tempo che intercorre tra il verificarsi di un errore e la ricezione del corrispondente errore hardware da parte del mirroring del database.  
  
> [!NOTE]  
>  L'unico controllo degli errori attivo eseguito per il mirroring del database si verifica per i casi di errori software. Per ulteriori informazioni, vedere "Errori provocati da errori software" di seguito in questo argomento.  
  
 Per interpretare correttamente le condizioni di errore che si verificano nella rete, richiedere a un tecnico della rete di indicare quali messaggi di errore vengono inviati a una porta quando si verificano gli eventi seguenti in una connessione TCP:  
  
-   DNS non funziona correttamente.  
  
-   I cavi sono scollegati.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows blocca una porta specifica.  
  
-   Si è verificato un errore nell'applicazione che esegue il monitoraggio di una porta.  
  
-   Un server Windows è stato rinominato.  
  
-   Un server Windows è stato riavviato.  
  
> [!NOTE]  
>  Il mirroring non fornisce protezione da problemi specifici dei client che accedono ai server. Si consideri, ad esempio, un caso in cui una scheda di rete pubblica gestisce le connessioni client all'istanza del server principale, mentre una scheda di interfaccia di rete privata gestisce tutto il traffico di mirroring tra le istanze del server. In questo caso, l'errore della scheda di rete pubblica impedirebbe l'accesso dei client al database, anche senza interruzione del mirroring del database.  
  
## <a name="failures-due-to-soft-errors"></a>Errori provocati da errori software  
 Di seguito vengono riportate alcune possibili cause dei timeout per il mirroring:  
  
-   Errori di rete, ad esempio timeout di collegamenti TCP, pacchetti ignorati, danneggiati o in ordine errato.  
  
-   Sistema operativo, server o database che non risponde.  
  
-   Timeout di un server Windows.  
  
-   Risorse di elaborazione insufficienti, ad esempio overload della CPU o del disco, esaurimento dello spazio del log delle transazioni o esaurimento della memoria o dei thread di sistema. In questi casi è necessario aumentare il periodo di timeout, ridurre il carico di lavoro o cambiare l'hardware per gestire il carico di lavoro.  
  
### <a name="the-mirroring-time-out-mechanism"></a>Meccanismo di timeout per il mirroring  
 Poiché non sono direttamente rilevabili da un'istanza del server, gli errori software possono provocare l'attesa illimitata da parte di un'istanza del server. Per evitare questo problema, il mirroring del database implementa un proprio meccanismo di timeout in base al quale ogni istanza del server di una sessione di mirroring invia un ping a ogni connessione aperta a intervalli fissi.  
  
 Per mantenere aperta una connessione, è necessario che un'istanza del server riceva un ping su tale connessione entro il periodo definito dalla somma del valore del timeout e del tempo necessario per l'invio del ping successivo. La ricezione di un ping durante il periodo di timeout indica che la connessione è ancora aperta e che le istanze del server comunicano attraverso tale connessione. Quando riceve un ping, un'istanza del server reimposta il contatore del timeout su tale connessione.  
  
 Se durante il periodo di timeout non vengono ricevuti ping in una connessione, l'istanza del server considera scaduta la connessione. L'istanza del server chiude la connessione scaduta e gestisce l'evento di timeout a seconda dello stato e della modalità operativa della sessione.  
  
 Anche se l'altro server funziona correttamente, un timeout viene considerato un errore. Se il valore di timeout per una sessione è troppo breve rispetto alla normale velocità di risposta di uno dei due partner, possono verificarsi falsi errori. Questo tipo di errore si verifica quando un'istanza del server riesce a contattarne un'altra il cui tempo di risposta è troppo breve per consentire la ricezione dei ping prima della scadenza del periodo di timeout.  
  
 Nelle sessioni in modalità a prestazioni elevate, il periodo di timeout corrisponde sempre a 10 secondi. Questa impostazione consente generalmente di evitare i falsi errori. Nelle sessioni in modalità a protezione elevata, il periodo di timeout predefinito è 10 secondi, ma può essere modificato. Per evitare falsi errori, è consigliabile impostare il periodo di timeout per il mirroring su 10 secondi o più.  
  
 **Per modificare il valore di timeout (solo modalità a protezione elevata)**  
  
-   Usare l'istruzione [ALTER DATABASE \<database> SET PARTNER TIMEOUT \<integer>](../../t-sql/statements/alter-database-transact-sql.md).  
  
 **Per visualizzare il valore di timeout corrente**  
  
-   Eseguire una query su **mirroring_connection_timeout** in [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
## <a name="responding-to-an-error"></a>Risposta a un errore  
 Indipendentemente dal tipo di errore, un'istanza del server che rileva un errore esegue l'azione appropriata in base al proprio ruolo, alla modalità operativa della sessione e allo stato delle altre connessioni della sessione. Per informazioni sulle conseguenze della perdita di un partner, vedere [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Stimare l'interruzione del servizio durante il cambio di ruolo &#40;mirroring del database&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)   
 [Modalità di funzionamento del mirroring del database](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [Cambio di ruolo durante una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
