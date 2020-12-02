---
description: Applicazione delle modifiche alla destinazione
title: Applicare le modifiche alla destinazione | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],applying changes
ms.assetid: 338a56db-cb14-4784-a692-468eabd30f41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0176dcc68e8c7f72c4bd9b78b1f3d926e52ad215
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123742"
---
# <a name="apply-the-changes-to-the-destination"></a>Applicazione delle modifiche alla destinazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Nel flusso di dati di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] che esegue un caricamento incrementale dei dati delle modifiche la terza e ultima attività consistono nell'applicare le modifiche alla destinazione. Sarà necessario un componente per applicare gli inserimenti, uno per applicare gli aggiornamenti e uno per applicare le eliminazioni.  
  
> [!NOTE]  
>  La seconda attività nel progettare il flusso di dati di un pacchetto che esegue un caricamento incrementale dei dati delle modifiche consiste nel separare inserimenti, aggiornamenti ed eliminazioni. Per altre informazioni su questo componente, vedere [Elaborare inserimenti, aggiornamenti ed eliminazioni](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md). Per una descrizione del processo complessivo di creazione di un pacchetto in cui viene eseguito un caricamento incrementale dei dati delle modifiche, vedere [Change Data Capture &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="applying-inserts"></a>Applicazione di inserimenti  
 Per applicare inserimenti, è necessario utilizzare una destinazione OLE DB in quanto le nuove righe non richiedono alcuna gestione speciale.  
  
#### <a name="to-process-inserts-by-using-an-ole-db-destination"></a>Per elaborare gli inserimenti utilizzando una destinazione OLE DB  
  
1.  Nella scheda **Flusso di dati** aggiungere una destinazione OLE DB.  
  
2.  Connettere l'output contenente gli inserimenti dalla trasformazione Suddivisione condizionale alla destinazione OLE DB.  
  
3.  Nella pagina **Gestione connessione** in **Editor destinazione OLE DB** selezionare le opzioni seguenti:  
  
    1.  Selezionare o creare una gestione connessione OLE DB per il database di destinazione.  
  
    2.  Selezionare un'opzione per **Modalità di accesso ai dati** e quindi selezionare la tabella di destinazione o immettere un'istruzione SQL contenente le colonne di destinazione.  
  
4.  Nella pagina **Mapping** dell'editor eseguire il mapping tra le colonne appropriate dei dati delle modifiche e la tabella di destinazione.  
  
## <a name="applying-updates"></a>Applicazione di aggiornamenti  
 Per applicare aggiornamenti, è necessario utilizzare una trasformazione Comando OLE DB. Viene utilizzata questa trasformazione in quanto è necessario utilizzare un'istruzione UPDATE con parametri per aggiornare una riga per volta con i nuovi valori di colonna.  
  
> [!NOTE]  
>  Per applicare gli aggiornamenti, è inoltre possibile utilizzare componenti di destinazione. Quando si sceglie questo approccio, è necessario utilizzare i componenti di destinazione per salvare le righe in tabelle temporanee create allo scopo. È quindi necessario utilizzare attività Esegui SQL per eseguire operazioni di aggiornamento bulk e di eliminazione bulk sulla destinazione dalle tabelle temporanee.  
  
#### <a name="to-process-updates-by-using-an-ole-db-command-transformation"></a>Per elaborare gli aggiornamenti utilizzando una trasformazione Comando OLE DB  
  
1.  Nella scheda **Flusso di dati** aggiungere una trasformazione Comando OLE DB.  
  
2.  Connettere l'output contenente gli aggiornamenti dalla trasformazione Suddivisione condizionale alla trasformazione Comando OLE DB.  
  
3.  Nella scheda **Gestione connessione** in **Editor avanzato per Comando OLE DB** selezionare o creare una gestione connessione OLE DB per il database di destinazione.  
  
4.  Nella scheda **Proprietà componente** in **Editor avanzato per Comando OLE DB** immettere un'istruzione UPDATE con parametri per **SqlCommand**.  
  
     Un'istruzione UPDATE per una tabella Customer, ad esempio, potrebbe avere la sintassi seguente:  
  
    ```sql
    update CDCSample.Customer  
    set TerritoryID  = ?,  
        CustomerType  = ?,  
        rowguid  = ?,  
        ModifiedDate  = ?  
    where CustomerID = ?  
  
    ```  
  
5.  Nella scheda **Mapping colonne** dell'editor eseguire il mapping tra le colonne appropriate dei dati delle modifiche e i parametri nell'istruzione UPDATE.  
  
## <a name="applying-deletes"></a>Applicazione di eliminazioni  
 Per applicare eliminazioni, è necessario utilizzare una trasformazione Comando OLE DB. Si utilizza questa trasformazione in quanto è necessario utilizzare un'istruzione DELETE con parametri che elimina una sola riga per volta in base al valore di colonna che identifica in modo univoco la riga.  
  
> [!NOTE]  
>  Per applicare le eliminazioni, è inoltre possibile utilizzare componenti di destinazione. Quando si sceglie questo approccio, è necessario utilizzare i componenti di destinazione per salvare le righe in tabelle temporanee create allo scopo. È quindi necessario utilizzare attività Esegui SQL per eseguire operazioni di aggiornamento bulk e di eliminazione bulk sulla destinazione dalle tabelle temporanee.  
  
#### <a name="to-process-deletes-by-using-an-ole-db-command-transformation"></a>Per elaborare le eliminazioni utilizzando una trasformazione Comando OLE DB  
  
1.  Nella scheda **Flusso di dati** aggiungere una trasformazione Comando OLE DB al flusso di dati.  
  
2.  Connettere l'output contenente le eliminazioni dalla trasformazione Suddivisione condizionale alla trasformazione Comando OLE DB.  
  
3.  Aprire l'editor avanzato per configurare la trasformazione.  
  
4.  Nella scheda **Gestione connessione** in **Editor avanzato per Comando OLE DB** selezionare o creare una gestione connessione OLE DB per il database di destinazione.  
  
5.  Nella scheda **Proprietà componente** in **Editor avanzato per Comando OLE DB** immettere un'istruzione DELETE con parametri per **SqlCommand**.  
  
     Un'istruzione DELETE per una tabella Customer, ad esempio, potrebbe avere la sintassi seguente:  
  
    ```sql
    delete from Customer where CustomerID = ?  
  
    ```  
  
6.  Nella scheda **Mapping colonne** dell'editor eseguire il mapping tra la colonna appropriata dei dati delle modifiche e il parametro nell'istruzione DELETE.  
  
## <a name="optimizing-inserts-and-updates-by-using-merge-functionality"></a>Ottimizzazione di inserimenti e aggiornamenti tramite la funzionalità MERGE  
 È possibile ottimizzare l'elaborazione di inserimenti e aggiornamenti combinando alcune opzioni di Change Data Capture con l'utilizzo della parola chiave MERGE di Transact-SQL. Per ulteriori informazioni sulla parola chiave MERGE, vedere [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md).  
  
 Nell'istruzione Transact-SQL che recupera i dati delle modifiche, è possibile specificare *all with merge* come valore del parametro *row_filter_option* quando si chiama la funzione **cdc.fn_cdc_get_net_changes_<capture_instance>**. La funzione Change Data Capture ha maggiore efficacia quando non deve eseguire l'elaborazione aggiuntiva necessaria per distinguere gli inserimenti dagli aggiornamenti. Quando si specifica il valore di parametro *all with merge* , il valore **__$operation** dei dati delle modifiche è 1 per le eliminazioni e 5 per le modifiche prodotte da inserimenti o aggiornamenti. Per ulteriori informazioni sulla funzione Transact-SQL usata per recuperare i dati delle modifiche, vedere [Recuperare e comprendere i dati delle modifiche](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md). Dopo avere recuperato le modifiche con il valore di parametro *all with merge*, è possibile applicare le eliminazioni ed eseguire l'output delle righe rimanenti in una tabella temporanea o una tabella di staging. In un'attività Esegui SQL a valle è quindi possibile utilizzare una singola istruzione MERGE per applicare tutti gli inserimenti o gli aggiornamenti dalla tabella di staging alla destinazione.  
  
  
