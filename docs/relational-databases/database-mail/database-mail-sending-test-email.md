---
description: Inviare un messaggio di prova con Posta elettronica database
title: Inviare un messaggio di prova con Posta elettronica database | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 58b9594c7069347f7e458a5a375bdaf2e2d29103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272352"
---
# <a name="send-a-test-email-with-database-mail"></a>Inviare un messaggio di prova con Posta elettronica database  
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Usare la finestra di dialogo Invia messaggio di prova per verificare se è possibile inviare messaggi con un profilo specifico.

## <a name="permissions"></a>Autorizzazioni

È necessario essere membri del ruolo predefinito del server sysadmin per usare la finestra di dialogo Invia messaggio di prova. Gli utenti che non sono membri del ruolo predefinito del server sysadmin possono verificare il funzionamento di Posta elettronica database tramite la procedura [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md).

## <a name="procedure"></a>Procedura

1. In Esplora oggetti in [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) connettersi a un'istanza del Motore di database di SQL Server in cui è configurato Posta elettronica database, espandere Gestione, fare clic con il pulsante destro del mouse su Posta elettronica database e quindi scegliere Invia messaggio di prova. Se non esiste alcun profilo di Posta elettronica database, verrà visualizzata una finestra di dialogo per chiedere all'utente di creare un profilo e viene avviata la Configurazione guidata Posta elettronica database.
1. Nella finestra di dialogo **Invia messaggio di prova** da <instance name> selezionare il profilo da provare nella casella Profilo di posta elettronica database.
1. Nella casella **A** digitare il nome di posta elettronica del destinatario del messaggio di prova.
1. Nella casella **Oggetto** digitare l'oggetto del messaggio di prova. Modificare il testo predefinito in modo da indicare chiaramente che si tratta di un messaggio di posta elettronica inviato per la risoluzione di problemi.
1. Nella casella **Corpo** digitare il corpo del messaggio di prova. Modificare il testo predefinito in modo da indicare chiaramente che si tratta di un messaggio di posta elettronica inviato per la risoluzione di problemi.
1. Selezionare **Invia messaggio di prova** per inviare il messaggio di prova nella coda di Posta elettronica database.
1. Quando si invia il messaggio di prova, viene visualizzata la finestra di dialogo Messaggio di prova di Posta elettronica database. Prendere nota del numero visualizzato nella casella Messaggio inviato. Questo numero rappresenta il valore mailitem_id del messaggio di prova. Selezionare OK.
1. Fare clic su Nuova query sulla barra degli strumenti per aprire una finestra dell'editor di query. Eseguire l'istruzione T-SQL seguente per verificare lo stato del messaggio di prova:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_allitems 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```

    La colonna sent_status indica se il messaggio di prova è stato inviato.

1. Se si sono verificati errori, eseguire l'istruzione seguente per visualizzare il messaggio di errore:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log 
    WHERE mailitem_id = <the mailitem_id from the previous step> ;
    ```


##  <a name="see-also"></a><a name="RelatedContent"></a> Vedere anche 
  
-   [Oggetti di configurazione di Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)
-   [Oggetti di messaggistica di Posta elettronica database](../../relational-databases/database-mail/database-mail-messaging-objects.md)
-   [Programma esterno di Posta elettronica database](../../relational-databases/database-mail/database-mail-external-program.md)
-   [Controlli e registrazione di Posta elettronica database](../../relational-databases/database-mail/database-mail-log-and-audits.md)
-   [Configurare Posta elettronica di SQL Server Agent per l'uso di Posta elettronica database](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)
  
  
