---
title: Driver Microsoft OLE DB per SQL Server
description: Microsoft OLE DB Driver per SQL Server abilita la connettività per SQL Server e il database SQL di Azure tramite le API OLE DB standard.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server
- MSOLEDBSQL
- native data access [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ed2642e679cb9822f98285c7dd13f1bdfeda6ef
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748304"
---
# <a name="microsoft-ole-db-driver-for-sql-server"></a>Driver Microsoft OLE DB per SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

OLE DB Driver for SQL Server è un'API (Application Programming Interface) di accesso ai dati autonoma usata in OLE DB, introdotta in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. OLE DB Driver for SQL Server offre il driver OLE DB per SQL in un'unica libreria di collegamento dinamico (DLL). Fornisce inoltre nuove funzionalità che estendono quelle fornite da Windows Data Access Components (Windows DAC, applicazione livello dati, precedentemente noto come Microsoft Data Access Components o MDAC). È possibile usare OLE DB Driver for SQL Server per creare nuove applicazioni o per migliorare applicazioni esistenti al fine di sfruttare le nuove caratteristiche di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], ad esempio MARS (Multiple Active Result Set), tipi di dati definiti dall'utente (UDT), notifica delle query, isolamento dello snapshot e supporto del tipo di dati XML.  
  
> [!NOTE]  
> Per un elenco delle differenze tra OLE DB Driver for SQL Server e Windows DAC, oltre a informazioni sui problemi da considerare prima di aggiornare un'applicazione Windows DAC a OLE DB Driver for SQL Server, vedere [Aggiornamento di un'applicazione a OLE DB Driver for SQL Server da MDAC](../oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).  

> [!IMPORTANT]
> Il [provider Microsoft OLE DB per SQL Server](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) (SQLOLEDB) e il provider [OLE DB SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client.md) (SQLNCLI) precedenti rimangono deprecati e non è consigliabile usarli per nuovi progetti di sviluppo.
  
 Il provider OLE DB del driver OLE DB per SQL Server può essere usato con i servizi principali OLE DB inclusi con Windows DAC (applicazione livello dati), ma non si tratta di un requisito obbligatorio. La scelta di usare o meno i servizi di base dipende dai requisiti dell'applicazione specifica (ad esempio se è richiesto il pool di connessioni).  
  
 Sebbene le applicazioni ADO (ActiveX Data Object) possano usare OLE DB Driver for SQL Server, è consigliabile usare ADO in combinazione con la parola chiave della stringa di connessione **DataTypeCompatibility** (o la proprietà **DataSource** corrispondente). Quando si usa OLE DB Driver for SQL Server, le applicazioni ADO possono sfruttare le nuove funzionalità introdotte in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] disponibili tramite OLE DB Driver for SQL Server attraverso le parole chiave delle stringhe di connessione o le proprietà OLE DB o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per altre informazioni sull'uso di queste funzionalità con ADO, vedere [Uso di ADO con OLE DB Driver for SQL Server](../oledb/applications/using-ado-with-oledb-driver-for-sql-server.md).  
  
 Il driver OLE DB per SQL Server è stato progettato come metodo semplificato per ottenere l'accesso ai dati nativo in SQL Server tramite OLE DB o ODBC. Fornisce un modo per innovare ed evolvere nuove funzionalità di accesso ai dati senza modificare gli attuali componenti Windows DAC, che ora fanno parte della piattaforma Microsoft Windows.  
  
 Anche se OLE DB Driver per SQL Server usa componenti di Windows DAC, non dipende in modo esplicito da una determinata versione di Windows DAC. È possibile usare OLE DB Driver for SQL Server con la versione di Windows DAC installata con qualsiasi sistema operativo supportato da OLE DB Driver for SQL Server.  

 ## <a name="different-generations-of-ole-db-drivers"></a>Generazioni diverse di driver OLE DB

Sono disponibili tre generazioni distinte di provider Microsoft OLE DB per SQL Server.

