---
description: SQL Server - destinazione
title: Destinazione SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlserverdest.f1
- sql13.dts.designer.sqlserverdestadapter.connection.f1
- sql13.dts.designer.sqlserverdestadapter.mappings.f1
- sql13.dts.designer.sqlserverdestadapter.advanced.f1
helpviewer_keywords:
- SQL Server destination
- loading data
- destinations [Integration Services], SQL Server
- inserting data
- bulk load [Integration Services]
ms.assetid: a0227cd8-6944-4547-87e8-7b2507e26442
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e980e075f23d3a943870fc6dd02aa420797f8a4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596483"
---
# <a name="sql-server-destination"></a>SQL Server - destinazione

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La destinazione SQL Server si connette a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale ed esegue il caricamento bulk dei dati nelle tabelle e nelle viste di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Non è possibile usare la destinazione SQL Server nei pacchetti che accedono a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un server remoto. Per tali pacchetti, utilizzare la destinazione OLE DB. Per altre informazioni, vedere [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Gli utenti che eseguono pacchetti che includono la destinazione SQL Server devono disporre dell'autorizzazione "Creazione oggetti globali", che può essere concessa tramite lo strumento Criteri di sicurezza locali, accessibile dal menu **Strumenti di amministrazione** . Se durante l'esecuzione di un pacchetto che utilizza la destinazione SQL Server viene visualizzato un messaggio di errore, verificare che l'account utilizzato per eseguire il pacchetto disponga dell'autorizzazione "Creazione oggetti globali".  
  
## <a name="bulk-inserts"></a>Inserimenti bulk  
 Se si tenta di utilizzare la destinazione SQL Server per il caricamento bulk dei dati in un database remoto di SQL Server, potrebbe venire visualizzato un messaggio di errore simile a "È disponibile un record OLE DB. Origine: "Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client" Hresult: 0x80040E14 Descrizione: "Impossibile eseguire il caricamento bulk perché non è stato possibile aprire l'oggetto di mapping dei file SSIS 'Global\DTSQLIMPORT'. Codice di errore del sistema operativo 2 (Impossibile trovare il file specificato). Verificare che l'accesso venga effettuato a un server locale tramite la sicurezza di Windows.""  
  
 La destinazione SQL Server offre lo stesso tipo di inserimento dei dati ad alta velocità in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantito dall'attività Inserimento bulk. Se si usa la destinazione SQL Server, tuttavia, un pacchetto può applicare le trasformazioni ai dati delle colonne prima che vengano caricati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per il caricamento dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è consigliabile utilizzare la destinazione SQL Server invece della destinazione OLE DB.  
  
### <a name="bulk-insert-options"></a>Opzioni per l'inserimento bulk  
 Se la destinazione SQL Server utilizza una modalità di accesso ai dati con caricamento rapido, sarà possibile specificare le opzioni di caricamento rapido seguenti:  
  
-   È possibile mantenere i valori Identity del file di dati importato o utilizzare valori univoci assegnati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   È possibile mantenere i valori Null durante l'operazione di caricamento bulk.  
  
-   È possibile verificare i vincoli sulla tabella o vista di destinazione durante l'operazione di importazione bulk.  
  
-   È possibile acquisire un blocco a livello di tabella per la durata dell'operazione di caricamento bulk.  
  
-   È possibile eseguire trigger di inserimento definiti sulla tabella di destinazione durante l'operazione di caricamento bulk.  
  
-   È possibile specificare il numero della prima riga nell'input da caricare durante l'operazione di inserimento bulk.  
  
-   È possibile specificare il numero dell'ultima riga nell'input da caricare durante l'operazione di inserimento bulk.  
  
-   È possibile specificare il numero massimo di errori che si possono verificare prima che l'operazione di caricamento bulk venga annullata. Ogni riga che non può essere importata viene conteggiata come errore.  
  
-   È possibile specificare le colonne nell'input che contengono dati ordinati.  
  
 Per altre informazioni sulle opzioni di caricamento bulk, vedere [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
#### <a name="performance-improvements"></a>Miglioramenti delle prestazioni  
 Per migliorare le prestazioni dell'inserimento bulk e dell'accesso ai dati delle tabelle durante un'operazione di inserimento bulk, modificare le opzioni predefinite nel modo seguente:  
  
-   Non verificare i vincoli sulla tabella o vista di destinazione durante l'operazione di importazione bulk.  
  
-   Non eseguire trigger di inserimento definiti sulla tabella di destinazione durante l'operazione di caricamento bulk.  
  
-   Non applicare blocchi alla tabella, in modo che rimanga disponibile ad altri utenti e applicazioni durante l'operazione di inserimento bulk.  
  
## <a name="configuration-of-the-sql-server-destination"></a>Configurazione della destinazione SQL Server  
 È possibile configurare la destinazione SQL Server nei modi seguenti:  
  
-   Specificare la tabella o vista in cui eseguire il caricamento bulk dei dati.  
  
-   Personalizzare l'operazione di caricamento bulk specificando opzioni quale il controllo dei vincoli.  
  
-   Specificare se deve essere eseguito il commit di tutte le righe in un batch oppure impostare il numero massimo di righe di cui eseguire il commit in un batch.  
  
-   Specificare un timeout per l'operazione di caricamento bulk.  
  
 Per connettersi a un'origine dei dati questa destinazione utilizza una gestione connessione OLE DB, che specifica il provider OLE DB da utilizzare. Per altre informazioni, vedere [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fornisce inoltre l'oggetto origine dei dati da cui è possibile creare una gestione connessione OLE DB, in modo da rendere disponibili le origini dei dati e le viste origine dati alla destinazione SQL Server.  
  
 La destinazione SQL Server include un input. Non supporta un output degli errori.  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Nella finestra di dialogo **Editor avanzato** sono disponibili le proprietà che è possibile impostare a livello di codice. Per ulteriori informazioni sulle proprietà che è possibile impostare nella finestra di dialogo **Editor avanzato** o a livello di codice, fare clic su uno degli argomenti seguenti:  
  
-   [Proprietà comuni](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Proprietà personalizzate della destinazione SQL Server](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
 Per ulteriori informazioni sulle procedure per l'impostazione delle proprietà, fare clic su uno degli argomenti seguenti:  
  
-   [Caricamento bulk dei dati tramite la destinazione SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Impostazione delle proprietà di un componente del flusso di dati](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Attività correlate  
  
-   [Caricamento bulk dei dati tramite la destinazione SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Impostazione delle proprietà di un componente del flusso di dati](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Articolo tecnico relativo alla possibile [visualizzazione dell'errore "Impossibile preparare l'attività Inserimento bulk SSIS per l'inserimento dei dati" nei sistemi con Controllo dell'account utente abilitato](/troubleshoot/sql/integration-services/error-you-run-ssis-package)sul sito support.microsoft.com.  
  
-   Articolo tecnico relativo alla [guida alle prestazioni del caricamento dati](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100))sul sito msdn.microsoft.com.  
  
-   Articolo tecnico relativo all' [utilizzo di SQL Server Integration Services per il caricamento bulk dei dati](https://go.microsoft.com/fwlink/?LinkId=233701)sul sito Web simple-talk.com.  
  
## <a name="sql-destination-editor-connection-manager-page"></a>Editor destinazione SQL Server (pagina Gestione connessione)
  Usare la pagina **Gestione connessione** della finestra di dialogo **Editor destinazione SQL** per specificare le informazioni sull'origine dei dati e visualizzare un'anteprima dei risultati. La destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] carica i dati in tabelle o viste di un database di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="options"></a>Opzioni  
 **Gestione connessione OLE DB**  
 Selezionare una connessione esistente nell'elenco oppure fare clic su **Nuova** per creare una nuova connessione.  
  
 **Nuovo**  
 Consente di creare una nuova connessione usando la finestra di dialogo **Configura gestione connessione OLE DB** .  
  
 **Tabella o vista**  
 Consente di selezionare una tabella o vista esistente nell'elenco oppure di creare una nuova connessione facendo clic su **Nuova**.  
  
 **Nuovo**  
 Consente di creare una nuova tabella usando la finestra di dialogo **Crea tabella** .  
  
> [!NOTE]  
>  Quando si fa clic su **Nuova**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] genera un'istruzione CREATE TABLE predefinita basata sull'origine dati connessa. Questa istruzione CREATE TABLE predefinita non includerà l'attributo FILESTREAM anche se la tabella di origine include una colonna con l'attributo FILESTREAM dichiarato. Per eseguire un componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con l'attributo FILESTREAM, implementare innanzitutto l'archiviazione di FILESTREAM nel database di destinazione. Aggiungere quindi l'attributo FILESTREAM all'istruzione CREATE TABLE nella finestra di dialogo **Crea tabella** . Per altre informazioni, vedere [Dati BLOB &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Anteprima**  
 Consente di visualizzare in anteprima i risultati nella finestra di dialogo **Anteprima risultati query** . L'anteprima supporta la visualizzazione di un massimo di 200 righe.  
  
## <a name="sql-destination-editor-mappings-page"></a>Editor destinazione SQL Server (pagina Mapping)
  Utilizzare la pagina **Mapping** della finestra di dialogo **Editor destinazione SQL** per eseguire il mapping tra colonne di input e colonne di destinazione.  
  
### <a name="options"></a>Opzioni  
 **Colonne di input disponibili**  
 Consente di visualizzare l'elenco delle colonne di input disponibili. Eseguire un'operazione di trascinamento della selezione per impostare il mapping tra le colonne di input disponibili nella tabella e le colonne di destinazione.  
  
 **Colonne di destinazione disponibili**  
 Consente di visualizzare l'elenco delle colonne di destinazione disponibili. Eseguire un'operazione di trascinamento della selezione per impostare il mapping tra le colonne di destinazione disponibili nella tabella e le colonne di input.  
  
 **Colonna di input**  
 Consente di visualizzare le colonne di input selezionate nella tabella precedente. È possibile modificare i mapping utilizzando l'elenco **Colonne di input disponibili**.  
  
 **Colonna di destinazione**  
 Consente di visualizzare tutte le colonne di destinazione disponibili, indipendentemente dal fatto che siano mappate o meno.  
  
## <a name="sql-destination-editor-advanced-page"></a>Editor destinazione SQL Server (pagina Avanzate)
  Usare la pagina **Avanzate** della finestra di dialogo **Editor destinazione SQL** per specificare le opzioni di inserimento bulk avanzate.  
  
### <a name="options"></a>Opzioni  
 **Mantieni valori Identity**  
 Consente di specificare se l'attività deve inserire valori nelle colonne Identity. Il valore predefinito di questa proprietà è **False**.  
  
 **Mantieni valori Null**  
 Consente di specificare se l'attività deve mantenere i valori Null. Il valore predefinito di questa proprietà è **False**.  
  
 **Blocco a livello di tabella**  
 Consente di specificare se la tabella viene bloccata durante il caricamento dei dati. Il valore predefinito di questa proprietà è **True**.  
  
 **Vincoli CHECK**  
 Consente di specificare se l'attività deve verificare i vincoli. Il valore predefinito di questa proprietà è **True**.  
  
 **Attive trigger**  
 Consente di specificare se l'inserimento bulk deve attivare i trigger nelle tabelle. Il valore predefinito di questa proprietà è **False**.  
  
 **Prima riga**  
 Consente di specificare la prima riga da inserire. Il valore predefinito della proprietà è **-1**, a indicare che non è stato assegnato alcun valore.  
  
> [!NOTE]  
>  Deselezionare la casella in **Editor destinazione SQL** per indicare che non si vuole assegnare alcun valore alla proprietà. Usare -1 nella finestra **Proprietà** , in **Editor avanzato** e nel modello a oggetti.  
  
 **Ultima riga**  
 Consente di specificare l'ultima riga da inserire. Il valore predefinito della proprietà è **-1**, a indicare che non è stato assegnato alcun valore.  
  
> [!NOTE]  
>  Deselezionare la casella in **Editor destinazione SQL** per indicare che non si vuole assegnare alcun valore alla proprietà. Usare -1 nella finestra **Proprietà** , in **Editor avanzato** e nel modello a oggetti.  
  
 **Numero massimo di errori**  
 Consente di specificare il numero massimo di errori che possono verificarsi prima dell'arresto dell'inserimento bulk. Il valore predefinito della proprietà è **-1**, a indicare che non è stato assegnato alcun valore.  
  
> [!NOTE]  
>  Deselezionare la casella in **Editor destinazione SQL** per indicare che non si vuole assegnare alcun valore alla proprietà. Usare -1 nella finestra **Proprietà** , in **Editor avanzato** e nel modello a oggetti.  
  
 **Timeout**  
 Consente di specificare il numero di secondi di attesa prima che l'inserimento bulk venga arrestato a causa di un timeout.  
  
 **Colonne di ordinamento**  
 Consente di digitare i nomi delle colonne di ordinamento. È possibile ordinare ogni colonna in ordine crescente o decrescente. Se si utilizzando più colonne di ordinamento, delimitare l'elenco con virgole.  
  
## <a name="see-also"></a>Vedere anche  
 [Flusso di dati](../../integration-services/data-flow/data-flow.md)  
