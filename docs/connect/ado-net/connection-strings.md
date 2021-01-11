---
title: Stringhe di connessione
description: Informazioni sulle stringhe di connessione nel provider di dati Microsoft SqlClient per SQL Server, che contengono informazioni di inizializzazione trasmesse come parametro da un provider di dati a un'origine dati.
ms.date: 11/13/2020
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 257ed7d43f8ab204c7c7e7575c69251be6f2efdf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771628"
---
# <a name="connection-strings-in-adonet"></a>Stringhe di connessione in ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una stringa di connessione contiene informazioni di inizializzazione che vengono passate come parametro da un provider di dati a un'origine dati. Il provider di dati riceve la stringa di connessione come valore della proprietà <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType>. Il provider analizza la stringa di connessione e garantisce che la sintassi sia corretta e che le parole chiave siano supportate. Quindi il metodo <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> passa i parametri di connessione analizzati all'origine dati. L'origine dati esegue una convalida ulteriore e stabilisce una connessione.

## <a name="connection-string-syntax"></a>Sintassi della stringa di connessione

Una stringa di connessione è un elenco delimitato da punti e virgola composto da coppie di parametri chiave/valore:

```csharp
keyword1=value; keyword2=value;
```

Le parole chiave non fanno distinzione tra maiuscole e minuscole. I valori, tuttavia, possono fare distinzione tra maiuscole e minuscole, a seconda dell'origine dati. Sia le parole chiave che i valori possono contenere [spazi vuoti](https://en.wikipedia.org/wiki/Whitespace_character#Unicode). Lo spazio vuoto iniziale e finale viene ignorato nelle parole chiave e nei valori non racchiusi tra virgolette.

Se un valore contiene il punto e virgola, [caratteri di controllo Unicode](https://en.wikipedia.org/wiki/Unicode_control_characters) o uno spazio vuoto iniziale o finale, deve essere racchiuso tra virgolette singole o doppie. Esempio:

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

Il carattere di inclusione potrebbe non essere presente nel valore che racchiude. Pertanto, un valore contenente virgolette singole può essere racchiuso solo tra virgolette doppie e viceversa:

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

È anche possibile tralasciare il carattere di inclusione usandone due insieme:

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

Le virgolette stesse, così come il segno di uguale, non richiedono caratteri di escape, pertanto sono valide le seguenti stringhe di connessione:

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

Poiché ogni valore viene letto fino al punto e virgola successivo o alla fine della stringa, il valore nel secondo esempio è `a=b=c`e il punto e virgola finale è facoltativo.

Tutte le stringhe di connessione condividono la stessa sintassi di base descritta in precedenza. Il set di parole chiave riconosciute dipende dal provider. Il provider di dati *Microsoft SqlClient* per *SQL Server* supporta molte parole chiave di API precedenti, ma è in genere più flessibile e accetta sinonimi per molte delle parole chiave comuni della stringa di connessione.

Gli errori di ortografia possono produrre errori. Ad esempio `Integrated Security=true` è valido, mentre `IntegratedSecurity=true` genera un errore.

Le stringhe di connessione create manualmente in fase di esecuzione da input dell'utente non convalidato sono vulnerabili ad attacchi injection alle stringhe e compromettono la sicurezza nell'origine dati. Per risolvere questi problemi, è stata creata la classe <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. Questo [generatore di stringhe di connessione](connection-string-builders.md) espone i parametri come proprietà fortemente tipizzate e consente di convalidare la stringa di connessione prima che venga inviata all'origine dati.

## <a name="in-this-section"></a>Contenuto della sezione

[Generatore di stringhe di connessione](connection-string-builders.md)\
Viene illustrato come usare la classe `ConnectionStringBuilder` per costruire stringhe di connessione valide in fase di esecuzione.

[Stringhe di connessione e file di configurazione](connection-strings-and-configuration-files.md)\
Viene illustrato come archiviare e recuperare le stringhe di connessione nei file di configurazione.

[Sintassi della stringa di connessione](connection-string-syntax.md)\
Viene descritto come configurare stringhe di connessione specifiche del provider per `SqlClient`.

[Protezione delle informazioni di connessione](protecting-connection-information.md)\
Vengono illustrate tecniche per proteggere le informazioni usate per la connessione a un'origine dati.

## <a name="see-also"></a>Vedere anche

- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
