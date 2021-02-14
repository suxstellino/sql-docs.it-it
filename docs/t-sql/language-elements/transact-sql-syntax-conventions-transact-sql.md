---
description: Convenzioni della sintassi Transact-SQL (Transact-SQL)
title: Convenzioni della sintassi Transact-SQL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.TSQLExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- conventions [SQL Server]
- Applies to section in Transact-SQL topics
- code example conventions [SQL Server]
- objects [SQL Server], names
- code [SQL Server], conventions
- multipart names [SQL Server]
- Transact-SQL syntax conventions
- syntax conventions [SQL Server]
- code [SQL Server]
- Transact-SQL
- naming conventions [SQL Server]
- syntax [SQL Server], Transact-SQL
ms.assetid: 35fbcf7f-8b55-46cd-a957-9b8c7b311241
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed6fee3cc0fae70b0258bc427322a66a32f39eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340637"
---
# <a name="transact-sql-syntax-conventions-transact-sql"></a>Convenzioni della sintassi Transact-SQL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Nella tabella seguente vengono elencate e descritte le convenzioni utilizzate nei diagrammi della sintassi nella Guida di riferimento a [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Convenzione|Utilizzo|  
|----------------|--------------|  
|LETTERE MAIUSCOLE|Parole chiave [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|_corsivo_|Parametri della sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] specificati dall'utente.|  
|**grassetto**|Digitare nomi di database, tabelle, colonne, indici, stored procedure, utilità, tipi di dati e testo esattamente come indicato.|  
|sottolineato|Indica il valore predefinito applicato quando la clausola che include il valore sottolineato viene omessa dall'istruzione.|  
|&#124; (barra verticale)|Separazione di elementi sintattici racchiusi tra parentesi quadre o graffe. Indica che è possibile utilizzare solo uno degli elementi.|  
|`[ ]` (parentesi quadre)|Elementi sintattici facoltativi. Le parentesi quadre non devono essere digitate.|  
|{ } (parentesi graffe)|Elementi sintattici obbligatori. Le parentesi graffe non devono essere digitate.|  
|[ **,** ..._n_]|Indica che l'elemento precedente può essere ripetuto _n_ volte. Le occorrenze sono separate da virgole.|  
|[..._n_]|Indica che l'elemento precedente può essere ripetuto _n_ volte. Le varie occorrenze dell'elemento sono separate da spazi.|  
|;|Carattere di terminazione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Anche se per la maggior parte delle istruzioni in questa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è necessario usare il punto e virgola, questo requisito verrà introdotto in una versione futura.|  
|\<label> ::=|Nome di un blocco della sintassi. Usare questa convenzione per raggruppare ed etichettare sezioni di sintassi estese oppure un'unità sintattica che è possibile usare in più posizioni di un'istruzione. Ogni posizione in cui si potrebbe usare il blocco di sintassi è indicata dall'etichetta racchiusa tra parentesi angolari: \<label>.<br /><br /> Un set è una raccolta di espressioni, ad esempio \<grouping set>, mentre un elenco è una raccolta di set, ad esempio \<composite element list>.|  
  
## <a name="multipart-names"></a>Nomi composti da più parti  
Se non specificato diversamente, tutti i riferimenti [!INCLUDE[tsql](../../includes/tsql-md.md)] al nome di un oggetto di database possono essere composti da quattro elementi nel formato seguente:  
  
_server\_name_.[_database\_name_].[_schema\_name_]._object\_name_  
  
| _database\_name_.[_schema\_name_]._object\_name_  
 
| _schema\_name_._object\_name_  
  
| _object\_name_  
  
_server\_name_  
Specifica il nome del server collegato o remoto.  
  
_database\_name_  
Specifica il nome di un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quando l'oggetto è contenuto in una istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'oggetto è disponibile in un server collegato, *database_name* specifica un catalogo OLE DB.  
  
_schema\_name_  
Specifica il nome dello schema che contiene l'oggetto se l'oggetto è disponibile in un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'oggetto è disponibile in un server collegato, *schema_name* specifica il nome di uno schema OLE DB.  
  
