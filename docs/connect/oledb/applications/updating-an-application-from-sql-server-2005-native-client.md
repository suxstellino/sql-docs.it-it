---
title: Aggiornamento di un'applicazione da SQL Server 2005 Native Client | Microsoft Docs
description: Informazioni sulle modifiche di rilievo introdotte in OLE DB Driver per SQL Server da SQL Server Native Client in SQL Server 2005 (9.x).
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, updating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8f53c33d04e76c362737b7c7bdc3ad803846cf9
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793136"
---
# <a name="updating-applications-from-sql-server-2005-native-client"></a>Aggiornamento di applicazioni da SQL Server 2005 Native Client

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

In questo articolo vengono illustrate le modifiche di rilievo apportate al driver OLE DB per SQL Server da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] .

Quando si esegue l'aggiornamento da Microsoft Data Access Components (MDAC) al driver OLE DB per SQL Server, si possono anche notare alcune differenze di comportamento. Per altre informazioni, vedere [Aggiornamento di un'applicazione a OLE DB Driver per SQL Server da MDAC](updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 fornito con [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 fornito con [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.5 fornito con [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 fornito con [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] e [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)].

|Comportamento modificato in OLE DB Driver per SQL Server rispetto a [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Native Client|Descrizione|
|------------------------------------------------------------------------------------|-----------------|
|In OLE DB viene applicato il riempimento solo in base alla scala definita.|Per le conversioni in cui i dati convertiti vengono inviati al server, OLE DB Driver per SQL Server inserisce zeri finali nei dati solo fino alla lunghezza massima dei valori **datetime**. In SQL Server Native Client 9.0 viene applicato un riempimento fino a un massimo di 9 cifre.|
|Convalida di DBTYPE_DBTIMESTAMP per ICommandWithParameter::SetParameterInfo.|OLE DB Driver per SQL Server implementa il requisito di OLE DB per *bScale* in ICommandWithParameter::SetParameterInfo in modo che venga impostato sulla precisione dei secondi frazionari per DBTYPE_DBTIMESTAMP.|
|La stored procedure **sp_columns** restituisce ora **"NO"** anziché **"NO "** per la colonna IS_NULLABLE.|In OLE DB Driver per SQL Server la stored procedure **sp_columns** restituisce ora **"NO"** anziché **"NO "** per una colonna IS_NULLABLE.|
|Quando la data non è inclusa nell'intervallo consentito viene restituito un errore differente.|Per il tipo **datetime**, viene restituito da OLE DB Driver per SQL Server un numero di errore diverso per una data non compresa nell'intervallo rispetto a quello restituito nelle versioni precedenti.<br /><br /> In particolare, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 restituiva 22007 per tutti i valori di anno non compresi nell'intervallo nelle conversioni di stringa in **datetime**, mentre OLE DB Driver per SQL Server restituisce 22008 quando la data è compresa nell'intervallo supportato da **datetime2** ma è esterna all'intervallo supportato da **datetime** o **smalldatetime**.|
|Il valore **datetime** tronca i secondi frazionari e non viene arrotondato se tale arrotondamento comporta la modifica del giorno.|Nelle versioni precedenti a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, il client arrotonda i valori **datetime** inviati al server al valore più vicino corrispondente a 1/300 di secondo. In OLE DB Driver per SQL Server questo scenario causa il troncamento dei secondi frazionari se l'arrotondamento comporta la modifica del giorno.|
|Possibile troncamento di secondi per il valore **DateTime** .|In un'applicazione compilata con il driver OLE DB per SQL Server che si connette a un server [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2005 vengono troncati i secondi e i secondi frazionari per la porzione relativa al tempo dei dati inviati al server se si esegue l'associazione a una colonna datetime con un identificatore di tipo DBTYPE_DBTIMESTAMP (OLE DB) o SQL_TIMESTAMP (ODBC) e una scala di 0.<br /><br /> Ad esempio:<br /><br /> Dati di input: 1994-08-21 21:21:36.000<br /><br /> Dati inseriti: 1994-08-21 21:21:00.000|
|La conversione dei dati OLE DB da DBTYPE_DBTIME a DBTYPE_DATE non può più causare la modifica del giorno.|Nelle versioni precedenti a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, se la parte relativa all'ora di un tipo DBTYPE_DATE è entro mezzo secondo dalla mezzanotte, il codice di conversione OLE DB causa la modifica del giorno. In OLE DB Driver per SQL Server il giorno non viene modificato (i secondi frazionari vengono troncati ma non vengono arrotondati).|
|Modifiche della conversione IBCPSession::BCColFmt.|In OLE DB Driver per SQL Server quando si usa IBCPSession::BCOColFmt per convertire SQLDATETIME or SQLDATETIME in un tipo stringa, viene esportato un valore frazionario. Se ad esempio si converte il tipo SQLDATETIME nel tipo SQLNVARCHARMAX, le versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 restituiscono<br /> 1989-02-01 00:00:00.<br />Il driver OLE DB per SQL Server restituisce <br />1989-02-01 00:00:00.0000000.|
|Le applicazioni personalizzate che utilizzano l'API BCP ora possono visualizzare un avviso.|L'API BCP genererà un messaggio di avviso se la lunghezza dei dati di tutti i tipi è superiore a quella specificata per un campo. In precedenza, questo avviso veniva visualizzato solo per i dati di tipo carattere e non per tutti i tipi.|
|Se si inserisce una stringa vuota in una colonna **sql_variant** associata come tipo data/ora, viene generato un errore.|In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 l'inserimento di una stringa vuota in una colonna **sql_variant** associata come tipo data/ora non generava alcun errore. In questa situazione OLE DB Driver per SQL Server genera correttamente un errore.|
|Quando è in esecuzione un trigger, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] potrebbe restituire risultati diversi.|A causa delle modifiche introdotte in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], un'istruzione che ha causato l'esecuzione di un trigger con **NOCOUNT OFF** attivo potrebbe restituire a un'applicazione risultati diversi. In una situazione di questo tipo l'applicazione potrebbe generare un errore. Per correggere l'errore, impostare **NOCOUNT ON** nel trigger.|

## <a name="see-also"></a>Vedere anche

[Driver OLE DB per SQL Server](../oledb-driver-for-sql-server.md)
