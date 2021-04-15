---
title: Definire la serializzazione di dati XML | Microsoft Docs
description: Informazioni sulle regole usate per la serializzazione di dati XML in SQL Server.
ms.custom: ''
ms.date: 12/07/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- entitization rules [XML in SQL Server]
- serialization
- reparsing serialized XML structures
- encoding [XML in SQL Server]
- XML [SQL Server], serialization
- xml data type [SQL Server], serialization
- typed XML
ms.assetid: 42b0b5a4-bdd6-4a60-b451-c87f14758d4b
author: rothja
ms.author: jroth
ms.openlocfilehash: f40c6c7aedda2cfab4747a271ff5f12e395ecd48
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488819"
---
# <a name="define-the-serialization-of-xml-data"></a>Definire la serializzazione di dati XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Quando si esegue il cast esplicito o implicito del tipo di dati xml a un tipo SQL stringa o binario, il contenuto del tipo di dati xml verrà serializzato in base alle regole illustrate in questo argomento.  
  
## <a name="serialization-encoding"></a>Codifica della serializzazione  
 Se il tipo SQL di destinazione è VARBINARY, il risultato viene serializzato nel formato UTF-16 preceduto da un indicatore dell'ordine dei byte UTF-16, ma senza una dichiarazione XML. Se il tipo di destinazione è di grandezza troppo ridotta, viene generato un errore.  
  
 Ad esempio:  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as VARBINARY(MAX))  
```  
  
 Risultato:  
  
```console
0xFFFE3C0094032F003E00  
```  
  
 Se il tipo SQL di destinazione è NVARCHAR o NCHAR, il risultato viene serializzato nel formato UTF-16 non preceduto dall'indicatore dell'ordine dei byte e senza una dichiarazione XML. Se il tipo di destinazione è di grandezza troppo ridotta, viene generato un errore.  
  
 Ad esempio:  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as NVARCHAR(MAX))  
```  
  
 Risultato:  
  
```console
<Δ/>  
```  
  
 Se il tipo di destinazione SQL è VARCHAR o CHAR, il risultato viene serializzato nella codifica corrispondente alla tabella codici delle regole di confronto del database, senza un indicatore dell'ordine dei byte o una dichiarazione XML. Se il tipo di destinazione è di grandezza troppo ridotta o se non è possibile eseguire il mapping del valore alla tabella codici delle regole di confronto di destinazione, viene generato un errore.  
  
 Ad esempio:  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as VARCHAR(MAX))  
```  
  
 In questo caso è possibile che venga generato un errore se la tabella codici delle regole di confronto corrente non è in grado di rappresentare il carattere Unicode Δ, oppure tale carattere verrà rappresentato nella codifica specifica.  
  
 Per la restituzione dei risultati XML al lato client, i dati verranno inviati con la codifica UTF-16. Il provider sul lato client esporrà quindi i dati in base alle regole delle relative API.  
  
## <a name="serialization-of-the-xml-structures"></a>Serializzazione delle strutture XML  
 Il contenuto di un tipo di dati **xml** viene serializzato nel modo standard. In particolare, viene eseguito il mapping dei nodi elemento al markup dell'elemento e il mapping dei nodi di testo al contenuto testuale. Nelle sezioni seguenti vengono tuttavia illustrate le situazioni in cui i caratteri vengono sostituiti con entità e le modalità di serializzazione dei valori atomici tipizzati.  
  
## <a name="entitization-of-xml-characters-during-serialization"></a>Sostituzione dei caratteri XML con entità durante la serializzazione  
 È consigliabile analizzare tutte le strutture XML serializzate. La serializzazione di alcuni caratteri deve pertanto prevedere la sostituzione con entità, in modo tale da mantenere per i caratteri la possibilità di eseguire il round trip durante la fase di normalizzazione del parser XML. La sostituzione di alcuni caratteri con entità deve tuttavia garantire che il documento sia ben formato e possa pertanto essere analizzato. Di seguito sono illustrate le regole della sostituzione con entità applicabili alla serializzazione:  
  
-   I caratteri &, \<, and > vengono sempre sostituiti rispettivamente con le entità `&amp;`, `&lt;` e `&gt;` se sono presenti in un valore di attributo o nel contenuto di un elemento.  
  
-   SQL Server racchiude i valori di attributo tra virgolette singole (U+0022) e pertanto la virgoletta nei valori di attributo viene sostituita con l'entità `&quot;`.  
  
-   Una coppia di surrogati viene sostituita dall'entità rappresentata da un riferimento a un singolo carattere numerico, se il cast viene eseguito solo nel server. Ad esempio la coppia di surrogati U+D800 U+DF00 viene sostituita dall'entità rappresentata dal riferimento al carattere numerico `&#x00010300;`.  
  
