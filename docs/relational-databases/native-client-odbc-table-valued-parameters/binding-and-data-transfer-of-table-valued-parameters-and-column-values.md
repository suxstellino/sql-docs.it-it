---
title: Trasferimento dati di parametri di Table-Valued
description: Descrivere Trasferimento dati di parametri di Table-Valued
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: native-client
ms.topic: reference
ms.assetid: 0a2ea462-d613-42b6-870f-c7fa086a6b42
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 07/01/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a7a3ea0c3448de104a2742f7255c026946b405b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752431"
---
# <a name="binding-and-data-transfer-of-table-valued-parameters-and-column-values"></a>Associazione e trasferimento dati di valori di colonna e parametri con valori di tabella

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

I parametri con valori di tabella (TVP), come gli altri parametri, devono essere associati prima che vengano passati al server. L'applicazione associa i parametri con valori di tabella nello stesso modo in cui vengono associati altri parametri: utilizzando SQLBindParameter o chiamate equivalenti a SQLSetDescField o SQLSetDescRec. Il tipo di dati del server per un parametro con valori di tabella è SQL_SS_TABLE. Il tipo C può essere specificato come SQL_C_DEFAULT o SQL_C_BINARY.  

In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive, sono supportati solo parametri con valori di tabella di input. Pertanto, qualsiasi tentativo di impostare SQL_DESC_PARAMETER_TYPE su un valore diverso da SQL_PARAM_INPUT restituisce SQL_ERROR con SQLSTATE = HY105 e il messaggio "tipo di parametro non valido".  

È possibile assegnare valori predefiniti a intere colonne dei parametri con valori di tabella utilizzando l'attributo SQL_CA_SS_COL_HAS_DEFAULT_VALUE. Ai singoli valori della colonna di parametri con valori di tabella, tuttavia, non è possibile assegnare valori predefiniti utilizzando SQL_DEFAULT_PARAM in *StrLen_or_IndPtr* con SQLBindParameter. I parametri con valori di tabella nel suo complesso non possono essere impostati su un valore predefinito usando SQL_DEFAULT_PARAM in *StrLen_or_IndPtr* con SQLBindParameter. Se queste regole non sono seguite, SQLExecute o SQLExecDirect restituisce SQL_ERROR. Viene generato un record di diagnostica con SQLSTATE = 07S01 e il messaggio "utilizzo non valido del parametro predefinito per il parametro \<p> ", dove \<p> è l'ordinale di TVP nell'istruzione di query.  

> [!Note]
> I parametri con valori di tabella non hanno un valore predefinito che può essere impostato, perché SQL_DEFAULT_PARAM non indica righe. Quindi, se non sono presenti righe, non sono presenti colonne da associare.

Dopo avere associato il parametro con valori di tabella, dovrà essere associata anche ogni colonna del parametro. A tale scopo, l'applicazione chiama prima SQLSetStmtAttr per impostare SQL_SOPT_SS_PARAM_FOCUS sull'ordinale di un parametro con valori di tabella. L'applicazione associa le colonne del parametro con valori di tabella tramite chiamate alle routine seguenti: SQLBindParameter, SQLSetDescRec e SQLSetDescField. Impostando SQL_SOPT_SS_PARAM_FOCUS su 0, viene ripristinato l'effetto consueto di SQLBindParameter, SQLSetDescRec e SQLSetDescField in operando sui parametri di primo livello normali.

> [!Note]
> Per i driver ODBC Linux e Mac con unixODBC 2.3.1 in 2.3.4, quando si imposta il nome TVP tramite SQLSetDescField con il campo del descrittore SQL_CA_SS_TYPE_NAME, unixODBC non esegue automaticamente la conversione tra le stringhe ANSI e Unicode a seconda della funzione esatta chiamata (SQLSetDescFieldA/SQLSetDescFieldW). Per impostare il nome TVP, è necessario usare sempre SQLBindParameter o SQLSetDescFieldW con una stringa Unicode (UTF-16).

La ricezione e l'invio di dati effettivi non riguardano il parametro con valori di tabella stesso ma ognuna delle colonne che lo costituiscono. Poiché il parametro con valori di tabella è una pseudo colonna, i parametri per SQLBindParameter fanno riferimento a attributi diversi rispetto ad altri tipi di dati, come indicato di seguito:  

