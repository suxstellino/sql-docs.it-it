---
description: 'Passaggio 6: Le modifiche vengono inviate al server (esercitazione su RDS)'
title: 'Passaggio 6: le modifiche vengono inviate al server (esercitazione su RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
author: rothja
ms.author: jroth
ms.openlocfilehash: d6a769a22a86e145727079af6fc067b1552f2bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031713"
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>Passaggio 6: Le modifiche vengono inviate al server (esercitazione su RDS)
Se l'oggetto **Recordset** viene modificato, tutte le modifiche, ovvero le righe aggiunte, modificate o eliminate, possono essere inviate nuovamente al server.  
  
> [!NOTE]
>  Il comportamento predefinito di RDS può essere richiamato in modo implicito con gli oggetti ADO e il provider Microsoft OLE DB Remoting. Le query possono restituire **Recordset** s e i **Recordset** modificati possono aggiornare l'origine dati. Questa esercitazione non richiama Servizi Desktop remoto con gli oggetti ADO, ma questo è il modo in cui verrebbe eseguita l'operazione:  
  
```vb
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=https://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **Parte A** Si supponga, in questo caso, che sia stato utilizzato solo il Servizi Desktop remoto [. DataControl](../../reference/rds-api/datacontrol-object-rds.md) e che un oggetto **Recordset** è ora associato a **RDS. DataControl**. Il metodo [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) aggiorna l'origine dati con tutte le modifiche apportate all'oggetto **Recordset** se le proprietà [server](../../reference/rds-api/server-property-rds.md) e [Connect](../../reference/rds-api/connect-property-rds.md) sono ancora impostate.  
  
```vb
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "https://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **Parte B** In alternativa, è possibile aggiornare il server con l'oggetto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) , specificando una connessione e un oggetto **Recordset** .  
  
```vb
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **L'esercitazione è terminata.**  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Vedere anche  
 [Provider Microsoft OLE DB Remoting (provider di servizi ADO)](../appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [Esercitazione su RDS](./rds-tutorial.md)   
 [Esercitazione su RDS (VBScript)](./rds-tutorial-vbscript.md)