---
title: Tipo di connessione XML | Microsoft Docs
description: Informazioni sul tipo di connessione XML per connettersi e recuperare dati da documenti XML, servizi Web o codice XML incorporato nella query.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 5b55fff2-1b15-4156-83ef-15ad9cf9f509
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6ca1df39a5aeed8b57de5c6eff7ab66e725062d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596535"
---
# <a name="xml-connection-type-ssrs"></a>Tipo di connessione XML (SSRS)
  Per includere dati nel report da un'origine dati XML, è necessario disporre di un set di dati basato su un'origine dati del report di tipo XML. Questo tipo di origine dati incorporato è basato sull'estensione per i dati XML. Utilizzare questo tipo di origine dati per connettersi e recuperare dati da documenti XML, servizi Web o valori XML incorporati nella query.  
  
 Questa estensione per i dati supporta parametri e credenziali gestiti separatamente dalla stringa di connessione.  
  
 Usare le informazioni presenti in questo argomento per compilare un'origine dati. Per istruzioni dettagliate, vedere [Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Stringa di connessione  
 La stringa di connessione deve essere un URL che punta al servizio Web, all'applicazione Web o al documento XML disponibile tramite HTTP. I documenti XML devono avere estensione xml. È inoltre possibile utilizzare una stringa di connessione vuota per i dati XML incorporati nella query del set di dati.  
  
 Nell'esempio seguente viene illustrata la sintassi della stringa di connessione, rispettivamente per un servizio Web e per un documento XML. Il protocollo `file://` non è supportato.  
  
