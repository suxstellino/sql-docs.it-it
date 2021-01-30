---
description: sp_table_validation (Transact-SQL)
title: sp_table_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_table_validation_TSQL
- sp_table_validation
helpviewer_keywords:
- sp_table_validation
ms.assetid: 31b25f9b-9b62-496e-a97e-441d5fd6e767
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 255c30ea6b3144e781007bff3ae748c8e5a960ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189260"
---
# <a name="sp_table_validation-transact-sql"></a>sp_table_validation (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sul conteggio delle righe o sul valore di checksum per una tabella o vista indicizzata oppure confronta le informazioni sul conteggio delle righe o sul valore di checksum specificate con la tabella o vista indicizzata. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione e nel database di sottoscrizione del Sottoscrittore. *Non supportato per i Publisher Oracle*.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_table_validation [ @table = ] 'table'  
    [ , [ @expected_rowcount = ] type_of_check_requested OUTPUT]  
    [ , [ @expected_checksum = ] expected_checksum OUTPUT]  
    [ , [ @rowcount_only = ] rowcount_only ]  
    [ , [ @owner = ] 'owner' ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @table_name = ] table_name ]  
    [ , [ @column_list = ] 'column_list' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table = ] 'table'` Nome della tabella. *Table* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @expected_rowcount = ] expected_rowcountOUTPUT` Specifica se restituire il numero previsto di righe nella tabella. *expected_rowcount* è di **tipo int** e il valore predefinito è null. con il quale viene restituito il conteggio delle righe effettivo come parametro di output. Se viene specificato un altro valore, questo viene confrontato con il conteggio delle righe effettivo per rilevare eventuali differenze.  
  
`[ @expected_checksum = ] expected_checksumOUTPUT` Specifica se restituire il valore di checksum previsto per la tabella. *expected_checksum* è **numerico** e il valore predefinito è null. con cui viene restituito il valore di checksum effettivo come parametro di output. Se viene specificato un altro valore, questo viene confrontato con il valore di checksum effettivo per rilevare eventuali differenze.  
  
`[ @rowcount_only = ] type_of_check_requested` Specifica il tipo di checksum o di conteggio delle righe da eseguire. *type_of_check_requested* è di **smallint** e il valore predefinito è **1**.  
  
 Se è **0**, eseguire un conteggio delle righe e un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] checksum compatibile con 7,0.  
  
 Se è **1**, eseguire solo un controllo RowCount.  
  
 Se è **2**, eseguire un conteggio delle righe e un checksum binario.  
  
`[ @owner = ] 'owner'` Nome del proprietario della tabella. *owner* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @full_or_fast = ] full_or_fast` Metodo utilizzato per calcolare il conteggio delle righe. *full_or_fast* è di **tinyint** e il valore predefinito è **2**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Esegue un conteggio completo con COUNT(*).|  
|**1**|Esegue un conteggio rapido da **sysindexes. Rows**. Il conteggio delle righe in **sysindexes** è molto più veloce rispetto al conteggio delle righe nella tabella effettiva. Tuttavia, poiché **sysindexes** viene aggiornato in modo differito, il conteggio delle righe potrebbe non essere accurato.|  
|**2** (impostazione predefinita)|Esegue un conteggio rapido condizionale eseguendo innanzitutto un tentativo con il metodo rapido. Se il metodo rapido evidenzia delle differenze, viene applicato il metodo completo. Se *expected_rowcount* è null e il stored procedure viene usato per ottenere il valore, viene sempre usato un conteggio completo (*).|  
  
`[ @shutdown_agent = ] shutdown_agent` Se il agente di distribuzione è in esecuzione **sp_table_validation**, specifica se il agente di distribuzione dovrebbe essere arrestato immediatamente dopo il completamento della convalida. *shutdown_agent* è di **bit** e il valore predefinito è **0**. Se è **0**, l'agente di replica non viene arrestato. Se è **1**, viene generato l'errore 20578 e l'agente di replica viene segnalato per l'arresto. Questo parametro viene ignorato quando **sp_table_validation** viene eseguito direttamente da un utente.  
  
`[ @table_name = ] table_name` Nome della tabella della vista utilizzata per i messaggi di output. *table_name* è di **tipo sysname** e il valore predefinito è **\@ Table**.  
  
`[ @column_list = ] 'column_list'` Elenco di colonne da utilizzare nella funzione checksum. *column_list* è di **tipo nvarchar (4000)** e il valore predefinito è null. Abilita la convalida degli articoli di tipo merge per specificare un elenco di colonne che non include le colonne calcolate e timestamp.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Se si esegue una convalida del checksum e il valore di checksum previsto è uguale al checksum nella tabella, **sp_table_validation** restituisce un messaggio che indica che la tabella ha superato la convalida di checksum. In caso contrario, restituisce un messaggio per indicare che la tabella potrebbe non essere sincronizzata e specifica la differenza tra il numero di righe previsto e quello effettivo.  
  
 Se si esegue una convalida con conteggio delle righe e il numero previsto di righe è uguale al numero della tabella, **sp_table_validation** restituisce un messaggio che indica che la tabella ha superato la convalida del conteggio delle righe. In caso contrario, restituisce un messaggio per indicare che la tabella potrebbe non essere sincronizzata e specifica la differenza tra il numero di righe previsto e quello effettivo.  
  
## <a name="remarks"></a>Commenti  
 **sp_table_validation** viene utilizzato in tutti i tipi di replica. **sp_table_validation** non è supportato per i Publisher Oracle.  
  
 Con l'operazione di checksum viene eseguito un controllo di ridondanza ciclico (CRC, Cyclic Redundancy Check) a 32 bit sull'intera immagine delle righe all'interno della pagina. Non esegue un controllo solo su colonne specifiche e non è eseguibile in una vista o in una partizione verticale della tabella. Inoltre, il checksum ignora il contenuto delle colonne **Text** e **Image** (per impostazione predefinita).  
  
 Quando si esegue un'operazione di checksum, è necessario che la struttura della tabella nei due server sia identica, ovvero le tabelle nei due server devono includere le stesse colonne nel medesimo ordine aventi lo stesso tipo di dati, la stessa lunghezza e le stesse condizioni NULL/NOT NULL. Se, ad esempio, nel server di pubblicazione è stata eseguita un'istruzione CREATE TABLE e quindi un'istruzione ALTER TABLE per l'inserimento di colonne, ma lo script applicato al Sottoscrittore è una tabella CREATE semplice, la struttura NON è identica. Se non si è certi che la struttura delle due tabelle sia identica, esaminare [syscolumns](../../relational-databases/system-compatibility-views/sys-syscolumns-transact-sql.md) e verificare che l'offset in ogni tabella sia lo stesso.  
  
 È probabile che i valori a virgola mobile generino differenze di checksum se è stata utilizzata l' **utilità bcp** in modalità carattere, ovvero se la pubblicazione include [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non. Ciò è dovuto a differenze di precisione minime ma inevitabili nella conversione da e verso la modalità carattere.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire **sp_table_validation**, è necessario disporre delle autorizzazioni SELECT per la tabella da convalidare.  
  
## <a name="see-also"></a>Vedere anche  
 [CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)   
 [sp_article_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_publication_validation &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
