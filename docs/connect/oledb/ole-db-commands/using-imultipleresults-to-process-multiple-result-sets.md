---
title: Uso di IMultipleResults per elaborare più set di risultati | Microsoft Docs
description: Informazioni su come i consumer usano l'interfaccia IMultipleResults per elaborare i risultati restituiti dall'esecuzione dei comandi di OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- IMultipleResults interface
- multiple-rowset results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5c9c73e56b257251b59dbfb611ce15058d78602
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735397"
---
# <a name="using-imultipleresults-to-process-multiple-result-sets"></a>Utilizzo dell'interfaccia IMultipleResults per elaborare più set di risultati
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I consumer usano l'interfaccia **IMultipleResults** per elaborare i risultati restituiti dall'esecuzione dei comandi del driver OLE DB per SQL Server. Quando il driver OLE DB per SQL Server invia un comando per l'esecuzione, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esegue le istruzioni e restituisce tutti i risultati.  
  
 Tutti i risultati ottenuti dall'esecuzione dei comandi devono essere elaborati da un client. Dal momento che l'esecuzione dei comandi del driver OLE DB per SQL Server può generare come risultato oggetti con più set di righe, usare l'interfaccia **IMultipleResults** per assicurare che il recupero dei dati dell'applicazione completi il round trip iniziato dal client.  
  
 L'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente genera più set di righe, alcuni dei quali contengono dati di riga della tabella **OrderDetails**, mentre altri contengono i risultati della clausola COMPUTE BY:  
  
```  
SELECT OrderID, FullPrice = (UnitPrice * Quantity), Discount,  
    Discounted = UnitPrice * (1 - Discount) * Quantity  
FROM OrderDetails  
ORDER BY OrderID  
COMPUTE  
    SUM(UnitPrice * Quantity), SUM(UnitPrice * (1 - Discount) * Quantity)  
    BY OrderID  
```  
  
 Se un consumer esegue un comando contenente questo testo e richiede un set di righe come interfaccia dei risultati restituiti, viene restituito solo il primo set di righe. Il consumer può elaborare tutte le righe del set di righe restituito. Tuttavia, se la proprietà dell'origine dati DBPROP_MULTIPLECONNECTIONS è impostata su VARIANT_FALSE e sulla connessione non è abilitato il servizio MARS, sull'oggetto sessione non è possibile eseguire nessun altro comando (il driver OLE DB per SQL Server non creerà un'altra connessione) fino a quando il comando non verrà annullato. Se sulla connessione non è abilitato il servizio MARS, il driver OLE DB per SQL Server restituisce un errore DB_E_OBJECTOPEN nel caso in cui DBPROP_MULTIPLECONNECTIONS sia impostato su VARIANT_FALSE e restituisce E_FAIL nel caso in cui sia presente una transazione attiva.  
  
 Il driver OLE DB per SQL Server restituirà anche DB_E_OBJECTOPEN quando si usano parametri di output inviati come flusso e l'applicazione non ha usato tutti i valori del parametro di output restituiti prima di chiamare **IMultipleResults::GetResults** per ottenere il set di risultati successivo. Se MARS non è abilitato e la connessione è occupata poiché è in esecuzione un comando che non produce un set di righe o che produce un set di righe diverso da un cursore del server e se la proprietà dell'origine dati DBPROP_MULTIPLECONNECTIONS è impostata su VARIANT_TRUE, il driver OLE DB per SQL Server crea delle connessioni aggiuntive per supportare gli oggetti comando simultanei, a meno che non sia attiva una transazione. In tal caso restituisce un errore. Le transazioni e il blocco vengono gestiti da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per singola connessione. Se viene generata una seconda connessione, il comando sulle connessioni separate non condivide i blocchi. È necessario assicurarsi che un comando non blocchi un altro comando mantenendo attivi i blocchi sulle righe richieste da quest'ultimo. Se MARS è abilitato, è possibile disporre di più comandi attivi sulle connessioni e se vengono utilizzate transazioni esplicite tutti i comandi condividono una transazione comune.  
  
 Il consumer può annullare il comando usando [ISSAbort::Abort](../../oledb/ole-db-interfaces/issabort-abort-ole-db.md) oppure rilasciando tutti i riferimenti mantenuti sull'oggetto comando e sul set di righe derivato.  
  
 Se **IMultipleResults** viene usato in tutte le istanze, il consumer può ottenere tutti i set di righe generati dall'esecuzione dei comandi e determinare in modo appropriato il momento in cui annullare l'esecuzione dei comandi e liberare un oggetto sessione per consentirne l'uso da parte di altri comandi.  
  
> [!NOTE]  
>  Quando si utilizzano cursori [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'esecuzione di comandi crea il cursore. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] restituisce l'esito positivo o negativo della creazione del cursore. Pertanto, il round trip all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene completato al momento della restituzione da parte dell'esecuzione di comandi. Ogni chiamata a **GetNextRows** diventa quindi un round trip. In questo modo, possono esistere più oggetti comando attivi, ognuno dei quali elabora un set di righe che rappresenta il risultato di un recupero dal cursore del server. Per altre informazioni, vedere [Set di righe e cursori SQL Server](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../oledb/ole-db-commands/commands.md)  
  
  
