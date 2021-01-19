---
title: Aggiungere o rimuovere articoli (pubblicazione esistente)
description: Informazioni su come aggiungere ed eliminare articoli in pubblicazioni esistenti per SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], dropping
- deleting articles
- removing articles
- dropping articles
- adding articles
- administering replication, articles
- publications [SQL Server replication], adding and dropping articles
- articles [SQL Server replication], adding
ms.assetid: b148e907-e1f2-483b-bdb2-59ea596efceb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 183619e1802b435fbc21c2b44e32848bf7e578de
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171903"
---
# <a name="add-articles-to-and-drop-articles-from-existing-publications"></a>Aggiunta ed eliminazione di articoli a e da pubblicazioni esistenti
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Dopo la creazione di una pubblicazione, è possibile aggiungere ed eliminare articoli. Gli articoli possono essere aggiunti in qualsiasi momento, ma le azioni necessarie per eliminarli dipendono dal tipo di replica e dal momento in cui avviene l'eliminazione.  
  
## <a name="adding-articles"></a>aggiunta di articoli  
 L'aggiunta di un articolo comporta l'aggiunta dell'articolo alla pubblicazione, la creazione di un nuovo snapshot per la pubblicazione e la sincronizzazione della sottoscrizione per applicare lo schema e i dati per il nuovo articolo.  
  
> [!NOTE]
>  Se in una pubblicazione di tipo merge si aggiunge un nuovo articolo dal quale dipende un articolo esistente, è necessario specificare l'ordine di elaborazione per entrambi gli articoli tramite il parametro **\@processing_order** di [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) e [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Si consideri lo scenario seguente: viene pubblicata una tabella, ma non viene pubblicata una funzione a cui fa riferimento la tabella. Se non si pubblica la funzione, la tabella non può essere creata nel Sottoscrittore. Quando si aggiunge la funzione alla pubblicazione, specificare il valore **1** per il parametro **\@processing_order** di **sp_addmergearticle** e il valore **2** per il parametro **\@processing_order** di **sp_changemergearticle**, indicando il nome della tabella per il parametro **\@article**. Questo ordine di elaborazione consente di creare la funzione nel Sottoscrittore prima della tabella dipendente. È possibile utilizzare numeri diversi per ogni articolo, purché il numero per la funzione sia inferiore a quello per la tabella.  
  
1.  Aggiungere uno o più articoli tramite uno dei metodi seguenti:  
  
    -   [Aggiungere ed eliminare articoli in una pubblicazione &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)  
  
    -   [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)  
  
