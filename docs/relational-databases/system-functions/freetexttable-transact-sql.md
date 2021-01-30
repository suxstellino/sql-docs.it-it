---
description: FREETEXTTABLE (Transact-SQL)
title: FREETEXTTABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- FREETEXTTABLE_TSQL
- FREETEXTTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- search conditions [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXTTABLE function (Transact-SQL)
- ranked results [full-text search]
- column searches [full-text search]
ms.assetid: 4523ae15-4260-40a7-a53c-8df15e1fee79
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0b4fdaa1e9f583c27d2aca3657d7a5b1a86ef8a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207491"
---
# <a name="freetexttable-transact-sql"></a>FREETEXTTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Funzione utilizzata nella [clausola from](../../t-sql/queries/from-transact-sql.md) di un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione SELECT per eseguire una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ricerca full-text in colonne con indicizzazione full-text contenenti tipi di dati basati su caratteri. Questa funzione restituisce una tabella di zero, una o più righe per le colonne contenenti valori che corrispondono al significato e non solo all'esatta formulazione, del testo nel *freetext_string* specificato. Viene fatto riferimento a FREETEXTTABLE come se fosse un normale nome di tabella.  
  
 FREETEXTTABLE è utile per gli stessi tipi di corrispondenze di [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md),  
  
 Le query che utilizzano FREETEXTTABLE restituiscono un valore di classificazione per pertinenza (RANK) e una chiave full-text (KEY) per ogni riga.  
  
> [!NOTE]  
>  Per informazioni sulle forme di ricerca full-text supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md).  
  
