---
description: Flussi di comandi
title: Flussi di comandi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command streams [ADO]
- streams [ADO], command
ms.assetid: 0ac09dbe-2665-411e-8fbb-d1efe6c777be
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c0872f20754d093020ece2830d389de32f09032
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037741"
---
# <a name="command-streams"></a>Flussi di comandi
ADO ha sempre supportato l'input del comando in formato stringa specificato dalla proprietà **CommandText** . In alternativa, con ADO 2,7 o versioni successive, è anche possibile usare un flusso di informazioni per l'input del comando assegnando il flusso alla proprietà **CommandStream** . È possibile assegnare un oggetto **flusso** ADO o qualsiasi oggetto che supporti l'interfaccia **IStream** com.  
  
 Il contenuto del flusso di comandi viene semplicemente passato da ADO al provider, quindi il provider deve supportare l'input del comando in base al flusso per il funzionamento di questa funzionalità. Ad esempio, SQL Server supporta le query sotto forma di modelli XML o di estensioni OpenXML per Transact-SQL.  
  
 Poiché i dettagli del flusso devono essere interpretati dal provider, è necessario specificare il dialetto del comando impostando la proprietà **dialect** . Il valore del **dialetto** è una stringa che contiene un GUID, definito dal provider. Per informazioni sui valori validi per il **dialetto** supportato dal provider, vedere la documentazione del provider.  
  
## <a name="xml-template-query-example"></a>Esempio di query del modello XML  
 Nell'esempio seguente viene scritto in VBScript nel database Northwind.  
  
 Per prima cosa, inizializzare e aprire l'oggetto **flusso** che verrà usato per contenere il flusso di query:  
  
```  
Dim adoStreamQuery  
Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
adoStreamQuery.Open  
```  
  
 Il contenuto del flusso di query sarà una query modello XML.  
  
 La query modello richiede un riferimento allo spazio dei nomi XML identificato dal prefisso SQL: del \<sql:query> tag. Un'istruzione SQL SELECT viene inclusa come contenuto del modello XML e assegnata a una variabile stringa come indicato di seguito:  
  
```  
sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME </sql:query>  
</ROOT>"  
```  
  
 Quindi, scrivere la stringa nel flusso:  
  
```  
adoStreamQuery.WriteText sQuery, adWriteChar  
adoStreamQuery.Position = 0  
```  
  
 Assegnare adoStreamQuery alla proprietà **CommandStream** di un oggetto **comando** ADO:  
  
```  
Dim adoCmd  
Set adoCmd  = Server.CreateObject("ADODB.Command"")  
adoCmd.CommandStream = adoStreamQuery  
```  
  
 Specificare il **dialetto** della lingua del comando, che indica il modo in cui il Provider di SQL Server OLE DB deve interpretare il flusso di comandi. Dialetto specificato da un GUID specifico del provider:  
  
```  
adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
```  
  
 Infine, eseguire la query e restituire i risultati a un oggetto **Recordset** :  
  
```  
Set objRS = adoCmd.Execute  
```
