---
description: Guida di riferimento ai metodi per il tipo di dati hierarchyid
title: hierarchyid (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- hierarchyid
- hierarchyid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Hierarchy data type
- hierarchyid data type
ms.assetid: 69b756e0-a1df-45b3-8a24-6ded8658aefe
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b11771fe7c2e73854a2214e7218cff1bfda1250f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165150"
---
# <a name="hierarchyid-data-type-method-reference"></a>Guida di riferimento ai metodi per il tipo di dati hierarchyid
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il tipo di dati **hierarchyid** è un tipo di dati di sistema di lunghezza variabile. Usare **hierarchyid** per rappresentare la posizione in una gerarchia. Una colonna di tipo **hierarchyid** non rappresenta automaticamente un albero. È compito dell'applicazione generare e assegnare i valori **hierarchyid** in maniera tale che la relazione desiderata tra le righe sia riflessa nei valori.
  
Un valore del tipo di dati **hierarchyid** rappresenta una posizione in un albero gerarchico. I valori per **hierarchyid** hanno le proprietà seguenti:
  
-   Estremamente compresso  
     Il numero medio di bit richiesto per rappresentare un nodo in un albero con *n* nodi dipende dal fanout medio, ovvero il numero medio di elementi figlio di un nodo. Per i fanout di piccole dimensioni (0-7), la dimensione è approssimativamente 6\*logA *n* bit, dove A è il fanout medio. Un nodo in una gerarchia organizzativa di 100.000 persone con un fanout medio di 6 livelli richiede circa 38 bit. Viene arrotondato a 40 bit, o 5 byte, per l'archiviazione.  
-   Il confronto avviene in ordine di scorrimento in profondità  
     Dati due valori **hierarchyid****a** e **b**, **a<b** indica che a precede b nell'attraversamento del primo livello di profondità dell'albero. Gli indici sui tipi di dati **hierarchyid** sono in ordine di scorrimento della profondità e i nodi l'uno vicino all'altro nell'attraversamento del primo livello di profondità della struttura sono archiviati l'uno vicino all'altro. Ad esempio, i figli di un record sono archiviati adiacenti al record specifico. Per altre informazioni, vedere [Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md).  
-   Supporto per eliminazioni e inserimenti arbitrari  
     Usando il metodo [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md) , è sempre possibile generare un elemento di pari livello a destra o a sinistra di un nodo oppure tra due elementi di pari livello. La proprietà del confronto è gestita quando un numero arbitrario di nodi viene inserito o eliminato dalla gerarchia. La maggior parte degli inserimenti e delle eliminazioni mantiene la proprietà di compattezza. Tuttavia, gli inserimenti tra due nodi produrranno i valori hierarchyid con una rappresentazione leggermente meno compatta.  
-   La codifica usata nel tipo **hierarchyid** è limitata a 892 byte. I nodi che hanno troppi livelli nella rappresentazione per rientrare in 892 byte non possono di conseguenza essere rappresentati dal tipo **hierarchyid**.  
  
Il tipo **hierarchyid** è disponibile per i client CLR come tipo di dati **SqlHierarchyId**.
  
## <a name="remarks"></a>Osservazioni  
Il tipo **hierarchyid** codifica logicamente le informazioni su un solo nodo in un albero gerarchico codificando il percorso dalla radice dell'albero al nodo. Tale percorso è rappresentato logicamente come una sequenza di etichette dei nodi di tutti gli elementi figlio visitati dopo la radice. La rappresentazione inizia con una barra. Un percorso che visita solo la radice è rappresentato da una singola barra. Per i livelli sotto la radice, ogni etichetta è codificata come una sequenza di numeri interi separata dai punti. Il confronto tra gli elementi figlio viene eseguito confrontando le sequenze di numeri interi separati da punti in base all'ordinamento del dizionario. Ogni livello è seguito da una barra. Una barra separa quindi i padri dai figli. Ad esempio, gli elementi seguenti sono percorsi **hierarchyid** validi rispettivamente di lunghezza di 1, 2, 2, 3 e 3 livelli:
  
-   /  
  
-   /1/  
  
-   /0.3.-7/  
  
-   /1/3/  
  
-   /0.1/0.2/  
  
I nodi possono essere inseriti in qualsiasi posizione. I nodi inseriti dopo **/1/2/** ma prima di **/1/3/** possono essere rappresentati come **/1/2.5/**. Nodi inseriti prima di 0 sono rappresentati logicamente come numero negativo. Ad esempio, un nodo che precede **/1/1/** può essere rappresentato come **/1/-1/**. Nei nodi non possono essere presenti zero iniziali. Ad esempio, **/1/1.1/** è valido, mentre **/1/1.01/** non è valido. Per evitare errori, inserire nodi usando il metodo [GetDescendant](../../t-sql/data-types/getdescendant-database-engine.md).
  
## <a name="data-type-conversion"></a>Conversione dei tipi di dati
È possibile convertire il tipo di dati **hierarchyid** in altri tipi di dati come segue:
-   Usare il metodo [ToString()](../../t-sql/data-types/tostring-database-engine.md) per convertire il valore **hierarchyid** nella rappresentazione logica come tipo di dati **nvarchar(4000)**.  
-   Usare [Read ()](../../t-sql/data-types/read-database-engine.md) e [Write ()](../../t-sql/data-types/write-database-engine.md) per convertire **hierarchyid** in **varbinary**.  
-   Per trasmettere i parametri **hierarchyid** tramite SOAP, eseguire prima il cast dei parametri come stringhe.  
  
## <a name="upgrading-databases"></a>Aggiornamento di database
Quando un database viene aggiornato a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], il nuovo assembly e il tipo di dati **hierarchyid** saranno installati automaticamente. Le regole di preparazione aggiornamento rilevano qualsiasi tipo di utente o assembly con nomi in conflitto. La preparazione aggiornamento indicherà di rinominare qualsiasi assembly in conflitto e di rinominare eventuali tipi in conflitto o di utilizzare nomi in due parti nel codice che si riferiscano allo specifico tipo di utente preesistente.
  
Se un aggiornamento del database rileva un assembly utente con nome in conflitto, rinominerà automaticamente l'assembly e metterà il database in modalità sospetta.
  
Se durante l'aggiornamento esiste un utente con nome in conflitto, non viene eseguita alcuna operazione. Dopo l'aggiornamento, esisteranno sia il tipo di utente obsoleto che il nuovo tipo di sistema. Il tipo di utente sarà disponibile solo tramite nomi in due parti.
  
## <a name="using-hierarchyid-columns-in-replicated-tables"></a>Utilizzo di colonne hierarchyid nelle tabelle replicate
Le colonne di tipo **hierarchyid** possono essere usate su qualsiasi tabella replicata. I requisiti per l'applicazione dipendono dal tipo di replica direzionale o bidirezionale e dalla versioni del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato.
  
### <a name="one-directional-replication"></a>Replica direzionale
La replica direzionale include la replica snapshot, la replica transazionale e la replica di tipo merge in cui le modifiche non sono apportate al Sottoscrittore. Il modo in cui le colonne **hierarchyid** funzionano con una replica direzionale dipende dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che il sottoscrittore sta eseguendo.
-   Un server di pubblicazione [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] può replicare colonne **hierarchyid** in un sottoscrittore [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]senza alcuna speciale considerazione.  
-   Un server di pubblicazione [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] deve convertire le colonne **hierarchyid** per replicarle in un sottoscrittore che sta eseguendo [!INCLUDE[ssEW](../../includes/ssew-md.md)] o una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] e le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supportano le colonne **hierarchyid**. Se si utilizza una di queste versioni, è ancora possibile replicare dati in un Sottoscrittore. A questo scopo, è necessario impostare un'opzione dello schema o il livello di compatibilità della pubblicazione (per replica di tipo merge) in modo che sia possibile convertire la colonna in un tipo di dati compatibile.  
  
L'applicazione di filtri di colonna è supportata in entrambi questi scenari. Include filtri per l'esclusione di colonne **hierarchyid**. L'applicazione di filtri di riga è supportata se il filtro non include una colonna **hierarchyid**.
  
### <a name="bi-directional-replication"></a>Replica bidirezionale
La replica bidirezionale include la replica transazionale con aggiornamento di sottoscrizioni, la replica transazionale peer-to-peer e la replica di tipo merge in cui le modifiche sono apportate al Sottoscrittore. La replica consente di configurare una tabella con colonne **hierarchyid** per replica bidirezionale. Tenere presente i requisiti e considerazioni indicati di seguito.
-   Il server di pubblicazione e tutti i Sottoscrittori devono eseguire [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
-   La replica esegue la replica dei dati come byte e non esegue alcuna convalida al fine di assicurare l'integrità della gerarchia.  
-   La gerarchia delle modifiche apportate all'origine (Sottoscrittore o server di pubblicazione) non viene mantenuta quando le modifiche vengono replicate sulla destinazione.  
-   I valori hash per le colonne **hierarchyid** sono specifici al database nel quale sono generati. Pertanto, lo stesso valore potrebbe essere generato sul server di pubblicazione e sul Sottoscrittore, ma potrebbe essere applicato a righe diverse. La replica non verifica questa condizione e non è disponibile nessuna modalità integrata per partizionare i valori della colonna **hierarchyid** analogamente a quanto accade per le colonne IDENTITY. Le applicazioni devono utilizzare vincoli o gli altri meccanismi per evitare che tali conflitti passino inosservati.  
-   È possibile che le righe inserite nel Sottoscrittore saranno rese orfane. La riga padre della riga inserita può essere stata eliminata sul server di pubblicazione. Questo non consente di rilevare un conflitto quando una riga del Sottoscrittore viene inserita al server di pubblicazione.  
-   I filtri colonne non possono filtrare colonne **hierarchyid** che non ammettono valori Null: gli inserimenti dal sottoscrittore non verranno eseguiti perché non vi è alcun valore predefinito per la colonna **hierarchyid** nel server di pubblicazione.  
-   L'applicazione di filtri di riga è supportata se il filtro non include una colonna **hierarchyid**.  
  
## <a name="see-also"></a>Vedere anche
[Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[Guida di riferimento ai metodi per il tipo di dati hierarchyid]()
  
