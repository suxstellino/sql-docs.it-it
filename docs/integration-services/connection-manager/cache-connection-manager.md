---
description: gestione connessione della cache
title: Gestione connessione della cache | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cacheconnection.f1
helpviewer_keywords:
- Cache connection manager
ms.assetid: bdc92038-3720-4795-8a5c-79b963f2c952
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fba6ab0ff6c58228f702983b6338c2bd9965e5e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338439"
---
# <a name="cache-connection-manager"></a>gestione connessione della cache

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La gestione connessione della cache consente di leggere i dati dalla trasformazione cache o da un file di cache (con estensione caw) e di salvarli in un file di cache. Se si configura la gestione connessione della cache in modo da utilizzare un file di cache, i dati sono archiviati sempre in memoria.  
  
 La trasformazione Trasformazione cache consente di scrivere i dati da un'origine dati connessa nel flusso di dati a una gestione connessione della cache. La trasformazione Ricerca in un pacchetto consente di effettuare ricerche nei dati.  
  
> [!NOTE]  
>  I tipi di dati BLOB (oggetto binario di grandi dimensioni), ovvero DT_TEXT, DT_NTEXT e DT_IMAGE, non sono supportati nella Gestione connessione cache. Se il set di dati di riferimento contiene un tipo di dati BLOB, il componente avrà esito negativo quando viene eseguito il pacchetto. È possibile utilizzare **Editor gestione connessione cache** per modificare i tipi di dati di colonna. Per altre informazioni, vedere [Editor gestione connessione cache]().  
  
> [!NOTE]  
>  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="configuration-of-the-cache-connection-manager"></a>Configurazione della gestione connessione cache  
 Per configurare la gestione connessione cache, procedere nel modo seguente:  
  
-   Indicare se utilizzare un file di cache.  
  
     Se la gestione connessione della cache viene configurata in modo da utilizzare un file di cache, la gestione connessione eseguirà una delle seguenti azioni:  
  
    -   Salvare i dati in un file quando la trasformazione Cache viene configurata in modo da scrivere i dati da un'origine dati nel flusso di dati alla gestione connessione della cache.  
  
    -   Leggere i dati dal file di cache.  
  
     Per ulteriori informazioni, vedere [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Modificare i metadati per le colonne archiviate nella cache.  
  
-   Aggiornare il nome del file di cache in fase di esecuzione usando un'espressione per impostare la proprietà ConnectionString. Per altre informazioni, vedere [Utilizzo delle espressioni di proprietà nei pacchetti](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o a livello di codice.  
  
 Per informazioni su come configurare una gestione connessione a livello di programmazione, vedere <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> e [Aggiunta di connessioni a livello di programmazione](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="cache-connection-manager-editor"></a>Editor gestione connessione cache
  La Gestione connessione cache consente di leggere un set di dati di riferimento dalla trasformazione cache o da un file di cache (.caw) e può salvare i dati in un file di cache. I dati vengono sempre archiviati in memoria.  
  
> [!NOTE]  
>  I tipi di dati BLOB (oggetto binario di grandi dimensioni), ovvero DT_TEXT, DT_NTEXT e DT_IMAGE, non sono supportati nella Gestione connessione cache. Se il set di dati di riferimento contiene un tipo di dati BLOB, il componente avrà esito negativo quando viene eseguito il pacchetto. È possibile utilizzare **Editor gestione connessione cache** per modificare i tipi di dati di colonna.  
  
 La trasformazione Ricerca esegue ricerche sul set di dati di riferimento.  
  
 La finestra di dialogo **Editor gestione connessione della cache** include le schede seguenti:  
  
###  <a name="general-tab"></a><a name="generaltab"></a> Scheda Generale  
 Usare la scheda **Generale** della finestra di dialogo **Editor gestione connessione della cache** per indicare se leggere la cache da un file o salvare la cache in un file.  
  
#### <a name="options"></a>Opzioni  
 **Nome gestione connessione**  
 Consente di specificare un nome univoco per la connessione cache nel flusso di lavoro. Il nome specificato verrà visualizzato in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Descrizione**  
 Consente di aggiungere una descrizione per la connessione. È consigliabile includere nella descrizione informazioni sugli scopi della connessione, in modo da ottenere pacchetti autodocumentati e semplificarne quindi la gestione.  
  
 **Usa cache di file**  
 Indicare se utilizzare un file di cache.  
  
> [!NOTE]  
>  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
 Se la gestione connessione della cache viene configurata in modo da utilizzare un file di cache, la gestione connessione eseguirà una delle seguenti azioni:  
  
-   Salvare i dati in un file quando la trasformazione Cache viene configurata in modo da scrivere i dati da un'origine dati nel flusso di dati alla gestione connessione della cache. Per ulteriori informazioni, vedere [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Leggere i dati dal file di cache.  
  
 **Nome file**  
 Digitare il percorso e il nome file del file di cache.  
  
 **Sfoglia**  
 Individuare il file di cache.  
  
 **Aggiorna metadati**  
 Eliminare i metadati della colonna nella Gestione connessione cache e ripopolare la Gestione connessione cache con i metadati della colonna da un file di cache selezionato.  
  
###  <a name="columns-tab"></a><a name="columnstab"></a> Scheda Colonne  
 Utilizzare la scheda **Colonne** della finestra di dialogo **Editor gestione connessione cache** per configurare le proprietà di ciascuna colonna nella cache.  
  
#### <a name="options"></a>Opzioni  
 **Colonna**  
 Consente di specificare il nome della colonna.  
  
 **Posizione dell'indice**  
 Consente di specificare quali colonne sono colonne dell'indice specificando la relativa posizione di ogni colonna. L'indice è un insieme di una o più colonne.  
  
 Per le colonne non dell'indice, la posizione è 0.  
  
 Per le colonne di indice, la posizione di indice è un numero sequenziale e positivo. Questo numero indica l'ordine con cui la trasformazione Ricerca confronta le righe nel set di dati di riferimento alle righe nell'origine dei dati di input. La colonna con più valori univoci deve disporre della posizione di indice più bassa.  
  
> [!NOTE]  
>  Quando la trasformazione Ricerca viene configurata per utilizzare una Gestione connessione cache, è possibile eseguire il mapping solo delle colonne di indice nel set di dati di riferimento alle colonne di input. Inoltre, è necessario eseguire il mapping di tutte le colonne di indice.  
  
 **Tipo**  
 Consente di specificare il tipo di dati della colonna.  
  
 **Lunghezza**  
 Specifica il tipo di dati della colonna. Se applicabile al tipo di dati, è possibile aggiornare **Length**.  
  
 **Precisione**  
 Specifica la precisione per certi tipi di dati di colonna. La precisione è il numero di cifre in un numero. Se applicabile al tipo di dati, è possibile aggiornare **Precision**.  
  
 **Ridimensionamento**  
 Specifica la scala per certi tipi di dati di colonna. La scala è il numero di cifre a destra della virgola decimale in un numero. Se applicabile al tipo di dati, è possibile aggiornare **Scale**.  
  
 **Tabella codici**  
 Specifica la tabella codici per il tipo di colonna. Se applicabile al tipo di dati, è possibile aggiornare **Code Page**.  
  
## <a name="related-tasks"></a>Attività correlate  
 [Implementare una trasformazione Ricerca in modalità Full Cache tramite la gestione connessione della cache](lookup-transformation-full-cache-mode-cache-connection-manager.md)  
  
