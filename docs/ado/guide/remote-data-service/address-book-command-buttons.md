---
description: Pulsanti di comando di Address Book
title: Pulsanti di comando Rubrica | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO], command buttons
- RDS scenarios [ADO], command buttons
ms.assetid: 80676831-6488-4dad-a558-c47c52256a22
author: rothja
ms.author: jroth
ms.openlocfilehash: dccdb22981bb1662cd50da1e28db8fa0a2ce47d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032048"
---
# <a name="address-book-command-buttons"></a>Pulsanti di comando di Address Book
L'applicazione Address Book include i pulsanti di comando seguenti:  
  
-   Pulsante **trova** per inviare una query al database.  
  
-   Pulsante **Cancella** per cancellare le caselle di testo prima di avviare una nuova ricerca.  
  
-   Pulsante **Aggiorna profilo** per salvare le modifiche apportate a un record dipendente.  
  
-   Pulsante **Annulla modifiche per annullare** le modifiche.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="find-button"></a>Pulsante trova  
 Se si fa clic sul pulsante **trova** , viene attivata la subroutine Find_OnClick VBScript, che compila e Invia la query SQL. Quando si fa clic su questo pulsante viene popolata la griglia dei dati.  
  
## <a name="building-the-sql-query"></a>Compilazione della query SQL  
 La prima parte della sottoprocedura Find_OnClick compila la query SQL, una frase alla volta, aggiungendo stringhe di testo a un'istruzione SQL SELECT globale. Inizia impostando la variabile `myQuery` su un'istruzione SQL SELECT che richiede tutte le righe di dati dalla tabella di origine dati. Successivamente, la procedura secondaria analizza ognuna delle quattro caselle di input nella pagina.  
  
 Poiché il programma usa la parola `like` nella compilazione delle istruzioni SQL, le query sono ricerche Substringhe anziché corrispondenze esatte.  
  
 Se, ad esempio, nella casella **Cognome** è contenuta la voce "Berge" e la casella **titolo** contiene la voce "Program Manager", verrà letta l'istruzione SQL (valore di `myQuery` ):  
  
```sql
Select FirstName, LastName, Title, Email, Building, Room, Phone from Employee where lastname like 'Berge%' and title like 'Program Manager%'  
```  
  
 Se la query ha avuto esito positivo, tutte le persone con un cognome contenente il testo "Berg", ad esempio Berg e Berger, e con un titolo contenente le parole "Program Manager" (ad esempio, Program Manager, Advanced Technologies) vengono visualizzate nella griglia dei dati HTML.  
  
## <a name="preparing-and-sending-the-query"></a>Preparazione e invio della query  
 L'ultima parte della procedura secondaria Find_OnClick è costituita da due istruzioni. La prima istruzione assegna la proprietà [SQL](../../reference/rds-api/sql-property.md) di [RDS. Oggetto DataControl](../../reference/rds-api/datacontrol-object-rds.md) uguale alla query SQL compilata dinamicamente. La seconda istruzione genera il Servizi Desktop remoto **. Oggetto DataControl** ( `DC1` ) per eseguire una query sul database e quindi visualizzare i nuovi risultati della query nella griglia.  
  
```vb
Sub Find_OnClick  
   '...  
   DC1.SQL = myQuery  
   DC1.Refresh  
End Sub  
```  
  
## <a name="update-profile-button"></a>Pulsante Aggiorna profilo  
 Se si fa clic sul pulsante **Aggiorna profilo** , viene attivata la subroutine Update_OnClick VBScript, che esegue il Servizi Desktop remoto [.](../../reference/rds-api/datacontrol-object-rds.md) Metodi dell'oggetto DataControl ( `DC1` ) [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) e [Refresh](../../reference/rds-api/refresh-method-rds.md) .  
  
```vb
Sub Update_OnClick  
   DC1.SubmitChanges  
   DC1.Refresh  
End Sub  
```  
  
 Quando `DC1.SubmitChanges` viene eseguito, il servizio dati remoto crea il pacchetto di tutte le informazioni di aggiornamento e le invia al server tramite http. L'aggiornamento è All-or-Nothing; Se una parte dell'aggiornamento ha esito negativo, non viene apportata alcuna modifica e viene restituito un messaggio di stato. `DC1.Refresh` non è necessario dopo **SubmitChanges** con Remote Data Service, ma garantisce l'aggiornamento dei dati.  
  
## <a name="cancel-changes-button"></a>Pulsante Annulla modifiche  
 Se si fa clic su **Annulla modifiche** , viene attivata la subroutine Cancel_OnClick VBScript, che esegue il Servizi Desktop remoto [. Oggetto DataControl](../../reference/rds-api/datacontrol-object-rds.md) (metodo `DC1)` [CancelUpdate](../../reference/rds-api/cancelupdate-method-rds.md) .  
  
```vb
Sub Cancel_OnClick  
   DC1.CancelUpdate  
End Sub  
```  
  
 Quando `DC1.CancelUpdate` viene eseguito, vengono ignorate le modifiche apportate da un utente a un record dipendente nella griglia dei dati dall'ultima query o aggiornamento. Vengono ripristinati i valori originali.  
  
## <a name="see-also"></a>Vedere anche  
 [Pulsanti di spostamento Rubrica](./address-book-navigation-buttons.md)   
 [Oggetto DataControl (Servizi Desktop remoto)](../../reference/rds-api/datacontrol-object-rds.md)