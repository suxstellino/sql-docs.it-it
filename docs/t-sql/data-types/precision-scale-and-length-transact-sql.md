---
description: Precisione, scala e lunghezza (Transact-SQL)
title: Precisione, scala e lunghezza (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], length
- data types [SQL Server], scale
- precision [SQL Server], data types
- lengths [SQL Server], data types
- number of digits
- scale [SQL Server]
- scale [SQL Server], data types
- data types [SQL Server], precision
ms.assetid: fbc9ad2c-0d3b-4e98-8fdd-4d912328e40a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: da816b6e55bc1578a820a53d6e1036b685086713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338048"
---
# <a name="precision-scale-and-length-transact-sql"></a>Precisione, scala e lunghezza (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La precisione è il numero di cifre in un numero. La scala è il numero di cifre a destra della virgola decimale in un numero. Il numero 123,45, ad esempio, ha una precisione di 5 e una scala di 2.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la precisione massima predefinita dei tipi di dati **numeric** e **decimal** è 38. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'impostazione massima predefinita era 28.
  
La lunghezza di un tipo di dati numerico è il numero di byte utilizzati per l'archiviazione del numero. Per varchar e char, la lunghezza di una stringa di caratteri è data dal numero di byte. Per nvarchar e nchar, la lunghezza di una stringa di caratteri è data dal numero di coppie di byte. La lunghezza per i tipi di dati **binary**, **varbinary** e **image** è il numero di byte. Il tipo di dati **int** può ad esempio contenere 10 cifre, viene archiviato in 4 byte e non consente l'uso dei decimali. Il tipo di dati **int** ha una precisione pari a 10, una lunghezza pari a 4 e una scala pari a 0.
  
Se si concatenano due espressioni di tipo **char**, **varchar**, **binary** o **varbinary**, la lunghezza dell'espressione risultante corrisponde alla somma delle lunghezze delle due espressioni di origine, fino a un massimo di 8.000 byte.
  
Se si concatenano due espressioni di tipo **nchar** o **nvarchar**, la lunghezza dell'espressione risultante corrisponde alla somma delle lunghezze delle due espressioni di origine, fino a un massimo di 4.000 coppie di byte.
  
Se si confrontano due espressioni dello stesso tipo di dati ma di lunghezze diverse usando UNION, EXCEPT o INTERSECT, la lunghezza risultante corrisponde a quella più lunga delle due espressioni.
  
La precisione e la scala dei tipi di dati numerici, ad eccezione di **decimal**, sono fisse. Quando un operatore aritmetico collega due espressioni dello stesso tipo, il risultato è costituito dallo stesso tipo di dati, con la precisione e la scala definiti per tale tipo. Se un operatore collega due espressioni con tipi di dati numerici diversi, il tipo di dati del risultato viene stabilito dalle regole relative alla precedenza dei tipi di dati. La precisione e la scala del risultato corrispondono a quelle definite per il tipo di dati.
  
Nella tabella seguente vengono descritti i criteri per il calcolo della precisione e della scala del risultato, quando il risultato di un'operazione è di tipo **decimal**. Il risultato è **decimal** quando si verifica una delle condizioni seguenti:
-   Entrambe le espressioni sono di tipo **decimal**.  
-   Un'espressione è di tipo **decimal** e l'altra è di un tipo di dati con precedenza inferiore rispetto a **decimal**.  
  
Le espressioni dell'operando sono definite come espressione e1, con precisione p1 e scala s1, e come espressione e2, con precisione p2 e scala s2. La precisione e la scala per qualsiasi espressione non di tipo **decimal** corrispondono alla precisione e alla scala definite per il tipo di dati dell'espressione. La funzione max(a,b) significa: prendi il valore maggiore di "a" o "b". Analogamente, min(a,b) significa: prendi il valore minore di "a" o "b".
  
|Operazione|Precisione del risultato|Scala del risultato*|  
|---|---|---|
|e1 + e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 - e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 * e2|p1 + p2 + 1|s1 + s2|  
|e1 / e2|p1 - s1 + s2 + max(6, s1 + p2 + 1)|max(6, s1 + p2 + 1)|  
|e1 { UNION &#124; EXCEPT &#124; INTERSECT } e2|max(s1, s2) + max(p1-s1, p2-s2)|max(s1, s2)|  
|e1 % e2|min(p1-s1, p2 -s2) + max( s1,s2 )|max(s1, s2)|  
  
\* La precisione e la scala del risultato hanno un valore massimo assoluto pari a 38. Quando la precisione del risultato è superiore a 38, viene ridotta a 38 e la scala corrispondente viene ridotta per evitare il troncamento della parte intera del risultato. In alcuni casi, quali la moltiplicazione o la divisione, il fattore di scala non viene ridotto per mantenere la precisione decimale, anche se può essere generato un errore di overflow.

Nelle operazioni di addizione e sottrazione sono necessarie le posizioni `max(p1 - s1, p2 - s2)` per archiviare la parte integrale del numero decimale. Se non è disponibile spazio sufficiente per l'archiviazione, ad esempio `max(p1 - s1, p2 - s2) < min(38, precision) - scale`, la scala viene ridotta per offrire spazio sufficiente per la parte integrale. La scala risultante è `MIN(precision, 38) - max(p1 - s1, p2 - s2)`, pertanto è possibile arrotondare la parte frazionaria per adattarla alla scala risultante.

Nelle operazioni di moltiplicazione e divisione sono necessarie le posizioni `precision - scale` per archiviare la parte integrale del risultato. La scala può essere ridotta tramite le regole seguenti:
1.  La scala risultante viene ridotta a `min(scale, 38 - (precision-scale))` se la parte integrale è minore di 32, perché non può essere maggiore di `38 - (precision-scale)`. In questo caso il risultato può essere arrotondato.
1. La scala non viene modificata se è minore di 6 e se la parte integrale è maggiore di 32. In questo caso, può essere generato un errore di overflow se decimal(38, scala) non è sufficiente 
1. La scala viene impostata su 6 se è maggiore di 6 e se la parte integrale è maggiore di 32. In questo caso, sia la parte integrale che la scala possono essere ridotte e il tipo del risultato è decimal(38,6). Il risultato può essere arrotondato a 6 posizioni decimali. In caso contrario, se per la parte integrale non sono sufficienti 32 cifre, si genera un errore di overflow.

## <a name="examples"></a>Esempi
L'espressione seguente restituisce il risultato `0.00000090000000000` senza arrotondamento, perché il risultato può essere contenuto in `decimal(38,17)`:
```sql
select cast(0.0000009000 as decimal(30,20)) * cast(1.0000000000 as decimal(30,20)) [decimal 38,17]
```
In questo caso la precisione è 61 e la scala è 40.
La parte integrale (precisione-scala = 21) è minore di 32, quindi questo caso è riconducibile al caso (1) delle regole della moltiplicazione e la scala viene calcolata come `min(scale, 38 - (precision-scale)) = min(40, 38 - (61-40)) = 17`. Il tipo di risultato è `decimal(38,17)`.

L'espressione seguente restituisce il risultato `0.000001` che deve essere inserito in `decimal(38,6)`:
```sql
SELECT CAST(0.0000009000 AS DECIMAL(30,10)) * CAST(1.0000000000 AS DECIMAL(30,10)) [decimal(38, 6)]
```
In questo caso la precisione è 61 e la scala è 20.
La scala è maggiore di 6 e la parte integrale (`precision-scale = 41`) è maggiore di 32. Si tratta del caso (3) delle regole di moltiplicazione e il tipo di risultato è `decimal(38,6)`.

## <a name="see-also"></a>Vedere anche
[Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
