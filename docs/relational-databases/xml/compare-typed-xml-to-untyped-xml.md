---
title: Confrontare dati XML tipizzati con dati XML non tipizzati | Microsoft Docs
description: Informazioni sulle differenze tra XML tipizzato e non tipizzato.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- xml data type [SQL Server], variables
- parameters [XML in SQL Server]
- facets [XML in SQL Server]
- xml data type [SQL Server], columns
- untyped XML
- xml data type [SQL Server], typed xml
- XML [SQL Server], typed
- variables [XML in SQL Server], creating
- xml data type [SQL Server], untyped xml
- columns [XML in SQL Server], creating
- typed XML
- document mode processing [SQL Server]
- XML [SQL Server], untyped
- xml data type [SQL Server], parameters
ms.assetid: 4bc50af9-2f7d-49df-bb01-854d080c72c7
author: rothja
ms.author: jroth
ms.openlocfilehash: d5be6303cb92befc15d5f5cfcc3062deb31238ab
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491019"
---
# <a name="compare-typed-xml-to-untyped-xml"></a>Confronto dati XML tipizzati con dati XML non tipizzati
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile creare variabili, parametri e colonne di tipo **xml** . È inoltre possibile associare una raccolta XML Schema a una variabile, un parametro o una colonna di tipo **xml** . In questo caso, l'istanza del tipo di dati **xml** viene definita *tipizzata*. In caso contrario, l'istanza XML è definita *non tipizzata*.  
  
## <a name="well-formed-xml-and-the-xml-data-type"></a>Tipi di dati XML corretti e xml  
 Il tipo di dati **xml** usa il tipo di dati standard ISO **xml** . Ciò consente pertanto l'archiviazione in una colonna XML non tipizzata di documenti di formato XML 1.0 corretto e frammenti di contenuto XML, con nodi di testo e un numero arbitrario di elementi di livello principale. Il sistema verifica che il formato dei dati sia corretto, non richiede che la colonna sia associata a XML Schema e rifiuta i dati con formato non corretto in senso esteso. Questo vale anche per le variabili e i parametri XML non tipizzati.  
  
## <a name="xml-schemas"></a>XML Schema  
 XML Schema fornisce quanto segue:  
  
-   **Vincoli di convalida.** Ogni volta che un'istanza xml tipizzata viene assegnata o modificata, tale istanza viene convalidata da SQL Server.  
  
-   **Informazioni sui tipi di dati.** Gli schemi offrono informazioni sui tipi di attributi e di elementi presenti nell'istanza del tipo di dati **xml** . Le informazioni sul tipo offrono una semantica operativa più precisa ai valori contenuti nell'istanza rispetto a quanto sia possibile con **xml** non tipizzato. ad esempio è possibile eseguire le operazioni aritmetiche decimali su un valore decimale, ma non su un valore stringa. Per questo motivo, i tipi di dati XML archiviati possono essere estremamente più compatti rispetto ai dati XML non tipizzati.  
  
## <a name="choosing-typed-or-untyped-xml"></a>Scelta tra dati XML tipizzati o non tipizzati  
 Usare il tipo di dati **xml** non tipizzato nelle situazioni seguenti:  
  
-   Non è disponibile uno schema per i dati XML.  
  
-   Gli schemi sono disponibili ma non si desidera che i dati vengano convalidati dal server. Questo avviene talvolta quando un'applicazione esegue una convalida sul lato client prima di archiviare i dati sul server, archivia temporaneamente dati XML non validi in base allo schema oppure utilizza componenti di schema non supportati dal server.  
  
 Usare il tipo di dati **xml** tipizzato nelle situazioni seguenti:  
  
-   Gli schemi per i dati XML sono disponibili e si desidera che i dati XML vengano convalidati dal server in base a XML Schema.  
  
-   Si desidera avvalersi delle ottimizzazioni query e dell'archiviazione basandosi sulle informazioni sui tipi.  
  
