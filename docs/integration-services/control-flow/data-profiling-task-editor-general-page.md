---
description: Editor attività Profiling dati (pagina Generale)
title: Editor attività Profiling dati (pagina Generale) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.general.f1
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: eec15906-d757-4079-b2f6-aca4e52b3b4c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8a30c9fe99378aa06ebf9bfc69dca24ab09028a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352100"
---
# <a name="data-profiling-task-editor-general-page"></a>Editor attività Profiling dati (pagina Generale)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Usare la pagina **Generale** di **Editor attività Profiling dati** per configurare le opzioni seguenti:  
  
-   Specificare la destinazione per l'output del profilo.  
  
-   Utilizzare le impostazioni predefinite per semplificare l'attività di profiling di una singola tabella o vista.  
  
 Per altre informazioni sull'uso dell'attività Profiling dati, vedere [Impostazione dell'attività Profiling dati](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Per altre informazioni sull'uso del Visualizzatore profilo dati per analizzare l'output dell'attività Profiling dati, vedere [Visualizzatore profilo dati](../../integration-services/control-flow/data-profile-viewer.md).  
  
 **Per aprire la pagina Generale in Editor attività Profiling dati**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenente l'attività Profiling dati.  
  
2.  Nella scheda **Flusso di controllo** fare doppio clic sull'attività Profiling dati.  
  
3.  In **Editor attività Profiling dati** fare clic su **Generale**.  
  
## <a name="data-profiling-options"></a>Opzioni di profiling dei dati  
 **Timeout**  
 Specifica il numero di secondi dopo il quale attivare il timeout dell'attività Profiling dati e arrestarne l'esecuzione. Il valore predefinito è 0, che non specifica alcun timeout.  
  
## <a name="destination-options"></a>Opzioni destinazione  
  
> [!IMPORTANT]  
>  Il file di output potrebbe contenere dati sensibili sul database e i dati inclusi nel database. Per suggerimenti su come migliorare la sicurezza di questo file, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
 **DestinationType**  
 Consente di specificare se salvare l'output del profilo dei dati in un file o una variabile:  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**FileConnection**|Consente di salvare l'output del profilo in un file nel percorso specificato in una gestione connessione file.<br /><br /> Nota: è possibile specificare la gestione connessione file da usare tramite l'opzione **Destination** .|  
|**Variabile**|Consente di salvare l'output del profilo in una variabile del pacchetto.<br /><br /> Nota: è possibile specificare la variabile del pacchetto da usare tramite l'opzione **Destination** .|  
  
 **Destinazione**  
 Consente di specificare la gestione connessione file o la variabile del pacchetto contenente l'output del profilo dei dati:  
  
-   Se l'opzione **DestinationType** è impostata su **FileConnection**, l'opzione **Destination** visualizza le gestioni connessione file disponibili. Selezionare una delle gestioni connessione oppure fare clic su \<New File connection> per creare una nuova gestione connessione file.  
  
-   Se l'opzione **DestinationType** è impostata su **Variable**, l'opzione **Destination** visualizza le variabili del pacchetto disponibili nell'elenco **Destination** . Selezionare una delle variabili oppure fare clic su \<New Variable> per crearne una nuova.  
  
 **OverwriteDestination**  
 Consente di specificare se sovrascrivere il file di output, se presente. Il valore predefinito è **False**. Il valore di questa proprietà viene utilizzato solo quando l'opzione DestinationType è impostata su FileConnection. Quando l'opzione DestinationType è impostata su Variable, l'attività sovrascrive sempre il valore precedente della variabile.  
  
> [!IMPORTANT]  
>  Se si tenta di eseguire l'attività Profiling dati più di una volta senza modificare il nome del file di output o impostare il valore della proprietà **OverwriteDestination** su **True**, l'attività restituirà un errore e un messaggio indicante che il file di output è già presente.  
  
## <a name="other-options"></a>Altre opzioni  
 **Profilo rapido**  
 Consente di visualizzare l'opzione **Form profilo rapido singola tabella**. Questo form semplifica l'attività di profiling di una singola tabella o vista tramite le impostazioni predefinite. Per altre informazioni, vedere [Form profilo rapido singola tabella &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md).  
  
 **Apri Visualizzatore profilo**  
 Consente di aprire il Visualizzatore profilo dati. Il Visualizzatore profilo dati autonomo visualizza l'output del profilo dei dati dall'attività Profiling dati. È possibile visualizzare l'output del profilo dei dati dopo avere eseguito l'attività Profiling dati nel pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e aver calcolato i profili dati.  
  
> [!NOTE]  
>  È anche possibile aprire il Visualizzatore profilo dati eseguendo DataProfileViewer.exe nella cartella *\<drive>* :\Programmi (x86) | Programmi\Microsoft SQL Server\110\DTS\Binn.  
  
## <a name="see-also"></a>Vedere anche  
 [Form profilo rapido singola tabella &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)   
 [Editor attività Profiling dati &#40;pagina Richieste profilo&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)  
  
  
