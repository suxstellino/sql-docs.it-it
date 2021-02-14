---
description: Procedure per la risoluzione dei problemi generali relativi a Posta elettronica database
title: Risoluzione dei problemi generali relativi a Posta elettronica database
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e9b7f5b5aec4f1b111983f893783e46ad14e021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352955"
---
# <a name="general-database-mail-troubleshooting-steps"></a>Procedure per la risoluzione dei problemi generali relativi a Posta elettronica database 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Per risolvere i problemi relativi a Posta elettronica database, è necessario controllare le aree generali seguenti del sistema di Posta elettronica database. Queste procedure vengono indicate in ordine logico, ma possono essere completate in qualsiasi sequenza.

## <a name="permissions"></a>Autorizzazioni

È necessario essere membri del ruolo predefinito del server sysadmin per risolvere i problemi correlati a tutti gli aspetti di Posta elettronica database. Gli utenti che non sono membri del ruolo predefinito del server sysadmin possono ottenere informazioni solo sui messaggi di posta elettronica che tentano di inviare personalmente e non su quelli inviati da altri utenti.

## <a name="is-database-mail-enabled"></a>Posta elettronica database è abilitato

1. In SQL Server Management Studio connettersi a un'istanza di SQL Server usando una finestra dell'editor di query e quindi eseguire il codice seguente:

    ```sql
    sp_configure 'show advanced', 1; 
    GO
    RECONFIGURE;
    GO
    sp_configure;
    GO
    ```

   Nel riquadro dei risultati confermare che run_value per [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) sia impostato su 1.
   Se run_value non è 1, Posta elettronica database non è abilitato. Tale caratteristica non è abilitata automaticamente per ridurre il numero di caratteristiche che potrebbero essere soggette a un attacco da parte di un utente malintenzionato. Per altre informazioni, vedere [Informazioni su Configurazione superficie di attacco](../security/surface-area-configuration.md).

1. Se si decide che l'abilitazione di Posta elettronica database non comporta alcun problema, eseguire il codice seguente:

    ```sql
    sp_configure 'Database Mail XPs', 1; 
    GO
    RECONFIGURE;
    GO
    ```

1. Per ripristinare lo stato predefinito per la procedura sp_configure, che non mostra le opzioni avanzate, eseguire il codice seguente:

    ```sql 
    sp_configure 'show advanced', 0; 
    GO
    RECONFIGURE;
    GO
    ```

## <a name="are-users-properly-configured-to-send-mail"></a>Gli utenti sono configurati correttamente per l'invio di posta elettronica

1. Per inviare Posta elettronica database, è necessario essere un membro del ruolo del database DatabaseMailUserRole nel database msdb. I membri del ruolo predefinito del server sysadmin e del ruolo msdb db_owner appartengono automaticamente anche al ruolo DatabaseMailUserRole. Per elencare tutti gli altri membri di DatabaseMailUserRole, eseguire l'istruzione seguente:

    ```sql
    EXEC msdb.sys.sp_helprolemember 'DatabaseMailUserRole';
    ```

1. Per aggiungere utenti al ruolo DatabaseMailUserRole, usare l'istruzione seguente:

    ```sql
    USE msdb;
    GO
    
    sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<database user>';
    ```

1. Per poter inviare messaggi di Posta elettronica database, gli utenti devono avere accesso ad almeno un profilo di Posta elettronica database. Per elencare gli utenti (entità) e i profili a cui hanno accesso, eseguire l'istruzione seguente.

    ```sql
    EXEC msdb.dbo.sysmail_help_principalprofile_sp;
    ```

1. Utilizzare la Configurazione guidata posta elettronica database per creare profili e consentire agli utenti di accedervi.
 
## <a name="is-database-mail-started"></a>Posta elettronica database è avviato

1. Il [programma esterno Posta elettronica database](database-mail-external-program.md) viene attivato quando sono presenti messaggi di posta elettronica da elaborare. In assenza di messaggi da inviare per il periodo di timeout specificato, il programma viene chiuso. Per verificare che Posta elettronica database sia stato avviato, eseguire l'istruzione seguente.

    ```sql
    EXEC msdb.dbo.sysmail_help_status_sp;
    ```
1. Se il programma esterno Posta elettronica database non è in esecuzione, eseguire l'istruzione seguente per avviarlo:

    ```sql
    EXEC msdb.dbo.sysmail_start_sp;
    ```

