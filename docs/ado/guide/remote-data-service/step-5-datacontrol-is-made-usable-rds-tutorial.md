---
description: "Passaggio 5: L'oggetto DataControl viene reso utilizzabile (esercitazione su RDS)"
title: 'Passaggio 5: DataControl è reso utilizzabile (esercitazione su RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 6453043514657b03e72eeea032451f647f6c2eec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036091"
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>Passaggio 5: L'oggetto DataControl viene reso utilizzabile (esercitazione su RDS)
L'oggetto **Recordset** restituito è disponibile per l'utilizzo. È possibile esaminarla, spostarla o modificarla come per qualsiasi altro **Recordset**. Gli elementi che è possibile eseguire con il **Recordset** dipendono dall'ambiente in uso. Visual Basic e Visual C++ hanno controlli visivi che possono usare un **Recordset** direttamente o indirettamente con l'ausilio di un controllo di abilitazione dei dati.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Se, ad esempio, si visualizza una pagina Web in Microsoft Internet Explorer, è possibile che si desideri visualizzare i dati dell'oggetto **Recordset** in un controllo visivo. I controlli visivi in una pagina Web non possono accedere direttamente a un oggetto **Recordset** . Tuttavia, possono accedere all'oggetto **Recordset** tramite [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md). **RDS. DataControl** diventa utilizzabile da un controllo visivo quando la relativa proprietà [SourceRecordset](../../reference/rds-api/recordset-sourcerecordset-properties-rds.md) è impostata sull'oggetto **Recordset** .  
  
 Il parametro **dataSrc** dell'oggetto controllo visivo deve essere impostato su **RDS. DataControl** e la relativa proprietà **dataFld** impostata su un campo oggetto **Recordset** (colonna).  
  
 In questa esercitazione, impostare la proprietà **SourceRecordset** :  
  
```vb
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Passaggio 6: le modifiche vengono inviate al server (esercitazione su RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [Esercitazione su RDS (VBScript)](./rds-tutorial-vbscript.md)