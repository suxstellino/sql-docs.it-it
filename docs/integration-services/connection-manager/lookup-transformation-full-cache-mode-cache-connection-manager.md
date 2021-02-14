---
description: Trasformazione Ricerca in modalità Full Cache - Gestione connessione della cache
title: Trasformazione Ricerca in modalità Full Cache - Gestione connessione della cache | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: 58bc7611-5fb5-4113-9742-10959e06b94c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c6560951e789dc16c78788b278579abb906fa3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350329"
---
# <a name="lookup-transformation-full-cache-mode---cache-connection-manager"></a>Trasformazione Ricerca in modalità Full Cache - Gestione connessione della cache

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  È possibile configurare la trasformazione Ricerca per utilizzare la modalità Full Cache e una gestione connessione della cache. Nella modalità Full Cache, il set di dati di riferimento viene caricato nella cache prima dell'esecuzione della trasformazione Ricerca.  
  
> [!NOTE]  
>  I tipi di dati BLOB (oggetto binario di grandi dimensioni), ovvero DT_TEXT, DT_NTEXT e DT_IMAGE, non sono supportati nella Gestione connessione cache. Se il set di dati di riferimento contiene un tipo di dati BLOB, il componente avrà esito negativo quando viene eseguito il pacchetto. È possibile utilizzare **Editor gestione connessione cache** per modificare i tipi di dati di colonna. Per altre informazioni, vedere [Editor gestione connessione cache](./cache-connection-manager.md).  
  
 La trasformazione Ricerca esegue ricerche creando un join dei dati contenuti nelle colonne di input da un'origine dati connessa con le colonne in un set di dati di riferimento. Per altre informazioni, vedere [Trasformazione Ricerca](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Utilizzare uno dei seguenti elementi per generare un set di dati di riferimento:  
  
-   File cache (.caw)  
  
     Configurare la Gestione connessione cache per leggere i dati provenienti da un file di cache esistente.  
  
-   Origine dati collegata nel flusso di dati  
  
     Utilizzare una Trasformazione di tipo cache per scrivere i dati da un'origine dati connessa nel flusso di dati a una Gestione connessione cache. I dati vengono sempre archiviati in memoria.  
  
     È necessario aggiungere la trasformazione Ricerca a un flusso di dati separato. Questo consente alla Trasformazione cache di popolare la Gestione connessione cache prima dell'esecuzione della trasformazione Ricerca. I flussi di dati possono essere nello stesso pacchetto o in due pacchetti separati.  
  
     Se i flussi di dati sono nello stesso pacchetto, utilizzare un vincolo di precedenza per connettere i flussi di dati. Questo consente l'esecuzione della Trasformazione cache prima dell'esecuzione della trasformazione Ricerca.  
  
     Se i flussi di dati sono in pacchetti separati, è possibile utilizzare l'attività Esegui pacchetto per chiamare il pacchetto figlio dal pacchetto padre. È possibile chiamare più pacchetti figlio aggiungendo più attività Esegui pacchetto all'attività Contenitore Sequenza nel pacchetto padre.  
  
 È possibile condividere il set di dati di riferimento archiviato in cache tra più trasformazioni Ricerca utilizzando uno dei metodi seguenti:  
  
-   Configurare le trasformazioni Ricerca in un singolo pacchetto per utilizzare la stessa modalità Gestione connessione cache.  
  
-   Configurare le gestioni connessione della cache nei diversi pacchetti per l'utilizzo dello stesso file di cache.  
  
 Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Trasformazione Cache](../../integration-services/data-flow/transformations/cache-transform.md)  
  
-   [Gestione connessione della cache](../../integration-services/connection-manager/cache-connection-manager.md)  
  
-   [Vincoli di precedenza](../../integration-services/control-flow/precedence-constraints.md)  
  
-   [Attività Esegui pacchetto](../../integration-services/control-flow/execute-package-task.md)  
  
