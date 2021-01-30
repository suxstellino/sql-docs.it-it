---
description: CREATE SPATIAL INDEX (Transact-SQL)
title: CREATE SPATIAL INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SPATIAL INDEX
- CREATE SPATIAL INDEX
- CREATE_SPATIAL_INDEX_TSQL
- SPATIAL_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], creating
- index creation [SQL Server], spatial indexes
- CREATE SPATIAL INDEX statement
- CREATE INDEX statement
ms.assetid: ee6b9116-a7ff-463a-a9f0-b360804d8678
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 07f7bdead063167b789bd465fac6f66b653a0cbb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191024"
---
# <a name="create-spatial-index-transact-sql"></a>CREATE SPATIAL INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un indice spaziale in una tabella e in una colonna specificate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'indice può essere creato prima dell'immissione dei dati nella tabella. È possibile creare indici per tabelle o viste di un altro database specificando un nome di database completo. Per gli indici spaziali è richiesto che nella tabella sia inclusa una chiave primaria cluster. Non è possibile specificare indici spaziali in viste indicizzate. Per informazioni sugli indici spaziali, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE SPATIAL INDEX index_name
  ON <object> ( spatial_column_name )  
    {  
       <geometry_tessellation> | <geography_tessellation>  
    }
  [ ON { filegroup_name | "default" } ]  
[;]
  
<object> ::=  
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<geometry_tessellation> ::=  
{
  <geometry_automatic_grid_tessellation>
| <geometry_manual_grid_tessellation>
}  
  
<geometry_automatic_grid_tessellation> ::=  
{  
    [ USING GEOMETRY_AUTO_GRID ]  
          WITH  (  
        <bounding_box>  
            [ [,] <tessellation_cells_per_object> [ ,...n] ]  
            [ [,] <spatial_index_option> [ ,...n] ]  
                 )  
}  
  
<geometry_manual_grid_tessellation> ::=  
{  
       [ USING GEOMETRY_GRID ]  
         WITH (  
                    <bounding_box>  
                        [ [,]<tessellation_grid> [ ,...n] ]  
                        [ [,]<tessellation_cells_per_object> [ ,...n] ]  
                        [ [,]<spatial_index_option> [ ,...n] ]  
   )  
}
  
<geography_tessellation> ::=  
{  
      <geography_automatic_grid_tessellation> | <geography_manual_grid_tessellation>  
}  
  
<geography_automatic_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_AUTO_GRID ]  
    [ WITH (  
        [ [,] <tessellation_cells_per_object> [ ,...n] ]  
        [ [,] <spatial_index_option> ]  
     ) ]  
}  
  
<geography_manual_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_GRID ]  
    [ WITH (  
                [ <tessellation_grid> [ ,...n] ]  
                [ [,] <tessellation_cells_per_object> [ ,...n] ]  
                [ [,] <spatial_index_option> [ ,...n] ]  
                ) ]  
}  
  
<bounding_box> ::=  
{  
      BOUNDING_BOX = ( {  
       xmin, ymin, xmax, ymax
       | <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>
  } )  
}  
  
<named_bb_coordinate> ::= { XMIN = xmin | YMIN = ymin | XMAX = xmax | YMAX=ymax }  
  
<tessellation_grid> ::=  
{
    GRIDS = ( { <grid_level> [ ,...n ] | <grid_size>, <grid_size>, <grid_size>, <grid_size>  }
        )  
}  
<tessellation_cells_per_object> ::=  
{
   CELLS_PER_OBJECT = n
}  
  
<grid_level> ::=  
{  
     LEVEL_1 = <grid_size>
  |  LEVEL_2 = <grid_size>
  |  LEVEL_3 = <grid_size>
  |  LEVEL_4 = <grid_size>
}  
  
<grid_size> ::= { LOW | MEDIUM | HIGH }  
  
