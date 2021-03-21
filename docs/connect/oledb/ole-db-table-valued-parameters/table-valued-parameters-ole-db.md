---
title: Parametri con valori di tabella (OLE DB Driver)
description: Questi articoli descrivono il supporto per i parametri con valori di tabella in OLE DB Driver per SQL Server, inclusa la creazione del set di righe di parametri e l'individuazione del tipo di parametro.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, table-valued parameters
- table-valued parameters (OLE DB)
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7386ae8dcd59df4fd404136167e4b092882fc450
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749171"
---
# <a name="table-valued-parameters-ole-db"></a>Parametri con valori di tabella (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questa sezione illustra il supporto per i parametri con valori di tabella in OLE DB Driver per SQL Server. Per altre informazioni generali, vedere [Parametri con valori di tabella &#40;OLE DB Driver for SQL Server&#41;](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md). Per un esempio, vedere [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md).  
  
## <a name="remarks"></a>Osservazioni  
 Attualmente, è possibile inviare al server dati a più righe come parametri di una procedura con set di parametri (parametro DBPARAMS in **ICommand::Execute**). Con i set di parametri, ogni elemento del set deve essere inviato al server in una richiesta di chiamata di procedura remota (RPC) separata. I parametri con valori di tabella forniscono funzionalità simili, ma offrono una maggiore integrazione con il server. Ciò determina la riduzione del numero di richieste RPC e l'abilitazione delle operazioni basate sul set nel server.  
  
 I parametri con valori di tabella sono supportati in OLE DB Driver for SQL Server come oggetti **Rowset** di OLE DB. Gli oggetti **Rowset** possono essere offerti dal consumer, ovvero dall'applicazione client che usa il driver OLE DB per SQL Server, come segnaposto per i parametri con valori di tabella. I parametri con valori di tabella vengono considerati come gli altri tipi di parametro di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. OLE DB Driver per SQL Server fornisce interfacce per gli schemi, l'associazione, la specifica, l'individuazione e la creazione.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Creazione di un set di righe di parametri con valori di tabella](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md)  
  
-   [Individuazione dei tipi di parametro con valori di tabella](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-type-discovery.md)  
  
-   [Esecuzione di comandi contenenti parametri con valori di tabella](../../oledb/ole-db-table-valued-parameters/executing-commands-containing-table-valued-parameters.md)  
  
-   [Inserimento di dati in parametri con valori di tabella](../../oledb/ole-db-table-valued-parameters/inserting-data-into-table-valued-parameters.md)  
  
-   [Set di righe dello schema modificati per i parametri con valori di tabella OLE DB](../../oledb/ole-db-table-valued-parameters/schema-rowsets-changed-for-ole-db-table-valued-parameters.md)  
  
-   [Supporto dei tipi di parametro con valori di tabella OLE DB](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)  
  
-   [Supporto dei tipi di parametro con valori di tabella OLE DB &#40;metodi&#41;](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md)  
  
-   [Supporto dei tipi di parametro con valori di tabella OLE DB &#40;proprietà&#41;](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
