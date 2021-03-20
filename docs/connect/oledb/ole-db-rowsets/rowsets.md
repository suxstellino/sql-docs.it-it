---
title: Set di righe (OLE DB Driver)
description: Informazioni sulle interfacce che supportano un consumer nella creazione di un set di righe in una sessione in OLE DB Driver per SQL Server. Per informazioni dettagliate, vedere gli articoli in questa sezione.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- OLE DB Driver for SQL Server, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e41444c601eac8829ce502e46aa96b784df8b799
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746081"
---
# <a name="rowsets"></a>Set di righe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Le righe di un set di righe contengono colonne di dati. I set di righe sono oggetti centrali che consentono a tutti i provider di dati OLE DB di esporre dati di set di risultati in formato tabulare.  
  
 Dopo aver creato una sessione mediante il metodo **IDBCreateSession::CreateSession**, il consumer può usare l'interfaccia **IOpenRowset** o **IDBCreateCommand** nella sessione per creare un set di righe. OLE DB Driver per SQL Server supporta entrambe le interfacce. Di seguito sono descritti i due metodi.  
  
-   Creare un set di righe chiamando il metodo **IOpenRowset::OpenRowset**.  
  
     Questo metodo equivale a chiamare un set di righe in una singola tabella e consente di aprire e restituire un set di righe che include tutte le righe di una singola tabella di base. Uno degli argomenti di **OpenRowset** è un ID di tabella che identifica la tabella dalla quale creare il set di righe.  
  
-   Creare un oggetto comando chiamando il metodo **IDBCreateCommand::CreateCommand**.  
  
     L'oggetto comando esegue comandi supportati dal provider. Con il driver OLE DB per SQL Server, il consumer può specificare qualsiasi istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)], ad esempio un'istruzione SELECT o una chiamata a una stored procedure. Di seguito sono elencati i passaggi per la creazione di un set di righe tramite un oggetto comando:  
  
    1.  Il consumer chiama il metodo **IDBCreateCommand::CreateCommand** nella sessione per ottenere un oggetto comando che richieda l'interfaccia **ICommandText** sull'oggetto stesso. Questa interfaccia **ICommandText** imposta e recupera il testo del comando effettivo. Il consumer inserisce il comando di testo chiamando il metodo **ICommandText::SetCommandText**.  
  
    2.  L'utente chiama il metodo **ICommand::Execute** sul comando. L'oggetto set di righe compilato durante l'esecuzione del comando contiene il set di risultati restituito dal comando.  
  
 Nel consumer è possibile usare l'interfaccia **ICommandProperties** per ottenere o impostare le proprietà per il set di righe restituito dal comando eseguito dalle interfacce **ICommand::Execute**. Le proprietà generalmente più richieste sono le interfacce che il set di righe deve supportare. Oltre alle interfacce, il consumer può richiedere proprietà che modificano il comportamento del set di righe o dell'interfaccia.  
  
 I consumer rilasciano set di righe con il metodo **IRowset::Release**. Il rilascio di un set di righe comporta anche il rilascio di tutti gli handle di riga gestiti dal consumer per tale set di righe, ma non comporta il rilascio delle funzioni di accesso. Se si ha un'interfaccia **IAccessor**, questa deve essere comunque rilasciata.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Creazione di un set di righe con IOpenRowset](../../oledb/ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [Creazione di set di righe con ICommand::Execute](../../oledb/ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [Proprietà e comportamenti dei set di righe](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [Set di righe e cursori SQL Server](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [Recupero di righe](../../oledb/ole-db-rowsets/fetching-rows.md)  
  
-   [Recupero di una sola riga con IRow](../../oledb/ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [Segnalibri](../../oledb/ole-db-rowsets/bookmarks.md)  
  
-   [Aggiornamento dei dati nei set di righe](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
