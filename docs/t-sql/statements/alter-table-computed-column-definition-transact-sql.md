---
description: ALTER TABLE computed_column_definition (Transact-SQL)
title: computed_column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER TABLE statement
ms.assetid: 746eabda-3b4f-4940-b0b5-1c379f5cf7a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bee0490c99746365998caa1723c6ccf53377dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185957"
---
# <a name="alter-table-computed_column_definition-transact-sql"></a>ALTER TABLE computed_column_definition (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica le proprietà di una colonna calcolata aggiunta a una tabella tramite l'istruzione [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
column_name AS computed_column_expression  
[ PERSISTED [ NOT NULL ] ]  
[   
    [ CONSTRAINT constraint_name ]  
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [ WITH FILLFACTOR = fillfactor ]  
        [ WITH ( <index_option> [, ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ) | filegroup   
            | "default" } ]  
    | [ FOREIGN KEY ]   
        REFERENCES ref_table [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE } ]   
        [ ON UPDATE { NO ACTION } ]   
        [ NOT FOR REPLICATION ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*column_name*  
 Nome della colonna da modificare, aggiungere o eliminare. *column_name* può avere un numero di caratteri compreso tra 1 e 128. Nel caso di nuove colonne, è possibile omettere *column_name* per le colonne che sono state create con il tipo di dati **timestamp**. Se non viene specificato il nome *column_name* per una colonna con il tipo di dati **timestamp**, viene usato il nome **timestamp**.  
  
*computed_column_expression*  
 Espressione che definisce il valore di una colonna calcolata. Una colonna calcolata è una colonna virtuale non archiviata fisicamente nella tabella, ma calcolata in base a un'espressione che utilizza altre colonne nella stessa tabella. Un'espressione deve restituire un valore. La definizione di una colonna calcolata potrebbe essere: cost AS price * qty. Un altro esempio con gli operatori bit per bit: is_finalised is_checked | is_approved. L'espressione può essere un nome di colonna non calcolata, una costante, una funzione, una variabile e qualsiasi combinazione di questi elementi uniti da uno o più operatori. L'espressione non può essere una condizione di ricerca, una sottoquery o includere dati di tipo alias.  
  
 È possibile utilizzare colonne calcolate in elenchi di selezione, clausole WHERE, clausole ORDER BY e nelle posizioni in cui è possibile utilizzare espressioni regolari, con le eccezioni seguenti:  
  
-   Una colonna calcolata non può essere utilizzata come definizione di vincolo DEFAULT o FOREIGN KEY o con la definizione di vincolo NOT NULL. È tuttavia possibile utilizzare una colonna calcolata come colonna chiave di un indice o come parte di un vincolo PRIMARY KEY o UNIQUE, a condizione che il valore della colonna calcolata sia definito da un'espressione deterministica e il tipo di dati del risultato sia supportato nelle colonne dell'indice.  
  
     Se, ad esempio, la tabella contiene le colonne di tipo integer a e b, è possibile indicizzare la colonna calcolata a + b, ma non la colonna calcolata a+DATEPART(dd, GETDATE()), in quanto durante chiamate successive il valore potrebbe cambiare.  
  
-   Non è possibile usare una colonna calcolata in un'istruzione INSERT o UPDATE.  
  
    > [!NOTE]  
    >  Poiché valori delle colonne coinvolte in una colonna calcolata possono essere diversi in ogni riga di una tabella, è possibile che nelle varie righe il risultato della colonna calcolata sia diverso.  
  
PERSISTED  
 Specifica che [!INCLUDE[ssDE](../../includes/ssde-md.md)] archivierà fisicamente i valori calcolati nella tabella e aggiornerà i valori in caso di aggiornamento di eventuali altre colonne da cui dipende la colonna calcolata. Se si contrassegna una colonna calcolata come PERSISTED, sarà possibile creare un indice in una colonna calcolata deterministica, ma non precisa. Per altre informazioni, vedere [Indici per le colonne calcolate](../../relational-databases/indexes/indexes-on-computed-columns.md). Le colonne calcolate utilizzate come colonne di partizionamento di una tabella partizionata devono essere contrassegnate con PERSISTED in modo esplicito. Se è specificato PERSISTED, il valore di *computed_column_expression* deve essere deterministico. 

NULL | NOT NULL  
 Specifica se i valori NULL sono supportati. L'opzione NULL non è esattamente un vincolo, ma può essere specificata allo stesso modo di NOT NULL. È possibile specificare NOT NULL per le colonne calcolate solo se è specificato PERSISTED.  
  
CONSTRAINT  
 Specifica l'inizio della definizione di un vincolo PRIMARY KEY o UNIQUE.  
  
*constraint_name*  
 Nuovo vincolo. I nomi di vincolo devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md), con l'eccezione che il nome non può iniziare con il simbolo di cancelletto (#). Se *constraint_name* viene omesso, al vincolo viene assegnato un nome generato dal sistema.  
  
PRIMARY KEY  
 Vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. È possibile creare un solo vincolo PRIMARY KEY per ogni tabella.  
  
UNIQUE  
 Vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco.  
  
CLUSTERED | NONCLUSTERED  
 Imposta la creazione di un indice cluster o non cluster per il vincolo PRIMARY KEY o UNIQUE. Per impostazione predefinita per i vincoli PRIMARY KEY è impostata l'opzione CLUSTERED. Per impostazione predefinita per i vincoli UNIQUE è impostata l'opzione NONCLUSTERED.  
  
 Se in una tabella esiste già un vincolo o un indice cluster, non è possibile specificare l'opzione CLUSTERED. In questo caso, inoltre, i vincoli PRIMARY KEY sono impostati su NONCLUSTERED.  
  
