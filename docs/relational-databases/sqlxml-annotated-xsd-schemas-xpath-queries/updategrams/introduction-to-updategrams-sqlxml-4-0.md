---
title: Introduzione agli updategram (SQLXML)
description: Informazioni sugli updategram di SQLXML 4.0 che possono essere usati per inserire, aggiornare o eliminare dati in un database.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c06f4934b2932dbaea0228a00f65e2cefb2939b2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491514"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Introduzione sugli updategram (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  È possibile modificare (inserire, aggiornare o eliminare) un database in da un documento XML esistente usando un updategram o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la funzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] OPENXML.  
  
 La funzione OPENXML modifica un database suddividendo il documento XML esistente e fornendo un set di righe che può essere passato a un'istruzione INSERT, UPDATE o DELETE. Con OPENXML, le operazioni vengono eseguite direttamente sulle tabelle del database. OPENXML rappresenta pertanto la scelta più appropriata ogni volte che un provider di set di righe, ad esempio una tabella, può essere visualizzato come origine.  
  
 Analogamente a OPENXML, un updategram consente di inserire, aggiornare o eliminare dati nel database. Un updategram, tuttavia, agisce sulle viste XML fornite dallo schema XSD (o XDR) con annotazioni, applicando, ad esempio, gli aggiornamenti alla vista XML fornita dallo schema di mapping. Lo schema di mapping, a sua volta, dispone delle informazioni necessarie per eseguire il mapping di elementi e attributi XML alle tabelle e alle colonne di database corrispondenti. L'updategram utilizza queste informazioni di mapping per aggiornare le tabelle e le colonne di database.  
  
> [!NOTE]  
>  In questa documentazione si presuppone che l'utente disponga di una certa familiarità con i modelli e il supporto dello schema di mapping in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Introduction to Annotated XSD Schemas &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Per le applicazioni legacy che usano XDR, vedere [Annotated XDR Schemas &#40;deprecato in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Spazi dei nomi necessari nell'updategram  
 Le parole chiave in un updategram, ad esempio , e , sono presenti nello spazio dei nomi **\<sync>** **\<before>** **\<after>** **urn:schemas-microsoft-com:xml-updategram.** Il prefisso dello spazio dei nomi utilizzato è arbitrario. In questa documentazione il prefisso **updg** indica lo spazio dei nomi **updategram.**  
  
## <a name="reviewing-syntax"></a>Esame della sintassi  
 Un updategram è un modello con **\<sync>** blocchi , e che formano la **\<before>** **\<after>** sintassi dell'updategram. Tale sintassi, nella sua forma più semplice, viene illustrata nel codice seguente:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Nelle definizioni seguenti viene descritto il ruolo di ogni blocco:  
  
 **\<before>**  
 Identifica lo stato esistente, denominato anche "stato before", dell'istanza del record.  
  
 **\<after>**  
 Identifica il nuovo stato in cui devono essere modificati i dati.  
  
 **\<sync>**  
 Contiene i **\<before>** blocchi **\<after>** e . Un **\<sync>** blocco può contenere più set di blocchi **\<before>** e **\<after>** . Se sono presenti più set di blocchi e , questi blocchi , anche se sono vuoti, devono **\<before>** **\<after>** essere specificati come coppie. Inoltre, un updategram può avere più di un **\<sync>** blocco. Ogni blocco è un'unità di transazione, ovvero tutte le operazioni nel blocco vengono eseguite o non **\<sync>** **\<sync>** viene eseguita alcuna operazione. Se si specificano **\<sync>** più blocchi in un updategram, l'errore di un blocco non **\<sync>** influisce sugli altri **\<sync>** blocchi.  
  
 L'eliminazione, l'inserimento o l'aggiornamento di un'istanza di record da parte di un updategram dipende dal contenuto dei **\<before>** blocchi e **\<after>** :  
  
-   Se un'istanza di record viene visualizzata solo nel blocco senza un'istanza corrispondente nel blocco **\<before>** , l'updategram **\<after>** esegue un'operazione di eliminazione.  
  
-   Se un'istanza di record viene visualizzata solo **\<after>** nel blocco senza un'istanza corrispondente nel blocco , si tratta di **\<before>** un'operazione di inserimento.  
  
-   Se un'istanza di record viene visualizzata **\<before>** nel blocco e ha un'istanza corrispondente nel blocco , si tratta di **\<after>** un'operazione di aggiornamento. In questo caso, l'updategram aggiorna l'istanza del record ai valori specificati nel **\<after>** blocco .  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Definizione di uno schema di mapping nell'updategram  
 In un updategram l'astrazione XML fornita da uno schema di mapping (sono supportati gli schemi XSD e XDR) può essere implicita o esplicita, ovvero un updategram può essere utilizzato con o senza uno schema di mapping specificato. Se non si specifica uno schema di mapping, l'updategram presuppone un mapping implicito (mapping predefinito), in cui ogni elemento del blocco o del blocco viene mappato a una tabella e **\<before>** l'attributo o l'elemento figlio di ogni elemento viene mappato a una colonna nel **\<after>** database. Se si specifica in modo esplicito uno schema di mapping, gli elementi e gli attributi nell'updategram deve corrispondere agli elementi e agli attributi nello schema di mapping.  
  
