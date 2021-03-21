---
description: Utilizzare set di colonne
title: Usare set di colonne | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, column sets
- column sets
ms.assetid: a4f9de95-dc8f-4ad8-b957-137e32bfa500
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8585b1274536856fc1b54a7cdb4cf96700693304
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747691"
---
# <a name="use-column-sets"></a>Utilizzare set di colonne
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Nelle tabelle che utilizzano colonne di tipo sparse è possibile definire un set di colonne per restituire tutte le colonne di tipo sparse della tabella. Un set di colonne è una rappresentazione XML non tipizzata che combina tutte le colonne di tipo sparse di una tabella in un output strutturato. Un set di colonne è analogo a una colonna calcolata poiché non è archiviato fisicamente nella tabella, ma differisce da una colonna calcolata poiché è direttamente aggiornabile.  
  
 È necessario utilizzare i set di colonne quando il numero di colonne di una tabella è elevato e l'esecuzione di operazioni sulle singole colonne risulta eccessivamente complessa. Potrebbe essere possibile ottenere un miglioramento delle prestazioni relative alle applicazioni in caso di selezione e inserimento dei dati utilizzando set di colonne in tabelle in cui il numero di colonne è elevato. Le prestazioni relative ai set di colonne, tuttavia, possono risultare ridotte quando nelle colonne della tabella sono definiti molti indici, poiché la quantità di memoria necessaria per un piano di esecuzione aumenta.  
  
 Per definire un set di colonne, usare le parole chiave *<nome_set_colonne>* FOR ALL_SPARSE_COLUMNS nelle istruzioni [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="guidelines-for-using-column-sets"></a>Linee guida per l'utilizzo di set di colonne  
 Quando si utilizzano set di colonne, tenere presente le linee guida seguenti:  
  
-   Le colonne di tipo sparse e un set di colonne possono essere aggiunti come parte della stessa istruzione.  
  
-   Un set di colonne non può essere aggiunto a una tabella in cui sono già contenute colonne di tipo sparse.  
  
-   Il set di colonne non può essere modificato. Per modificare un set di colonne, è necessario eliminarlo e ricreare le colonne di tipo sparse e il set di colonne.  
  
-   Un set di colonne può essere aggiunto a una tabella in cui non è contenuta alcuna colonna di tipo sparse. Se le colonne di tipo sparse vengono aggiunte alla tabella in un secondo momento, verranno visualizzate nel set di colonne.  
  
-   Per ogni tabella è consentito un unico set di colonne.  
  
-   Un set di colonne è facoltativo e non è necessario per utilizzare colonne di tipo sparse.  
  
-   Non è possibile definire vincoli o valori predefiniti in un set di colonne.  
  
-   Nelle colonne calcolate non possono essere contenute colonne di un set di colonne.  
  
-   Le query distribuite non sono supportate in tabelle che contengono set di colonne.  
  
-   I set di colonne non sono supportati dalla replica.  
  
-   I set di colonne non sono supportati da Change Data Capture.  
  
-   Un set di colonne non può appartenere ad alcun tipo di indice, ad esempio indici XML, indici full-text e viste indicizzate. Non è inoltre possibile aggiungere un set di colonne come colonna inclusa in un indice.  
  
-   Un set di colonne non può essere utilizzato nell'espressione di filtro di un indice filtrato o di statistiche filtrate.  
  
-   Quando in una vista è incluso un set di colonne, quest'ultimo viene visualizzato nella vista come una colonna XML.  
  
-   Un set di colonne non può essere incluso nella definizione di una vista indicizzata.  
  
-   Le viste partizionate che includono tabelle in cui sono contenuti set di colonne sono aggiornabili quando la vista partizionata specifica le colonne di tipo sparse in base al nome. Una vista partizionata non è aggiornabile quando fa riferimento al set di colonne.  
  
-   Le notifiche delle query che fanno riferimento a set di colonne non sono consentite.  
  
-   Le dimensioni dei dati XML non possono superare i 2 GB. Se i dati combinati di tutte le colonne di tipo sparse diverse da Null in una riga superano questo limite, la query o l'operazione DML genererà un errore.  
  
-   Per informazioni sui dati restituiti dalla funzione COLUMNS_UPDATED, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="guidelines-for-selecting-data-from-a-column-set"></a>Linee guida per la selezione di dati da un set di colonne  
 Per selezionare dati da un set di colonne, tenere presente le linee guida seguenti:  
  
-   Concettualmente, un set di colonne è un tipo di colonna XML calcolata e aggiornabile, che aggrega un set di colonne relazionali sottostanti in un'unica rappresentazione XML. Il set di colonne supporta solo la proprietà ALL_SPARSE_COLUMNS, utilizzata per aggregare tutti i valori diversi da Null di tutte le colonne di tipo sparse per una riga specifica.  
  
-   Nell'editor di tabella di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] i set di colonne vengono visualizzati come un campo XML modificabile. Definire i set di colonne nel formato seguente:  
  
    ```  
    <column_name_1>value1</column_name_1><column_name_2>value2</column_name_2>...  
    ```  
  
     Di seguito vengono riportati alcuni esempi di valori di set di colonne:  
  
    -   `<sparseProp1>10</sparseProp1><sparseProp3>20</sparseProp3>`  
  
    -   `<DocTitle>Bicycle Parts List</DocTitle><Region>West</Region>`  
  
-   Le colonne di tipo sparse in cui sono contenuti valori Null vengono omesse dalla rappresentazione XML del set di colonne.  
  
> [!WARNING]  
>  L'aggiunta di un set di colonne modifica il comportamento delle query `SELECT *`. La query restituirà il set di colonne come una colonna XML e non restituirà le singole colonne di tipo sparse. È necessario che i progettisti degli schemi e gli sviluppatori del software prestino particolare attenzione a non causare l'interruzione delle applicazioni esistenti.  
  
## <a name="inserting-or-modifying-data-in-a-column-set"></a>Inserimento o modifica di dati in un set di colonne  
 Per manipolare i dati di una colonna di tipo sparse, è possibile utilizzare il nome delle colonne singole o fare riferimento al nome del set di colonne e specificarne i valori utilizzando il formato XML del set di colonne stesso. Nella colonna XML le colonne di tipo sparse possono essere visualizzate in qualsiasi ordine.  
  
 Quando i valori di una colonna di tipo sparse vengono inseriti o aggiornati usando il set di colonne XML, i valori inseriti nelle colonne di tipo sparse sottostanti vengono convertiti implicitamente dal tipo di dati **xml** . Nel caso di colonne numeriche, un valore vuoto nel codice XML per la colonna numerica viene convertito in una stringa vuota, provocando l'inserimento di uno zero nella colonna numerica come illustrato nell'esempio seguente.  
  
```sql  
CREATE TABLE t (i int SPARSE, cs xml column_set FOR ALL_SPARSE_COLUMNS);  
GO  
INSERT t(cs) VALUES ('<i/>');  
GO  
SELECT i FROM t;  
GO  
```  
  
 In questo esempio per la colonna `i`non è stato specificato alcun valore, ma è stato inserito il valore `0` .  
  
## <a name="using-the-sql_variant-data-type"></a>Utilizzo del tipo di dati sql_variant  
 Il tipo di dati **sql_variant** consente di archiviare più tipi di dati diversi, ad esempio **int**, **char** e **date**. I set di colonne restituiscono informazioni sul tipo di dati, ad esempio scala, precisione e informazioni sulle impostazioni locali, associate a un valore **sql_variant** come attributi nella colonna XML generata. Se si tenta di specificare questi attributi un'istruzione XML generata dall'utente come input per un'operazione di inserimento o di aggiornamento su un set di colonne, alcuni di questi attributi risultano obbligatori e ad alcuni di essi viene assegnato un valore predefinito. Nella tabella seguente vengono elencati i tipi di dati e i valori predefiniti generati dal server quando il valore non viene specificato.  
  
|Tipo di dati|localeID*|sqlCompareOptions|sqlCollationVersion|SqlSortId|Lunghezza massima|Precisione|Scalabilità|  
|---------------|----------------|-----------------------|-------------------------|---------------|--------------------|---------------|-----------|  
|**char**, **varchar**, **binary**|-1|Impostazione predefinita|0|0|8000|Non applicabile**|Non applicabile|  
|**nvarchar**|-1|Impostazione predefinita|0|0|4000|Non applicabile|Non applicabile|  
|**decimal**, **float**, **real**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|18|0|  
|**integer**, **bigint**, **tinyint**, **smallint**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|  
|**datetime2**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|7|  
|**datetime offset**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|7|  
|**datetime**, **date**, **smalldatetime**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|  
|**money**, **smallmoney**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|  
|**time**|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|Non applicabile|7|  
  
 \*  localeID -1 indica le impostazioni locali predefinite. Le impostazioni locali per l'inglese sono rappresentate dal valore 1033.  
  
 ** Non applicabile = Non viene restituito alcun valore per questi attributi durante un'operazione di selezione sul set di colonne. Quando per questo attributo viene specificato un valore dal chiamante nella rappresentazione XML fornita per un set di colonne in un'operazione di inserimento o di aggiornamento, viene generato un errore.  
  
## <a name="security"></a>Sicurezza  
 Il funzionamento del modello di sicurezza per un set di colonne è analogo a quello del modello di sicurezza che esiste tra una tabella e le relative colonne. I set di colonne possono essere visualizzati come una tabella di dimensioni ridotte e un'operazione di selezione è analoga a un'operazione SELECT * eseguita su tale tabella. La relazione tra il set di colonne e le colonne di tipo sparse, tuttavia, rappresenta un raggruppamento anziché rappresentare esclusivamente un contenitore. Il modello controlla la sicurezza sulla colonna del set di colonne e applica le operazioni DENY sulle colonne di tipo sparse sottostanti. Di seguito sono riportate le caratteristiche aggiuntive del modello di sicurezza.  
  
-   È possibile concedere e revocare le autorizzazioni di sicurezza alla colonna del set di colonne, analogamente a qualsiasi altra colonna della tabella.  
  
-   Un'istruzione GRANT o REVOKE dell'autorizzazione SELECT, INSERT, UPDATE, DELETE e REFERENCES su una colonna del set di colonne non si propaga alle colonne membro sottostanti di tale set. Questa condizione si applica solo all'utilizzo della colonna del set di colonne. L'autorizzazione DENY su un set di colonne si propaga alle colonne di tipo sparse sottostanti della tabella.  
  
-   Per eseguire le istruzioni SELECT, INSERT, UPDATE e DELETE nella colonna del set di colonne, è necessario che l'utente disponga delle autorizzazioni corrispondenti sulla colonna stessa, nonché dell'autorizzazione corrispondente su tutte le colonne di tipo sparse della tabella. Poiché il set di colonne rappresenta tutte le colonne di tipo sparse della tabella, è necessario disporre dell'autorizzazione su tutte le colonne di tipo sparse, incluse quelle che potrebbero anche non subire modifiche.  
  
-   L'esecuzione di un'istruzione REVOKE su una colonna di tipo sparse o su un set di colonne imposta automaticamente la sicurezza sul relativo oggetto padre.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti una tabella in cui sono contenuti i documenti è presente il set di colonne comune `DocID` e `Title`. Il gruppo Production richiede una colonna `ProductionSpecification` e una colonna `ProductionLocation` per tutti i documenti relativi alla produzione, mentre il gruppo Marketing richiede una colonna `MarketingSurveyGroup` per i documenti relativi al marketing.  
  
### <a name="a-creating-a-table-that-has-a-column-set"></a>R. Creazione di una tabella in cui è presente un set di colonne  
 Nell'esempio seguente viene creata la tabella che utilizza colonne di tipo sparse e include il set di colonne `SpecialPurposeColumns`. Vengono inserite due righe nella tabella, quindi vengono selezionati dati dalla tabella.  
  
> [!NOTE]  
>  Questa tabella è costituita solo da cinque colonne per semplificare la visualizzazione e la lettura.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStoreWithColumnSet  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL,  
     MarketingProgramID int SPARSE NULL,  
     SpecialPurposeColumns XML COLUMN_SET FOR ALL_SPARSE_COLUMNS);  