<spatial_index_option> ::=  
{  
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE }  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *index_name*     
 Nome dell'indice. I nomi degli indici devono essere univoci all'interno di una tabella, ma non all'interno di un database. Devono essere anche conformi alle regole degli [identificatori](../../relational-databases/databases/database-identifiers.md).  
  
 ON \<object> (*spatial_column_name*)     
 Specifica l'oggetto, ovvero database, schema o tabella, in cui deve essere creato l'indice e il nome di una colonna spaziale.  
  
 *spatial_column_name* specifica la colonna spaziale su cui è basato l'indice. È possibile specificare una sola colonna spaziale in una singola definizione di indice spaziale. È invece possibile creare più indici spaziali in una colonna di tipo **geometry** o **geography**.  
  
 USING      
 Indica lo schema a mosaico dell'indice spaziale. Questo parametro utilizza il valore specifico del tipo, mostrato nella tabella seguente:  
  
|Tipo di dati della colonna|Schema a mosaico|  
|-------------------------|-------------------------|  
|**geometry**|GEOMETRY_GRID|  
|**geometry**|GEOMETRY_AUTO_GRID|  
|**geography**|GEOGRAPHY_GRID|  
|**geography**|GEOGRAPHY_AUTO_GRID|  
  
 Un indice spaziale può essere creato solo in una colonna di tipo **geometry** o **geography**. In caso contrario, viene generato un errore. Viene generato un errore quando viene passato un parametro non valido per un determinato tipo.  
  
 Per informazioni sull'[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]implementazione dello schema a mosaico, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ON *filegroup_name*      
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Crea l'indice specificato nel filegroup specificato. Se non viene specificato alcun percorso e la tabella non è partizionata, l'indice utilizza lo stesso filegroup della tabella sottostante. Il filegroup deve essere già esistente.  
  
 ON "default"     
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Crea l'indice specificato nel filegroup predefinito.  
  
 In questo contesto il termine default non rappresenta una parola chiave, Si tratta di un identificatore del filegroup predefinito e deve essere delimitato, ad esempio ON "default" o ON [default]. Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **\<object>::=**     
 Oggetto con nome completo o non completo da indicizzare.  
  
 *database_name*      
 Nome del database.  
  
 *schema_name*     
 Nome dello schema a cui appartiene la tabella.  
  
 *table_name*      
 Nome della tabella da indicizzare.  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] supporta il formato del nome in tre parti database_name.[schema_name].object_name quando database_name è il database corrente o database_name è tempdb e object_name inizia con #.  
  
### <a name="using-options"></a>Opzioni USING

 GEOMETRY_GRID      
 Specifica lo schema a mosaico per la griglia **geometrica** in uso. È possibile specificare GEOMETRY_GRID solo in una colonna con tipo di dati **geometry**.  GEOMETRY_GRID consente la regolazione manuale dello schema a mosaico.  
  
 GEOMETRY_AUTO_GRID      
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Può essere specificata solo in una colonna con tipo di dati geometry. Si tratta dell'impostazione predefinita per questo tipo di dati e non è necessario specificarla.  
  
 GEOGRAPHY_GRID      
 Specifica lo schema a mosaico per la griglia geografica. È possibile specificare GEOGRAPHY_GRID solo in una colonna con tipo di dati **geography**.  
  
 GEOGRAPHY_AUTO_GRID      
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Può essere specificata solo in una colonna con tipo di dati geography.  Si tratta dell'impostazione predefinita per questo tipo di dati e non è necessario specificarla.  
  
### <a name="with-options"></a>Opzioni WITH

