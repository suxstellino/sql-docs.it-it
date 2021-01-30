---
description: ALTER FULLTEXT INDEX (Transact-SQL)
title: ALTER FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT INDEX
- ALTER_FULLTEXT_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- modifying full-text indexes
- full-text indexes [SQL Server], modifying
- search property lists [SQL Server], associating with full-text indexes
- ALTER FULLTEXT INDEX statement
ms.assetid: b6fbe9e6-3033-4d1b-b6bf-1437baeefec3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 17981c3c04c17b0c0291714a245dfa1856a69af7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204224"
---
# <a name="alter-fulltext-index-transact-sql"></a>ALTER FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica le proprietà di un indice full-text in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER FULLTEXT INDEX ON table_name  
   { ENABLE   
   | DISABLE  
   | SET CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF }  
   | ADD ( column_name   
           [ TYPE COLUMN type_column_name ]   
           [ LANGUAGE language_term ]  
           [ STATISTICAL_SEMANTICS ]  
           [,...n]   
         )  
     [ WITH NO POPULATION ]  
   | ALTER COLUMN column_name  
     { ADD | DROP } STATISTICAL_SEMANTICS  
     [ WITH NO POPULATION ]  
   | DROP ( column_name [,...n] )  
     [ WITH NO POPULATION ]   
   | START { FULL | INCREMENTAL | UPDATE } POPULATION  
   | {STOP | PAUSE | RESUME } POPULATION   
   | SET STOPLIST [ = ] { OFF| SYSTEM | stoplist_name }  
     [ WITH NO POPULATION ]   
   | SET SEARCH PROPERTY LIST [ = ] { OFF | property_list_name }  
     [ WITH NO POPULATION ]   
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *table_name*  
 Nome della tabella o della vista indicizzata contenente la colonna o le colonne incluse nell'indice full-text. I nomi dei proprietari del database e della tabella sono facoltativi.  
  
 ENABLE | DISABLE  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se raccogliere i dati dell'indice full-text per *table_name*. ENABLE consente di attivare l'indice full-text, DISABLE di disabilitarlo. La tabella non supporterà query full-text mentre l'indice è disabilitato.  
  
 Disabilitare un indice full-text consente di disattivare il rilevamento delle modifiche conservando l'indice full-text, che può essere riattivato utilizzando ENABLE in qualsiasi momento. Quando l'indice full-text è disabilitato, i relativi metadati vengono mantenuti nelle tabelle di sistema. Se l'opzione CHANGE_TRACKING è abilitata (aggiornamento manuale o automatico) quando l'indice full-text è disabilitato, lo stato dell'indice viene bloccato, eventuali ricerche per indicizzazione in corso vengono arrestate e le nuove modifiche apportate ai dati della tabella non vengono registrate o propagate nell'indice.  
  
 SET CHANGE_TRACKING {MANUAL | AUTO | OFF}  
 Specifica se le modifiche (aggiornamenti, eliminazioni o inserimenti) apportate alle colonne della tabella coperte dall'indice full-text verranno propagate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'indice full-text. Le modifiche apportate ai dati tramite WRITETEXT e UPDATETEXT non vengono riflesse nell'indice full-text, pertanto non vengono registrate dalla funzione di rilevamento delle modifiche.  
  
