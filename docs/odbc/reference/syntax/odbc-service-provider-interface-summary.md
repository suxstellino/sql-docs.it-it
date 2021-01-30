---
description: Riepilogo dell'interfaccia del provider di servizi ODBC
title: Riepilogo dell'interfaccia del provider di servizi ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ace6085b-355b-435b-8734-503fc3c12ec2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9583d75f98283b40d6e8c5d3e320d82533bf55d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174615"
---
# <a name="odbc-service-provider-interface-summary"></a>Riepilogo dell'interfaccia del provider di servizi ODBC
Nella tabella seguente vengono descritte le funzioni di interfaccia del provider di servizi ODBC. Per ulteriori informazioni sulla sintassi e la semantica per ogni funzione, vedere [riferimento a ODBC Service Provider Interface (SPI)](../../../odbc/reference/syntax/odbc-service-provider-interface-spi-reference.md).  
  
|Nome della funzione|Scopo|  
|-------------------|-------------|  
|[SQLSetConnectAttrForDbcInfo](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Uguale a [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), ma imposta l'attributo sul token delle informazioni di connessione anziché sull'handle di connessione.|  
|[SQLSetDriverConnectInfo](../../../odbc/reference/syntax/sqldrivertodatasource-function.md)|Imposta la stringa di connessione nel token delle informazioni di connessione per la chiamata [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) di un'applicazione.|  
|[SQLSetConnectInfo](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Imposta l'origine dati, l'ID utente e la password nel token di informazioni di connessione per la chiamata [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) di un'applicazione.|  
|[SQLGetPoolID](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Recupera l'ID del pool.|  
|[SQLRateConnection](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Determina se un driver può riutilizzare una connessione esistente nel pool di connessioni.|  
|[SQLPoolConnect](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Crea una nuova connessione se non è possibile riutilizzare una connessione nel pool.|  
|[SQLCleanupConnectionPoolID](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Informa un driver che è stato raggiunto il timeout di un ID pool.|
