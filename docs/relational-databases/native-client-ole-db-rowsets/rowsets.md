---
title: Set di righe (provider OLE DB Native Client)
description: Abilitare tutti i provider di dati per esporre i dati del set di risultati in formato tabulare. In OLE DB utilizzare la funzionalità set di righe, ovvero un set di righe contenente colonne di dati.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
ms.assetid: 5e7b3cbe-3670-4e18-8172-2226e0b6b142
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b149038a7129b2327156cc28967520f6386d1b6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755881"
---
# <a name="rowsets-native-client-ole-db-provider"></a>Set di righe (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le righe di un set di righe contengono colonne di dati. I set di righe sono oggetti centrali che consentono a tutti i provider di dati OLE DB di esporre dati di set di risultati in formato tabulare.  
  
 Dopo aver creato una sessione mediante il metodo **IDBCreateSession::CreateSession**, il consumer può usare l'interfaccia **IOpenRowset** o **IDBCreateCommand** nella sessione per creare un set di righe. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client supporta entrambe le interfacce. Di seguito sono descritti i due metodi.  
  
-   Creare un set di righe chiamando il metodo **IOpenRowset::OpenRowset**.  
  
     Questo metodo equivale a chiamare un set di righe in una singola tabella e consente di aprire e restituire un set di righe che include tutte le righe di una singola tabella di base. Uno degli argomenti di **OpenRowset** è un ID di tabella che identifica la tabella dalla quale creare il set di righe.  
  
-   Creare un oggetto comando chiamando il metodo **IDBCreateCommand::CreateCommand**.  
  
     L'oggetto comando esegue comandi supportati dal provider. Con il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, il consumer può specificare qualsiasi istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio un'istruzione SELECT o una chiamata a una stored procedure. Di seguito sono elencati i passaggi per la creazione di un set di righe tramite un oggetto comando:  
  
    1.  Il consumer chiama il metodo **IDBCreateCommand::CreateCommand** nella sessione per ottenere un oggetto comando che richieda l'interfaccia **ICommandText** sull'oggetto stesso. Questa interfaccia **ICommandText** imposta e recupera il testo del comando effettivo. Il consumer inserisce il comando di testo chiamando il metodo **ICommandText::SetCommandText**.  
  
    2.  L'utente chiama il metodo **ICommand::Execute** sul comando. L'oggetto set di righe compilato durante l'esecuzione del comando contiene il set di risultati restituito dal comando.  
  
 Nel consumer è possibile usare l'interfaccia **ICommandProperties** per ottenere o impostare le proprietà per il set di righe restituito dal comando eseguito dalle interfacce **ICommand::Execute**. Le proprietà generalmente più richieste sono le interfacce che il set di righe deve supportare. Oltre alle interfacce, il consumer può richiedere proprietà che modificano il comportamento del set di righe o dell'interfaccia.  
  
 I consumer rilasciano set di righe con il metodo **IRowset::Release**. Il rilascio di un set di righe comporta anche il rilascio di tutti gli handle di riga gestiti dal consumer per tale set di righe, ma non comporta il rilascio delle funzioni di accesso. Se si ha un'interfaccia **IAccessor**, questa deve essere comunque rilasciata.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Creazione di un set di righe con IOpenRowset](../../relational-databases/native-client-ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [Creazione di set di righe con ICommand::Execute](../../relational-databases/native-client-ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [Proprietà e comportamenti dei set di righe](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [Set di righe e cursori SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [Recupero di righe](../../relational-databases/native-client-ole-db-rowsets/fetching-rows.md)  
  
-   [Recupero di una sola riga con IRow](../../relational-databases/native-client-ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [Segnalibri](../../relational-databases/native-client-ole-db-rowsets/bookmarks.md)  
  
-   [Aggiornamento dei dati nei set di righe](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