| Parametro | Attributo correlato per i tipi di parametro non con valori di tabella, incluse le colonne | Attributo correlato per i parametri con valori di tabella |
|-----------|---------------------------------------------------------------------------|-----------------------------------------------|
|*InputOutputType*|SQL_DESC_PARAMETER_TYPE in IPD.<br /><br /> Per le colonne dei parametri con valori di tabella, deve corrispondere all'impostazione per il parametro con valori di tabella stesso.|SQL_DESC_PARAMETER_TYPE in IPD.<br /><br /> Deve essere SQL_PARAM_INPUT.|  
|*ValueType*|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in APD.|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in APD.<br /><br /> Deve essere SQL_C_DEFAULT o SQL_C_BINARY.|  
|*ParameterType*|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in IPD.|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in IPD.<br /><br /> Deve essere SQL_SS_TABLE.|  
|*ColumnSize*|SQL_DESC_LENGTH o SQL_DESC_PRECISION in IPD.<br /><br /> Dipende dal valore di *ParameterType*.|SQL_DESC_ARRAY_SIZE<br /><br /> Può essere impostato anche utilizzando SQL_ATTR_PARAM_SET_SIZE quando lo stato attivo del parametro è impostato sul parametro con valori di tabella.<br /><br /> Per un parametro con valori di tabella, è il numero di righe nei buffer delle colonne dei parametri con valori di tabella.|  
|*DecimalDigits*|SQL_DESC_PRECISION o SQL_DESC_SCALE in IPD.|Non utilizzato. Deve essere 0.<br /><br /> Se questo parametro non è 0, SQLBindParameter restituisce SQL_ERROR e viene generato un record di diagnostica con SQLSTATE = HY104 e il messaggio "precisione o scala non valida".|  
|*ParameterValuePtr*|SQL_DESC_DATA_PTR in APD.|SQL_CA_SS_TYPE_NAME.<br /><br /> Si tratta di un valore facoltativo per le chiamate a stored procedure e può essere specificato se non è obbligatorio. Deve essere specificato per istruzioni SQL che non sono chiamate di procedura.<br /><br /> Questo parametro serve anche come valore univoco utilizzabile dall'applicazione per identificare il parametro con valori di tabella quando viene utilizzata l'associazione variabile di righe. Per ulteriori informazioni, vedere la sezione "Associazione variabile di righe di parametri con valori di tabella" più avanti n questo argomento.<br /><br /> Quando si specifica un nome di tipo di parametro con valori di tabella in una chiamata a SQLBindParameter, è necessario specificarlo come valore Unicode, anche nelle applicazioni compilate come applicazioni ANSI. Il valore utilizzato per il parametro *StrLen_or_IndPtr* deve essere SQL_NTS o la lunghezza della stringa del nome moltiplicato per le dimensioni di (WCHAR).|  
|*BufferLength*|SQL_DESC_OCTET_LENGTH in APD.|Lunghezza del nome del tipo di parametro con valori di tabella espressa in byte.<br /><br /> Questo può essere SQL_NTS se il nome del tipo è con terminazione null o 0 se il nome del tipo di parametro con valori di tabella non è obbligatorio.|  
|*StrLen_or_IndPtr*|SQL_DESC_OCTET_LENGTH_PTR in APD.|SQL_DESC_OCTET_LENGTH_PTR in APD.<br /><br /> Per i parametri con valori di tabella è un conteggio di righe anziché una lunghezza di dati.|  
||||

Sono supportate due modalità di trasferimento dati per i parametri con valori di tabella: associazione di righe fissa e variabile.  

## <a name="fixed-table-valued-parameter-row-binding"></a>Associazione fissa di righe di parametri con valori di tabella

Per l'associazione fissa di righe, un'applicazione alloca i buffer (o le matrici di buffer) di dimensioni sufficienti a contenere tutti i possibili valori delle colonne di input. L'applicazione effettua quanto segue:  

1. Associa tutti i parametri usando le chiamate SQLBindParameter, SQLSetDescRec o SQLSetDescField.  

    1. Imposta SQL_DESC_ARRAY_SIZE sul numero massimo di righe che possono essere trasferite per ogni parametro con valori di tabella. Questa operazione può essere eseguita nella chiamata SQLBindParameter.  

2. Chiama SQLSetStmtAttr per impostare SQL_SOPT_SS_PARAM_FOCUS sull'ordinale di ogni parametro con valori di tabella.  

    1. Per ogni parametro con valori di tabella, associa le colonne di parametri con valori di tabella tramite chiamate SQLBindParameter, SQLSetDescRec o SQLSetDescField.  

    2. Per ogni colonna di parametri con valori di tabella con valori predefiniti, chiama SQLSetDescField per impostare SQL_CA_SS_COL_HAS_DEFAULT_VALUE su 1.  

3. Chiama SQLSetStmtAttr per impostare SQL_SOPT_SS_PARAM_FOCUS su 0. Questa operazione deve essere eseguita prima della chiamata a SQLExecute o SQLExecDirect. In caso contrario, viene restituito SQL_ERROR e viene generato un record di diagnostica con SQLSTATE = HY024 e il messaggio "valore attributo non valido, SQL_SOPT_SS_PARAM_FOCUS (deve essere zero in fase di esecuzione)".  

4. Imposta *StrLen_or_IndPtr* o SQL_DESC_OCTET_LENGTH_PTR su SQL_DEFAULT_PARAM per un parametro con valori di tabella senza righe o il numero di righe da trasferire alla chiamata successiva di SQLExecute o SQLExecDirect se il parametro con valori di tabella contiene righe. Impossibile impostare *StrLen_or_IndPtr* o SQL_DESC_OCTET_LENGTH_PTR su SQL_NULL_DATA per un parametro con valori di tabella perché i parametri con valori di tabella non ammettono valori null, anche se le colonne costituenti dei parametri con valori di tabella possono essere nullable. Se questa proprietà è impostata su un valore non valido, SQLExecute o SQLExecDirect restituisce SQL_ERROR e viene generato un record di diagnostica con SQLSTATE = HY090 e il messaggio "lunghezza di stringa o di buffer non valida per il parametro \<p> ", dove p è il numero di parametro.

