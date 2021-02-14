---
description: Visualizzatore profilo dati
title: Visualizzatore profilo dati | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dataprofileviewer.f1
helpviewer_keywords:
- Data Profile Viewer [Integration Services]
- Data Profiling task [Integration Services], output viewer
ms.assetid: b9043428-ce26-45bb-910c-588d07579565
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aaa5d2276908feccd86aac75012703e093b861d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352132"
---
# <a name="data-profile-viewer"></a>Visualizzatore profilo dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La visualizzazione e l'analisi dei profili dati costituiscono il passaggio successivo del processo di profiling dei dati. È possibile visualizzare tali profili dopo avere eseguito l'attività Profiling dati in un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e avere calcolato i profili dati. Per altre informazioni su come configurare ed eseguire le attività Profiling dati, vedere [Impostazione dell'attività Profiling dati](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
> [!IMPORTANT]  
>  Il file di output potrebbe contenere dati sensibili sul database e i dati inclusi nel database. Per suggerimenti su come rendere più sicuro questo file, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="data-profiles"></a>Profili dei dati  
 Per visualizzare i profili dati, è necessario configurare l'attività Profiling dati per inviare l'output a un file e quindi utilizzare il Visualizzatore profilo dati autonomo. Per aprire il Visualizzatore profilo dati, effettuare una delle azioni seguenti:  
  
-   Fare clic con il pulsante destro del mouse sull'attività **Profiling dati** in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , quindi scegliere **Modifica**. Fare clic su **Apri Visualizzatore profilo** nella pagina **Generale** dell' **Editor attività Profiling dati**.  
  
-   Nella cartella *\<drive>* :\Programmi (x86) | Programmi\Microsoft SQL Server\110\DTS\Binn eseguire DataProfileViewer.exe.  
  
 Il visualizzatore contiene più riquadri per visualizzare i profili richiesti e i risultati calcolati, con funzionalità di drill-down e dettagli facoltativi:  
  
 Riquadro **Profili**  
 Nel riquadro **Profili** vengono visualizzati i profili richiesti nell'attività Profiling dati. Per visualizzare i risultati calcolati per il profilo, selezionare il profilo nel riquadro **Profili** . I risultati verranno visualizzati negli altri riquadri del visualizzatore.  
  
 Riquadro **Risultati**  
 Il riquadro **Risultati** usa una singola riga per riepilogare i risultati calcolati del profilo. Se, ad esempio, si richiede un **profilo Distribuzione lunghezze di colonna**, questa riga include le lunghezze minima e massima e il conteggio delle righe. Per la maggior parte dei profili è possibile selezionare questa riga nel riquadro **Risultati** per visualizzare altri dettagli nel riquadro **Dettagli** facoltativo.  
  
 Riquadro **Dettagli**  
 Per la maggior parte dei tipi di profilo, il riquadro **Dettagli** contiene informazioni aggiuntive sui risultati del profilo selezionati nel riquadro **Risultati** . Se, ad esempio, si richiede un **profilo Distribuzione lunghezze di colonna**, nel riquadro **Dettagli** verrà visualizzata ogni lunghezza di colonna rilevata. Nel riquadro viene inoltre visualizzato il numero e la percentuale di righe in cui il valore di colonna ha la lunghezza di colonna specificata.  
  
 Per i tre tipi di profilo calcolati in più colonne (profili Chiave candidata, Dipendenza funzionale e Inclusione valore), nel riquadro **Dettagli** vengono visualizzate le eventuali violazioni della relazione prevista. Se, ad esempio, si richiede un profilo Chiave candidata, nel riquadro Dettagli verranno visualizzati i valori duplicati che violano l'univocità della chiave candidata.  
  
 Se l'origine dati usata per calcolare il profilo è disponibile, è possibile fare doppio clic su una riga nel riquadro **Dettagli** per visualizzare le righe di dati corrispondenti nel riquadro **Drill-down** .  
  
 Riquadro **Drill-down**  
 È possibile fare doppio clic su una riga nel riquadro **Dettagli** per visualizzare le righe di dati corrispondenti nel riquadro **Drill-down** quando sono vere le condizioni seguenti:  
  
