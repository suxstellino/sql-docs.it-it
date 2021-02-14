---
title: Gestione query XQuery dati relazionali | Microsoft Docs
description: 'Informazioni su come associare dati relazionali non XML a XML utilizzando le estensioni XQuery SQL: Column () e SQL: Variable ().'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- relational data [XQuery]
- XQuery, relational data
ms.assetid: 9812b71a-52ec-48a0-92f3-016a93660229
author: rothja
ms.author: jroth
ms.openlocfilehash: cadad1b4db6a0f0a6284ecb4beb6eadba9e59adc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344955"
---
# <a name="xqueries-handling-relational-data"></a>XQuery per la gestione di dati relazionali
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  È possibile specificare XQuery su una colonna o una variabile di tipo **XML** utilizzando uno dei [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md). Questi includono **query ()**, **value ()**, **exist ()** o **Modify ()**. L'espressione XQuery viene eseguita sull'istanza XML identificata nella query che genera il codice XML.  
  
 Il codice XML generato dall'esecuzione di un'espressione XQuery può includere valori recuperati da altre colonne di set di righe o di variabili Transact-SQL. Per eseguire l'associazione di dati relazionali non XML al codice XML risultante, in SQL Server sono disponibili le pseudofunzioni seguenti come estensioni XQuery:  
  
-   funzione **SQL: Column ()**  
  
-   funzione **SQL: Variable ()**  
  
 È possibile utilizzare queste estensioni XQuery quando si specifica un'espressione XQuery nel metodo **query ()** del tipo di dati **XML** . Di conseguenza, il metodo **query ()** può produrre codice XML che combina i dati da tipi di dati XML e non **XML** .  
  
 È inoltre possibile utilizzare queste funzioni quando si utilizzano i metodi con tipo di dati **XML** **Modify ()**, **value ()**, **query ()** e **exist ()** per esporre un valore relazionale all'interno di XML.  
  
 Per ulteriori informazioni, vedere funzione [SQL: Column () (XQuery)](../xquery/xquery-extension-functions-sql-column.md) e [funzione SQL: Variable () (XQuery)](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Costrutto XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