### <a name="1-microsoft-ole-db-provider-for-sql-server-sqloledb"></a>1. Provider Microsoft OLE DB per SQL Server (SQLOLEDB)
Il [provider Microsoft OLE DB per SQL Server](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) (SQLOLEDB) viene ancora fornito come parte di [Windows Data Access Components](/previous-versions/windows/desktop/ms692897(v=vs.85)). Non viene più aggiornato e non è consigliabile usare questo driver per nuovi sviluppi.

### <a name="2-sql-server-native-client-snac"></a>2. SQL Server Native Client (SNAC)
A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [SQL Server Native Client (SNAC)](../../relational-databases/native-client/sql-server-native-client.md) include un'interfaccia del provider OLE DB (SQLNCLI) ed è il provider OLE DB fornito con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] fino a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].

È stato [annunciato come deprecato nel 2011](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access) e non è consigliabile usare questo driver per nuovi sviluppi. Per altre informazioni sul ciclo di vita di SNAC e i download disponibili, vedere i [dettagli del ciclo di vita di SNAC](/archive/blogs/sqlreleaseservices/snac-lifecycle-explained).

### <a name="3-microsoft-ole-db-driver-for-sql-server-msoledbsql"></a>3. Driver Microsoft OLE DB per SQL Server (MSOLEDBSQL)
OLE DB non è [più deprecato](/archive/blogs/sqlnativeclient/announcing-the-new-release-of-ole-db-driver-for-sql-server) ed è stato rilasciato nel 2018.

Il nuovo provider OLE DB viene chiamato Microsoft OLE DB Driver for SQL Server (MSOLEDBSQL). Il nuovo provider verrà aggiornato con le funzionalità del server più recenti.

> [!NOTE]
> Per usare il nuovo Microsoft OLE DB Driver for SQL Server nelle applicazioni esistenti, è consigliabile convertire le stringhe di connessione da SQLOLEDB o SQLNCLI a MSOLEDBSQL.
  
## <a name="in-this-section"></a>Contenuto della sezione  
[Quando usare il driver OLE DB per SQL Server](../oledb/when-to-use-oledb-driver-for-sql-server.md)  
 Viene illustrato in che modo il driver OLE DB per SQL Server si integra con le tecnologie di accesso ai dati di Microsoft, viene presentato un confronto con Windows DAC (applicazione livello dati) e ADO.NET e vengono visualizzate informazioni utili per decidere quale tecnologia di accesso ai dati usare.  
  
 [Funzionalità di OLE DB Driver for SQL Server](../oledb/features/oledb-driver-for-sql-server-features.md )  
 Descrive le funzionalità supportate da OLE DB Driver for SQL Server.  
  
 [Compilazione di applicazioni con OLE DB Driver for SQL Server](../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
 Viene presentata una panoramica dello sviluppo con il driver OLE DB per SQL Server, incluse le differenze rispetto a Windows DAC (applicazione livello dati), i componenti usati e la modalità di uso di ADO con questo prodotto.  
  
 Questa sezione illustra anche l'installazione e la distribuzione di OLE DB Driver for SQL Server, incluse le procedure per ridistribuire la libreria di OLE DB Driver for SQL Server.  
  
 [Requisiti di sistema per OLE DB Driver for SQL Server](../oledb/system-requirements-for-oledb-driver-for-sql-server.md)  
 Vengono presentate le risorse di sistema necessarie per usare OLE DB Driver for SQL Server.  
  
 [Programmazione con OLE DB Driver for SQL Server](../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
 Vengono fornite informazioni sull'utilizzo di OLE DB Driver for SQL Server.  
  
 [Ricerca di altre informazioni su OLE DB Driver for SQL Server](../oledb/finding-more-oledb-driver-for-sql-server-information.md)  
 Vengono fornite risorse aggiuntive su OLE DB Driver for SQL Server, tra cui collegamenti a risorse esterne e istruzioni per ottenere assistenza.  
  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiornamento di un'applicazione da SQL Server 2005 Native Client](../oledb/applications/updating-an-application-from-sql-server-2005-native-client.md)    
 [Procedure relative a OLE DB](../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
  
