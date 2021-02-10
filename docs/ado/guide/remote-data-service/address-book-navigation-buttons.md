---
description: Pulsanti di spostamento di Address Book
title: Pulsanti di spostamento della Rubrica | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], navigation buttons
- address book application scenario [ADO], navigation buttons
ms.assetid: f0dd84c6-5c33-4ab9-82b4-4c42dfdd2277
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee0bb698c64de6fb04f776caa4e34c27b8d49a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032062"
---
# <a name="address-book-navigation-buttons"></a>Pulsanti di spostamento di Address Book
L'applicazione Address Book Visualizza i pulsanti di spostamento nella parte inferiore della pagina Web. È possibile utilizzare i pulsanti di navigazione per spostarsi tra i dati nella visualizzazione della griglia HTML selezionando la prima o l'ultima riga di dati o le righe adiacenti alla selezione corrente.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="navigation-sub-procedures"></a>Procedure secondarie di navigazione  
 L'applicazione Rubrica contiene diverse procedure che consentono agli utenti di fare clic sui pulsanti **primo**, **Avanti**, **indietro** e **ultimo** per spostarsi tra i dati.  
  
 Se ad esempio si fa clic sul **primo** pulsante, viene attivata la subroutine First_OnClick VBScript. La stored procedure esegue un metodo [MoveFirst](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , che rende la selezione corrente la prima riga di dati. Se si fa clic sul pulsante **ultimo** , viene attivata la procedura secondaria Last_OnClick, che richiama il metodo [MoveLast](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , rendendo l'ultima riga di dati la selezione corrente. I pulsanti di spostamento rimanenti funzionano in modo simile.  
  
```vb
' Move to the first record in the bound Recordset.  
Sub First_OnClick  
   DC1.Recordset.MoveFirst  
End Sub  
  
' Move to the next record from the current position   
' in the bound Recordset.  
Sub Next_OnClick  
   If Not DC1.Recordset.EOF Then    ' Cannot move beyond bottom record.  
      DC1.Recordset.MoveNext  
      Exit Sub  
   End If     
End Sub  
  
' Move to the previous record from the current position in the bound   
' Recordset.  
Sub Prev_OnClick  
   If Not DC1.Recordset.BOF Then    ' Cannot move beyond top record.  
      DC1.Recordset.MovePrevious  
      Exit Sub  
   End If  
End Sub  
  
' Move to the last record in the bound Recordset.  
Sub Last_OnClick  
   DC1.Recordset.MoveLast  
End Sub  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Metodi MoveFirst, MoveLast, MoveNext e MovePrevious (Servizi Desktop remoto)](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)