---
title: Usare l'opzione BINARY BASE64 | Microsoft Docs
description: Informazioni su come usare l'opzione BINARY BASE64 in una query SQL per restituire dati binari nel formato di codifica base64.
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, BINARY BASE64 option
ms.assetid: 86a7bb85-7f83-412a-b775-d2c379702fe9
author: RothJa
ms.author: jroth
ms.openlocfilehash: f3f34874dc641eeb080514f5d6acb1bb00eefb5d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477162"
---
# <a name="use-the-binary-base64-option"></a>Utilizzo dell'opzione BINARY BASE64

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Se nella query è specificata l'opzione BINARY BASE64, i dati binari verranno restituiti con la codifica Base64.

Se l'opzione BINARY BASE64 non viene specificata nella query, per impostazione predefinita la modalità AUTO supporta la codifica URL dei dati binari. Viene restituito un riferimento a un URL relativo alla radice virtuale del database. Questo riferimento è al database in cui è stata eseguita la query. Il riferimento restituito può essere usato per accedere ai dati binari effettivi nelle operazioni successive. Per ottenere questo accesso, usare la query dbobject ISAPI SQLXML. La query deve fornire informazioni sufficienti per identificare l'immagine. Tali informazioni possono includere le colonne della chiave primaria.

## <a name="column-alias"></a>Alias di colonna

Non usare un alias per una colonna binaria quando si esegue una query su una vista e si usa la modalità FOR XML AUTO. Se si usa un alias, l'alias viene restituito nella codifica URL dei dati binari. Nelle operazioni successive l'alias non è significativo. Non è possibile usare l'alias non significativo e la codifica URL per recuperare l'immagine.

### <a name="cast-to-a-blob"></a>Eseguire il cast a un BLOB

In una query SELECT, il cast di una colonna a un oggetto binario di grandi dimensioni (BLOB) rende la colonna un'entità temporanea. Essendo temporaneo, il BLOB perde il nome della tabella e il nome della colonna associati. Le query in modalità AUTO genereranno pertanto un errore perché il sistema non è in grado di determinare la posizione in cui inserire questo valore nella gerarchia XML.

Si consideri, ad esempio, la tabella seguente contenente una sola riga.

```sql
CREATE TABLE MyTable (Col1 int PRIMARY KEY, Col2 binary)
INSERT INTO MyTable VALUES (1, 0x7);
```

La query seguente genera un errore, causato dal cast a un oggetto binario di grandi dimensioni (BLOB):

```sql
SELECT Col1,
CAST(Col2 as image) as Col2
FROM MyTable
FOR XML AUTO;
```

Per risolvere il problema, è possibile aggiungere l'opzione BINARY BASE64 alla clausola FOR XML. Se si rimuove il cast, la query genera risultati validi.

```sql
SELECT Col1,
CAST(Col2 as image) as Col2
FROM MyTable
FOR XML AUTO, BINARY BASE64;
```

Il risultato valido previsto è il seguente:

```console
<MyTable Col1="1" Col2="Bw==" />
```

## <a name="see-also"></a>Vedere anche

[Usare la modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)
