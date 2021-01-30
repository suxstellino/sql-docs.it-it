---
description: sp_tableoption (Transact-SQL)
title: sp_tableoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tableoption_TSQL
- sp_tableoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tableoption
ms.assetid: 0a57462c-1057-4c7d-bce3-852cc898341d
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d2846b289c31777adec6d548addba11a9c7e463d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204342"
---
# <a name="sp_tableoption-transact-sql"></a>sp_tableoption (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Imposta i valori delle opzioni per le tabelle definite dall'utente. sp_tableoption può essere utilizzato per controllare il comportamento all'interno di righe delle tabelle con colonne di tipo **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **Text**, **ntext**, **Image** o di grandi dimensioni definite dall'utente.  
  
> [!IMPORTANT]  
>  La caratteristica text in row verrà rimossa nelle versioni future di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per archiviare dati di valori di grandi dimensioni, è consigliabile utilizzare i tipi di dati **varchar (max)**, **nvarchar (max)** e **varbinary (max)** .  
  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_tableoption [ @TableNamePattern = ] 'table'   
     , [ @OptionName = ] 'option_name'   
     ,[ @OptionValue =] 'value'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @TableNamePattern =]'*tabella*'  
 Nome completo o non qualificato di una tabella di database definita dall'utente. Nel caso di un nome qualificato di tabella, ovvero contenente un nome di database, il nome del database deve corrispondere a quello del database corrente. Non è possibile configurare le opzioni tabella per più tabelle contemporaneamente. *Table* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito.  
  
 [ @OptionName =]'*option_name*'  
 Nome di un'opzione di tabella. *option_name* è di tipo **varchar (35)** e non prevede alcun valore predefinito null. *option_name* può essere uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|table lock on bulk load|Quando questa opzione è disabilitata (impostazione predefinita), durante il processo di caricamento bulk nelle tabelle definite dall'utente vengono acquisiti blocchi di riga. Se abilitata, viene acquisito un blocco di tipo aggiornamento bulk.|  
