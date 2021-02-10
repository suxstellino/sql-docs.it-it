---
description: Impostazioni del progetto (migrazione) (SybaseToSQL)
title: Impostazioni progetto (migrazione) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 82f8857f-7ab1-4738-ab6e-b1e95ea94924
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 96b5550449e11cd314e736a36e3845ba92f542c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069935"
---
# <a name="project-settings-migration-sybasetosql"></a>Impostazioni del progetto (migrazione) (SybaseToSQL)
La pagina Migrazione della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA esegue la migrazione dei dati da Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Il riquadro migrazione è disponibile nelle finestre di dialogo **Impostazioni** progetto e **Impostazioni progetto predefinite** .  
  
-   Per specificare le impostazioni per tutti i progetti SSMA, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, selezionare il tipo di progetto di migrazione per il quale è necessario visualizzare le impostazioni/changed dall'elenco a discesa **versione destinazione migrazione** fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
-   Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **migrazione**.  
  
## <a name="date-correction-options"></a>Opzioni di correzione della data  
  
|Termine|Definizione|  
|--------|--------------|  
|**Sostituisci date non supportate**|Specifica se SSMA deve correggere le date antecedenti alla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  data di data/ora precedente (01 gennaio 1753).<br /><br />Per memorizzare i valori correnti della data, selezionare **non eseguire alcuna operazione**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non accetterà le date precedenti al 01 gennaio 1753 in una colonna DateTime. Se si utilizzano date precedenti, è necessario convertire i valori DateTime in valori di tipo carattere.<br /><br />Per convertire le date precedenti al 01 gennaio 1753 in NULL, selezionare **Sostituisci con null**.<br /><br />Per sostituire le date precedenti al 01 gennaio 1753 con una data supportata, selezionare **Sostituisci con la data più vicina supportata**.<br /><br />**Modalità predefinita**: non eseguire alcuna operazione<br /><br />**Modalità ottimistica**: non eseguire alcuna operazione<br /><br />**Modalità completa**: sostituire con la data più vicina supportata|  
  
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
|**Mantieni valori Identity**|Specifica se SSMA conserva i valori Identity di Sybase quando aggiunge dati a SQL Server. Il valore false fa sì che i valori Identity vengano assegnati dalla destinazione.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**Mantieni valori Null**|Specifica se SSMA conserva i valori null nei dati di origine quando aggiunge dati a SQL Server, indipendentemente dai valori predefiniti specificati nel SQL Server.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**In caso di errore**|Interrompe la migrazione dei dati quando si verifica un errore. Sono disponibili tre opzioni:<br /><br />**Interrompi migrazione:** Arresta l'operazione di migrazione dei dati<br /><br />**Vai alla tabella successiva:** Interrompe la migrazione dei dati alla tabella corrente e procede a quello successivo<br /><br />**Vai al batch successivo:** Interrompe la migrazione dei dati al batch corrente e procede a quello successivo<br /><br />**Modalità predefinita**: passare al batch successivo<br /><br />**Modalità ottimistica**: passare al batch successivo<br /><br />**Modalità completa**: passare al batch successivo|  
|**Parte frazionaria arrotondata dei numeri**|Specifica se tagliare le parti frazionarie dei dati numerici e decimali durante la migrazione ai tipi Integer o visualizzare il messaggio di errore se la parte frazionaria è non semplice<br /><br />**Modalità predefinita**: No<br /><br />**Modalità ottimistica**: No<br /><br />**Modalità completa**: No|  
|**Sybase Unicode endian**|Specifica il tipo endian per le stringhe Unicode Sybase. Per questa particolare impostazione è possibile impostare le opzioni seguenti:<br /><br />Little-Endian<br /><br />Big endian<br /><br />**Modalità predefinita**: Little-Endian<br /><br />**Modalità ottimistica**: Little-Endian<br /><br />**Modalità completa**: Little-Endian|  
|**Blocco a livello di tabella**|Specifica se SSMA blocca le tabelle quando aggiunge dati alle tabelle durante la migrazione dei dati. Ottiene un blocco di aggiornamento in blocco per la durata dell'operazione di copia bulk. Se il valore è false, viene impostato un blocco a livello di riga.<br /><br />**Modalità predefinita**: true<br /><br />**Modalità ottimistica**: true<br /><br />**Modalità completa**: true|  
|**Usa cursori**|Se questa opzione è impostata, i dati vengono recuperati dal database di origine utilizzando i cursori.<br /><br />**Modalità predefinita**: false<br /><br />**Modalità ottimistica**: false<br /><br />**Modalità completa**: false|  
  
## <a name="parallel-data-migration"></a>Migrazione di dati paralleli  
  
|Termine|Definizione|  
|--------|--------------|  
|**Modalità di migrazione dei dati parallela**|Specifica la modalità utilizzata per la divisione dei thread per abilitare la migrazione dei dati parallela. In modalità auto, SSMA sceglie il numero di thread (10 per impostazione predefinita) con fork per eseguire la migrazione dei dati. In modalità personalizzata, l'utente può specificare il numero di thread con fork per la migrazione dei dati (il valore minimo è 1 e il valore massimo è 100). Attualmente, solo il motore di migrazione dei dati lato client supporta la migrazione dei dati paralleli.<br /><br />**Modalità predefinita**: auto<br /><br />**Modalità ottimistica**: auto<br /><br />**Modalità completa**: auto|  
  
> [!IMPORTANT]  
> Quando l'opzione **modalità di migrazione dei dati parallela** è impostata su **personalizzata**, viene visualizzata una nuova opzione per l'impostazione del **numero di thread** . Specifica il numero di thread usati per la migrazione dei dati.  
  
