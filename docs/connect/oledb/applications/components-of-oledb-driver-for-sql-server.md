---
title: Componenti del driver OLE DB per SQL Server | Microsoft Docs
description: Informazioni sui componenti di OLE DB Driver per SQL Server, inclusa la libreria che contiene la funzionalità del driver, altre librerie e un file di intestazione.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], components
- components [OLE DB Driver for SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b4d4ea136dbc216f758bed78caff5d84382caa45
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751281"
---
# <a name="components-of-ole-db-driver-for-sql-server"></a>Componenti del driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server contiene i componenti seguenti:  

|Componente|Descrizione|  
|---------------|-----------------|  
|msoledbsql.dll|File DLL che contiene tutte le funzionalità di OLE DB Driver per SQL Server.|  
|msoledbsqlr.rll|File di risorse associato per la libreria di OLE DB Driver per SQL Server.|   
|msoledbsql.h|File di intestazione di OLE DB Driver per SQL Server che contiene tutte le nuove definizioni necessarie per usare OLE DB Driver per SQL Server. Questo file di intestazione sostituisce il file di intestazione sqloledb.h.<br /><br /> Nota: è possibile fare riferimento a msoledbsql.h e a sqloledb.h nello stesso programma purché sqloledb.h venga definito per primo.|  
|msoledbsql.lib|File di libreria necessario per chiamare direttamente la funzione [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) che fa parte di OLE DB Driver per SQL Server.<br /><br /> Nota: se si fa riferimento al file msoledbsql.lib nel codice di programmazione, è necessario assicurarsi che il file msoledbsql.dll si trovi nel proprio percorso di sistema e in quello degli utenti che usano l'applicazione.|  

## <a name="see-also"></a>Vedere anche  
 [Compilazione di applicazioni con il driver OLE DB per SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
