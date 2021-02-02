---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3c5c23b2cdccf464ec86d88f50073143a979ca8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237117"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Specifica le proprietà di una colonna che vengono aggiunte a una tabella tramite [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *column_name*  
 Nome della colonna da modificare, aggiungere o eliminare. *column_name* può essere costituito da un numero di caratteri compreso tra 1 e 128. Nel caso di nuove colonne create con il tipo di dati timestamp, è possibile omettere *column_name*. Se non viene specificato il nome *column_name* per una colonna con il tipo di dati **timestamp**, viene usato il nome **timestamp**.  
  
 [ _type_schema_name_ **.** ] *type_name*  
 Tipo di dati della colonna che viene aggiunta e dello schema a cui appartiene.  
  
 *TYPE_NAME* può essere:  
  
-   Tipo di dati di sistema di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Tipo di dati alias basato su un tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per poter essere utilizzati in una definizione di tabella, i tipi di dati alias devono venire creati tramite CREATE TYPE.  
  
-   Tipo definito dall'utente (UDT) di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] e schema a cui appartiene. Per poter essere utilizzato in una definizione di tabella, un tipo definito dall'utente (UDT) di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] deve venire creato tramite CREATE TYPE.  
  
 Se *type_schema_name* non viene specificato, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] fa riferimento a *type_name* in base all'ordine seguente:  
  
-   Tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Schema predefinito dell'utente corrente nel database corrente.  
  
-   Schema **dbo** nel database corrente.  
  