(https://azure.microsoft.com/documentation/articles/sql-database-preview-whats-new/?WT.mc_id=TSQL_GetItTag)).|  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
FREETEXTTABLE (table , { column_name | (column_list) | * }   
          , 'freetext_string'   
     [ , LANGUAGE language_term ]   
     [ , top_n_by_rank ] )  
```  
  
## <a name="arguments"></a>Argomenti  
 *tabella*  
 Nome della tabella contrassegnata per query full-text. la *tabella* o la *vista* può essere un nome di oggetto di database costituito da una, due o tre parti. L'esecuzione della ricerca in una vista può interessare solo una tabella di base con indicizzazione full-text.  
  
 la *tabella* non può specificare un nome di server e non può essere utilizzata nelle query sui server collegati.  
  
 *column_name*  
 Nome di una o più colonne indicizzate full-text della tabella specificata nella clausola FROM. La colonna o le colonne possono essere di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** r **varbinary(max)** .  
  
 *column_list*  
 Viene indicato che è possibile specificare più colonne, separate da virgola. *column_list* deve essere racchiuso tra parentesi. La lingua di tutte le colonne di *column_list* deve essere la stessa, a meno che non sia specificato *language_term*.  
  
 \*  
 Specifica che la ricerca della stringa specificata in *freetext_string* deve essere eseguita in tutte le colonne registrate per la ricerca full-text. A meno che non sia specificato *language_term* , la lingua di tutte le colonne con indicizzazione full-text nella tabella deve essere la stessa.  
  
 *freetext_string*  
 Testo da cercare nella colonna specificata in *column_name*. È possibile specificare qualsiasi testo, comprese parole e frasi. Vengono generate corrispondenze se nell'indice full-text viene trovato un termine o vengono trovate le forme di un termine.  
  
 A differenza di quanto avviene nella condizione di ricerca CONTAINs, dove e è una parola chiave, se utilizzata in *freetext_string* la parola ' and ' viene considerata una parola non significativa, o [parola non significativa](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md), e verrà ignorata.  
  
 Non è consentito utilizzare WEIGHT, FORMSOF, caratteri jolly, NEAR e altra sintassi. *freetext_string* viene sottoposta alla sillabazione, all'analisi morfologica e al thesaurus.  
  
 LANGUAGE *language_term*  
 Lingua le cui risorse verranno utilizzate per il word breaking, lo stemming, il thesaurus e la rimozione di parole non significative come parte della query. Questo parametro è facoltativo e può essere specificato come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) di una lingua. Se si specifica *language_term*, la lingua rappresentata dall'argomento verrà applicata a tutti gli elementi della condizione di ricerca. Se non si specifica alcun valore, verrà utilizzata la lingua full-text della colonna.  
  
 Se documenti di lingue diverse vengono archiviati insieme come oggetti BLOB in una singola colonna, l'identificatore delle impostazioni locali (LCID) di un documento specifico determina la lingua da utilizzare per indicizzarne il contenuto. Quando si esegue una query su una colonna di questo tipo, specificando la *lingua language_term* possibile aumentare la probabilità di una corrispondenza corretta.  
  
 Se l'argomento *language_term* viene specificato come stringa, corrisponde al valore della colonna **alias** nella vista di compatibilità [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).  La stringa deve essere racchiusa tra virgolette singole chiuse, come in '*language_term*'. Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale di LCID. Il valore esadecimale non deve superare le otto cifre, inclusi gli zeri iniziali.  
  
 Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirà in Unicode.  
  
 Se la lingua specificata non è valida o non vi sono risorse installate corrispondenti a tale lingua, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore. Per usare le risorse della lingua neutra, specificare 0x0 per *language_term*.  
  
 *top_n_by_rank*  
 Specifica che vengono restituite solo le *n* corrispondenze con classificazione più alta, in ordine decrescente. Si applica solo quando viene specificato un valore integer, *n*. Se il parametro *top_n_by_rank* viene combinato con altri parametri, la query potrebbe restituire un numero inferiore di righe rispetto al numero di righe effettivamente corrispondenti a tutti i predicati. *top_n_by_rank* consente di aumentare le prestazioni delle query richiamando solo i riscontri più rilevanti.  
  
## <a name="remarks"></a>Commenti  
 I predicati e le funzioni full-text possono essere utilizzati in una singola tabella, specificata in modo implicito nel predicato FROM. Per cercare in più tabelle, utilizzare una tabella unita in join nella clausola FROM, che consente di eseguire una ricerca in un set di risultati prodotto da due o più tabelle.  
  
 La funzione FREETEXTTABLE utilizza le stesse condizioni di ricerca del predicato FREETEXT.  
  
 Analogamente a CONTAINSTABLE, la tabella restituita include colonne denominate **Key** e **Rank**, a cui viene fatto riferimento all'interno della query per ottenere le righe appropriate e utilizzare i valori di classificazione delle righe.  
  
## <a name="permissions"></a>Autorizzazioni  
 La funzione FREETEXTTABLE può essere richiamata soltanto dagli utenti che dispongono delle autorizzazioni SELECT appropriate per la tabella specificata o per le colonne della tabella a cui viene fatto riferimento.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-example"></a>R. Esempio semplice  
 Nell'esempio seguente viene creata e popolata una semplice tabella di due colonne, che elenca 3 contee e i colori nei rispettivi flag. Consente di creare e popolare un indice e un catalogo full-text nella tabella. Viene quindi illustrata la sintassi **FREETEXTTABLE** .  
  
```  
CREATE TABLE Flags (Country nvarchar(30) NOT NULL, FlagColors varchar(200));  
CREATE UNIQUE CLUSTERED INDEX FlagKey ON Flags(Country);  
INSERT Flags VALUES ('France', 'Blue and White and Red');  
INSERT Flags VALUES ('Italy', 'Green and White and Red');  
INSERT Flags VALUES ('Tanzania', 'Green and Yellow and Black and Yellow and Blue');  
SELECT * FROM Flags;  
GO  
  
CREATE FULLTEXT CATALOG TestFTCat;  
CREATE FULLTEXT INDEX ON Flags(FlagColors) KEY INDEX FlagKey ON TestFTCat;  
GO   
  
SELECT * FROM Flags;  
SELECT * FROM FREETEXTTABLE (Flags, FlagColors, 'Blue');  
SELECT * FROM FREETEXTTABLE (Flags, FlagColors, 'Yellow');  
```  
  
### <a name="b-using-freetext-in-an-inner-join"></a>B. Utilizzo di FREETEXT in INNER JOIN  
 Nell'esempio seguente vengono restituiti la descrizione e il rango di tutti i prodotti con una descrizione che corrisponde al significato di `high level of performance` .  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Description  
    ,KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL   
    INNER JOIN FREETEXTTABLE(Production.ProductDescription,  
    Description,   
    'high level of performance') AS KEY_TBL  
ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY RANK DESC;  
GO  
```  
  
### <a name="c-specifying-language-and-highest-ranked-matches"></a>C. Specifica della lingua e delle corrispondenze di pertinenza maggiore  
 L'esempio seguente è identico e Mostra l'uso dei `LANGUAGE` parametri *language_term* e *top_n_by_rank* .  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Description  
    ,KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL   
    INNER JOIN FREETEXTTABLE(Production.ProductDescription,  
    Description,   
    'high level of performance',  
    LANGUAGE N'English', 2) AS KEY_TBL  
ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY RANK DESC;  
GO  
```  
  
> [!NOTE]
>  Il parametro LANGUAGE *language_term* non è necessario per usare il parametro *top_n_by_rank* .  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alla ricerca full-text](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Creare e gestire cataloghi full-text](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Creazione e gestione di indici full-text](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Eseguire query con ricerca full-text](../../relational-databases/search/query-with-full-text-search.md)   
 [Creare query di ricerca full-text &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [Funzioni per i set di righe &#40;&#41;Transact-SQL ](../../t-sql/functions/opendatasource-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [Opzione di configurazione del server precompute rank](../../database-engine/configure-windows/precompute-rank-server-configuration-option.md)  
  
