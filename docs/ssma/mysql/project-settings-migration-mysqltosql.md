---
description: Impostazioni del progetto (migrazione) (MySQLToSQL)
title: Impostazioni progetto (migrazione) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2a3cba9e-cd54-4a8b-b858-8fc4cf2580d9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92c6e91ca49046561c24d7716dd5a3e152e686f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017371"
---
# <a name="project-settings-migration-mysqltosql"></a>Impostazioni del progetto (migrazione) (MySQLToSQL)
La pagina Migrazione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA esegue la migrazione dei dati da MySQL a SQL Server.  
  
Il riquadro migrazione è disponibile nelle finestre di dialogo **Impostazioni progetto** e **Impostazioni progetto predefinite** .  
  
-   Per specificare le impostazioni per tutti i progetti SSMA, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto nell'elenco a discesa **versione destinazione di migrazione** di cui si desidera accedere alle impostazioni, fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
## <a name="options"></a>Opzioni  
  
### <a name="bulk-copy"></a>Copia bulk  
  
|Termine|Definizione|  
|--------|--------------|  
|**Dimensioni batch**|Specifica la dimensione del batch utilizzata durante la migrazione dei dati.<br /><br />**Modalità predefinita**: 1000<br /><br />**Modalità ottimistica**: 1000<br /><br />**Modalità completa**: 1000|  
|**Vincoli CHECK**|Specifica se SSMA deve verificare i vincoli quando inserisce i dati in tabelle SQL Server.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
|**Attive trigger**|Specifica se SSMA deve attivare i trigger di inserimento quando aggiunge dati alle tabelle SQL Server.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
|**Mantieni valori Identity**|Specifica se SSMA conserva i valori Identity di MySQL quando aggiunge dati a SQL Server. Il valore false fa sì che i valori Identity vengano assegnati dalla destinazione.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**Mantieni valori Null**|Specifica se SSMA conserva i valori null nei dati di origine quando aggiunge dati a SQL Server, indipendentemente dai valori predefiniti specificati nel SQL Server.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**Blocco a livello di tabella**|Specifica se SSMA blocca le tabelle quando aggiunge dati alle tabelle durante la migrazione dei dati. Ottiene un blocco di aggiornamento in blocco per la durata dell'operazione di copia bulk. Se il valore è false, viene impostato un blocco a livello di riga.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
  
### <a name="data-modification"></a>Modifica dei dati  
  
|Termine|Definizione|  
|--------|--------------|  
|**Migrazione di date non valide**|Specifica come eseguire la migrazione di date non valide con like ' 2007-04-23' o ' 2000-06-31 10:00:00' nei formati data e DATETIME.<br /><br />**Modalità predefinita**: impostare null<br /><br />**Modalità ottimistica**: impostare null<br /><br />**Modalità completa**: impostare null|  
|**Migrazione dei valori TEMPORALi negativi**|Specifica come eseguire la migrazione di valori negativi come '-30:11:00' nelle colonne TEMPORALi.<br /><br />**Modalità predefinita**: impostare null<br /><br />**Modalità ottimistica**: impostare null<br /><br />**Modalità completa**: impostare null|  
|**Valori TEMPORALi per la migrazione a 24 ore**|Specifica come eseguire la migrazione di valori TEMPORALi maggiori di ' 23:59:59' nelle colonne TEMPORALi.<br /><br />**Modalità predefinita**: impostare null<br /><br />**Modalità ottimistica**: impostare null<br /><br />**Modalità completa**: impostare null|  
|**Troncare i valori binari per rientrare nella colonna**|In caso affermativo, SSMA tronca i valori binari da MySQL che non rientrano nelle colonne della tabella SQL e genera un messaggio di errore appropriato. Se no, la riga genera un errore<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: No|  
|**Troncare i valori di carattere da inserire nella colonna**|SSMA tronca i valori di carattere da MySQL che non rientrano nelle colonne della tabella SQL e genera un messaggio di errore appropriato.<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: No|  
|**Migrazione di date zero**|Specifica come eseguire la migrazione di zero date come ' 0000-00-00' o ' 0000-00-00 00:00:00' nelle colonne DATE e DATETIME.<br /><br />**Modalità predefinita**: impostare null<br /><br />**Modalità ottimistica**: impostare null<br /><br />**Modalità completa**: impostare null|  
|**Migrazione di zero nella data**|Specifica la modalità di migrazione delle date con zero parti, ad esempio ' 2009-01-00' o ' 2000-00-00 11:00:00', nelle colonne DATE e DATETIME.<br /><br />**Modalità predefinita**: impostare null<br /><br />**Modalità ottimistica**: impostare null<br /><br />**Modalità completa**: impostare null|  
  
