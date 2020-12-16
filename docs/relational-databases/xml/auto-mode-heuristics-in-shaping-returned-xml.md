---
title: Approccio euristico della modalità AUTO per la determinazione della struttura dei valori XML restituiti | Microsoft Docs
description: Informazioni su come usare l'euristica in modalità AUTO con la clausola FOR XML per confrontare i valori di colonna nelle righe adiacenti e determinare la forma del codice XML restituito dalla query.
ms.custom: fresh2019may
ms.date: 04/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, heuristics in shaping returned XML
ms.assetid: 6c5cb6c1-2921-4ba1-8100-0bf8074f9103
author: RothJa
ms.author: jroth
ms.openlocfilehash: b4683364ed4f2a27d5b9f25f3e368dfd40ef56aa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474542"
---
# <a name="auto-mode-heuristics-in-shaping-returned-xml"></a>Approccio euristico della modalità AUTO per la determinazione della struttura dei valori XML restituiti

[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

La modalità AUTO determina la struttura del valore XML restituito in base alla query. Per determinare come devono essere nidificati gli elementi, la modalità AUTO, che utilizza un approccio euristico, confronta i valori delle colonne nelle righe adiacenti. Vengono confrontate colonne di tutti i tipi, ad eccezione di **ntext**, **text**, **image** e **xml**. Vengono confrontate le colonne di tipo **(n)varchar(max)** e **varbinary(max)** .  
  
 Nell'esempio seguente viene illustrato l'approccio euristico utilizzato dalla modalità AUTO per determinare la struttura del valore XML risultante:  
  
```sql
SELECT T1.Id, T2.Id, T1.Name  
FROM   T1, T2  
WHERE ...  
ORDER BY T1.Id
FOR XML AUTO;
```  
  
 Per determinare la posizione iniziale dell'elemento <`T1`>, se non è specificata la chiave della tabella T1 vengono confrontati tutti i valori di colonna di T1, ad eccezione di quelli di tipo **ntext**, **text**, **image** e **xml**. Supporre quindi che la colonna **Name** sia di tipo **nvarchar(40)** e che l'istruzione SELECT restituisca il set di righe seguente:  
  
```  
T1.Id  T1.Name  T2.Id  
-----------------------  
1       Andrew    2  
1       Andrew    3  
1       Nancy     4  
```  
  
 Utilizzando un approccio euristico, la modalità AUTO confronta tutti i valori della tabella T1, ovvero le colonne Id e Name. Le prime due righe contengono gli stessi valori nelle colonne Id e Name, quindi al risultato viene aggiunto un elemento \<T1> con due elementi figlio \<T2>.  
  
 Il valore XML restituito è il seguente:  
  
```xml
<T1 Id="1" Name="Andrew">  
    <T2 Id="2" />  
    <T2 Id="3" />  
</T1>  
<T1 Id="1" Name="Nancy" >  
      <T2 Id="4" />  
</T>  
```  
  
 Si supponga ora che la colonna Name sia di tipo **text** . A causa dell'approccio euristico, la modalità AUTO non confronta valori di questo tipo, ma presuppone che siano diversi. Viene pertanto generato il valore XML illustrato di seguito:  
  
```xml
<T1 Id="1" Name="Andrew" >  
  <T2 Id="2" />  
</T1>  
<T1 Id="1" Name="Andrew" >  
  <T2 Id="3" />  
</T1>  
<T1 Id="1" Name="Nancy" >  
  <T2 Id="4" />  
</T1>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare la modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  
