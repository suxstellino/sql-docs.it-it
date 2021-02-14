---
description: Impostazione dell'attività Profiling dati
title: Impostazione dell'attività Profiling dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling task [Integration Services], configuring
ms.assetid: fe050ca4-fe45-43d7-afa9-99478041f9a8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 873c118622dbdd1a6b65431fcf5ba76e0c08d5cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343270"
---
# <a name="setup-of-the-data-profiling-task"></a>Impostazione dell'attività Profiling dati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Prima di poter esaminare un profilo dei dati di origine, configurare ed eseguire l'attività Profiling dati. È necessario creare questa attività all'interno di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Per configurare l'attività Profiling dati, utilizzare lo strumento Editor attività Profiling dati. Questo editor consente di selezionare la destinazione dell'output dei profili e i profili da calcolare. Dopo avere configurato l'attività, è necessario eseguire il pacchetto per calcolare i profili dati.  
  
## <a name="requirements-and-limitations"></a>Requisiti e limitazioni  
 L'attività Profiling dati funziona solo con i dati archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'attività non può essere utilizzata con origini dati di terze parti o basate su file.  
  
 Per eseguire un pacchetto contenente l'attività Profiling dati, inoltre, è necessario utilizzare un account che disponga di autorizzazioni di lettura/scrittura per il database tempdb.  
  
## <a name="data-profiling-task-in-a-package"></a>Attività Profiling dati in un pacchetto  
 L'attività Profiling dati consente solo di configurare i profili e creare il file di output contenente i profili calcolati. Per esaminare questo file di output, è necessario utilizzare Visualizzatore profilo dati, un programma di visualizzazione autonomo. Poiché è necessario visualizzare separatamente l'output, è possibile utilizzare l'attività Profiling dati in un pacchetto che non contiene altre attività.  
  
 Non è tuttavia necessario utilizzare Profiling dati come unica attività in un pacchetto. Se si desidera eseguire l'attività Profiling dati nel flusso di lavoro o nel flusso di dati di un pacchetto più complesso, sono disponibili le opzioni seguenti:  
  
-   Per implementare la logica condizionale basata sul file di output dell'attività, nel flusso di controllo del pacchetto inserire un'attività Script dopo l'attività Profiling dati. Tale attività Script potrà essere utilizzata per eseguire query sul file di output.  
  