-   L'origine dati utilizzata per calcolare il profilo è disponibile.  
  
-   Si dispone dell'autorizzazione per visualizzare i dati.  
  
 Per la connessione al database di origine per una richiesta di drill-down, il Visualizzatore profilo dati utilizza l'autenticazione di Windows e le credenziali dell'utente corrente. Tramite il Visualizzatore profilo dati non vengono utilizzate le informazioni di connessione archiviate nel pacchetto che ha eseguito l'attività Profiling dati.  
  
> [!IMPORTANT]  
>  La funzionalità di drill-down, disponibile nel Visualizzatore profilo dati, consente di inviare query in tempo reale all'origine dati originale. Queste query possono avere un impatto negativo sulle prestazioni del server.  
>   
>  Se si esegue il drill-down da un file di output non creato recentemente, le query di drill-down potrebbero restituire un set di righe diverso rispetto a quello usato per calcolare l'output originale.  
  
 Per altre informazioni sull'interfaccia utente del Visualizzatore profilo dati, vedere [Guida sensibile al contesto del Visualizzatore profilo dati]().  
  
## <a name="data-profile-viewer-f1-help"></a>Guida sensibile al contesto del Visualizzatore profilo dati
  Utilizzare il Visualizzatore profilo dati per visualizzare l'output dell'attività Profiling dati.  
  
 Per altre informazioni sull'uso del Visualizzatore profilo dati, vedere [Visualizzatore profilo dati](../../integration-services/control-flow/data-profile-viewer.md). Per altre informazioni sull'uso dell'attività Profiling dati che crea l'output del profilo analizzato nel Visualizzatore profilo dati, vedere [Impostazione dell'attività Profiling Dati](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
### <a name="static-options"></a>Opzioni statiche  
 **Apri**  
 Consente di accedere al file salvato che contiene l'output dell'attività Profiling dati.  
  
 Riquadro **Profili**  
 Espandere l'albero nel riquadro **Profili** per visualizzare i profili inclusi nell'output. Selezionare un profilo per visualizzarne i risultati.  
  
 Riquadro **Messaggio**  
 Visualizza i messaggi di stato.  
  
 Riquadro **Drill-down**  
 Visualizza le righe di dati che corrispondono a un valore nell'output, se l'origine dati utilizzata dall'attività Profiling dati è disponibile.  
  
 Se, ad esempio, si visualizza l'output di un profilo Distribuzione valori di colonna per una colonna US State, il riquadro **Distribuzione dettagliata valori** potrebbe contenere una riga per "WA". Fare doppio clic sulla riga nel riquadro **Distribuzione dettagliata valori** per visualizzare le righe di dati in cui il valore della colonna di stato è "WA" nel riquadro di drill-down.  
  
### <a name="dynamic-options"></a>Opzioni dinamiche  
  
#### <a name="profile-type--column-length-distribution-profile"></a>Tipo di profilo: profilo Distribuzione lunghezze di colonna  
  
##### <a name="column-length-distribution-profile---column-pane"></a>Profilo Distribuzione lunghezze di colonna - Riquadro \<column>  
 **Lunghezza minima**  
 Visualizza la lunghezza minima dei valori nella colonna.  
  
 **Lunghezza massima**  
 Visualizza la lunghezza massima dei valori nella colonna.  
  
 **Ignora spazi iniziali**  
 Indica se il profilo è stato calcolato con un valore **IgnoreLeadingSpaces** impostato su True o False. Questa proprietà è stata impostata nella pagina **Richieste profilo** in Editor attività Profiling dati.  
  
 **Ignora spazi finali**  
 Indica se il profilo è stato calcolato con un valore **IgnoreTrailingSpaces** impostato su True o False. Questa proprietà è stata impostata nella pagina **Richieste profilo** in Editor attività Profiling dati.  
  
 **Conteggio righe**  
 Visualizza il numero di righe della tabella o della vista.  
  
##### <a name="detailed-length-distribution-pane"></a>Riquadro Distribuzione dettagliata lunghezze  
 **Lunghezza**  
 Visualizza le lunghezze di colonna rilevate nella colonna analizzata.  
  
 **Numero**  
 Visualizza il numero di righe in cui il valore della colonna analizzata ha la lunghezza indicata nella colonna **Lunghezza** .  
  
 **Percentuale**  
 Visualizza la percentuale di righe in cui il valore della colonna analizzata ha la lunghezza indicata nella colonna **Lunghezza** .  
  
#### <a name="profile-type--column-null-ratio-profile"></a>Tipo di profilo: profilo Rapporto di valori Null nella colonna  
  
##### <a name="column-null-ratio-profile---column-pane"></a>Profilo Rapporto di valori Null nella colonna - Riquadro \<column>  
 **Conteggio valori Null**  
 Visualizza il numero di righe in cui il valore della colonna analizzata è un valore Null.  
  
 **Percentuale valori Null**  
 Visualizza la percentuale di righe in cui il valore della colonna analizzata è un valore Null.  
  
 **Conteggio righe**  
 Visualizza il numero di righe della tabella o della vista.  
  
#### <a name="profile-type--column-pattern-profile"></a>Tipo di profilo: profilo Criteri di ricerca colonna  
  
##### <a name="column-pattern-profile---column-pane"></a>Profilo Criteri di ricerca colonna - Riquadro \<column>  
 **Conteggio righe**  
 Visualizza il numero di righe della tabella o della vista.  
  
##### <a name="pattern-distribution-pane"></a>Riquadro Distribuzione criteri di ricerca  
 **Modello**  
 Visualizza i modelli calcolati per la colonna analizzata.  
  
 **Percentuale**  
 Visualizza la percentuale di righe i cui valori corrispondono al modello visualizzato nella colonna **Modello** .  
  
#### <a name="profile-type--column-statistics-profile"></a>Tipo di profilo: profilo Statistiche di colonna  
  
##### <a name="column-statistics-profile---column-pane"></a>Profilo Statistiche di colonna - Riquadro \<column>  
 **Minimi**  
 Visualizza il valore minimo rilevato nella colonna analizzata.  
  
 **Massimo**  
 Visualizza il valore massimo rilevato nella colonna analizzata.  
  
 **Media**  
 Visualizza la media dei valori rilevati nella colonna analizzata.  
  
 **Deviazione standard**  
 Visualizza la deviazione standard dei valori rilevati nella colonna analizzata.  
  
#### <a name="profile-type--column-value-distribution-profile"></a>Tipo di profilo: profilo Distribuzione valori di colonna  
  
##### <a name="column-value-distribution-profile---column-pane"></a>Profilo Distribuzione valori di colonna - Riquadro \<column>  
 **Numero di valori distinct**  
 Visualizza il conteggio dei valori distinct rilevati nella colonna analizzata.  
  
 **Conteggio righe**  
 Visualizza il numero di righe della tabella o della vista.  
  
##### <a name="detailed-value-distribution-pane"></a>Riquadro Distribuzione dettagliata valori  
 **Valore**  
 Visualizza i valori distinct rilevati nella colonna analizzata.  
  
 **Numero**  
 Visualizza il numero di righe in cui la colonna analizzata ha il valore indicato nella colonna **Valore** .  
  
 **Percentuale**  
 Visualizza la percentuale di righe in cui la colonna analizzata ha il valore indicato nella colonna **Valore** .  
  
#### <a name="profile-type--candidate-key-profile"></a>Tipo di profilo: profilo Chiave candidata  
  
##### <a name="candidate-key-profile---table-pane"></a>Profilo Chiave candidata - Riquadro \<table>  
 **Colonne chiave**  
 Visualizza le colonne selezionate per l'analisi come chiave candidata.  
  
 **Attendibilità chiave**  
 Visualizza il livello di attendibilità (come percentuale) della colonna o combinazione di colonne chiave candidata. Un livello di attendibilità inferiore al 100% indica la presenza di valori duplicati.  
  
##### <a name="key-violations-pane"></a>Riquadro Violazioni chiave  
 **\<column1>\<column2> così via.**  
 Visualizza i valori duplicati rilevati nella colonna analizzata.  
  
 **Numero**  
 Visualizza il numero di righe in cui la colonna specificata ha il valore indicato nella prima colonna.  
  
#### <a name="profile-type--functional-dependency-profile"></a>Tipo di profilo: profilo Dipendenza funzionale  
  
##### <a name="functional-dependency-profile-pane"></a>Riquadro Profilo Dipendenza funzionale  
 **Colonne determinanti**  
 Visualizza la colonna o le colonne selezionate come colonne determinanti. Nell'esempio in cui uno stesso codice postale ZIP (Stati Uniti) deve essere associato sempre allo stesso stato, il codice postale rappresenta la colonna determinante.  
  
 **Colonna dipendente**  
 Visualizza la colonna o le colonne selezionate come colonne dipendenti. Nell'esempio in cui uno stesso codice postale ZIP (Stati Uniti) deve essere associato sempre allo stesso stato, lo stato rappresenta la colonna dipendente.  
  
 **Attendibilità dipendenza funzionale**  
 Visualizza il livello di attendibilità (come percentuale) della dipendenza funzionale tra colonne. Un livello di attendibilità inferiore al 100% indica che vi sono casi in cui il valore determinante non determina il valore dipendente. Nell'esempio in cui uno stesso codice postale ZIP (Stati Uniti) deve essere associato sempre allo stesso stato, tale livello di attendibilità indica che alcuni valori di stato non sono validi.  
  
##### <a name="functional-dependency-violations-pane"></a>Riquadro Violazioni dipendenza funzionale  
  
> [!NOTE]  
>  Una percentuale elevata di valori non corretti nei dati può provocare risultati imprevisti da un profilo Dipendenza funzionale. Ad esempio, il 90% delle righe contiene il valore di stato "WI" per il valore di codice postale ZIP "98052". Il profilo segnala le righe che contengono il valore di stato "WA" corretto come violazioni.  
  
 **\<determinant column name>**  
 Visualizza il valore della colonna determinante o della combinazione di colonne determinanti in questa istanza di una violazione della dipendenza funzionale.  
  
 **\<dependent column name>**  
 Visualizza il valore della colonna dipendente in questa istanza di una violazione della dipendenza funzionale.  
  
 **Conteggio del supporto**  
 Visualizza il numero di righe in cui il valore della colonna determinante determina la colonna dipendente.  
  
 **Conteggio violazioni**  
 Visualizza il numero di righe in cui il valore della colonna determinante non determina la colonna dipendente. Si tratta delle righe in cui i valori dipendenti corrispondono ai valori indicati nella colonna **\<dependent column name>** .  
  
 **Percentuale del supporto**  
 Visualizza la percentuale di righe in cui la colonna determinante determina la colonna dipendente.  
  
#### <a name="profile-type--value-inclusion-profile"></a>Tipo di profilo: profilo Inclusione valore  
  
##### <a name="value-inclusion-profile-pane"></a>Riquadro Profilo Inclusione valore  
 **Colonne lato subset**  
 Visualizza la colonna o la combinazione di colonne analizzate per determinare se sono incluse nelle colonne del superset.  
  
 **Colonne lato superset**  
 Visualizza la colonna o la combinazione di colonne analizzate per determinare se includono i valori delle colonne del subset.  
  
 **Attendibilità inclusione**  
 Visualizza il livello di attendibilità (come percentuale) della sovrapposizione tra colonne. Un livello di attendibilità inferiore al 100% indica che vi sono casi in cui il valore del subset non viene rilevato tra i valori del superset.  
  
##### <a name="inclusion-violations-pane"></a>Riquadro Violazioni inclusione  
 **\<column1>\<column2> così via.**  
 Visualizza i valori nella colonna o colonne del subset che non sono disponibili nella colonna o colonne del superset.  
  
 **Numero**  
 Visualizza il numero di righe in cui la colonna specificata ha il valore indicato nella prima colonna.  
