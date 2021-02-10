---
title: Configurare la riproduzione per gli aggiornamenti di SQL Server
description: Usare Database Experimentation Assistant (DEA) per accedere agli strumenti di Riesecuzione distribuita. Utilizzare gli strumenti di per riprodurre una traccia acquisita su un ambiente di testing aggiornato.
ms.custom: seo-lt-2019
ms.date: 01/24/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 18428e87d1c769d10a4d1acbf0033c739db09d0d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017061"
---
# <a name="configure-distributed-replay-for-database-experimentation-assistant"></a>Configurare Riesecuzione distribuita per Database Experimentation Assistant

Database Experimentation Assistant (DEA) utilizza gli strumenti di Riesecuzione distribuita dell'installazione SQL Server per riprodurre una traccia acquisita su un ambiente di testing aggiornato. È consigliabile eseguire un'esecuzione dei test utilizzando un piccolo file di traccia prima di eseguire una riproduzione completa per garantire la riproduzione corretta delle query.

## <a name="distributed-replay-requirements"></a>Requisiti relativi a Riesecuzione distribuita

- Per creare file IRF nel computer del controller Riesecuzione distribuita è necessario un ulteriore 78% di spazio su disco rigido.
- 200 MB o 512 MB sono le dimensioni ideali per il rollover della traccia da usare per acquisire le tracce di produzione o prestazioni.
- I requisiti minimi di CPU e RAM per il controller Riesecuzione distribuita e i computer client sono una CPU a core singolo con 3,5 GB di RAM.
- Il tempo di riproduzione richiede circa 1,55 volte più lungo del tempo di acquisizione, perché per riprodurre la traccia di produzione vengono usati un controller e quattro computer figlio.
- Se si utilizzano le versioni "pubblicate" dei file di definizione della traccia di produzione e delle prestazioni e la definizione della traccia delle prestazioni filtra le tracce per un database di interesse, l'analisi mostra che la dimensione della **traccia delle prestazioni** è circa 15 volte maggiore rispetto alla dimensione della **traccia di produzione** .

## <a name="set-up-a-virtual-network-or-domain"></a>Configurare una rete virtuale o un dominio

Riesecuzione distribuita richiede l'uso di account comuni tra computer. A causa di questo requisito e, per motivi di sicurezza, è consigliabile eseguire Riesecuzione distribuita in una rete virtuale o in una rete controllata dal dominio:

- Creare il controller e i computer client nell'ambiente.
- Verificare che il controller e i computer client possano eseguire il ping reciprocamente sulla rete.
- Riesecuzione distribuita i computer client devono disporre di connettività al computer di destinazione della riproduzione che esegue SQL Server.

## <a name="set-up-the-controller-service"></a>Configurare il servizio controller

Per configurare il servizio controller:

1. Installare il controller di Riesecuzione distribuita usando il programma di installazione di SQL Server. Se è stato ignorato il passaggio del programma di installazione guidata di SQL Server che configura il controller Riesecuzione distribuita, è possibile configurare il controller tramite il file di configurazione. In un'installazione tipica, il file di configurazione si trova in C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayController\DReplayController.config.
2. I log del controller di Riesecuzione distribuita si trovano in C:\Programmi (x86) \Microsoft SQL Server \<version\> \Tools\DReplayController\Log.
3. Aprire Services. msc e passare al servizio **SQL Server riesecuzione distribuita controller** .
4. Fare clic con il pulsante destro del mouse sul servizio, quindi scegliere **Proprietà**. Impostare l'account del servizio su un account comune per il controller e i computer client nella rete.
5. Selezionare **OK** per chiudere la finestra delle **Proprietà** .
6. Riavviare il servizio **SQL Server riesecuzione distribuita controller** da Services. msc. È anche possibile eseguire i comandi seguenti dalla riga di comando per riavviare il servizio:

   `NET STOP "SQL Server Distributed Replay Controller"`</br>
   `NET START "SQL Server Distributed Replay Controller"`

Per altre opzioni di configurazione, vedere [Configure riesecuzione distribuita](../tools/distributed-replay/configure-distributed-replay.md).

## <a name="configure-dcom"></a>Configurare DCOM

Questa configurazione è necessaria solo sul computer controller.

1. Aprire dcomcnfg.exe.
2. Espandere **computer Servizi componenti**  >    >  **computer locale**  >  **configurazione DCOM**.
3. In **configurazione DCOM**, fare clic con il pulsante destro del mouse su **DReplayController** e quindi scegliere **proprietà**.
4. Selezionare la scheda **Sicurezza**.
5. In **autorizzazioni di esecuzione e attivazione** selezionare **Personalizza**, quindi fare clic su **modifica**.
6. Aggiungere l'utente che avvierà la riproduzione. Assegnare le autorizzazioni di avvio locale e di attivazione locale dell'utente. Se l'utente intende avviare o attivare in remoto, concedere all'utente le autorizzazioni di avvio remoto e attivazione remota.
7. Selezionare **OK** per salvare le modifiche e tornare alla scheda **sicurezza** .
8. In **autorizzazioni di accesso** selezionare **Personalizza**, quindi selezionare **modifica**.
9. Aggiungere l'utente che avvierà la riproduzione. Concedere all'utente le autorizzazioni di accesso locale. Se l'utente prevede di accedere al servizio controller in remoto, concedere all'utente le autorizzazioni di accesso remoto.
10. Selezionare **OK** per salvare le modifiche e tornare alla scheda **sicurezza** .
11. Selezionare **OK** per eseguire il commit delle modifiche.
12. Riavviare il servizio SQL Server Riesecuzione distribuita controller da Services. msc. È anche possibile eseguire i comandi seguenti dalla riga di comando per riavviare il servizio:

    `NET STOP "SQL Server Distributed Replay Controller"`</br>
    `NET START "SQL Server Distributed Replay Controller"`

