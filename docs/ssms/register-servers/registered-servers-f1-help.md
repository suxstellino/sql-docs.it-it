---
description: Guida sensibile al contesto di Server registrati
title: Guida sensibile al contesto di Server registrati
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], Help
- Registered Servers [SQL Server], help
- SQL Server Management Studio Help [SQL Server], registered servers
ms.assetid: 59f76b28-ba78-4a1a-b5d5-8b581f30114d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d4b3a2e75a00e3688ec53c5e64e6ab7727375a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341045"
---
# <a name="registered-servers-f1-help"></a>Guida sensibile al contesto di Server registrati
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Questa sezione contiene gli argomenti della Guida sensibile al contesto per il componente Server registrati di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Descrive le varie opzioni.
  
 Per informazioni su Server registrati e collegamenti alle operazioni correlate, vedere l'argomento [Registrazione di server](./register-servers.md) . 
 

 Fare clic su questo pulsante per salvare le impostazioni relative ai server registrati. 
 
 ## <a name="reporting-services-new-or-edit-server-registration-general-tab"></a>Nuova registrazione server o Modifica proprietà registrazione server in Reporting Services (scheda Generale) 
  Utilizzare questa scheda per specificare le opzioni per la registrazione di un'istanza di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Per accedere a questa pagina, in Server registrati fare clic su **Reporting Services** sulla barra degli strumenti **Server registrati** , fare clic con il pulsante destro del mouse su qualsiasi gruppo di server registrati, ad esempio **Reporting Services**, scegliere **Nuovo** e quindi fare clic su **Nuova registrazione server**.  
  
### <a name="options"></a>Opzioni  
 **Tipo di server**  
 Quando si esegue la registrazione di un server da Server registrati, la casella **Tipo server** è di sola lettura e corrisponde al tipo di server visualizzato nel riquadro **Server registrati** . Per registrare un tipo diverso di server, selezionare il server desiderato dalla barra degli strumenti **Server registrati** prima di iniziare la registrazione del nuovo server.  
  
 **Nome server**  
 Consente di selezionare l'istanza del server di report a cui connettersi. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]è possibile accedere a un server di report tramite il nome della relativa istanza. È possibile disporre di un'istanza del server di report per ogni istanza di SQL Server installata Se si utilizza l'istanza predefinita, digitare il nome dell'istanza di SQL Server. Se si utilizza un'istanza denominata, indicarla nel formato MSSQL$instancename per la connessione al server di report.  
  
 **autenticazione**  
 Il processo di autenticazione a un server di report viene eseguito tramite Internet Information Services (IIS). Selezionare una delle modalità di autenticazione disponibili seguenti per la connessione a Reporting Services:  
  
 **Modalità di autenticazione di Windows (Autenticazione di Windows)**  
 Consente di connettersi a un'istanza del server di report tramite le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Autenticazione base**  
 Consente di connettersi mediante la modalità di **Autenticazione di base** se l'installazione di Reporting Services è configurata per usarla.  
  
 **Autenticazione basata su form**  
 Consente di connettersi mediante la modalità **Autenticazione basata su form** se l'installazione di Reporting Services è configurata per usare un'estensione di autenticazione personalizzata.  
  
 **Nome utente**  
 Immettere il nome account di accesso da utilizzare per la connessione. Questa opzione è disponibile solo se si è scelto di utilizzare **l'autenticazione di base** o **basata su form**.  
  
 **Password**  
 Immettere il nome utente e la password Questa opzione può essere modificata solo se si è scelto di utilizzare **l'autenticazione di base** o **basata su form**.  
  
 **Memorizza password**  
 Consente di archiviare la password immessa. Questa opzione è disponibile solo se si è scelto di utilizzare **l'autenticazione di base** o **basata su form**.  
  