-   [Sequenza - contenitore](../../integration-services/control-flow/sequence-container.md)  
  
 Per visualizzare un video che illustra come implementare una trasformazione Ricerca in modalità Full Cache usando la gestione connessione della cache, vedere [How to: Implement a Lookup Transformation in Full Cache Mode (SQL Server Video)](/previous-versions/sql/sql-server-2008/cc952929(v=sql.100))(Procedura: Implementare una trasformazione Ricerca nella modalità Full Cache).  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-one-package-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Per implementare una trasformazione Ricerca nella modalità Full Cache in un pacchetto utilizzando la gestione connessione della cache e un'origine dati nel flusso di dati  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire un progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e quindi aprire un pacchetto.  
  
2.  Nella scheda **Flusso di controllo** aggiungere due attività Flusso di dati e quindi connettere le attività usando un connettore verde:  
  
3.  Nel primo flusso di dati, aggiungere una trasformazione del tipo cache e quindi connettere la trasformazione a un'origine dati.  
  
     Configurare l'origine dati in base alle esigenze.  
  
4.  Fare doppio clic sulla Trasformazione cache e quindi in **Editor trasformazione cache**, nella pagina **Gestione connessione** fare clic su **Nuova** per creare una nuova gestione connessione della cache.  
  
5.  Fare clic sulla scheda **Colonne** della finestra di dialogo **Editor gestione connessione cache** e quindi specificare le colonne di indice usando l'opzione **Posizione dell'indice** .  
  
     Per le colonne non dell'indice, la posizione è 0. Per le colonne di indice, la posizione di indice è un numero sequenziale e positivo.  
  
    > [!NOTE]  
    >  Quando la trasformazione Ricerca viene configurata per utilizzare una Gestione connessione cache, è possibile eseguire il mapping solo delle colonne di indice nel set di dati di riferimento alle colonne di input. Inoltre, è necessario eseguire il mapping di tutte le colonne di indice. Per altre informazioni, vedere [Editor gestione connessione cache](./cache-connection-manager.md).  
  
6.  Per salvare la cache in un file, in **Editor gestione connessione della cache**, nella scheda **Generale** configurare la Gestione connessione cache impostando le opzioni seguenti:  
  
    -   Selezionare **Usa cache dei file**.  
  
    -   In **Nome file** digitare il percorso del file o fare clic su **Sfoglia** per selezionare il file.  
  
         Se viene digitato un percorso di file che non esiste, il sistema crea il file quando viene eseguito il pacchetto.  
  
    > [!NOTE]  
    >  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
