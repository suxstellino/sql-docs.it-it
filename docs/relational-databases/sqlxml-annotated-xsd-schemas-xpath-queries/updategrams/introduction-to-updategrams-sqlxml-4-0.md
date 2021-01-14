---
title: Introduzione agli updategram (SQLXML)
description: Informazioni sugli updategram SQLXML 4,0 che possono essere utilizzati per inserire, aggiornare o eliminare dati in un database.
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
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a416950247208c682d0fdc923db71357405fd0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170623"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Introduzione sugli updategram (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  È possibile modificare (inserire, aggiornare o eliminare) un database in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] da un documento XML esistente utilizzando un updategram o la [!INCLUDE[tsql](../../../includes/tsql-md.md)] funzione OPENXML.  
  
 La funzione OPENXML modifica un database suddividendo il documento XML esistente e fornendo un set di righe che può essere passato a un'istruzione INSERT, UPDATE o DELETE. Con OPENXML, le operazioni vengono eseguite direttamente sulle tabelle del database. OPENXML rappresenta pertanto la scelta più appropriata ogni volte che un provider di set di righe, ad esempio una tabella, può essere visualizzato come origine.  
  
 Analogamente a OPENXML, un updategram consente di inserire, aggiornare o eliminare dati nel database. Un updategram, tuttavia, agisce sulle viste XML fornite dallo schema XSD (o XDR) con annotazioni, applicando, ad esempio, gli aggiornamenti alla vista XML fornita dallo schema di mapping. Lo schema di mapping, a sua volta, dispone delle informazioni necessarie per eseguire il mapping di elementi e attributi XML alle tabelle e alle colonne di database corrispondenti. L'updategram utilizza queste informazioni di mapping per aggiornare le tabelle e le colonne di database.  
  
> [!NOTE]  
>  In questa documentazione si presuppone che l'utente disponga di una certa familiarità con i modelli e il supporto dello schema di mapping in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [Introduzione agli schemi XSD con Annotazioni &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Per le applicazioni legacy che usano XDR, vedere la pagina relativa agli [schemi XDR con Annotazioni &#40;deprecati in SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Spazi dei nomi necessari nell'updategram  
 Le parole chiave in un updategram, ad esempio **\<sync>** , **\<before>** e **\<after>** , sono presenti nello spazio dei nomi **urn: schemas-microsoft-com: xml-updategram** . Il prefisso dello spazio dei nomi utilizzato è arbitrario. In questa documentazione il prefisso **attributo updg** indica lo spazio dei nomi dell' **updategram** .  
  
## <a name="reviewing-syntax"></a>Esame della sintassi  
 Un updategram è un modello con **\<sync>** **\<before>** blocchi, e **\<after>** che formano la sintassi dell'updategram. Tale sintassi, nella sua forma più semplice, viene illustrata nel codice seguente:  
  
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
 Contiene i **\<before>** **\<after>** blocchi e. Un **\<sync>** blocco può contenere più di un set di **\<before>** **\<after>** blocchi e. Se è presente più di un set di **\<before>** **\<after>** blocchi e, questi blocchi (anche se sono vuoti) devono essere specificati come coppie. Inoltre, un updategram può avere più di un **\<sync>** blocco. Ogni **\<sync>** blocco è costituito da un'unità di transazione (il che significa che tutti gli elementi del **\<sync>** blocco sono completati o che non viene eseguita alcuna operazione). Se si specificano più **\<sync>** blocchi in un updategram, l'errore di un **\<sync>** blocco non influisce sugli altri **\<sync>** blocchi.  
  
 Se un updategram elimina, inserisce o aggiorna un'istanza di record, dipende dal contenuto dei **\<before>** **\<after>** blocchi e:  
  
-   Se un'istanza di record viene visualizzata solo nel **\<before>** blocco senza alcuna istanza corrispondente nel **\<after>** blocco, l'updategram esegue un'operazione di eliminazione.  
  
-   Se un'istanza di record viene visualizzata solo nel **\<after>** blocco senza alcuna istanza corrispondente nel **\<before>** blocco, si tratta di un'operazione di inserimento.  
  
-   Se un'istanza di record viene visualizzata nel **\<before>** blocco e dispone di un'istanza corrispondente nel **\<after>** blocco, si tratta di un'operazione di aggiornamento. In questo caso, l'updategram aggiorna l'istanza del record ai valori specificati nel **\<after>** blocco.  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Definizione di uno schema di mapping nell'updategram  
 In un updategram l'astrazione XML fornita da uno schema di mapping (sono supportati gli schemi XSD e XDR) può essere implicita o esplicita, ovvero un updategram può essere utilizzato con o senza uno schema di mapping specificato. Se non si specifica uno schema di mapping, l'updategram presuppone un mapping implicito (il mapping predefinito), in cui ogni elemento nel **\<before>** blocco o nel **\<after>** blocco viene mappato a una tabella e l'attributo o l'elemento figlio di ogni elemento viene mappato a una colonna nel database. Se si specifica in modo esplicito uno schema di mapping, gli elementi e gli attributi nell'updategram deve corrispondere agli elementi e agli attributi nello schema di mapping.  
  
