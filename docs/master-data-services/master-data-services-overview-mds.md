---
title: Panoramica
description: Informazioni sulle principali funzionalità di gestione e organizzazione dei dati di Master Data Services. Master Data Services consente di gestire un set di dati master.
ms.custom: ''
ms.date: 02/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
keywords:
- what is master data
helpviewer_keywords:
- Master Data Services, overview
- Master Data Services
ms.assetid: 8a4c28b1-6061-4850-80b6-132438b8c156
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1ddbda633eb54060ea63b8bdd5f17ca3142c4ec2
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833836"
---
# <a name="master-data-services-overview-mds"></a>Panoramica di Master Data Services (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

In questo argomento sono descritte le principali funzionalità di organizzazione e gestione dei dati di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. 
  
 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] consente di gestire un set di dati master dell'organizzazione. È possibile organizzare i dati in modelli, creare regole per l'aggiornamento dei dati e controllare chi aggiorna i dati. Con Excel è possibile condividere il set di dati master con altri utenti nell'organizzazione. 
  
 >  Per una descrizione dell'architettura di [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] , vedere l'articolo [Master Data Services -- The Basics](https://www.simple-talk.com/sql/database-delivery/master-data-services-basics) (Master Data Services -- Nozioni di base) in simple-talk.com. Per informazioni sulle nuove funzionalità di [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] , vedere Novità [di Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)  
   **Per istruzioni su come installare [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], impostare il database e il sito Web e distribuire i modelli di esempio, vedere** [Master Data Services](../master-data-services/master-data-services-installation-and-configuration.md).  
  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]il modello corrisponde al contenitore di livello più alto nella struttura dei dati master. Un modello viene creato per gestire gruppi di dati simili, ad esempio i dati di prodotto online. Un modello contiene una o più entità e le entità contengono i membri che corrispondono ai record di dati. Un'entità è simile a una tabella.  
  
 Ad esempio, il modello di prodotto online può contenere entità come Product, Color e Style. L'entità Color può contenere membri per i colori rosso, argento e nero.  
  
 ![Entità Color](../master-data-services/media/mds-productmodel-colorentity-composite.png "Entità Color")  
  
 I modelli contengono anche attributi definiti all'interno delle entità. Un attributo contiene i valori che consentono di descrivere i membri dell'entità. Gli attributi possono essere di tipo freeform o basati su dominio.  Un attributo basato su dominio contiene valori popolati dai membri di un'entità e che possono essere usati come valori di attributo per altre entità.  
  
 Ad esempio, l'entità Product potrebbe avere attributi freeform per il costo e il peso. È presente un attributo basato su dominio per il colore ![numero 1](../master-data-services/media/mds-number1.png "Numero 1") che contiene i valori popolati dai membri dell'entità Color. Questo elenco principale di colori viene utilizzato come valore di attributo per l'entità Product ![numero 2](../master-data-services/media/mds-number2.png "Numero 2").  
  
 ![Attributo basato su dominio per il colore](../master-data-services/media/mds-productentity-color-domainattribute.png "Attributo basato su dominio per il colore")  
  
 Le gerarchie derivate provengono dalle relazioni tra entità in un modello. Si tratta delle relazioni tra attributi basati su dominio. Nel modello di prodotto, ad esempio, è possibile avere una gerarchia derivata dal colore ![numero 1](../master-data-services/media/mds-number1.png "Numero 1") che deriva dalla relazione tra le entità Color ![numero 2](../master-data-services/media/mds-number2.png "Numero 2") e ![numero prodotto 3](../master-data-services/media/mds-number3.png "Numero 3") .  
  
 ![Gerarchia derivata da colore](../master-data-services/media/mds-derivedhierarchy.png "Gerarchia derivata da colore")  
  
 Dopo aver definito una struttura di base per i dati, è possibile iniziare ad aggiungere record di dati (membri) usando la funzionalità di importazione. Caricare i dati nelle tabelle di staging, convalidarli usando le regole business e caricarli nelle tabelle MDS.  Le regole business possono essere usate anche per impostare i valori dell'attributo.  
  
 La tabella seguente descrive le attività principali di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Se non diversamente specificato, tutte le procedure riportate di seguito richiedono privilegi di amministratore di modelli. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
> [!NOTE]  
>  Si consiglia di completare le attività seguenti in un ambiente di test e di utilizzare i dati di esempio forniti al momento dell'installazione di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Per altre informazioni, vedere [Distribuzione di modelli &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md).  
  
