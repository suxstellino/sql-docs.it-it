---
description: Linguaggio XML di manipolazione dei dati (XML DML)
title: Linguaggio XML di manipolazione dei dati (XML DML)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- modifying data [SQL Server], XML DML
- XML [SQL Server], DML
- XML DML [SQL Server]
- data modification language [XML DML]
- data modifications [XML DML]
- DML [XML in SQL Server]
- XQuery, XML DML
- xml data type [SQL Server], XML DML
ms.assetid: 20ce50d2-c07b-4e41-93a7-1380d2cd49cb
author: rothja
ms.author: jroth
ms.openlocfilehash: 641c2b6cc4fb72670a1d90716f59b91f1ee2af01
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488043"
---
# <a name="xml-data-modification-language-xml-dml"></a>Linguaggio XML di manipolazione dei dati (XML DML)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Il linguaggio XML DML è un'estensione del linguaggio XQuery. Come definito dal consorzio W3C, il linguaggio XQuery non include la parte relativa alla manipolazione dei dati (DML). Il linguaggio XML DML illustrato in questo argomento e il linguaggio XQuery offrono un linguaggio per le query e per la modifica di dati perfettamente funzionante che può essere usato per il tipo di dati **xml**.  
  
 Rispetto al linguaggio XQuery, nel linguaggio XML DML sono state aggiunte le parole chiave con distinzione tra maiuscole e minuscole seguenti:  
  
-   **insert**  
  
-   **delete**  
  
-   **replace value of**  
  
 Come illustrato nell'argomento [Colonne e tipo di dati XML&#40;SQL Server&#41;](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md) è possibile creare variabili e colonne di tipo **xml** e assegnar loro documenti o frammenti XML. Per modificare o aggiornare queste istanze XML, eseguire le operazioni seguenti:  
  
-   Usare il [metodo modify() (tipo di dati XML)](../../t-sql/xml/modify-method-xml-data-type.md) del tipo di dati **xml**.  
  
-   Specificare le istruzioni XML DML appropriate nel metodo **modify()** .  
  
 Si noti che alcuni attributi non possono essere inseriti o eliminati o non è possibile modificarne il valore. Ad esempio:  
  
-   Per il codice XML tipiato o non **tipidato,** gli attributi sono **xmlns**, **xmlns: \* *_, e _* xml:base**.  
  
-   Solo per il codice **xml** tipizzato, tali attributi sono **xsi:nil** e **xsi:type**.  
  
 Sono inoltre valide le restrizioni seguenti:  
  
-   Per il codice **xml** tipizzato o non tipizzato, l'inserimento dell'attributo **xml:base** avrà esito negativo.  
  
-   Per il codice **xml** tipizzato, l'eliminazione e la modifica dell'attributo **xsi:nil** avrà esito negativo. Per il codice **xml** non tipizzato, è possibile eliminare l'attributo o modificarne il valore.  
  
-   Per il codice **xml** tipizzato, la modifica del valore dell'attributo **xs:type** avrà esito negativo. Per il codice **xml** non tipizzato, è possibile modificare il valore dell'attributo.  
  
 Quando si modifica un'istanza XML tipizzata, il formato finale deve essere un'istanza valida del tipo specifico, in caso contrario verrà restituito un errore di convalida.  
  
## <a name="see-also"></a>Vedere anche  
 [insert &#40;XML DML&#41;](../../t-sql/xml/insert-xml-dml.md)   
 [delete &#40;XML DML&#41;](../../t-sql/xml/delete-xml-dml.md)   
 [replace value of &#40;XML DML&#41;](../../t-sql/xml/replace-value-of-xml-dml.md)   
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [metodi con tipo di dati xml](../../t-sql/xml/xml-data-type-methods.md)  
  
  