_object\_name_  
Fa riferimento al nome dell'oggetto.  
  
Quando si fa riferimento a un oggetto specifico, non è sempre necessario specificare server, database e schema per l'identificazione dell'oggetto in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Se tuttavia l'oggetto non viene trovato viene restituito un errore.  
  
> [!NOTE]  
> Per evitare errori di risoluzione dei nomi, è consigliabile specificare il nome dello schema ogni volta che si specifica un oggetto con ambito schema.  
  
Per omettere i nodi intermedi, contrassegnare queste posizioni con un punto. Nella tabella seguente sono inclusi i formati validi per i nomi di oggetto.  
  
|Formato per i riferimenti agli oggetti.|Descrizione|  
|-----------------------------|-----------------|  
|_server_._database_._schema_._object_|Nome composto da quattro parti.|  
|_server_._database_.._object_|Il nome dello schema viene omesso.|  
|_server_.._schema_._object_|Il nome del database viene omesso.|  
|_server_..._object_|Il nome del database e dello schema viene omesso.|  
|_database_._schema_._object_|Il nome del server viene omesso.|  
|_database_.._object_|Il nome del server e del database viene omesso.|  
|_schema_._object_|Il nome del server e del database viene omesso.|  
|_object_|Il nome dello schema, del database e del server viene omesso.|  
  
## <a name="code-example-conventions"></a>Convenzioni del codice di esempio  
Se non indicato diversamente, gli esempi inclusi nella Guida di riferimento a [!INCLUDE[tsql](../../includes/tsql-md.md)] sono stati testati tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e le relative impostazioni predefinite per le opzioni seguenti:  
  
-   ANSI_NULLS  
-   ANSI_NULL_DFLT_ON  
-   ANSI_PADDING  
-   ANSI_WARNINGS  
-   CONCAT_NULL_YIELDS_NULL  
-   QUOTED_IDENTIFIER  
  
La maggior parte degli esempi di codice inclusi nella Guida di riferimento a [!INCLUDE[tsql](../../includes/tsql-md.md)] è stata testata in server in cui viene utilizzato un criterio di ordinamento con distinzione tra maiuscole e minuscole e in cui è attiva la tabella codici ANSI/ISO 1252.  
  
In molti esempi di codice viene usata la lettera **N** come prefisso per le costanti stringa di caratteri Unicode. Senza il prefisso **N**, la stringa viene convertita nella tabella codici predefinita del database. La tabella codici predefinita potrebbe non riconoscere certi caratteri.  
  
## <a name="applies-to-references"></a>Riferimenti per "Si applica a"  
Le informazioni di riferimento per [!INCLUDE[tsql](../../includes/tsql-md.md)] includono articoli correlati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)].   

Nella parte superiore di ogni articolo c'è una sezione che indica i prodotti che supportano l'oggetto dell'articolo. Se un prodotto viene omesso, la funzionalità illustrata dall'articolo non è disponibile in quel prodotto. Ad esempio, i gruppi di disponibilità sono stati introdotti in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L'articolo **CREATE AVAILABILITY GROUP** indica che è applicabile a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive) poiché non è applicabile a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Il tema generale dell'articolo potrebbe essere usato in un prodotto, ma in alcuni casi non sono supportati tutti gli argomenti. Ad esempio gli utenti di database indipendenti sono stati introdotti in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L'istruzione **CREATE USER** si può usare in qualsiasi prodotto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ma la sintassi **WITH PASSWORD** non può essere usata nelle versioni meno recenti. Alcune sezioni **Si applica a** aggiuntive vengono inserite nelle descrizioni di argomento appropriate nel corpo dell'articolo.  
  
## <a name="see-also"></a>Vedere anche  
[Guida di riferimento a Transact-SQL &#40;Motore di database&#41;](../language-reference.md)    
[Parole chiave riservate &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)      
[Problemi di progettazione di Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193411(v=vs.100))    
[Problemi relativi alla denominazione di Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193246(v=vs.100))        
[Problemi relativi alle prestazioni di Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd172117(v=vs.100))