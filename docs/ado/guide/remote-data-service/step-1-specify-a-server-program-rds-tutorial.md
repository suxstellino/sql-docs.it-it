---
description: 'Passaggio 1: Specificare un programma del server (esercitazione su RDS)'
title: 'Passaggio 1: specificare un programma server (esercitazione su RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], specifying server program
ms.assetid: d8bb35b1-c02a-4231-8d55-016e56e53b95
author: rothja
ms.author: jroth
ms.openlocfilehash: d0e59c52ee311c5172b3cdc15f58c5e9448f7110
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036400"
---
# <a name="step-1-specify-a-server-program-rds-tutorial"></a>Passaggio 1: Specificare un programma del server (esercitazione su RDS)
Nel caso più generale, utilizzare [RDS. DataSpace](../../reference/rds-api/dataspace-object-rds.md) oggetto [CreateObject](../../reference/rds-api/createobject-method-rds.md) metodo per specificare il programma server predefinito, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)o il proprio programma server personalizzato (oggetto business). Viene creata un'istanza di un programma server nel server e viene restituito un riferimento al programma server o al *proxy*.  
  
 In questa esercitazione viene usato il programma server predefinito:  
  
```vb
Sub RDSTutorial1()  
   Dim DS as New RDS.DataSpace  
   Dim DF as Object  
   Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
...  
```  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Vedere anche  
 [Passaggio 2: richiamare il programma del server (esercitazione su RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)   
 [Esercitazione su RDS (VBScript)](./rds-tutorial-vbscript.md)