### <a name="migration-engine"></a>Motore di migrazione  
  
|Termine|Definizione|  
|--------|--------------|  
|**Motore di migrazione**|Specifica il motore di database utilizzato durante la migrazione dei dati. La migrazione dei dati lato client si riferisce al client SSMA che recupera i dati dall'origine e li inserisce in SQL Server. La migrazione dei dati lato server fa riferimento al motore di migrazione dei dati di SSMA (programma per la copia bulk) in esecuzione nella casella SQL Server come processo di SQL Agent che recupera i dati dall'origine e li inserisce direttamente in SQL Server evitando così un hop client-hop aggiuntivo (prestazioni migliori).<br /><br />**Modalità predefinita**: motore di migrazione dati lato client<br /><br />**Modalità ottimistica**: motore di migrazione dati lato client<br /><br />**Modalità completa**: motore di migrazione dati lato client|  
  
> [!IMPORTANT]  
> Quando l'opzione del **motore di migrazione** è impostata sul **motore di migrazione dei dati lato server**, viene visualizzata una nuova opzione di impostazione del progetto **utilizza motore di migrazione dati lato server a 32 bit** . Specifica se per la migrazione dei dati viene utilizzata l'utilità BCP (bit per la copia bulk) a 32 bit o 64.  
  
### <a name="misc"></a>Varie  
  
|Termine|Definizione|  
|--------|--------------|  
|**Opzioni di migrazione dei dati estese**|Mostra le opzioni di migrazione dei dati aggiuntive per ogni tabella nella scheda Dettagli separata.<br /><br />**Modalità predefinita**: Nascondi<br /><br />**Modalità ottimistica**: Nascondi<br /><br />**Modalità completa**: Nascondi|  
|**In caso di errore**|Interrompe la migrazione dei dati quando si verifica un errore. Sono disponibili tre opzioni:<br /><br />**Interrompi migrazione:** Arresta l'operazione di migrazione dei dati<br /><br />**Vai alla tabella successiva:** Interrompe la migrazione dei dati alla tabella corrente e procede a quello successivo<br /><br />**Vai al batch successivo:** Interrompe la migrazione dei dati al batch corrente e procede a quello successivo<br /><br />**Modalità predefinita**: passare al batch successivo<br /><br />**Modalità ottimistica**: passare al batch successivo<br /><br />**Modalità completa**: passare al batch successivo|  
  
### <a name="parallel-data-migration"></a>Migrazione di dati paralleli  
  
|Termine|Definizione|  
|--------|--------------|  
|**Modalità di migrazione dei dati parallela**|Specifica la modalità utilizzata per la divisione dei thread per abilitare la migrazione dei dati parallela. In modalità auto, SSMA sceglie il numero di thread (10 per impostazione predefinita) con fork per eseguire la migrazione dei dati. In modalità personalizzata, l'utente può specificare il numero di thread con fork per la migrazione dei dati (il valore minimo è 1 e il valore massimo è 100). Attualmente, solo il motore di migrazione dei dati lato client supporta la migrazione dei dati paralleli.<br /><br />**Modalità predefinita**: auto<br /><br />**Modalità ottimistica**: auto<br /><br />**Modalità completa**: auto|  
  
> [!IMPORTANT]  
> Quando l'opzione **modalità di migrazione dei dati parallela** è impostata su **personalizzata**, viene visualizzata una nuova opzione per l'impostazione del **numero di thread** . Specifica il numero di thread usati per la migrazione dei dati.  
  
### <a name="spatial-data"></a>Dati spaziali  
  
|Termine|Definizione|  
|--------|--------------|  
|**Gestione degli errori**|Specifica come gestire gli errori di migrazione dei valori dei tipi di dati spaziali. Se si specifica ' Sostituisci con NULL ', tutti i valori spaziali che causano errori verranno sostituiti con NULL. In caso contrario, non viene eseguita alcuna sostituzione.<br /><br />**Modalità predefinita**: genera un errore<br /><br />**Modalità ottimistica**: generare un errore<br /><br />**Modalità completa**: genera un errore|  
|**Convalida del valore**|Specifica come gestire i valori spaziali non validi. Se si specifica ' try make valid ', viene effettuato un tentativo di modificare i valori non validi per renderli validi.<br /><br />**Modalità predefinita**: rendi valida<br /><br />**Modalità ottimistica**: non modificare<br /><br />**Modalità completa**: rendi valida|  
  