### <a name="implicit-default-mapping"></a>Mapping implicito (predefinito)  
 Nella maggior parte dei casi, un updategram che esegue aggiornamenti semplici può non richiedere uno schema di mapping. In questo caso, l'updategram si basa sullo schema di mapping predefinito.  
  
 Nell'updategram seguente viene illustrato il mapping implicito. In questo esempio l'updategram inserisce un nuovo cliente nella tabella Sales.Customer. Poiché questo updategram utilizza il mapping implicito, l' \<Sales.Customer> elemento esegue il mapping alla tabella Sales. Customer e gli attributi CustomerID e SalesPersonID vengono mappati alle colonne corrispondenti nella tabella Sales. Customer.  
  
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
  
 Se l'updategram esegue un aggiornamento complesso, ad esempio l'inserimento di record in più tabelle in base alla relazione padre-figlio specificata nello schema di mapping, è necessario specificare in modo esplicito lo schema di mapping utilizzando l'attributo **mapping-schema** sul quale viene eseguito l'updategram.  
  
 Poiché un updategram è un modello, il percorso specificato per lo schema di mapping nell'updategram è relativo al percorso del file di modello (relativo alla posizione di archiviazione dell'updategram). Per ulteriori informazioni, vedere [specifica di uno schema di mapping con annotazioni in un Updategram &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Mapping incentrato sugli elementi e mapping incentrato sugli attributi negli updategram  
 Utilizzando il mapping predefinito, quando lo schema di mapping non viene specificato nell'updategram, gli elementi dell'updategram vengono mappati a tabelle, mentre gli elementi figlio (nel caso di mapping incentrato sugli elementi) e gli attributi (nel caso di mapping incentrato sugli attributi) vengono mappati a colonne.  
  
### <a name="element-centric-mapping"></a>Mapping incentrato sugli elementi  
 In un updategram incentrato sugli elementi, un elemento contiene elementi figlio che indicano le proprietà dell'elemento. Si consideri, ad esempio, l'updategram seguente. L' **\<Person.Contact>** elemento contiene gli **\<FirstName>** **\<LastName>** elementi figlio e. Questi elementi figlio sono proprietà dell' **\<Person.Contact>** elemento.  
  
 Poiché questo updategram non specifica uno schema di mapping, l'updategram utilizza il mapping implicito, in cui viene eseguito il mapping dell' **\<Person.Contact>** elemento alla tabella Person. Contact e i relativi elementi figlio vengono mappati alle colonne FirstName e LastName.  
  
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
 In un mapping incentrato sugli attributi, gli elementi includono attributi. Nell'updategram seguente viene utilizzato il mapping incentrato sugli attributi. In questo esempio l' **\<Person.Contact>** elemento è costituito dagli attributi **FirstName** e **LastName** . Questi attributi sono le proprietà dell' **\<Person.Contact>** elemento. Come nell'esempio precedente, questo updategram non specifica alcuno schema di mapping, pertanto si basa sul mapping implicito per eseguire il mapping dell' **\<Person.Contact>** elemento alla tabella Person. Contact e degli attributi dell'elemento alle rispettive colonne nella tabella.  
  
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
 È possibile specificare una combinazione di mapping incentrato sugli elementi e mapping incentrato sugli attributi, come illustrato nell'updategram seguente. Si noti che l' **\<Person.Contact>** elemento contiene sia un attributo sia un elemento figlio. L'updategram si basa inoltre su un mapping implicito. Pertanto, l'attributo **FirstName** e l' **\<LastName>** elemento figlio vengono mappati alle colonne corrispondenti nella tabella Person. Contact.  
  
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
  
 Per codificare i caratteri che sono [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificatori validi ma non sono identificatori XML validi, usare ' __xHHHH \_ \_ ' come valore di codifica, dove HHHH rappresenta il codice UCS-2 esadecimale a quattro cifre per il carattere nell'ordine del primo bit più significativo. Utilizzando questo schema di codifica, un carattere spazio viene sostituito con x0020 (il codice esadecimale a quattro cifre per uno spazio); in questo modo, il nome della tabella [Order Details] in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] diventa _x005B_Order_x0020_Details_x005D \_ in XML.  
  
 Analogamente, potrebbe essere necessario specificare nomi di elementi in tre parti, ad esempio \<[database].[owner].[table]> . Poiché i caratteri parentesi quadre ([e]) non sono validi in XML, è necessario specificarlo come \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> , dove _x005B \_ è la codifica per la parentesi quadra aperta ([) e _x005D \_ è la codifica per la parentesi quadra chiusa (]).  
  
## <a name="executing-updategrams"></a>Esecuzione di updategram  
 Poiché un updategram è un modello, utilizza tutti i meccanismi di elaborazione di un modello. Per SQLXML 4.0, è possibile eseguire un updategram in una delle modalità seguenti:  
  
-   Inviandolo in un comando ADO.  
  
-   Inviandolo come comando OLE DB.  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza degli updategram &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
