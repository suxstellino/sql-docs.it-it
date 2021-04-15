---
title: Uso di annotazioni negli schemi XSD (SQLXML)
description: Informazioni su come usare le annotazioni supportate dal linguaggio dello schema XSD in SQLXML 4.0 per specificare il mapping da XML a relazionale all'interno di uno schema XSD.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, about annotated XSD schemas
- annotations [SQLXML]
- relationships [SQLXML]
- relationships [SQLXML], hierarchical relationships
- hierarchical relationships [SQLXML]
- mapping schema [SQLXML], scenarios for using
ms.assetid: 78f318a4-7a36-473b-9852-a4bae6940ce5
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 65ae1ddc1ce718e2fad4ef06941b504fe461a7f0
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490757"
---
# <a name="using-annotations-in-xsd-schemas-sqlxml-40"></a>Utilizzo delle annotazioni negli schemi XSD (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQLXML 4.0 il linguaggio dello schema XSD supporta le annotazioni in una modalità analoga a quella delle annotazioni introdotte con il linguaggio dello schema XDR (XML-Data Reduced). In XSD sono state introdotte alcune annotazioni aggiuntive che non sono supportate in XDR.  
  
 Queste annotazioni possono essere utilizzate all'interno dello schema XSD per specificare un mapping tra dati XML e dati relazionali, incluso un mapping tra elementi e attributi nello schema XSD e tabelle (viste) e colonne nei database.  
  
 Se non si specificano le annotazioni, viene eseguito il mapping predefinito. Per impostazione predefinita, un elemento XSD con un tipo complesso viene mappato a un nome di tabella (vista) nel database specificato e un elemento o attributo con un tipo semplice viene mappato alla colonna che riporta lo stesso nome dell'elemento o dell'attributo.  
  
 Queste annotazioni possono essere usate anche per specificare le relazioni gerarchiche in XML, rappresentando così le relazioni nel database, perché uno schema XSD è semplicemente una visualizzazione XML dei dati relazionali.  
  
 In questa sezione vengono descritte le annotazioni che è possibile utilizzare con schemi XSD ed esempi pratici del loro utilizzo.  
  
> [!NOTE]  
>  In tutti gli esempi di questa sezione sono specificate query XPath semplici sullo schema XSD con annotazioni descritto in ogni esempio. Si presuppone che l'utente abbia già acquisito familiarità con il linguaggio XPath.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Annotazioni XSD &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/xsd-annotations-sqlxml-4-0.md)  
 Vengono elencate le annotazioni che è possibile utilizzare con gli schemi XSD, le relative descrizioni e le annotazioni equivalenti per XDR.  
  
 [Mapping predefinito di elementi e attributi XSD a tabelle e colonne &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
 Viene illustrato il mapping predefinito e vengono forniti esempi di attività relative al mapping predefinito.  
  
 [Mapping esplicito di elementi e attributi XSD a tabelle e colonne &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/explicit-mapping-xsd-elements-and-attributes-to-tables-and-columns.md)  
 Illustra il mapping esplicito con **le annotazioni sql:relation** e **sql:field** e fornisce esempi.  
  
 [Specifica di relazioni tramite sql:relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)  
 Descrive e fornisce esempi dell'annotazione **sql:relationship.**  
  
 [Specifica dell'attributo sql:inverse in sql:relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)  
 Descrive **l'annotazione sql:inverse.**  
  
 [Creazione di elementi costanti usando sql:is-constant &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-constant-elements-using-sql-is-constant-sqlxml-4-0.md)  
 Descrive e fornisce esempi dell'annotazione **sql:is-constant.**  
  
 [Esclusione di elementi dello schema dal documento XML risultante usando sql:mapped &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/excluding-schema-elements-from-the-xml-document-using-sql-mapped.md)  
 Descrive e fornisce esempi dell'annotazione **sql:mapped.**  
  
 [Filtro dei valori tramite sql:limit-field e sql:limit-value &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/filtering-values-using-sql-limit-field-and-sql-limit-value-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi di **annotazioni sql:limit-field** **e sql:limit-value.**  
  
 [Identificazione delle colonne chiave tramite sql:key-fields &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)  
 Descrive e fornisce esempi dell'annotazione **sql:key-fields.**  
  
 [Specifica di uno spazio dei nomi di destinazione usando l'attributo targetNamespace &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'attributo targetNamespace.**  
  
 [Creazione di attributi di tipo ID, IDREF e IDREFS validi usando sql:prefix &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-valid-id-idref-and-idrefs-type-attributes-using-sql-prefix-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:prefix.**  
  
 [Conversioni dei tipi di dati e annotazione sql:datatype &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:datatype.**  
  
 [Mapping dei tipi di dati XSD ai tipi di dati XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)  
 Viene fornita una tabella in cui i tipi di dati XSD, XDR e XPath vengono messi a confronto e le relative conversioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono elencate.  
  
 [Creazione di sezioni CDATA tramite sql:use-cdata &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-cdata-sections-using-sql-use-cdata-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:use-data.**  
  
 [Richiesta di riferimenti URL a dati BLOB tramite sql:encode &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/requesting-url-references-to-blob-data-using-sql-encode-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:encode.**  
  
 [Recupero di dati non consumati tramite il comando sql:overflow-field &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/retrieving-unconsumed-data-using-the-sql-overflow-field-sqlxml-4-0.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:overflow-field.**  
  
 [Nascondere gli elementi e gli attributi utilizzando sql:hide](../../relational-databases/sqlxml-annotated-xsd-schemas-using/hiding-elements-and-attributes-by-using-sql-hide.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:hide.**  
  
 [Utilizzo delle annotazioni sql:identity e sql:guid](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)  
 Vengono descritti e forniti esempi delle **annotazioni sql:identity** e **sql:guid.**  
  
 [Specifica del livello di nidificazione nelle relazioni ricorsive mediante sql:max-depth](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-depth-in-recursive-relationships-by-using-sql-max-depth.md)  
 Vengono descritti e forniti esempi **dell'annotazione sql:max-depth.**  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza degli schemi &#40;sqlxml 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)  
  
  
