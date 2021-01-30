---
description: WITH XMLNAMESPACES
title: WITH XMLNAMESPACES (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH_XMLNAMESPACES_TSQL
- WITH XMLNAMESPACES
dev_langs:
- TSQL
helpviewer_keywords:
- adding XML namespaces
- XML namespace declarations [SQL Server]
- clauses [SQL Server], WITH XMLNAMESPACES
- WITH XMLNAMESPACES clause
- declaring XML namespaces
ms.assetid: 3b32662b-566f-454d-b7ca-e247002a9a0b
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6ec08509389c8b5aadd080245253e289cc23100c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181742"
---
# <a name="with-xmlnamespaces"></a>WITH XMLNAMESPACES
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Dichiara uno o più spazi dei nomi XML.  
  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
WITH XMLNAMESPACES ( <XML namespace declaration item>  
[ { , <XML namespace declaration item> }...] )   
  
<XML namespace declaration item> ::=  
<xml_namespace_uri> AS <xml_namespace_prefix>  
| <XML default namespace declaration item>  
<xml_namespace_uri> ::= <character string literal>  
```  
  
```syntaxsql
  
<xml_namespace_prefix> ::= <identifier>  
```  
  
```syntaxsql
  
<XML default namespace declaration item> ::=  
DEFAULT <xml_namespace_uri>  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *xml_namespace_uri*  
 URI (Uniform Resource Identifier) che identifica lo spazio dei nomi XML da dichiarare. *xml_namespace_uri* è una stringa SQL.  
  
 *xml_namespace_prefix*  
 Specifica un prefisso da mappare e associare al valore dell'URI dello spazio dei nomi in *xml_namespace_uri*. *xml_namespace_prefix* deve essere un identificatore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Osservazioni  
 Se si utilizza la clausola WITH XMLNAMESPACES in un'istruzione che include inoltre un'espressione di tabella comune, è necessario che la clausola WITH XMLNAMESPACES preceda tale espressione.  
  
 Di seguito sono riportate le regole sintattiche generali in caso di utilizzo della clausola WITH XMLNAMESPACES:  
  
-   Ogni dichiarazione di spazio dei nomi XML deve contenere almeno un elemento di dichiarazione dello spazio dei nomi XML predefinito.  
  
-   Ogni prefisso dello spazio dei nomi XML utilizzato deve essere un nome privo di due punti (NCName) in cui il carattere due punti (:) non fa parte del nome.  
  
-   Non è possibile definire un prefisso di spazio dei nomi due volte.  
  
-   I prefissi degli spazi dei nomi XML e gli URI fanno distinzione tra maiuscole e minuscole.  
  
-   Non è possibile dichiarare il prefisso di spazio dei nomi XML `xmlns`.  
  
-   Il prefisso di spazio dei nomi XML `xml` non può essere sovrascritto da uno spazio dei nomi diverso dall'URI dello spazio dei nomi `'http://www.w3.org/XML/1998/namespace'`. A questo URI non può essere assegnato un prefisso diverso.  
  
-   Il prefisso di spazio dei nomi XML `xsi` non può essere dichiarato nuovamente se la direttiva ELEMENTS XSINIL è utilizzata nella query.  

-   Non è necessario dichiarare che "http://www.w3.org/2001/XMLSchema-instance" usi lo spazio dei nomi dello standard xsi. Viene aggiunto in modo implicito dal processore XML/XPATH se non viene specificato e le espressioni xpath possono usare il prefisso xsi, purché lo schema "http://www.w3.org/2001/XMLSchema-instance" sia dichiarato correttamente nel documento xml.

-   I valori stringa dell'URI sono codificati in base alla tabella codici delle regole di confronto del database corrente e vengono convertite internamente in Unicode.  
  
-   Gli spazi vuoti dell'URI dello spazio dei nomi XML verranno compressi in base alle regole XSD di compressione degli spazi vuoti usate per **xs:anyURI**. Non viene eseguita alcuna operazione di sostituzione con entità o sostituzione con caratteri nei valori di URI dello spazio dei nomi XML.  

-   Nell'URI dello spazio dei nomi XML verrà controllato se sono inclusi caratteri XML 1.0 non validi. Se il controllo ha esito positivo, verrà generato un errore, ad esempio U+0007.  
  
-   In seguito alla compressione di tutti gli spazi vuoti, l'URI dello spazio dei nomi XML non può essere una stringa di lunghezza zero. In caso contrario viene generato un errore per segnalare che un URI dello spazio dei nomi vuoto non è valido.  
  
-   La parola chiave XMLNAMESPACES è riservata nel contesto della clausola WITH.  
  
## <a name="examples"></a>Esempi  
 Per alcuni esempi, vedere [Aggiungere spazi dei nomi alle query con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
  