1. Se il programma esterno Posta elettronica database è in esecuzione, eseguire l'istruzione seguente per controllare lo stato della coda della posta:

    ```sql
    EXEC msdb.dbo.sysmail_help_queue_sp @queue_type = 'mail';
    ```
  
   Lo stato della coda della posta dovrebbe essere RECEIVES_OCCURRING. La coda dello stato può variare da un momento all'altro. Se lo stato della coda di posta elettronica non è RECEIVES_OCCURRING, provare a riavviare la coda. Arrestare la coda mediante l'istruzione seguente:
   
```sql
EXEC msdb.dbo.sysmail_stop_sp;
```

Avviare quindi la coda mediante l'istruzione seguente:

```sql
EXEC msdb.dbo.sysmail_start_sp;
```

  > [!NOTE]
  >  Usare la colonna length nel set di risultati di sysmail_help_queue_sp per verificare il numero di messaggi di posta elettronica nella coda della posta.

## <a name="do-problems-affect-some-or-all-accounts"></a>I problemi influiscono su alcuni o tutti gli account

1. Se si è appurato che solo alcuni profili possono inviare posta, è possibile che esistano problemi con gli account di Posta elettronica database utilizzati dai profili che non riescono a inviare messaggi. Per verificare quali account possono inviare posta, eseguire l'istruzione seguente:

    ```sql
    SELECT sent_account_id, sent_date FROM msdb.dbo.sysmail_sentitems;
    ```

1. Se un profilo che crea problemi non usa alcuno degli account elencati, è possibile che tutti gli account disponibili per tale profilo non funzionino correttamente. Per verificare i singoli account, usare la Configurazione guidata Posta elettronica database per creare un nuovo profilo con un singolo account e quindi provare a inviare un messaggio usando il nuovo account dalla finestra di dialogo Invia messaggio di prova. 
1. Per visualizzare i messaggi di errore restituiti da Posta elettronica database, eseguire l'istruzione seguente:

    ```sql
    SELECT * FROM msdb.dbo.sysmail_event_log;
    ```

   > [!NOTE]
   > Posta elettronica database contrassegna i messaggi come inviati quando sono stati recapitati correttamente a un server di posta elettronica SMTP. Il recapito dei messaggi può essere impedito anche da successivi errori, ad esempio un indirizzo di posta elettronica errato per un destinatario, che tuttavia non vengono registrati nel log di Posta elettronica database.

## <a name="retry-mail-delivery"></a>Nuovi tentativi di recapito della posta elettronica

1. Se si è appurato che gli errori di Posta elettronica database sono dovuti a problemi di contatto del server SMTP, potrebbe essere possibile migliorare la frequenza di recapito aumentando il numero di tentativi eseguiti da Posta elettronica database per l'invio di ogni messaggio. Avviare la Configurazione guidata Posta elettronica database e selezionare l'opzione Visualizza o modifica i parametri di sistema. In alternativa, è possibile associare più account al profilo in modo che in caso di errore di recapito con l'account principale sia possibile utilizzare l'account di failover per l'invio dei messaggi di posta elettronica.
1. Nella pagina Configurazione parametri di sistema i valori predefiniti pari a 60 volte per Tentativi account e cinque per Ritardo tentativi account indicano che il recapito dei messaggi avrà esito negativo se non è possibile contattare il server SMTP entro 5 minuti. Impostare un valore più alto per questi parametri per aumentare il periodo di tempo che deve trascorrere prima che il recapito dei messaggi abbia esito negativo.

    > [!NOTE]
    > Se la quantità di messaggi inviata è notevole, l'impostazione di valori predefiniti elevati può consentire di aumentare l'affidabilità, ma comporta un aumento sostanziale dell'utilizzo delle risorse a causa dei ripetuti tentativi di invio per numerosi messaggi. Affrontare il problema alla radice risolvendo l'errore di rete o del server SMTP che impedisce a Posta elettronica database di contattare tempestivamente il server SMTP.



##  <a name="see-also"></a><a name="RelatedContent"></a> Vedere anche
  
-   [Oggetti di configurazione di Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)  
  
-   [Oggetti di messaggistica di Posta elettronica database](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
-   [Programma esterno di Posta elettronica database](../../relational-databases/database-mail/database-mail-external-program.md)  
  
-   [Controlli e registrazione di Posta elettronica database](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
-   [Configurare Posta elettronica di SQL Server Agent per l'uso di Posta elettronica database](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
  
  
