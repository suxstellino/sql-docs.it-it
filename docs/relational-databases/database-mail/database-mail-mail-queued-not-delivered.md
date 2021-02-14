---
description: 'Posta elettronica database: Posta elettronica accodata, non recapitata'
title: 'Posta elettronica database: Posta elettronica accodata, non recapitata | Microsoft Docs'
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
ms.openlocfilehash: ebf8096c4b679cc6d5ac01b164bb9ae4bb014e6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352940"
---
# <a name="database-mail-mail-queued-not-delivered"></a>Posta elettronica database: Posta elettronica accodata, non recapitata 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

In questo argomento vengono descritte le procedure di risoluzione dei problemi relativi al mancato recapito di messaggi di posta elettronica inseriti senza errori in una coda.

## <a name="diagnose-the-problem"></a>Diagnosticare il problema 

Il programma esterno Posta elettronica database registra le attività di posta elettronica nel database **msdb**.

Verificare prima di tutto che Posta elettronica database sia abilitato tramite l'[opzione Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) della stored procedure di sistema **sp_configure**, come nell'esempio seguente:

```sql 
EXEC sp_configure 'show advanced', 1;  
RECONFIGURE; 
EXEC sp_configure; 
GO
```

Eseguire quindi l'istruzione seguente nel database **msdb** per verificare lo stato della coda della posta elettronica:

```sql
sysmail_help_queue_sp @queue_type = 'Mail' ;
```

Per una spiegazione dettagliata delle colonne, vedere [sysmail_help_queue_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-queue-sp-transact-sql.md#result-set).

Verificare l'attività nella vista **sysmail_event_log**. La vista dovrebbe contenere una voce che specifica che il programma esterno Posta elettronica database è stato avviato. Se la vista **sysmail_event_log** non contiene voci, vedere il sintomo [Messages Queued, No Entries](database-mail-common-errors.md#database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log) in **sysmail_event_log**. Se la vista **sysmail_event_log** contiene errori, risolvere il problema specifico.

Se la vista **sysmail_event_log** contiene errori, verificare lo stato dei messaggi nella vista **sysmail_allitems**.

## <a name="message-status-unsent"></a>Stato del messaggio non inviato 

Lo stato **non inviato** indica che il [programma esterno Posta elettronica database](database-mail-external-program.md) non ha ancora elaborato il messaggio. È possibile che il programma sia in ritardo con l'elaborazione dei messaggi. La velocità di elaborazione dipende dalle condizioni della rete, dal timeout fra i tentativi di invio, dal volume dei messaggi e dalla capacità del server SMTP. Se il problema persiste, è consigliabile utilizzare più di un profilo per la distribuzione di messaggi fra più server SMTP.

Controllare la data dell'ultimo recapito senza errori. Se è trascorso del tempo dall'ultimo recapito senza errori, controllare la vista sysmail_event_log per verificare che il programma esterno sia stato avviato in modo corretto da Service Broker. Se durante l'ultimo tentativo il programma esterno non è stato avviato, verificare che il programma esterno Posta elettronica database si trovi nella directory corretta e che l'account del servizio per SQL Server abbia l'autorizzazione per l'avvio dell'eseguibile.

   > [!NOTE]
   > Per eliminare i messaggi precedenti non inviati, attendere fino a quando i messaggi non recapitabili non diventano i meno recenti nella coda e quindi usare [sysmail_delete_mailitems_sp](../system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) per eliminarli.

## <a name="message-status-retrying"></a>Stato del messaggio nuovo tentativo in corso

Questo stato indica che Posta elettronica database ha tentato di recapitare il messaggio al server SMTP, ma l'operazione non è riuscita. In genere, questo problema è causato da un'interruzione di rete, un errore sul server SMTP o un account di Posta elettronica database configurato in modo non corretto. Alla fine il messaggio dovrebbe riuscire o non riuscire e pubblicare un messaggio nel log eventi.

## <a name="message-status-sent"></a>Stato del messaggio inviato

Lo stato **inviato** indica che il programma esterno Posta elettronica database ha recapitato correttamente il messaggio al server SMTP. Se il messaggio non è giunto a destinazione, il server SMTP ha accettato il messaggio da Posta elettronica database, ma non lo ha recapitato al destinatario finale. Verificare i log del server SMTP, oppure rivolgersi all'amministratore del server SMTP. È inoltre possibile eseguire un test del server di posta elettronica SMTP utilizzando un altro client, ad esempio Outlook Express.

## <a name="message-status-failed"></a>Stato del messaggio non riuscito

Lo stato non riuscito indica che il programma esterno Posta elettronica database non è stato in grado di recapitare il messaggio al server SMTP. In questo caso, la vista **sysmail_event_log** contiene informazioni dettagliate provenienti dal programma esterno. Per un esempio di query che unisce **sysmail_faileditems** e **sysmail_event_log** per recuperare i messaggi di errore dettagliati, vedere [Controllare lo stato di messaggi di posta elettronica inviati con Posta elettronica database](check-the-status-of-e-mail-messages-sent-with-database-mail.md). Le cause di errore più frequenti sono la presenza di un indirizzo di destinazione errato oppure problemi di rete che impediscono al programma esterno di raggiungere uno o più account di failover. È inoltre possibile che la posta venga respinta per problemi specifici a livello del server SMTP. Usando Configurazione guidata Posta elettronica database, modificare l'impostazione dell'opzione **Livello di registrazione** su **Dettagliato** e inviare un messaggio di prova per individuare il punto di errore.



##  <a name="see-also"></a><a name="RelatedContent"></a> Vedere anche
  
-  [Panoramica di Posta elettronica database](database-mail.md)

  
  
