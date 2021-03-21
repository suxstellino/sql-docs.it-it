---
description: SQLSpecialColumns
title: SQLSpecialColumns | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSpecialColumns function
ms.assetid: dffe02ed-8f79-4c9a-af34-98130bbe5462
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 753afbb5bd61b84a9c1b6a91137984bb65cb370a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754821"
---
# <a name="sqlspecialcolumns"></a>SQLSpecialColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quando vengono richiesti identificatori di riga (*IdentifierType* SQL_BEST_ROWID), **SQLSpecialColumns** restituisce un set di risultati vuoto (nessuna riga di dati) per qualsiasi ambito richiesto diverso da SQL_SCOPE_CURROW. Il set di risultati generato indica che le colonne sono valide solo all'interno di questo ambito.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta pseudocolonne per gli identificatori. Il set di risultati **SQLSpecialColumns** identificherà tutte le colonne come SQL_PC_NOT_PSEUDO.  
  
 **SQLSpecialColumns** può essere eseguito su un cursore statico. Un tentativo di eseguire **SQLSpecialColumns** su un oggetto aggiornabile (gestito da keyset o dinamico) restituisce SQL_SUCCESS_WITH_INFO indicante che il tipo di cursore è stato modificato.  
  
## <a name="sqlspecialcolumns-support-for-enhanced-date-and-time-features"></a>Supporto di SQLSpecialColumns per le caratteristiche avanzate di data e ora  
 Per informazioni sui valori restituiti per le colonne DATA_TYPE, TYPE_NAME, COLUMN_SIZE, BUFFER_LENGTH e DECIMAL_DIGTS per i tipi data/ora, vedere [metadati del catalogo](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Per informazioni più generali, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlspecialcolumns-support-for-large-clr-udts"></a>Supporto di SQLSpecialColumns per tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLSpecialColumns** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLSpecialColumns (funzione)](../../odbc/reference/syntax/sqlspecialcolumns-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
