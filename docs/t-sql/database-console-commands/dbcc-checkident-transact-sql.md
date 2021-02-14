---
description: DBCC CHECKIDENT (Transact-SQL)
title: DBCC CHECKIDENT (Transact-SQL)
ms.custom: ''
ms.date: 03/07/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKIDENT
- DBCC CHECKIDENT
- CHECKIDENT_TSQL
- DBCC_CHECKIDENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checking identity values
- reseeding identity values
- resetting identity values
- identity values [SQL Server]
- identity values [SQL Server], checking
- modifying identity values
- current identity values
- DBCC CHECKIDENT statement
- identity values [SQL Server], reseeding
- reporting current identity values
ms.assetid: 2c00ee51-2062-4e47-8b19-d90f524c6427
author: pmasl
ms.author: umajay
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017 || = azure-sqldw-latest
ms.openlocfilehash: bb5a426514c8ddd9607114f444119429deb58aeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346067"
---
# <a name="dbcc-checkident-transact-sql"></a>DBCC CHECKIDENT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Verifica il valore Identity corrente per la tabella specificata in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] e, se necessario, lo modifica. È inoltre possibile utilizzare DBCC CHECKIDENT per impostare manualmente un nuovo valore Identity corrente per la colonna Identity.  
  
 ![Icona di collegamento a un articolo](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un articolo") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

DBCC CHECKIDENT
 (
    table_name  
        [, { NORESEED | { RESEED [, new_reseed_value ] } } ]  
)  
[ WITH NO_INFOMSGS ]  
```  

```syntaxsql
-- Syntax for Azure Synapse Analytics
DBCC CHECKIDENT   
 (   
    table_name  
        [RESEED, new_reseed_value ]   
)  
[ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *table_name*  
 Nome della tabella di cui verificare il valore Identity corrente. La tabella specificata deve includere una colonna Identity. I nomi delle tabelle devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). I nomi in due o tre parti devono essere delimitati, ad esempio 'Person.AddressType' o [Person.AddressType].
  
 NORESEED  
 Specifica che non è necessario modificare il valore Identity corrente.  
  
 RESEED  
 Specifica che è necessario modificare il valore Identity corrente.  
  
 *new_reseed_value*  
 Nuovo valore da utilizzare come valore corrente della colonna Identity.  
  
 WITH NO_INFOMSGS  
 Disattiva tutti i messaggi informativi.  
  
## <a name="remarks"></a>Osservazioni

 Le correzioni specifiche apportate al valore Identity corrente dipendono dalle specifiche di parametro.  
  
|Comando DBCC CHECKIDENT|Correzione o correzioni Identity apportate|  
|-----------------------------|---------------------------------------------|  
|DBCC CHECKIDENT ( *table_name*, NORESEED )|Il valore Identity corrente non viene reimpostato. DBCC CHECKIDENT restituisce il valore Identity corrente e il valore massimo corrente della colonna Identity. Se i due valori non corrispondono, è consigliabile reimpostare il valore Identity per evitare potenziali errori o gap nella sequenza dei valori.|  
|DBCC CHECKIDENT ( *table_name* )<br /><br /> o<br /><br /> DBCC CHECKIDENT ( *table_name*, RESEED )|Se il valore Identity corrente di una tabella è inferiore al valore Identity massimo archiviato nella colonna Identity, questo viene reimpostato in base al valore massimo della colonna Identity. Vedere la sezione "Eccezioni" riportata di seguito.|  
|DBCC CHECKIDENT ( *table_name*, RESEED, *new_reseed_value* )|Il valore Identity corrente è impostato su *new_reseed_value*. Se dopo la creazione della tabella non è stata inserita alcuna riga o se tutte le righe sono state rimosse mediante l'istruzione TRUNCATE TABLE, la prima riga inserita dopo l'esecuzione di DBCC CHECKIDENT usa *new_reseed_value* come valore Identity. Se sono presenti righe nella tabella o se tutte le righe sono state rimosse tramite l'istruzione DELETE, per la successiva riga inserita viene usato *new_reseed_value* più il valore di [incremento corrente](../../t-sql/functions/ident-incr-transact-sql.md). Se una transazione inserisce una riga e viene in seguito sottoposta a rollback, per la successiva riga inserita viene usato *new_reseed_value* + il valore di [incremento corrente](../../t-sql/functions/ident-incr-transact-sql.md) come se la riga fosse stata eliminata. Se la tabella non è vuota, l'impostazione del valore Identity su un numero inferiore al valore massimo della colonna Identity può determinare una delle condizioni seguenti:<br /><br /> \- Se esiste un vincolo PRIMARY KEY o UNIQUE nella colonna Identity, verrà generato il messaggio di errore 2627 per le successive operazioni di inserimento nella tabella poiché il valore Identity generato è in conflitto con i valori esistenti.<br /><br /> \- Se non esiste un vincolo PRIMARY KEY o UNIQUE, le successive operazioni di inserimento causeranno valori Identity duplicati.|  
  
## <a name="exceptions"></a>Eccezioni

 Nella tabella seguente vengono elencate le condizioni in cui DBCC CHECKIDENT non reimposta automaticamente il valore Identity corrente e vengono specificati i metodi per la reimpostazione del valore.  
  
|Condizione|Metodi di reimpostazione|  
|---------------|-------------------|  
|Il valore Identity corrente è maggiore del valore massimo della tabella.|Eseguire DBCC CHECKIDENT (*table_name*, NORESEED) per determinare il valore massimo corrente nella colonna. Specificare quindi tale valore come *new_reseed_value* in un comando DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*).<br /><br /> -OPPURE-<br /><br /> Eseguire DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) con *new_reseed_value* impostato su un valore molto basso, quindi eseguire DBCC CHECKIDENT (*table_name*, RESEED) per correggere il valore.|  
|Tutte le righe sono state eliminate dalla tabella.|Eseguire DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) con *new_reseed_value* impostato sul nuovo valore iniziale.|  
  
