---
description: OBJECTPROPERTYEX (Transact-SQL)
title: OBJECTPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECTPROPERTYEX
- OBJECTPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTYEX function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: be36b3e3-3309-4332-bfb5-c7e9cf8dc8bd
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 611d0f17ae0ed8cd0e7679d149b186563f802ba4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237476"
---
# <a name="objectpropertyex-transact-sql"></a>OBJECTPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni sugli oggetti con ambito schema nel database corrente. Per un elenco di questi oggetti, vedere [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). La funzione OBJECTPROPERTYEX non può essere utilizzata per oggetti non definiti a livello di ambito dello schema, ad esempio notifiche degli eventi e trigger DDL (Data Definition Language).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
OBJECTPROPERTYEX ( id , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *id*  
 Espressione che rappresenta l'ID dell'oggetto nel database corrente. *id* è di tipo **int** e si presuppone che sia un oggetto con ambito schema nel contesto di database corrente.  
  
 *property*  
 Espressione che contiene le informazioni da restituire per l'oggetto specificato dall'ID. Il tipo restituito è **sql_variant**. Nella tabella seguente viene indicato il tipo di dati di base per ogni valore di questo argomento.  
  
> [!NOTE]  
>  Se non specificato diversamente, viene restituito NULL quando *property* non è un nome di proprietà valido, *id* non è un ID di oggetto valido, *id* è un tipo di oggetto non supportato per la *property* specificata oppure il chiamante non dispone dell'autorizzazione necessaria per visualizzare i metadati dell'oggetto.  
  
|Nome proprietà|Tipo oggetto|Descrizione e valori restituiti|  
|-------------------|-----------------|-------------------------------------|  
|BaseType|Qualsiasi oggetto con ambito schema|Identifica il tipo di base dell'oggetto. Quando l'oggetto specificato è di tipo SYNONYM, viene restituito il tipo di base dell'oggetto sottostante.<br /><br /> Valore diverso da Null = Tipo di oggetto<br /><br /> Tipo di dati di base: **char(2)**|  
|CnstIsClustKey|Vincolo|Vincolo PRIMARY KEY con un indice cluster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsColumn|Vincolo|Vincolo CHECK, DEFAULT o FOREIGN KEY per una singola colonna.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsDeleteCascade|Vincolo|Vincolo FOREIGN KEY con l'opzione ON DELETE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsDisabled|Vincolo|Vincolo disabilitato.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsNonclustKey|Vincolo|Vincolo PRIMARY KEY con un indice non cluster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsNotRepl|Vincolo|Vincolo definito con le parole chiave NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsNotTrusted|Vincolo|Vincolo abilitato senza eseguire una verifica delle righe esistenti. Il vincolo pertanto potrebbe non essere valido per tutte le righe.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|CnstIsUpdateCascade|Vincolo|Vincolo FOREIGN KEY con l'opzione ON UPDATE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsAfterTrigger|Trigger|Trigger AFTER.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsAnsiNullsOn|Funzione [!INCLUDE[tsql](../../includes/tsql-md.md)], procedura [!INCLUDE[tsql](../../includes/tsql-md.md)], trigger [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Impostazione di ANSI_NULLS in fase di creazione.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsDeleteTrigger|Trigger|Trigger DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsFirstDeleteTrigger|Trigger|Primo trigger attivato quando viene eseguita un'istruzione DELETE sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsFirstInsertTrigger|Trigger|Primo trigger attivato quando viene eseguita un'istruzione INSERT sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsFirstUpdateTrigger|Trigger|Primo trigger attivato quando viene eseguita un'istruzione UPDATE sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsInsertTrigger|Trigger|Trigger INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsInsteadOfTrigger|Trigger|Trigger INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsLastDeleteTrigger|Trigger|Ultimo trigger attivato quando viene eseguita un'istruzione DELETE sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsLastInsertTrigger|Trigger|Ultimo trigger attivato quando viene eseguita un'istruzione INSERT sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsLastUpdateTrigger|Trigger|Ultimo trigger attivato quando viene eseguita un'istruzione UPDATE sulla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsQuotedIdentOn|Funzione [!INCLUDE[tsql](../../includes/tsql-md.md)], procedura [!INCLUDE[tsql](../../includes/tsql-md.md)], trigger [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Impostazione di QUOTED_IDENTIFIER in fase di creazione.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsStartup|Procedura|Procedura di avvio.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsTriggerDisabled|Trigger|Trigger disabilitato.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsTriggerNotForRepl|Trigger|Trigger definito come NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsUpdateTrigger|Trigger|Trigger UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|ExecIsWithNativeCompilation|Stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)]|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> La stored procedure è compilata in modo nativo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|HasAfterTrigger|Tabella, vista|Tabella o vista che include un trigger AFTER.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|HasDeleteTrigger|Tabella, vista|Tabella o vista che include un trigger DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|HasInsertTrigger|Tabella, vista|Tabella o vista che include un trigger INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|HasInsteadOfTrigger|Tabella, vista|Tabella o vista che include un trigger INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|HasUpdateTrigger|Tabella, vista|Tabella o vista che include un trigger UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsAnsiNullsOn|Funzione [!INCLUDE[tsql](../../includes/tsql-md.md)], procedura [!INCLUDE[tsql](../../includes/tsql-md.md)], tabella, trigger [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|L'opzione ANSI NULLS per la tabella è impostata su ON, il che significa che tutti i confronti di un valore Null restituiscono UNKNOWN. Questa impostazione viene applicata a tutte le espressioni nella definizione di tabella, inclusi i vincoli e le colonne calcolate, per tutto il tempo in cui la tabella è disponibile.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsCheckCnst|Qualsiasi oggetto con ambito schema|Vincolo CHECK.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsConstraint|Qualsiasi oggetto con ambito schema|Vincolo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsDefault|Qualsiasi oggetto con ambito schema|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Valore predefinito associato.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsDefaultCnst|Qualsiasi oggetto con ambito schema|Vincolo DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsDeterministic|Funzioni con valori scalari e di tabella, vista|Proprietà deterministica della funzione o della vista.<br /><br /> 1 = Deterministica<br /><br /> 0 = Non deterministica<br /><br /> Tipo di dati di base: **int**|  
|IsEncrypted|Funzione [!INCLUDE[tsql](../../includes/tsql-md.md)], procedura [!INCLUDE[tsql](../../includes/tsql-md.md)], tabella, trigger [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Indica che il testo originale dell'istruzione del modulo è stato convertito in un formato offuscato. L'output dell'offuscamento non è visibile direttamente nelle viste del catalogo in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Gli utenti che non hanno accesso a tabelle di sistema o file del database non possono recuperare il testo offuscato. Il testo è tuttavia disponibile per gli utenti con privilegi di accesso a tabelle di sistema attraverso la [porta DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) oppure con privilegi di accesso diretto a file del database. Gli utenti in grado di collegare un debugger al processo del server possono inoltre recuperare la procedura originale dalla memoria in fase di esecuzione.<br /><br /> 1 = Crittografato<br /><br /> 0 = Non crittografato<br /><br /> Tipo di dati di base: **int**|  
|IsExecuted|Qualsiasi oggetto con ambito schema|L'oggetto può essere eseguito (vista, procedura, funzione o trigger).<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsExtendedProc|Qualsiasi oggetto con ambito schema|Procedura estesa.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsForeignKey|Qualsiasi oggetto con ambito schema|Vincolo FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsIndexed|Tabella, vista|Tabella o vista che include un indice.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsIndexable|Tabella, vista|Tabella o vista per cui è possibile creare un indice.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsInlineFunction|Funzione|Funzione inline.<br /><br /> 1 = Funzione inline<br /><br /> 0 = Funzione non inline<br /><br /> Tipo di dati di base: **int**|  
|IsMSShipped|Qualsiasi oggetto con ambito schema|Oggetto creato durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsPrecise|Colonna calcolata, funzione, tipo definito dall'utente, vista|L'oggetto contiene un calcolo impreciso, ad esempio operazioni a virgola mobile.<br /><br /> 1 = Preciso<br /><br /> 0 = Impreciso<br /><br /> Tipo di dati di base: **int**|  
|IsPrimaryKey|Qualsiasi oggetto con ambito schema|Vincolo PRIMARY KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsProcedure|Qualsiasi oggetto con ambito schema|Procedura.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsQuotedIdentOn|Vincolo CHECK, definizione DEFAULT, funzione [!INCLUDE[tsql](../../includes/tsql-md.md)], procedura [!INCLUDE[tsql](../../includes/tsql-md.md)], tabella, trigger [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|L'opzione relativa all'utilizzo dell'identificatore tra virgolette per l'oggetto è impostata su ON, il che significa che gli identificatori sono racchiusi tra virgolette doppie in tutte le espressioni a cui viene fatto riferimento nella definizione dell'oggetto.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsQueue|Qualsiasi oggetto con ambito schema|Coda di Service Broker.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsReplProc|Qualsiasi oggetto con ambito schema|Procedura di replica.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsRule|Qualsiasi oggetto con ambito schema|Regola associata.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsScalarFunction|Funzione|Funzione con valori scalari.<br /><br /> 1 = Funzione con valori scalari<br /><br /> 0 = Funzione senza valori scalari<br /><br /> Tipo di dati di base: **int**|  
|IsSchemaBound|Funzione, procedura, vista|Funzione o vista associata a schema creata con la clausola SCHEMABINDING.<br /><br /> 1 = Associata a uno schema<br /><br /> 0 = Non associata a schema<br /><br /> Tipo di dati di base: **int**|  
|IsSystemTable|Tabella|Tabella di sistema.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsSystemVerified|Colonna calcolata, funzione, tipo definito dall'utente, vista|Le proprietà deterministiche e di precisione dell'oggetto possono essere verificate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsTable|Tabella|Tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsTableFunction|Funzione|Funzione con valori di tabella.<br /><br /> 1 = Funzione con valori di tabella<br /><br /> 0 = Funzione senza valori di tabella<br /><br /> Tipo di dati di base: **int**|  
|IsTrigger|Qualsiasi oggetto con ambito schema|Trigger.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsUniqueCnst|Qualsiasi oggetto con ambito schema|Vincolo UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsUserTable|Tabella|Tabella definita dall'utente.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|IsView|Visualizzazione|Vista.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|OwnerId|Qualsiasi oggetto con ambito schema|Proprietario dell'oggetto.<br /><br /> **Nota:** Il proprietario dello schema non corrisponde necessariamente al proprietario dell'oggetto. Ad esempio, gli oggetti figlio (per i quali il parametro *parent_object_id* è impostato su un valore diverso da Null) restituiscono sempre lo stesso ID di proprietario del padre.<br /><br /> Valore diverso da Null = ID utente del database del proprietario dell'oggetto.<br /><br /> NULL = Tipo di oggetto non supportato oppure ID di oggetto non valido.<br /><br /> Tipo di dati di base: **int**|  
|SchemaId|Qualsiasi oggetto con ambito schema|ID dello schema associato all'oggetto.<br /><br /> Valore diverso da Null = ID dello schema dell'oggetto.<br /><br /> Tipo di dati di base: **int**|  
|SystemDataAccess|Funzione, vista|L'oggetto accede a dati di sistema, cataloghi di sistema o tabelle di sistema virtuali nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = Nessuna<br /><br /> 1 = Lettura<br /><br /> Tipo di dati di base: **int**|  
|TableDeleteTrigger|Tabella|La tabella include un trigger DELETE.<br /><br /> >1 = ID del primo trigger del tipo specificato.<br /><br /> Tipo di dati di base: **int**|  
|TableDeleteTriggerCount|Tabella|La tabella include il numero specificato di trigger DELETE.<br /><br /> Valore diverso da Null = Numero di trigger DELETE.<br /><br /> Tipo di dati di base: **int**|  
|TableFullTextMergeStatus|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Indica se una tabella dispone di un indice full-text su cui è attualmente in corso un'operazione di unione.<br /><br /> 0 = La tabella non dispone di un indice full-text o sull'indice full-text non è in corso un'operazione di unione.<br /><br /> 1 = Sull'indice full-text è in corso un'operazione di unione.|  
|TableFullTextBackgroundUpdateIndexOn|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Nella tabella è abilitato l'indice full-text ad aggiornamento in background (rilevamento automatico delle modifiche).<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextCatalogId|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> ID del catalogo full-text contenente i dati dell'indice full-text per la tabella.<br /><br /> Valore diverso da zero = ID del catalogo full-text associato all'indice univoco che identifica le righe di una tabella con indicizzazione full-text.<br /><br /> 0 = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**|  
|TableFullTextChangeTrackingOn|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Nella tabella è abilitato il rilevamento delle modifiche full-text.<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextDocsProcessed|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di righe elaborate dopo l'avvio dell'indicizzazione full-text. In una tabella sottoposta a indicizzazione ai fini della ricerca full-text tutte le colonne di una riga vengono considerate come appartenenti a un unico documento da indicizzare.<br /><br /> 0 = Nessuna ricerca per indicizzazione attiva oppure l'indicizzazione full-text è stata completata.<br /><br /> > 0 = uno dei seguenti (A o B): A) Numero di documenti elaborati dalle operazioni di inserimento o aggiornamento dopo l'avvio del popolamento con rilevamento delle modifiche completo, incrementale o manuale ; B) Numero di righe elaborate da operazioni di inserimento o aggiornamento dopo l'attivazione del rilevamento delle modifiche con popolamento dell'indice di aggiornamento in background, la modifica dello schema dell'indice full-text, la ricompilazione del catalogo full-text o il riavvio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e così via.<br /><br /> NULL = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**<br /><br /> **Note** Questa proprietà non monitora né conta le righe eliminate.|  
|TableFulltextFailCount|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di righe non indicizzate dalla ricerca full-text.<br /><br /> 0 = Popolamento completato.<br /><br /> > 0 = una delle seguenti opzioni (A o B): A) Numero di documenti non indicizzati dopo l'avvio del popolamento di aggiornamento con rilevamento delle modifiche completo, incrementale e manuale; B) Per il rilevamento delle modifiche con aggiornamento indice in background, numero di righe non indicizzate dopo l'inizio del popolamento o il riavvio del popolamento. La mancata indicizzazione potrebbe essere dovuta a una modifica dello schema, alla ricompilazione del catalogo, al riavvio del server e così via.<br /><br /> NULL = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextItemCount|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Valore diverso da Null = Numero di righe per cui l'indicizzazione full-text ha avuto esito positivo.<br /><br /> NULL = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextKeyColumn|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> ID della colonna associata all'indice univoco costituito da una sola colonna che fa parte della definizione di un indice full-text e di un indice semantico.<br /><br /> 0 = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextPendingChanges|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Numero di voci in sospeso del rilevamento delle modifiche da elaborare.<br /><br /> 0 = Il rilevamento delle modifiche non è abilitato.<br /><br /> NULL = La tabella non include un indice full-text.<br /><br /> Tipo di dati di base: **int**|  
|TableFulltextPopulateStatus|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> 0 = Inattivo<br /><br /> 1 = Popolamento completo in corso.<br /><br /> 2 = Popolamento incrementale in corso.<br /><br /> 3 = Propagazione delle modifiche rilevate in corso.<br /><br /> 4 = Operazione in corso per l'indice ad aggiornamento in background, ad esempio il rilevamento automatico delle modifiche.<br /><br /> 5 = Indicizzazione full-text rallentata o sospesa.<br /><br /> 6 = Si è verificato un errore. Esaminare il registro di popolamento per i dettagli. Per altre informazioni, vedere la sezione **Risoluzione di errori in un popolamento full-text (ricerca per indicizzazione)** di [Populare gli indici full-text](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Tipo di dati di base: **int**|  
|TableFullTextSemanticExtraction|Tabella|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> La tabella è abilitata per l'indicizzazione semantica.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasActiveFulltextIndex|Tabella|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> La tabella include un indice full-text attivo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasCheckCnst|Tabella|La tabella include un vincolo CHECK.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasClustIndex|Tabella|La tabella include un indice cluster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasDefaultCnst|Tabella|La tabella include un vincolo DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasDeleteTrigger|Tabella|La tabella include un trigger DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasForeignKey|Tabella|La tabella include un vincolo FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasForeignRef|Tabella|Un vincolo FOREIGN KEY fa riferimento alla tabella.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasIdentity|Tabella|La tabella include una colonna Identity.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasIndex|Tabella|La tabella include un indice di qualsiasi tipo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasInsertTrigger|Tabella|L'oggetto include un trigger INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasNonclustIndex|Tabella|La tabella include un indice non cluster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasPrimaryKey|Tabella|La tabella include una chiave primaria.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasRowGuidCol|Tabella|La tabella include un ROWGUIDCOL per una colonna **uniqueidentifier**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasTextImage|Tabella|La tabella include una colonna **text**, **ntext**, o **immagine**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasTimestamp|Tabella|La tabella include una colonna **timestamp**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasUniqueCnst|Tabella|La tabella include un vincolo UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasUpdateTrigger|Tabella|L'oggetto include un trigger UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableHasVarDecimalStorageFormat|Tabella|Tabella abilitata per il formato di archiviazione **vardecimal**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|Tabella|La tabella include un trigger INSERT.<br /><br /> >1 = ID del primo trigger del tipo specificato.<br /><br /> Tipo di dati di base: **int**|  
|TableInsertTriggerCount|Tabella|La tabella include il numero specificato di trigger INSERT.<br /><br /> >0 = Numero di trigger INSERT.<br /><br /> Tipo di dati di base: **int**|  
|TableIsFake|Tabella|La tabella non è reale, ma viene creata internamente su richiesta da [!INCLUDE[ssDE](../../includes/ssde-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableIsLockedOnBulkLoad|Tabella|La tabella è bloccata a causa di un processo **bcp** o BULK INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**|  
|TableIsMemoryOptimized|Tabella|**Si applica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e versioni successive.<br /><br /> La tabella prevede l'ottimizzazione per la memoria<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo di dati di base: **int**<br /><br /> Per altre informazioni, vedere [OLTP in memoria &#40;ottimizzazione in memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).|  
|TableIsPinned|Tabella|La tabella è bloccata per essere mantenuta nella cache dei dati.<br /><br /> 0 = False<br /><br /> Questa funzionalità non è supportata in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive.|  
|TableTextInRowLimit|Tabella|Per la tabella è stata impostata l'opzione text in row.<br /><br /> > 0 = Numero massimo di byte consentito per l'opzione text in row.<br /><br /> 0 = L'opzione Text in row non è impostata.<br /><br /> Tipo di dati di base: **int**|  
|TableUpdateTrigger|Tabella|La tabella include un trigger UPDATE.<br /><br /> > 1 = ID del primo trigger del tipo specificato.<br /><br /> Tipo di dati di base: **int**|  
|TableUpdateTriggerCount|Tabella|La tabella include il numero specificato di trigger UPDATE.<br /><br /> > 0 = Numero di trigger UPDATE.<br /><br /> Tipo di dati di base: **int**|  
|UserDataAccess|Funzione, vista|L'oggetto accede a dati utente (tabelle utente) nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Lettura<br /><br /> 0 = Nessuna<br /><br /> Tipo di dati di base: **int**|  
|TableHasColumnSet|Tabella|Indica se la tabella include un set di colonne.<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> Per altre informazioni, vedere [Usare set di colonne](../../relational-databases/tables/use-column-sets.md).|  
|Cardinalità|Tabella (definita dal sistema o dall'utente), vista o indice|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> Numero di righe nell'oggetto specificato.|  
|TableTemporalType|Tabella|**Si applica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> Specifica il tipo di tabella.<br /><br /> 0 = tabella non temporale<br /><br /> 1 = tabella di cronologia per tabella con controllo delle versioni di sistema<br /><br /> 2 = tabella temporale con controllo delle versioni di sistema|  
  
## <a name="return-types"></a>Tipi restituiti  
 **sql_variant**  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto.  
  
 Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come OBJECTPROPERTYEX possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Commenti  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] presuppone che *object_id* si trovi nel contesto di database corrente. Una query in cui viene fatto riferimento a un *object_id* in un altro database restituisce NULL oppure risultati non corretti. Nella query seguente, ad esempio, il contesto di database corrente è il database master. [!INCLUDE[ssDE](../../includes/ssde-md.md)] tenterà di restituire il valore della proprietà per l'*object_id* specificato in tale database anziché nel database specificato nella query. La query restituisce risultati non corretti perché la vista `vEmployee` non si trova nel database master.  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTYEX(*view_id*, 'IsIndexable') può richiedere una quantità elevata di memoria perché la valutazione della proprietà IsIndexable richiede l'analisi della definizione, normalizzazione e ottimizzazione parziale della vista. Sebbene la proprietà IsIndexable identifichi tabelle o viste che è possibile indicizzare, la creazione effettiva dell'indice può avere esito negativo se non vengono soddisfatti determinati requisiti riguardanti la chiave di indice. Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 OBJECTPROPERTYEX (*table_id*, 'TableHasActiveFulltextIndex') restituisce il valore 1 (true) quando almeno una colonna di una tabella viene aggiunta per l'indicizzazione. L'indicizzazione full-text risulta attiva per il popolamento non appena viene aggiunta la prima colonna per l'indicizzazione.  
  
 Il set di risultati è soggetto ad alcune restrizioni riguardanti la visibilità dei metadati. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-finding-the-base-type-of-an-object"></a>R. Ricerca del tipo di base di un oggetto  
 Nell'esempio seguente viene creato un oggetto SYNONYM `MyEmployeeTable` per la tabella `Employee` nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e quindi viene restituito il tipo di base di SYNONYM.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SYNONYM MyEmployeeTable FOR HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX ( object_id(N'MyEmployeeTable'), N'BaseType')AS [Base Type];  
GO  
```  
  
 Nel set di risultati viene indicato che il tipo di base dell'oggetto sottostante, ovvero la tabella `Employee`, è una tabella utente.  
  
 ```
Base Type 
--------  
U
```  
  
### <a name="b-returning-a-property-value"></a>B. Restituzione del valore di una proprietà  
 Nell'esempio seguente viene restituito il numero di trigger UPDATE per la tabella specificata.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'HumanResources.Employee'), N'TABLEUPDATETRIGGERCOUNT');  
GO  
```  
  
### <a name="c-finding-tables-that-have-a-foreign-key-constraint"></a>C. Ricerca delle tabelle con vincolo FOREIGN KEY  
 Nell'esempio seguente viene utilizzata la proprietà `TableHasForeignKey` per restituire tutte le tabelle con vincolo FOREIGN KEY.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name, object_id, schema_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTYEX(object_id, N'TableHasForeignKey') = 1  
ORDER BY name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-finding-the-base-type-of-an-object"></a>D. Ricerca del tipo di base di un oggetto  
 Nell'esempio seguente viene restituito il tipo di base dell'oggetto `dbo.DimReseller`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OBJECTPROPERTYEX ( object_id(N'dbo.DimReseller'), N'BaseType')AS BaseType;  
```  
  
 Nel set di risultati viene indicato che il tipo di base dell'oggetto sottostante, ovvero la tabella `dbo.DimReseller`, è una tabella utente.  
  
```  
BaseType   
--------   
U   
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)  
  
  

