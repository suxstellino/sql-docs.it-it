---
title: Tipo di connessione dell'elenco SharePoint | Microsoft Docs
description: Questo articolo include informazioni sul tipo di connessione Elenco SharePoint e su come creare un'origine dati.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 2c4adf2f-e9c4-4fae-bd3c-97fe64436caf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7bb6380e168c1e4bddefab539354d80bf14805c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033471"
---
# <a name="sharepoint-list-connection-type-ssrs"></a>Tipo di connessione dell'elenco SharePoint (SSRS)

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)] [!INCLUDE [ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

Per includere dati da un elenco Microsoft SharePoint nel report, è necessario aggiungere o creare un set di dati basato su un'origine dati del report di tipo Elenco Microsoft SharePoint. Si tratta di un tipo di origine dati predefinito basato sull'estensione per i dati dell'Elenco SharePoint di Microsoft SQL Server Reporting Services. Usare questo tipo di origine dati per connettersi e recuperare i dati dell'elenco dai siti di SharePoint 2013 e seguenti.

Usare le informazioni presenti in questo argomento per compilare un'origine dati. Per istruzioni dettagliate, vedere [Aggiungere e verificare una connessione dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  

##  <a name="connection-string"></a><a name="Connection"></a> Stringa di connessione  
 La stringa di connessione a un elenco SharePoint è l'URL al sito principale o secondario di SharePoint, ad esempio `https://MySharePointWeb/MySharePointSite` o `https://MySharePointWeb/MySharePointSite/Subsite`.  
  
 In Progettazione query vengono visualizzati automaticamente gli elenchi SharePoint a cui è possibile accedere perché si dispone delle autorizzazioni sufficienti.  
  
 Per altri esempi di stringhe di connessione, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenziali  
 Le credenziali sono necessarie per eseguire query, nonché per visualizzare l'anteprima del report in locale e dal server di report. Dopo aver pubblicato il report, potrebbe essere necessario modificare le credenziali per l'origine dati affinché quando il report viene eseguito nel server di report, le autorizzazioni per il recupero dei dati risultino valide. I tipi di credenziali che è possibile usare con questa estensione per i dati dipendono dalla configurazione della tecnologia SharePoint per l'elenco SharePoint usato come origine dati.  
  
 Nelle tabelle seguenti viene illustrato il comportamento di recupero delle credenziali per l'estensione dell'elenco SharePoint, quando ci si connette a un elenco SharePoint di una farm locale e a un elenco SharePoint remoto.  
  
 La **Tabella 1** è indicata per i report distribuiti in un sito Windows SharePoint legacy. Un sito Windows legacy supporta solo Kerberos, NTLM e l'autenticazione basata su moduli (FBA, Forms Based Authentication). La **tabella 2** è indicata per i report distribuiti in un sito di SharePoint basato sulle attestazioni.  
  
 **Tabella 1**  
  
|Tipo di recupero|Credenziali supportate|Autenticazione di Windows in modalità classica|\* Autenticazione attestazioni|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|Elenco SharePoint di una farm locale|Autenticazione di Windows (integrata) o token utente di SharePoint|Sì|Sì|  
||Archiviate, su richiesta, nessuna (con credenziali di Windows)<br /><br /> Le credenziali archiviate e su richiesta con credenziali non di Windows non sono supportate.|Sì|No|  
|Elenco SharePoint remoto|Autenticazione di Windows (integrata) o token utente di SharePoint|Sì|No<br /><br /> L'autenticazione basata su form e l'autenticazione delle attestazioni non sono supportate per gli elenchi SharePoint remoti.|  
||Archiviate, su richiesta, nessuna (con credenziali di Windows)<br /><br /> Le credenziali archiviate e su richiesta con credenziali non di Windows non sono supportate.|Sì|No<br /><br /> L'autenticazione basata su form e l'autenticazione delle attestazioni non sono supportate per gli elenchi SharePoint remoti.|  
  
 * Autenticazione di Windows, autenticazione basata su form (FBA), token SAML (Secure Application Markup Language), altri provider di identità o una combinazione di più provider di autenticazione tra quelli indicati in precedenza.  
  
 **Tabella 2**  
  
|Tipo di recupero|Credenziali supportate|Autenticazione di Windows in modalità classica|\* Autenticazione attestazioni|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|Elenco SharePoint di una farm locale|Autenticazione di Windows (integrata) o token utente di SharePoint|Sì|Sì|  
||Archiviate, su richiesta, nessuna (con credenziali di Windows)<br /><br /> Le credenziali archiviate e su richiesta con credenziali non di Windows non sono supportate.|No|No|  
|Elenco SharePoint remoto|Autenticazione di Windows (integrata) o token utente di SharePoint|Sì|No<br /><br /> L'autenticazione basata su form e l'autenticazione delle attestazioni non sono supportate per gli elenchi SharePoint remoti.|  
||Archiviate, su richiesta, nessuna (con credenziali di Windows)<br /><br /> Le credenziali archiviate e su richiesta con credenziali non di Windows non sono supportate.|No|No<br /><br /> L'autenticazione basata su form e l'autenticazione delle attestazioni non sono supportate per gli elenchi SharePoint remoti.|  
  
 * Autenticazione di Windows, autenticazione basata su form (FBA), token SAML (Secure Application Markup Language), altri provider di identità o una combinazione di più provider di autenticazione tra quelli indicati in precedenza.  
  
 **Autenticazione di Windows**  
 Per una tecnologia SharePoint configurata per essere usata con un server di report in modalità Account attendibile, questa opzione non è supportata. Si applica solo alle versioni precedenti di SQL Server 2012 Reporting Services.

 Per una tecnologia SharePoint configurata per essere usata con un server di report nella modalità integrata di Windows, questa opzione si applica sia all'utente corrente di Windows sia all'utente corrente di SharePoint.
 
 Per una tecnologia SharePoint configurata per essere usata senza un server di report (modalità locale), questa opzione non è supportata. Per altre informazioni sulla modalità locale, vedere [Report in modalità locale e con connessione nel visualizzatore di report &#40;Reporting Services in modalità SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md).  
  
 **Credenziali non richieste (Non usare credenziali):**  
 Per usare questa opzione, è necessario configurare l'account di esecuzione automatica sul server di report. Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Per informazioni sul supporto dell'autenticazione delle attestazioni nello stack di Microsoft Business Intelligence, vedere la pagina relativa all' [utilizzo dell'autenticazione delle attestazioni nello stack di Microsoft Business Intelligence](https://social.technet.microsoft.com/wiki/contents/articles/15274.using-claims-authentication-across-the-microsoft-bi-stack.aspx).  
  
 Per altre informazioni, vedere [Creare stringhe di connessione dati - Generatore report e SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md), [Specificare credenziali e informazioni di connessione per le origini dati del report](specify-credential-and-connection-information-for-report-data-sources.md) e [Origini dati supportate da Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
##  <a name="queries"></a><a name="Query"></a> Query  
 Per progettare una query, creare un nuovo set di dati basato sull'origine dati, quindi aprire la finestra Progettazione query associata. Per altre informazioni, vedere [Creare un set di dati condiviso o un set di dati incorporato &#40;Generatore report e SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md).  
  
 Nella finestra Progettazione query con interfaccia grafica dell'elenco SharePoint vengono visualizzati quattro riquadri:  
  
 **Elenchi SharePoint**  Viene visualizzato un elenco di tutti gli elenchi SharePoint nel sito per questa origine dati. Selezionare un elenco, quindi selezionare i campi desiderati per la query. I nomi di campi in questo riquadro sono i nomi descrittivi di SharePoint, noti anche come nomi visualizzati. Posizionarsi su un elemento per visualizzare le proprietà seguenti nella descrizione comando:  
  
-   **Nome** Il nome univoco del campo.  
  
-   **Identificatore** L'identificatore univoco del campo.  
  
-   **Tipo campo** Il tipo di dati del campo.  
  
-   **Nascosto** Se il campo viene visualizzato nella visualizzazione elenco SharePoint.  
  
 La selezione di campi da più elenchi non è supportata. È possibile creare un set di dati per ogni elenco e selezionare campi da ogni set di dati. Se gli elenchi includono un campo comune, sarà possibile usare la funzione Lookup in un'area di dati tablix associata a un set di dati per recuperare un valore dall'altro set di dati non associato all'area di dati. Per altre informazioni, vedere [Funzione Lookup &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/report-builder-functions-lookup-function.md).  
  
-   **Campi selezionati**  Vengono visualizzati i campi selezionati. I nomi di campi in questo riquadro sono i nomi descrittivi specificati da un utente di SharePoint. Quando si chiude Progettazione query, questi nomi vengono visualizzati nella raccolta campi del set di dati nel riquadro Dati report. La relazione tra nomi univoci e nomi descrittivi è disponibile nella pagina [Finestra di dialogo Proprietà set di dati, Campi &#40;Generatore report&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md).  
  
-   **Filtri applicati**  Consente di limitare i dati restituiti dall'elenco SharePoint, prima di essere restituiti al report. Selezionare il nome campo, l'operatore e il valore da usare per limitare i dati recuperati nell'elenco. Gli operatori variano a seconda del tipo di dati del valore selezionato.  
  
     Non è possibile modificare il tipo di ordinamento o specificare gruppi nella finestra Progettazione query con interfaccia grafica. Per eseguire tale operazione, impostare le espressioni di ordinamento nel set di dati del report e le espressioni di raggruppamento nelle aree dati del report. I parametri di query non sono supportati. Per filtrare i dati nel report, usare i filtri del report o i parametri del report creati. Per altre informazioni, vedere [Filtrare, raggruppare e ordinare i dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) e [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
-   **Risultati query**  Vengono visualizzate le righe di esempio restituite quando viene eseguita la query. Se i valori dell'elenco SharePoint vengono frequentemente modificati sul sito di SharePoint, i valori visualizzati nel riquadro dei risultati della query potrebbero differire da quelli visualizzati nel report.  
  
-   **Campi selezionati**  Vengono visualizzati i campi selezionati. I nomi di campi in questo riquadro sono i nomi descrittivi specificati da un utente di SharePoint. Quando si chiude Progettazione query, questi nomi vengono visualizzati nella raccolta campi del set di dati nel riquadro Dati report. La relazione tra nomi univoci e nomi descrittivi è disponibile nella pagina [Finestra di dialogo Proprietà set di dati, Campi &#40;Generatore report&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md).  
  
-   **Filtri applicati**  Consente di limitare i dati restituiti dall'elenco SharePoint, prima di essere restituiti al report. Selezionare il nome campo, l'operatore e il valore da usare per limitare i dati recuperati nell'elenco. Gli operatori variano a seconda del tipo di dati del valore selezionato.  
  
     Non è possibile modificare il tipo di ordinamento o specificare gruppi nella finestra Progettazione query con interfaccia grafica. Per eseguire tale operazione, impostare le espressioni di ordinamento nel set di dati del report e le espressioni di raggruppamento nelle aree dati del report. I parametri di query non sono supportati. Per filtrare i dati nel report, usare i filtri del report o i parametri del report creati. Per altre informazioni, vedere [Filtrare, raggruppare e ordinare i dati &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) e [Parametri report &#40;Generatore report e Progettazione report&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
-   **Risultati query**  Vengono visualizzate le righe di esempio restituite quando viene eseguita la query. Se i valori dell'elenco SharePoint vengono frequentemente modificati sul sito di SharePoint, i valori visualizzati nel riquadro dei risultati della query potrebbero differire da quelli visualizzati nel report.  
  
 Per altre informazioni, vedere [Progettazione query di elenco di SharePoint &#40;Generatore report&#41;](../../reporting-services/report-data/sharepoint-list-query-designer-report-builder.md).  
  
### <a name="query-text"></a>Testo della query  
 Per visualizzare la query generata dalla finestra Progettazione query con interfaccia grafica., passare a Progettazione query basata su testo. In questa visualizzazione è possibile vedere l'XML creato dalla finestra Progettazione query con interfaccia grafica. Nell'XML sono inclusi gli elementi per il nome dell'elenco, la raccolta campi e il filtro.  
  
#### <a name="example-1-specified-fields-for-a-list"></a>Esempio 1. Campi specificati per un elenco  
 Nell'esempio seguente viene mostrata una query di SharePoint valida:  
  
```  
<RSSharePointList>  
<listName>MyList</listName>  
<viewFields>  
  <FieldRef Name="Field1"/>  
  <FieldRef Name="Field4"/>  
</viewFields>  
<Query>  
  <Where>  
    <And>  
      <Gt>  
        <FieldRef Name="Field1"/>  
        <Value Type="Integer">1</Value>  
      </Gt>  
      <IsNotNull>  
        <FieldRef Name="Field2"/>  
        <Value Type="string"/>  
      </IsNotNull>   
    </And>  
  </Where>  
</Query>  
</RSSharePointList>  
```  
  
 È possibile modificare questa vista della query finché rimane il testo XML valido.  
  
#### <a name="example-2-all-fields-for-a-list"></a>Esempio 2. Tutti i campi per un elenco  
 È possibile specificare anche solo il nome di un elenco, e tutti i campi, inclusi i campi nascosti, restituiti. Nell'esempio seguente vengono recuperati tutti i campi da un elenco denominato Attività:  
  
```  
<RSSharePointList>  
<listName>Tasks</listName>  
</RSSharePointList>  
```  
  
 Tutti i campi per l'elenco Attività vengono restituiti nei risultati della query.  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parametri  
 I parametri non sono supportati da questa estensione per i dati.  
  
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

[Parametri di report](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtro, raggruppamento e ordinamento di dati](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Espressioni &#40;Generatore report e SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
