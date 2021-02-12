---
title: Estrarre, pubblicare e registrare file con estensione dacpac
description: Informazioni sulle azioni che è possibile eseguire con le applicazioni livello dati. Ad esempio, l'estrazione, la pubblicazione e la registrazione di file snapshot (con estensione dacpac).
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DacTableChooser
- sql.data.tools.DacPublishDialog
- sql.data.tools.DacPropertiesDialog
- sql.data.tools.publishdacproject
- sql.data.tools.DacExtractDialog
ms.assetid: ed900f93-d3df-40f5-8e62-4d722595e041
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7d63d6b26d8f5f07b6a4344e84f3d490012c91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077058"
---
# <a name="extract-publish-and-register-dacpac-files"></a>Estrarre, pubblicare e registrare file con estensione dacpac

Questo argomento descrive quattro procedure eseguibili facendo clic con il pulsante destro del mouse su un database connesso in Esplora oggetti di SQL Server:  
  
-   Pubblicare l'applicazione livello dati  
  
-   Estrarre l'applicazione livello dati  
  
-   Registrare l'applicazione livello dati  
  
-   Annullare la registrazione dell'applicazione livello dati  
  
## <a name="publish-data-tier-application"></a>Pubblicare l'applicazione livello dati  
È possibile pubblicare un file con estensione dacpac in un database. Tramite l'azione di pubblicazione viene aggiornato in modo incrementale uno schema del database affinché corrisponda allo schema di un file di origine con estensione dacpac. Se il database non esiste nel server, viene creato durante l'operazione di pubblicazione.  
  
Questa azione è disponibile anche selezionando il nodo Database.  
  
Selezionare il file con estensione dacpac. Dopo aver specificato un file con estensione dacpac, viene abilitato il pulsante **Proprietà DAC**. Nella finestra di dialogo **Proprietà DAC** vengono visualizzate le informazioni sul file con estensione dacpac.  
  
Specificare la stringa di connessione al server di database e, successivamente, il nome del database, se non incluso nella stringa di connessione.  
  
A questo punto i pulsanti **Pubblica** e **Genera script** sono abilitati. Se viene generato uno script, questo viene visualizzato in una finestra del documento e può essere salvato in base alle esigenze. Se si sceglie di pubblicare direttamente nel database, è possibile visualizzare un riepilogo dell'aggiornamento e dello script effettivo dalla finestra **Operazioni con strumenti dati**.  
  
Se è selezionata la casella di controllo **Registra come applicazione livello dati**, viene eseguita la registrazione del database risultante come applicazione livello dati nei metadati del server. Se il database in cui si sta eseguendo la pubblicazione viene registrato, è possibile che l'operazione di pubblicazione non venga completata se lo schema del database è diverso dal relativo file dacpac corrente registrato.  
  
La configurazione di pubblicazione aggiuntiva è disponibile nella finestra di dialogo **Impostazioni di pubblicazione avanzate** a cui è possibile accedere facendo clic sul pulsante **Avanzate**.  
  
## <a name="extract-data-tier-application"></a>Estrarre l'applicazione livello dati  
È possibile estrarre un file con estensione dacpac da un database. L'operazione di estrazione crea un file di snapshot del database (DACPAC) da un database SQL di Azure o SQL Server attivo, che potrebbe contenere dati di tabelle utente, oltre allo schema del database.  
  
Specificare il file con estensione dacpac da creare. Il pulsante **Proprietà DAC** visualizza la finestra di dialogo **Proprietà DAC** che consente di specificare le proprietà del file con estensione dacpac.  
  
Modificare, in base alle esigenze, la configurazione del processo di estrazione. Nella sezione **Impostazioni di estrazione** è possibile scegliere di eseguire solo l'estrazione dello schema oppure, oltre a questa operazione, di includere anche i dati della tabella. Se si sceglie di estrarre lo schema e i dati, è possibile selezionare le tabelle per le quali si desidera eseguire l'estrazione dei dati.  
  
## <a name="register-data-tier-application"></a>Registrare l'applicazione livello dati  
È possibile registrare un database come applicazione livello dati (DAC) nell'istanza. In questo modo viene archiviata una rappresentazione dello stato corrente dello schema del database nei metadati di sistema.  
  
Nella finestra di dialogo **Registra applicazione livello dati** specificare le proprietà dell'applicazione livello dati registrata.  
  
## <a name="unregister-data-tier-application"></a>Annullare la registrazione dell'applicazione livello dati  
Tramite l'annullamento della registrazione è possibile rimuovere i metadata di un'applicazione livello dati registrata dall'istanza. L'annullamento della registrazione non comporta l'eliminazione del database registrato.  
  
## <a name="see-also"></a>Vedere anche  
[Sviluppo del database connesso](../ssdt/connected-database-development.md)  
  