## <a name="changing-the-seed-value"></a>Modifica del valore di inizializzazione

 Il valore di inizializzazione è il valore inserito in una colonna Identity per la prima riga caricata nella tabella. Tutte le righe successive contengono il valore Identity corrente più il valore dell'incremento dove il valore Identity corrente è l'ultimo valore Identity generato per la tabella o vista.  
  
 Non è possibile usare DBCC CHECKIDENT per le attività seguenti:  
  
- Modificare il valore di inizializzazione originale specificato per una colonna Identity quando è stata creata la tabella o la vista.  
  
- Reinizializzare righe esistenti in una tabella o vista.  
  
 Per modificare il valore di inizializzazione originale e reinizializzare le eventuali righe esistenti, eliminare la colonna Identity e ricrearla specificando il nuovo valore di inizializzazione. Quando la tabella contiene dati, i numeri di identità vengono aggiunti alle righe esistenti con i valori di inizializzazione e incremento specificati. L'ordine con cui le righe vengono aggiornate non è prevedibile.  
  
## <a name="result-sets"></a>Set di risultati

 Indipendentemente dalla definizione delle opzioni per una tabella che include una colonna Identity, DBCC CHECKIDENT restituisce il messaggio seguente per tutte le operazioni tranne una, ovvero quando si specifica un nuovo valore di inizializzazione.  
  
`Checking identity information: current identity value '\<current identity value>', current column value '\<current column value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
 Quando DBCC CHECKIDENT viene usato per specificare un nuovo valore di inizializzazione usando RESEED *new_reseed_value*, viene restituito il messaggio seguente.  
  
`Checking identity information: current identity value '\<current identity value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
## <a name="permissions"></a>Autorizzazioni

 Il chiamante deve essere proprietario dello schema contenente la tabella o membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_owner** e **db_ddladmin**.

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] richiede le autorizzazioni **db_owner**.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-resetting-the-current-identity-value-if-its-needed"></a>R. Reimpostazione del valore Identity corrente, se necessario  
 Nell'esempio seguente viene reimpostato il valore Identity corrente, se necessario, della tabella specificata nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType');  
GO  
```  
  
### <a name="b-reporting-the-current-identity-value"></a>B. Visualizzazione del valore Identity corrente

 Nell'esempio seguente viene visualizzato il valore Identity corrente della tabella specificata nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e, se è errato, non viene corretto.  
  
```sql
USE AdventureWorks2012;
GO  
DBCC CHECKIDENT ('Person.AddressType', NORESEED);
GO  
```  
  
### <a name="c-forcing-the-current-identity-value-to-a-new-value"></a>C. Forzatura del valore Identity corrente su un nuovo valore  
 Nell'esempio seguente il valore Identity corrente della colonna `AddressTypeID` nella tabella `AddressType` viene impostato su 10. Poiché nella tabella sono presenti righe, la successiva riga inserita userà il valore 11, ovvero il nuovo valore Identity corrente definito per la colonna più 1, che rappresenta il valore di incremento della colonna.  

```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType', RESEED, 10);  
GO  
```

### <a name="d-resetting-the-identity-value-on-an-empty-table"></a>D. Reimpostazione del valore Identity in una tabella vuota

 L'esempio seguente il valore Identity corrente della colonna `ErrorLogID` nella tabella `ErrorLog` viene impostato su 1, dopo l'eliminazione di tutti i record dalla tabella. Poiché nella tabella non sono presenti righe, la successiva riga inserita userà il valore 1, ovvero il nuovo valore Identity corrente senza aggiungere il valore di incremento definito per la colonna.  
  
```sql
USE AdventureWorks2012;  
GO  
TRUNCATE TABLE dbo.ErrorLog
GO
DBCC CHECKIDENT ('dbo.ErrorLog', RESEED, 1);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche

[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[IDENTITY &#40;proprietà&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
[Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md)  
[USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
[IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)  
[IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)  
