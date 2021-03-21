---
title: BLOB e oggetti OLE (OLE DB Driver) | Microsoft Docs
description: Informazioni sul modo in cui l'interfaccia ISequentialStream supporta l'accesso del consumer ai tipi di dati SQL Server come oggetti binari di grandi dimensioni in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- BLOBs
- storage object [OLE DB]
- OLE DB Driver for SQL Server, BLOBs
- large data, OLE objects
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80c1c1eff66c19b009dcfdd9164129734f6c4362
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749181"
---
# <a name="blobs-and-ole-objects"></a>Oggetti BLOB e OLE
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server espone l'interfaccia **ISequentialStream** per supportare l'accesso del consumer ai tipi di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **ntext**, **text**<a href="#text_note"><sup>**1**</sup></a>, **image**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** e xml come oggetti binari di grandi dimensioni (BLOB). Il metodo **Read** in **ISequentialStream** consente al consumer di recuperare una quantità elevata di dati in blocchi gestibili.

 <b id="text_note">[1]:</b> L'uso dell'interfaccia ISequentialStream per l'inserimento di dati con codifica UTF-8 in una colonna di testo legacy è limitata solo ai server che supportano UTF-8. Un tentativo di eseguire questo scenario quando la destinazione è un server che non supporta UTF-8 comporterà la visualizzazione del messaggio di errore seguente nel driver: "*Flusso non supportato per il tipo di colonna selezionato*".

 Per un esempio che illustra questa funzionalità, vedere [Impostare dati di grandi dimensioni &#40;OLE DB&#41;](../../oledb/ole-db-how-to/set-large-data-ole-db.md).  
  
 Il driver OLE DB per SQL Server può usare un'interfaccia **IStorage** implementata dal consumer quando quest'ultimo fornisce il puntatore di interfaccia in una funzione di accesso associata per la modifica dei dati.  
  
 Nel caso di tipi di dati per valori di grandi dimensioni, il driver OLE DB per SQL Server verifica i presupposti relativi alle dimensioni dei tipi nelle interfacce **IRowset** e DDL. Le colonne che hanno tipi di dati **varchar**, **nvarchar** e **varbinary** e con dimensioni massime impostate su un valore illimitato verranno rappresentate come ISLONG tramite i set di righe dello schema e tramite le interfacce che restituiscono tipi di dati di colonna.  
  
 Il driver OLE DB per SQL Server espone i tipi **varchar(max)** , **varbinary(max)** e **nvarchar(max)** rispettivamente come DBTYPE_STR, DBTYPE_BYTES e DBTYPE_WSTR.  
  
 In un'applicazione è possibile utilizzare questi tipi nei modi seguenti:  
  
-   Eseguire l'associazione come tipo (DBTYPE_STR, DBTYPE_BYTES, DBTYPE_WSTR). Se le dimensioni del buffer non sono sufficienti, si verificherà il troncamento, esattamente come accade per questi tipi nelle versioni precedenti, anche se ora sono disponibili valori più grandi.  
  
-   Eseguire l'associazione come tipo e specificare DBTYPE_BYREF.  
  
-   Eseguire l'associazione come DBTYPE_IUNKNOWN e utilizzare il flusso.  
  
 Se si esegue l'associazione a DBTYPE_IUNKNOWN, viene utilizzata la funzionalità di flusso di ISequentialStream. OLE DB Driver per SQL Server supporta i parametri di output di associazione come DBTYPE_IUNKNOWN per i tipi di dati valore di grandi dimensioni. Ciò consente di supportare gli scenari in cui una stored procedure restituisce questi tipi di dati come valori restituiti, che verranno restituiti come DBTYPE_IUNKNOWN al client.  
  
## <a name="storage-object-limitations"></a>Limitazioni degli oggetti di archiviazione  
  
-   OLE DB Driver per SQL Server può supportare un solo oggetto di archiviazione aperto. I tentativi di aprire più di un oggetto di archiviazione (per ottenere un riferimento su più di un puntatore di interfaccia **ISequentialStream**) restituiscono DBSTATUS_E_CANTCREATE.  
  
-   Nel driver OLE DB per SQL Server il valore predefinito della proprietà di sola lettura DBPROP_BLOCKINGSTORAGEOBJECTS è VARIANT_TRUE. Pertanto, se un oggetto di archiviazione è attivo, alcuni metodi (diversi da quelli degli oggetti di archiviazione) non riusciranno e verrà restituito E_UNEXPECTED.  
  
-   La lunghezza dei dati presentati da un oggetto di archiviazione implementato dal consumer deve essere resa nota al driver OLE DB per SQL Server quando viene creata la funzione di accesso alle righe che fa riferimento all'oggetto di archiviazione. Il consumer deve associare un indicatore di lunghezza nella struttura DBBINDING utilizzata per la creazione della funzione di accesso.  
  
-   Se una riga contiene più di un valore di dati di grandi dimensioni e DBPROP_ACCESSORDER non è DBPROPVAL_AO_RANDOM, il consumer deve usare un set di righe supportato dal cursore del driver OLE DB per SQL Server per recuperare i dati di riga o elaborare tutti i valori di dati di grandi dimensioni prima di recuperare altri valori di riga. Se DBPROP_ACCESSORDER è DBPROPVAL_AO_RANDOM, il driver OLE DB per SQL Server memorizza nella cache tutti i tipi di dati xml come oggetti binari di grandi dimensioni (BLOB) in modo da consentirne l'accesso in qualsiasi ordine.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Recupero di dati di grandi dimensioni](../../oledb/ole-db-blobs/getting-large-data.md)  
  
-   [Impostazione di dati di grandi dimensioni](../../oledb/ole-db-blobs/setting-large-data.md)  
  
-   [Supporto del flusso per parametri di output BLOB](../../oledb/ole-db-blobs/streaming-support-for-blob-output-parameters.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)        
 [Uso di tipi valore di grandi dimensioni](../../oledb/features/using-large-value-types.md)  
  
  
