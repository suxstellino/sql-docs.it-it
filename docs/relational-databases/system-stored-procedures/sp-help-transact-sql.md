---
description: sp_help (Transact-SQL)
title: sp_help (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help
- sp_help_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help
ms.assetid: 913cd5d4-39a3-4a4b-a926-75ed32878884
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e97e2118c90521881dbf55d686910cff01ef79e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176573"
---
# <a name="sp_help-transact-sql"></a>sp_help (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni su un oggetto di database (qualsiasi oggetto elencato nella visualizzazione di compatibilità **degli oggettisys.sys** ), un tipo di dati definito dall'utente o un tipo di dati.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help [ [ @objname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'name'` Nome di qualsiasi oggetto, in **sysobjects** o qualsiasi tipo di dati definito dall'utente nella tabella **systypes** . *Name* è di **tipo nvarchar (** 776 **)** e il valore predefinito è null. I nomi di database non sono validi.  I nomi in due o tre parti devono essere delimitati, ad esempio 'Person.AddressType' o [Person.AddressType].   
   
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 I set di risultati restituiti variano a seconda che il *nome* venga specificato, quando viene specificato e quale oggetto di database è.  
  
1.  Se **sp_help** viene eseguito senza argomenti, vengono restituite informazioni di riepilogo sugli oggetti di tutti i tipi presenti nel database corrente.  
  
    |Nome colonna|Tipo di dati|Descrizione|  
    |-----------------|---------------|-----------------|  
    |**Nome**|**nvarchar (** 128 **)**|Nome dell'oggetto|  
    |**Proprietario**|**nvarchar (** 128 **)**|Proprietario dell'oggetto. Si tratta dell'entità di database proprietaria dell'oggetto. Corrispondente per impostazione predefinita al proprietario dello schema contenente l'oggetto.|  
    |**Object_type**|**nvarchar (** 31 **)**|Tipo di oggetto|  
  
2.  Se *Name* è un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo di dati o un tipo di dati definito dall'utente, **sp_help** restituisce il set di risultati.  
  
    |Nome colonna|Tipo di dati|Descrizione|  
    |-----------------|---------------|-----------------|  
    |**Type_name**|**nvarchar (** 128 **)**|Nome del tipo di dati.|  
    |**Storage_type**|**nvarchar (** 128 **)**|Nome del tipo di archiviazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
    |**Length**|**smallint**|Lunghezza fisica del tipo di dati in byte.|  
    |**Prec**|**int**|Precisione, ovvero il numero totale di cifre.|  
    |**Ridimensiona**|**int**|Numero di cifre a destra del separatore decimale.|  
    |**Ammette i valori Null**|**varchar (** 35 **)**|Indica se i valori NULL sono supportati. I possibili valori sono Yes o No.|  
    |**Default_name**|**nvarchar (** 128 **)**|Nome del valore predefinito associato al tipo di dati specificato.<br /><br /> NULL = Non è associata alcuna regola predefinita.|  
    |**Rule_name**|**nvarchar (** 128 **)**|Nome di una regola associata al tipo di dati specificato.<br /><br /> NULL = Non è associata alcuna regola predefinita.|  
    |**Regole di confronto**|**sysname**|Regole di confronto per il tipo di dati. Per i tipi di dati non carattere, è NULL.|  
  
3.  Se *Name* è un oggetto di database diverso da un tipo di dati, **sp_help** restituisce il set di risultati e anche altri set di risultati in base al tipo di oggetto specificato.  

    |Nome colonna|Tipo di dati|Descrizione|  
    |-----------------|---------------|-----------------|  
    |**Nome**|**nvarchar (** 128 **)**|Nome tabella|  
    |**Proprietario**|**nvarchar (** 128 **)**|Proprietario della tabella.|  
    |**Tipo**|**nvarchar (** 31 **)**|Tipo di tabella.|  
    |**Created_datetime**|**datetime**|Data di creazione della tabella.|  
  
     A seconda dell'oggetto di database specificato, **sp_help** restituisce set di risultati aggiuntivi.  
  
     Se *Name* è una tabella di sistema, una tabella utente o una vista, **sp_help** restituisce i set di risultati seguenti. Per le viste non viene tuttavia restituito il set di risultati relativo alla posizione del file di dati in un filegroup.  
  
    -   Set di risultati aggiuntivo restituito per gli oggetti colonna  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**Column_name**|**nvarchar (** 128 **)**|Nome colonna.|  
        |**Tipo**|**nvarchar (** 128 **)**|Tipo di dati della colonna.|  
        |**Calcolata**|**varchar (** 35 **)**|Indica se i valori della colonna sono calcolati (Yes o No).|  
        |**Length**|**int**|Lunghezza della colonna in byte.<br /><br /> Nota: se il tipo di dati della colonna è un tipo di valore di grandi dimensioni (**varchar (max)**, **nvarchar (max)**, **varbinary (max)** o **XML**), il valore verrà visualizzato come-1.|  
        |**Prec**|**char (** 5 **)**|Precisione della colonna.|  
        |**Ridimensiona**|**char (** 5 **)**|Scala della colonna.|  
        |**Ammette i valori Null**|**varchar (** 35 **)**|Indica se nella colonna sono consentiti i valori Null. I possibili valori sono Yes o No.|  
        |**TrimTrailingBlanks**|**varchar (** 35 **)**|Specifica se gli spazi vuoti finali devono essere eliminati o meno. Restituisce Yes o No.|  
        |**FixedLenNullInSource**|**varchar (** 35 **)**|Disponibile solo per compatibilità con le versioni precedenti.|  
        |**Regole di confronto**|**sysname**|Regole di confronto della colonna. NULL per i tipi di dati non carattere.|  
  
    -   Set di risultati aggiuntivo restituito per le colonne Identity  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**Identità**|**nvarchar (** 128 **)**|Nome della colonna il cui tipo di dati viene dichiarato come Identity.|  
        |**Seed**|**numeric**|Valore iniziale per la colonna Identity.|  
        |**Incremento valore Identity**|**numeric**|Incremento da utilizzare per i valori della colonna.|  
        |**Non applicare in processi di replica**|**int**|La proprietà IDENTITY non viene applicata quando un account di accesso per la replica, ad esempio **sqlrepl**, inserisce i dati nella tabella:<br /><br /> 1 = True<br /><br /> 0 = False|  
  
    -   Set di risultati aggiuntivo restituito per le colonne  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**RowGuidCol**|**sysname**|Nome della colonna che include il valore GUID.|  
  
    -   Set di risultati aggiuntivo restituito per i filegroup  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**Data_located_on_filegroup**|**nvarchar (** 128 **)**|Filegroup in cui si trovano i dati (primario, secondario o log delle transazioni).|  
  
    -   Set di risultati aggiuntivo restituito per gli indici:  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**index_name**|**sysname**|Nome dell'indice.|  
        |**Index_description**|**varchar (** 210 **)**|Descrizione dell'indice.|  
        |**index_keys**|**nvarchar (** 2078 **)**|Nomi delle colonne in cui viene compilato l'indice. Restituisce NULL per gli indici columnstore con ottimizzazione per la memoria xVelocity.|  
  
    -   Set di risultati aggiuntivo restituito per i vincoli:  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**constraint_type**|**nvarchar (** 146 **)**|Tipo di vincolo.|  
        |**constraint_name**|**nvarchar (** 128 **)**|Nome del vincolo.|  
        |**delete_action**|**nvarchar (** 9 **)**|Indica se l'azione DELETE è NO_ACTION, CASCADE, SET_NULL, SET_DEFAULT o N/A.<br /><br /> Valido solo per i vincoli FOREIGN KEY.|  
        |**update_action**|**nvarchar (** 9 **)**|Indica se l'azione UPDATE è NO_ACTION, CASCADE, SET_NULL, SET_DEFAULT o N/A.<br /><br /> Valido solo per i vincoli FOREIGN KEY.|  
        |**status_enabled**|**varchar (** 8 **)**|Indica se il vincolo è abilitato. I possibili valori sono Enabled, Disabled o N/D.<br /><br /> Valido solo per i vincoli CHECK e FOREIGN KEY.|  
        |**status_for_replication**|**varchar (** 19 **)**|Indica se il vincolo è relativo alla replica.<br /><br /> Valido solo per i vincoli CHECK e FOREIGN KEY.|  
        |**constraint_keys**|**nvarchar (** 2078 **)**|Nomi delle colonne che formano il vincolo o, nel caso di valori predefiniti e regole, il testo che definisce il valore predefinito o la regola.|  
  
    -   Set di risultati aggiuntivo restituito per gli oggetti di riferimento  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**Table is referenced by**|**nvarchar (** 516 **)**|Identifica gli oggetti di database che fanno riferimento alla tabella.|  
  
    -   Set di risultati aggiuntivo restituito per stored procedure, funzioni o stored procedure estese  
  
        |Nome colonna|Tipo di dati|Descrizione|  
        |-----------------|---------------|-----------------|  
        |**Parameter_name**|**nvarchar (** 128 **)**|Nome del parametro della stored procedure.|  
        |**Tipo**|**nvarchar (** 128 **)**|Tipo di dati del parametro della stored procedure.|  
        |**Length**|**smallint**|Capacità massima di archiviazione fisica in byte.|  
        |**Prec**|**int**|Precisione, ovvero il numero totale di cifre.|  
        |**Ridimensiona**|**int**|Numero di cifre a destra del separatore decimale.|  
        |**Param_order**|**smallint**|Ordine del parametro.|  
  
## <a name="remarks"></a>Commenti  
 La routine **sp_help** Cerca un oggetto solo nel database corrente.  
  
 Quando il *nome* non è specificato, **sp_help** elenca i nomi di oggetto, i proprietari e i tipi di oggetto per tutti gli oggetti nel database corrente. **sp_helptrigger** fornisce informazioni sui trigger.  
  
 **sp_help** espone solo le colonne di indice ordinabili; Pertanto, non espone informazioni sugli indici XML o spaziali.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** . L'utente deve avere almeno un'autorizzazione per *ObjName*. Per visualizzare chiavi del vincolo di colonna, impostazioni predefinite o regole, è necessario disporre dell'autorizzazione VIEW DEFINITION per la tabella.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-information-about-all-objects"></a>R. Restituzione di informazioni su tutti gli oggetti  
 Nell'esempio seguente vengono elencate le informazioni su ogni oggetto incluso nel database `master`.  
  
```  
USE master;  
GO  
EXEC sp_help;  
GO  
```  
  
### <a name="b-returning-information-about-a-single-object"></a>B. Restituzione di informazioni su un solo oggetto  
 Nell'esempio seguente vengono visualizzate informazioni sulla tabella `Person`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help 'Person.Person';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_helpindex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sp_helpuser &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [ Oggettisys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md)  
  
  
