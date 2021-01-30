---
description: Ottimizzazione dei filtri di riga con parametri
title: Ottimizzare i filtri di riga con parametri | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- precomputed partitions [SQL Server replication]
- filters [SQL Server replication], parameterized
- merge replication precomputed partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], optimizing
ms.assetid: 49349605-ebd0-4757-95be-c0447f30ba13
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c8b58c7cf7ffe2b56feb8f59df9b0adef857ab02
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076659"
---
# <a name="optimize-parameterized-row-filters"></a>Ottimizzazione dei filtri di riga con parametri
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si descrive come ottimizzare i filtri di riga con parametri in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
-   **Per ottimizzare i filtri di riga con parametri, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Quando si utilizzano filtri con parametri, è possibile controllare in che modo i filtri vengono elaborati dalla replica di tipo merge specificando l'opzione **use partition groups** o **keep partition changes** durante la creazione di una pubblicazione. Queste opzioni consentono di migliorare le prestazioni di sincronizzazione delle pubblicazioni con gli articoli filtrati tramite l'archiviazione di metadati aggiuntivi nel database di pubblicazione. È possibile controllare la modalità di condivisione dei dati tra i Sottoscrittori impostando l'opzione **partition options** durante la creazione di un articolo. Per altre informazioni su tali requisiti, vedere [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Con i Sottoscrittori [!INCLUDE[ssEW](../../../includes/ssew-md.md)], keep_partition_changes deve essere impostato su true per assicurarsi che le eliminazioni vengano propagate correttamente. Se impostato su false, nel Sottoscrittore potrebbero essere presenti più righe rispetto al previsto.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 È possibile utilizzare le impostazioni seguenti per ottimizzare i filtri di riga con parametri:  
  
 **Partition Options**  
 Impostare questa opzione nella pagina **Proprietà** della finestra di dialogo **Proprietà articolo - \<Article>** o nella finestra di dialogo **Aggiungi filtro**. Entrambe le finestre di dialogo sono disponibili nella Creazione guidata nuova pubblicazione e nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . La finestra di dialogo **Proprietà articolo - \<Article>** consente di specificare altri valori per questa opzione, che non sono disponibili nella finestra di dialogo **Aggiungi filtro**.  
  
 **Pre-calcola partizioni**  
 Per impostazione predefinita, questa opzione viene impostata su **True** se gli articoli nella pubblicazione rispondono a una serie di requisiti. Per altre informazioni su questi requisiti, vedere [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Modificare questa opzione nella pagina **Opzioni sottoscrizione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
  
 **Ottimizza sincronizzazione**  
 Questa opzione deve essere impostata su **True** solo se **Pre-calcola partizioni** viene impostata su **False**. Impostare questa opzione nella pagina **Opzioni sottoscrizione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
  
 Per altre informazioni sull'uso della Creazione guidata nuova pubblicazione e sull'accesso alla finestra di dialogo **Proprietà pubblicazione - \<Publication>** , vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-set-partition-options-in-the-add-filter-or-edit-filter-dialog-box"></a>Per impostare le opzioni delle partizioni nella finestra di dialogo Aggiungi filtro o Modifica filtro  
  
1.  Nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtra righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **Aggiungi** e quindi su **Aggiungi filtro**.  
  
2.  Creare un filtro con parametri. Per altre informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
3.  Selezionare l'opzione corrispondente alla modalità desiderata di condivisione dei dati tra i Sottoscrittori:  
  
    -   **Una riga di questa tabella verrà inviata a più sottoscrizioni**  
  
    -   **Una riga di questa tabella verrà inviata a una sola sottoscrizione**  
  
     Selezionando **Una riga di questa tabella verrà inviata a una sola sottoscrizione** è possibile ottimizzare le prestazioni della replica di tipo merge archiviando ed elaborando una minore quantità di metadati. È tuttavia necessario garantire che i dati vengano partizionati in modo da non consentire la replica di una riga in più Sottoscrittori. Per altre informazioni, vedere la sezione relativa all'impostazione delle opzioni delle partizioni nell'argomento [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  
  
#### <a name="to-set-partition-options-in-the-article-properties---article-dialog-box"></a>Per impostare Opzioni partizioni nella finestra di dialogo Proprietà articolo - \<Article>  
  
1.  Nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare una tabella e quindi fare clic su **Proprietà articolo**.  
  
2.  Fare clic su **Imposta proprietà dell'articolo di tabelle evidenziato** o su **Imposta proprietà di tutti gli articoli di tabelle**.  
  
3.  Nella sezione **Oggetto di destinazione** della scheda **Proprietà** della finestra di dialogo **Proprietà articolo - \<Article>** specificare uno dei seguenti valori per **Opzioni partizioni**:  
  
    -   **Sovrapposte**  
  
    -   **Sovrapposte, non ammesse modifiche dei dati fuori partizione**  
  
    -   **Non sovrapposte, sottoscrizione unica**  
  
    -   **Non sovrapposte, condivise dalle sottoscrizioni**  
  
     Per ulteriori informazioni su queste opzioni e sulla loro relazione con le opzioni disponibili nelle finestre di dialogo **Aggiungi filtro** e **Modifica filtro** , vedere la sezione relativa all'impostazione delle opzioni partizioni nell'argomento [i filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  
  
#### <a name="to-set-precompute-partitions"></a>Per impostare Pre-calcola partizioni  
  
1.  Nella pagina **Opzioni sottoscrizione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un valore per l'opzione **Pre-calcola partizioni**. Questa proprietà è in sola lettura se:  
  
    -   La pubblicazione non soddisfa i requisiti delle partizioni pre-calcolate.  
  
    -   Non è ancora stato generato lo snapshot per la pubblicazione. In questo caso, l'opzione visualizza il valore **Imposta automaticamente alla creazione di uno snapshot**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-set-optimize-synchronization"></a>Per impostare Ottimizza sincronizzazione  
  
1.  Nella pagina **Opzioni sottoscrizione** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare il valore `True` per l'opzione **Ottimizza sincronizzazione**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Per la definizione delle opzioni di filtraggio per `@keep_partition_changes` e `@use_partition_groups`, vedere [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).  
  
#### <a name="to-specify-merge-filter-optimizations-when-creating-a-new-publication"></a>Per specificare le ottimizzazioni del filtro di merge durante la creazione di una nuova pubblicazione  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Specificare `@publication` e il valore `true` per uno dei parametri seguenti:  
  
    -   `@use_partition_groups`: massima ottimizzazione delle prestazioni, purché gli articoli siano conformi ai requisiti per le partizioni precalcolate. Per altre informazioni, vedere [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   `@keep_partition_changes`: usare questa ottimizzazione se non è possibile usare partizioni precalcolate.  
  
2.  Aggiungere un processo snapshot per la pubblicazione. Per altre informazioni, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md).  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)specificando i parametri seguenti:  
  
    -   `@publication`: nome della pubblicazione ottenuto al passaggio 1.  
  
    -   `@article`: nome dell'articolo.  
  
    -   `@source_object`: oggetto di database da pubblicare.  
  
    -   `@subset_filterclause`: clausola di filtro con parametri facoltativa usata per filtrare l'articolo in senso orizzontale.  
  
    -   `@partition_options`: opzioni delle partizioni per l'articolo filtrato.  
  
4.  Ripetere il passaggio 3 per ogni articolo della pubblicazione.  
  
5.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) per definire un filtro di join tra due articoli. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
#### <a name="to-view-and-modify-merge-filter-behaviors-for-an-existing-publication"></a>Per visualizzare e modificare i comportamenti del filtro di merge per una pubblicazione esistente  
  
1.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), specificando `@publication`. Tenere presente il valore di `keep_partition_changes` e `use_partition_groups` nel set di risultati.  
  
2.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Specificare il valore `use_partition_groups` per `@property` e `true` o `false` per `@value`.  
  
3.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Specificare il valore `keep_partition_changes` per `@property` e `true` o `false` per `@value`.  
  
    > [!NOTE]  
    >  Quando si abilita `keep_partition_changes`, è necessario prima disabilitare `use_partition_groups` e specificare il valore `1` per `@force_reinit_subscription`.  
  
4.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore `partition_options` per `@property` e il valore appropriato per `@value`. Per le definizioni di queste opzioni di filtro, vedere [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) .  
  
5.  (Facoltativo) Avviare l'agente snapshot per rigenerare lo snapshot se necessario. Per informazioni sulle modifiche necessarie per la generazione di un nuovo snapshot, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Generare automaticamente un set di filtri di join tra gli articoli di merge &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)   
 [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
