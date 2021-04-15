---
title: Tipi di dati e comportamento di caricamento bulk XML (SQLXML)
description: Informazioni sui tipi di dati e sul comportamento del caricamento bulk XML in SQLXML 4.0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- bulk load [SQLXML], data types
- data types [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], data types
ms.assetid: d1ac1939-1f6c-4398-b7a7-a79ca608a4f1
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 82c47b4a20642625b596caee66ae274cb3b18fe7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490636"
---
# <a name="data-types-and-xml-bulk-load-behavior-sqlxml-40"></a>Tipi di dati e comportamento del caricamento bulk XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  I tipi di dati specificati nello schema di mapping (tipo XSD o XDR e **sql:datatype**) vengono in genere ignorati, ad eccezione dei casi seguenti:  
  
 In XSD:  
  
-   Se il tipo è **dateTime** o **time**, è necessario specificare **sql:datatype** perché il caricamento bulk XML esegue la conversione dei dati prima di inviare i dati a Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Quando si esegue il caricamento bulk in una colonna di tipo **uniqueidentifier** in e il valore XSD è un GUID che include parentesi graffe ({ e }), è necessario specificare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **sql:datatype="uniqueidentifier"** per rimuovere le parentesi graffe prima che il valore venga inserito nella colonna. Se **sql:datatype non** viene specificato, il valore viene inviato con le parentesi graffe e l'inserimento non riesce.  
  
 Per altre informazioni su **sql:datatype**, vedere Coercizioni del tipo di dati e l'annotazione [sql:datatype &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md).  
  
 In XDR:  
  
-   Se **dt:type** è **datetime**, **time**, **dateTime.tz** o **time.tz**, è necessario specificare entrambi i tipi di dati **dt:type** e **sql:datatype** perché il caricamento bulk XML esegue la conversione dei dati prima di inviare i dati a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Se i dati XML sono di tipo **uuid**, è necessario specificare **sql:datatype.** **È necessario anche dt:type="uuid",** a meno che i dati non siano dati stringa. Se non si specifica **dt:uuid**, il caricamento bulk XML accetta stringhe con parentesi graffe e le rimuove se necessario.  
  
-   Se i dati XML sono **bin.base64** o **bin.hex,** è necessario specificare il tipo di dati XML con **dt:type**. Il caricamento bulk XML carica quindi i dati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] come rappresentazione esadecimale dei dati.  
  
  
