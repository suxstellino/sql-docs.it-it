---
description: Esercitazione su RDS (VBScript)
title: Esercitazione su RDS (VBScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RDS tutorial [ADO], VBScript
ms.assetid: e2a48c4d-88b1-43ff-a202-9cdec54997d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 693f5858a80d5bc986dc16f993aa5c5a78c54d7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031873"
---
# <a name="rds-tutorial-vbscript"></a>Esercitazione su RDS (VBScript)
Questa è l'esercitazione su RDS, scritta in Microsoft Visual Basic Scripting Edition. Per una descrizione dello scopo di questa esercitazione, vedere l' [esercitazione su RDS](./rds-tutorial.md).  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 In questa esercitazione, Servizi Desktop remoto [. DataControl](../../reference/rds-api/datacontrol-object-rds.md) e Servizi Desktop remoto [. Gli spazi](../../reference/rds-api/dataspace-object-rds.md) dei nomi vengono creati in fase di progettazione, ovvero vengono definiti con tag oggetto, come segue: `<OBJECT>...</OBJECT>` . In alternativa, è possibile crearli in fase di esecuzione con il metodo [CreateObject metodo (RDS)](../../reference/rds-api/createobject-method-rds.md) . Ad esempio, il Servizi Desktop remoto **.** È possibile creare un oggetto DataControl analogo al seguente:  
  
```vb
Set DC = Server.CreateObject("RDS.DataControl")  
   <!-- RDS.DataControl -->  
   <OBJECT   
      ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33">  
   </OBJECT>  
  
   <!-- RDS.DataSpace -->  
   <OBJECT   
      ID="DS1" WIDTH=1 HEIGHT=1  
      CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
   </OBJECT>  
  
   <SCRIPT LANGUAGE="VBScript">  
  
   Sub RDSTutorial()  
   Dim DF1   
```  
  
## <a name="step-1---specify-a-server-program"></a>Passaggio 1: specificare un programma server  
 VBScript può individuare il nome del server Web IIS in cui è in esecuzione accedendo al metodo **Request. ServerVariables** di VBScript disponibile per le pagine Active Server:  
  
```vb
"https://<%=Request.ServerVariables("SERVER_NAME")%>"  
```  
  
 Per questa esercitazione, tuttavia, usare il server immaginaria "yourServer".  
  
> [!NOTE]
>  Prestare attenzione al tipo di dati degli argomenti **ByRef** . VBScript non consente di specificare il tipo di variabile, pertanto è necessario passare sempre una **variante**. Quando si usa HTTP, RDS consente di passare una variante a un metodo che prevede un non variant se viene richiamato con **RDS.** Metodo [CreateObject](../../reference/rds-api/createobject-method-rds.md) dell'oggetto DataSpace. Quando si usa DCOM o un server in-process, è necessario associare i tipi di parametro sui lati client e server oppure verrà visualizzato un errore di tipo "mancata corrispondenza di tipo".  
  
```vb
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "https://yourServer")  
```  
  
## <a name="step-2a---invoke-the-server-program-with-rdsdatacontrol"></a>Passaggio 2a: richiamare il programma server con Servizi Desktop remoto. DataControl  
 Questo esempio è semplicemente un commento che mostra il comportamento predefinito di Servizi Desktop remoto **. DataControl** consiste nell'eseguire la query specificata.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial2A()  
   Dim RS  
   DC1.Refresh  
   Set RS = DC1.Recordset  
...  
```  
  
## <a name="step-2b---invoke-the-server-program-with-rdsserverdatafactory"></a>Passaggio 2b: richiamare il programma server con RDSServer. DataFactory  
  
## <a name="step-3---server-obtains-a-recordset"></a>Passaggio 3: il server ottiene un recordset  
  
## <a name="step-4---server-returns-the-recordset"></a>Passaggio 4: il server restituisce il recordset  
  
```vb
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors")  
```  
  
## <a name="step-5---datacontrol-is-made-usable-by-visual-controls"></a>Passaggio 5-DataControl viene reso utilizzabile dai controlli visivi  
  
```vb
' Assign the returned recordset to the DataControl.  
  
DC1.SourceRecordset = RS  
```  
  
## <a name="step-6a---changes-are-sent-to-the-server-with-rdsdatacontrol"></a>Passaggio 6a: le modifiche vengono inviate al server con Servizi Desktop remoto. DataControl  
 Questo esempio è semplicemente un commento che illustra il modo in cui **RDS. DataControl** esegue gli aggiornamenti.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial6A()  
Dim RS  
DC1.Refresh  
...  
Set RS = DC1.Recordset  
' Edit the Recordset object...  
' The SERVER and CONNECT properties are already set from Step 2A.  
Set DC1.SourceRecordset = RS  
...  
DC1.SubmitChanges  
```  
  
## <a name="step-6b---changes-are-sent-to-the-server-with-rdsserverdatafactory"></a>Passaggio 6b: le modifiche vengono inviate al server con RDSServer. DataFactory  
  
```vb
DF.SubmitChanges "DSN=Pubs", RS  
  
End Sub  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 **L'esercitazione è terminata.**  
  
## <a name="see-also"></a>Vedere anche  
 [Esercitazione su RDS](./rds-tutorial.md)