|insert row lock|Non più supportata.<br /><br /> Questa opzione non influisce sulla funzionalità di blocco di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed è disponibile solo per compatibilità con script e procedure esistenti.|  
|text in row|Quando questa opzione è disabilitata, ovvero impostata su OFF o 0 (impostazione predefinita), il funzionamento corrente rimane invariato e la riga non contiene valori BLOB.<br /><br /> Quando è specificato e @OptionValue è attivato (abilitato) o un valore intero compreso tra 24 e 7000, le nuove stringhe di tipo **Text**, **ntext** o **Image** vengono archiviate direttamente nella riga di dati. Tutti i BLOB esistenti (oggetto binario di grandi dimensioni: dati di tipo **Text**, **ntext** o **Image** ) verranno convertiti in testo in formato di riga quando il valore del BLOB viene aggiornato. Per altre informazioni, vedere la sezione Osservazioni.|  
|large value types out of row|1 = le colonne **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** e di tipo definito dall'utente (UDT) di grandi dimensioni nella tabella vengono archiviate all'esterno di righe, con un puntatore di 16 byte alla radice.<br /><br /> 0 = i valori **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** e UDT di grandi dimensioni vengono archiviati direttamente nella riga di dati, fino a un limite di 8000 byte e fino a quando il valore può rientrare nel record. Se le dimensioni del record non sono sufficienti per il valore, all'interno della riga viene archiviato un puntatore e i dati restanti vengono archiviati all'esterno della riga nello spazio di archiviazione LOB. Il valore predefinito è 0.<br /><br /> Il tipo definito dall'utente (UDT) di grandi dimensioni si applica a: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive. <br /><br /> Utilizzare l'opzione TEXTIMAGE_ON di [Create Table](../../t-sql/statements/create-table-transact-sql.md) per specificare un percorso per l'archiviazione di tipi di dati di grandi dimensioni. |  
|formato di archiviazione vardecimal|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> Se TRUE, ON o 1, la tabella designata è abilitata per il formato di archiviazione vardecimal. Se FALSE, OFF o 0, il formato di archiviazione vardecimal non è abilitato per la tabella. Il formato di archiviazione vardecimal può essere abilitato solo quando il database è stato abilitato per il formato di archiviazione vardecimal utilizzando [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md). In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive il formato di archiviazione **vardecimal** è deprecato. ed è necessario usare il tipo di compressione ROW. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md). Il valore predefinito è 0.|  
  
 [ @OptionValue =]'*valore*'  
 Indica se la *option_name* è abilitata (true, on o 1) oppure disabilitata (false, off o 0). *value* è di tipo **varchar (12)** e non prevede alcun valore predefinito. il *valore* non fa distinzione tra maiuscole e minuscole.  
  
 Per l'opzione text in row, i valori dell'opzione validi sono 0, ON, OFF o un numero intero compreso tra 24 e 7000. Quando *value* è on, il valore predefinito del limite è 256 byte.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o numero di errore (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_tableoption può essere utilizzata solo per impostare i valori delle opzioni per le tabelle definite dall'utente. Per visualizzare le proprietà della tabella, utilizzare OBJECTPROPERTY o eseguire una query su sys. Tables.  
  
 In sp_tableoption è possibile abilitare o disabilitare l'opzione text in row solo in tabelle contenenti colonne di testo. Nel caso di tabelle prive di colonne di questo tipo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un errore.  
  
 Quando l'opzione text in row è abilitata, il @OptionValue parametro consente agli utenti di specificare le dimensioni massime da archiviare in una riga per un BLOB. I possibili valori sono compresi tra 24 e 7000 byte. Il valore predefinito è 256 byte.  
  
 le stringhe di tipo **Text**, **ntext** o **Image** vengono archiviate nella riga di dati se si applicano le condizioni seguenti:  
  
-   L'opzione text in row è abilitata.  
  
-   La lunghezza della stringa è inferiore al valore limite specificato in .@OptionValue  
  
-   Nella riga di dati lo spazio disponibile è sufficiente.  
  
 Quando le stringhe BLOB vengono archiviate nella riga di dati, la lettura e la scrittura delle stringhe di tipo **Text**, **ntext** o **Image** possono essere veloci come la lettura o la scrittura di stringhe di caratteri e binarie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non accede a pagine separate per la lettura o scrittura di stringhe BLOB.  
  
 Se una stringa di tipo **Text**, **ntext** o **Image** è più grande del limite specificato o dello spazio disponibile nella riga, i puntatori vengono invece archiviati nella riga. Le condizioni per l'archiviazione delle stringhe BLOB nella riga devono comunque essere soddisfatte, ovvero lo spazio disponibile nella riga di dati deve essere sufficiente per includervi i puntatori.  
  
 Le stringhe e i puntatori BLOB archiviati nella riga di una tabella vengono gestiti in modo analogo alle stringhe a lunghezza variabile, ovvero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza solo il numero di byte necessari per l'archiviazione della stringa o del puntatore.  
  
 Quando si abilita l'opzione text in row per la prima volta, le stringhe BLOB esistenti non vengono convertite immediatamente, ma solo in fase di aggiornamento. Analogamente, quando si aumenta il limite dell'opzione text in Row, le stringhe di tipo **Text**, **ntext** o **Image** già presenti nella riga di dati non verranno convertite in modo da rispettare il nuovo limite fino al momento in cui vengono aggiornate.  
  
> [!NOTE]  
>  Per disabilitare l'opzione text in row o ridurne il valore limite, è necessario convertire tutti i valori BLOB. L'operazione può pertanto richiedere tempi lunghi, a seconda del numero di stringhe BLOB da convertire. Durante il processo di conversione la tabella viene bloccata.  
  
 Per una variabile di tabella, così come per una funzione che restituisce una variabile di tabella, l'opzione text in row viene abilitata automaticamente con il valore predefinito 256 per il parametro inline limit. Questa opzione non può essere modificata.  
  
 L'opzione text in row supporta le funzioni TEXTPTR, WRITETEXT, UPDATETEXT e READTEXT. Gli utenti possono leggere parti di un valore BLOB tramite la funzione SUBSTRING(). È importante sottolineare, tuttavia, che i limiti massimi relativi a durata e numero per i puntatori di testo all'interno di righe sono diversi da quelli degli altri puntatori di testo.  
  
 Per modificare di nuovo il formato di archiviazione di una tabella da vardecimal a decimal, è necessario che per il database sia impostata la modalità di recupero SIMPLE. La modifica della modalità di recupero causa l'interruzione della catena di log per il backup. Dopo la rimozione del formato di archiviazione vardecimal da una tabella è pertanto necessario creare un backup completo del database.  
  
 Se si converte una colonna con tipo di dati LOB esistente (text, ntext o image) in tipi di valori di grandi dimensioni da piccoli a medi (varchar (max), nvarchar (max) o varbinary (max)), la maggior parte delle istruzioni non fa riferimento alle colonne di tipo valore grande nell'ambiente in uso, provare a modificare **large_value_types_out_of_row** in **1** per ottenere prestazioni ottimali. Quando viene modificato il valore dell'opzione **large_value_types_out_of_row** , i valori varchar (max), nvarchar (max), varbinary (max) e XML esistenti non vengono convertiti immediatamente. L'archiviazione delle stringhe viene modificata nel corso del successivo aggiornamento. Qualsiasi nuovo valore inserito in una tabella viene archiviato in base all'opzione di tabella attiva. Per i risultati immediati, effettuare una copia dei dati e quindi ripopolare la tabella dopo aver modificato l'impostazione di **large_value_types_out_of_row** o aggiornare ogni colonna di tipo valore small-to-Medium Large in modo che l'archiviazione delle stringhe venga modificata con l'opzione table attiva. Provare a ricompilare gli indici nella tabella dopo l'aggiornamento o il ripopolamento per ridurre la tabella. 
    
  
## <a name="permissions"></a>Autorizzazioni  
 Per l'esecuzione di sp_tableoption è richiesta l'autorizzazione ALTER per la tabella.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-storing-xml-data-out-of-the-row"></a>R. Archiviazione di dati xml all'esterno delle righe  
 Nell'esempio seguente viene specificato che i dati **XML** nella `HumanResources.JobCandidate` tabella vengono archiviati all'esterno di righe.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'HumanResources.JobCandidate', 'large value types out of row', 1;  
```  
  
### <a name="b-enabling-vardecimal-storage-format-on-a-table"></a>B. Abilitazione del formato di archiviazione vardecimal in una tabella  
 Nell'esempio seguente viene modificata la `Production.WorkOrderRouting` tabella per archiviare il `decimal` tipo di dati nel `vardecimal` formato di archiviazione.  

```sql  
USE master;  
GO  
-- The database must be enabled for vardecimal storage format  
-- before a table can be enabled for vardecimal storage format  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'Production.WorkOrderRouting',   
   'vardecimal storage format', 'ON';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