-   Si desidera avvalersi delle informazioni sui tipi durante la compilazione delle query.  
  
 Nelle colonne, nelle variabili e nei parametri XML tipizzati è possibile archiviare documenti o contenuto XML. Nella dichiarazione è tuttavia necessario specificare, tramite un flag, se si desidera archiviare un documento o del contenuto. È inoltre necessario fornire la raccolta XML Schema. Specificare DOCUMENT se ogni istanza XML include esattamente un elemento di livello principale, CONTENT in caso contrario. Nella verifica dei tipi eseguita durante la compilazione delle query il compilatore utilizza il flag DOCUMENT per derivare gli elementi singleton di livello principale.  
  
## <a name="creating-typed-xml"></a>Creazione di dati XML tipizzati  
 Prima di creare variabili, parametri o colonne di tipo **xml** tipizzato, è necessario registrare la raccolta XML Schema usando [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md). È quindi possibile associare la raccolta XML Schema alle variabili, ai parametri o alle colonne del tipo di dati **xml** .  
  
 Negli esempi seguenti, per specificare il nome della raccolta XML Schema viene utilizzata una convenzione di denominazione in due parti. La prima parte è il nome dello schema e la seconda parte è il nome della raccolta XML Schema.  
  
### <a name="example-associating-a-schema-collection-with-an-xml-type-variable"></a>Esempio: Associazione di una raccolta di schemi a una variabile di tipo XML  
 L'esempio seguente crea una variabile di tipo **xml** alla quale viene associata una raccolta di schemi. La raccolta di schemi specificata è già stata importata nel database **AdventureWorks** .  
  
```  
DECLARE @x xml (Production.ProductDescriptionSchemaCollection);   
```  
  
### <a name="example-specifying-a-schema-for-an-xml-type-column"></a>Esempio: Specifica di uno schema per una colonna di tipo XML  
 L'esempio seguente crea una tabella con una colonna di tipo **xml** e specifica uno schema per la colonna:  
  
```  
CREATE TABLE T1(  
 Col1 int,   
 Col2 xml (Production.ProductDescriptionSchemaCollection)) ;  
```  
  
### <a name="example-passing-an-xml-type-parameter-to-a-stored-procedure"></a>Esempio: Passaggio di un parametro di tipo XML a una stored procedure  
 L'esempio seguente passa un parametro di tipo **xml** a una stored procedure e specifica uno schema per la variabile:  
  
```  
CREATE PROCEDURE SampleProc   
  @ProdDescription xml (Production.ProductDescriptionSchemaCollection)   
AS   
...  
```  
  
 Per la raccolta XML Schema, si noti quanto segue:  
  