> [!NOTE]  
>  Per altre informazioni, vedere [Interazioni del rilevamento delle modifiche con NO POPULATION](#change-tracking-no-population).
  
 MANUAL  
 Specifica che le modifiche rilevate verranno propagate manualmente chiamando l'istruzione ALTER FULLTEXT INDEX ... START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*popolamento manuale*). Per chiamare questa istruzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] periodicamente, è possibile utilizzare [!INCLUDE[tsql](../../includes/tsql-md.md)] Agent.  
  
 AUTO  
 Specifica che le modifiche rilevate verranno propagate automaticamente quando i dati nella tabella di base vengono modificati (*popolamento automatico*). Sebbene le modifiche vengano propagate automaticamente, tali modifiche potrebbero non risultare immediatamente nell'indice full-text. AUTO è l'impostazione predefinita.  
  
 OFF  
 Specifica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non manterrà un elenco delle modifiche apportate ai dati indicizzati.  
  
 ADD | DROP *column_name*  
 Specifica le colonne da aggiungere o eliminare in un indice full-text. La colonna o le colonne devono essere di tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** o **varbinary(max)**.  
  
 Utilizzare la clausola DROP solo per colonne abilitate in precedenza per l'indicizzazione full-text.  
  
 Usare TYPE COLUMN e LANGUAGE con la clausola ADD per impostare queste proprietà su *column_name*. Se si aggiunge una colonna, è necessario ripopolare l'indice full-text della tabella per consentire il corretto funzionamento delle query full-text su tale colonna.  
  
> [!NOTE]  
>  Il popolamento dell'indice full-text dopo l'aggiunta o l'eliminazione di una colonna varia a seconda che sia abilitato il rilevamento delle modifiche e sia specificato WITH NO POPULATION. Per altre informazioni, vedere [Interazioni del rilevamento delle modifiche con NO POPULATION](#change-tracking-no-population).
  
 TYPE COLUMN *type_column_name*  
 Specifica il nome di una colonna della tabella, *type_column_name*, usato per contenere il tipo di documento per un documento **varbinary**, **varbinary(max)** o **image**. Questa colonna, nota come colonna di tipo, contiene un'estensione di file fornita dall'utente (doc, pdf, xls e così via). La colonna del tipo deve essere di tipo **char**, **nchar**, **varchar** o **nvarchar**.  
  
 Specificare TYPE COLUMN *type_column_name* solo se *column_name* specifica una colonna **varbinary**, **varbinary(max)** o **image** in cui i dati vengono archiviati come dati binari; in caso contrario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore.  
  
> [!NOTE]  
>  Durante l'indicizzazione il motore di ricerca full-text usa l'abbreviazione nella colonna di tipo di ogni riga di tabella per identificare il filtro di ricerca full-text da usare per il documento in *column_name*. Il filtro carica il documento come flusso binario, rimuove le informazioni sulla formattazione e invia il testo dal documento al componente word breaker. Per altre informazioni, vedere [Configurazione e gestione di filtri per la ricerca](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 LANGUAGE *language_term*  
 È la lingua dei dati archiviati in **column_name**.  
  
 *language_term* è facoltativo e può essere specificato come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) di una lingua. Se si specifica *language_term*, la lingua rappresentata dall'argomento verrà applicata a tutti gli elementi della condizione di ricerca. Se non viene specificato alcun valore, verrà usata la lingua predefinita della funzionalità full-text per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Usare la stored procedure **sp_configure** per accedere alle informazioni sulla lingua predefinita della funzionalità full-text per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se l'argomento *language_term* viene specificato come stringa, corrisponde al valore della colonna **alias** nella tabella di sistema **syslanguages**. La stringa deve essere racchiusa tra virgolette singole chiuse, come in '*language_term*'. Se l'argomento *language_term* viene specificato come valore intero, corrisponde all'LCID effettivo che identifica la lingua. Se si specifica un valore esadecimale, *language_term* è 0x seguito dal valore esadecimale dell'LCID. Il valore esadecimale deve essere composto al massimo da otto cifre, zero iniziali inclusi.  
  
 Se il valore è in formato DBCS (Double-Byte Character Set), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirà in Unicode.  
  
 Per la lingua specificata in *language_term* è necessario abilitare risorse quali word breaker e stemmer. Se tali risorse non supportano la lingua specificata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituirà un errore.  
  
 Utilizzare la risorsa per la lingua neutra (0x0) per le colonne non BLOB e non XML che contengono dati di testo in più lingue oppure nei casi in cui la lingua del testo archiviato nella colonna è sconosciuta. Per i documenti archiviati in colonne di tipo XML o BLOB, in fase di indicizzazione verrà utilizzata la codifica di lingua del documento. Nelle colonne XML, ad esempio, la lingua è identificata dall'attributo xml:lang nei documenti XML. In fase di query, il valore precedentemente specificato in *language_term* diventa la lingua predefinita per le query full-text, a meno che non si specifichi *language_term* all'interno di una query full-text.  
  
 STATISTICAL_SEMANTICS  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Consente di creare la frase chiave aggiuntiva e gli indici di somiglianza dei documenti che fanno parte dell'indicizzazione semantica statistica. Per altre informazioni, vedere [Ricerca semantica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 [ **,** _...n_]  
 Indica che è possibile specificare più colonne per la clausola ADD, ALTER o DROP. Se si specificano più colonne, utilizzare la virgola come separatore.  
  
 WITH NO POPULATION  
 Specifica che l'indice full-text non verrà popolato dopo un'operazione ADD o DROP per le colonne o un'operazione SET STOPLIST. L'indice verrà popolato solo se si esegue un comando START...POPULATION.  
  
 Se si specifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non popola un indice. L'indice viene popolato solo quando si esegue un comando ALTER FULLTEXT INDEX...START POPULATION. Se non si specifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] popola l'indice.  
  
 Se l'opzione CHANGE_TRACKING è abilitata e si specifica WITH NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un errore. Se l'opzione CHANGE_TRACKING è abilitata e non si specifica WITH NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue un popolamento completo dell'indice.  
  
> [!NOTE]  
>  Per altre informazioni, vedere [Interazioni del rilevamento delle modifiche con NO POPULATION](#change-tracking-no-population).
  
 {ADD | DROP } STATISTICAL_SEMANTICS  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Consente di abilitare o disabilitare l'indicizzazione semantica statistica per le colonne specificate. Per altre informazioni, vedere [Ricerca semantica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 START {FULL|INCREMENTAL|UPDATE} POPULATION  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di avviare il popolamento dell'indice full-text di *table_name*. Se il popolamento dell'indice full-text è già in corso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un avviso e non avvia un nuovo popolamento.  
  
 FULL  
 Specifica che devono essere recuperate tutte le righe della tabella per l'indicizzazione full-text, anche se le righe sono già state indicizzate.  
  
 INCREMENTAL  
 Specifica che per l'indicizzazione full-text devono essere recuperate solo le righe modificate dopo l'ultimo popolamento. INCREMENTAL può essere applicato solo se la tabella ha una colonna di tipo **timestamp**. Se una tabella nel catalogo full-text non include una colonna di tipo **timestamp**, la tabella viene sottoposta a un popolamento di tipo FULL.  
  
 UPDATE  
 Specifica che devono essere elaborate tutte le operazioni di inserimento, aggiornamento o eliminazione eseguite dopo l'ultimo aggiornamento dell'indice con rilevamento delle modifiche. È necessario che il popolamento con rilevamento delle modifiche sia abilitato per una tabella, ma l'indice ad aggiornamento in background o il rilevamento automatico delle modifiche non deve essere attivato.  
  
 {STOP | PAUSE | RESUME } POPULATION  
 Arresta o sospende qualsiasi popolamento in corso oppure arresta o riprende qualsiasi popolamento sospeso.  
  
 STOP POPULATION non arresta il rilevamento automatico delle modifiche o l'indice ad aggiornamento in background. Per arrestare il rilevamento delle modifiche, utilizzare SET CHANGE_TRACKING OFF.  
  
 PAUSE POPULATION e RESUME POPULATION possono essere utilizzati soltanto per popolamenti completi. Non sono rilevanti per altri tipi di popolamento poiché le ricerche per indicizzazione vengono riprese negli altri popolamenti dal punto in cui sono state arrestate.  
  
 SET STOPLIST { OFF| SYSTEM | *stoplist_name* }  
 Cambia l'elenco di parole non significative full-text associato all'indice, se presente.  
  
 OFF  
 Specifica che all'indice full-text non deve essere associato alcun elenco di parole non significative.  
  
 SYSTEM  
 Specifica che per l'indice full-text deve essere utilizzato l'elemento STOPLIST full-text di sistema predefinito.  
  
 *stoplist_name*  
 Specifica il nome dell'elenco di parole non significative da associare all'indice full-text.  
  
 Per altre informazioni, vedere [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
 SET SEARCH PROPERTY LIST { OFF | *property_list_name* } [ WITH NO POPULATION ]  
 **Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Modifica l'elenco delle proprietà di ricerca associate all'indice, se presente.  
  
 OFF  
 Specifica che all'indice full-text non deve essere associato alcun elenco di proprietà. Quando si disattiva l'elenco delle proprietà di ricerca di un indice full-text (ALTER FULLTEXT INDEX ... SET SEARCH PROPERTY LIST OFF), non è più possibile eseguire la ricerca delle proprietà nella tabella di base.  
  
 Per impostazione predefinita, quando si disattiva un elenco delle proprietà di ricerca esistente, l'indice full-text viene ripopolato automaticamente. Se si specifica WITH NO POPULATION quando si disattiva l'elenco delle proprietà di ricerca, il ripopolamento automatico non ha luogo. È tuttavia consigliabile eseguire un popolamento completo su questo indice full-text. Il ripopolamento dell'indice full-text comporta la rimozione dei metadati specifici di ogni proprietà di ricerca eliminata, rendendo più piccolo e più efficiente l'indice full-text.  
  
 *property_list_name*  
 Specifica il nome dell'elenco delle proprietà di ricerca da associare all'indice full-text.  
  
 L'aggiunta di un elenco delle proprietà di ricerca a un indice full-text richiede il ripopolamento dell'indice per indicizzare le proprietà di ricerca registrate per l'elenco delle proprietà di ricerca associato. Se si specifica WITH NO POPULATION quando si aggiunge l'elenco delle proprietà di ricerca, è necessario eseguire un popolamento sull'indice, in un momento appropriato.  
  
> [!IMPORTANT]  
>  Se l'indice full-text è stato in precedenza associato a una ricerca diversa, è necessario ricostruire l'elenco delle proprietà per rendere coerente lo stato dell'indice. L'indice viene troncato immediatamente e rimane vuoto fino al termine del popolamento completo. Per altre informazioni, vedere [La modifica dell'elenco delle proprietà di ricerca richiede la ricompilazione dell'indice](#change-search-property-rebuild-index). 
  
> [!NOTE]  
>  È possibile associare un elenco delle proprietà di ricerca specificato a più indici full-text nello stesso database.  
  
 **Per trovare gli elenchi di proprietà di ricerca nel database corrente**  
  
-   [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 Per altre informazioni sugli elenchi delle proprietà di ricerca, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a><a name="change-tracking-no-population"></a> Interazioni del rilevamento delle modifiche con NO POPULATION  
 Il popolamento dell'indice full-text dipende dal fatto che il rilevamento delle modifiche sia o meno abilitato e che si specifichi o meno WITH NO POPULATION nell'istruzione ALTER FULLTEXT INDEX. Nella tabella seguente è riepilogato il risultato di tale interazione.  
  
|Rilevamento modifiche|WITH NO POPULATION|Risultato|  
|---------------------|------------------------|------------|  
|Non abilitato|Non specificato|Viene eseguito un popolamento completo dell'indice.|  
|Non abilitato|Specificata|L'indice non viene popolato fino a quando non viene eseguita un'istruzione ALTER FULLTEXT INDEX...START POPULATION.|  
|Attivato|Specificata|Viene generato un errore e l'indice non viene modificato.|  
|Attivato|Non specificato|Viene eseguito un popolamento completo dell'indice.|  
  
 Per altre informazioni sul popolamento degli indici full-text, vedere [Popolare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="changing-the-search-property-list-causes-rebuilding-the-index"></a><a name="change-search-property-rebuild-index"></a> La modifica dell'elenco delle proprietà di ricerca richiede la ricompilazione dell'indice  
 La prima volta in cui un indice full-text viene associato a un elenco delle proprietà di ricerca, è necessario ripopolare l'indice per indicizzare i termini di ricerca specifici delle proprietà. I dati dell'indice esistenti non vengono troncati.  
  
 Se tuttavia si associa l'indice full-text a un elenco delle proprietà diverso, l'indice viene ricompilato. La ricompilazione immediata comporta il troncamento dell'indice full-text, ovvero la rimozione di tutti i dati esistenti, e l'indice deve essere ripopolato. Nel corso del popolamento, le query full-text sulla tabella di base eseguono ricerche solo nelle righe della tabella che sono già state indicizzate dal popolamento. I dati dell'indice ripopolato includeranno metadati dalle proprietà registrate dell'elenco delle proprietà di ricerca appena aggiunto.  
  
 Scenari che comportano la ricompilazione includono:  
  
-   Passaggio diretto a un elenco delle proprietà di ricerca diverso (vedere "Scenario A", più avanti in questa sezione).  
  
-   Disattivazione dell'elenco delle proprietà di ricerca e successiva associazione dell'indice a qualsiasi elenco delle proprietà di ricerca (vedere "Scenario B" più avanti in questa sezione)  
  
> [!NOTE]  
>  Per altre informazioni sul funzionamento della ricerca full-text con gli elenchi delle proprietà di ricerca, vedere [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md). Per informazioni sui popolamenti completi, vedere [Popolare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).  
  
### <a name="scenario-a-switching-directly-to-a-different-search-property-list"></a>Scenario A: Passaggio diretto a un elenco delle proprietà di ricerca diverso  
  
1.  Un indice full-text viene creato in `table_1` con un elenco delle proprietà di ricerca `spl_1`:  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1,   
       CHANGE_TRACKING OFF, NO POPULATION;   
    ```  
  
2.  Viene eseguito un popolamento completo sull'indice full-text:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 START FULL POPULATION;  
    ```  
  
3.  L'indice full-text viene associato in seguito a un elenco delle proprietà di ricerca diverso, `spl_2`, utilizzando l'istruzione seguente:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_2;  
    ```  
  
     Questa istruzione comporta un popolamento completo, ovvero il comportamento predefinito.  Tuttavia, prima di avviare questo popolamento, il motore di ricerca full-text tronca automaticamente l'indice.  
  
### <a name="scenario-b-turning-off-the-search-property-list-and-later-associating-the-index-with-any-search-property-list"></a>Scenario B: Disattivazione dell'elenco delle proprietà di ricerca e successiva associazione dell'indice a qualsiasi elenco delle proprietà di ricerca  
  
1.  Un indice full-text viene creato in `table_1` con un elenco delle proprietà di ricerca `spl_1`, seguito da un popolamento completo automatico (comportamento predefinito):  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1;   
    ```  
  
2.  L'elenco delle proprietà di ricerca viene disattivato, come illustrato di seguito:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1   
       SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
    ```  
  
3.  L'indice full-text è ancora una volta associato allo stesso elenco delle proprietà di ricerca o a uno diverso.  
  
     Ad esempio, tramite l'istruzione seguente l'indice full-text viene nuovamente associato all'elenco delle proprietà di ricerca originale, ovvero `spl_1`:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_1;  
    ```  
  
     Questa istruzione avvia un popolamento completo, ovvero il comportamento predefinito.  
  
    > [!NOTE]  
    >  Potrebbe essere necessario eseguire la ricompilazione per un elenco delle proprietà di ricerca diverso, ad esempio `spl_2`.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve avere l'autorizzazione ALTER per la tabella o la vista indicizzata oppure deve essere un membro del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_ddladmin** o **db_owner**.  
  
 Se si specifica SET STOPLIST, l'utente deve disporre dell'autorizzazione REFERENCES per l'elenco di parole non significative. Se si specifica SET SEARCH PROPERTY LIST, l'utente deve disporre dell'autorizzazione REFERENCES per l'elenco delle proprietà di ricerca. Il proprietario dell'elenco di parole non significative o dell'elenco delle proprietà di ricerca specificato può concedere l'autorizzazione REFERENCES, se il proprietario dispone delle autorizzazioni ALTER FULLTEXT CATALOG.  
  
> [!NOTE]  
>  Agli utenti viene concessa l'autorizzazione REFERENCES per l'elenco di parole non significative predefinito fornito con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-setting-manual-change-tracking"></a>R. Impostazione del rilevamento manuale delle modifiche  
 Nell'esempio seguente viene impostato il rilevamento manuale delle modifiche per l'indice full-text nella tabella `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate  
   SET CHANGE_TRACKING MANUAL;  
GO  
```  
  
### <a name="b-associating-a-property-list-with-a-full-text-index"></a>B. Associazione di un elenco delle proprietà a un indice full-text  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Nell'esempio seguente l'elenco delle proprietà `DocumentPropertyList` viene associato all'indice full-text della tabella `Production.Document`. Questa istruzione ALTER FULLTEXT INDEX avvia un popolamento completo, che corrisponde al comportamento predefinito della clausola SET SEARCH PROPERTY LIST.  
  
> [!NOTE]  
>  Per un esempio in cui viene creato l'elenco di proprietà `DocumentPropertyList`, vedere [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList;   
GO  
```  
  
### <a name="c-removing-a-search-property-list"></a>C. Rimozione di un elenco delle proprietà di ricerca  
  
**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.  
  
 Nell'esempio seguente viene rimosso l'elenco delle proprietà `DocumentPropertyList` dall'indice full-text della tabella `Production.Document`. In questo esempio non è necessario affrettarsi a rimuovere le proprietà dall'indice, pertanto viene specificata l'opzione WITH NO POPULATION. Tuttavia, la ricerca a livello di proprietà non è più consentita in questo indice full-text.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
GO  
```  
  
### <a name="d-starting-a-full-population"></a>D. Avvio di un popolamento completo  
 Nell'esempio seguente viene avviato un popolamento completo nell'indice full-text nella tabella `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   START FULL POPULATION;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [Popolamento degli indici full-text](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