BOUNDING_BOX      
Specifica una tupla numerica di quattro elementi che definisce le quattro coordinate del rettangolo di selezione: le coordinate x-min e y-min dell'angolo inferiore sinistro e le coordinate x-max e y-max dell'angolo superiore destro.  
  
 *xmin*      
 Specifica la coordinata x dell'angolo inferiore sinistro del rettangolo di selezione.  
  
 *ymin*      
 Specifica la coordinata y dell'angolo inferiore sinistro del rettangolo di selezione.  
  
 *xmax*     
 Specifica la coordinata x dell'angolo superiore destro del rettangolo di selezione.  
  
 *ymax*     
 Specifica la coordinata y dell'angolo superiore destro del rettangolo di selezione.  
  
 XMIN = *xmin*     
 Specifica il nome della proprietà e il valore della coordinata x dell'angolo inferiore sinistro del rettangolo di selezione.  
  
 YMIN =*ymin*      
 Specifica il nome della proprietà e il valore della coordinata y dell'angolo inferiore sinistro del rettangolo di selezione.  
  
 XMAX =*xmax*      
 Specifica il nome della proprietà e il valore della coordinata x dell'angolo superiore destro del rettangolo di selezione.  
  
 YMAX =*ymax*     
 Specifica il nome della proprietà e il valore della coordinata y dell'angolo superiore destro del rettangolo di selezione.  
  
 > [!NOTE]
 > Le coordinate dei rettangoli di selezione si applicano solo all'interno di una clausola USING GEOMETRY_GRID.  
 >
 > *xmax* deve essere maggiore di *xmin* e *ymax* deve essere maggiore di *ymin*. È possibile specificare qualsiasi rappresentazione di valore [float](../../t-sql/data-types/float-and-real-transact-sql.md) valida, presupponendo che: *xmax* > *xmin* e *ymax* > *ymin*. In caso contrario, vengono generati errori.  
 >
 > Non sono previsti valori predefiniti.  
 >
 > Per i nomi delle proprietà dei rettangoli di selezione non viene fatta distinzione tra maiuscole e minuscole, indipendentemente dalle regole di confronto del database.  
  
 Per specificare i nomi delle proprietà, è necessario specificare ogni nome una sola volta. I nomi possono essere specificati in qualsiasi ordine. Le clausole seguenti sono ad esempio equivalenti:  
  
- BOUNDING_BOX =( XMIN =*xmin*, YMIN =*ymin*, XMAX =*xmax*, YMAX =*ymax* )  
  
- BOUNDING_BOX =( XMIN =*xmin*, XMAX =*xmax*, YMIN =*ymin*, YMAX =*ymax*)  
  
GRIDS      
Definisce la densità della griglia a ogni livello di uno schema a mosaico. Se GEOMETRY_AUTO_GRID e GEOGRAPHY_AUTO_GRID sono selezionate, questa opzione è disabilitata.  
  
 Per informazioni sull'implementazione dello schema a mosaico, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 I parametri di GRID sono i seguenti:  
  
 LEVEL_1     
 Specifica la griglia di primo livello (principale).  
  
 LEVEL_2    
 Specifica la griglia di secondo livello.  
  
 LEVEL_3    
 Specifica la griglia di terzo livello.  
  
 LEVEL_4    
 Specifica la griglia di quarto livello.  
  
 LOW    
 Specifica la più bassa densità possibile per la griglia a un determinato livello. LOW equivale a 16 celle (griglia 4x4).  
  
 **MEDIUM**     
 Specifica la densità media per la griglia a un determinato livello. MEDIUM equivale a 64 celle (una griglia 8x8).  
  
 HIGH     
 Specifica la più alta densità possibile per la griglia a un determinato livello. HIGH equivale a 256 celle (griglia 16x16).  
  
> [!NOTE]
> L'utilizzo di nomi per i livelli consente di specificare i livelli in qualsiasi ordine e di ometterne alcuni. Se si utilizza il nome per un livello, è necessario utilizzarlo anche per qualsiasi altro livello specificato. Se si omette un livello, per la densità viene utilizzato il valore predefinito MEDIUM.  

> [!WARNING]
> Se si specifica una densità non valida, viene generato un errore.  
  
CELLS_PER_OBJECT =*n*     
Viene specificato il numero di celle dello schema a mosaico utilizzabile per un singolo oggetto spaziale nell'indice dal processo dello schema a mosaico. *n* può essere qualsiasi numero intero compreso tra 1 e 8192 inclusi. Se viene passato un numero non valido o il numero è maggiore del numero massimo di celle per lo schema a mosaico specificato, viene generato un errore.  
  
 CELLS_PER_OBJECT dispone dei valori predefiniti seguenti:  
  
|Opzione USING|Celle predefinite per oggetto|  
|------------------|------------------------------|  
|GEOMETRY_GRID|**16**|  
|GEOMETRY_AUTO_GRID|**8**|  
|GEOGRAPHY_GRID|**16**|  
|GEOGRAPHY_AUTO_GRID|**12**|  
  
Se al livello principale un oggetto include più celle rispetto a quanto specificato da *n*, l'indicizzazione usa il numero di celle necessario per offrire uno schema a mosaico di livello principale completo. In tali casi un oggetto può ricevere un numero di celle maggiore di quello specificato: In questo caso, il numero massimo è il numero di celle generate dalla griglia di livello principale, che dipende dalla densità.  
  
