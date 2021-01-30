---
description: 'Da SQL a C: dati binari'
title: 'Da SQL a C: binario | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], binary
- binary data type [ODBC]
- data conversions from SQL to C types [ODBC], binary
- binary data transfers [ODBC]
ms.assetid: 8c519072-ae4c-4d32-9d4e-775e3d3d6389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 30fbabde654f69af7a0cb68e2606c0f2d93697c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160821"
---
# <a name="sql-to-c-binary"></a>Da SQL a C: dati binari
Gli identificatori per i tipi di dati SQL ODBC binari sono:  
  
 SQL_BINARY  
  
 SQL_VARBINARY  
  
 SQL_LONGVARBINARY  
  
 Nella tabella seguente sono illustrati i tipi di dati ODBC C in cui è possibile convertire i dati SQL binari. Per una spiegazione delle colonne e dei termini della tabella, vedere [conversione di dati da SQL a tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificatore di tipo C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|(Lunghezza in byte dei dati) \* 2 < *bufferLength*<br /><br /> (Lunghezza in byte dei dati) \* 2 >= *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte|n/d<br /><br /> 01004|  
|SQL_C_WCHAR|(Lunghezza dei caratteri di dati) \* 2 < *bufferLength*<br /><br /> (Lunghezza dei caratteri di dati) \* 2 >= *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in caratteri<br /><br /> Lunghezza dei dati in caratteri|n/d<br /><br /> 01004|  
|SQL_C_BINARY|Lunghezza in byte dei dati <= *bufferLength*<br /><br /> Lunghezza in byte dei dati > *bufferLength*|Data<br /><br /> Dati troncati|Lunghezza dei dati in byte<br /><br /> Lunghezza dei dati in byte|n/d<br /><br /> 01004|  
  
 Quando i dati SQL binari vengono convertiti in dati di tipo carattere C, ogni byte (8 bit) dei dati di origine viene rappresentato come due caratteri ASCII. Questi caratteri sono la rappresentazione di caratteri ASCII del numero nel formato esadecimale. Ad esempio, un 00000001 binario viene convertito in "01" e un valore binario 11111111 viene convertito in "FF".  
  
 Il driver converte sempre i singoli byte in coppie di cifre esadecimali e termina la stringa di caratteri con un byte null. Per questo motivo, se *bufferLength* è pari ed è inferiore alla lunghezza dei dati convertiti, non viene usato l'ultimo byte del buffer **TargetValuePtr* . I dati convertiti richiedono un numero pari di byte, il byte successivo all'ultimo è un byte null e l'ultimo byte non può essere usato.  
  
> [!NOTE]  
>  Gli sviluppatori di applicazioni sono sconsigliati di associare dati SQL binari a un tipo di dati carattere C. Questa conversione è in genere inefficiente e lenta.
