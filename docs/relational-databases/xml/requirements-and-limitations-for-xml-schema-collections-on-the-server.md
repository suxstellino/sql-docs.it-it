---
title: Requisiti e limitazioni (raccolte XML Schema) | Microsoft Docs
description: Informazioni sui requisiti e sulle limitazioni relativi alla modifica delle raccolte di XML Schema in SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- identifiers [XML schema collections]
- XML schema collections [SQL Server], limitations
- substitution groups [XML in SQL Server]
- XML schema collections [SQL Server], guidelines
- lax validation
- enumeration facets [XML in SQL Server]
- decimal precision [XML in SQL Server]
- repeated XML schema collection values
- schema collections [SQL Server], limitations
- time zones [XML in SQL Server]
- precision decimals [XML in SQL Server]
- schema collections [SQL Server], guidelines
- lexical representation
ms.assetid: c2314fd5-4c6d-40cb-a128-07e532b40946
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 438b875e03a74aff6e26e886eaee1eaa9c396c84
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487696"
---
# <a name="requirements-and-limitations-for-xml-schema-collections-on-the-server"></a>Requisiti e limitazioni per l'utilizzo di raccolte di XML Schema nel server
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Esistono alcune limitazioni relative alla convalida del linguaggio XSD (XML Schema Definition) per le colonne SQL che usano i il tipo di dati **xml** . La tabella seguente fornisce informazioni dettagliate su tali limitazioni, nonché alcune linee guida per modificare lo schema XSD in modo che sia supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli argomenti in questa sezione forniscono informazioni aggiuntive su limiti specifici e indicazioni per il loro utilizzo.  
  