|Tipo di documento XML|Esempio di stringa di connessione|  
|-----------------------|-------------------------------|  
|Servizio Web|`https://adventure-works.com/results.aspx`|  
|Documento XML|`https://localhost/XML/Customers.xml`|  
|Documento XML incorporato|*Vuoto*|  
  
 Per altri esempi di stringhe di connessione, vedere [Connessioni dati, origini dati e stringhe di connessione in Generatore report](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenziali  
 Le credenziali sono necessarie per eseguire query, nonché per visualizzare l'anteprima del report in locale e dal server di report.  
  
 Dopo aver pubblicato il report, potrebbe essere necessario modificare le credenziali per l'origine dati affinché quando il report viene eseguito nel server di report, le autorizzazioni per il recupero dei dati risultino valide.  
  
 Da un client di creazione di report sono disponibili le opzioni seguenti per la specifica delle credenziali:  
  
-   Utente di Windows corrente (nota anche come sicurezza integrata).  
  
-   Non sono necessarie credenziali. Se non si specificano credenziali, viene utilizzato l'accesso anonimo. Verificare di aver definito l'account di esecuzione automatica per il server di report per eseguire la connessione a un'origine dei dati esterna. L'estensione per l'elaborazione di dati XML non passa credenziali all'URL di destinazione o al servizio Web, pertanto la connessione ha esito positivo solo se è stato definito l'account di esecuzione automatica. Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Credenziali archiviate e credenziali fornite dall'utente non sono supportate. Se la sicurezza integrata di Windows è disabilitata, non è possibile utilizzarla per recuperare dati. Se si specificano credenziali archiviate o fornite dall'utente, si verificherà un errore in fase di esecuzione.  
  
 Per altre informazioni, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) o [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Query  
 Una query consente di specificare quali dati recuperare per un set di dati del report. Le colonne nel set di risultati per una query popolano la raccolta dei campi per un set di dati. In un report viene elaborato solo il primo set di risultati recuperato da una query.  
  
 Per creare la query, è necessario utilizzare la finestra Progettazione query basata su testo. La query deve restituire dati XML.  
  
 Per altre informazioni sulla finestra Progettazione query basata su testo, vedere [Interfaccia utente di Progettazione query basata su testo &#40;Generatore report &#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 I valori possibili di una query del set di dati per un'origine dati di tipo XML sono illustrati di seguito.  
  
-   *Vuoto*  
  
     Utilizzare una query vuota per creare un set di risultati predefinito. La query predefinita viene creata leggendo l'origine dei dati e attraversando la gerarchia del nodo XML fino alla prima raccolta foglia. Il set di risultati include tutti i nodi con valori di testo e tutti gli attributi dei nodi nel percorso. Sulle colonne del set di risultati viene eseguito il mapping ai campi del set di dati.  
  
-   *Percorso di elemento*  
  
     Specifica la sequenza di nodi da utilizzare per recuperare i dati XML dall'origine dei dati.  
  
-   *Elemento Query XML*  
  
     Specifica di query XML con gli elementi facoltativi seguenti:  
  
    -   **L'origine dati XML è un servizio Web**  
  
         Elementi XML obbligatori:  
  
         `<Method Namespace=` *"namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *soap action* `</SoapAction>`  
  
         Elementi XML facoltativi:  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
         `<Method Namespace=` *"namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *soap action* `</SoapAction>`  
  
    -   **L'origine dati XML è un documento XML**  
  
         Elementi XML obbligatori: nessuno  
  
         Elementi XML facoltativi:  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
    -   **L'origine dati XML è un documento XML incorporato**  
  
         Elementi XML obbligatori:  
  
         `<XmlData>` inner XML `</XmlData>`  
  
         Elementi XML facoltativi:  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
         `-- or --`  
  
         `<ElementPath IgnoreNamespaces="true">`  *element path*  `</ElementPath>`  
  
 Per altre informazioni sulla sintassi delle query, vedere [Sintassi di XML Query per i dati del report XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).  
  
 Per gli esempi, vedere [Reporting Services: uso di origini dati XML e servizio Web](/previous-versions/sql/sql-server-2005/administrator/aa964129(v=sql.90)).  
  
### <a name="requirements-for-retrieving-xml-web-service-data"></a>Requisiti per il recupero di dati del servizio Web XML  
 Lo schema non viene rilevato automaticamente dall'estensione per l'elaborazione dati XML. È pertanto necessario essere in grado di individuare i metodi SOAP tramite i quali verranno recuperati i dati desiderati. È inoltre necessario comprendere lo spazio dei nomi o lo schema di indirizzamento che il servizio Web utilizza per i dati.  
  
 Per un servizio Web, è possibile specificare un elemento \<**Query**> che definisce un metodo da chiamare o un'azione SOAP. È possibile lasciare la query vuota e utilizzare la query predefinita se l'origine dei dati XML ha una struttura gerarchica che genera i dati che si desidera utilizzare per il report. Per gli attributi e i valori del nodo elemento XML recuperati durante l'esecuzione della query viene eseguito il mapping ai campi del set di dati utilizzati nel report.  
  
### <a name="requirements-for-retrieving-xml-document-data"></a>Requisiti per il recupero di dati di documenti XML  
 Se si usa il protocollo HTTP, il server deve restituire dati XML oppure i dati XML devono essere incorporati nell'elemento **Query** XML. Se si fa riferimento a un documento XML direttamente utilizzando il protocollo HTTP, l'estensione deve essere xml.  
  
 È necessario conoscere la procedura di creazione di una query XML per il recupero di tutti i dati che si desidera utilizzare. Se non viene specificato un percorso di elemento, il comportamento predefinito previsto per l'analisi di un documento XML consiste nel selezionare il primo percorso disponibile di una raccolta di nodi foglia nel documento XML. Se nel documento XML sono inclusi percorsi aggiuntivi di altre raccolte di nodi foglia di pari livello, tali nodi verranno ignorati a meno che non venga specificato un percorso nella query.  
  
 È possibile specificare un percorso di elemento utilizzando una sintassi XML simile a XQuery.  
  
 Per altre informazioni, vedere [Sintassi del percorso di elemento per i dati del report XML &#40;SSRS&#41;](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md).  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parametri  
 La query non viene analizzata per identificare parametri.  
  
 Per aggiungere i parametri, è necessario crearli manualmente usando la pagina **Parametri** nella finestra di dialogo [Proprietà set di dati](/previous-versions/sql/) .  
  
##  <a name="remarks"></a><a name="Remarks"></a> Osservazioni  
 L'estensione per i dati XML supporta report di dati XML tabulari e non gerarchici. Per altre informazioni, vedere [Aggiungere dati da origini dati esterne &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 Non è disponibile alcun supporto predefinito per il recupero di documenti XML da un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Procedure  
 In questa sezione sono contenute istruzioni dettagliate per l'utilizzo di connessioni dati, origini dati e set di dati.  
  
 [Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Creare un set di dati condiviso o un set di dati incorporato &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Aggiungere un filtro a un set di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
##  <a name="related-sections"></a><a name="Related"></a> Sezioni correlate  
 In queste sezioni della documentazione sono incluse informazioni concettuali approfondite sui dati dei report, nonché le informazioni necessarie sulle procedure per definire, personalizzare e usare parti di un report correlate ai dati.  
  
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
 [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtro, raggruppamento e ordinamento di dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
