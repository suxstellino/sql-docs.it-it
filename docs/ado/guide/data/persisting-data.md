---
description: Persistenza dei dati
title: Salvataggio permanente dei dati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: 21c162ca-2845-4dd8-a49d-e715aba8c461
author: rothja
ms.author: jroth
ms.openlocfilehash: f9a2ad809efbc847a2dfaf2292a15c1c7376e9b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037211"
---
# <a name="persisting-data"></a>Persistenza dei dati
Il computer portatile, ad esempio l'utilizzo di computer portatili, ha generato la necessità di applicazioni che possono essere eseguite sia in stato connesso che in stato disconnesso. ADO ha aggiunto il supporto a questo scopo, offrendo allo sviluppatore la possibilità di salvare un **Recordset** del cursore client su disco e di ricaricarlo in un secondo momento.  
  
 Esistono diversi scenari in cui è possibile utilizzare questo tipo di funzionalità, incluse le seguenti:  
  
-   In **viaggio:** Quando si acquisisce l'applicazione in viaggio, è fondamentale fornire la possibilità di apportare modifiche e aggiungere nuovi record che possono essere riconnessi al database in un secondo momento e di cui è stato eseguito il commit.  
  
-   **Ricerche aggiornate raramente:** Spesso in un'applicazione, le tabelle vengono utilizzate come ricerche, ad esempio le tabelle delle imposte di stato. Vengono aggiornati raramente e sono di sola lettura. Anziché rileggere i dati dal server ogni volta che l'applicazione viene avviata, l'applicazione può semplicemente caricare i dati da un **Recordset** salvato in locale.  
  
 Per salvare e caricare **Recordset** in ADO, utilizzare i metodi **Recordset. Save** e **Recordset. Open (,,,, ADCMDFILE)** sull'oggetto **Recordset** ADO.  
  
 È possibile utilizzare il metodo **Save del recordset per salvare** in modo permanente il **Recordset** ADO in un file su disco. È anche possibile salvare un **Recordset** in un oggetto **flusso** ADO. Gli oggetti **flusso** vengono descritti più avanti nella guida. Successivamente, è possibile usare il metodo **Open** per riaprire il **Recordset** quando si è pronti a usarlo. Per impostazione predefinita, ADO salva il **Recordset** nel formato proprietario Microsoft Advanced Data TABLEGRAM (ADTG). Questo formato binario viene specificato usando il valore **PersistFormatEnum di adPersistADTG** . In alternativa, è possibile scegliere di salvare il **Recordset** come XML usando invece **adPersistXML**. Per ulteriori informazioni sul salvataggio dei recordset come XML, vedere la pagina relativa [alla permanenza dei record in formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md).  
  
 La sintassi del metodo **Save** è la seguente:  
  
```  
  
recordset.  
Save  
Destination, PersistFormat  
  
```  
  
 La prima volta che si salva il **Recordset**, è facoltativo specificare la *destinazione*. Se si omette la *destinazione*, verrà creato un nuovo file con un nome impostato sul valore della proprietà [source](../../../ado/reference/ado-api/source-property-ado-recordset.md) del **Recordset**.  
  
 Omettere la *destinazione* quando si chiama successivamente **Save** dopo il primo salvataggio o si verificherà un errore di run-time. Se successivamente si chiama **Save** con una nuova *destinazione*, il **Recordset** viene salvato nella nuova destinazione. Tuttavia, la nuova destinazione e la destinazione originale saranno entrambe aperte.  
  
 **Salva** non chiude il **Recordset** o la *destinazione*, pertanto è possibile continuare a lavorare con il **Recordset** e salvare le modifiche più recenti. La *destinazione* rimane aperta fino alla chiusura del **Recordset** , durante il quale altre applicazioni possono leggere ma non scrivere nella *destinazione*.  
  
 Per motivi di sicurezza, il metodo **Save** consente solo l'utilizzo di impostazioni di protezione minime e personalizzate da uno script eseguito da Microsoft Internet Explorer.  
  
 Se il metodo **Save** viene chiamato mentre è in corso un'operazione di recupero, esecuzione o aggiornamento di un **Recordset** asincrono, **Save** attende fino al completamento dell'operazione asincrona.  
  
 I record vengono salvati a partire dalla prima riga del **Recordset**. Al termine del metodo **Save** , la posizione della riga corrente viene spostata nella prima riga del **Recordset**.  
  
 Per ottenere risultati ottimali, impostare la proprietà [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) su **adUseClient** con **Save**. Se il provider non supporta tutte le funzionalità necessarie per salvare gli oggetti **Recordset** , il servizio Cursor fornirà tale funzionalità.  
  
 Quando un **Recordset** viene reso permanente con la proprietà **CursorLocation** impostata su **adUseServer come**, la funzionalità di aggiornamento per il **Recordset** è limitata. In genere, sono consentiti solo aggiornamenti, inserimenti ed eliminazioni di tabelle singole (a seconda della funzionalità del provider). Anche il metodo di [Risincronizzazione](../../../ado/reference/ado-api/resync-method.md) non è disponibile in questa configurazione.  
  
 Poiché il parametro di *destinazione* può accettare qualsiasi oggetto che supporti l'interfaccia **IStream** di OLE DB, è possibile salvare un **Recordset** direttamente nell'oggetto **risposta** ASP.  
  
 Nell'esempio seguente vengono usati i metodi Save e **Open** per **salvare** in modo permanente un **Recordset** e riaprirlo in un secondo momento:  
  
```  
'BeginPersist  
   conn.ConnectionString = _  
   "Provider=SQLOLEDB;Data Source=MySQLServer;" _  
      & "Integrated Security=SSPI;Initial Catalog=pubs"  
   conn.Open  
  
   conn.Execute "create table testtable (dbkey int " & _  
      "primary key, field1 char(10))"  
   conn.Execute "insert into testtable values (1, 'string1')"  
  
   Set rst.ActiveConnection = conn  
   rst.CursorLocation = adUseClient  
  
   rst.Open "select * from testtable", conn, adOpenStatic, _  
      adLockBatchOptimistic  
  
   'Change the row on the client  
   rst!field1 = "NewValue"  
  
   'Save to a file--the .dat extension is an example; choose  
   'your own extension. The changes will be saved in the file  
   'as well as the original data.  
   MyFile = Dir("c:\temp\temptbl.dat")  
   If MyFile <> "" Then  
       Kill "c:\temp\temptbl.dat"  
   End If  
  
   rst.Save "c:\temp\temptbl.dat", adPersistADTG  
   Set rst = Nothing  
  
   'Now reload the data from the file  
   Set rst = New ADODB.Recordset  
   rst.Open "c:\temp\temptbl.dat", , adOpenStatic, _  
      adLockBatchOptimistic, adCmdFile  
  
   Debug.Print "After Loading the file from disk"  
   Debug.Print "   Current Edited Value: " & rst!field1.Value  
   Debug.Print "   Value Before Editing: " & rst!field1.OriginalValue  
  
   'Note that you can reconnect to a connection and  
   'submit the changes to the data source  
   Set rst.ActiveConnection = conn  
   rst.UpdateBatch  
'EndPersist  
```  
  
## <a name="remarks"></a>Commenti  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Altre informazioni sulla persistenza dei recordset](../../../ado/guide/data/more-about-recordset-persistence.md)  
  
-   [Persistenza di recordset filtrati e gerarchici](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md)  
  
-   [Persistenza di record in formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
