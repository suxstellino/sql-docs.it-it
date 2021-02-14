---
description: Supporto del tipo di dati Teradata
title: Supporto del tipo di dati Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346525"
---
# <a name="data-type-support"></a>Supporto dei tipi di dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Per caricare e trasferire dati su un database Teradata, i componenti SSIS usano l'API TPT (Teradata Parallel Transporter) e, pertanto, in SSIS è possibile usare solo i tipi di dati supportati dall'API TPT.

> [!NOTE]
>
> In Teradata, i tipi di dati TIME, TIMESTAMP e INTERVAL vengono gestiti dall'API TPT come stringhe di caratteri a dimensione fissa. Vengono gestiti invece dai componenti SSIS per Teradata come stringhe.

## <a name="data-type-mapping"></a>Mapping dei tipi di dati

Nella tabella seguente vengono illustrati i tipi di dati di database Teradata e il mapping predefinito ai tipi di dati di SSIS. Sono anche indicati i tipi di dati Teradata non supportati.

|Tipo di dati SQL|Tipo di dati SSIS|
|:-|:-|
|DECIMAL/NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|INTEGER|DT_I4|
|FLOAT/REAL/DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TIME<br>TIME(n)|DT_STR|
|timestamp<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR (DT_WSTR per set di caratteri Unicode)<br>**Note**:<br> La lunghezza massima supportata per VARCHAR è 32000. <br> La lunghezza massima consentita per DT_STR è di 8000 caratteri, mentre per DT_WSTR è di 4000 caratteri. Se questi limiti vengono superati, i dati risultano troncati.|
|LONG VARCHAR|Non supportate|
|CLOB|Non supportate|
|BYTE|DT_BYTES<br>**Nota**: La lunghezza massima consentita è di 8000 byte. Se questi limiti vengono superati, i dati risultano troncati.|
|VARBYTE|DT_BYTES<br>**Nota**: La lunghezza massima consentita è di 8000 byte. Se questi limiti vengono superati, i dati risultano troncati.|
|BLOB|Non supportate|

## <a name="next-steps"></a>Passaggi successivi

- Configurare la [gestione connessione Teradata](teradata-connection-manager.md)
- Configurare l'[origine Teradata](teradata-source.md)
- Configurare la [destinazione Teradata](teradata-destination.md)
- Per eventuali domande, visitare [TechCommunity](https://aka.ms/AA6iwdw).
