---
title: Oggetti origine dati (OLE DB Driver) | Microsoft Docs
description: Informazioni su come un consumer OLE DB Driver per SQL Server crea un'istanza di un oggetto origine dati per un provider.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], data source objects
- OLE DB Driver for SQL Server, data source objects
- MSOLEDBSQL, data source objects
- OLE DB Driver for SQL Server, data source objects
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- CLSID
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 53208b567de362b388e5fb2183c67dccdabb2d2d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735213"
---
# <a name="data-source-objects-ole-db"></a>Oggetti origine dati (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server usa il termine origine dati per il set di interfacce OLE DB usate per stabilire un collegamento a un archivio dati, ad esempio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La creazione di un'istanza dell'oggetto di origine dati del provider è la prima attività di un consumer di OLE DB Driver per SQL Server.  
  
 Ogni provider OLE DB dichiara un identificatore di classe (CLSID) per se stesso. L'identificatore CLSID per OLE DB Driver per SQL Server è il GUID C/C++ CLSID_MSOLEDBSQL. Il simbolo MSOLEDBSQL_CLSID restituirà il progid corretto nel file msoledbsql.h a cui si fa riferimento. Con il CLSID, il consumer usa la funzione OLE **CoCreateInstance** per produrre un'istanza dell'oggetto origine dati.  
  
 OLE DB Driver per SQL Server è un server in-process. Le istanze degli oggetti del driver OLE DB per SQL Server vengono create usando la macro CLSCTX_INPROC_SERVER per indicare il contesto di esecuzione.  
  
 L'oggetto origine dati del driver OLE DB per SQL Server espone le interfacce di inizializzazione OLE DB che consentono al consumer di connettersi ai database [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistenti.  
  
 Ogni connessione eseguita tramite OLE DB Driver per SQL Server imposta automaticamente queste opzioni:  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 In questo esempio viene usata la macro dell'identificatore di classe per creare un oggetto origine dati del driver OLE DB per SQL Server e ottenere un riferimento alla relativa interfaccia **IDBInitialize**.  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_MSOLEDBSQL, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 In seguito alla creazione di un'istanza di un oggetto origine dati del driver OLE DB per SQL Server, l'applicazione consumer può proseguire inizializzando l'origine dati e creando le sessioni. Le sessioni OLE DB presentano le interfacce che consentono l'accesso ai dati e la relativa modifica.  
  
 Il driver OLE DB per SQL Server crea la prima connessione a un'istanza specificata di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] come parte di una corretta inizializzazione dell'origine dati. La connessione viene mantenuta a condizione che venga mantenuto un riferimento in una delle interfacce di inizializzazione dell'origine dati o fino a quando non viene chiamato il metodo **IDBInitialize::Uninitialize**.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Proprietà delle origini dati &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [Proprietà delle informazioni sulle origini dati](../../oledb/ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [Proprietà di inizializzazione e di autorizzazione](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [Sessioni](../../oledb/ole-db-data-source-objects/sessions.md)  
  
-   [Proprietà sessione - Driver OLE DB per SQL Server](../../oledb/ole-db-data-source-objects/session-properties-oledb-driver-for-sql-server.md)  
  
-   [Oggetti di origine dati persistenti](../../oledb/ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
