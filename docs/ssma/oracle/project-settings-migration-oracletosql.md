---
description: Impostazioni del progetto (migrazione) (OracleToSQL)
title: Impostazioni progetto (migrazione) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: fcd6b988-633b-4b2b-9f36-6368b5e86b60
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 2ee27caa615f26b20a953478095b252e127bcf9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067896"
---
# <a name="project-settings-migration-oracletosql"></a>Impostazioni del progetto (migrazione) (OracleToSQL)
La pagina Migrazione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA esegue la migrazione dei dati da Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Il riquadro migrazione è disponibile nelle finestre di dialogo **Impostazioni** progetto e **Impostazioni progetto predefinite** .  
  
-   Per specificare le impostazioni per tutti i progetti SSMA, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto di migrazione per cui è necessario visualizzare o modificare le impostazioni dall'elenco a discesa **versione destinazione migrazione** fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
## <a name="migration-engine"></a>Motore di migrazione  
  
|Termine|Definizione|  
|--------|--------------|  
|**Motore di migrazione**|Specifica il motore di database utilizzato durante la migrazione dei dati. La migrazione dei dati lato client si riferisce al client SSMA che recupera i dati dall'origine e li inserisce in SQL Server. La migrazione dei dati lato server fa riferimento al motore di migrazione dei dati di SSMA (programma per la copia bulk) in esecuzione nella casella SQL Server come processo di SQL Agent che recupera i dati dall'origine e li inserisce direttamente in SQL Server evitando così un hop client-hop aggiuntivo (prestazioni migliori).<br /><br />**Modalità predefinita**: motore di migrazione dati lato client<br /><br />**Modalità ottimistica**: motore di migrazione dati lato client<br /><br />**Modalità completa**: motore di migrazione dati lato client|  
  
> [!IMPORTANT]  
> Quando l'opzione del **motore di migrazione** è impostata sul **motore di migrazione dei dati lato server**, viene visualizzata una nuova opzione di impostazione del progetto **utilizza motore di migrazione dati lato server a 32 bit** . Specifica se per la migrazione dei dati viene utilizzata l'utilità BCP (bit per la copia bulk) a 32 bit o 64.  
  
## <a name="miscellaneous-options"></a>Opzioni varie  
  
|Termine|Definizione|  
|--------|--------------|  
|**Dimensioni batch**|Specifica la dimensione del batch utilizzata durante la migrazione dei dati.<br /><br />**Modalità predefinita**: 10000<br /><br />**Modalità ottimistica**: 10000<br /><br />**Modalità completa**: 10000|  
|**Vincoli CHECK**|Specifica se SSMA deve verificare i vincoli quando inserisce i dati in tabelle SQL Server.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
|**Timeout migrazione dati**|Specifica il timeout utilizzato durante la migrazione dei dati<br /><br />**Modalità predefinita**: 15<br /><br />**Modalità ottimistica**: 15<br /><br />**Modalità completa**: 15|  
|**Opzioni di migrazione dei dati estese**|Mostra le opzioni di migrazione dei dati aggiuntive per ogni tabella nella scheda Dettagli separata.<br /><br />**Modalità predefinita**: Nascondi<br /><br />**Modalità ottimistica**: Nascondi<br /><br />**Modalità completa**: Nascondi|  
|**Attive trigger**|Specifica se SSMA deve attivare i trigger di inserimento quando aggiunge dati alle tabelle SQL Server.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
|**Mantieni valori Identity**|Specifica se SSMA conserva i valori null nei dati di origine quando aggiunge dati a SQL Server, indipendentemente dai valori predefiniti specificati nel SQL Server.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: false|  
|**Mantieni valori Null**|Specifica se SSMA conserva i valori null nei dati di origine quando aggiunge dati a SQL Server, indipendentemente dai valori predefiniti specificati nel SQL Server.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**Contrassegno operazione Trim stringa con errore**|Se la dimensione della colonna di destinazione è inferiore alla lunghezza della stringa di origine, il valore verrà tagliato e contrassegnato come un errore.<br /><br />**Modalità predefinita**: Sì<br /><br />**Modalità ottimistica**: Sì<br /><br />**Modalità completa**: Sì|  
|**In caso di errore**|Interrompe la migrazione dei dati quando si verifica un errore. Sono disponibili tre opzioni:<br /><br />**Interrompi migrazione:** Arresta l'operazione di migrazione dei dati<br /><br />**Vai alla tabella successiva:** Interrompe la migrazione dei dati alla tabella corrente e procede a quello successivo<br /><br />**Vai al batch successivo:** Interrompe la migrazione dei dati al batch corrente e procede a quello successivo<br /><br />**Modalità predefinita**: passare al batch successivo<br /><br />**Modalità ottimistica**: passare al batch successivo<br /><br />**Modalità completa**: passare al batch successivo|  
|**Sostituisci date non supportate**|Specifica se SSMA deve correggere le date antecedenti alla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  data di data/ora precedente (01 gennaio 1753).<br /><br />Per memorizzare i valori correnti della data, selezionare **non eseguire alcuna operazione**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non accetterà le date precedenti al 01 gennaio 1753 in una colonna DateTime. Se si utilizzano date precedenti, è necessario convertire i valori DateTime in valori di tipo carattere.<br /><br />Per convertire le date precedenti al 01 gennaio 1753 in NULL, selezionare **Sostituisci con null**.<br /><br />Per sostituire le date precedenti al 01 gennaio 1753 con una data supportata, selezionare **Sostituisci con la data più vicina supportata**.<br /><br />**Modalità predefinita**: non eseguire alcuna operazione<br /><br />**Modalità ottimistica**: non eseguire alcuna operazione<br /><br />**Modalità completa**: sostituire con la data più vicina supportata|  
|**Blocco a livello di tabella**|Specifica se SSMA blocca le tabelle quando aggiunge dati alle tabelle durante la migrazione dei dati. Ottiene un blocco di aggiornamento in blocco per la durata dell'operazione di copia bulk. Se il valore è false, viene impostato un blocco a livello di riga.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
  
## <a name="parallel-data-migration"></a>Migrazione di dati paralleli  
  
|Termine|Definizione|  
|--------|--------------|  
|**Modalità di migrazione dei dati parallela**|Specifica la modalità utilizzata per la divisione dei thread per abilitare la migrazione dei dati parallela. In modalità auto, SSMA sceglie il numero di thread (10 per impostazione predefinita) con fork per eseguire la migrazione dei dati. In modalità personalizzata, l'utente può specificare il numero di thread con fork per la migrazione dei dati (il valore minimo è 1 e il valore massimo è 100). Attualmente, solo il motore di migrazione dei dati lato client supporta la migrazione dei dati paralleli.<br /><br />**Modalità predefinita**: auto<br /><br />**Modalità ottimistica**: auto<br /><br />**Modalità completa**: auto|  
  
> [!IMPORTANT]  
> Quando l'opzione **modalità di migrazione dei dati parallela** è impostata su **personalizzata**, viene visualizzata una nuova opzione per l'impostazione del **numero di thread** . Specifica il numero di thread usati per la migrazione dei dati.  
  