## <a name="set-up-the-client-service"></a>Configurare il servizio client

Prima di configurare il servizio client, utilizzare strumenti di rete come ping per verificare che il controller e i computer client possano comunicare.

1. Installare il client di Riesecuzione distribuita usando il programma di installazione di SQL Server.
2. Aprire Services. msc e passare alla SQL Server Riesecuzione distribuita servizio client.
3. Fare clic con il pulsante destro del mouse sul servizio, quindi scegliere **Proprietà**. Impostare l'account del servizio su un account comune per il controller e i computer client nella rete.
4. Selezionare **OK** per chiudere la finestra delle **Proprietà** . Se è stato ignorato il passaggio del programma di installazione guidata di SQL Server per configurare il client di Riesecuzione distribuita, è possibile configurarlo tramite il file di configurazione. In un'installazione tipica, il file di configurazione si trova in C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayClient\DReplayClient.config.
5. Verificare che il file DReplayClient.config contenga il nome del computer del controller come controller per la registrazione.
6. Riavviare il servizio client Riesecuzione distribuita di SQL Server da Services. msc. È anche possibile eseguire i comandi seguenti dalla riga di comando per riavviare il servizio:

    `NET STOP "SQL Server Distributed Replay Client"`</br>
    `NET START "SQL Server Distributed Replay Client"`

    I log del controller di Riesecuzione distribuita si trovano in C:\Programmi (x86) \Microsoft SQL Server \<version\> \Tools\DReplayClient\Log. I log indicano se il client può registrarsi con il controller.

    Se la configurazione ha esito positivo, nel log viene visualizzato il messaggio **registrato con il \> controller <nome del controller**.

Per altre opzioni di configurazione, vedere [Configure riesecuzione distribuita](../tools/distributed-replay/configure-distributed-replay.md).

## <a name="set-up-distributed-replay-administration-tools"></a>Configurare Riesecuzione distribuita strumenti di amministrazione

È possibile utilizzare Riesecuzione distribuita strumenti di amministrazione per verificare rapidamente se Riesecuzione distribuita funziona correttamente nell'ambiente. Il test della configurazione può essere particolarmente utile in un ambiente in cui più computer client vengono registrati con un controller. Potrebbe essere necessario installare SQL Server Management Studio (SSMS) per ottenere gli strumenti di amministrazione.

1. Passare al percorso di installazione di SSMS e cercare lo strumento di amministrazione Riesecuzione distribuita dreplay.exe e i componenti dipendenti. Attualmente, [SSMS 17](../ssms/release-notes-ssms.md#1791) è la versione più recente di SSMS per includere dreplay.exe.
2. Al prompt dei comandi eseguire `dreplay.exe status -f 1` .

Se i passaggi precedenti hanno avuto esito positivo, l'output della console indica che il controller può visualizzare i client in uno `READY` stato.

## <a name="configure-the-firewall-for-remote-distributed-replay-access"></a>Configurare il firewall per l'accesso Riesecuzione distribuita remoto

Per accedere in remoto a Riesecuzione distribuita è necessario aprire le porte visibili all'interno del dominio o della rete virtuale.

1. Aprire **Windows Firewall** con **sicurezza avanzata**.
2. Passare a **Regole connessioni in ingresso**.
3. Creare una nuova regola del firewall in ingresso per il programma C:\Program Files (x86) \Microsoft SQL Server \<version\>\Tools\DReplayController\DReplayController.exe.
4. Consente l'accesso a livello di dominio a tutte le porte affinché DReplayController.exe sia in grado di comunicare con il servizio controller in modalità remota.
5. Salvare la regola.

## <a name="set-up-target-computers"></a>Configurare i computer di destinazione

Per eseguire un test A/B o un esperimento sono necessarie due riesecuzioni. In altre circostanze, potrebbero essere necessarie due istanze separate di SQL Server installazioni per uno scenario di migrazione.

È anche possibile installare le due versioni di SQL Server istanze nello stesso computer. Un avvertimento consiste nel verificare che le istanze siano isolate quando è in corso una riproduzione.

Per ogni riproduzione è necessario eseguire i passaggi seguenti:

1. Ripristinare il backup del database.
2. Fornire le autorizzazioni per consentire all'utente dell'account del servizio client di accedere ai database nell'istanza di SQL Server. Le autorizzazioni sono necessarie per l'esecuzione delle query nell'istanza di SQL Server.
3. Avviare la riproduzione.

## <a name="see-also"></a>Vedi anche

- Per informazioni su come riprodurre una traccia acquisita in un ambiente di testing aggiornato, vedere [riprodurre una traccia in database Experimentation Assistant](database-experimentation-assistant-replay-trace.md).
