---
description: 'Alternative: uso di istruzioni SQL'
title: 'Alternative: uso di istruzioni SQL | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ADO]
- editing data [ADO], sql statements
- ADO, SQL statements
ms.assetid: 8b528b23-063d-45ea-8dea-6a90d4060b20
author: rothja
ms.author: jroth
ms.openlocfilehash: 91111602587b7857559a633a816d7fbb6b125dcb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028055"
---
# <a name="alternatives-using-sql-statements"></a>Alternative: uso di istruzioni SQL
ADO consente inoltre di utilizzare i comandi come alternative alle proprietà e ai metodi predefiniti per la modifica dei dati. A seconda del provider, tutte le operazioni descritte in questa sezione possono essere eseguite anche passando i comandi all'origine dati. È ad esempio possibile utilizzare le istruzioni SQL UPDATE per modificare i dati senza utilizzare la proprietà **value** di un **campo**. È possibile utilizzare le istruzioni SQL INSERT per aggiungere nuovi record a un'origine dati, anziché il metodo ADO **AddNew**. Per ulteriori informazioni su SQL o sul linguaggio di manipolazione dei dati del provider, vedere la documentazione dell'origine dati.  
  
 Ad esempio, è possibile passare una stringa SQL contenente un'istruzione DELETE a un database, come illustrato nel codice seguente:  
  
```  
'BeginSQLDelete  
strSQL = "DELETE FROM Shippers WHERE ShipperID = " & intId  
objConn1.Execute strSQL, , adCmdText + adExecuteNoRecords  
'EndSQLDelete  
```