GO  
```  
  
### <a name="b-inserting-data-to-a-table-by-using-the-names-of-the-sparse-columns"></a>B. Inserimento di dati in una tabella utilizzando i nomi delle colonne di tipo sparse  
 Negli esempi seguenti vengono inserite due righe nella tabella creata nell'esempio A. Negli esempi vengono utilizzati i nomi delle colonne di tipo sparse e non si fa riferimento al set di colonne.  
  
```sql  
INSERT DocumentStoreWithColumnSet (DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStoreWithColumnSet (DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
### <a name="c-inserting-data-to-a-table-by-using-the-name-of-the-column-set"></a>C. Inserimento di dati in una tabella utilizzando il nome del set di colonne  
 Nell'esempio seguente viene inserita una terza riga nella tabella creata nell'esempio A. In questo caso i nomi delle colonne di tipo sparse non vengono utilizzati, ma viene utilizzato il nome del set di colonne. L'inserimento fornisce i valori per due delle quattro colonne di tipo sparse in formato XML.  
  
```sql  
INSERT DocumentStoreWithColumnSet (DocID, Title, SpecialPurposeColumns)  
VALUES (3, 'Tire Spec 2', '<ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>');  
GO  
```  
  
### <a name="d-observing-the-results-of-a-column-set-when-select--is-used"></a>D. Analisi dei risultati di un set di colonne quando viene utilizzata l'istruzione SELECT *  
 Nell'esempio seguente vengono selezionate tutte le colonne dalla tabella che contiene un set di colonne. Viene restituita una colonna XML con i valori combinati delle colonne di tipo sparse, ma non vengono restituite le singole colonne di tipo sparse.  
  
```sql  
SELECT DocID, Title, SpecialPurposeColumns FROM DocumentStoreWithColumnSet ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        SpecialPurposeColumns  
 1      Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>  
 2      Survey 2142  <MarketingSurveyGroup>Men 25 - 35</MarketingSurveyGroup>  
 3      Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation> 
 ```
  
### <a name="e-observing-the-results-of-selecting-the-column-set-by-name"></a>E. Analisi dei risultati della selezione del set di colonne in base al nome  
 Poiché i dati di marketing non interessano il reparto Production, in questo esempio viene aggiunta una clausola `WHERE` per limitare l'output. Nell'esempio viene utilizzato il nome del set di colonne.  
  
```sql  
SELECT DocID, Title, SpecialPurposeColumns  
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        SpecialPurposeColumns  
 1      Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>  
 3      Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>  
 ```
 
### <a name="f-observing-the-results-of-selecting-sparse-columns-by-name"></a>F. Analisi dei risultati della selezione di colonne di tipo sparse in base al nome  
 Quando una tabella contiene un set di colonne, è ancora possibile eseguire una query sulla tabella utilizzando i nomi delle singole colonne come illustrato nell'esempio seguente.  
  
```sql  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        ProductionSpecification ProductionLocation`  
 1      Tire Spec 1  AXZZ217                 27`  
 3      Tire Spec 2  AXW9R411                38`  
 ```
 
### <a name="g-updating-a-table-by-using-a-column-set"></a>G. Aggiornamento di una tabella tramite un set di colonne  
 Nell'esempio seguente viene aggiornato il terzo record con i nuovi valori per entrambe le colonne di tipo sparse utilizzate da tale riga.  
  
```sql  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification><ProductionLocation>38</ProductionLocation>'  
WHERE DocID = 3 ;  
GO  
```  
  
> [!IMPORTANT]  
>  Un'istruzione UPDATE che utilizza un set di colonne aggiorna tutte le colonne di tipo sparse della tabella. Le colonne di tipo sparse alle quali non viene fatto riferimento vengono impostate su NULL.  
  
 Nell'esempio seguente viene aggiornato il terzo record, ma viene specificato solo il valore di una delle due colonne popolate. La seconda colonna `ProductionLocation` non è inclusa nell'istruzione `UPDATE` e viene impostata su NULL.  
  
```sql  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification>'  
WHERE DocID = 3 ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md)  
  
  