|Action|Dettagli|Argomenti correlati|  
|------------|-------------|--------------------|  
|Creare un modello|Quando si crea un modello, questo viene considerato come VERSION_1.|[Modelli &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)<br /><br /> [Creare un modello &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)|  
|Creare entità|Creare tutte le entità che saranno necessarie per contenere i membri.|[Entità &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)<br /><br /> [Creare un'entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)|  
|Creare entità da utilizzare come attributi basati su dominio|Per creare un attributo basato su dominio, creare innanzitutto l'entità per popolare l'elenco di valori di attributo.|[Attributi basati su dominio &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)<br /><br /> [Creare un attributo basato su dominio &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)|  
|Creare attributi per le entità|Creare attributi per descrivere membri. Gli attributi Name e Code vengono inclusi automaticamente in tutte le entità e non possono essere rimossi. È possibile creare altri attributi in formato libero che conterranno testo, date, numeri o file.|[Attributi &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)<br /><br /> [Creare un attributo di testo &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)<br /><br /> [Creare un attributo numerico &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md)<br /><br /> [Creare un attributo di data &#40;Master Data Services&#41;](../master-data-services/create-a-date-attribute-master-data-services.md)<br /><br /> [Creare un attributo di collegamento &#40;Master Data Services&#41;](../master-data-services/create-a-link-attribute-master-data-services.md)<br /><br /> [Creare un attributo di file &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)|  
|Creare gruppi di attributi|Se si dispone di più di quattro o cinque attributi per un'entità, si consiglia di creare gruppi di attributi. Questi gruppi corrispondono alle schede visualizzate sopra la griglia in **Esplora** e facilitano la navigazione grazie al raggruppamento di attributi in singole schede.|[Gruppi di attributi &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)<br /><br /> [Creare un gruppo di attributi &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)|  
|Importare membri per le entità di supporto|Importare i dati per le entità di supporto usando il processo di gestione temporanea. Per il modello Product potrebbe trattarsi di colori o dimensioni. È inoltre possibile creare membri manualmente.<br /><br /> <br /><br /> Nota: gli utenti possono creare membri in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] se hanno almeno l'autorizzazione di **Aggiornamento** per l'oggetto modello foglia di un'entità e l'accesso all'area funzionale **Esplora** .|[Panoramica: Importazione di dati da tabelle &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)<br /><br /> [Creare un membro foglia &#40;Master Data Services&#41;](../master-data-services/create-a-leaf-member-master-data-services.md)|  
|Creare e applicare regole di business per garantire la qualità dei dati|Creare e pubblicare regole business per assicurare l'accuratezza dei dati. È possibile utilizzare regole business per:<br /><br /> Impostare valori di attributi predefiniti.<br /><br /> Modificare valori di attributi.<br /><br /> Inviare notifiche tramite posta elettronica quando i dati non superano la convalida delle regole business.|[Regole di business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)<br /><br /> [Creare e pubblicare una regola business &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)<br /><br /> [Convalidare membri specifici rispetto a regole business (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)<br /><br /> [Configurare notifiche di posta elettronica &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)<br /><br /> [Configurare le regole di business per l'invio di notifiche &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|Importare membri per le entità primarie e applicare regole business|Importare i membri per le entità primarie tramite il processo di gestione temporanea. Al termine, convalidare la versione che applica regole business a tutti i membri nella versione del modello.<br /><br /> È quindi possibile correggere gli eventuali problemi di convalida tramite regole business.|[Convalida &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)<br /><br /> [Convalidare una versione usando le regole di business &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)<br /><br /> [Stored procedure di convalida &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md)|  
|Creare gerarchie derivate|Le gerarchie derivate possono essere aggiornate con il mutare delle esigenze aziendali e assicurano che tutti i membri siano rappresentati al livello appropriato.|[Gerarchie derivate &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)<br /><br /> [Creare una gerarchia derivata &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Creare gerarchie esplicite, se necessario|Se si desidera creare gerarchie non basate su livelli, che includano membri da una singola entità, è possibile creare gerarchie esplicite.|[Gerarchie esplicite &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)<br /><br /> [Creare una gerarchia esplicita &#40;Master Data Services&#41;](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|Creare raccolte, se necessario|Se si desidera visualizzare diversi raggruppamenti di membri per la creazione di report o l'analisi e non è necessaria una gerarchia completa, creare una raccolta.<br /><br /> <br /><br /> Nota: gli utenti possono creare raccolte in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] se hanno almeno l'autorizzazione **Aggiornamento** per l'oggetto modello raccolta e l'accesso all'area funzionale **Esplora** .|[Raccolte &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)<br /><br /> [Creare una raccolta &#40;Master Data Services&#41;](../master-data-services/create-a-collection-master-data-services.md)|  
|Creare metadati definiti dall'utente|Per descrivere gli oggetti modello, aggiungere metadati definiti dall'utente al modello. I metadati possono includere il proprietario di un oggetto o l'origine da cui provengono i dati.||  
|Bloccare una versione del modello e assegnare un flag di versione|Bloccare una versione del modello per impedire modifiche ai membri, salvo che vengano apportate da amministratori. Dopo che i dati della versione sono stati convalidati correttamente rispetto a regole business, è possibile eseguire il commit della versione impedendo modifiche ai membri da parte di tutti gli utenti.<br /><br /> Creare e assegnare un flag di versione al modello. I flag consentono a utenti e sistemi di sottoscrizione di identificare la versione di un modello da utilizzare.|[Versioni &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)<br /><br /> [Bloccare una versione &#40;Master Data Services&#41;](../master-data-services/lock-a-version-master-data-services.md)<br /><br /> [Creare un flag di versione &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)|  
|Creare viste sottoscrizioni|Affinché i sistemi di sottoscrizione possano utilizzare i dati master, creare viste sottoscrizioni che creano viste standard nel database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Panoramica: Esportazione di dati &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)<br /><br /> [Creare una vista sottoscrizioni per esportare i dati &#40;Master Data Services&#41;](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|Configurare autorizzazioni utenti e gruppi|Non è possibile copiare autorizzazioni per utenti e gruppi da un ambiente di test a un ambiente di produzione. È tuttavia possibile utilizzare l'ambiente di test per determinare la sicurezza che si desidera utilizzare in produzione.|[Sicurezza &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)<br /><br /> [Aggiungere un gruppo &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)<br /><br /> [Aggiungere un utente &#40;Master Data Services&#41;](../master-data-services/add-a-user-master-data-services.md)|  
  
 Al termine, è possibile distribuire il modello, con o senza dati, all'ambiente di produzione. Per altre informazioni, vedere [Distribuzione di modelli &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md).  
  
  

