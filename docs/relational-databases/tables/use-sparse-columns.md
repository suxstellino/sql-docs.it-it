---
description: Utilizzo di colonne di tipo sparse
title: Usare le colonne di tipo sparse | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, described
- null columns
- sparse columns
ms.assetid: ea7ddb87-f50b-46b6-9f5a-acab222a2ede
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b2d569c3f37370bd132485e843da005b1769b17
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211538"
---
# <a name="use-sparse-columns"></a>Utilizzo di colonne di tipo sparse

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Le colonne di tipo sparse sono colonne comuni che dispongono di archiviazione ottimizzata per i valori Null. Tali colonne consentono di ridurre i requisiti di spazio per i valori Null aumentando tuttavia l'overhead per il recupero dei valori non Null. È consigliabile utilizzare colonne di tipo sparse quando la quantità di spazio risparmiata è compresa almeno tra il 20% e il 40%. Le colonne di tipo sparse e i set di colonne vengono definiti utilizzando l'istruzione [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 Le colonne di tipo sparse possono essere utilizzate con set di colonne e indici filtrati:  
  
-   Set di colonne  
  
     Le istruzioni INSERT, UPDATE e DELETE possono fare riferimento alle colonne di tipo sparse in base al nome. È tuttavia possibile visualizzare e utilizzare tutte le colonne di tipo sparse di una tabella combinate in una singola colonna XML, denominata set di colonne. Per altre informazioni sui set di colonne, vedere [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md).  
  
-   Indici filtrati  
  
     Poiché le colonne di tipo sparse contengono molte righe con valori Null, sono particolarmente adatte per l'utilizzo di indici filtrati. Un indice filtrato applicato a una colonna di tipo sparse consente di indicizzare solo le righe popolate con valori. In questo modo, viene creato un indice più efficiente e di dimensioni minori. Per altre informazioni, vedere [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 Le colonne di tipo sparse e gli indici filtrati consentono alle applicazioni, ad esempio [!INCLUDE[winSPServ](../../includes/winspserv-md.md)], di archiviare in modo efficiente un elevato numero di proprietà definite dall'utente e accedervi usando [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="properties-of-sparse-columns"></a>Proprietà delle colonne di tipo sparse  
 Le colonne di tipo sparse hanno le caratteristiche seguenti:  
  
-   Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilizza la parola chiave SPARSE nella definizione di una colonna per ottimizzare l'archiviazione dei valori in tale colonna. Di conseguenza, quando il valore della colonna è Null per qualsiasi riga della tabella, i valori non devono essere archiviati.  
  
-   Le viste del catalogo per una tabella contenente colonne di tipo sparse sono identiche a quelle di una tabella tipica. La vista del catalogo sys.columns contiene una riga per ogni colonna della tabella e include un set di colonne, se definito.  
  
-   Le colonne di tipo sparse sono una proprietà del livello dell'archiviazione, piuttosto che la tabella logica. Di conseguenza un'istruzione SELECT...INTO non viene copiata sopra la proprietà della colonna di tipo sparse in una nuova tabella.  
  
-   La funzione COLUMNS_UPDATED restituisce un valore **varbinary** per indicare tutte le colonne aggiornate durante un'azione DML. I bit restituiti dalla funzione COLUMNS_UPDATED vengono impostati come indicato di seguito:  
  
    -   Quando una colonna di tipo sparse viene aggiornata in modo esplicito, il bit corrispondente per tale colonna e il bit per il set di colonne vengono impostati su 1.  
  
    -   Quando un set di colonne viene aggiornato in modo esplicito, il bit per il set di colonne e i bit per tutte le colonne di tipo sparse della tabella vengono impostati su 1.  
  
    -   Per le operazioni di inserimento, tutti i bit vengono impostati su 1.  
  
     Per altre informazioni sui set di colonne, vedere [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md).  
  
 I tipi di dati seguenti non possono essere specificati come SPARSE:  
  
|||  
|-|-|  
|**geography**|**text**|  
|**geometry**|**timestamp**|  
|**image**|**tipi di dati definiti dall'utente**|  
|**ntext**||  
  
## <a name="estimated-space-savings-by-data-type"></a>Risparmio stimato in termini di spazio in base al tipo di dati  
 Le colonne di tipo sparse richiedono una quantità maggiore di spazio di archiviazione per i valori non Null rispetto a quella necessaria per dati identici non contrassegnati come SPARSE. Nelle tabelle seguenti viene illustrato l'utilizzo dello spazio per ogni tipo di dati. La colonna **Percentuale valori Null** indica la percentuale di dati con valore Null necessaria per ottenere un risparmio netto del 40% in termini di spazio.  
  
 **Tipi di dati a lunghezza fissa**  
  
|Tipo di dati|Byte non di tipo sparse|Byte di tipo sparse|Percentuale valori Null|  
|---------------|---------------------|------------------|---------------------|  
|**bit**|0,125|5|98%|  
|**tinyint**|1|5|86%|  
|**smallint**|2|6|76%|  
|**int**|4|8|64%|  
|**bigint**|8|12|52%|  
|**real**|4|8|64%|  
|**float**|8|12|52%|  
|**smallmoney**|4|8|64%|  
|**money**|8|12|52%|  
|**smalldatetime**|4|8|64%|  
|**datetime**|8|12|52%|  
|**uniqueidentifier**|16|20|43%|  
|**date**|3|7|69%|  
  
 **Tipi di dati di lunghezza dipendente dalla precisione**  
  
|Tipo di dati|Byte non di tipo sparse|Byte di tipo sparse|Percentuale valori Null|  
|---------------|---------------------|------------------|---------------------|  
|**datetime2(0)**|6|10|57%|  
|**datetime2(7)**|8|12|52%|  
|**time(0)**|3|7|69%|  
|**time(7)**|5|9|60%|  
|**datetimetoffset(0)**|8|12|52%|  
|**datetimetoffset (7)**|10|14|49%|  
|**decimal/numeric(1,s)**|5|9|60%|  
|**decimal/numeric(38,s)**|17|21|42%|  
|**vardecimal(p,s)**|Usare il tipo **decimal** come stima conservativa.|||  
  
 **Tipi di dati di lunghezza dipendente dai dati**  
  
|Tipo di dati|Byte non di tipo sparse|Byte di tipo sparse|Percentuale valori Null|  
|---------------|---------------------|------------------|---------------------|  
|**sql_variant**|Varia in base al tipo di dati sottostante|||  
|**varchar** o **char**|2*|4*|60%|  
|**nvarchar** o **nchar**|2*|4*+|60%|  
|**varbinary** o **binary**|2*|4*|60%|  
|**xml**|2*|4*|60%|  
|**hierarchyid**|2*|4*|60%|  
  
 *La lunghezza è uguale alla media dei dati contenuti nel tipo, più 2 o 4 byte.  
  
## <a name="in-memory-overhead-required-for-updates-to-sparse-columns"></a>Overhead in memoria necessario per gli aggiornamenti alle colonne di tipo sparse  
 Quando si progettano tabelle con colonne di tipo sparse, ricordare che sono necessari 2 byte aggiuntivi di overhead per ogni colonna di tipo sparse non Null nella tabella quando viene aggiornata una riga. Dato questo requisito di memoria aggiuntivo, gli aggiornamenti potrebbero non riuscire in modo imprevisto con l'errore 576 quando le dimensioni totali della riga, incluso l'overhead di memoria, supera 8019 e nessuna colonna può essere spostata all'esterno della riga.  
  
 Si consideri l'esempio di una tabella contenente 600 colonne di tipo sparse di tipo bigint. Se le colonne non Null sono 571, la dimensione totale su disco è pari a 571 * 12 = 6852 byte. Dopo avere incluso l'overhead di riga aggiuntivo e l'intestazione della colonna di tipo sparse, i byte aumentano a 6895. Per la pagina sono ancora disponibili su disco 1124 byte. Ciò può dare l'impressione che sia possibile aggiornare correttamente le colonne aggiuntive. Durante l'aggiornamento, tuttavia, si verifica un ulteriore overhead in memoria pari a 2\*(numero di colonne di tipo sparse non Null). In questo esempio, incluso l'overhead aggiuntivo, ovvero 2 \* 571 = 1142 byte, le dimensioni della riga su disco aumentano a 8037 byte. Queste dimensioni superano le dimensioni massime consentite di 8019 byte. Poiché tutte le colonne sono tipi di dati a lunghezza fissa, non è possibile spostarle all'esterno della riga. Di conseguenza, durante l'aggiornamento si verificherà l'errore 576.  
  
## <a name="restrictions-for-using-sparse-columns"></a>Restrizioni relative all'utilizzo di colonne di tipo sparse  
 Le colonne di tipo sparse possono essere di qualsiasi tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e presentano un comportamento analogo a qualsiasi altra colonna, ma con le restrizioni seguenti:  
  
-   Una colonna di tipo sparse deve ammettere i valori Null e non può includere proprietà ROWGUIDCOL o IDENTITY. Una colonna di tipo sparse non può essere costituita dai tipi di dati **text**, **ntext**, **image**, **timestamp**, tipi di dati definiti dall'utente, **geometry** o **geography**, né disporre dell'attributo FILESTREAM.  
  
-   Una colonna di tipo sparse non può avere un valore predefinito.  
  
-   Una colonna di tipo sparse non può essere associata a una regola.  
  
-   Sebbene possa contenere una colonna di tipo sparse, una colonna calcolata non può essere contrassegnata come SPARSE.  
  
-   È possibile definire una maschera dati in una colonna di tipo sparse, ma non in una colonna di tipo sparse che fa parte di un set di colonne.  
  
-   Una colonna di tipo sparse non può far parte di un indice cluster o di un indice di chiave primaria univoco. Sia le colonne calcolate persistenti sia quelle non persistenti definite in colonne di tipo sparse, tuttavia, possono far parte di una chiave cluster.  
  
-   Una colonna di tipo sparse non può essere utilizzata come chiave di partizione di un indice cluster o di un heap, ma può essere utilizzata come chiave di partizione di un indice non cluster.  
  
-   Una colonna di tipo sparse non può far parte di un tipo di tabella definito dall'utente, utilizzato in variabili di tabella e in parametri con valori di tabella.  
  
-   Le colonne di tipo sparse sono incompatibili con la compressione dei dati. Non è pertanto possibile aggiungere colonne di tipo sparse alle tabelle compresse, né comprimere tabelle contenenti colonne di tipo sparse.  
  
-   Per modificare una colonna di tipo sparse in non sparse o viceversa, è necessario modificare il formato di archiviazione della colonna. Per effettuare questa modifica, nel Motore di database di SQL Server viene utilizzata la procedura seguente:  
  
    1.  Viene aggiunta una nuova colonna alla tabella con le nuove dimensioni e nel nuovo formato di archiviazione.  
  
    2.  Per ogni riga della tabella, il valore archiviato nella colonna precedente viene aggiornato e copiato nella nuova colonna.  
  
    3.  La colonna precedente viene rimossa dallo schema della tabella.  
  
    4.  Viene ricompilata la tabella, se non include un indice cluster, oppure viene ricompilato l'indice cluster per recuperare lo spazio utilizzato dalla colonna precedente.  
  
    > [!NOTE]  
    >  È possibile che il passaggio 2 non venga completato correttamente se i dati della riga superano le dimensioni di riga massime consentite. Tali dimensioni includono quelle dei dati archiviati nella colonna precedente e quelle dei dati aggiornati archiviati nella nuova colonna. Il limite è di 8060 byte per le tabelle che non contengono colonne di tipo sparse o di 8018 byte per le tabelle che contengono colonne di tipo sparse. Questo errore può verificarsi anche se tutte le colonne idonee sono state spostate all'esterno delle righe.  
  
-   Quando una colonna non di tipo sparse viene modificata in una di tipo sparse, quest'ultima utilizzerà una quantità di spazio maggiore per i valori non Null. Quando le dimensioni di una riga si avvicinano al limite massimo consentito, potrebbe non essere possibile completare l'operazione.  
  
## <a name="sql-server-technologies-that-support-sparse-columns"></a>Tecnologie SQL Server che supportano le colonne di tipo sparse  
 In questa sezione viene descritto il supporto delle colonne di tipo sparse nelle tecnologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seguenti:  
  
-   Replica transazionale  
  
     La replica transazionale supporta le colonne di tipo sparse, ma non i set di colonne che possono essere utilizzati con le colonne di tipo sparse. Per altre informazioni sui set di colonne, vedere [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md).  
  
     La replica dell'attributo SPARSE è determinata da un'opzione di schema specificata utilizzando [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) o la finestra di dialogo **Article Properties** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supportano le colonne di tipo sparse. Se è necessario replicare dati in una versione precedente, specificare che l'attributo SPARSE non deve essere replicato.  
  
     Per le tabelle pubblicate, non è possibile aggiungere nuove colonne di tipo sparse a una tabella né modificare la proprietà sparse di una colonna esistente. Se questa operazione è necessaria, eliminare e ricreare la pubblicazione.  
  
-   Replica di tipo merge  
  
     La replica di tipo merge non supporta le colonne di tipo sparse né i set di colonne.  
  
-   Rilevamento modifiche  
  
     Il rilevamento delle modifiche supporta le colonne di tipo sparse e i set di colonne. Quando un set di colonne viene aggiornato in una tabella, il rilevamento delle modifiche considera questa operazione un aggiornamento all'intera riga. Non è disponibile alcun rilevamento delle modifiche dettagliato per ottenere il set esatto di colonne di tipo sparse aggiornate mediante l'aggiornamento del set di colonne. Se le colonne di tipo sparse vengono aggiornate in modo esplicito mediante un'istruzione DML, il rilevamento delle modifiche su tali colonne funzionerà nel modo usuale e verrà identificato il set esatto delle colonne modificate.  
  
-   Change Data Capture  
  
     La funzionalità Change Data Capture supporta le colonne di tipo sparse, ma non i set di colonne.  
  
-   La proprietà di tipo sparse di una colonna non viene mantenuta in caso di copia della tabella.  
  
## <a name="examples"></a>Esempi  
 In questo esempio viene illustrata una tabella Document che contiene un set comune in cui sono presenti le colonne `DocID` e `Title`. Il gruppo Production richiede una colonna `ProductionSpecification` e una colonna `ProductionLocation` per tutti i documenti relativi alla produzione, mentre il gruppo Marketing richiede una colonna `MarketingSurveyGroup` per i documenti relativi al marketing. Tramite il codice incluso nell'esempio viene creata una tabella che utilizza colonne di tipo sparse, vengono inserite due righe nella tabella, quindi vengono selezionati dati nella tabella.  
  
> [!NOTE]  
>  Questa tabella è costituita solo da cinque colonne per semplificare la visualizzazione e la lettura. Se è impostata l'opzione ANSI_NULL_DFLT_ON, è facoltativo dichiarare che le colonne di tipo sparse devono ammettere i valori Null.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStore  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL ) ;  
GO  
  
INSERT DocumentStore(DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStore(DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
 Selezionando tutte le colonne della tabella viene restituito un set di risultati comune.  
  
```  
SELECT * FROM DocumentStore ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation  MarketingSurveyGroup`  
  
 `1      Tire Spec 1  AXZZ217                  27                  NULL`  
  
 `2      Survey 2142  NULL                     NULL                Men 25-35`  
  
 Poiché i dati di marketing non interessano il reparto Production, questo desidera utilizzare un elenco di colonne che restituisca solo colonne di interesse specifico, come illustrato nella query seguente.  
  
```  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStore   
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation`  
  
 `1      Tire Spec 1  AXZZ217                  27`  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzare set di colonne](../../relational-databases/tables/use-column-sets.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