WITH FILLFACTOR =*fillfactor*  
 Specifica la percentuale utilizzata da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] per riempire ogni pagina dell'indice utilizzata per archiviare dati dell'indice. I valori per *fillfactor* specificati dall'utente possono essere compresi tra 1 e 100. Se non viene specificato alcun valore, il valore predefinito è 0.  
  
> [!IMPORTANT]  
>  WITH FILLFACTOR = *fillfactor* è documentata come unica opzione di indice per i vincoli PRIMARY KEY o UNIQUE solo per motivi di compatibilità con le versioni precedenti. Non sarà più documentata in questo senso nelle versioni future. È possibile specificare altre opzioni di indice nella clausola [index_option &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-index-option-transact-sql.md) di ALTER TABLE.  
  
FOREIGN KEY REFERENCES  
 Vincolo che impone l'integrità referenziale dei dati di una o più colonne. È necessario che ogni valore delle colonne sia incluso nelle colonne di riferimento corrispondenti della tabella di riferimento. È possibile fare riferimento a vincoli FOREIGN KEY solo nelle colonne che nella tabella con riferimenti corrispondono a vincoli PRIMARY KEY o UNIQUE e nelle colonne a cui viene fatto riferimento in un indice univoco nella tabella di riferimento. È necessario contrassegnare come PERSISTED anche le chiavi esterne nelle colonne calcolate.  
  
*ref_table*  
 Nome della tabella a cui fa riferimento il vincolo FOREIGN KEY.  
  
(*ref_column* )  
 Colonna della tabella a cui fa riferimento il vincolo FOREIGN KEY.  
  
ON DELETE { **NO ACTION** | CASCADE }  
 Specifica l'azione eseguita nelle righe della tabella se tali righe includono una relazione referenziale e se la riga a cui viene fatto riferimento viene eliminata dalla tabella principale. Il valore predefinito è NO ACTION.  
  
NO ACTION  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore e viene eseguito il rollback dell'azione di eliminazione della riga nella tabella padre.

CASCADE  
 Le righe corrispondenti vengono eliminate dalla tabella di riferimento se la riga viene eliminata dalla tabella padre.  
  
 Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], ad esempio, la tabella ProductVendor include una relazione referenziale con la tabella Vendor. La chiave esterna ProductVendor.BusinessEntityID fa riferimento alla chiave primaria Vendor.BusinessEntityID.  
  
 Se viene eseguita un'istruzione DELETE in una riga della tabella Vendor e viene specificata un'azione ON DELETE CASCADE per ProductVendor.BusinessEntityID, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica se esistono una o più righe dipendenti nella tabella ProductVendor. Se la verifica ha esito positivo, le righe dipendenti nella tabella ProductVendor vengono eliminate assieme alla riga a cui viene fatto riferimento nella tabella Vendor.  
  
 Al contrario, se viene specificato NO ACTION, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue in rollback dell'azione di eliminazione per la riga della tabella Vendor quando esiste almeno una riga nella tabella ProductVendor che vi fa riferimento.  
  
 Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici. Per altre informazioni sui record logici, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
ON UPDATE { **NO ACTION** }  
 Specifica l'azione eseguita nelle righe della tabella creata se tali righe includono una relazione referenziale e la riga a cui viene fatto riferimento è stata aggiornata nella tabella padre. Se si specifica NO ACTION, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore e viene eseguito il rollback dell'operazione di aggiornamento nella riga Vendor se esiste almeno una riga nella tabella ProductVendor che fa riferimento a essa.  
  
NOT FOR REPLICATION  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Questa clausola può essere specificata per i vincoli FOREIGN KEY e CHECK. Se per un vincolo si specifica questa clausola, il vincolo non viene imposto quando gli agenti di replica eseguono le operazioni di inserimento, aggiornamento o eliminazione.  
  
CHECK  
 Vincolo che impone l'integrità di dominio tramite la limitazione dei valori che è possibile inserire in una o più colonne. È necessario contrassegnare come PERSISTED anche i vincoli CHECK nelle colonne calcolate.  
  
*logical_expression*  
 Espressione logica che restituisce TRUE o FALSE. L'espressione non può contenere riferimenti a un tipo di dati alias.  
  
ON { *partition_scheme_name*(*partition_column_name*) | *filegroup*| "default"}  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica il percorso di archiviazione dell'indice creato per il vincolo. Se si specifica *partition_scheme_name*, l'indice viene partizionato e viene eseguito il mapping delle partizioni ai filegroup specificati da *partition_scheme_name*. Se si specifica *filegroup* l'indice viene creato nel filegroup specificato. Se si specifica "default" o se si omette ON, l'indice viene creato nello stesso filegroup della tabella. Se si specifica ON quando si aggiunge un indice cluster per un vincolo PRIMARY KEY o UNIQUE, l'intera tabella viene spostata nel filegroup specificato durante la creazione dell'indice cluster.  
  
> [!NOTE]  
>  In questo contesto, default non è una parola chiave, Si tratta di un identificatore del filegroup predefinito e deve essere delimitato, ad esempio ON "default" o ON [default]. Se si specifica "default", l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 Ogni vincolo PRIMARY KEY e UNIQUE genera un indice. Il numero di vincoli UNIQUE e PRIMARY KEY non deve generare un numero di indici della tabella maggiore di 999 nel caso di indici non cluster e maggiore di 1 nel caso di indici cluster.  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
