---
description: Transazioni in SQL Server Native Client
title: Transazioni (provider OLE DB Native Client)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- transactions [OLE DB]
- SQL Server Native Client OLE DB provider, transactions
ms.assetid: 3b41e33a-c1ca-4b2a-9464-312b0ed3ca89
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ed64680091b7b1170de641c9e1fe04d825dc044
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462112"
---
# <a name="transactions-in-sql-server-native-client"></a>Transazioni in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client implementa il supporto per le transazioni locali. Il consumer può utilizzare transazioni distribuite o coordinate tramite Microsoft Distributed Transaction Coordinator (MS DTC). Per i consumer che richiedono il controllo delle transazioni che si estende su più sessioni, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client può unire in join le transazioni avviate e gestite da MS DTC.  
  
 Per impostazione predefinita, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client utilizza una modalità di transazione con autocommit, in cui ogni azione discreta in una sessione del consumer comprende una transazione completa su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] modalità autocommit del provider OLE DB di Native Client è locale e le transazioni autocommit non si estendono mai più di una singola sessione.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client espone l'interfaccia **ITransactionLocal** , consentendo al consumer di utilizzare in modo esplicito e implicito le transazioni di avvio in una singola connessione a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non supporta le transazioni locali nidificate.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Supporto delle transazioni locali](../../relational-databases/native-client-ole-db-transactions/supporting-local-transactions.md)  
  
-   [Supporto di transazioni distribuite](../../relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions.md)  
  
-   [Livelli di isolamento &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-transactions/isolation-levels-ole-db.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
