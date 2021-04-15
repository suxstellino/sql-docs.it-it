---
title: Considerazioni sulla sicurezza del caricamento bulk (SQLXML)
description: Informazioni sulle linee guida sulla sicurezza per l'uso del caricamento bulk XML in SQLXML 4.0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- bulk load [SQLXML], security
- security [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], security
ms.assetid: 192fc6d4-ecbc-4a4d-a5cb-55e1f64af318
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c9e1669f2bb6a4b410e19f036797fd8cb5197170
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491628"
---
# <a name="bulk-load-security-considerations-sqlxml-40"></a>Considerazioni sulla sicurezza del caricamento bulk (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Di seguito sono riportate alcune linee guida relative alla sicurezza quando si utilizza il caricamento bulk XML:  
  
-   Quando si specifica che l'operazione di caricamento bulk deve essere eseguita come transazione, usare la proprietà **TempFilePath** per specificare una cartella in cui creare i file temporanei.  
  
     Il processo di caricamento bulk crea questi file temporanei con le autorizzazioni seguenti:  
  
    -   Al processo di caricamento bulk vengono concessi gli accessi di tipo Lettura/Scrittura/Eliminazione.  
  
    -   L'autorizzazione Lettura viene concessa a tutti gli utenti, in quanto l'account con cui Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] accederà a tali file è sconosciuto. È possibile limitare l'accesso a questi file temporanei impostando le autorizzazioni appropriate sulla cartella che li contiene.  
  
-   Il caricamento bulk XML non include impostazioni specifiche per le autorizzazioni. Si presuppone che il database sia impostato correttamente e che per il contesto dell'utente, ovvero l'account di accesso utilizzato per il caricamento bulk, siano state impostate le autorizzazioni appropriate.  
  
-   Se si verifica un errore durante il processo di caricamento bulk in modalità non transazionale, è possibile che i dati restino in uno stato di caricamento parziale, ovvero il caricamento bulk si arresta nel punto in cui si trova. Per risolvere questo problema è possibile utilizzare la modalità transazionale.  
  
-   Se vengono visualizzati messaggi di errore relativi al caricamento bulk, è possibile che includano informazioni sul database, ad esempio il nome di una tabella o di una colonna o la specifica del tipo di colonna. Quando si utilizza il caricamento bulk, è necessario prestare particolare attenzione agli errori generati dal processo di caricamento bulk e restituire un messaggio di errore generico anziché esporre gli errori direttamente agli utenti.  
  
-   Per il caricamento bulk non è previsto alcun limite sulla quantità di dati da gestire né alcun controllo sulla dimensione dei dati da caricare. È responsabilità dell'utente eseguire il caricamento bulk per garantire che la memoria disponibile sia sufficiente per elaborare il file specificato e che lo spazio disponibile nel database sia sufficiente per archiviare i dati caricati.  
  
-   Durante il caricamento bulk non viene effettuato alcun tentativo di utilizzo dei dati forniti come codice. I dati di input non vengono mai eseguiti e gli eventuali comandi o frammenti di codice presenti in tali dati vengono considerati come dati normali e pertanto non vengono eseguiti.  
  
-   Il caricamento bulk consente di apportare modifiche relative alla formattazione ai dati specificati in base alle differenze tra i modelli di dati XML e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il formato per la specifica dell'ora è ad esempio diverso. Poiché durante il caricamento bulk si tenterà di risolvere queste differenze, si potrebbero perdere alcune informazioni sulla precisione.  
  
-   Il caricamento bulk non prevede l'impostazione di alcun limite sulla quantità di tempo necessaria per l'elaborazione dei dati. L'elaborazione continuerà finché non sarà stata completata o non si verificherà un errore.  
  
-   Il caricamento bulk consente di creare ed eliminare tabelle temporanee all'interno del database ma a tale scopo è necessario disporre di autorizzazioni specifiche. Le autorizzazioni per queste tabelle verranno concesse allo stesso utente che si sta connettendo al database per il processo di caricamento bulk.  
  
-   Il caricamento bulk consente di creare ed eliminare i file temporanei utilizzati durante l'elaborazione in modalità transazionale ma a tale scopo è necessario disporre di autorizzazioni specifiche. Questi file vengono creati con le stesse autorizzazioni di cui dispone l'utente corrente del thread in cui è in esecuzione il caricamento bulk.  
  
-   Se l'utente imposta un file di log degli errori in cui scrivere gli errori relativi a SQLXML, a ogni esecuzione del caricamento bulk il file verrà sovrascritto con i dati dell'ultimo processo di caricamento bulk.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione del caricamento bulk di dati XML &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
