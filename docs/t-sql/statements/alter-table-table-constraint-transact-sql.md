---
description: ALTER TABLE table_constraint (Transact-SQL)
title: table_constraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONSTRAINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table_constraint
ms.assetid: ac2a11e0-cc77-4e27-b107-4fe5bc6f5195
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a2ebbdee1d7e56a06136ec41384782ded0fb5556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193636"
---
# <a name="alter-table-table_constraint-transact-sql"></a>ALTER TABLE table_constraint (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica le proprietà di un vincolo PRIMARY KEY, UNIQUE, FOREIGN KEY, CHECK oppure di una definizione DEFAULT aggiunta a una tabella usando [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
[ CONSTRAINT constraint_name ]   
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        (column [ ASC | DESC ] [ ,...n ] )  
        [ WITH FILLFACTOR = fillfactor   
        [ WITH ( <index_option>[ , ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ... )  
          | filegroup | "default" } ]   
    | FOREIGN KEY   
        ( column [ ,...n ] )  
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | DEFAULT constant_expression FOR column [ WITH VALUES ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 CONSTRAINT  
 Specifica l'inizio di una definizione di un vincolo PRIMARY KEY, UNIQUE, FOREIGN KEY o CHECK oppure di una definizione DEFAULT.  
  
 *constraint_name*  
 Nome del vincolo. I nomi di vincolo devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md), con l'eccezione che il nome non può iniziare con il simbolo di cancelletto (#). Se constraint_name viene omesso, al vincolo viene assegnato un nome generato dal sistema.  
  
 PRIMARY KEY  
 Vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco. È possibile creare un solo vincolo PRIMARY KEY per ogni tabella.  
  
 UNIQUE  
 Vincolo che impone l'integrità di entità per una o più colonne specificate tramite un indice univoco.  
  
 CLUSTERED | NONCLUSTERED  
 Imposta la creazione di un indice cluster o non cluster per il vincolo PRIMARY KEY o UNIQUE. Per impostazione predefinita per i vincoli PRIMARY KEY è impostata l'opzione CLUSTERED. Per impostazione predefinita per i vincoli UNIQUE è impostata l'opzione NONCLUSTERED.  
  
 Se in una tabella esiste già un vincolo o un indice cluster, non è possibile specificare l'opzione CLUSTERED. In questo caso, inoltre, i vincoli PRIMARY KEY sono impostati su NONCLUSTERED.  
  
 Le colonne con tipo di dati **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** o **image** non possono essere specificate come colonne di un indice.  
  
 *column*  
 Colonna o elenco di colonne tra parentesi utilizzate in un nuovo vincolo.  
  
 [ **ASC** | DESC ]  
 Specifica l'ordinamento della colonna o delle colonne che fanno parte dei vincoli di tabella. Il valore predefinito è ASC.  
  
 WITH FILLFACTOR **=** _fillfactor_  
 Specifica la percentuale utilizzata da [!INCLUDE[ssDE](../../includes/ssde-md.md)] per riempire ogni pagina dell'indice utilizzata per archiviare dati dell'indice. I valori per *fillfactor* specificati dall'utente possono essere compresi tra 1 e 100. Se non viene specificato alcun valore, il valore predefinito è 0.  
  
> [!IMPORTANT]  
>  WITH FILLFACTOR = *fillfactor* è documentata come unica opzione di indice per i vincoli PRIMARY KEY o UNIQUE solo per motivi di compatibilità con le versioni precedenti. Non sarà più documentata in questo senso nelle versioni future. È possibile specificare altre opzioni di indice nella clausola [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md) di ALTER TABLE.  
  
 ON { _partition\_scheme\_name_ **(** _partition\_column\_name_ **)**  | _filegroup_|  **"** default **"** }  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Specifica il percorso di archiviazione dell'indice creato per il vincolo. Se si specifica *partition_scheme_name*, l'indice viene partizionato e viene eseguito il mapping delle partizioni ai filegroup specificati da *partition_scheme_name*. Se si specifica *filegroup* l'indice viene creato nel filegroup specificato. Se si specifica **"** default **"** o si omette ON, l'indice viene creato nello stesso filegroup della tabella. Se si specifica ON quando si aggiunge un indice cluster per un vincolo PRIMARY KEY o UNIQUE, l'intera tabella viene spostata nel filegroup specificato durante la creazione dell'indice cluster.  
  
 In questo contesto, default non è una parola chiave bensì un identificatore del filegroup predefinito e deve essere delimitato, come in ON **"** default **"** o ON **[** default **]** . Se si specifica " **"** default **"** , l'opzione QUOTED_IDENTIFIER deve essere impostata su ON per la sessione corrente. Si tratta dell'impostazione predefinita.  
  
 FOREIGN KEY REFERENCES  
 Vincolo che impone l'integrità referenziale per i dati nella colonna. Per i vincoli FOREIGN KEY è necessario che ogni valore della colonna esista nella colonna specificata della tabella a cui viene fatto riferimento.  
  
 *referenced_table_name*  
 Tabella a cui fa riferimento il vincolo FOREIGN KEY.  
  
 *ref_column*  
 Colonna o elenco di colonne tra parentesi a cui fa riferimento il nuovo vincolo FOREIGN KEY.  
  
 ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT }  
 Specifica quale azione si verifica nelle righe della tabella che viene modificata se tali righe includono una relazione referenziale e se la riga a cui viene fatto riferimento viene eliminata dalla tabella padre. Il valore predefinito è NO ACTION.  
  
 NO ACTION  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] genera un errore e viene eseguito il rollback dell'azione di eliminazione della riga nella tabella padre.  
  
 CASCADE  
 Le righe corrispondenti vengono eliminate dalla tabella di riferimento se la riga viene eliminata dalla tabella padre.  
  
 SET NULL  
 Tutti i valori che costituiscono la chiave esterna vengono impostati su NULL quando viene eliminata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che le colonne chiave esterna ammettano valori Null.  
  
 SET DEFAULT  
 Tutti i valori che costituiscono la chiave esterna vengono impostati sui valori predefiniti quando viene eliminata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che per tutte le colonne chiave esterna siano definiti valori predefiniti. Se una colonna ammette valori Null e non viene impostato un valore predefinito esplicito, NULL diventa il valore predefinito implicito della colonna.  
  
 Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici. Per altre informazioni sui record logici, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Non è possibile definire ON DELETE CASCADE se esiste già un trigger INSTEAD OF per ON DELETE nella tabella che viene modificata.  
  
 Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], ad esempio, la tabella **ProductVendor** ha una relazione referenziale con la tabella **Vendor**. La chiave esterna **ProductVendor.VendorID** fa riferimento alla chiave primaria **Vendor.VendorID**.  
  
 Se viene eseguita un'istruzione DELETE in una riga della tabella **Vendor** e viene specificata un'azione ON DELETE CASCADE per **ProductVendor.VendorID**, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica se esistono una o più righe dipendenti nella tabella **ProductVendor**. In caso affermativo, le righe dipendenti della tabella **ProductVendor** vengono eliminate, oltre alla riga a cui viene fatto riferimento nella tabella **Vendor**.  
  
 Al contrario, se viene specificato NO ACTION, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore ed esegue il rollback dell'azione di eliminazione per la riga della tabella **Vendor** quando esiste almeno una riga nella tabella **ProductVendor** che vi fa riferimento.  
  
 ON UPDATE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT }  
 Specifica l'azione eseguita nelle righe della tabella modificata se tali righe includono una relazione referenziale e la riga a cui viene fatto riferimento è stata aggiornata nella tabella padre. Il valore predefinito è NO ACTION.  
  
 NO ACTION  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un errore e viene eseguito il rollback dell'azione di aggiornamento della riga nella tabella padre.  
  
 CASCADE  
 Le righe corrispondenti vengono aggiornate nella tabella di riferimento quando la riga viene aggiornata nella tabella padre.  
  
 SET NULL  
 Tutti i valori che costituiscono la chiave esterna vengono impostati su NULL quando viene aggiornata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che le colonne chiave esterna ammettano valori Null.  
  
 SET DEFAULT  
 Tutti i valori che costituiscono la chiave esterna vengono impostati sui rispettivi valori predefiniti quando viene aggiornata la riga corrispondente nella tabella padre. Per l'esecuzione di questo vincolo, è necessario che per tutte le colonne chiave esterna siano definiti valori predefiniti. Se una colonna ammette valori Null e non viene impostato un valore predefinito esplicito, NULL diventa il valore predefinito implicito della colonna.  
  
 Non specificare CASCADE se la tabella verrà inclusa in una pubblicazione di tipo merge che utilizza record logici. Per altre informazioni sui record logici, vedere [Raggruppare modifiche alle righe correlate con record logici](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Non è possibile specificare ON UPDATE CASCADE, SET NULL o SET DEFAULT se nella tabella che viene modificata esiste già un trigger INSTEAD OF per ON UPDATE.  
  
 Nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], ad esempio, la tabella **ProductVendor** ha una relazione referenziale con la tabella **Vendor**. La chiave esterna **ProductVendor.VendorID** fa riferimento alla chiave primaria **Vendor.VendorID**.  
  
 Se viene eseguita un'istruzione UPDATE per una riga della tabella **Vendor** e viene specificata un'azione ON UPDATE CASCADE per **ProductVendor.VendorID**, [!INCLUDE[ssDE](../../includes/ssde-md.md)] verifica se esistono una o più righe dipendenti nella tabella **ProductVendor**. L'eventuale riga dipendente individuata nella tabella **ProductVendor** viene aggiornata insieme alla riga a cui viene fatto riferimento nella tabella **Vendor**.  
  
 Al contrario, se si specifica NO ACTION, nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene generato un errore e viene eseguito il rollback dell'azione di aggiornamento per la riga **Vendor** quando esiste almeno una riga nella tabella **ProductVendor** che fa riferimento a essa.  
  
 NOT FOR REPLICATION  
 **Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.  
  
 Questa clausola può essere specificata per i vincoli FOREIGN KEY e CHECK. Se per un vincolo si specifica questa clausola, il vincolo non viene imposto quando gli agenti di replica eseguono le operazioni di inserimento, aggiornamento o eliminazione.  

 CONNECTION Specifica la coppia di tabelle nodi alla quale è autorizzato a connettersi il vincolo di bordo dato. ON DELETE Specifica le operazioni eseguite nelle righe della tabella bordi quando vengono eliminati i nodi connessi tramite i bordi della tabella bordi. 
 
 DEFAULT  
 Specifica il valore predefinito per la colonna. Le definizioni DEFAULT possono essere utilizzate per assegnare valori a una nuova colonna nelle righe di dati esistenti. Non è possibile aggiungere definizioni DEFAULT a colonne che hanno un tipo di dati **timestamp**, una proprietà IDENTITY, una definizione DEFAULT esistente o un valore predefinito associato. Se alla colonna è associato un valore predefinito, è necessario rimuoverlo prima di aggiungere il nuovo valore predefinito. Se si specifica un valore predefinito per una colonna di tipo definito dall'utente, il tipo deve supportare la conversione implicita da *constant_expression* nel tipo definito dall'utente. Per garantire la compatibilità con le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile assegnare un nome di vincolo a una definizione DEFAULT.  
  
 *constant_expression*  
 Valore letterale, valore Null o funzione di sistema utilizzato come valore predefinito della colonna. Se la funzione *constant_expression* viene usata insieme a una colonna definita come tipo definito dall'utente di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], l'implementazione del tipo deve supportare una conversione implicita da *constant_expression* nel tipo definito dall'utente.  
  
 FOR *column*  
 Specifica la colonna associata a una definizione DEFAULT a livello di tabella.  
  
 WITH VALUES  
 Quando si aggiunge una colonna e un vincolo DEFAULT, se la colonna consente valori NULL con WITH VALUES, per le righe esistenti, il valore della nuova colonna verrà impostato su valore specificato in DEFAULT *constant_expression*. Se la colonna da aggiungere non consente valori NULL, per le righe esistenti, il valore della colonna verrà sempre impostato sul valore specificato in DEFAULT *constant_expression*. A partire da SQL Server 2012 questa potrebbe essere un'operazione sui metadati (vedere [Aggiunta di colonne NOT NULL come operazione online](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation)).