*precisione*  
 Precisione del tipo di dati specificato. Per altre informazioni sui valori di precisione validi, vedere [Precisione, scala e lunghezza&#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
*scale*  
 Scala per il tipo di dati specificato. Per altre informazioni sui valori di scala validi, vedere [Precisione, scala e lunghezza &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
**max**  
 Si applica solo al tipo di dati **varchar**, **nvarchar** e **varbinary**. Vengono utilizzati per l'archiviazione di 2^31 byte di dati di tipo carattere e binari e 2^30 byte di dati Unicode.  
  
**CONTENT**  
 Specifica che ogni istanza del tipo di dati **xml** in *column_name* può contenere più elementi principali. CONTENT si applica solo al tipo di dati **xml** e può essere specificato solo se viene specificato anche *xml_schema_collection*. Se viene omesso, per impostazione predefinita viene utilizzato CONTENT.  
  
DOCUMENT  
 Specifica che ogni istanza del tipo di dati **xml** in *column_name* può contenere più elementi principali. DOCUMENT si applica solo al tipo di dati **xml** e può essere specificato solo se viene specificato anche *xml_schema_collection*.  
  
 *xml_schema_collection*  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Si applica solo al tipo di dati **xml** per associare una raccolta di XML Schema al tipo. Per poter digitare una colonna **xml** in uno schema, è necessario creare lo schema nel database usando [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
FILESTREAM  
 Facoltativamente specifica l'attributo di archiviazione FILESTREAM per la colonna con *type_name* di **varbinary(max)** .  
  
 Quando viene specificato FILESTREAM per una colonna, la tabella deve avere anche una colonna per il tipo di dati **uniqueidentifier** con l'attributo ROWGUIDCOL. Questa colonna non deve consentire valori Null e deve avere un vincolo a colonna singola UNIQUE o PRIMARY KEY. Il valore GUID per la colonna deve essere fornito da un'applicazione all'inserimento dei dati o da un vincolo DEFAULT che utilizza la funzione NEWID ().  
  
 Non è possibile eliminare la colonna ROWGUIDCOL, né modificare i vincoli correlati quando per la tabella è stata definita una colonna FILESTREAM. La colonna ROWGUIDCOL può essere eliminata solo dopo l'eliminazione dell'ultima colonna FILESTREAM.  
  
 Quando l'attributo di archiviazione FILESTREAM viene specificato per una colonna, tutti i valori per quella colonna vengono archiviati in un contenitore di dati FILESTREAM nel file system.  
  
 Per un esempio che illustri come usare la definizione di colonna, vedere [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
COLLATE *collation_name*  
 Specifica le regole di confronto per la colonna. Se viene omesso, alla colonna vengono assegnate le regole di confronto predefinite del database. È possibile usare nomi di regole di confronto di Windows o SQL. Per un elenco e altre informazioni, vedere [Nome delle regole di confronto di Windows; &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) e [Nome delle regole di confronto di SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 La clausola COLLATE consente di specificare le regole di confronto solo delle colonne dei tipi di dati **char**, **varchar**, **nchar** e **nvarchar**.  
  
 Per altre informazioni sulla clausola COLLATE, vedere [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 NULL | NOT NULL  
 Determina se i valori Null sono supportati nella colonna. L'opzione NULL non è esattamente un vincolo, ma può essere specificata allo stesso modo di NOT NULL.  
  
[ CONSTRAINT *constraint_name* ]  
 Specifica l'inizio di una definizione del valore DEFAULT. Per garantire la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile assegnare un nome di vincolo a una definizione DEFAULT. *constraint_name* deve essere conforme alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md), con l'eccezione che il nome non può iniziare con il simbolo di cancelletto (#). Se *constraint_name* non viene specificato, alla definizione DEFAULT viene assegnato un nome generato dal sistema.  
  
DEFAULT  
 Parola chiave che specifica il valore predefinito della colonna. Le definizioni DEFAULT possono essere utilizzate per assegnare valori a una nuova colonna nelle righe di dati esistenti. Le definizioni DEFAULT non possono essere applicate alle colonne **timestamp** o alle colonne con una proprietà IDENTITY. Se si specifica un valore predefinito per una colonna di tipo definito dall'utente, il tipo deve supportare la conversione implicita da *constant_expression* nel tipo definito dall'utente.  
  
*constant_expression*  
 Valore letterale, valore Null o funzione di sistema utilizzati come valore predefinito della colonna. Se usato insieme a una colonna definita come tipo definito dall'utente di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], l'implementazione del tipo deve supportare una conversione implicita da *constant_expression* nel tipo definito dall'utente.  
  
WITH VALUES   
 Quando si aggiunge una colonna e un vincolo DEFAULT, se la colonna consente valori NULL con WITH VALUES, per le righe esistenti, il valore della nuova colonna verrà impostato su valore specificato in DEFAULT *constant_expression*. Se la colonna da aggiungere non consente valori NULL, per le righe esistenti, il valore della colonna verrà sempre impostato sul valore specificato in DEFAULT *constant_expression*. A partire da SQL Server 2012 questa potrebbe essere un'operazione sui metadati (vedere [Aggiunta di colonne NOT NULL come operazione online](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation)).
Se viene usata quando non viene aggiunta anche la colonna correlata, non ha alcun effetto.
 
 Specifica che il valore assegnato in DEFAULT *constant_expression* viene archiviato in una nuova colonna aggiunta alle righe esistenti. Se la colonna aggiunta ammette valori Null e viene specificata la clausola WITH VALUES, il valore predefinito viene archiviato nella nuova colonna aggiunta alle righe esistenti. Se la clausola WITH VALUES non viene specificata per le colonne che consentono valori Null, il valore NULL viene archiviato nella nuova colonna nelle righe esistenti. Se la nuova colonna non ammette valori Null, il valore predefinito viene archiviato nelle nuove righe, indipendentemente dal fatto che la clausola WITH VALUES sia o meno specificata.  
  
IDENTITY  
 Specifica che la nuova colonna è una colonna Identity. Il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] fornisce un valore univoco incrementale per la colonna. Quando si aggiungono colonne identificatore alle tabelle esistenti, alle righe esistenti della tabella vengono aggiunti i valori Identity con i valori di inizializzazione e di incremento. L'ordine con cui le righe vengono aggiornate non è prevedibile. Vengono inoltre generati valori Identity per qualsiasi nuova riga aggiunta.  
  
 Le colonne Identity vengono comunemente utilizzate in combinazione con vincoli PRIMARY KEY per fungere da identificatore di riga univoco per la tabella. La proprietà IDENTITY può essere assegnata a una colonna **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Ogni tabella può includere una sola colonna Identity. Per una colonna Identity inoltre non è possibile utilizzare la parola chiave DEFAULT con associati valori predefiniti. È necessario specificare sia il valore di inizializzazione che l'incremento oppure nessuno dei due valori. Nel secondo caso il valore predefinito è (1,1).  
  
> [!NOTE]  
>  Non è possibile modificare una colonna della tabella esistente per aggiungere la proprietà IDENTITY.  
  
 Non è possibile aggiungere una colonna Identity a una tabella pubblicata perché questo può impedire la convergenza quando le colonne vengono replicate nel Sottoscrittore. I valori della colonna Identity nel server di pubblicazione dipendono dall'ordine in cui vengono fisicamente archiviate le righe della tabella interessata. Le righe possono essere archiviate in modo diverso nel Sottoscrittore, pertanto il valore della colonna Identity può essere diverso per le stesse righe.  
  
 Per disabilitare la proprietà IDENTITY di una colonna consentendo l'inserimento esplicito dei valori, usare [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md).  
  
*seed*  
 Valore utilizzato per la prima riga caricata nella tabella.  
  
*increment*  
 Valore incrementale aggiunto al valore Identity della riga precedente caricata.  
  
NOT FOR REPLICATION  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Può essere specificato per la proprietà IDENTITY. Se si specifica questa clausola per la proprietà IDENTITY, i valori non vengono incrementati nelle colonne Identity quando gli agenti di replica eseguono operazioni di inserimento.  
  
ROWGUIDCOL  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica che la colonna è una colonna GUID di riga. È possibile assegnare ROWGUIDCOL solo a una colonna **uniqueidentifier** e solo una colonna **uniqueidentifier** per tabella può essere designata come colonna ROWGUIDCOL. Non è possibile assegnare ROWGUIDCOL a colonne di tipo definito dall'utente (UDT).  
  
 La proprietà ROWGUIDCOL non impone l'univocità dei valori archiviati nella colonna. Inoltre non genera automaticamente valori per le nuove righe inserite nella tabella. Per generare valori univoci per ogni colonna, è necessario utilizzare la funzione NEWID con istruzioni INSERT o specificare la funzione NEWID come valore predefinito della colonna. Per altre informazioni, vedere [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md) e [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
SPARSE  
 Indica che la colonna è di tipo sparse. L'archiviazione delle colonne di tipo sparse è ottimizzata per valori Null. Non è possibile designare le colonne di tipo sparse come NOT NULL. Per altre restrizioni e informazioni relative alle colonne di tipo sparse, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).  
  
\<column_constraint>  
 Per le definizioni degli argomenti di vincolo di colonna, vedere [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md).  
  
 ENCRYPTED WITH  
 Specifica la crittografia di colonne usando la funzionalità [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
 COLUMN_ENCRYPTION_KEY = *key_name*  
 Specifica la chiave di crittografia della colonna. Per altre informazioni, vedere [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md).  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 La **crittografia deterministica** usa un metodo che genera sempre lo stesso valore crittografato per qualsiasi valore di testo normale specificato. L'uso della crittografia deterministica consente di eseguire operazioni di ricerca usando il confronto di uguaglianza, il raggruppamento e il join di tabelle con join di uguaglianza basati su valori crittografati, ma può anche consentire a utenti non autorizzati di ipotizzare informazioni sui valori crittografati esaminando i criteri nella colonna crittografata. È possibile creare un join di due tabelle nelle colonne crittografate in modo deterministico solo se entrambe le colonne vengono crittografate usando la stessa chiave di crittografia della colonna. La crittografia deterministica deve usare regole di confronto a livello di colonna con un ordinamento binario2 per colonne di tipo carattere.  
  
 La **crittografia casuale** usa un metodo di crittografia dei dati meno prevedibile. La crittografia casuale è più sicura, ma impedisce i calcoli e l'indicizzazione delle colonne crittografate, a meno che l'istanza di SQL Server non supporti [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).
  
 Se si usa Always Encrypted (senza enclave sicuri), usare la crittografia deterministica per le colonne in cui eseguire la ricerca con parametri o parametri di raggruppamento, ad esempio un numero ID per gli enti pubblici. Usare la crittografia casuale per i dati, ad esempio il numero di carta di credito, che non sono raggruppati con altri record o usati per il join di tabelle e in cui non vengono eseguite ricerche perché si usano altre colonne, ad esempio un numero di transazione, per trovare la riga che contiene la colonna crittografata che interessa.  

 Se si usa Always Encrypted con enclave sicuri, la crittografia casuale è il tipo di crittografia consigliato.  
  
 Le colonne devono essere di un tipo di dati idoneo.  
  
ALGORITHM  
**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
Deve essere **'AEAD_AES_256_CBC_HMAC_SHA_256'** .  
  
 Per altre informazioni sui vincoli della funzionalità, vedere [Always Encrypted &#40;Motore di database&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
   
ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
 **Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Specifica una maschera dati dinamica. *mask_function* è il nome della funzione di maschera con i parametri appropriati. Sono disponibili le funzioni seguenti:  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 Per i parametri di funzione, vedere [Mascheramento dati dinamici](../../relational-databases/security/dynamic-data-masking.md).  
  
## <a name="remarks"></a>Osservazioni  
 Se viene aggiunta una colonna con tipo di dati **uniqueidentifier**, può essere definita con un valore predefinito che usa la funzione NEWID() per l'inserimento dei valori di identificatore univoco nella nuova colonna per ogni riga esistente nella tabella.  
  
 In [!INCLUDE[ssDE](../../includes/ssde-md.md)] non è previsto un ordine specifico per DEFAULT, IDENTITY, ROWGUIDCOL o vincoli di colonna in una definizione di colonna.  
  
 L'istruzione ALTER TABLE avrà esito negativo se, a seguito dell'aggiunta della colonna, la dimensione della riga di dati supererà 8060 byte.  
  
## <a name="examples"></a>Esempi  
 Per esempi, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
