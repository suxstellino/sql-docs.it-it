---
description: Supporto per colonne di tipo sparse (ODBC)
title: Supporto per colonne di tipo sparse (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 11ae959f-2fb6-4b85-ac5d-1476a82136d4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: abec37f7b5683e5cab713800700e4056bf852f2c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745531"
---
# <a name="sparse-columns-support-odbc"></a>Supporto per colonne di tipo sparse (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento viene descritto il supporto ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client per le colonne di tipo sparse. Per un esempio che illustra il supporto ODBC per le colonne di tipo sparse, vedere [chiamare SQLColumns in una tabella con colonne di tipo sparse](../../../relational-databases/native-client-odbc-how-to/call-sqlcolumns-on-a-table-with-sparse-columns.md). Per ulteriori informazioni sulle colonne di tipo sparse, vedere [supporto di colonne di tipo sparse in SQL Server Native Client](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md).  
  
## <a name="statement-metadata"></a>Metadati di istruzione  
 Il campo di descrizione del parametro dell'applicazione (APD) e l'attributo dell'istruzione SQL_SOPT_SS_NAME_SCOPE accettano i valori aggiuntivi SQL_SS_NAME_SCOPE_EXTENDED e SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET. Questi valori specificano le colonne incluse nel set di risultati restituito da [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md). Per ulteriori informazioni su SQL_SOPT_SS_NAME_SCOPE, vedere [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Un nuovo descrittore della riga di implementazione (IRD), un campo SQLSMALLINT di sola lettura denominato SQL_CA_SS_IS_COLUMN_SET, può essere utilizzato per determinare se una colonna è un valore di **column_set** XML. SQL_CA_SS_IS_COLUMN_SET accetta i valori SQL_TRUE e SQL_FALSE.  
  
## <a name="catalog-metadata"></a>Metadati del catalogo  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Sono state aggiunte due colonne specifiche (SS_IS_SPARSE e SS_IS_COLUMN_SET) al set di risultati per [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="odbc-function-support-for-sparse-columns"></a>Supporto della funzione ODBC per colonne di tipo sparse  
 Sono state aggiornate le funzioni ODBC seguenti per supportare colonne di tipo sparse in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client:  
  
-   [SQLColAttribute](../../../relational-databases/native-client-odbc-api/sqlcolattribute.md)  
  
-   [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md)  
  
-   [SQLGetDescField](../../../relational-databases/native-client-odbc-api/sqlgetdescfield.md)  
  
-   [SQLSetDescField](../../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)  
  
-   [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
