---
title: Uso dei file di intestazione e di libreria del driver OLE DB per SQL Server | Microsoft Docs
description: Informazioni su come usare i file di intestazione e di libreria di OLE DB Driver per SQL Server nell'ambiente di sviluppo.
ms.custom: ''
ms.date: 02/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- header files [OLE DB Driver for SQL Server]
- MSOLEDBSQL, header files
- OLE DB, header files
- library files [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, header files
- data access [OLE DB Driver for SQL Server], header files
- data access [OLE DB Driver for SQL Server], library files
- OLE DB Driver for SQL Server, library files
- MSOLEDBSQL, library files
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9126ef09a59a04fe54c29050839de35afca84ca5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742951"
---
# <a name="using-the-ole-db-driver-for-sql-server-header-and-library-files"></a>Uso dei file di intestazione e di libreria del driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I file di intestazione e di libreria di OLE DB Driver per SQL Server vengono installati quando viene selezionata l'opzione OLE DB Driver per SQL Server SDK durante il processo di installazione. Quando si sviluppa un'applicazione, è importante copiare e installare nell'ambiente di sviluppo tutti i file necessari per lo sviluppo. Per altre informazioni sull'installazione e la ridistribuzione di OLE DB Driver per SQL Server, vedere [Installazione di OLE DB Driver per SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md).  
  
 I file di intestazione e di libreria di OLE DB Driver per SQL Server vengono installati nel percorso seguente:  
  
 *%PROGRAM FILES%* \Microsoft SQL Server\Client SDK\OLEDB\182\SDK  
  
 È possibile usare il file di intestazione di OLE DB Driver per SQL Server (msoledbsql.h) per aggiungere la funzionalità di accesso ai dati di OLE DB Driver per SQL Server alle applicazioni personalizzate. Il file di intestazione del driver OLE DB per SQL Server contiene tutte le definizioni, gli attributi, le proprietà e le interfacce necessari per usare le nuove funzionalità di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Oltre al file di intestazione di OLE DB Driver per SQL Server, è disponibile anche il file di libreria msoledbsql.lib, che è la libreria di esportazione per la funzionalità [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md).  
  
 Il file di intestazione del driver OLE DB per SQL Server è compatibile con le versioni precedenti del file di intestazione sqloledb.h usato con Microsoft Data Access Components (MDAC) ma non contiene i CLSID per SQLOLEDB (il provider OLE DB per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] incluso con MDAC) o simboli per la funzionalità XML (non supportata dal driver OLE DB per SQL Server).    
  
 Le applicazioni OLE DB che usano OLE DB Driver per SQL Server devono fare riferimento solo a msoledbsql.h. Se un'applicazione usa sia MDAC (SQLOLEDB) sia il driver OLE DB per SQL Server, può fare riferimento sia a sqloledb.h sia amsoledbsql.h, ma il primo riferimento deve essere a sqloledb.h.  
  
## <a name="using-the-ole-db-driver-for-sql-server-header-file"></a>Uso del file di intestazione di OLE DB Driver per SQL Server  
 Per usare il file di intestazione di OLE DB Driver per SQL Server, è necessario usare un'istruzione **include** all'interno del codice di programmazione C/C++. Le sezioni seguenti descrivono come eseguire l'operazione nelle applicazioni OLE DB.  
  
> [!NOTE]  
>  I file di intestazione e di libreria di OLE DB Driver per SQL Server possono essere compilati solo usando Visual Studio C++ 2012 o versione successiva.  
  
### <a name="ole-db"></a>OLE DB  
 Per usare il file di intestazione di OLE DB Driver per SQL Server in un'applicazione OLE DB, usare le righe di codice di programmazione seguenti:  
  
```    
include "msoledbsql.h";  
```  
  
> [!NOTE]  
>  Se nell'applicazione è presente un'istruzione **include** per sqloledb.h, l'istruzione **include** per msoledbsql.h deve essere successiva.  
  
 Quando si crea una connessione a un'origine dati tramite OLE DB Driver per SQL Server, usare "MSOLEDBSQL" come stringa del nome del provider.  

  
## <a name="component-names-and-properties-by-version"></a>Proprietà e nomi dei componenti per versione  

|Proprietà|Driver OLE DB per SQL Server|MDAC|  
|--------|----------------------------|----|   
|OLE DB PROGID|MSOLEDBSQL|SQLOLEDB|  
|Nome file di intestazione OLE DB|msoledbsql.h|Sqloledb.h|  
|DLL del provider OLE DB|msoledbsql.dll|Sqloledb.dll| 
  
  
## <a name="static-linking-and-bcp-functions"></a>Collegamento statico e funzioni BCP  
 Quando in un'applicazione vengono utilizzate funzioni BCP, è importante specificare nella stringa di connessione il driver della stessa versione fornita con il file di intestazione e la libreria utilizzati per compilare l'applicazione.  
  
 Per altre informazioni, vedere [Esecuzione di operazioni di copia bulk](../../oledb/features/performing-bulk-copy-operations.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni con il driver OLE DB per SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
  
  