-   Una raccolta XML Schema è disponibile solo nel database in cui è stata registrata tramite [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
-   Se si esegue il cast da una stringa a un tipo di dati **xml** tipizzato, durante l'analisi vengono inoltre eseguite la convalida e la tipizzazione, in base agli spazi dei nomi XML Schema della raccolta specificata.  
  
-   È possibile eseguire il cast da un tipo di dati **xml** tipizzato a un tipo di dati **xml** non tipizzato e viceversa.  
  
 Per altre informazioni sugli altri modi disponibili per generare codice XML in SQL Server, vedere [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md). Dopo avere generato il codice XML, è possibile assegnarlo a una variabile con tipo di dati **xml** o archiviarlo in colonne di tipo **xml** per elaborazioni successive.  
  
 Nella gerarchia dei tipi di dati, il tipo di dati **xml** è visualizzato sotto al tipo **sql_variant** e ai tipi definiti dall'utente, ma sopra ai tipi predefiniti.  
  
### <a name="example-specifying-facets-to-constrain-a-typed-xml-column"></a>Esempio: Specifica di facet per vincolare una colonna xml tipizzata  
 È possibile vincolare le colonne **xml** tipizzate in modo tale che in ogni istanza archiviata in tali colonne siano consentiti solo elementi principali singoli. A tale scopo, è possibile specificare il facet facoltativo `DOCUMENT` durante la creazione di una tabella, come illustrato nell'esempio seguente:  
  
```  
CREATE TABLE T(Col1 xml   
   (DOCUMENT Production.ProductDescriptionSchemaCollection));  
GO  
DROP TABLE T;  
GO  
```  
  
 Per impostazione predefinita, le istanze archiviate nella colonna **xml** tipizzata sono archiviate come contenuto XML e non come documenti XML. Ciò consente quanto segue:  
  
-   Zero elementi principali oppure nessuno  
  
-   Nodi di testo negli elementi principali  
  
 È inoltre possibile specificare in modo esplicito questo comportamento aggiungendo il facet `CONTENT` , come illustrato nell'esempio seguente:  
  
```  
CREATE TABLE T(Col1 xml(CONTENT Production.ProductDescriptionSchemaCollection));  
GO -- Default  
```  
  
 Si noti che è possibile specificare i facet facoltativi DOCUMENT/CONTENT in qualsiasi posizione in cui viene definito il tipo **xml** (xml tipizzato). Ad esempio, quando si crea una variabile **xml** tipizzata, è possibile aggiungere il facet DOCUMENT/CONTENT, come illustrato nell'esempio seguente:  
  
```  
declare @x xml (DOCUMENT Production.ProductDescriptionSchemaCollection);  
```  
  
## <a name="document-type-definition-dtd"></a>Definizione del tipo di documento (DTD, Document Type Definition)  
 Le colonne, le variabili e i parametri con tipo di dati **xml** possono essere tipizzati tramite XML Schema, ma non tramite una definizione DTD. Sia per i tipi di dati XML tipizzati che non tipizzati, è tuttavia possibile utilizzare DTD inline per specificare valori predefiniti e sostituire i riferimenti alle entità con le corrispondenti forme espanse.  
  
 È possibile convertire DTD in documenti di XML Schema tramite strumenti di terze parti e caricare XML Schema nel database.  
  
## <a name="upgrading-typed-xml-from-sql-server-2005"></a>Aggiornamento di dati XML tipizzati da SQL Server 2005  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] include diverse estensioni per il supporto di XML Schema, compreso il supporto per la convalida di tipo lax, la gestione migliorata dei dati delle istanze **xs:date**, **xs:time** e **xs:dateTime** e il supporto aggiunto per i tipi elenco e unione. Nella maggior parte dei casi le modifiche non influiscono sull'esperienza dell'aggiornamento. Se tuttavia è stata utilizzata una raccolta XML Schema in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] che consentiva valori di tipo **xs:date**, **xs:time** o **xs:dateTime** (o qualsiasi sottotipo), quando si collega il database [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a una versione successiva di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]si verificano i seguenti passaggi di aggiornamento:  
  
1.  Per ogni colonna XML, tipizzata con una raccolta XML Schema che contiene elementi o attributi tipizzati come **xs:anyType**, **xs:anySimpleType**, **xs:date** o uno qualsiasi dei sottotipi, **xs:time** o uno qualsiasi dei suoi sottotipi oppure **xs:dateTime** o uno qualsiasi dei suoi sottotipi o i tipi unione ed elenco che contengono uno di questi tipi, si verifica quanto riportato di seguito:  
  
    1.  Tutti gli indici XML nella colonna saranno disabilitati.  
  
    2.  Tutti i valori [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] continueranno a essere rappresentati nel fuso orario Z, perché sono stati normalizzati al fuso orario Z.  
  
    3.  Qualsiasi valore **xs:date** o **xs:dateTime** precedente al 1° gennaio dell'anno 1 determinerà un errore di runtime quando l'indice viene ricompilato oppure viene eseguita un'istruzione XQuery o SML-DML rispetto al tipo di dati XML contenente quel valore.  
  
2.  Ogni anno negativo nei facet **xs:date** o **xs:dateTime** o nei valori predefiniti in una raccolta XML Schema sarà aggiornato automaticamente al valore più piccolo consentito dal tipo di base **xs:date** o **xs:dateTime** (ad esempio, 0001-01-01T00: 00:00.0000000 Z per **xs:dateTime**).  

 Si noti che ancora è possibile utilizzare una semplice istruzione Select di SQL per recuperare il tipo di dati XML intero, anche se contiene anni negativi. Si consiglia di sostituire gli anni negativi con un anno all'interno del nuovo intervallo supportato o modificare il tipo dell'elemento o dell'attributo in **xs:string**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md)   
 [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
