---
description: Tipi di dati Microsoft Access
title: Tipi di dati di Microsoft Access | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0e7a55c6929d63bf036c9f1585d2eb039f6fec77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205710"
---
# <a name="microsoft-access-data-types"></a>Tipi di dati Microsoft Access
Nella tabella seguente vengono illustrati i tipi di dati di Microsoft Access, i tipi di dati utilizzati per creare tabelle e i tipi di dati SQL ODBC.  
  
|Tipo di dati Microsoft Access|Tipo di dati (CREATETABLE)|Tipo di dati SQL ODBC|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|LONGBINARY|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|COUNTER|COUNTER|SQL_INTEGER|  
|CURRENCY|CURRENCY|SQL_NUMERIC|  
|DATA/ORA|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|BINARIO LUNGO|LONGBINARY|SQL_LONGVARBINARY|  
|TESTO LUNGO|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|MEMO|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|NUMERO (DimensioneCampo = SINGLE)|SINGOLO|SQL_REAL|  
|NUMERO (DimensioneCampo = DOUBLE)|DOUBLE|SQL_DOUBLE|  
|NUMERO (DimensioneCampo = BYTE)|BYTE SENZA SEGNO|SQL_TINYINT|  
|NUMERO (DimensioneCampo = INTEGER)|SHORT|SQL_SMALLINT|  
|NUMERO (DimensioneCampo = LONG INTEGER)|LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE|LONGBINARY|SQL_LONGVARBINARY|  
|TEXT|VARCHAR|SQL_VARCHAR [1] SQL_WVARCHAR [2]|  
|VARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] accedere solo alle applicazioni 4,0. Lunghezza massima di 4000 byte. Comportamento simile a LONGBINARY.  
  
 [2] solo applicazioni ANSI.  
  
 [3] solo per le applicazioni Unicode e Access 4,0.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** restituisce tipi di dati ODBC. Non restituirà tutti i tipi di dati di Microsoft Access se è stato eseguito il mapping di più di un tipo di Microsoft Access allo stesso tipo di dati SQL ODBC. Tutte le conversioni nell'Appendice D di *ODBC Programmer ' s Reference* sono supportate per i tipi di dati SQL elencati nella tabella precedente.  
  
 Nella tabella seguente vengono illustrate le limitazioni relative ai tipi di dati di Microsoft Access.  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|BINARY, VARBINARY e VARCHAR|La creazione di una colonna BINARIa, VARBINARY o VARCHAR di zero o di lunghezza non specificata restituisce effettivamente una colonna a 510 byte.|  
|BYTE|Anche se un campo numero di Microsoft Access con DimensioneCampo uguale a BYTE non è firmato, è possibile inserire un numero negativo nel campo quando si usa il driver Microsoft Access.|  
|CHAR, LONGVARCHAR e VARCHAR|Un valore letterale stringa di caratteri può contenere qualsiasi carattere ANSI (1-255 decimale). Utilizzare due virgolette singole consecutive ('') per rappresentare una virgoletta singola (').<br /><br /> È consigliabile utilizzare le procedure per passare dati di tipo carattere quando si utilizza un carattere speciale in una colonna con tipo di dati character.|  
|DATE|I valori di data devono essere delimitati in base al formato di data canonico di ODBC o delimitati dal delimitatore DateTime ("#"). In caso contrario, Microsoft Access considererà il valore come espressione aritmetica e non genererà un avviso o un errore.<br /><br /> Ad esempio, la data "5 marzo 1996" deve essere rappresentata come {d'1996-03-05'} o #03/05/1996 #; in caso contrario, se viene inviato solo 03/05/1993, Microsoft Access valuterà questo risultato come 3 diviso 5 diviso per 1996. Questo valore viene arrotondato per eccesso al numero intero 0 e poiché il giorno zero viene mappato a 1899-12-31, si tratta della data utilizzata.<br /><br /> Non è possibile usare un carattere barra verticale (&#124;) in un valore di data, anche se racchiuso tra virgolette.|  
|GUID|Tipo di dati limitato a Microsoft Access 4,0.|  
|NUMERIC|Tipo di dati limitato a Microsoft Access 4,0.|  
  
 Per altre limitazioni sui tipi di dati, vedere [limitazioni del tipo di dati](../../odbc/microsoft/data-type-limitations.md).
