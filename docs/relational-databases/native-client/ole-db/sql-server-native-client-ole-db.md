---
title: OLE DB
description: Il provider di OLE DB SQL Server Native Client è un'API COM per l'accesso ai dati, usati per strumenti, utilità o componenti di basso livello che richiedono prestazioni elevate.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, about SQL Server Native Client OLE DB provider
- OLE DB, SQL Server Native Client OLE DB provider
- data access [SQL Server Native Client], OLE DB
- SQLNCLI, OLE DB
- OLE DB
- SQL Server Native Client OLE DB provider
- SQL Server Native Client, OLE DB
ms.assetid: da846da4-ec19-4a4f-81fb-7d5a2b2bf80a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6749bffc5903f2fd0345e6770dd18d00213103bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744931"
---
# <a name="sql-server-native-client-ole-db"></a>SQL Server Native Client (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client (SQLNCLI) è un'API com di basso livello utilizzata per l'accesso ai dati. Il provider OLE DB di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client è consigliato per lo sviluppo di strumenti, utilità o componenti di basso livello che necessitano di prestazioni elevate. Il provider OLE DB di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client un provider nativo a prestazioni elevate che accede direttamente al protocollo TDS (Tabular Data Stream) di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fornisce supporto OLE DB alle applicazioni che si connettono a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client è un provider OLE DB conforme alla versione 2,0.  
 
> [!IMPORTANT]
> Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB di Native Client (SQLNCLI) rimane deprecato e non è consigliabile utilizzarlo per nuove attività di sviluppo. Usare invece il nuovo [Microsoft OLE DB Driver per SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) che verrà aggiornato con le funzionalità server più recenti.
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Creazione di un'applicazione del provider OLE DB di SQL Server Native Client](../../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
-   [Oggetti di origine dati &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
-   [Comandi](../../../relational-databases/native-client-ole-db-commands/commands.md)  
  
-   [Set di righe](../../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
-   [Stored procedure](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
-   [BLOB e oggetti OLE](../../../relational-databases/native-client-ole-db-blobs/blobs-and-ole-objects.md)  
  
-   [Tabelle e indici](../../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
-   [Tipi di dati &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
-   [Supporto del set di righe dello schema &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowset-support-ole-db.md)  
  
-   [Parametri con valori di tabella &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)  
  
-   [Miglioramenti relativi a data e ora &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
-   [Tipi CLR definiti dall'utente di grandi dimensioni &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/large-clr-user-defined-types-ole-db.md)  
  
-   [Supporto FILESTREAM &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/filestream-support-ole-db.md)  
  
-   [Transazioni](../../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
-   [Errori](../../../relational-databases/native-client-ole-db-errors/errors.md)  
  
-   [Nomi SPN &#40;Service Principal Name&#41; nelle connessioni client &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
-   [Supporto per colonne di tipo sparse &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sparse-columns-support-ole-db.md)  
  
-   [Riferimento SQL Server Native Client &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md)  
  
-   [Procedure relative a OLE DB](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Programmazione in SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  
