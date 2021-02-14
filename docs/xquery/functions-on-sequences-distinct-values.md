---
title: Funzione distinct-values (XQuery) | Microsoft Docs
description: Informazioni su come usare la funzione distinct-values in un'espressione XQuery per rimuovere i valori duplicati da una sequenza.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
author: rothja
ms.author: jroth
ms.openlocfilehash: b9a3561c25ca0c64e01c2603b64656320d499518
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341903"
---
# <a name="functions-on-sequences---distinct-values"></a>Funzioni su sequenze - distinct-values
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Rimuove i valori duplicati dalla sequenza specificata dal *$arg*. Se *$arg* è una sequenza vuota, la funzione restituisce la sequenza vuota.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>Argomenti  
 *$arg*  
 Sequenza di valori atomici.  
  
## <a name="remarks"></a>Commenti  
 Tutti i tipi di valori atomizzati passati a **distinct-values ()** devono essere sottotipi dello stesso tipo di base. I tipi di base accettati sono i tipi che supportano l'operazione **EQ** . Tra questi tipi sono inclusi i tre tipi di base numerici predefiniti, ovvero i tipi di base di data/ora xs:string, xs:boolean e xdt:untypedAtomic. Per i valori di tipo xdt:untypedAtomic viene eseguito il cast a xs:string. In presenza di una combinazione di questi tipi o nel caso in cui vengano passati altri valori di altri tipi, viene restituito un errore statico.  
  
 Il risultato di **distinct-values ()** riceve il tipo di base dei tipi passati, ad esempio xs: String nel caso di xdt: untypedAtomic, con la cardinalità originale. Se l'input è una sequenza vuota calcolata in modo statico, la sequenza vuota è implicita e viene restituito un errore statico.  
  
 I valori di tipo xs:string vengono confrontati con le regole di confronto dei punti di codice Unicode predefinite XQuery.  
  
## <a name="examples"></a>Esempio  
 In questo argomento vengono forniti esempi di XQuery sulle istanze XML archiviate in diverse colonne di tipo **XML** nel database AdventureWorks.  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>R. Utilizzo della funzione distinct-values() per rimuovere valori duplicati dalla sequenza  
 In questo esempio, un'istanza XML che contiene numeri di telefono viene assegnata a una variabile di tipo **XML** . L'espressione XQuery specificata in base a questa variabile utilizza la funzione **distinct-values ()** per compilare un elenco di numeri di telefono che non contengono duplicati.  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 Risultato:  
  
```  
111-111-1111 222-222-2222    
```  
  
 Nella query seguente viene passata una sequenza di numeri (1, 1, 2) alla funzione **distinct-values ()** . La funzione rimuove quindi il duplicato dalla sequenza e restituisce gli altri due.  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 La query restituisce 1 2.  
  
### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Limitazioni:  
  
-   La funzione **distinct-values ()** esegue il mapping di valori interi a xs: Decimal.  
  
-   La funzione **distinct-values ()** supporta solo i tipi indicati in precedenza e non supporta la combinazione di tipi di base.  
  
-   La funzione **distinct-values ()** sui valori xs: Duration non è supportata.  
  
-   Non è supportata l'opzione sintattica che fornisce le regole di confronto.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
