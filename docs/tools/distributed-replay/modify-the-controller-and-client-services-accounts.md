---
title: Modificare gli account dei servizi controller e client
titleSuffix: SQL Server Distributed Replay
description: Informazioni su come modificare gli account del servizio client e del controller di Riesecuzione distribuita e quindi come riapplicare gli elenchi di controllo di accesso.
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 44a73ddb-18ad-415c-bfbe-126ab2e3290b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 22d126dae4356bd6830c801369375b8705e37b37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349526"
---
# <a name="modify-the-controller-and-client-services-accounts"></a>Modificare gli account dei servizi controller e client

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento verrà illustrato come modificare gli account dei servizi client e controller di Riesecuzione distribuita e quindi riapplicare gli elenchi di controllo di accesso (ACL).  
  
### <a name="to-start-or-stop-the-distributed-replay-services-using-computer-management"></a>Per avviare o arrestare i servizi client Riesecuzione distribuita tramite Gestione computer  
  
1.  Nel computer in cui sono installati i servizi di ridistribuzione distribuita digitare **dcomcnfg** al prompt dei comandi.  
  
2.  Fare doppio clic su **Servizi**, scorrere verso il basso e fare clic con il pulsante destro del mouse su Riesecuzione distribuita **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] \<service name>** , quindi scegliere **Avvia** o **Arresta**.  
  
### <a name="to-modify-the-distributed-replay-controller-service"></a>Per modificare il servizio controller di Riesecuzione distribuita  
  
1.  Nel computer controller arrestare il servizio controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  In **Servizi** fare clic con il pulsante destro del mouse su **Controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , quindi scegliere **Proprietà**.  
  
3.  Nella scheda **Accesso** della finestra **Proprietà di Controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** selezionare **Account seguente**, digitare il nuovo account di accesso o fare clic su **Sfoglia** per selezionarlo, quindi fare clic su **OK**.  
  
     **Importante**: quando si configura il controller di Riesecuzione distribuita, è possibile specificare uno o più account utente da usare per eseguire i servizi client di Riesecuzione distribuita. Di seguito viene fornito l'elenco degli account supportati:  
  
    -   Account utente di dominio  
  
    -   Account utente locale creato dall'utente  
  
    -   Amministratore  
  
    -   Account virtuale e account del servizio gestito  
  
    -   Servizi di rete, Servizi locali e Sistema  
  
     Gli account di gruppo (locale o dominio) e gli altri account predefiniti (come Everyone) non sono accettati.  
  
4.  Avviare il servizio controller di Riesecuzione distribuita.  
  
### <a name="to-modify-the-distributed-replay-client-service"></a>Per modificare il servizio client Riesecuzione distribuita  
  
1.  Prima di modificare il servizio client Riesecuzione distribuita, verificare che l'account del servizio client da modificare sia stato specificato durante l'installazione, nel parametro CTLRUSERS del computer controller. Se l'account del servizio client da modificare non è stato specificato durante l'installazione, è necessario effettuare innanzitutto i passaggi seguenti:  
  
    1.  Arrestare il servizio controller di Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    2.  Nel computer controller in cui è installato il servizio controller, digitare **dcomcnfg** al prompt dei comandi.  
  
    3.  Nella finestra **Servizi componenti** passare a **Radice console -> Servizi componenti -> Computer -> Risorse del computer -> Config DCOM ->DReplayController**.  
  
    4.  Fare clic con il pulsante destro del mouse su **DReplayController**, quindi scegliere **Proprietà**.  
  
    5.  Nella scheda **Sicurezza** nella finestra **Proprietà di DReplayController** fare clic su **Modifica** nella sezione **Autorizzazioni di esecuzione e attivazione** .  
  
    6.  Concedere al nuovo account di accesso del servizio client le autorizzazioni **Attivazione locale e Attivazione remota** , quindi fare clic su **OK**.  
  
    7.  Fare clic su **Modifica** nella sezione **Autorizzazioni di accesso** e concedere al nuovo account di accesso del servizio client le autorizzazioni **Accesso locale e Accesso remoto** , quindi fare clic su **OK**.  
  
    8.  Fare clic su **OK** per chiudere la finestra **Proprietà di DReplayController** .  
  
    9. Nel computer controller aggiungere l'account di accesso del servizio client modificato al gruppo **Distributed COM Users** .  
  
    10. Avviare il servizio SQL Server Distributed Replay Controller.  
  
2.  Arrestare il servizio client Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Nella scheda **Accesso** della finestra **Proprietà di Client Riesecuzione distribuita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** scegliere **Account seguente**, digitare il nuovo account di accesso o fare clic su **Sfoglia** per selezionarlo e quindi fare clic su **OK**.  
  
4.  Avviare il servizio client Riesecuzione distribuita.  
  
  