Il valore CELLS_PER_OBJECT viene utilizzato dalla regola dello schema a mosaico di celle per oggetto. Per informazioni sull'implementazione dello schema a mosaico, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
PAD_INDEX = { ON | **OFF** }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
Specifica il riempimento dell'indice. Il valore predefinito è OFF.  
  
ON     
Indica che la percentuale di spazio disponibile specificata in *fillfactor* viene applicata alle pagine di livello intermedio dell'indice.  
  
OFF o *fillfactor* non è specificato     
Indica che le pagine di livello intermedio vengono riempite poco al di sotto della capacità massima, in modo che lo spazio residuo sia sufficiente per almeno una riga della dimensione massima supportata dall'indice, in base al set di chiavi nelle pagine intermedie.  
  
L'opzione PAD_INDEX risulta utile solo quando si specifica FILLFACTOR, in quanto PAD_INDEX usano la percentuale specificata in FILLFACTOR. Se la percentuale specificata in FILLFACTOR non consente l'inserimento di una riga, [!INCLUDE[ssDE](../../includes/ssde-md.md)] sostituisce internamente tale percentuale in modo da rendere disponibile lo spazio minimo necessario. Il numero di righe di una pagina intermedia dell'indice non è mai minore di due, indipendentemente dal valore di *fillfactor*.  
  
FILLFACTOR =*fillfactor*     
 **Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica una percentuale che indica il livello di riempimento del livello foglia di ogni pagina di indice applicato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione o la ricompilazione dell'indice. *fillfactor* deve essere un valore intero compreso tra 1 e 100. Il valore predefinito è 0. Se *fillfactor* è 100 o 0, tramite [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono creati indici con pagine foglia riempite fino alla capacità massima.  
  
