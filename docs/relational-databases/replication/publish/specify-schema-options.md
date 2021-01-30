---
description: Specificare le opzioni dello schema per la replica di SQL Server
title: Specificare le opzioni dello schema per la replica di SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- schemas [SQL Server replication], options
- articles [SQL Server replication], transactional replication options
- articles [SQL Server replication], merge replication options
- articles [SQL Server replication], schema options
ms.assetid: 1f85a479-bd6e-4023-abf7-7435a7e5b567
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53c7c3b803a16fd7ee95f35f33ee2439deb86199
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076619"
---
# <a name="specify-schema-options-for-sql-server-replication"></a>Specificare le opzioni dello schema per la replica di SQL Server
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento si illustra come impostare le opzioni dello schema in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Quando si pubblica una tabella o una vista, è possibile controllare le opzioni di creazione degli oggetti che vengono replicate per l'oggetto pubblicato. È possibile impostare queste opzioni quando viene creato l'articolo ed è possibile modificarle anche successivamente. Se queste opzioni non vengono specificate in modo esplicito per un articolo, verrà definito un set predefinito di opzioni.  
  
> [!NOTE]  
>  Le opzioni predefinite dello schema disponibili quando si utilizzano le stored procedure di replica possono essere diverse dalle opzioni predefinite utilizzate quando gli articoli vengono aggiunti tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
-   **Per specificare le opzioni dello schema tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se si modificano le opzioni dello schema dopo la creazione di una pubblicazione, è necessario generare un nuovo snapshot.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Per un elenco completo delle opzioni dello schema, vedere il parametro `@schema_option` di [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) e [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Specificare le opzioni dello schema, ad esempio se copiare i vincoli e i trigger nei sottoscrittori, nella scheda **Proprietà** della finestra di dialogo **Proprietà articolo - \<Article>** . Questa scheda è disponibile nella Creazione guidata nuova pubblicazione e nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-schema-options"></a>Per specificare le opzioni dello schema  
  
1.  Nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un articolo e quindi fare clic su **Proprietà articolo**.  
  
2.  Selezionare gli articoli a cui si applicano le modifiche delle opzioni dello schema:  
  
    -   Fare clic su **Imposta proprietà dell'articolo di \<ObjectType> evidenziato** per aprire la finestra di dialogo **Proprietà articolo - \<ObjectName>** . Le modifiche apportate alle proprietà in questa finestra di dialogo vengono applicate solo all'oggetto evidenziato nel riquadro degli oggetti nella pagina **Articoli**.  
  
    -   Fare clic su **Imposta proprietà di tutti gli articoli di \<ObjectType>** per aprire la finestra di dialogo **Proprietà di tutti gli articoli \<ObjectType>** . Le modifiche apportate alle proprietà in questa finestra di dialogo vengono applicate a tutti gli oggetti del tipo indicato nel riquadro degli oggetti all'interno della pagina **Articoli**, inclusi quelli non ancora selezionati per la pubblicazione.  
  
        > [!NOTE]  
        >  Le modifiche apportate alle proprietà nella finestra di dialogo **Proprietà di tutti gli articoli \<ObjectType>** sostituiscono tutte le modifiche eseguite precedentemente nella finestra di dialogo **Proprietà articolo - \<ObjectName>** . Se ad esempio si desidera impostare alcuni valori predefiniti per tutti gli articoli di un tipo di oggetto e, al contempo, alcune proprietà per singoli oggetti, è necessario impostare innanzitutto i valori predefiniti per tutti gli articoli, quindi le proprietà relative ai singoli oggetti.  
  
3.  Nelle sezioni **Copia oggetti e impostazioni nel Sottoscrittore** e **Oggetto di destinazione** della scheda **Proprietà** della finestra di dialogo **Proprietà articolo - \<Article>** specificare i valori per le opzioni.  
  
4.  Se necessario, modificare le proprietà e quindi fare clic su **OK**.  
  
5.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Le opzioni dello schema vengono specificate come valore esadecimale che corrisponde al risultato [| (OR bit per bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) di una o più opzioni. Per ulteriori informazioni, vedere [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) e [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
> [!NOTE]  
>  È necessario convertire i valori delle opzioni dello schema da **binario** a **int** prima di eseguire un'operazione bit per bit. Per altre informazioni, vedere [CAST and CONVERT &#40;Transact-SQL&#41;](../../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-snapshot-or-transactional-publication"></a>Per specificare le opzioni dello schema durante la definizione di un articolo per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, l'oggetto di database da pubblicare per `@source_object`, il tipo di oggetto di database per `@type` e il risultato [| (OR bit per bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) di una o più opzioni dello schema per `@schema_option`. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-merge-publication"></a>Per specificare le opzioni dello schema durante la definizione di un articolo per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, l'oggetto di database da pubblicare per `@source_object` e il risultato [| (OR bit per bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) di una o più opzioni dello schema per `@schema_option`. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Per modificare le opzioni dello schema per un articolo esistente in una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication` e il nome dell'articolo per `@article`. Tenere presente il valore della colonna `schema_option` nel set di risultati.  
  
2.  Eseguire un'operazione [& (AND bit per bit)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) usando il valore del passaggio 1 e il valore dell'opzione dello schema desiderata per determinare se l'opzione è impostata.  
  
    -   Se il risultato è **0**, l'opzione non è impostata.  
  
    -   Se il risultato corrisponde al valore dell'opzione, l'opzione è già impostata.  
  
3.  Se l'opzione non è impostata, eseguire un'operazione [| (OR bit per bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) utilizzando il valore del passaggio 1 e il valore dell'opzione dello schema desiderata.  
  
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, il valore `schema_option` per `@property` e il risultato esadecimale del passaggio 3 per `@value`.  
  
5.  Eseguire l'agente snapshot per generare un nuovo snapshot. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-merge-publication"></a>Per modificare le opzioni dello schema per un articolo esistente in una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication` e il nome dell'articolo per `@article`. Si noti il valore della colonna **schema_option** nel set di risultati.  
  
2.  Eseguire un'operazione [& (AND bit per bit)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) usando il valore del passaggio 1 e il valore dell'opzione dello schema desiderata per determinare se l'opzione è impostata.  
  
    -   Se il risultato è **0**, l'opzione non è impostata.  
  
    -   Se il risultato corrisponde al valore dell'opzione, l'opzione è già impostata.  
  
3.  Se l'opzione non è impostata, eseguire un'operazione [| (OR bit per bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) utilizzando il valore del passaggio 1 e il valore dell'opzione dello schema desiderata.  
  
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, il valore `schema_option` per `@property` e il risultato esadecimale del passaggio 3 per `@value`.  
  
5.  Eseguire l'agente snapshot per generare un nuovo snapshot. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
