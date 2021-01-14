---
title: Novità
description: Informazioni sulle modifiche, i miglioramenti e gli aggiornamenti nella SQL Server versione 2019 di Master Data Services.
ms.custom: ''
ms.date: 07/08/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad530f60-d480-4457-ba7a-93a10c8a1695
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7e731d14967d808aa57f3fa0e001e5e593238ffd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170433"
---
# <a name="what39s-new-in-master-data-services-mds"></a>Novità in Master Data Services (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Questo argomento riepiloga le modifiche e gli aggiornamenti disponibili nella versione [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. 
  
 Per una panoramica sull'organizzazione dei dati in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], vedere [Panoramica di Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md). 
  
 **Per installare Master Data Services, configurare il database e il sito Web e distribuire i modelli di esempio, vedere** [Panoramica di Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).  
  
 **Scaricare**  
  
-   Per scaricare [!INCLUDE[ssSQL15](../includes/sssql16-md.md)], passare a  **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   Se si ha un account di Azure,  fare clic **[qui](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview)** per creare rapidamente una macchina virtuale in cui è già installato [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] .  
  
##  <a name="improved-performance"></a>Prestazioni migliorate  
  
 I miglioramenti delle prestazioni consentono di creare modelli di dimensioni maggiori, caricare i dati in modo più efficiente e ottenere migliori prestazioni complessive. I miglioramenti riguardano anche le prestazioni del componente aggiuntivo per Microsoft Excel in cui sono stati ridotti i tempi di caricamento dei dati ed è stata abilitata la funzionalità che permette di gestire entità di dimensioni maggiori.  
  
 Per altre informazioni sul componente aggiuntivo per Microsoft Excel, vedere [Componente aggiuntivo Master Data Services per Microsoft Excel](../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md).  
  
 Sono inclusi i seguenti miglioramenti di funzionalità.  
  
-   Compressione dei dati a livello di entità, abilitata per impostazione predefinita. Quando è abilitata la compressione dei dati, tutte le tabelle e gli indici associati all'entità vengono compressi con la compressione a livello di riga SQL. In questo modo vengono ridotte notevolmente le operazioni I/O del disco durante la lettura o l'aggiornamento dei dati master, in particolare quando i dati master includono milioni di righe e/o hanno molte colonne con valori NULL.  
  
     A causa di un leggero incremento nell'utilizzo della CPU sul lato del motore di SQL Server, se la CPU è associata al server è possibile disattivare la compressione dei dati modificando l'entità.  
  
     Per altre informazioni, vedere [Creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md) e [Compressione dei dati](../relational-databases/data-compression/data-compression.md).  
  