2.  Dopo avere aggiunto un articolo a una pubblicazione, è necessario creare un nuovo snapshot per la pubblicazione. Nel caso si tratti di una pubblicazione di tipo merge con filtri con parametri, sarà necessario creare tutte le partizioni. Tramite l'agente di distribuzione o l'agente di merge vengono quindi copiati lo schema e i dati per il nuovo articolo nel Sottoscrittore, senza reinizializzare l'intera pubblicazione.  
  
    -   Per creare un nuovo snapshot, vedere [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Per creare un nuovo snapshot per una pubblicazione di tipo merge con filtri con parametri, vedere [Creare uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
3.  Dopo la creazione dello snapshot, sincronizzare la sottoscrizione per copiare lo schema e i dati per il nuovo articolo.  

    -   Per sincronizzazione di una sottoscrizione push, vedere [Synchronize a Push Subscription](../../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
    -   Per sincronizzazione di una sottoscrizione pull, vedere [Synchronize a Pull Subscription](../../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
## <a name="dropping-articles"></a>rimozione di articoli  
 Gli articoli possono essere eliminati da una pubblicazione in qualsiasi momento, ma è necessario tenere in considerazione i comportamenti seguenti:  
  
-   L'eliminazione di un articolo da una pubblicazione non comporta la rimozione dell'oggetto dal database di pubblicazione o dell'oggetto corrispondente dal database di sottoscrizione. Usare DROP \<Object> per rimuovere questi oggetti, se necessario. Quando si elimina un articolo correlato ad altri articoli pubblicati tramite vincoli di chiave esterna, è consigliabile eliminare la tabella nel Sottoscrittore manualmente oppure eseguendo uno script su richiesta: specificare uno script che includa le istruzioni DROP \<Object> appropriate. Per altre informazioni, vedere [Eseguire script durante la sincronizzazione &#40;programmazione Transact-SQL della replica&#41;](../../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md).  
  
-   Per pubblicazioni di tipo merge con un livello di compatibilità di 90RTM o superiore, gli articoli possono essere eliminati in qualsiasi momento, ma è necessario un nuovo snapshot. Inoltre:  
  
    -   Se un articolo è un articolo padre in una relazione tra record logici o filtri join, è necessario che prima vengano eliminate le relazioni e di conseguenza sarà necessario eseguire una reinizializzazione.  
  
    -   Se un articolo ha l'ultimo filtro con parametri in una pubblicazione, è necessario reinizializzare le sottoscrizioni.  
  
-   Per pubblicazioni di tipo merge con un livello di compatibilità inferiore a 90RTM, gli articoli possono essere eliminati senza particolari considerazioni prima della sincronizzazione iniziale delle sottoscrizioni. Se un articolo viene eliminato dopo la sincronizzazione di una o più sottoscrizioni, è necessario che le sottoscrizioni vengano eliminate, ricreate e sincronizzate.  
  
-   Per le pubblicazioni snapshot o transazionali, gli articoli possono essere eliminati senza particolari considerazioni prima della creazione delle sottoscrizioni. Se un articolo viene eliminato dopo la creazione di una o più sottoscrizioni, è necessario che le sottoscrizioni vengano eliminate, ricreate e sincronizzate. Per altre informazioni sull'eliminazione delle sottoscrizioni, vedere [Sottoscrivere le pubblicazioni](../../../relational-databases/replication/subscribe-to-publications.md) e [sp_dropsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). **sp_dropsubscription** consente all'utente di eliminare un solo articolo dalla sottoscrizione anziché la sottoscrizione intera.  
  
1.  L'eliminazione di un articolo da una pubblicazione implica l'eliminazione dell'articolo e la creazione di un nuovo snapshot per la pubblicazione. Se si elimina un articolo, lo snapshot corrente non è più valido ed è perciò necessario creare un nuovo snapshot.  
  
    -   Per eliminare un articolo da una pubblicazione, vedere [Aggiungere ed eliminare articoli in una pubblicazione &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md) o [Eliminare un articolo](../../../relational-databases/replication/publish/delete-an-article.md).  
  
2.  Dopo avere eliminato un articolo da una pubblicazione, è necessario creare un nuovo snapshot per la pubblicazione. Nel caso si tratti di una pubblicazione di tipo merge con filtri con parametri, sarà necessario creare tutte le partizioni.  
  
    -   Per creare un nuovo snapshot, vedere [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Per creare un nuovo snapshot per una pubblicazione di tipo merge con filtri con parametri, vedere [Creare uno snapshot per una pubblicazione di tipo merge con filtri con parametri](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 Come evidenziato sopra, in alcuni casi l'eliminazione di un articolo richiede che le sottoscrizioni vengano eliminate, ricreate e quindi sincronizzate. Per altre informazioni, vedere [Sottoscrivere le pubblicazioni](../../../relational-databases/replication/subscribe-to-publications.md) e [Sincronizzare i dati](../../../relational-databases/replication/synchronize-data.md).  
 
 > [!NOTE]
 > **[!INCLUDE[ssSQL15](../../../includes/sssql14-md.md)] Service Pack 2** o versione successiva e **[!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] Service Pack 1** o versione successiva supportano l'eliminazione di un tabella mediante il comando DDL **DROP TABLE** per gli articoli che partecipano alla replica transazionale. Se il comando DLL DROP TABLE è supportato dalla pubblicazione, l'operazione DROP TABLE eliminerà la tabella dalla pubblicazione e dal database. L'agente di lettura log eseguirà un comando di pulizia per il database di distribuzione contenente la tabella eliminata ed eseguirà la pulizia dei metadati del server di pubblicazione. Se l'agente di lettura log non elabora tutti i record del log che fanno riferimento alla tabella eliminata, i nuovi comandi associati a tale tabella verranno ignorati. I record già elaborati verranno recapitati ai database di distribuzione. Possono essere applicati al database Sottoscrittore se l'agente di distribuzione li elabora prima che l'agente di lettura log esegua la pulizia degli articoli obsoleti (eliminati). L'impostazione **default** per tutte le pubblicazioni di replica transazionale non prevede il supporto del comando DDL DROP TABLE. L'articolo [KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1) contiene altri dettagli relativi a questo miglioramento.

  
## <a name="see-also"></a>Vedere anche  
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Apportare modifiche allo schema nei database di pubblicazione](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
