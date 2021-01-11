---
title: Tipo di connessione Azure SQL | Microsoft Docs
description: L'estensione per i dati della connessione Azure SQL supporta parametri multivalore, aggregazioni server e credenziali gestiti separatamente dalla stringa di connessione.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.date: 02/15/2019
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 4dc1fcf4aa738a018274f65d7d707f6461b9c45c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878844"
---
# <a name="azure-sql-connection-type-ssrs"></a>Tipo di connessione Azure SQL (SSRS)

[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] è un database relazionale ospitato e basato sul cloud, creato con le tecnologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per includere dati da un'origine dati esterna [!INCLUDE[ssSDS](../../includes/sssds-md.md)] nel report, è necessario disporre di un set di dati basato su un'origine dati del report di tipo [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Questo tipo di origine dati predefinito è basato sull'estensione per i dati di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Questo tipo di origine dati può essere utilizzato per connettersi e recuperare dati da [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
Questa estensione per i dati supporta parametri multivalore, aggregazioni server e credenziali gestiti separatamente dalla stringa di connessione.  
  
[!INCLUDE[ssSDS](../../includes/sssds-md.md)] è simile a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale e, a parte alcune eccezioni, l'acquisizione di dati da [!INCLUDE[ssSDS](../../includes/sssds-md.md)] è identica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Quando si apre una connessione a [!INCLUDE[ssSDS](../../includes/sssds-md.md)], impostare il timeout connessione su 30 secondi.
  
Per altre informazioni, vedere [Documentazione sul database SQL di Azure](/azure/sql-database/).  
  
Usare le informazioni presenti in questo argomento per compilare un'origine dati. Per istruzioni dettagliate, vedere [Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="connection-string"></a><a name="Connection"></a> Stringa di connessione

Quando ci si connette a [!INCLUDE[ssSDS](../../includes/sssds-md.md)], si esegue la connessione a un oggetto di database nel cloud. Proprio come i database in loco, il database di hosting potrebbe disporre di più schemi aventi più tabelle, viste e stored procedure. L'oggetto di database viene specificato in Progettazione query. Se non si specifica un database nella stringa di connessione, la connessione viene eseguita al database predefinito assegnato dall'amministratore.  
  
Contattare l'amministratore del database per ottenere le informazioni di connessione e le credenziali da utilizzare per connettersi all'origine dati. Nella seguente stringa di connessione di esempio viene specificato un database di hosting di esempio denominato AdventureWorks.  
  
```
Data Source=<host>;Initial Catalog=AdventureWorks; Encrypt=True;  
```
  
Viene inoltre usata la finestra di dialogo **Proprietà origini dati** per fornire credenziali quale il nome utente e la password. Le opzioni `User Id` e `Password` vengono aggiunte automaticamente alla stringa di connessione; non è necessario digitarle come parte della stringa di connessione.  
  
Per altre informazioni ed esempi di stringhe di connessione, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
## <a name="credentials"></a><a name="Credentials"></a> Credenziali

L'autenticazione di Windows (sicurezza integrata) non è supportata. Se si tenta di connettersi a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] utilizzando l'autenticazione di Windows, si verificherà un errore. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] supporta solo l'autenticazione di SQL Server (nome utente e password) e gli utenti devono fornire le proprie credenziali (account di accesso e password) ogni volta che si connettono a [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
Le credenziali devono essere sufficienti per accedere al database. A seconda della query, potrebbe essere necessario utilizzare altre autorizzazioni, ad esempio le autorizzazioni per eseguire stored procedure e accedere a tabelle e viste. Il proprietario dell'origine dati esterna deve configurare le credenziali sufficienti a fornire l'accesso in sola lettura agli oggetti di database di cui si necessita.  
  
Da un client di creazione di report sono disponibili le opzioni seguenti per la specifica delle credenziali:  
  
- Utilizzare un nome utente e una password archiviati. Per negoziare il doppio hop che si verifica quando il database contenente i dati del report è diverso dal server di report, selezionare le opzioni per utilizzare le credenziali come credenziali di Windows. È inoltre possibile scegliere di rappresentare l'utente autenticato dopo essersi connessi all'origine dati.  
  
- Non sono necessarie credenziali. Per utilizzare questa opzione, è necessario aver configurato l'account di esecuzione automatica sul server di report. Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
Per altre informazioni, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) o [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](specify-credential-and-connection-information-for-report-data-sources.md).  
  
## <a name="queries"></a><a name="Query"></a> Query