-   La funzionalità IIS Compressione contenuto dinamico è abilitata per impostazione predefinita. Questo riduce in modo significativo le dimensioni della risposta XML e salva le operazioni I/O di rete, anche se aumenta l'utilizzo della CPU. Se si ha una CPU associata al server, è possibile disattivare la compressione dei dati aggiungendo l'impostazione seguente al file Web.config di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
    ```  
    <configuration>  
       \<system.webServer>  
          <urlCompression doStaticCompression="true" doDynamicCompression="false " />  
       \</system.webServer>  
    </configuration>  
  
    ```  
  
     Per altre informazioni, vedere [Compressione degli URL](https://www.iis.net/configreference/system.webserver/urlcompression)  
  
-   I processi di SQL Server Agent seguenti eseguono la manutenzione di indici e log.  
  
    -   MDS_MDM_Sample_Index_Maintenace  
  
    -   MDS_MDM_Sample_Log_Maintenace  
  
 Per impostazione predefinita, il processo MDS_MDM_Sample_Index_Maintenance viene eseguito ogni settimana. È possibile modificare la pianificazione. È anche possibile eseguire manualmente il processo in qualsiasi momento usando la stored procedure udpDefragmentation. Si consiglia di eseguire la stored procedure ogni volta che viene inserita o aggiornata una grande quantità di dati master oppure dopo la creazione di una nuova versione dalla versione esistente.  
  
 Un indice con una frammentazione maggiore del 30% viene ricompilato online. Durante la ricompilazione, può verificarsi una riduzione del livello delle prestazioni dell'operazione CRUD nella stessa tabella. Se la riduzione del livello delle prestazioni rappresenta un problema, si consiglia di eseguire la stored procedure durante l'orario non lavorativo. Per ulteriori informazioni sulla frammentazione degli indici, vedere [Reorganize and Rebuild Indexes](../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Per altre informazioni, vedere questo post del blog su Master Data Services relativo al [miglioramento delle prestazioni e della scalabilità in SQL Server 2016](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-and-scale-improvement-in-sql-server-2016/ba-p/388225).  
  
##  <a name="improved-security"></a>Sicurezza migliorata  
  
 La nuova autorizzazione della funzione Utente con privilegi avanzati concede a un utente o a un gruppo le stesse autorizzazioni di amministratore del server della versione precedente di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. L'autorizzazione Utente con privilegi avanzati può essere assegnata a più utenti e gruppi. Nella versione precedente l'utente che aveva installato inizialmente [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] era l'amministratore del server ed era difficile trasferire questa autorizzazione a un altro utente o gruppo. Per altre informazioni, vedere [Autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 Ora l'autorizzazione di amministratore può essere assegnata a un utente in modo esplicito al livello del modello. Ciò significa che l'utente non perderà l'autorizzazione di amministratore se in un secondo momento gli vengono assegnate autorizzazioni nel sottoalbero di modello, ad esempio per il livello di entità.  
  
 In questa versione di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] vengono forniti altri livelli di autorizzazioni grazie all'introduzione di questi nuovi tipi: lettura, creazione, aggiornamento ed eliminazione. Ad esempio, un utente che ha solo l'autorizzazione di aggiornamento ora può aggiornare i dati master senza creare o eliminare i dati. Quando si concede l'autorizzazione di creazione, aggiornamento o eliminazione, all'utente viene assegnata automaticamente anche l'autorizzazione di lettura. È anche possibile combinare le autorizzazioni di lettura, creazione, aggiornamento ed eliminazione.  
  
 Durante l'aggiornamento a [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], le autorizzazioni precedenti vengono convertite nelle nuove autorizzazioni, come illustrato nella tabella seguente.  
  
|Autorizzazione nella versione precedente|Nuova autorizzazione|  
|------------------------------------|--------------------|  
|L'utente che installa inizialmente [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ha l'autorizzazione di amministratore del server.|L'utente ha l'autorizzazione della funzione Utente con privilegi avanzati|  
|L'utente ha autorizzazioni di aggiornamento al livello del modello e nessuna autorizzazione nel sottoalbero di modello, quindi è implicitamente un amministratore del modello.|L'utente ha autorizzazioni di amministratore esplicite al livello del modello.|  
|L'utente ha autorizzazioni di sola lettura.|L'utente ha autorizzazioni di accesso in lettura.|  
|L'utente ha autorizzazioni di aggiornamento.|L'utente ha tutte e quattro le autorizzazioni di accesso: creazione, aggiornamento, eliminazione e lettura.|  
|L'utente ha autorizzazioni di negazione|L'utente ha autorizzazioni di negazione|  
  
 Per altre informazioni sulle autorizzazioni, vedere [Sicurezza &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md).  
  
##  <a name="improved-transaction-log-maintenance"></a>Manutenzione dei log delle transazioni migliorata  
  
 Ora è possibile pulire i log delle transazioni a intervalli predeterminati o secondo una pianificazione usando le impostazioni di sistema e al livello del modello. In un sistema MDS con molte modifiche ai dati e processi ETL, queste tabelle possono aumentare in modo esponenziale e causare problemi di relativi allo spazio di archiviazione e alla riduzione del livello delle prestazioni.  
  
 I tipi di dati seguenti possono essere rimossi dai log.  
  
-   Cronologia delle transazioni anteriore a un numero di giorni specificato.  
  
-   Cronologia dei problemi di convalida anteriore a un numero di giorni specificato.  
  
-   Batch di gestione temporanea eseguiti prima di un numero di giorni specificato.  
  
 È possibile configurare la frequenza con cui i dati vengono rimossi dai log delle transazioni usando le impostazioni di sistema e al livello del modello. Per altre informazioni, vedere [Impostazioni di sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md) e [Creare un modello &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md). Per altre informazioni sulle transazioni, vedere [Transazioni &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
 Il processo di SQL Server Agent, MDS_MDM_Sample_Log_Maintenace, attiva la pulizia dei log delle transazioni e viene eseguito ogni notte. È possibile usare SQL Server Agent per modificare la pianificazione del processo.  
  
 È anche possibile chiamare le stored procedure per pulire i log delle transazioni. Per altre informazioni, vedere [Transazioni &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
## <a name="improved-troubleshooting"></a>Risoluzione dei problemi migliorata  
  
 In [!INCLUDE[ssSQL15](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]sono state aggiunte funzionalità per migliorare il debug e semplificare la risoluzione dei problemi. Per altre informazioni, vedere [Traccia &#40;Master Data Services&#41;](../master-data-services/tracing-master-data-services.md).  
  
## <a name="improved-manageability"></a>Gestione migliorata  
  
 I miglioramenti alla gestione consentono di ridurre i costi di manutenzione e hanno effetti positivi sul ritorno sugli investimenti (ROI). Includono miglioramenti alla sicurezza e alla manutenzione dei log delle transazioni, oltre alle nuove funzionalità seguenti.  
  
-   Uso di nomi di attributi non più lunghi di 50 caratteri.  
  
-   Possibilità di rinominare e nascondere gli attributi Name e Code.  
  
 Per ulteriori informazioni, vedere gli argomenti seguenti.  
  
-   [Modelli &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
-   [Entità &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
-   [Transazioni &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)  
  
-   [Sicurezza &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)  

## <a name="business-rule-improvements"></a>Miglioramenti alle regole business
 **Gestire le regole business (componente aggiuntivo MDS per Excel)**  
  
 Nel componente aggiuntivo Master Data Services per Excel è possibile gestire le regole business, ad esempio crearle e modificarle. Le regole business vengono usate per convalidare i dati.  
 
 **Business Rules Extension**  
  
 È possibile applicare gli script SQL definiti dall'utente come estensione delle azioni e delle condizioni della regola business. Le funzioni SQL possono essere usate come condizioni. Le stored procedure SQL possono essere usate come azioni. Per altre informazioni, vedere [Estensione delle regole business &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md). 
 
 **Nuova progettazione dell'esperienza di gestione delle regole business**  
  
 La gestione delle regole business in MDS è stata completamente riprogettata per migliorare l'esperienza degli utenti. Per altre informazioni su questa funzionalità, vedere [Regole business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md).  
  
 **Funzionalità di gestione delle regole business rimossa dal componente aggiuntivo MDS per Excel**  
  
 La funzionalità di gestione delle regole business è stata rimossa dal componente aggiuntivo MDS per Excel perché è stata introdotta una nuova progettazione.    

 **Nuove condizioni della regola business**  
  
 Per fornire un set completo di condizioni della regola business, ne sono state aggiunte altre sette completamente nuove. Per altre informazioni, vedere [Condizioni della regola business &#40;Master Data Services&#41;](../master-data-services/business-rule-conditions-master-data-services.md).  

## <a name="derived-hierarchy-improvements"></a>Miglioramenti alle gerarchie derivate

 **Relazioni molti-a-molti nelle gerarchie derivate**  
  
 Ora è possibile creare una gerarchia derivata che visualizza le relazioni molti-a-molti. Una relazione molti-a-molti tra due entità può essere modellata usando una terza entità che stabilisce un mapping tra di esse. L'entità di mapping è un'entità che contiene due o più attributi basati su dominio che fanno riferimento ad altre entità.  
  
 Ad esempio, l'entità M ha un attributo basato su dominio che fa riferimento ad A e un attributo basato su dominio che fa riferimento a B. È possibile creare una gerarchia da A a B usando l'entità di mapping.  
  
 Per altre informazioni, vedere [Mostrare le relazioni molti-a-molti nelle gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md).  
 
 **Modificare le relazioni molti-a-molti nelle gerarchie derivate**  
  
 È possibile modificare la relazione molti-a-molti cambiando i membri dell'entità di mapping. Per ulteriori informazioni, vedere la pagina relativa alla [visualizzazione delle relazioni molti-a-molti nelle gerarchie Derivate &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md).  
 
 **Esperienza di gestione delle gerarchie derivate migliorata**  
  
 L'esperienza di gestione delle gerarchie derivate in MDS è stata migliorata. Per altre informazioni su questa funzionalità, vedere [Creare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md).  
  
 La funzionalità di gestione delle regole business è stata rimossa dal componente aggiuntivo MDS per Excel perché è stata introdotta una nuova progettazione.  
 
## <a name="attribute-improvements"></a>Miglioramenti agli attributi   
    
 **Indici personalizzati**  
  
 Per migliorare le prestazioni delle query, è possibile creare un indice non cluster in un attributo (indice singolo) o in un elenco di attributi (indice composto) in un'entità. Per altre informazioni, vedere [Indice personalizzato &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md).  
 
  **Filtri di attributo**  
  
 Per un attributo basato su dominio di un membro foglia, è possibile usare un attributo padre di filtro per limitare i valori consentiti per l'attributo basato su dominio. Per altre informazioni, vedere [Creare un attributo basato su dominio &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
 
## <a name="entity-and-member-improvements"></a>Miglioramenti alle entità e ai membri 
  
 **Relazione di sincronizzazione delle entità**  
  
 È possibile condividere i dati delle entità tra i vari modelli creando una relazione di sincronizzazione delle entità. Per altre informazioni, vedere [Relazione di sincronizzazione delle entità &#40;Master Data Services&#41;](../master-data-services/entity-sync-relationship-master-data-services.md).  
  
 **Rimuovere i membri eliminati temporaneamente**  
  
 Ora è possibile rimuovere (eliminare definitivamente) tutti i membri eliminati temporaneamente nella versione di un modello. Con l'eliminazione, un membro viene solo disattivato o eliminato temporaneamente. Per altre informazioni, vedere [Ripulire i membri di versione &#40;Master Data Services&#41;](../master-data-services/purge-version-members-master-data-services.md).  
 
## <a name="improvements-for-managing-changes"></a>Miglioramenti per la gestione delle modifiche 
  
 **Cronologia delle revisioni del membro**  
  
 Una cronologia delle revisioni del membro viene registrata quando un membro viene modificato. È possibile eseguire il rollback di una cronologia delle revisioni, nonché visualizzare e annotare le revisioni. Con la proprietà **Giorni di conservazione log** , è possibile specificare per quanto tempo conservare i dati cronologici. Per altre informazioni, vedere [Cronologia delle revisioni del membro &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md).  
  
 **Conflitti di unione**  
  
 Se si prova a pubblicare dati modificati da un altro utente, la pubblicazione non riesce e viene visualizzato un errore di conflitto. Per risolvere questo errore, è possibile usare la funzionalità Conflitti di unione e ripubblicare le modifiche. Per altre informazioni, vedere [Conflitti di unione (Master Data Services)](../master-data-services/merge-conflicts-master-data-services.md) e [Conflitti di unione (componente aggiuntivo MDS per Excel)](../master-data-services/microsoft-excel-add-in/merge-conflicts-mds-add-in-for-excel.md).  
  
 **Insiemi di modifiche**  
  
 È possibile usare gli insiemi di modifiche per salvare, visualizzare e modificare le modifiche in sospeso di un'entità. Se l'entità richiede l'approvazione delle modifiche, è necessario salvare le modifiche in sospeso in un insieme di modifiche e inviarle all'amministratore per l'approvazione. Per ulteriori informazioni, vedere [insiemi di modifiche &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md).  
  
 **Gestione e notifiche tramite posta elettronica per l'insieme di modifiche**  
  
 In questa versione è possibile visualizzare e gestire tutte le modifiche in base al modello e alla versione. È anche possibile ricevere notifiche tramite posta elettronica ogni volta che viene modificato lo stato di un insieme di modifiche per un'entità che richiede l'approvazione. Per altre informazioni, vedere [Gestire gli insiemi di modifiche &#40;Master Data Services&#41;](../master-data-services/manage-changesets-master-data-services.md) e [Notifiche &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md).  
  
 **Visualizzare e gestire la cronologia delle revisioni**  
  
 È possibile visualizzare e gestire la cronologia delle revisioni, in base all'entità e al membro. Se si hanno le autorizzazioni di aggiornamento, è possibile eseguire il rollback di un membro alla versione precedente. Per altre informazioni, vedere [Cronologia delle revisioni del membro &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md).  
 
## <a name="tool-and-sample-improvements"></a>Miglioramenti agli strumenti e agli esempi 
  
 **Salvare o aprire file di query nel componente aggiuntivo MDS per Excel**  
  
 Dalla pagina Entity Explorer è possibile fare clic su **Excel** per salvare i file di query di collegamento. In alternativa, è possibile aprire il file di query archiviato nel computer nel componente aggiuntivo MDS per Excel. Il file salvato può essere aperto con l'applicazione QueryOpener. Per altre informazioni, vedere [File di query collegamento &#40;componente aggiuntivo MDS per Excel&#41;](../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
 Il file di query contiene i filtri e le informazioni di gerarchia recuperate nella pagina di Explorer.  
   
 **Pacchetti di distribuzione per i modelli di esempio aggiornati**  
  
 I pacchetti di esempio sono stati aggiornati per supportare nuovi scenari. Per altre informazioni, vedere [Esempi: pacchetti di distribuzione di modelli (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità di Master Data Services e Data Quality Services supportate dalle edizioni di SQL Server 2016](../master-data-services/master-data-services-and-data-quality-services-features-support.md)  
 [Funzionalità deprecate di Master Data Services](../master-data-services/deprecated-master-data-services-features.md)  
 [Funzionalità di Master Data Services non più supportate](../master-data-services/discontinued-master-data-services-features.md)