Se viene usata quando non viene aggiunta anche la colonna correlata, non ha alcun effetto. 
  
 CHECK  
 Vincolo che impone l'integrità di dominio tramite la limitazione dei valori che è possibile inserire in una o più colonne.  
  
 *logical_expression*  
 Espressione logica utilizzata in un vincolo CHECK che restituisce TRUE o FALSE. Se usata con vincoli CHECK, *logical_expression* non può fare riferimento a un'altra tabella ma può fare riferimento ad altre colonne nella stessa tabella per la stessa riga. L'espressione non può fare riferimento a un tipo di dati alias.  
  
## <a name="remarks"></a>Osservazioni  
 Quando si aggiungono vincoli FOREIGN KEY o CHECK, in tutti i dati esistenti viene verificata la presenza di eventuali violazioni dei vincoli, a meno che non venga specificata l'opzione WITH NOCHECK. Se si verificano violazioni, l'istruzione ALTER TABLE ha esito negativo e viene restituito un errore. Quando si aggiunge un nuovo vincolo PRIMARY KEY o UNIQUE a colonne esistenti, i dati delle colonne devono essere univoci. Se vengono individuati valori duplicati, l'istruzione ALTER TABLE ha esito negativo. L'opzione WITH NOCHECK non ha alcun effetto quando si aggiungono vincoli PRIMARY KEY o UNIQUE.  
  
 Ogni vincolo PRIMARY KEY e UNIQUE genera un indice. Il numero di vincoli UNIQUE e PRIMARY KEY non deve generare un numero di indici della tabella maggiore di 999 nel caso di indici non cluster e maggiore di 1 nel caso di indici cluster. I vincoli di chiave esterna non generano automaticamente un indice. Le colonne chiave esterna, tuttavia, vengono in genere utilizzate in criteri di join nelle query confrontando le colonne nel vincolo di chiave esterna di una tabella con le colonne chiave primaria o univoca nell'altra tabella. Un indice nelle colonne chiave esterna consente a [!INCLUDE[ssDE](../../includes/ssde-md.md)] di trovare rapidamente i dati correlati nella tabella della chiave esterna.  
  
## <a name="examples"></a>Esempi  
 Per esempi, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