|Elemento|Limitazione|  
|----------|----------------|  
|**minOccurs** e **maxOccurs**|I valori degli attributi **minOccurs** e **maxOccurs** devono essere contenibili in Integer a 4 byte. Gli schemi che non sono conformi vengono rifiutati dal server.|  
|**\<xsd:choice>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta gli schemi che includono una particella **\<xsd:choice>** senza elementi figlio, a meno che la particella non sia definita con un attributo **minOccurs** di valore pari a zero.|  
|**\<xsd:include>**|Attualmente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta questo elemento. Gli elementi XML Schema che includono tale elemento vengono rifiutati dal server.<br /><br /> Per risolvere questo problema, è possibile eseguire la pre-elaborazione di XML Schema che includono la direttiva **\<xsd:include>** per copiare e unire il contenuto di tutti gli schemi inclusi in un singolo schema da caricare nel server. Per alte informazioni, vedere [Pre-elaborazione di uno schema per unire schemi inclusi](../../relational-databases/xml/preprocess-a-schema-to-merge-included-schemas.md).|  
|**\<xsd:key>** , **\<xsd:keyref>** e **\<xsd:unique>**|Attualmente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta questi seguenti vincoli basati su XSD per imporre l'univocità o stabilire chiavi e relativi riferimenti. Non è possibile registrare XML Schema che contengono questi elementi.|  
|**\<xsd:redefine>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta questo elemento. Per informazioni su un'altra modalità di aggiornamento degli schemi, vedere [Elemento &#60;xsd:redefine&#62;](../../relational-databases/xml/the-xsd-redefine-element.md).|  
|Valori **\<xsd:simpleType>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta solo la precisione in millisecondi per i tipi semplici che hanno componenti per i secondi diversi da **xs:time** e **xs:dateTime** e una precisione di 100 nanosecondi per **xs:time** e **xs:dateTime**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pone limiti a tutte le enumerazioni di tipo semplice XSD riconosciute.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta l'utilizzo del valore "NaN" nelle dichiarazioni **\<xsd:simpleType>** .<br /><br /> Per alte informazioni, vedere[Valori per dichiarazioni &#60;xsd:simpleType&#62;](../../relational-databases/xml/values-for-xsd-simpletype-declarations.md).|  
|**xsi:schemaLocation** e **xsi:noNamespaceSchemaLocation**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignora questi attributi se sono presenti nei dati dell'istanza XML inseriti in una colonna o una variabile con tipo di dati **xml** .|  
|**xs:QName**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta tipi derivati da **xs:QName** che usano un elemento di restrizione XML Schema.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta tipi di unione con **xs:QName** come elemento membro.<br /><br /> Per altre informazioni, vedere [Xs:Tipo QName](../../relational-databases/xml/the-xs-qname-type.md).|  
|Aggiunta di membri a un gruppo di sostituzione esistente|Non è possibile aggiungere membri a un gruppo di sostituzione esistente in una raccolta di XML Schema. La restrizione relativa a un gruppo di sostituzione in un elemento XML Schema impone che l'elemento Head e tutti gli elementi membri corrispondenti vengano definiti nella stessa istruzione {CREATE &#124; ALTER} XML SCHEMA COLLECTION.|  
|Forme canoniche e restrizioni di pattern|La rappresentazione canonica di un valore non deve violare la restrizione del pattern per il tipo corrispondente. Per altre informazioni, vedere [Forme canoniche e restrizioni di pattern](../../relational-databases/xml/canonical-forms-and-pattern-restrictions.md).|  
|Facet di enumerazione|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta XML Schema con tipi che dispongono di facet basati su pattern o di enumerazioni che violano tali facet.|  
|Facet per la lunghezza|I facet **length**, **minLength** e **maxLength** vengono archiviati come tipo **long** , un tipo a 32 bit. Pertanto, l'intervallo di valori accettabili per tali valori è 2^31.|  
|Attributo ID|Ogni componente del XML Schema può disporre di un relativo attributo ID. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impone l'univocità delle dichiarazioni **\<xsd:attribute>** di tipo **ID**, ma non archivia tali valori. L'ambito di applicazione dell'univocità è l'istruzione {CREATE &#124; ALTER} XML SCHEMA COLLECTION.|  
|Tipo ID|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta gli elementi di tipo **xs:ID**, **xs:IDREF** o **xs:IDREFS**. Uno schema potrebbe non dichiarare elementi di questo tipo, o elementi derivati dalla restrizione o dall'estensione da questo tipo.|  
|Spazio dei nomi locale|Lo spazio dei nomi locale deve essere specificato in modo esplicito per l'elemento **\<xsd:any>** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta schemi che usano una stringa vuota ("") come valore per l'attributo dello spazio dei nomi. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede invece l'utilizzo esplicito di "##local" per indicare un elemento o un attributo non qualificato come istanza del carattere jolly.|  
|Tipo misto e contenuto semplice|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta la restrizione di un tipo misto in un contenuto semplice. Per altre informazioni, vedere [Tipo misto e contenuto semplice](../../relational-databases/xml/mixed-type-and-simple-content.md).|  
|Tipo NOTATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta il tipo NOTATION.|  
|Condizioni di memoria insufficiente|Durante l'utilizzo di raccolte di XML Schema di grandi dimensioni, può verificarsi una condizione di memoria insufficiente. Per le soluzioni a questo problema. vedere [Raccolte di XML Schema di grandi dimensioni e condizioni di memoria insufficiente](../../relational-databases/xml/large-xml-schema-collections-and-out-of-memory-conditions.md).|  
|Valori ripetuti|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta gli schemi in cui il blocco o l'attributo finale include valori ripetuti quali "restriction restriction" ed "extension extension".|  
|Identificatori dei componenti di schema|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limita gli identificatori dei componenti di schema a una lunghezza massima di 1000 caratteri Unicode. Non vengono inoltre supportate le coppie di caratteri surrogati all'interno degli identificatori.|  
|Informazioni sul fuso orario|In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive le informazioni sul fuso orario sono completamente supportate per i valori **xs:date**, **xs:time** e **xs:dateTime** per la convalida di XML Schema. Con la modalità di compatibilità con le versioni precedenti di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , le informazioni sul fuso orario sono sempre normalizzate in Coordinated Universal Time (ora di Greenwich). Per gli elementi di tipo **dateTime** , il server convertirà l'ora fornita in GMT usando il valore di offset ("-05:00") e restituendo l'ora GMT corrispondente.|  
|Tipi unione|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta restrizioni da tipi di unione.|  
|Decimali di precisione delle variabili|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta decimali di precisione delle variabili. Il tipo **xs:decimal** rappresenta numeri decimali di precisione arbitraria. I processori XML conformi devono supporto numeri decimali con almeno `totalDigits=18`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta `totalDigits=38,` , ma con un limite per le cifre frazionarie pari a 10. Tutti i valori **xs:decimal** per i quali viene creata un'istanza vengono rappresentati internamente dal server con il tipo numeric SQL (38, 10).|  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Forme canoniche e restrizioni di pattern](../../relational-databases/xml/canonical-forms-and-pattern-restrictions.md)|Spiegazione delle forme canoniche e delle restrizioni di pattern.|  
|[Componenti jolly e convalida del contenuto](../../relational-databases/xml/wildcard-components-and-content-validation.md)|Descrizione delle limitazioni dell'utilizzo di Elementi dei caratteri jolly, della convalida lax e dell'anyType con le raccolte di XML Schema.|  
|[Elemento &#60;xsd:redefine&#62;](../../relational-databases/xml/the-xsd-redefine-element.md)|Viene illustrata la limitazione all'uso dell'elemento \<xsd:redefine> e viene descritta una soluzione alternativa.|  
|[Tipo xs:QName](../../relational-databases/xml/the-xs-qname-type.md)|Descrizione del limite relativo al tipo xs:QName.|  
|[Valori per dichiarazioni &#60;xsd:simpleType&#62;](../../relational-databases/xml/values-for-xsd-simpletype-declarations.md)|Vengono descritte le limitazioni applicate alle dichiarazioni \<xsd:simpleType>.|  
|[Facet di enumerazione](../../relational-databases/xml/enumeration-facets.md)|Descrizione della limitazione relativa ai facet dell'enumerazione.|  
|[Tipo misto e contenuto semplice](../../relational-databases/xml/mixed-type-and-simple-content.md)|Descrizione del limite relativo alla restrizione di un tipo misto a un contenuto semplice.|  
|[Raccolte di XML Schema di grandi dimensioni e condizioni di memoria insufficiente](../../relational-databases/xml/large-xml-schema-collections-and-out-of-memory-conditions.md)|Indicazione delle soluzioni per la condizione di memoria insufficiente che qualche volta si verifica con le raccolte di schemi di grandi dimensioni.|  
|[Modelli di contenuto non deterministici](../../relational-databases/xml/non-deterministic-content-models.md)|Descrizione dei limiti relativi ai modelli di contenuto non deterministici.|  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Concedere autorizzazioni per una raccolta di XML Schema](../../relational-databases/xml/grant-permissions-on-an-xml-schema-collection.md)   
 [Vincolo di attribuzione di particelle univoche](../../relational-databases/xml/unique-particle-attribution-constraint.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