-   Per evitare che una tabulazione TAB (U+0009) e un avanzamento di riga (LF, U+000A) vengano normalizzati durante l'analisi, vengono sostituiti con le entità rappresentate dai relativi riferimenti a caratteri numerici, rispettivamente `&#x9;` e `&#xA;`, all'interno dei valori di attributo.  
  
-   Per evitare che un ritorno a capo (CR, U+000D) venga normalizzato durante l'analisi, viene sostituito con l'entità rappresentata dal relativo riferimento a un carattere numerico, `&#xD;` all'interno dei valori di attributo e nel contenuto dell'elemento.  
  
-   Per proteggere i nodi di testo che contengono solo spazi vuoti, uno degli spazi vuoti (in genere l'ultimo) viene sostituito con l'entità rappresentata dal relativo riferimento a un carattere numerico. In questo modo, durante l'analisi il nodo di testo con spazi vuoti viene mantenuto, indipendentemente da come vengono gestiti gli spazi vuoti durante l'analisi.  
  
 Ad esempio:  
  
```sql
declare @u NVARCHAR(50)  
set @u = N'<a a="  
    '+NCHAR(0xD800)+NCHAR(0xDF00)+N'>">   '+NCHAR(0xA)+N'</a>'  
select CAST(CONVERT(XML,@u,1) as NVARCHAR(50))  
```  
  
 Risultato:  
  
```console
<a a="  
    𐌀>">     
</a>  
```  
  
 Se non si vuole applicare l'ultima regola relativa agli spazi vuoti, è possibile usare l'opzione esplicita CONVERT 1 quando si esegue il cast dal tipo **xml** a un tipo string o binary. Ad esempio, per evitare la sostituzione con entità, è possibile specificare quanto segue:  
  
```sql
select CONVERT(NVARCHAR(50), CONVERT(XML, '<a>   </a>', 1), 1)  
```  
  
 Si noti che il [metodo query() (tipo di dati xml)](../../t-sql/xml/query-method-xml-data-type.md) restituisce un'istanza del tipo di dati xml. Qualsiasi risultato del metodo **query()** per cui viene eseguito il cast a un tipo string o binary viene pertanto sostituito con entità in base alle regole descritte in precedenza. Per evitare che i valori stringa ottenuti vengano sostituiti con entità, è consigliabile usare invece il [metodo value() (tipo di dati xml)](../../t-sql/xml/value-method-xml-data-type.md) . Di seguito è riportato un esempio d'uso del metodo **query()** :  
  
```sql
declare @x xml  
set @x = N'<a>This example contains an entitized char: <.</a>'  
select @x.query('/a/text()')  
```  
  
 Risultato:  
  
```console
This example contains an entitized char: <.  
```  
  
 Di seguito è riportato un esempio d'uso del metodo **value()** :  
  
```sql
select @x.value('(/a/text())[1]', 'nvarchar(100)')  
```  
  
 Risultato:  
  
```console
This example contains an entitized char: <.  
```  
  
## <a name="serializing-a-typed-xml-data-type"></a>Serializzazione di un tipo di dati xml tipizzato  
 Un'istanza di un tipo di dati **xml** tipizzato contiene valori tipizzati in base ai relativi tipi XML Schema. Tali valori vengono serializzati in base al tipo XML Schema nel stesso formato generato dal cast di XQuery a xs:string. Per altre informazioni, vedere [Regole del cast dei tipi in XQuery](../../xquery/type-casting-rules-in-xquery.md).  
  
 Ad esempio, il valore xs:double 1.34e1 viene serializzato in 13.4, come illustrato nell'esempio seguente:  
  
```sql
declare @x xml  
set @x =''  
select CAST(@x.query('1.34e1') as nvarchar(50))  
```  
  
 Viene restituito il valore stringa 13.4.  
  
## <a name="see-also"></a>Vedere anche  
 [Regole del cast dei tipi in XQuery](../../xquery/type-casting-rules-in-xquery.md)   
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
 
