---
title: Requisiti di sistema per driver OLE DB per SQL Server
description: Informazioni sui prerequisiti software necessari per usare le funzionalità di accesso ai dati di SQL Server come ad esempio MARS in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 03/18/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- system requirements [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], system requirements
- OLE DB Driver for SQL Server, system requirements
- MSOLEDBSQL, system requirements
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa46b5a4db292b5ac4ee5df71aa18abeb74c5559
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744871"
---
# <a name="system-requirements-for-ole-db-driver-for-sql-server"></a>Requisiti di sistema per driver OLE DB per SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

Per usare le caratteristiche di accesso ai dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio MARS, è necessario verificare che sia installato il software indicato di seguito:  

* OLE DB Driver per SQL Server sul client.  
* Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul server.

> [!NOTE]  
> Assicurarsi di accedere con privilegi di amministratore prima di installare il software.  

## <a name="operating-system-requirements"></a>Requisiti del sistema operativo  

Per un elenco dei sistemi operativi che supportano OLE DB Driver per SQL Server, vedere [Criteri di supporto per OLE DB Driver per SQL Server](../oledb/applications/support-policies-for-oledb-driver-for-sql-server.md).  

## <a name="azure-active-directory-authentication-requirements"></a>Requisiti di autenticazione di Azure Active Directory  

Quando si usano i metodi di autenticazione di Azure Active Directory con il driver OLE DB per SQL Server ***prima*** della versione 18.3, assicurarsi di avere installato [Active Directory Authentication Library per SQL Server](https://go.microsoft.com/fwlink/?LinkID=513072). (La versione 18.3 include la dipendenza come parte del relativo pacchetto di installazione.) ADAL non è necessario per gli altri metodi di autenticazione o per le operazioni OLE DB. Per altre informazioni, vedere: [Uso di Azure Active Directory](features/using-azure-active-directory.md).

## <a name="sql-server-requirements"></a>requisiti di SQL Server  

Per usare OLE DB Driver per SQL Server per accedere ai dati nei database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario che sia installata un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] supporta connessioni da tutte le versioni di MDAC, Windows Data Access Components e da tutte le versioni del driver OLE DB per SQL Server. Quando si stabilisce una connessione tra una versione client meno recente e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i tipi di dati del server non riconosciuti dal client vengono mappati a tipi compatibili con la versione client. Per altre informazioni, vedere [Compatibilità dei tipi di dati per le versioni client](#data-type-compatibility-for-client-versions).  

## <a name="cross-language-requirements"></a>Requisiti per lingue diverse  

La versione in lingua inglese di OLE DB Driver per SQL Server è supportata in tutte le versioni localizzate dei sistemi operativi supportati. Le versioni localizzate di OLE DB Driver per SQL Server sono supportate in tutti i sistemi operativi localizzati nella stessa lingua della versione localizzata di OLE DB Driver per SQL Server. Le versioni localizzate del driver OLE DB per SQL Server sono supportate anche in tutti i sistemi operativi in lingua inglese, purché siano installate le impostazioni nella lingua corrispondente.  

Per gli aggiornamenti:  

* Le versioni in lingua inglese di OLE DB Driver per SQL Server possono essere aggiornate a qualsiasi versione localizzata di OLE DB Driver per SQL Server.  
* Le versioni localizzate di OLE DB Driver per SQL Server possono essere aggiornate alle versioni localizzate di OLE DB Driver per SQL Server della stessa lingua.  
* Le versioni localizzate di OLE DB Driver per SQL Server possono essere aggiornate alla versione in lingua inglese di OLE DB Driver per SQL Server.  
* Le versioni localizzate di OLE DB Driver per SQL Server non possono essere aggiornate alle versioni localizzate di OLE DB Driver per SQL Server di una lingua diversa.  

## <a name="data-type-compatibility-for-client-versions"></a>Compatibilità dei tipi di dati per le versioni client  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il driver OLE DB per SQL Server eseguono il mapping dei nuovi tipi di dati a tipi di dati meno recenti compatibili con client di versioni precedenti, come illustrato nella tabella riportata di seguito.  

Nelle applicazioni OLE DB e ADO è possibile usare la parola chiave **DataTypeCompatibility** della stringa di connessione con OLE DB Driver per SQL Server per l'uso di tipi di dati di versioni precedenti. Se **DataTypeCompatibility=80**, i client OLE DB si connettono usando la versione del flusso TDS di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], anziché quella corrente. Ciò significa che per i tipi di dati di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, la conversione alle versioni precedenti verrà eseguita dal server e non dal driver OLE DB per SQL Server. Significa inoltre che le caratteristiche disponibili nella connessione saranno limitate al set di funzionalità di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. I tentativi di usare nuovi tipi di dati o caratteristiche vengono rilevati il prima possibile nelle chiamate API e, anziché tentare di passare richieste non valide al server, vengono restituiti errori all'applicazione chiamante.  

IDBInfo::GetKeywords restituisce sempre un elenco di parole chiave che corrisponde alla versione del server per la connessione e non è influenzato da **DataTypeCompatibility**.  

|Tipo di dati|SQL Server Native Client<br /><br />SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Driver OLE DB per SQL Server|Applicazioni OLE DB di Windows Data Access Components, MDAC e<br /><br /> Applicazioni OLE DB di OLE DB Driver per SQL Server con DataTypeCompatibility=80|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|Tipo definito dall'utente CLR (\<= 8 KB)|udt|udt|udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|varbinary|Immagine|  
|ntext|varchar|varchar|varchar|Text|  
|nvarchar(max)|NVARCHAR|NVARCHAR|NVARCHAR|Ntext|  
|Xml|Xml|Xml|Xml|Ntext|  
|Tipo CLR definito dall'utente (> 8 KB)|varbinary|udt|udt|Immagine|  
|Data|varchar|Data|Data|Varchar|  
|datetime2|varchar|datetime2|datetime2|Varchar|  
|datetimeoffset|varchar|datetimeoffset|datetimeoffset|Varchar|  
|time|varchar|time|time|Varchar|  

## <a name="see-also"></a>Vedere anche  

[Driver OLE DB per SQL Server](../oledb/oledb-driver-for-sql-server.md)  
[Installazione del driver OLE DB per SQL Server](../oledb/applications/installing-oledb-driver-for-sql-server.md)  
