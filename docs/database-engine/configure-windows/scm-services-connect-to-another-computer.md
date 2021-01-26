---
title: Connettersi a un altro computer (Gestione configurazione SQL Server) | Microsoft Docs
description: Informazioni su come gestire i servizi di un computer remoto. Scoprire come usare Gestione configurazione SQL Server o SQL Server Management Studio per questa attività.
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], other computers
ms.assetid: c4c1e94f-4f5f-431e-8b5b-d5ff97baf723
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b178e38f6e59499ddf74e9a8aaec037bbaa4d442
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783439"
---
# <a name="scm-services---connect-to-another-computer"></a>Connettersi a un altro computer (Gestione configurazione SQL Server)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo illustra come connettersi a un altro computer in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Attenersi alla prima procedura per aprire Gestione computer di Windows, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC), connettersi al computer ed espandere l'albero Servizi e applicazioni. Seguire la seconda procedura per creare un file con un collegamento a Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer remoto.

> [!NOTE]
> Alcune azioni non possono essere eseguite da Configuration Manager quando si effettua la connessione in remoto.

Per avviare, arrestare, sospendere o riprendere i servizi in un altro computer, è inoltre possibile connettersi al server tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], fare clic con il pulsante destro del mouse sul server o su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e quindi scegliere l'operazione desiderata.

## <a name="SSMSProcedure"></a>

### <a name="to-connect-to-another-computer-with-windows-computer-management"></a>Per connettersi a un altro computer utilizzando Gestione computer di Windows

1. Fare clic con il pulsante destro del mouse sul pulsante del menu **Start** e fare clic su **Gestione computer (locale)** .
2. Nel menu **Azione** fare clic su **Connetti a un altro computer**.
3. Nella finestra di dialogo **Seleziona computer** digitare il nome del computer che si vuole gestire nella casella di testo **Altro computer** quindi fare clic su **OK**.

   Gestione computer visualizza i servizi in esecuzione nel computer remoto. Il nodo di primo livello diventa **Gestione computer** \<*remotecomputer*>.

4. Nell'albero della console espandere **Servizi e applicazioni** e quindi espandere **Gestione configurazione SQL Server** per gestire i servizi del computer remoto.

### <a name="to-save-a-link-to-sql-server-configuration-manager-for-another-computer"></a>Per salvare un collegamento a Gestione configurazione SQL Server da un altro computer

1. Fare clic sul menu **Start** e scegliere **Esegui**.

2. Nella casella **Apri** digitare **mmc -a** (digitare **mmc /32 -a** in un computer a 64 bit) per aprire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console in modalità di modifica.
3. Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.
4. Nella finestra **Aggiungi/Rimuovi snap-in** fare clic su **Aggiungi**.
5. Nella finestra **Aggiungi snap-in autonomo** fare clic su **Gestione computer** e quindi su **Aggiungi**.
6. Nella finestra **Gestione computer** fare clic su **Altro computer**, digitare il nome del computer remoto da gestire e quindi fare clic su **Fine**.
7. Nella finestra **Aggiungi snap-in autonomo** fare clic su **Chiudi**.
8. Nella finestra **Aggiungi/Rimuovi snap-in** fare clic su **OK**.
9. Espandere **Gestione computer (** _\<computer name>_ **)** e **Servizi e applicazioni**.
10. Fare clic con il pulsante destro del mouse su **Gestione configurazione SQL Server** e quindi scegliere **Nuova finestra da qui**.
11. Scegliere **Radice console** dal menu **Finestra** per tornare alla prima finestra ed eliminare la finestra aperta.
12. Scegliere **Salva con nome** dal menu **File** e salvare il file nella cartella desiderata con un nome appropriato e l'estensione di file **msc** . Chiudere [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console.
13. Per aprire Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer di destinazione, fare doppio clic sul file. È possibile salvare un collegamento al file sul desktop o nel menu **Start** .

> [!CAUTION]
> Quando si utilizza Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer remoto, il nome del computer non è facilmente individuabile ed è possibile che venga arrestato o configurato il computer errato. Nella scheda **Servizio** selezionare la casella **Nome host** per verificare il nome del computer prima di modificare un servizio.

## <a name="see-also"></a>Vedere anche

- [Configurazione di WMI per mostrare lo stato del server in Strumenti SQL Server](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)