> **NOTA:** Se la password è stata memorizzata e si vuole evitarne la memorizzazione in futuro, deselezionare questa casella di controllo e fare clic su **Salva**.  
  
 **Nome server registrato**  
 Nome che si desidera venga visualizzato nel componente Server registrati. Non è necessario che questo nome corrisponda a quello indicato nella casella **Nome server** .  
  
 **Descrizione server registrato**  
 Consente di immettere una descrizione facoltativa del server.  
  
 **Test**  
 Consente di testare la connessione al server selezionato in **Nome server**.  
  
 
 ## <a name="analysis-services---multidimensional-data-new-or-edit-server-registration-general-tab"></a>Nuova registrazione server o Modifica proprietà registrazione server in Analysis Services - Dati multidimensionali (scheda Generale)
 
  Utilizzare questa scheda per specificare le opzioni per la registrazione di un'istanza di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Per accedere a questa pagina, in Server registrati fare clic su **Analysis Services** sulla barra degli strumenti Server registrati, fare clic con il pulsante destro del mouse su un gruppo di server registrati, ad esempio **Analysis Services**, scegliere **Nuovo** e quindi fare clic su **Registrazione server**.  
  
### <a name="options"></a>Opzioni  
 **Tipo di server**  
 Quando si esegue la registrazione di un server da Server registrati, la casella **Tipo server** è di sola lettura e corrisponde al tipo di server visualizzato nel riquadro Server registrati. Per registrare un tipo diverso di server, selezionare il server desiderato dalla barra degli strumenti **Server registrati** prima di iniziare la registrazione del nuovo server.  
  
 **Nome server**  
 Consente di selezionare l'istanza del server a cui connettersi. Per impostazione predefinita verrà visualizzata l'ultima istanza del server a cui è stata effettuata la connessione.  
  
 **autenticazione**  
 L'autenticazione di Windows consente a un utente di connettersi utilizzando le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, come utente di Windows o come membro di un gruppo di Windows.  
  
 **Nome utente**  
 Questa opzione non è disponibile in questa versione.  
  
 **Password**  
 Questa opzione non è disponibile in questa versione.  
  
 **Memorizza password**  
 Questa opzione non è disponibile in questa versione.  
  
 **Nome server registrato**  
 Nome che si desidera venga visualizzato nel componente Server registrati. Non è necessario che questo nome corrisponda a quello indicato nella casella **Nome server** .  
  
 **Descrizione server registrato**  
 Consente di immettere una descrizione facoltativa del server.  
  
 **Test**  
 Consente di testare la connessione al server selezionato in **Nome server**. 
 
 ## <a name="ssis-new-or-edit-server-registration-general-tab"></a>Nuova registrazione server o Modifica proprietà registrazione server in SSIS (scheda Generale) 
 
 Utilizzare questa scheda per specificare le opzioni di registrazione di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Per accedere a questa pagina, in Server registrati fare clic su **Integration Services** sulla barra degli strumenti **Server registrati** , fare clic con il pulsante destro del mouse su qualsiasi gruppo di server registrati, scegliere **Nuovo** e quindi fare clic su **Registrazione server**.  
  
### <a name="options"></a>Opzioni  
 **Tipo di server**  
 Quando si registra un server da Server registrati, la casella **Tipo server** è di sola lettura e corrisponde al tipo di server visualizzato in Server registrati. Per registrare un tipo diverso di server, fare clic su **Motore di database**, **Analysis Server**, **Reporting Services**, **SQL Server Compact** **Edition** o **Integration Services** sulla barra degli strumenti **Server registrati** prima di avviare la registrazione di un nuovo server.  
  
 **Nome server**  
 Consente di selezionare il server a cui connettersi. Viene visualizzato per impostazione predefinita l'ultimo server a cui ci si è connessi.  
  
 **autenticazione**  
 La modalità di autenticazione di Windows consente all'utente di utilizzare un account utente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows per la connessione.  
  
 **Nome utente**  
 Questa opzione non è disponibile in questa versione.  
  
 **Password**  
 Questa opzione non è disponibile in questa versione.  
  
 **Memorizza password**  
 Questa opzione non è disponibile in questa versione.  
  
 **Nome server registrato**  
 Nome che si vuole visualizzare nel componente **Server registrati**. Non è necessario che questo nome corrisponda a quello indicato nella casella **Nome server** .  
  
 **Descrizione server registrato**  
 Consente di immettere una descrizione facoltativa del server.  
  
 **Test**  
 Consente di testare la connessione al server selezionato in **Nome server**. 
  

 
 