> [!NOTE]  
> I valori 0 e 100 relativi al fattore di riempimento sono equivalenti.
  
 L'impostazione di FILLFACTOR viene applicata solo in fase di creazione o di ricompilazione dell'indice. La percentuale specificata di spazio vuoto delle pagine non viene mantenuta in modo dinamico da [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per visualizzare l'impostazione del fattore di riempimento, usare la vista del catalogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
> La creazione di un indice cluster con un valore FILLFACTOR minore di 100 influisce sulla quantità di spazio di archiviazione occupata dai dati perché i dati vengono ridistribuiti da [!INCLUDE[ssDE](../../includes/ssde-md.md)] durante la creazione dell'indice cluster.  
  
 Per altre informazioni, vedere [Specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
SORT_IN_TEMPDB = { ON | **OFF** }       
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Specifica se i risultati temporanei dell'ordinamento devono essere archiviati in tempdb. Il valore predefinito è OFF.  
  
 ON     
 I risultati intermedi dell'ordinamento utilizzati per la compilazione dell'indice vengono archiviati in tempdb. Questo potrebbe ridurre il tempo necessario per creare un indice se tempdb si trova in un set di dischi diverso rispetto al database utente. La quantità di spazio su disco utilizzata durante la compilazione dell'indice sarà tuttavia maggiore.  
  
 OFF     
 I risultati intermedi dell'ordinamento vengono archiviati nello stesso database dell'indice.  
  
 Oltre allo spazio necessario nel database utente per la creazione dell'indice, in tempdb deve essere disponibile una quantità di spazio aggiuntivo pressoché equivalente per l'archiviazione dei risultati intermedi dell'ordinamento. Per altre informazioni, vedere [Opzione SORT_IN_TEMPDB per gli indici](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
IGNORE_DUP_KEY =**OFF**     
Non ha effetto per gli indici spaziali perché il tipo di indice non è mai univoco. Non impostare questa opzione su ON. In caso contrario, viene generato un errore.  
  
STATISTICS_NORECOMPUTE = { ON | **OFF**}     
Specifica se le statistiche di distribuzione vengono ricalcolate. Il valore predefinito è OFF.  
  
 ON    
 Le statistiche non aggiornate non vengono ricalcolate automaticamente.  
  
 OFF    
 Abilita l'aggiornamento automatico delle statistiche.  
  
 Per ripristinare l'aggiornamento automatico delle statistiche, impostare l'opzione STATISTICS_NORECOMPUTE su OFF oppure eseguire UPDATE STATISTICS senza la clausola NORECOMPUTE.  
  
> [!IMPORTANT]  
> La disabilitazione del ricalcolo automatico delle statistiche di distribuzione può compromettere la selezione di piani di esecuzione ottimali per le query riguardanti la tabella in Query Optimizer.  
  
DROP_EXISTING = { ON | **OFF** }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Specifica che è necessario eliminare e quindi ricompilare l'indice spaziale denominato preesistente. Il valore predefinito è OFF.  
  
 ON     
 L'indice esistente deve essere eliminato e ricompilato. Il nome di indice specificato deve corrispondere a quello dell'indice esistente, mentre la definizione dell'indice può essere modificata. È possibile, ad esempio, specificare valori diversi per le colonne, il tipo di ordinamento, lo schema di partizione o le opzioni dell'indice.  
  
 OFF     
 Se il nome di indice specificato esiste già, viene visualizzato un messaggio di errore.  
  
 Il tipo di indice non può essere modificato tramite l'opzione DROP_EXISTING.  
  
ONLINE =**OFF**      
Specifica che le tabelle sottostanti e gli indici associati non sono disponibili per le query e la modifica dei dati durante l'operazione sull'indice. In questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le operazioni di compilazione di indici online non sono supportate per gli indici spaziali. Se questa opzione è impostata su ON per un indice spaziale, viene generato un errore. Omettere l'opzione ONLINE o impostare ONLINE su OFF.  
  
 Un'operazione offline sull'indice che crea, ricompila o elimina un indice spaziale acquisisce un blocco di modifica dello schema (SCH-M) per la tabella. Il blocco impedisce agli utenti di accedere alla tabella sottostante per la durata dell'operazione.  
  
> [!NOTE]  
> Le operazioni sugli indici online sono disponibili solo in alcune edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
ALLOW_ROW_LOCKS = { **ON** | OFF }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Specifica se sono consentiti blocchi di riga. Il valore predefinito è ON.  
  
 ON     
 I blocchi di riga sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando usare blocchi di riga.  
  
 OFF     
 I blocchi di riga non vengono utilizzati.  
  
ALLOW_PAGE_LOCKS = { **ON** | OFF }     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Specifica se sono consentiti blocchi a livello di pagina. Il valore predefinito è ON.  
  
 ON    
 I blocchi a livello di pagina sono consentiti durante l'accesso all'indice. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina quando utilizzare blocchi a livello di pagina.  
  
 OFF     
 I blocchi a livello di pagina non vengono utilizzati.  
  
MAXDOP =*max_degree_of_parallelism*      
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Esegue l'override dell'opzione di configurazione `max degree of parallelism` per la durata dell'operazione sugli indici. Utilizzare MAXDOP per limitare il numero di processori utilizzati durante l'esecuzione di un piano parallelo. Il valore massimo è 64 processori.  
  
> [!IMPORTANT]  
> Sebbene l'opzione MAXDOP sia supportata a livello di sintassi, CREATE SPATIAL INDEX attualmente utilizza sempre solo un processore singolo.  
  
 *max_degree_of_parallelism* può essere:  
  
 1     
 Disattiva la generazione di piani paralleli.  
  
 \>1    
 Consente di limitare al valore specificato, o a un valore più basso in base al carico di lavoro corrente del sistema, il numero massimo di processori utilizzati in un'operazione parallela sugli indici.  
  
 0 (predefinito)    
 Utilizza il numero effettivo di processori o un numero inferiore in base al carico di lavoro corrente del sistema.  
  
 Per altre informazioni, vedere [Configurazione di operazioni parallele sugli indici](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Le operazioni parallele sugli indici non sono disponibili in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
DATA_COMPRESSION = {NONE | ROW | PAGE}     
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Determina il livello di compressione dati utilizzata dall'indice.  
  
 NONE     
 Nessuna compressione utilizzata sui dati dall'indice  
  
 ROW    
 Compressione di riga utilizzata sui dati dall'indice  
  
 PAGE    
 Compressione di pagina utilizzata sui dati dall'indice  
  
## <a name="remarks"></a>Osservazioni
Ogni opzione può essere specificata una sola volta per ogni istruzione CREATE SPATIAL INDEX. Se un'opzione viene specificata due volte, viene generato un errore.  
  
È possibile creare fino a 249 indici spaziali in ogni colonna spaziale di una tabella. La creazione di più di un indice spaziale in una specifica colonna spaziale può essere utile, ad esempio, per indicizzare parametri di schema a mosaico diversi in un'unica colonna.  
  
> [!IMPORTANT]  
> La creazione di indici spaziali è soggetta anche ad altre limitazioni. Per altre informazioni, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
Per la compilazione di un indice non è possibile utilizzare il parallelismo di processi disponibile.  
  
## <a name="methods-supported-on-spatial-indexes"></a>Metodi supportati negli indici spaziali
In determinate condizioni, gli indici spaziali supportano diversi metodi geometrici orientati ai set. Per altre informazioni, vedere [Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="spatial-indexes-and-partitioning"></a>Indici spaziali e partizionamento
Per impostazione predefinita, se un indice spaziale viene creato in una tabella partizionata, l'indice viene partizionato in base allo schema di partizione della tabella. In questo modo, i dati dell'indice e la riga correlata vengono archiviati nella stessa partizione.  
  
In questo caso, per modificare lo schema di partizione della tabella di base, sarebbe necessario eliminare l'indice spaziale prima di ripartizionare la tabella di base. Per evitare questa limitazione, quando si crea un indice spaziale è possibile specificare l'opzione "ON filegroup". Per ulteriori informazioni, vedere "Indici spaziali e filegroup", più avanti in questo argomento.  
  
## <a name="spatial-indexes-and-filegroups"></a>Indici spaziali e filegroup
Per impostazione predefinita, gli indici spaziali vengono partizionati negli stessi filegroup della tabella in cui l'indice viene specificato. Questo comportamento può essere modificato utilizzando la specifica del filegroup:  
  
[ ON { *filegroup_name* | "default" } ]     
Se si specifica un filegroup per un indice spaziale, l'indice viene inserito in tale filegroup, indipendentemente dallo schema di partizione della tabella.  
  
## <a name="catalog-views-for-spatial-indexes"></a>Viste del catalogo per gli indici spaziali

 Le viste del catalogo seguenti sono specifiche degli indici spaziali:  
  
 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)  
 Rappresenta le principali informazioni per gli indici spaziali.  
  
 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)  
 Rappresenta le informazioni sullo schema a mosaico e i parametri di ognuno degli indici spaziali.  
  
## <a name="additional-remarks-about-creating-indexes"></a>Osservazioni aggiuntive sulla creazione degli indici

 Per altre informazioni sulla creazione degli indici, vedere la sezione "Osservazioni" in [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni
L'utente deve avere l'autorizzazione `ALTER` per la tabella o vista oppure essere membro del ruolo predefinito del server sysadmin o dei ruoli predefiniti del database `db_ddladmin` e `db_owner`.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-spatial-index-on-a-geometry-column"></a>R. Creazione di un indice spaziale in una colonna geometrica
Nell'esempio seguente viene creata una tabella denominata `SpatialTable` contenente una colonna del tipo **geometry**, `geometry_col`. Viene quindi creato un indice spaziale, `SIndx_SpatialTable_geometry_col1`, in `geometry_col` Nell'esempio viene utilizzato lo schema a mosaico predefinito e viene specificato il rettangolo di selezione.  
  
```sql  
CREATE TABLE SpatialTable(id int primary key, geometry_col geometry);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col1
   ON SpatialTable(geometry_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ) );  
```  
  
### <a name="b-creating-a-spatial-index-on-a-geometry-column"></a>B. Creazione di un indice spaziale in una colonna geometrica
Nell'esempio seguente viene creato un secondo indice spaziale, `SIndx_SpatialTable_geometry_col2`, nella colonna `geometry_col` della tabella `SpatialTable`. Nell'esempio viene specificato `GEOMETRY_GRID` come schema a mosaico. Vengono inoltre specificati il rettangolo di selezione, densità diverse su livelli diversi della griglia e 64 celle per oggetto. Nell'esempio viene inoltre impostato il riempimento dell'indice su `ON`.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col2  
   ON SpatialTable(geometry_col)  
   USING GEOMETRY_GRID  
   WITH (  
    BOUNDING_BOX = ( xmin=0, ymin=0, xmax=500, ymax=200 ),  
    GRIDS = (LOW, LOW, MEDIUM, HIGH),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="c-creating-a-spatial-index-on-a-geometry-column"></a>C. Creazione di un indice spaziale in una colonna geometrica
Nell'esempio seguente viene creato un terzo indice spaziale, `SIndx_SpatialTable_geometry_col3`, nella colonna `geometry_col` della tabella `SpatialTable`. Nell'esempio viene utilizzato lo schema a mosaico predefinito. Viene specificato il rettangolo di selezione e vengono utilizzate densità di celle diverse per il terzo e il quarto livello, mentre viene utilizzato il numero predefinito di celle per oggetto.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col3  
   ON SpatialTable(geometry_col)  
   WITH (  
    BOUNDING_BOX = ( 0, 0, 500, 200 ),  
    GRIDS = ( LEVEL_4 = HIGH, LEVEL_3 = MEDIUM ) );  
```  
  
### <a name="d-changing-an-option-that-is-specific-to-spatial-indexes"></a>D. Modifica di un'opzione specifica degli indici spaziali
Nell'esempio seguente viene ricompilato l'indice spaziale creato nell'esempio precedente, `SIndx_SpatialTable_geography_col3`, specificando una nuova densità per `LEVEL_3` con DROP_EXISTING = ON.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable(geography_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ),  
        GRIDS = ( LEVEL_3 = LOW ),  
        DROP_EXISTING = ON );  
```  
  
### <a name="e-creating-a-spatial-index-on-a-geography-column"></a>E. Creazione di un indice spaziale in una colonna geografica
Nell'esempio seguente viene creata una tabella denominata `SpatialTable2` contenente una colonna del tipo **geography**, `geography_col`. Viene quindi creato un indice spaziale, `SIndx_SpatialTable_geography_col1`, in `geography_col` e vengono utilizzati i valori dei parametri predefiniti dello schema a mosaico GEOGRAPHY_AUTO_GRID.  
  
```sql  
CREATE TABLE SpatialTable2(id int primary key, object GEOGRAPHY);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col1
   ON SpatialTable2(object);  
```  
  
> [!NOTE]  
> Per gli indici delle griglie geografiche non è possibile specificare un rettangolo di selezione.
  
### <a name="f-creating-a-spatial-index-on-a-geography-column"></a>F. Creazione di un indice spaziale in una colonna geografica
Nell'esempio seguente viene creato un secondo indice spaziale, `SIndx_SpatialTable_geography_col2`, nella colonna `geography_col` della tabella `SpatialTable2`. Nell'esempio viene specificato `GEOGRAPHY_GRID` come schema a mosaico. Vengono inoltre specificate densità della griglia diverse su livelli diversi e 64 celle per oggetto. Nell'esempio viene inoltre impostato il riempimento dell'indice su `ON`.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col2  
   ON SpatialTable2(object)  
   USING GEOGRAPHY_GRID  
   WITH (  
    GRIDS = (MEDIUM, LOW, MEDIUM, HIGH ),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="g-creating-a-spatial-index-on-a-geography-column"></a>G. Creazione di un indice spaziale in una colonna geografica
Nell'esempio seguente viene creato un terzo indice spaziale, `SIndx_SpatialTable_geography_col3`, nella colonna `geography_col` della tabella `SpatialTable2`. Nell'esempio vengono utilizzati lo schema a mosaico predefinito, GEOGRAPHY_GRID, e il valore di CELLS_PER_OBJECT predefinito (16).  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable2(object)  
   WITH ( GRIDS = ( LEVEL_3 = HIGH, LEVEL_2 = HIGH ) );  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)       
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)       
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)       
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)       
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)       
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)       
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)       
[DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)       
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)       
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)       
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)       
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)       
[sys.spatial_index_tessellations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)       
[sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)       
[Panoramica degli indici spaziali](../../relational-databases/spatial/spatial-indexes-overview.md)       
