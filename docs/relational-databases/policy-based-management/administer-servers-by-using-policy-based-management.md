---
title: Amministrazione di server tramite la gestione basata su criteri
description: Informazioni su come usare la gestione basata su criteri per gestire una o più istanze di SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- facet See facets
- Declarative Management Framework See Policy-Based Management
- surface area configuration [SQL Server], Policy-Based Management
- Policy-Based Management
- facets [Policy-Based Management]
- Policy-Based Management, administering
- conditions [Policy-Based Management]
- facets [Policy-Based Management], about facets
- PolicyAdministratorRole role
ms.assetid: ef2a7b3b-614b-405d-a04a-2464a019df40
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 721f2d692d95f8bdccbdf351193fb63c36ad146b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345527"
---
# <a name="administer-servers-by-using-policy-based-management"></a>Amministrazione di server tramite la gestione basata su criteri
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   La gestione basata su criteri è un sistema basato su criteri per la gestione di una o più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usarla per creare condizioni contenenti espressioni. Creare quindi i criteri per l'applicazione delle condizioni a oggetti di destinazione del database.  

Gli amministratori di database potrebbero, ad esempio, volersi assicurare che in alcuni server non venga abilitato Posta elettronica database e quindi creano una condizione e un criterio che imposta tale opzione del server. 
   
 > **IMPORTANTE** I criteri possono influire sul funzionamento di alcune funzionalità. La funzionalità Change Data Capture e la replica transazionale, ad esempio, utilizzano entrambe la tabella systranschemas, che non dispone di un indice. Se si abilitano criteri in base ai quali tutte le tabelle devono includere un indice, l'applicazione della conformità ai criteri comporterà il mancato funzionamento di tali funzionalità.  
  
 Usare SQL Server Management Studio per creare e gestire i criteri per:
  
1.  Selezionare un facet della gestione basata su criteri che contiene le proprietà da configurare.  
  
2.  Definire una condizione che specifica lo stato di un facet di gestione.  
  
3.  Definire i criteri contenenti la condizione, condizioni aggiuntive per filtrare i set di destinazioni e la modalità di valutazione.  
  
4.  Verificare se un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è conforme ai criteri.  
  
 Per i criteri con errori, in Esplora oggetti viene restituito un avviso di integrità critica come icona rossa accanto alla destinazione e ai nodi di livello superiore nell'albero.  
  
> **NOTA:** durante il calcolo del set di oggetti per i criteri, per impostazione predefinita gli oggetti di sistema sono esclusi.  Ad esempio, se il set di oggetti dei criteri si riferisce a tutte le tabelle, i criteri non verranno applicati alle tabelle di sistema. Se gli utenti desiderano valutare i criteri negli oggetti di sistema, possono aggiungere in modo esplicito questi oggetti al set di oggetti. Tuttavia, sebbene tutti i criteri siano supportati per la modalità di valutazione **Controllo su pianificazione** , per motivi di prestazioni, non tutti i criteri con i set di oggetti arbitrari sono supportati per la modalità di valutazione **Controllo su modifiche** . Per altre informazioni, vedere [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes)  
  
## <a name="three-policy-based-management-components"></a>Tre componenti della gestione basata su criteri  
 Nella gestione basata su criteri sono inclusi tre componenti:  
  
-   Gestione dei criteri. Gli amministratori dei criteri creano i criteri.  
  
-   Amministrazione esplicita. Gli amministratori selezionano una o più destinazioni gestite e verificano in modo esplicito che tali destinazioni siano conformi a criteri specifici oppure operano in modo esplicito per rendere le destinazioni conformi ai criteri.  
  
-   Modalità di valutazione. Sono disponibili quattro modalità di valutazione, tre delle quali possono essere automatizzate:  
  
    -   **Su richiesta**. Questa modalità consente di valutare i criteri quando questi vengono specificati direttamente dall'utente.  
  
    -   **Su modifica: impedisci esecuzione**. Questa modalità automatica utilizza trigger DDL per impedire violazioni dei criteri.  
  
        > **IMPORTANTE** Se l'opzione della configurazione del server relativa ai trigger nidificati è disabilitata, la modalità **Su modifica: impedisci esecuzione** non funzionerà correttamente. La gestione basata su criteri si basa su trigger DDL per rilevare ed eseguire il rollback di operazioni DDL non conformi ai criteri che utilizzano questa modalità di valutazione. La rimozione dei trigger DDL della gestione basata su criteri o la disabilitazione dei trigger nidificati provocherà un comportamento imprevisto o la mancata esecuzione di questa modalità di valutazione.  
  
    -   **Su modifica: solo log**. Questa modalità automatica utilizza la notifica degli eventi per valutare i criteri quando viene apportata una modifica importante.  
  
    -   **Su pianificazione**. Questa modalità automatica utilizza un processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per valutare periodicamente i criteri  
  
     Quando i criteri automatici non sono abilitati, la gestione basata su criteri non influisce sulle prestazioni del sistema.  
  
