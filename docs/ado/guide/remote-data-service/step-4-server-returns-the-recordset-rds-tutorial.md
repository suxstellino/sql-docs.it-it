---
description: 'Passaggio 4: Il server restituisce il recordset (esercitazione su RDS)'
title: 'Passaggio 4: il server restituisce il recordset (esercitazione su RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server returns Recordset
ms.assetid: 3d1855c4-419c-4810-b5ea-6c874b5e2905
author: rothja
ms.author: jroth
ms.openlocfilehash: 1008e4c9212f13bd8f4fb2ba8765ac52230aa612
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031733"
---
# <a name="step-4-server-returns-the-recordset-rds-tutorial"></a>Passaggio 4: Il server restituisce il recordset (esercitazione su RDS)
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 RDS converte l'oggetto **Recordset** recuperato in un modulo che può essere inviato al client (ovvero esegue il *marshalling* del **Recordset**). Il formato esatto della conversione e il modo in cui viene inviato variano a seconda che il server sia in Internet o in una rete Intranet, in una rete locale o in una libreria a collegamento dinamico. Tuttavia, questo dettaglio non è fondamentale. l'aspetto più importante è che RDS invia il **Recordset** al client.  
  
 Sul lato client, un oggetto **Recordset** viene restituito e assegnato a una variabile locale.  
  
```vb
Sub RDSTutorial4()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query("DSN=Pubs", "SELECT * FROM Authors")  
...  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Passaggio 5: DataControl è reso utilizzabile (esercitazione su RDS)](./step-5-datacontrol-is-made-usable-rds-tutorial.md)   
 [Esercitazione su RDS (VBScript)](./rds-tutorial-vbscript.md)