5. Chiama SQLExecute o SQLExecDirect.

    I valori della colonna di parametri con valori di tabella di input possono essere passati in parti se *StrLen_or_IndPtr* è impostato su SQL_LEN_DATA_AT_EXEC (*length*) o SQL_DATA_AT_EXEC per la colonna. Si tratta di una procedura analoga a quella utilizzata per passare i valori in parti quando vengono utilizzate matrici di parametri. Come per tutti i parametri data-at-execution, SQLParamData non indica la riga della matrice per cui il driver richiede i dati; l'applicazione deve occuparsi di questo. L'applicazione non può ipotizzare l'ordine in cui il driver richiede i valori.  

## <a name="variable-table-valued-parameter-row-binding"></a>Associazione variabile di righe di parametri con valori di tabella  

Per l'associazione di righe variabile, le righe vengono trasferite in batch in fase di esecuzione e l'applicazione passa le righe al driver su richiesta. Si tratta di una procedura simile al data-at-execution per i valori di parametri singoli. Per l'associazione variabile di righe, l'applicazione effettua quanto segue:  

1. Associa i parametri e le colonne dei parametri con valori di tabella, come descritto nei passaggi da 1 a 3 della sezione precedente, "Fixed Table-Valued Parameter Row binding".  

2. Imposta *StrLen_or_IndPtr* o SQL_DESC_OCTET_LENGTH_PTR per il passaggio di tutti i parametri con valori di tabella in fase di esecuzione al SQL_DATA_AT_EXEC. Se non viene impostato alcun oggetto, il parametro viene elaborato come descritto nella sezione precedente.  

3. Chiama SQLExecute o SQLExecDirect. Restituisce SQL_NEED_DATA se sono presenti parametri SQL_PARAM_INPUT o SQL_PARAM_INPUT_OUTPUT da gestire come parametri data-at-execution. In questo caso, l'applicazione effettua quanto segue:  

    - Chiama SQLParamData. Viene restituito il valore *ParameterValuePtr* per un parametro data-at-execution e un codice restituito di SQL_NEED_DATA. Quando tutti i dati dei parametri sono stati passati al driver, SQLParamData restituisce SQL_SUCCESS, SQL_SUCCESS_WITH_INFO o SQL_ERROR. Per i parametri data-at-execution, *ParameterValuePtr*, che corrisponde al campo del descrittore SQL_DESC_DATA_PTR, può essere considerato come un token per identificare un parametro per il quale un valore è richiesto in modo univoco. Tale "token" viene passato dall'applicazione al driver in fase di associazione, quindi viene passato nuovamente all'applicazione in fase di esecuzione.  
  
4. Per inviare i dati delle righe di parametri con valori di tabella per i parametri con valori di tabella null, se il parametro con valori di tabella non contiene righe, un'applicazione chiama SQLPutData con *StrLen_Or_Ind* impostato su SQL_DEFAULT_PARAM.  

    Per i TVP non Null, un'applicazione:

    - Imposta *Str_Len_or_Ind* per tutte le colonne di parametri con valori di tabella sui valori appropriati e popola i buffer dei dati per le colonne di parametri con valori di tabella che non devono essere parametri data-at-execution. È possibile utilizzare la funzionalità data-at-execution per le colonne di parametri con valori di tabella seguendo procedure analoghe a quelle necessarie per passare in parti i parametri ordinari al driver.  

    - Chiama SQLPutData con *Str_Len_or_Ind* impostato sul numero di righe da inviare al server. Qualsiasi valore non compreso nell'intervallo compreso tra 0 e SQL_DESC_ARRAY_SIZE o SQL_DEFAULT_PARAM è un errore e restituisce SQLSTATE HY090 con il messaggio "lunghezza di stringa o di buffer non valida". 0 indica che tutte le righe sono state inviate e che non sono disponibili altri dati per un parametro con valori di tabella (come indicato nel secondo elemento Bullet in questo elenco). SQL_DEFAULT_PARAM può essere utilizzato solo la prima volta che il driver richiede dati per un parametro con valori di tabella (come descritto nel primo punto elenco).  

5. Quando tutte le righe sono state inviate, chiama SQLPutData per il parametro con valori di tabella con un valore *Str_Len_or_Ind* pari a 0, quindi procedere con il passaggio 3a precedente.  

6. Chiama di nuovo SQLParamData. Se sono presenti parametri data-at-execution tra le colonne di parametri con valori di tabella, tali parametri sono identificati dal valore *ValuePtrPtr* restituito da SQLParamData. Quando sono disponibili tutti i valori di colonna, SQLParamData restituisce il valore *ParameterValuePtr* per il parametro con valori di tabella e l'applicazione viene riavviata.  

## <a name="next-steps"></a>Passaggi successivi

[ODBC Parameters con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)