## <a name="terms"></a>Termini  
 **Destinazione gestita della gestione basata su criteri** Entità gestite tramite la gestione basata su criteri, ad esempio un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], un database, una tabella o un indice. Tutte le destinazioni in un'istanza del server formano una gerarchia di destinazioni. Un set di destinazioni è il set risultante dall'applicazione di un set di filtri di destinazione alla gerarchia di destinazioni, ad esempio tutte le tabelle del database di proprietà dello schema HumanResources.  
  
 **Facet della gestione basata su criteri** Set di proprietà logiche che definiscono il comportamento o le caratteristiche di certi tipi di destinazioni gestite. Il numero e le caratteristiche delle proprietà vengono incorporati nel facet e possono essere aggiunti o rimossi solo dal creatore del facet. Un tipo di destinazione può implementare uno o più facet di gestione e un facet di gestione può essere implementato da uno o più tipi di destinazione. Alcune proprietà di un facet possono essere valide solo per una versione specifica.  
  
 **Condizione della gestione basata su criteri**  
 Espressione booleana che specifica un set di stati consentiti per una destinazione gestita tramite la gestione basata su criteri in relazione a un facet di gestione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di osservare le regole di confronto in caso di valutazione di una condizione. Quando le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non corrispondono esattamente a quelle di Windows, testare la condizione per determinare come risolvere i conflitti dell'algoritmo.  
  
 **Criteri di gestione basata su criteri**  
 Condizione della gestione basata su criteri e comportamento previsto, ad esempio la modalità di valutazione, i filtri di destinazione e la pianificazione. I criteri possono contenere solo una condizione. I criteri possono essere abilitati o disabilitati. I criteri vengono archiviati nel database msdb.  
  
 **Categoria di criteri della gestione basata su criteri**  
 Categoria definita dall'utente per semplificare la gestione dei criteri. Gli utenti possono classificare i criteri in diverse categorie. I criteri appartengono esclusivamente a un'unica categoria di criteri. Le categorie di criteri si applicano a database e server. Applicare le condizioni seguenti a livello di database:  
  
-   I proprietari del database possono sottoscrivere un set di categorie di criteri per un database.  
  
-   Solo i criteri delle categorie sottoscritte possono controllare un database.  
  
-   Tutti i database sottoscrivono implicitamente la categoria di criteri predefinita.  
  
 A livello di server, le categorie di criteri possono essere applicate a tutti i database.  
  
 **Criteri validi**  
 Vengono considerati validi per una destinazione i criteri che la controllano. I criteri sono validi in relazione a una destinazione solo se si verificano tutte le condizioni seguenti:  
  
-   I criteri sono abilitati.  
  
-   La destinazione appartiene al set di destinazioni dei criteri.  
  
-   La destinazione o uno dei predecessori delle destinazioni sottoscrive il gruppo di criteri che contiene i criteri.  
  
## <a name="links-to-specific-tasks"></a>Collegamenti a specifiche attività 

 - [Archiviare i criteri della gestione basata su criteri](policy-based-management-storage.md)
 - [Configurare avvisi per notificare agli amministratori eventuali errori dei criteri](../../relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md)
 - [Creare una nuova condizione della gestione basata su criteri](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md)
 - [Eliminare una condizione della gestione basata su criteri](../../relational-databases/policy-based-management/delete-a-policy-based-management-condition.md)
 - [Visualizzare o modificare le proprietà di una condizione della gestione basata su criteri](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)
 - [Esportare i criteri della gestione basata su criteri](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)
 - [Importare i criteri della gestione basata su criteri](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)
 - [Valutare i criteri della gestione basata su criteri da un oggetto](../../relational-databases/policy-based-management/evaluate-a-policy-based-management-policy-from-an-object.md)
 - [Usare i facet della gestione basata su criteri](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)
 - [Monitorare e applicare le procedure consigliate tramite la gestione basata su criteri](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)

## <a name="see-also"></a>Vedere anche  
 
 - [Esercitazione: Creare e applicare criteri Disattivata per impostazione predefinita](lesson-1-create-and-apply-an-off-by-default-policy.md)
 - [Esercitazione: Creare e applicare criteri per gli standard di denominazione](lesson-2-create-and-apply-a-naming-standards-policy.md)
 - [Viste di Gestione basata su criteri &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)