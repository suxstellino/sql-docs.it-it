---
description: Proprietà personalizzate dell'attività di controllo CDC
title: Proprietà personalizzate dell'attività di controllo CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2a073699-79a2-4ea1-a68e-fc17a80b74ba
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cac3695cd99d5065862604a77a986921568ac292
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049130"
---
# <a name="cdc-control-task-custom-properties"></a>Proprietà personalizzate dell'attività di controllo CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nella tabella seguente vengono descritte le proprietà personalizzate dell'attività di controllo CDC. Tutte le proprietà sono di lettura/scrittura.  
  
|Nome proprietà|Tipo di dati|Descrizione|  
|-------------------|---------------|-----------------|  
|Connessione|ADO.NET Connection|Connessione ADO.NET al database CDC di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] per l'accesso alle tabelle delle modifiche e allo stato CDC, se è archiviato nello stesso database.<br /><br /> La connessione deve essere stabilita a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abilitato per CDC e in cui si trova la tabella delle modifiche selezionata.|  
|TaskOperation|Integer (enumerazione)|Operazione selezionata per l'attività di controllo CDC. I valori possibili sono **Mark Initial Load Start**, **Mark Initial Load End**, **Mark CDC Start**, **Get Processing Range**, **Mark Processed Range** e **Reset CDC State**.<br /><br /> Se si seleziona **MarkCdcStart**, **MarkInitialLoadStart** o **MarkInitialLoadEnd** quando si usa CDC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ovvero non di Oracle, l'utente specificato nella gestione connessione deve essere  **db_owner** o **sysadmin**.<br /><br /> Per altre informazioni su queste operazioni, vedere [Editor attività Controllo CDC](./cdc-control-task.md) e [Attività di controllo CDC](../../integration-services/control-flow/cdc-control-task.md).|  
|OperationParameter|string|Proprietà attualmente usata con l'operazione **MarkCdcStart** . Questo parametro ammette input aggiuntivo necessario per l'operazione specifica, ad esempio il numero LSN necessario per l'operazione **MarkCdcStart** .|  
|StateVariable|string|Variabile del pacchetto SSIS in cui è archiviato lo stato CDC del contesto CDC corrente. L'attività di controllo CDC legge e scrive lo stato in **StateVariable** , ma non lo carica né lo memorizza in un archivio permanente a meno che non sia selezionata la proprietà **AutomaticStatePersistence** . Vedere [Definire una variabile di stato](../../integration-services/data-flow/define-a-state-variable.md).|  
|AutomaticStatePersistence|Boolean|L'attività di controllo CDC legge lo stato CDC dalla variabile del pacchetto dello stato CDC. In seguito a un'operazione, l'attività di controllo CDC aggiorna il valore della variabile del pacchetto dello stato CDC. La proprietà **AutomaticStatePersistence** specifica l'attività di controllo CDC responsabile della persistenza del valore di stato CDC tra esecuzioni del pacchetto SSIS.<br /><br /> Quando questa proprietà è **true**, l'attività di controllo CDC carica automaticamente il valore della variabile di stato CDC da una tabella di stato. Quando l'attività di controllo CDC aggiorna il valore della variabile di stato CDC, aggiorna anche il relativo valore nella stessa tabella **table.stores** di stato, lo stato in una tabella speciale e la variabile di stato. Lo sviluppatore può controllare il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenente la tabella di stato e il relativo nome. La struttura di questa tabella di stato è predefinita.<br /><br /> Quando questa proprietà è **false**, l'attività di controllo CDC non si occupa della persistenza del valore. Se la proprietà è true, l'attività di controllo CDC archivia lo stato in una tabella speciale e aggiorna StateVariable.<br /><br /> Il valore predefinito è **true** indica che la persistenza dello stato viene aggiornata automaticamente.|  
|StateConnection|ADO.NET Connection|Connessione ADO.NET al database in cui si trova la tabella di stato quando si usa **AutomaticStatePersistence**. Il valore predefinito è lo stesso valore di **Connection**.|  
|StateName|string|Nome associato allo stato persistente. Il caricamento completo e i pacchetti CDC che utilizzano lo stesso contesto CDC specificano un nome di contesto CDC comune. Questo nome viene utilizzato per cercare la riga di stato nella tabella di stato.<br /><br /> Questa proprietà è applicabile solo quando la proprietà **AutomaticStatePersistence** è impostata su **true**.|  
|StateTable|string|Specifica il nome della tabella in cui è archiviato lo stato del contesto CDC. Questa tabella deve essere accessibile tramite la connessione configurata per il componente. Questa tabella deve includere colonne varchar denominate **name** e **state**. La colonna **state** deve includere almeno 256 caratteri.<br /><br /> Questa proprietà è applicabile solo quando la proprietà **AutomaticStatePersistence** è impostata su **true**.|  
|CommandTimeout|numero intero|Questo valore indica il timeout (in secondi) da usare quando si comunica con il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo valore viene usato quando il tempo di risposta dal database è molto lento e il valore predefinito (30 secondi) non è sufficiente.|  
  
## <a name="see-also"></a>Vedere anche  
 [Attività di controllo CDC](../../integration-services/control-flow/cdc-control-task.md)   
 [Editor dell'attività di controllo CDC](./cdc-control-task.md)  
  