7.  Configurare la Trasformazione Cache in base alle esigenze. Per altre informazioni, vedere [Editor trasformazione cache &#40;pagina Gestione connessioni&#41;](../data-flow/transformations/cache-transform.md) e [Editor trasformazione cache &#40;pagina Mapping&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Nel secondo flusso di dati, aggiungere una trasformazione Ricerca e quindi configurare la trasformazione attenendosi alla seguente procedura:  
  
    1.  Connettere la trasformazione Ricerca al flusso di dati trascinando un connettore da un'origine o una trasformazione precedente alla trasformazione Ricerca.  
  
        > [!NOTE]  
        >  Una trasformazione Ricerca può non essere convalidata se si connette a un file flat che contiene un campo di tipo data vuoto. La convalida della trasformazione varia a seconda che per la gestione connessione relativa al file flat sia configurato o meno il mantenimento dei valori Null. Per assicurarsi che la Trasformazione Ricerca venga convalidata, in **Editor origine file flat** nella **pagina Gestione connessione** selezionare l'opzione **Mantieni i valori Null dell'origine come valori Null nel flusso di dati** .  
  
    2.  Fare doppio clic sulla trasformazione di origine o precedente per configurare il componente.  
  
    3.  Fare doppio clic su Trasformazione Ricerca, quindi nella pagina **Generale** di **Editor trasformazione Ricerca** selezionare **Full cache**.  
  
    4.  Nell'area **Tipo di connessione** selezionare **Gestione connessione cache**.  
  
    5.  Nell'elenco **Specifica come gestire le righe senza voci corrispondenti** selezionare un'opzione di gestione degli errori.  
  
    6.  Nella pagina **Connessione** selezionare una gestione connessione cache nell'elenco **Gestione connessione cache** .  
  
    7.  Fare clic sulla pagina **Colonne** e trascinare almeno una colonna dall'elenco **Colonne di input disponibili** a una colonna dell'elenco **Colonne di ricerca disponibili** .  
  
        > [!NOTE]  
        >  La trasformazione Ricerca esegue automaticamente il mapping delle colonne con lo stesso nome e lo stesso tipo di dati.  
  
        > [!NOTE]  
        >  È possibile eseguire il mapping solo di colonne con tipi di dati corrispondenti. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Selezionare le colonne nell'elenco **Colonne di ricerca disponibili** . Nell'elenco **Operazione di ricerca** specificare se i valori dalle colonne di ricerca sostituiscono quelli nella colonna di input o vengono scritti in una nuova colonna.  
  
    9. Per configurare l'output degli errori, fare clic sulla pagina **Output errori** e impostare le opzioni di gestione degli errori. Per altre informazioni, vedere [Editor trasformazione Ricerca &#40;pagina Output degli errori&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Fare clic su **OK** per salvare le modifiche nella trasformazione Ricerca.  
  
9. Eseguire il pacchetto.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-two-packages-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Per implementare una trasformazione Ricerca in modalità Full Cache in due pacchetti utilizzando Gestione connessione cache e un'origine dati nel flusso di dati  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], aprire un progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e quindi aprire i due pacchetti.  
  
2.  Nella scheda Flusso di controllo presente in ogni pacchetto, aggiungere un'attività Flusso di dati.  
  
3.  Nel pacchetto padre, aggiungere una trasformazione di tipo cache al flusso di dati e quindi connettere la trasformazione a un'origine dati.  
  
     Configurare l'origine dati in base alle esigenze.  
  
4.  Fare doppio clic sulla Trasformazione cache e quindi in **Editor trasformazione cache**, nella pagina **Gestione connessione** fare clic su **Nuova** per creare una nuova gestione connessione della cache.  
  
5.  In **Editor gestione connessione della cache**, nella scheda **Generale** configurare la Gestione connessione cache impostando le opzioni seguenti:  
  
    -   Selezionare **Usa cache dei file**.  
  
    -   In **Nome file** digitare il percorso del file o fare clic su **Sfoglia** per selezionare il file.  
  
         Se viene digitato un percorso di file che non esiste, il sistema crea il file quando viene eseguito il pacchetto.  
  
    > [!NOTE]  
    >  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Fare clic sulla scheda **Colonne** e quindi specificare quali colonne sono le colonne di indice usando l'opzione **Posizione dell'indice** .  
  
     Per le colonne non dell'indice, la posizione è 0. Per le colonne di indice, la posizione di indice è un numero sequenziale e positivo.  
  
    > [!NOTE]  
    >  Quando la trasformazione Ricerca viene configurata per utilizzare una Gestione connessione cache, è possibile eseguire il mapping solo delle colonne di indice nel set di dati di riferimento alle colonne di input. Inoltre, è necessario eseguire il mapping di tutte le colonne di indice. Per altre informazioni, vedere [Editor gestione connessione cache](./cache-connection-manager.md).  
  
7.  Configurare la Trasformazione Cache in base alle esigenze. Per altre informazioni, vedere [Editor trasformazione cache &#40;pagina Gestione connessioni&#41;](../data-flow/transformations/cache-transform.md) e [Editor trasformazione cache &#40;pagina Mapping&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Eseguire una delle seguenti operazioni per popolare la Gestione connessione cache utilizzata nel secondo pacchetto:  
  
    -   Eseguire il pacchetto padre.  
  
    -   Fare doppio clic sulla Gestione connessione cache creata nel passaggio 4, fare clic su **Colonne**, selezionare le righe e quindi premere CTRL+C per copiare i metadati della colonna.  
  
9. Nel pacchetto figlio creare una Gestione connessione cache: fare clic con il pulsante destro del mouse nell'area **Gestioni connessioni** e scegliere **Nuova connessione**, selezionare **CACHE** nella finestra di dialogo **Aggiungi gestione connessione SSIS** e quindi fare clic su **Aggiungi**.  
  
     L'area **Gestioni connessioni** viene visualizzata nella parte inferiore delle schede **Flusso di controllo**, **Flusso di dati** e **Gestori eventi** di Progettazione [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
10. In **Editor gestione connessione della cache**, nella scheda **Generale** configurare la Gestione connessione cache per leggere i dati dal file di cache selezionato impostando le seguenti opzioni:  
  
    -   Selezionare **Usa cache dei file**.  
  
    -   In **Nome file** digitare il percorso del file o fare clic su **Sfoglia** per selezionare il file.  
  
    > [!NOTE]  
    >  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
11. Se i metadati della colonna sono stati copiati nel passaggio 8, fare clic su **Colonne**, selezionare la riga vuota e quindi premere CTRL+V per incollare i metadati della colonna.  
  
12. Aggiungere una trasformazione Ricerca e quindi configurare la trasformazione attenendosi alla seguente procedura:  
  
    1.  Connettere la trasformazione Ricerca al flusso di dati trascinando un connettore da un'origine o una trasformazione precedente alla trasformazione Ricerca.  
  
        > [!NOTE]  
        >  Una trasformazione Ricerca può non essere convalidata se si connette a un file flat che contiene un campo di tipo data vuoto. La convalida della trasformazione varia a seconda che per la gestione connessione relativa al file flat sia configurato o meno il mantenimento dei valori Null. Per assicurarsi che la Trasformazione Ricerca venga convalidata, in **Editor origine file flat** nella **pagina Gestione connessione** selezionare l'opzione **Mantieni i valori Null dell'origine come valori Null nel flusso di dati** .  
  
    2.  Fare doppio clic sulla trasformazione di origine o precedente per configurare il componente.  
  
    3.  Fare doppio clic sulla trasformazione Ricerca e quindi selezionare **Full Cache** nella scheda **Generale** di **Editor trasformazione ricerca**.  
  
    4.  Selezionare **Gestione connessione cache** nell'area **Tipo di connessione** .  
  
    5.  Selezionare un'opzione di gestione degli errori per le righe senza voci corrispondenti nell'elenco **Specificare come gestire le righe senza voci corrispondenti** .  
  
    6.  Nella pagina **Connessione** selezionare la gestione connessione cache aggiunta nell'elenco **Gestione connessione cache** .  
  
    7.  Fare clic sulla pagina **Colonne** e trascinare almeno una colonna dall'elenco **Colonne di input disponibili** a una colonna dell'elenco **Colonne di ricerca disponibili** .  
  
        > [!NOTE]  
        >  La trasformazione Ricerca esegue automaticamente il mapping delle colonne con lo stesso nome e lo stesso tipo di dati.  
  
        > [!NOTE]  
        >  È possibile eseguire il mapping solo di colonne con tipi di dati corrispondenti. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Selezionare le colonne nell'elenco **Colonne di ricerca disponibili** . Nell'elenco **Operazione di ricerca** specificare se i valori dalle colonne di ricerca sostituiscono quelli nella colonna di input o vengono scritti in una nuova colonna.  
  
    9. Per configurare l'output degli errori, fare clic sulla pagina **Output errori** e impostare le opzioni di gestione degli errori. Per altre informazioni, vedere [Editor trasformazione Ricerca &#40;pagina Output degli errori&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Fare clic su **OK** per salvare le modifiche nella trasformazione Ricerca.  
  
13. Aprire il pacchetto padre, aggiungere un'attività Esegui pacchetto al flusso di controllo e quindi configurare l'attività per chiamare il pacchetto figlio. Per altre informazioni, vedere [Attività Esegui pacchetto](../../integration-services/control-flow/execute-package-task.md).  
  
14. Eseguire i pacchetti.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-cache-connection-manager-and-an-existing-cache-file"></a>Per implementare una trasformazione Ricerca nella modalità Full Cache utilizzando la Gestione connessione cache e un file di cache esistente  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire un progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e quindi aprire un pacchetto.  
  
2.  Fare clic con il pulsante destro del mouse in un punto qualsiasi dell'area **Gestioni connessioni** e quindi fare clic su **Nuova connessione**.  
  
     L'area **Gestioni connessioni** viene visualizzata nella parte inferiore delle schede **Flusso di controllo**, **Flusso di dati** e **Gestori eventi** di Progettazione [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
3.  Nella finestra di dialogo **Aggiungi gestione connessione SSIS** selezionare **CACHE** e quindi fare clic su **Aggiungi** per aggiungere una Gestione connessione cache.  
  
4.  Fare doppio clic su Gestione connessione cache per aprire **Editor gestione connessione cache**.  
  
5.  In **Editor gestione connessione della cache**, nella scheda **Generale** configurare la Gestione connessione cache impostando le opzioni seguenti:  
  
    -   Selezionare **Usa cache dei file**.  
  
    -   In **Nome file** digitare il percorso del file o fare clic su **Sfoglia** per selezionare il file.  
  
    > [!NOTE]  
    >  Il livello di protezione del pacchetto non si applica al file di cache. Se il file di cache contiene informazioni riservate, utilizzare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella nella quale verrà archiviato il file. È consigliabile consentire l'accesso solo a determinati account. Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Fare clic sulla scheda **Colonne** e quindi specificare quali colonne sono le colonne di indice usando l'opzione **Posizione dell'indice** .  
  
     Per le colonne non dell'indice, la posizione è 0. Per le colonne di indice, la posizione di indice è un numero sequenziale e positivo.  
  
    > [!NOTE]  
    >  Quando la trasformazione Ricerca viene configurata per utilizzare una Gestione connessione cache, è possibile eseguire il mapping solo delle colonne di indice nel set di dati di riferimento alle colonne di input. Inoltre, è necessario eseguire il mapping di tutte le colonne di indice. Per altre informazioni, vedere [Editor gestione connessione cache](./cache-connection-manager.md).  
  
7.  Nella scheda **Flusso di controllo** aggiungere un'attività Flusso di dati al pacchetto e quindi aggiungere una trasformazione Ricerca al flusso di dati.  
  
8.  Configurare la trasformazione Ricerca attenendosi alla seguente procedura:  
  
    1.  Connettere la trasformazione Ricerca al flusso di dati trascinando un connettore da un'origine o una trasformazione precedente alla trasformazione Ricerca.  
  
        > [!NOTE]  
        >  Una trasformazione Ricerca può non essere convalidata se si connette a un file flat che contiene un campo di tipo data vuoto. La convalida della trasformazione varia a seconda che per la gestione connessione relativa al file flat sia configurato o meno il mantenimento dei valori Null. Per assicurarsi che la Trasformazione Ricerca venga convalidata, in **Editor origine file flat** nella **pagina Gestione connessione** selezionare l'opzione **Mantieni i valori Null dell'origine come valori Null nel flusso di dati** .  
  
    2.  Fare doppio clic sulla trasformazione di origine o precedente per configurare il componente.  
  
    3.  Fare doppio clic su Trasformazione Ricerca, quindi nella pagina **Generale** di **Editor trasformazione Ricerca** selezionare **Full cache**.  
  
    4.  Selezionare **Gestione connessione cache** nell'area **Tipo di connessione** .  
  
    5.  Selezionare un'opzione di gestione degli errori per le righe senza voci corrispondenti nell'elenco **Specificare come gestire le righe senza voci corrispondenti** .  
  
    6.  Nella pagina **Connessione** selezionare la gestione connessione cache aggiunta nell'elenco **Gestione connessione cache** .  
  
    7.  Fare clic sulla pagina **Colonne** e trascinare almeno una colonna dall'elenco **Colonne di input disponibili** a una colonna dell'elenco **Colonne di ricerca disponibili** .  
  
        > [!NOTE]  
        >  La trasformazione Ricerca esegue automaticamente il mapping delle colonne con lo stesso nome e lo stesso tipo di dati.  
  
        > [!NOTE]  
        >  È possibile eseguire il mapping solo di colonne con tipi di dati corrispondenti. Per altre informazioni, vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Selezionare le colonne nell'elenco **Colonne di ricerca disponibili** . Nell'elenco **Operazione di ricerca** specificare se i valori dalle colonne di ricerca sostituiscono quelli nella colonna di input o vengono scritti in una nuova colonna.  
  
    9. Per configurare l'output degli errori, fare clic sulla pagina **Output errori** e impostare le opzioni di gestione degli errori. Per altre informazioni, vedere [Editor trasformazione Ricerca &#40;pagina Output degli errori&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Fare clic su **OK** per salvare le modifiche nella trasformazione Ricerca.  
  
9. Eseguire il pacchetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare una trasformazione Ricerca in modalità Full Cache tramite la gestione connessione OLE DB](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)   
 [Implementare una ricerca in modalità No Cache o Partial Cache](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Trasformazioni di Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