Una query consente di specificare quali dati recuperare per un set di dati del report. Le colonne nel set di risultati per una query popolano la raccolta dei campi per un set di dati. Se la query restituisce più set di risultati, nel report viene elaborato solo il primo set di risultati. Anche se esistono alcune differenze tra i database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i [!INCLUDE[ssSDS](../../includes/sssds-md.md)], quali ad esempio le dimensioni di database supportate, scrivere query per [!INCLUDE[ssSDS](../../includes/sssds-md.md)]è simile allo scrivere query per database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Alcune istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio BACKUP, non sono supportate in [!INCLUDE[ssSDS](../../includes/sssds-md.md)], ma non sono quelle che si utilizzano nelle query del report. Per altre informazioni, vedere [Tipo di connessione SQL Server &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
Per impostazione predefinita, se si crea una nuova query o si apre una query esistente che può essere rappresentata nella finestra Progettazione query con interfaccia grafica, è disponibile la progettazione query relazionale. È possibile specificare una query nei modi seguenti:  
  
- Compilare una query in modo interattivo. Usare la finestra Progettazione query relazionale in cui è visualizzata una vista gerarchica di tabelle, viste, stored procedure e altri elementi del database organizzati in base allo schema del database. Selezionare le colonne dalle tabelle o dalle viste o specificare le stored procedure o le funzioni con valori di tabella. Limitare il numero di righe di dati da recuperare specificando i criteri di filtro. Personalizzare il filtro quando il report viene eseguito impostando l'opzione del parametro.  
  
- Digitare o incollare una query. Usare questa finestra per immettere direttamente testo [!INCLUDE[tsql](../../includes/tsql-md.md)], per incollare testo di query da un'altra origine, per immettere query complesse che non è possibile compilare usando la progettazione query relazionale o per immettere espressioni basate su query.  
  
- Consente di importare una query esistente da un file o un report. Utilizzare il pulsante **Importa** da una finestra Progettazione query per individuare un file con estensione sql o rdl e importare una query.  
  
La finestra Progettazione query basata su testo supporta le due modalità seguenti:  
  
- [Testo](#QueryText) Digitare comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] che selezionano dati dall'origine dati.  
  
- [Stored procedure](#QueryStoredProcedure) Scegliere da un elenco di stored procedure.  
  
Per altre informazioni, vedere [Interfaccia utente di Progettazione query relazionale &#40;Generatore report&#41;](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) e [Interfaccia utente di Progettazione query basata su testo &#40;Generatore report&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
La finestra Progettazione query con interfaccia grafica usata da [!INCLUDE[ssSDS](../../includes/sssds-md.md)] offre il supporto predefinito per il raggruppamento e le aggregazioni per semplificare la scrittura di query che recuperano solo dati di riepilogo. Le funzionalità relative al linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] sono: la clausola GROUP BY, parola chiave DISTINCT e gli aggregati quali SUM e COUNT. La finestra Progettazione query basata su testo offre il supporto completo per il linguaggio [!INCLUDE[tsql](../../includes/tsql-md.md)] , inclusi il raggruppamento e le aggregazioni. Per altre informazioni su [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Guida di riferimento a Transact-SQL &#40;Motore di database&#41;](../../t-sql/language-reference.md).  
  
### <a name="using-query-type-text"></a><a name="QueryText"></a> Utilizzo di query di tipo Text

Nella finestra Progettazione query basata su testo, è possibile digitare i comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] per definire i dati in un set di dati. La query [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente, ad esempio, seleziona i nomi di tutti i dipendenti con mansioni di assistente marketing:

```sql
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```

Fare clic sul pulsante **Esegui** ( **!** ) sulla barra degli strumenti per eseguire la query e visualizzare il set di risultati.  
  
Per parametrizzare questa query, aggiungere un parametro di query. Modificare ad esempio la clausola WHERE nella stringa seguente:  

```sql
WHERE HumanResources.Employee.JobTitle = (@JobTitle)  
```

Quando si esegue la query, i parametri del report corrispondenti ai parametri di query verranno creati automaticamente. Per ulteriori informazioni, vedere [Parametri di query](#Parameters) di seguito in questo argomento.  
  
### <a name="using-query-type-storedprocedure"></a><a name="QueryStoredProcedure"></a> Utilizzo di query di tipo StoredProcedure

È possibile specificare una stored procedure per una query del set di dati in uno dei seguenti modi:  
  
- Nella finestra di dialogo **Proprietà set di dati** , impostare l'opzione **Stored procedure** . Scegliere dall'elenco a discesa di stored procedure e funzioni con valori di tabella.  
  
- Nel riquadro Vista di database della finestra Progettazione query relazionale, selezionare una stored procedure o una funzione con valori di tabella.  
  
- Nella finestra Progettazione query basata su testo, selezionare **StoredProcedure** sulla barra degli strumenti.  
  
Dopo aver selezionato una stored procedure o una funzione con valori di tabella, è possibile eseguire la query. Verranno richiesti i valori dei parametri di input. Quando si esegue la query, i parametri del report corrispondenti ai parametri di input verranno creati automaticamente. Per ulteriori informazioni, vedere [Parametri di query](#Parameters) di seguito in questo argomento.  
  
È supportato solo il primo set di risultati recuperato per una stored procedure. Se una stored procedure restituisce più set di risultati, viene utilizzato solo il primo.  
  
Se in una stored procedure è presente un parametro con un valore predefinito, è possibile accedere a tale valore utilizzando la parola chiave DEFAULT come valore per il parametro. Se il parametro di query è collegato a un parametro di report, l'utente può digitare o selezionare la parola DEFAULT nella casella di input del parametro di report.  
  
Per altre informazioni sulle stored procedure, vedere [Stored procedure (Motore di database)](../../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
## <a name="parameters"></a><a name="Parameters"></a> Parametri

Quando il testo della query contiene variabili di query o stored procedure con parametri di input, vengono generati automaticamente i parametri di query corrispondenti per il set di dati e i parametri di report per il report. Il testo della query non deve includere l'istruzione DECLARE per ogni variabile della query.  
  
 La query SQL seguente, ad esempio, crea un parametro di report denominato **EmpID**:  

```sql
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```

Per impostazione predefinita, ogni parametro del report dispone di tipo di dati Text e un set di dati creato automaticamente per fornire un elenco a discesa di valori disponibili. Dopo aver creato i parametri di report, potrebbe essere necessario modificare i valori predefiniti. Per ulteriori informazioni, vedere la pagina relativa al [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  

## <a name="remarks"></a><a name="Remarks"></a> Osservazioni
  
###### <a name="alternate-data-extensions"></a>Estensioni per i dati alternative

È possibile recuperare i dati anche da un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un tipo di origine dati ODBC. La connessione a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] tramite OLE DB non è supportata.  
  
Per altre informazioni, vedere [Tipo di connessione ODBC &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
###### <a name="platform-and-version-information"></a>Informazioni sulla piattaforma e sulla versione

Per altre informazioni sulle piattaforme e le versioni supportate, vedere [Origini dati supportate da Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  

::: moniker range=">=sql-server-2016"

## <a name="azure-sql-database-and-aad"></a>Database SQL di Azure e AAD

È possibile usare il database SQL di Azure con Azure Active Directory (AAD).

Questo scenario è supportato quando vengono configurati correttamente gli elementi seguenti:

- [Active Directory Authentication Library per SQL Server (ADALSQL)](https://www.microsoft.com/download/details.aspx?id=48742) è installato nel server di report.
- [Active Directory Federation Services (AD FS)](/windows-server/identity/active-directory-federation-services) è configurato per la federazione tra Active Directory (AD) locale e AAD.
- [La delega vincolata Kerberos (KDC)](/windows-server/security/kerberos/kerberos-constrained-delegation-overview) è configurata dal server di report al server AD FS.
- Configurare il report/l'origine dati per eseguire l'autenticazione al [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) come l'utente che visualizza il report.

::: moniker-end

## <a name="how-to-topics"></a><a name="HowTo"></a> Procedure

In questa sezione sono contenute istruzioni dettagliate per l'utilizzo di connessioni dati, origini dati e set di dati.  
  
[Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
[Creare un set di dati condiviso o un set di dati incorporato &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
[Aggiungere un filtro a un set di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
## <a name="related-sections"></a><a name="Related"></a> Sezioni correlate

In queste sezioni della documentazione sono incluse informazioni concettuali approfondite sui dati dei report, nonché le procedure per definire, personalizzare e utilizzare parti di un report correlate ai dati.  
  
[Set di dati del report &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
Viene fornita una panoramica sull'accesso ai dati del report.  
  
[Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
Vengono fornite informazioni sulle connessioni dati e sulle origini dati.  
  
[Set di dati condivisi e incorporati del report &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
Vengono fornite informazioni sui set di dati incorporati e condivisi.  
  
[Raccolta di campi del set di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
Vengono fornite informazioni sulla raccolta di campi di set di dati generata dalla query.  
  
[Origini dei dati supportate da Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
Vengono fornite informazioni dettagliate sul supporto delle piattaforme e delle versioni per ogni estensione per i dati.  
  
## <a name="see-also"></a>Vedere anche

[Database SQL di Microsoft Azure in docs.microsoft.com](/azure/sql-database/)  
[Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtro, raggruppamento e ordinamento di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
