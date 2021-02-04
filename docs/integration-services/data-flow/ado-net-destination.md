---
description: Destinazione ADO NET
title: Destinazione ADO NET | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetdest.f1
- sql13.dts.designer.adonetdest.connection.f1
- sql13.dts.designer.adonetdest.mappings.f1
- sql13.dts.designer.adonetdest.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], ADO.NET
- ADO.NET destination
ms.assetid: cb883990-d875-4d8b-b868-45f9f15ebeae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4ae905d868554c20948af3eb0191937382eff56f
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536377"
---
# <a name="ado-net-destination"></a>Destinazione ADO NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La destinazione ADO NET consente di caricare i dati in un'ampia gamma di database compatibili con [!INCLUDE[vstecado](../../includes/vstecado-md.md)]che utilizzano una tabella o una vista di database. È disponibile l'opzione per caricare i dati in una tabella o in una vista esistenti o è possibile creare una nuova tabella e caricare i dati in essa.  
  
 È possibile usare la destinazione ADO NET per connettersi a [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. La connessione a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] tramite OLE DB non è supportata. Per altre informazioni su [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vedere la pagina relativa alle [linee guida e limitazioni generali (database SQL di Azure)](/previous-versions/azure/ee336245(v=azure.100)).  
  
## <a name="troubleshooting-the-ado-net-destination"></a>Risoluzione dei problemi relativi alla destinazione ADO NET  
 È possibile registrare le chiamate eseguite dalla destinazione ADO NET a provider di dati esterni. Questa nuova funzionalità di registrazione può essere utilizzata per risolvere i problemi relativi al salvataggio di dati in origini di dati esterne da parte della destinazione ADO NET. Per registrare le chiamate eseguite dalla destinazione ADO NET a provider di dati esterni, abilitare la registrazione dei pacchetti e selezionare l'evento **Diagnostica** a livello di pacchetto. Per altre informazioni, vedere [Risoluzione dei problemi relativi agli strumenti per l'esecuzione del pacchetto](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-ado-net-destination"></a>Configurazione della destinazione ADO NET  
 Per connettersi a un'origine dati, questa destinazione utilizza una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] che specifica il provider [!INCLUDE[vstecado](../../includes/vstecado-md.md)] da utilizzare. Per altre informazioni, vedere [Gestione connessione ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
 Una destinazione ADO NET include i mapping tra le colonne di input e quelle nell'origine dei dati della destinazione. Non è necessario eseguire il mapping delle colonne di input su tutte le colonne di destinazione. Tuttavia, le proprietà di alcune colonne di destinazione possono richiedere il mapping delle colonne di input. In caso contrario, potrebbero verificarsi degli errori. Se ad esempio una colonna di destinazione non consente valori Null, sarà necessario eseguire il mapping di una colonna di input sulla colonna di destinazione. È inoltre necessario che i tipi di dati delle colonne di cui è stato eseguito il mapping siano compatibili. Ad esempio, non è possibile eseguire il mapping di una colonna di input con un tipo di dati stringa su una colonna di destinazione con un tipo di dati numerici se il provider [!INCLUDE[vstecado](../../includes/vstecado-md.md)] non supporta questo mapping.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta l'inserimento di testo nelle colonne il cui tipo di dati è impostato su immagine. Per altre informazioni sui tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
> [!NOTE]  
>  La destinazione ADO NET non supporta l'esecuzione del mapping di una colonna di input il cui tipo è impostato su DT_DBTIME su una colonna del database il cui tipo è impostato su datetime. Per altre informazioni su tutti i tipi di dati [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vedere [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 La destinazione ADO NET include un input regolare e un output degli errori.  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Nella finestra di dialogo **Editor avanzato** sono disponibili le proprietà che è possibile impostare a livello di codice. Per ulteriori informazioni sulle proprietà che è possibile impostare nella finestra di dialogo **Editor avanzato** o a livello di codice, fare clic su uno degli argomenti seguenti:  
  
-   [Proprietà comuni](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Proprietà personalizzate ADO NET](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
 Per altre informazioni su come impostare le proprietà, vedere [Impostazione delle proprietà di un componente del flusso di dati](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="ado-net-destination-editor-connection-manager-page"></a>Editor destinazione ADO NET (pagina Gestione connessione)
  Usare la pagina **Gestione connessione** della finestra di dialogo **Editor destinazione ADO NET** per selezionare la connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] per la destinazione. Tramite questa pagina è inoltre possibile selezionare una tabella o una vista del database.  
  
 **Per aprire la pagina Gestione connessione**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], aprire il pacchetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con la destinazione ADO NET.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sulla destinazione ADO NET.  
  
3.  In **Editor destinazione ADO NET** fare clic su **Gestione connessione**.  
  
### <a name="static-options"></a>Opzioni statiche  
 **Connection manager**  
 Selezionare una gestione connessione esistente nell'elenco o crearne una nuova facendo clic su **Nuova**.  
  
 **Nuovo**  
 Consente di creare una nuova gestione connessione usando la finestra di dialogo **Configura gestione connessione ADO.NET** .  
  
 **Tabella o vista**  
 Consente di selezionare una tabella o vista esistente nell'elenco oppure di creare una nuova tabella facendo clic su **Nuova**.  
  
 **Nuovo**  
 Consente di creare una nuova tabella o vista usando la finestra di dialogo **Crea tabella** .  
  
> [!NOTE]  
>  Quando si fa clic su **Nuova**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] genera un'istruzione CREATE TABLE predefinita basata sull'origine dati connessa. Questa istruzione CREATE TABLE predefinita non includerà l'attributo FILESTREAM anche se la tabella di origine include una colonna con l'attributo FILESTREAM dichiarato. Per eseguire un componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con l'attributo FILESTREAM, implementare innanzitutto l'archiviazione di FILESTREAM nel database di destinazione. Aggiungere quindi l'attributo FILESTREAM all'istruzione CREATE TABLE nella finestra di dialogo **Crea tabella** . Per altre informazioni, vedere [Dati BLOB &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Anteprima**  
 Consente di visualizzare in anteprima i risultati nella finestra di dialogo **Anteprima risultati query** . L'anteprima supporta la visualizzazione di un massimo di 200 righe.  
  
 **Utilizza inserimento bulk, se disponibile**  
 Specificare se usare l'interfaccia <xref:System.Data.SqlClient.SqlBulkCopy> per migliorare le prestazioni delle operazioni di inserimento bulk.  
  
 Solo i provider ADO.NET che restituiscono un oggetto <xref:System.Data.SqlClient.SqlConnection> supportano l'uso dell'interfaccia <xref:System.Data.SqlClient.SqlBulkCopy> . Il provider di dati .NET per SQL Server (SqlClient) restituisce un oggetto <xref:System.Data.SqlClient.SqlConnection> e un provider personalizzato può restituire un oggetto <xref:System.Data.SqlClient.SqlConnection> .  
  
 È possibile usare il provider di dati .NET per SQL Server (SqlClient) per connettersi a [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 Se si seleziona **Usa Inserimento bulk quando possibile** e si imposta l'opzione **Errore** su **Reindirizza riga**, il batch di dati che la destinazione reindirizza all'output degli errori può includere righe corrette. Per altre informazioni sulla gestione degli errori nelle operazioni bulk, vedere [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md). Per altre informazioni sull'opzione **Errore** , vedere [Editor destinazione ADO NET &#40;pagina Output degli errori&#41;]().  
  
> [!NOTE]
>  Se una tabella di origine SQL Server o Sybase include una colonna Identity, è necessario usare Esegui attività di SQL per abilitare IDENTITY_INSERT prima della destinazione ADO NET e per disabilitarla di nuovo in seguito. La proprietà della colonna Identity specifica un valore incrementale per la colonna. L'istruzione SET IDENTITY_INSERT consente l'inserimento di valori espliciti della tabella di origine nella colonna Identity della tabella di destinazione.  
> 
>   Per eseguire correttamente le istruzioni SET IDENTITY_INSERT e il caricamento dei dati, è necessario eseguire le operazioni seguenti.  
>       1. Usare la stessa gestione connessione ADO.NET per le attività Esegui SQL e la destinazione ADO.NET.  
>       2. Nella gestione connessione impostare le proprietà **RetainSameConnection** e **MultipleActiveResultSets** su True.  
>       3. Nella destinazione ADO.NET impostare la proprietà **UseBulkInsertWhenPossible** su False.   
> 
>  Per altre informazioni, vedere [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md) e [IDENTITY &#40;proprietà&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
## <a name="external-resources"></a>Risorse esterne  
 Articolo tecnico relativo alla [modalità rapida di caricamento di dati nel database SQL di Azure](https://techcommunity.microsoft.com/t5/datacat/loading-data-to-sql-azure-the-fast-way/ba-p/305066) nel sito Web sqlcat.com  
  
## <a name="ado-net-destination-editor-mappings-page"></a>Editor destinazione ADO NET (pagina Mapping)
  Utilizzare la pagina **Mapping** della finestra di dialogo **Editor destinazione ADO NET** per eseguire il mapping tra colonne di input e colonne di destinazione.  
  
 **Per aprire la pagina Mapping**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], aprire il pacchetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con la destinazione ADO NET.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sulla destinazione ADO NET.  
  
3.  In **Editor destinazione ADO NET**, fare clic su **Mapping**.  
  
### <a name="options"></a>Opzioni  
 **Colonne di input disponibili**  
 Consente di visualizzare l'elenco delle colonne di input disponibili. Eseguire un'operazione di trascinamento della selezione per impostare il mapping tra le colonne di input disponibili nella tabella e le colonne di destinazione.  
  
 **Colonne di destinazione disponibili**  
 Consente di visualizzare l'elenco delle colonne di destinazione disponibili. Eseguire un'operazione di trascinamento della selezione per impostare il mapping tra le colonne di destinazione disponibili nella tabella e le colonne di input.  
  
 **Colonna di input**  
 Consente di visualizzare le colonne di input selezionate dall'utente. È possibile rimuovere i mapping selezionando **\<ignore>** per escludere colonne dall'output.  
  
 **Colonna di destinazione**  
 Consente di visualizzare ogni colonna di destinazione disponibile, indipendentemente dal fatto che sia mappata o meno.  
  
## <a name="ado-net-destination-editor-error-output-page"></a>Editor destinazione ADO NET (pagina Output errori)
  Usare la pagina **Output errori** della finestra di dialogo **Editor destinazione ADO NET** per specificare le opzioni di gestione degli errori.  
  
 **Per aprire la pagina Output errori**  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], aprire il pacchetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con la destinazione ADO NET.  
  
2.  Nella scheda **Flusso di dati** fare doppio clic sulla destinazione ADO NET.  
  
3.  In **Editor destinazione ADO NET** fare clic su **Output errori**.  
  
### <a name="options"></a>Opzioni  
 **Input o output**  
 Consente di visualizzare il nome dell'input.  
  
 **Colonna**  
 Non usato.  
  
 **Error (Errore) (Error (Errore)e)**  
 Consente di specificare l'azione da eseguire in caso di errori, ovvero ignorare l'errore, reindirizzare la riga o interrompere il componente.  
  
 **Argomenti correlati:** [Gestione degli errori nei dati](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncamento**  
 Non usato.  
  
 **Descrizione**  
 Consente di visualizzare la descrizione dell'operazione.  
  
 **Imposta questo valore nelle celle selezionate**  
 Consente di specificare l'azione che dovrà interessare tutte le celle selezionate in caso di errore o troncamento: ignorare l'errore, reindirizzare la riga o interrompere il componente.  
  
 **Applica**  
 Consente di applicare l'opzione di gestione degli errori alle celle selezionate.  
  