### <a name="implicit-default-mapping"></a>Mapping implicito (predefinito)  
 Nella maggior parte dei casi, un updategram che esegue aggiornamenti semplici può non richiedere uno schema di mapping. In questo caso, l'updategram si basa sullo schema di mapping predefinito.  
  
 Nell'updategram seguente viene illustrato il mapping implicito. In questo esempio l'updategram inserisce un nuovo cliente nella tabella Sales.Customer. Poiché questo updategram usa il mapping implicito, l'elemento esegue il mapping alla tabella Sales.Customer e gli attributi CustomerID e SalesPersonID vengono mappati alle colonne corrispondenti nella \<Sales.Customer> tabella Sales.Customer.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>Mapping esplicito  
 Se si specifica uno schema di mapping (XSD o XDR), l'updategram utilizza lo schema per determinare le tabelle e le colonne di database che devono essere aggiornate.  
  
 Se l'updategram esegue un aggiornamento complesso, ad esempio l'inserimento di record in più tabelle in base alla relazione padre-figlio specificata nello schema di mapping, è necessario fornire in modo esplicito lo schema di mapping utilizzando l'attributo **dello schema** di mapping su cui viene eseguito l'updategram.  
  
 Poiché un updategram è un modello, il percorso specificato per lo schema di mapping nell'updategram è relativo al percorso del file di modello (relativo alla posizione di archiviazione dell'updategram). Per altre informazioni, vedere Specifica di uno schema di mapping con annotazioni in un [updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Mapping incentrato sugli elementi e mapping incentrato sugli attributi negli updategram  
 Utilizzando il mapping predefinito, quando lo schema di mapping non viene specificato nell'updategram, gli elementi dell'updategram vengono mappati a tabelle, mentre gli elementi figlio (nel caso di mapping incentrato sugli elementi) e gli attributi (nel caso di mapping incentrato sugli attributi) vengono mappati a colonne.  
  
### <a name="element-centric-mapping"></a>Mapping incentrato sugli elementi  
 In un updategram incentrato sugli elementi, un elemento contiene elementi figlio che indicano le proprietà dell'elemento. Si consideri, ad esempio, l'updategram seguente. **\<Person.Contact>** L'elemento contiene gli elementi figlio e **\<FirstName>** **\<LastName>** . Questi elementi figlio sono proprietà **\<Person.Contact>** dell'elemento .  
  
 Poiché questo updategram non specifica uno schema di mapping, l'updategram usa il mapping implicito, in cui l'elemento esegue il mapping alla tabella Person.Contact e i relativi elementi figlio vengono mappati alle colonne FirstName e **\<Person.Contact>** LastName.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>mapping incentrato sugli attributi  
 In un mapping incentrato sugli attributi, gli elementi includono attributi. Nell'updategram seguente viene utilizzato il mapping incentrato sugli attributi. In questo esempio **\<Person.Contact>** l'elemento è costituito da **attributi FirstName** **e LastName.** Questi attributi sono le proprietà **\<Person.Contact>** dell'elemento . Come nell'esempio precedente, questo updategram non specifica alcuno schema di mapping, quindi si basa sul mapping implicito per eseguire il mapping dell'elemento alla tabella Person.Contact e agli attributi dell'elemento alle rispettive colonne della **\<Person.Contact>** tabella.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>Utilizzo di una combinazione di mapping incentrato sugli elementi e mapping incentrato sugli attributi  
 È possibile specificare una combinazione di mapping incentrato sugli elementi e mapping incentrato sugli attributi, come illustrato nell'updategram seguente. Si noti che **\<Person.Contact>** l'elemento contiene sia un attributo che un elemento figlio. L'updategram si basa inoltre su un mapping implicito. Di conseguenza, **l'attributo FirstName** e l'elemento figlio vengono **\<LastName>** mappati alle colonne corrispondenti nella tabella Person.Contact.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>Utilizzo di caratteri validi in SQL Server ma non validi in XML  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] i nomi di tabella possono includere uno spazio. Questo tipo di nome di tabella, tuttavia, non è valido in XML.  
  
 Per codificare caratteri che sono identificatori validi ma non sono identificatori XML validi, usare '__xHHHH' come valore di codifica, dove HHHH è l'acronimo di codice [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \_ UCS-2 esadecimale a quattro cifre per il carattere nell'ordine \_ bit-first più significativo. Usando questo schema di codifica, uno spazio viene sostituito con x0020 (il codice esadecimale a quattro cifre per uno spazio); Di conseguenza, il nome della tabella [Dettagli ordine] in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] diventa _x005B_Order_x0020_Details_x005D \_ in XML.  
  
 Analogamente, potrebbe essere necessario specificare nomi di elementi in tre parti, ad esempio \<[database].[owner].[table]> . Poiché i caratteri parentesi quadre ([ e ]) non sono validi in XML, è necessario specificare come , dove _x005B è la codifica per la parentesi quadra aperta ([) e _x005D è la codifica per la parentesi quadra chiusa \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> \_ \_ (]).  
  
## <a name="executing-updategrams"></a>Esecuzione di updategram  
 Poiché un updategram è un modello, utilizza tutti i meccanismi di elaborazione di un modello. Per SQLXML 4.0, è possibile eseguire un updategram in una delle modalità seguenti:  
  
-   Inviandolo in un comando ADO.  
  
-   Inviandolo come comando OLE DB.  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza di Updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