-   Per eseguire l'attività Profiling dati del flusso di dati in seguito al caricamento e alla trasformazione dei dati, è necessario salvare temporaneamente i dati modificati in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A questo punto, è possibile eseguire il profiling dei dati salvati.  
  
 Per altre informazioni, vedere [Incorporamento di un'attività Profiling dati nel flusso di lavoro del pacchetto](../../integration-services/control-flow/incorporate-a-data-profiling-task-in-package-workflow.md).  
  
## <a name="setup-of-the-task-output"></a>Impostazione dell'output dell'attività  
 Quando l'attività Profiling dati si trova in un pacchetto, è necessario configurare l'output per i profili che verranno calcolati dall'attività. Per configurare l'output per i profili, è necessario usare la pagina **Generale** dello strumento Editor attività Profiling dati. Oltre a consentire di specificare la destinazione per l'output, la pagina **Generale** offre la possibilità di eseguire un rapido profiling dei dati. Quando si seleziona **Profilo rapido**, l'attività Profiling dati esegue il profiling di una tabella o di una vista usando alcuni o tutti i profili predefiniti con le relative impostazioni predefinite.  
  
 Per altre informazioni, vedere [Editor attività Profiling dati &#40;pagina Generale&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md) e [Form profilo rapido singola tabella &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md).  
  
> [!IMPORTANT]  
>  Il file di output potrebbe contenere dati sensibili sul database e i dati inclusi nel database. Per suggerimenti su come migliorare la sicurezza di questo file, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="selection-and-configuration-of-the-profiles-to-be-computed"></a>Selezione e configurazione dei profili da calcolare  
 Dopo avere configurato il file di output, è necessario selezionare i profili dati da calcolare. L'attività Profiling dati consente di calcolare otto profili dati diversi. Cinque di questi profili analizzano singole colonne e i tre rimanenti analizzano più colonne o relazioni tra colonne e tabelle. In una singola attività Profiling dati è possibile calcolare più profili per più colonne o combinazioni di colonne in più tabelle o viste.  
  
 Nella tabella seguente vengono descritti i report calcolati da ciascun profilo e i tipi di dati per cui il profilo è valido.  
  
|Elementi da calcolare|Valori identificati|Profilo da utilizzare|  
|----------------|-------------------------|----------------------|  
|Tutte le singole lunghezze dei valori stringa nella colonna selezionata e la percentuale di righe della tabella rappresentata da ogni lunghezza.|**Valori stringa non validi**: si analizza, ad esempio, una colonna che dovrebbe usare due caratteri per i codici di stato negli Stati Uniti, ma in cui si individua la presenza di valori più lunghi di due caratteri.|**Distribuzione lunghezze di colonna**: valido per una colonna con uno dei tipi di dati di tipo carattere indicati di seguito:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**|  
|Set di espressioni regolari relative alla percentuale specificata di valori in una colonna stringa.<br /><br /> Inoltre, espressioni regolari da utilizzare in futuro per convalidare nuovi valori.|**Valori stringa non validi o in formato non corretto**: un profilo di criteri di ricerca di una colonna Codice postale, ad esempio, potrebbe produrre le espressioni regolari \d{5}-\d{4}, \d{5} e \d{9}. Se l'output contiene altre espressioni regolari, i dati contengono valori non validi o in formato non corretto.|**Profilo Criteri di ricerca colonna**: valido per una colonna con uno dei tipi di dati di tipo carattere seguenti:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**|  
|Percentuale di valori Null nella colonna selezionata.|**Rapporto inaspettatamente elevato di valori Null in una colonna**: si analizza, ad esempio, una colonna che dovrebbe contenere i codici postali ZIP (Stati Uniti) ma si individua una percentuale troppo elevata di codici postali mancanti.|**Rapporto di valori Null nella colonna**: valido per una colonna con uno dei tipi di dati di tipo carattere seguenti:<br /><br /> **image**<br /><br /> **text**<br /><br /> **xml**<br /><br /> tipi definiti dall'utente<br /><br /> tipi variant|  
|Segnala le statistiche, ad esempio la deviazione minima, massima, media e standard per le colonne numeriche e minima e massima per le colonne di tipo **datetime** .|**Valori numerici e date non validi**: si analizza, ad esempio, una colonna di date storiche, ma si individua una data massima successiva a quella corrente.|**Profilo Statistiche di colonna**: valido per una colonna con uno dei tipi di dati indicati di seguito.<br /><br /> Tipi di dati numerici:<br /><br /> tipi integer (tranne **bit**<br /><br /> **money**<br /><br /> **smallmoney**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **real**<br /><br /> **numeric**<br /><br /> Tipi di dati di data e ora:<br /><br /> **datetime**<br /><br /> **smalldatetime**<br /><br /> **timestamp**<br /><br /> **date**<br /><br /> **time**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> Nota: per una colonna con un tipo di dati di data e ora, il profilo calcola esclusivamente il minimo e il massimo.|  
|Tutti i valori distinct nella colonna selezionata e percentuale di righe della tabella rappresentata da ciascun valore. In alternativa, valori che rappresentano più di una percentuale specificata nella tabella.|**Numero non corretto di valori distinct in un colonna**: si analizza, ad esempio, una colonna contenente gli stati degli Stati Uniti ma si individuano più di 50 valori distinct.|**Distribuzione valori di colonna**: valido per una colonna con uno dei tipi di dati seguenti.<br /><br /> Tipi di dati numerici:<br /><br /> tipi integer (tranne **bit**<br /><br /> **money**<br /><br /> **smallmoney**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **real**<br /><br /> **numeric**<br /><br /> Tipi di dati di tipo carattere:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**<br /><br /> Tipi di dati di data e ora:<br /><br /> **datetime**<br /><br /> **smalldatetime**<br /><br /> **timestamp**<br /><br /> **date**<br /><br /> **time**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**|  
|Se una colonna o un set di colonne è una chiave o una chiave approssimativa per la tabella selezionata.|**Valori duplicati in una colonna chiave potenziale**: si analizzano, ad esempio, le colonne Name e Address di una tabella Customer e si individuano valori duplicati laddove la combinazione di nome e indirizzo dovrebbe essere univoca.|**Chiave candidata**: profilo per più colonne che segnala se una colonna o un set di colonne può fungere da chiave per la tabella selezionata. Valido per colonne con uno dei tipi di dati indicati di seguito.<br /><br /> Tipi di dati integer:<br /><br /> **bit**<br /><br /> **tinyint**<br /><br /> **smallint**<br /><br /> **int**<br /><br /> **bigint**<br /><br /> Tipi di dati di tipo carattere:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**<br /><br /> Tipi di dati di data e ora:<br /><br /> **datetime**<br /><br /> **smalldatetime**<br /><br /> **timestamp**<br /><br /> **date**<br /><br /> **time**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**|  
|Grado di dipendenza dei valori inclusi in una colonna (colonna dipendente) dai valori presenti in un'altra colonna o set di colonne (colonna determinante).|**Valori non validi nelle colonne dipendenti**: si analizza, ad esempio, una dipendenza tra una colonna contenente i codici postali ZIP (Stati Uniti) e una colonna contenente gli stati degli Stati Uniti. Ciascun codice postale dovrebbe corrispondere sempre allo stesso stato. Il profilo individua tuttavia violazioni della dipendenza.|**Dipendenza funzionale**: valido per le colonne con uno dei tipi di dati indicati di seguito.<br /><br /> Tipi di dati integer:<br /><br /> **bit**<br /><br /> **tinyint**<br /><br /> **smallint**<br /><br /> **int**<br /><br /> **bigint**<br /><br /> Tipi di dati di tipo carattere:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**<br /><br /> Tipi di dati di data e ora:<br /><br /> **datetime**<br /><br /> **smalldatetime**<br /><br /> **timestamp**<br /><br /> **date**<br /><br /> **time**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**|  
|Se una colonna o un set di colonne può fungere da chiave esterna tra le tabelle selezionate.<br /><br /> Ovvero, il profilo segnala la sovrapposizione nei valori tra due colonne o set di colonne.|**Valori non validi**: si analizza, ad esempio, la colonna ProductID di una tabella Sales. Il profilo individua che la colonna contiene valori non inclusi nella colonna ProductID della tabella Products.|**Inclusione valore**: valido per le colonne con uno dei tipi di dati indicati di seguito:<br /><br /> Tipi di dati integer:<br /><br /> **bit**<br /><br /> **tinyint**<br /><br /> **smallint**<br /><br /> **int**<br /><br /> **bigint**<br /><br /> Tipi di dati di tipo carattere:<br /><br /> **char**<br /><br /> **nchar**<br /><br /> **varchar**<br /><br /> **nvarchar**<br /><br /> Tipi di dati di data e ora:<br /><br /> **datetime**<br /><br /> **smalldatetime**<br /><br /> **timestamp**<br /><br /> **date**<br /><br /> **time**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**|  
  
 Per selezionare i profili da calcolare, usare la pagina **Richieste profilo** di Editor attività Profiling dati. Per altre informazioni, vedere [Editor attività Profiling dati &#40;pagina Richieste profilo&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Nella pagina **Richieste profilo** è inoltre possibile specificare l'origine dati e configurare i profili dati. Quando si configura l'attività, tenere presenti le informazioni seguenti:  
  
-   Per semplificare la configurazione e l'individuazione delle caratteristiche di dati poco noti, è possibile usare il carattere jolly **(\*)** al posto di un singolo nome di colonna. Se si utilizza questo carattere jolly, l'attività eseguirà il profiling di tutte le colonne con un tipo di dati appropriato, il che rallenterà ulteriormente l'elaborazione.  
  
-   Quando la tabella o la vista selezionata è vuota, l'attività Profiling dati non calcola i profili.  
  
-   Quando tutti i valori nella colonna selezionata sono Null, l'attività Profiling dati calcola solo il profilo Rapporto di valori Null nella colonna. Non calcola il profilo Distribuzione lunghezze di colonna, Criteri di ricerca colonna, Statistiche di colonna o Distribuzione valori di colonna per la colonna vuota.  
  
 A ognuno dei profili dati disponibili si applicano opzioni di configurazione specifiche. Per ulteriori informazioni su tali opzioni, vedere gli argomenti seguenti:  
  
-   [Opzioni di Richiesta profilo Chiave candidata &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Distribuzione lunghezze di colonna &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Rapporto di valori Null nella colonna &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Criteri di ricerca colonna &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Statistiche di colonna &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Distribuzione valori di colonna &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Dipendenza funzionale &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md)  
  
-   [Opzioni di Richiesta profilo Inclusione valore &#40;Attività Profiling dati&#41;](../../integration-services/control-flow/value-inclusion-profile-request-options-data-profiling-task.md)  
  
## <a name="execution-of-the-package-that-contains-the-data-profiling-task"></a>Esecuzione del pacchetto contenente l'attività Profiling dati  
 Dopo avere configurato l'attività Profiling dati, è possibile eseguirla. L'attività calcola quindi i profili dati e restituisce queste informazioni in formato XML in un file o una variabile del pacchetto. La struttura di tale formato XML segue lo schema DataProfile.xsd. È possibile aprire lo schema in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o in un altro editor di schemi, in un editor XML o in un editor di testo, ad esempio il Blocco note. Questo schema per le informazioni sulla qualità dei dati può essere utile nelle situazioni seguenti:  
  
-   Scambio di informazioni sulla qualità dei dati all'interno di un'organizzazione e tra organizzazioni diverse.  
  
-   Compilazione di strumenti personalizzati da utilizzare con le informazioni sulla qualità dei dati.  
  
 Lo spazio dei nomi di destinazione viene identificato nello schema come [https://schemas.microsoft.com/sqlserver/2008/DataDebugger/](https://schemas.microsoft.com/sqlserver/2008/DataDebugger/).  
  
## <a name="next-step"></a>passaggio successivo  
 [Visualizzatore profilo dati](../../integration-services/control-flow/data-profile-viewer.md).  
  
  
