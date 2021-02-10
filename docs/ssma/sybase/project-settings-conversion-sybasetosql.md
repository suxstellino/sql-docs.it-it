---
description: Impostazioni del progetto (conversione) (SybaseToSQL)
title: Impostazioni progetto (conversione) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eeb80fa5-f530-4f21-beee-25f5a4b8ace6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 66d1ce573f464f14e0b738fb4c461be031db64ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070074"
---
# <a name="project-settings-conversion-sybasetosql"></a>Impostazioni del progetto (conversione) (SybaseToSQL)

La pagina **conversione** della finestra di dialogo **Impostazioni progetto** contiene impostazioni che consentono di personalizzare il modo in cui SSMA converte la sintassi di SAP Adaptive Server Enterprise (ASE) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la sintassi SQL di Azure.

Il riquadro **conversione** è disponibile nelle finestre di dialogo **Impostazioni** progetto e **Impostazioni progetto predefinite** :

- Se si desidera specificare le impostazioni per tutti i progetti SSMA, nel menu **strumenti** selezionare **Impostazioni progetto predefinite**, fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **conversione**.

- Per specificare le impostazioni per il progetto corrente, scegliere **Impostazioni progetto** dal menu **strumenti** , fare clic su **generale** nella parte inferiore del riquadro sinistro, quindi fare clic su **conversione**.

## <a name="miscellaneous-section"></a>Sezione varie

### <a name="error"></a>@@ERROR

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL e ambiente del servizio app usano codici di errore diversi.

Usare questa impostazione per specificare il tipo di messaggio (avviso o errore) visualizzato da SSMA nel riquadro Output o Elenco errori quando viene rilevato un riferimento a `@@ERROR` nel codice dell'ambiente del servizio app.

- Se si seleziona **Convert e Mark con Warning**, SSMA convertirà le istruzioni e le contrassegnerà con i commenti di avviso.
- Se si seleziona **contrassegna con errore**, SSMA ignorerà la conversione e contrassegnerà le istruzioni con commenti di errore.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Converti e contrassegno con avviso|
|Optimistic|Converti e contrassegno con avviso|
|Full|Contrassegno con errore|

### <a name="conversion-of-like-operator"></a>Conversione dell'operatore LIKE

Specifica se convertire gli `LIKE` operandi in modo che corrispondano al comportamento di SAP ASE. Il punto è che l'ambiente del servizio app rimuove gli spazi vuoti finali in un modello like. La soluzione alternativa consiste nel creare un cast di espressione a destra in un tipo di dati a lunghezza fissa con una precisione massima.
  
- Selezionare **conversione semplice** per convertire le espressioni senza alcuna correzione.
- Per usare il comportamento dell'ambiente del servizio app **, selezionare cast a lunghezza fissa.**
  
Quando si seleziona una modalità di conversione nella casella modalità, SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Conversione semplice|
|Optimistic|Conversione semplice|
|Full|Cast a lunghezza fissa|

### <a name="convert-or-cast-empty-strings-to-numeric-types"></a>CONVERTE o esegue il CAST di stringhe vuote in tipi numerici

Specifica come gestire stringhe vuote o vuote all'interno `CONVERT` di `CAST` espressioni o con tipo numerico come argomento DataType. Per questa impostazione sono disponibili le opzioni seguenti:

- Selezionare **conversione semplice** per convertire le espressioni senza alcuna correzione.
- Se è selezionata una **stringa vuota come valore numerico zero, il** parametro `{s}` di stringa verrà sostituito con `CASE ltrim(rtrim({s})) WHEN "" THEN 0 else {s} END` Expression.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Conversione semplice|
|Optimistic|Conversione semplice|
|Full|Stringa vuota come valore numerico zero|

### <a name="concatenation-of-null"></a>Concatenazione di valori NULL

Questa impostazione specifica come convertire la concatenazione di stringhe con `NULL` . Per questa particolare impostazione è possibile impostare le opzioni seguenti:

- Se l'opzione **Wrap with IsNull Function** è selezionata, verrà eseguito il wrapping di ogni non costante `string_expression` nella concatenazione con e gli oggetti `ISNULL(string_expression)` `NULL` verranno sostituiti con una stringa vuota.
- **Mantieni sintassi corrente** manterrà la sintassi originale.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Wrapping con la funzione ISNULL|

### <a name="conversion-of-empty-strings"></a>Conversione di stringhe vuote

Questa impostazione specifica come convertire le stringhe vuote. Per questa particolare impostazione è possibile impostare le opzioni seguenti:

- **Sostituisci tutte le espressioni stringa con lo spazio**
- **Sostituisci costanti stringa vuote con spazio**

Per usare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamento SQL di/Azure, selezionare **Mantieni sintassi corrente**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Sostituisci tutte le espressioni stringa con lo spazio|

### <a name="convert-and-cast-binary-string-conversion"></a>CONVERTIRE e eseguire il CAST della conversione di stringhe binarie

La conversione di valori binari in numeri può restituire valori diversi su piattaforme diverse. Ad esempio, nei processori x86 restituisce in ambiente del servizio app `CONVERT(integer, 0x00000100)` `65536` , ma `256` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ambiente del servizio app restituisce anche valori diversi a seconda dell'ordine dei byte.

Usare questa impostazione per controllare la modalità di conversione di SSMA `CONVERT` ed `CAST` espressioni contenenti valori binari:

- Selezionare **conversione semplice** per convertire le espressioni senza avvisi o correzioni. Usare questa impostazione se si è certi che il server dell'ambiente del servizio app ha un ordine dei byte che non richiede alcuna modifica del valore binario.
- Selezionare **Converti e Correggi** affinché SSMA converta e corregga le espressioni da utilizzare in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'ordine dei byte nelle costanti letterali verrà invertito. Tutti gli altri valori binari (ad esempio, le variabili e le colonne binarie) verranno contrassegnati con errori. Usare questo valore se si è certi che il server dell'ambiente del servizio app ha un ordine dei byte che richiede modifiche ai valori binari.

Selezionare **Converti e contrassegna con avviso** per fare in modo che SSMA converta e corregga le espressioni e contrassegna tutte le espressioni convertite con commenti di avviso.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Converti e contrassegno con avviso|
|Optimistic|Conversione semplice|
|Full|Converti e Correggi|

### <a name="dynamic-sql"></a>SQL dinamica

Usare questa impostazione per specificare il tipo di messaggio (avviso o errore) visualizzato da SSMA nel riquadro **output** o **Elenco errori** quando rileva SQL dinamico nel codice dell'ambiente del servizio app.

- Se si seleziona **Convert e Mark con Warning**, SSMA convertirà il SQL dinamico e contrassegnerà le istruzioni con commenti di avviso.
- Se si seleziona **contrassegna con errore**, SSMA ignorerà la conversione e contrassegnerà le istruzioni con commenti di errore.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Converti e contrassegno con avviso|
|Optimistic|Converti e contrassegno con avviso|
|Full|Contrassegno con errore|

### <a name="equality-check-conversion"></a>Conversione controllo di uguaglianza

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL, se l' `ANSI_NULLS` impostazione è on, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL restituisce `UNKNOWN` quando un confronto di uguaglianza contiene un `NULL` valore. Se `ANSI_NULLS` è off, i confronti di uguaglianza che contengono `NULL` valori restituiscono true quando la colonna e l'espressione confrontate o due espressioni sono entrambe `NULL` . Per impostazione predefinita ( `ANSINULL OFF` ) i confronti di uguaglianza di SAP ASE si comportano come [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL con `ANSI_NULLS OFF` .

- Se si seleziona **conversione semplice**, SSMA convertirà il codice dell'ambiente del servizio app in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure sintassi SQL senza ulteriori controlli per `NULL` i valori. Usare questa impostazione se `ANSI_NULLS` si trova `OFF` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL o se si vuole rivedere i confronti di uguaglianza per ogni singolo caso.
- Se si seleziona **considera i valori null**, SSMA aggiungerà i controlli per i `NULL` valori usando le `IS NULL` `IS NOT NULL` clausole e.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Conversione semplice|
|Optimistic|Conversione semplice|
|Full|Considera valori NULL|

### <a name="format-strings"></a>Stringhe di formato

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL non supporta più l' `format_string` argomento nelle `PRINT` `RAISERROR` istruzioni e. L' `format_string` argomento può inserire i parametri sostituibili direttamente nella stringa e quindi sostituire i parametri in fase di esecuzione. Richiede invece [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la stringa completa usando un valore letterale stringa o una stringa compilata usando una variabile. Per ulteriori informazioni, vedere l'argomento [Print ( [!INCLUDE[tsql](../../includes/tsql-md.md)] )](../../t-sql/language-elements/print-transact-sql.md) .

Quando SSMA rileva un `format_string` argomento, può compilare un valore letterale stringa usando le variabili o creare una nuova variabile e compilare una stringa usando tale variabile.

- Per usare un valore letterale stringa per `PRINT` le `RAISERROR` funzioni e, selezionare **Crea nuova stringa**.

    In questa modalità, se un'istruzione PRINT o RAISERROR non utilizza segnaposti e variabili locali, l'istruzione è invariata. Doppio carattere percentuale (%%) vengono modificati in un singolo carattere di percentuale% nei valori letterali stringa di stampa.

    Se un'istruzione PRINT o RAISERROR utilizza segnaposti e una o più variabili locali, ad esempio nell'esempio seguente:

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA lo convertirà nella sintassi seguente:

    ```sql
    PRINT 'Total: '+ CAST(@percent AS varchar(max)) + '%'
    ```

    Se `format_string` è una variabile, ad esempio nell'istruzione seguente:  

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA non è in grado di eseguire una conversione di stringa semplice e deve creare una nuova variabile:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        CAST (@arg1 AS varchar(max)))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        CAST (@arg2 AS varchar(max)))
    PRINT @print_format_1
    ```

    Quando usa la modalità di creazione di una **nuova stringa** , SSMA presuppone che l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opzione `CONCAT_NULL_YIELDS_NULL` sia `OFF` . SSMA non verifica quindi gli argomenti null.

- Per fare in modo che SSMA crei una nuova variabile per ogni `PRINT` `RAISERROR` istruzione e, quindi usare tale variabile per il valore stringa, selezionare **Crea nuova variabile**.

    In questa modalità, se un' `PRINT` istruzione o non `RAISERROR` Usa i segnaposto e le variabili locali, SSMA sostituisce tutti i caratteri di percentuale doppia ( `%%` ) con il singolo carattere di percentuale per conformarsi alla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi SQL di/Azure.

    Se un' `PRINT` `RAISERROR` istruzione o utilizza segnaposti e una o più variabili locali, ad esempio nell'esempio seguente:

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA lo convertirà nella sintassi seguente:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 = 'Total: %1!%'
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@percent AS VARCHAR(max)), ''))
    PRINT @print_format_1
    ```

    Se `format_string` è una variabile, ad esempio nell'istruzione seguente:

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA crea una nuova variabile come indicato di seguito, verificando la presenza di valori null in ogni argomento:

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@arg1 AS varchar(max)),''))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        ISNULL(CAST (@arg2 AS varchar(max)),''))
    PRINT @print_format_1
    ```

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Crea nuova stringa|
|Optimistic|Crea nuova stringa|
|Full|Crea nuova variabile|

### <a name="insert-an-explicit-value-into-a-timestamp-column"></a>Inserire un valore esplicito in una colonna timestamp

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL non supporta l'inserimento di valori espliciti in una colonna timestamp.

- Per escludere le colonne timestamp dalle `INSERT` istruzioni, selezionare **Escludi colonna**.
- Per stampare un messaggio di errore ogni volta che una colonna timestamp si trova in un' `INSERT` istruzione, selezionare **contrassegno con errore**. In questa modalità le `INSERT` istruzioni non verranno convertite e verranno contrassegnate con i commenti degli errori.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Escludi colonna|
|Optimistic|Escludi colonna|
|Full|Contrassegno con errore|

### <a name="store-temporary-objects-defined-in-procedures"></a>Archiviare oggetti temporanei definiti nelle procedure

Questa impostazione specifica se le definizioni degli oggetti temporanei visualizzate nelle procedure devono essere archiviate nei metadati di origine durante la conversione.

- Selezionare **Sì** per archiviare i metadati.
- Selezionare **No** se gli oggetti non devono essere archiviati.

|Modalità|Valore|
|-|-|
|Predefinito|Sì|
|Optimistic|Sì|
|Full|No|

### <a name="proxy-table-conversion"></a>Conversione di tabelle proxy

Specifica se le tabelle proxy dell'ambiente del servizio app vengono convertite in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabelle SQL/Azure o non vengono convertite e il codice è contrassegnato con commenti di errore.

- Selezionare **Converti** per convertire le tabelle proxy in tabelle normali.
- Selezionare **contrassegno con errore** per contrassegnare semplicemente il codice della tabella proxy con i commenti degli errori.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Contrassegno con errore|
|Optimistic|Contrassegno con errore|
|Full|Contrassegno con errore|

### <a name="raiserror-base-message-number"></a>Numero del messaggio di base RAISERROR

I messaggi utente dell'ambiente del servizio app vengono archiviati in ogni database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i messaggi utente vengono archiviati centralmente e resi disponibili tramite la `sys.messages` vista del catalogo. Inoltre, i messaggi utente dell'ambiente del servizio app iniziano da `20000` , ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i messaggi di errore iniziano da `50001` .

Questa impostazione specifica il numero da aggiungere al numero del messaggio utente dell'ambiente del servizio app per convertirlo in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] messaggio utente. Se il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispone di messaggi utente nella `sys.messages` vista del catalogo, potrebbe essere necessario modificare questo numero impostando un valore più alto. In questo modo i numeri di messaggio convertiti non sono in conflitto con i numeri dei messaggi esistenti.

Tenere presente quanto segue:
  
- I messaggi dell'ambiente del servizio app nell'intervallo `17000` - `19999` sono dalla `sysmessages` tabella di sistema e non vengono convertiti.
- Se il numero del messaggio a cui viene fatto riferimento nell' `RAISERROR` istruzione è una costante, SSMA aggiungerà il numero del messaggio di base alla costante per determinare il numero del nuovo messaggio utente.
- Se il numero del messaggio a cui viene fatto riferimento è una variabile o un'espressione, SSMA creerà una variabile locale intermedia.
- In **modalità ottimistica**, SSMA presuppone che l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opzione `CONCAT_NULL_YIELDS_NULL` sia `OFF` e non esegua controlli per gli `NULL` argomenti.
- In **modalità completa**, SSMA controlla gli `NULL` argomenti.
- `RAISERROR` con l' `arg-list` argomento non viene convertito.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|30001|
|Optimistic|30001|
|Full|30001|

### <a name="system-objects"></a>Oggetti di sistema

Usare questa impostazione per specificare il tipo di messaggio (avviso o errore) visualizzato da SSMA nel riquadro **output** o **Elenco errori** quando rileva l'uso degli oggetti di sistema dell'ambiente del servizio app.

- Se si seleziona **Convert e Mark con Warning**, SSMA convertirà i riferimenti in oggetti di sistema e contrassegnerà le istruzioni con commenti di avviso.
- Se si seleziona **contrassegna con errore**, SSMA non convertirà i riferimenti in oggetti di sistema e contrassegnerà le istruzioni con commenti di errore.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Converti e contrassegno con avviso|
|Optimistic|Converti e contrassegno con avviso|
|Full|Contrassegno con errore|

### <a name="unresolved-identifiers"></a>Identificatori non risolti

Usare questa impostazione per specificare il tipo di messaggio (avviso o errore) visualizzato da SSMA nel riquadro **output** o **Elenco errori** quando non è possibile risolvere un identificatore.

- Se si seleziona **Convert e Mark con Warning**, SSMA tenterà di convertire i riferimenti in identificatori non risolti e contrassegnerà le istruzioni con commenti di avviso.
- Se si seleziona **contrassegno con errore**, SSMA non convertirà i riferimenti in identificatori non risolti e contrassegnerà le istruzioni con commenti di errore.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Converti e contrassegno con avviso|
|Optimistic|Converti e contrassegno con avviso|
|Full|Contrassegno con errore|

## <a name="system-functions-section"></a>Sezione funzioni di sistema

### <a name="charindex-function"></a>CHARINDEX, funzione

In ASE `CHARINDEX` restituisce `NULL` solo se tutte le espressioni di input sono `NULL` . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL restituirà `NULL` se un'espressione di input è `NULL` .

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Sostituisci funzione**. Tutte le chiamate a `CHARINDEX` Function vengono sostituite con una chiamata a `CHARINDEX_VARCHAR` o a una  `CHARINDEX_NVARCHAR` funzione definita dall'utente in base al tipo di parametri passati (creati nel database utente sotto il nome dello schema `s2ss` ) per emulare il comportamento di SAP ASE.
- Per usare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamento SQL di/Azure, selezionare **Mantieni sintassi corrente**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Replace (funzione)|
  
### <a name="datalength-function"></a>DATALENGTH - funzione

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL e ambiente del servizio app differiscono nel valore restituito dalla `DATALENGTH` funzione quando il valore è uno spazio singolo. In questo caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL restituisce e l'ambiente del servizio app `0` restituisce `1` .

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Sostituisci funzione**. Tutte le chiamate a `DATALENGTH` Function vengono sostituite con `CASE` Expression per emulare il comportamento di SAP ASE.
- Per usare il comportamento predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, selezionare **Mantieni sintassi corrente**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Replace (funzione)|

### <a name="index_col-function"></a>INDEX_COL - funzione

L'ambiente del servizio app supporta un `user_id` argomento facoltativo per la `INDEX_COL` funzione, ma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL non supporta questo argomento. Se si usa l' `user_id` argomento, questa funzione non può essere convertita in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi SQL/Azure.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Converti funzione**. Se il codice contiene l' `user_id` argomento, SSMA visualizzerà un errore.
- Per visualizzare un messaggio di errore ogni volta che `INDEX_COL` viene rilevato, selezionare **contrassegna con errore**. SSMA non convertirà i riferimenti alla funzione e contrassegnerà l'istruzione con i commenti degli errori.

|Modalità|Valore|
|-|-|
|Predefinito|Contrassegno con errore|
|Optimistic|Contrassegno con errore|
|Full|Contrassegno con errore|

### <a name="index_colorder-function"></a>Funzione INDEX_COLORDER

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL non dispone di una `INDEX_COLORDER` funzione di sistema.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Converti funzione**. Tutte le chiamate a `INDEX_COLORDER` Function vengono sostituite con una chiamata a una funzione definita dall'utente con lo stesso nome `INDEX_COLORDER` , creato nel database utente con il nome dello schema `s2ss` , che emula il comportamento di SAP ASE.
- Per stampare un messaggio di errore ogni volta che `INDEX_COLORDER` viene rilevato, selezionare **contrassegna con errore**. SSMA non convertirà i riferimenti alla funzione e contrassegnerà l'istruzione con i commenti degli errori.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Contrassegno con errore|
|Optimistic|Contrassegno con errore|
|Full|Contrassegno con errore|

### <a name="left-and-right-functions"></a>Funzioni LEFT e RIGHT

`LEFT` le `RIGHT` funzioni e nell'ambiente del servizio app funzionano in modo diverso per il parametro di lunghezza negativa

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Sostituisci funzione**. Il parametro length viene quindi sostituito con `CASE` Expression che restituirà `NULL` per un valore negativo.
- Per utilizzare il comportamento di SQL Server, selezionare **Mantieni sintassi corrente**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Replace (funzione)|

> [!NOTE]
> Se il parametro length è un valore letterale e non un'espressione complessa, il valore length viene sempre sostituito con `NULL` indipendentemente dall'impostazione del progetto.

### <a name="next_identity-function"></a>Funzione NEXT_IDENTITY

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL non dispone di una `NEXT_IDENTITY` funzione di sistema.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Converti funzione**. Tutte le chiamate a `NEXT_IDENTITY` Function vengono sostituite con un'espressione `(IDENT_CURRENT(parameter Value) + IDENT_INCR(parameter Value)` che emula il comportamento di SAP ASE.
- Per stampare un messaggio di errore ogni volta che `NEXT_IDENTITY` viene rilevato, selezionare **contrassegna con errore**. SSMA non convertirà i riferimenti alla funzione e contrassegnerà l'istruzione con i commenti degli errori.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Contrassegno con errore|
|Optimistic|Contrassegno con errore|
|Full|Contrassegno con errore|

**Modalità predefinita/ottimistica/completa:** Contrassegno con errore

### <a name="patindex-function"></a>PATINDEX - funzione

Specifica se convertire la `PATINDEX` funzione in modo che corrisponda al comportamento di SAP ASE. Il punto è che l'ambiente del servizio app rimuove gli spazi vuoti finali in un criterio di ricerca. La soluzione alternativa consiste nell'eseguire un cast dell'espressione valore a un tipo di dati a lunghezza fissa con una precisione massima e applicare la `rtrim` funzione ai criteri di ricerca.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **use**.  
- Per usare il comportamento predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, selezionare **non usare**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Non utilizzare|
|Optimistic|Non utilizzare|
|Full|Uso|

### <a name="replicate-function"></a>REPLICATE - funzione

La `REPLICATE` funzione ripete una stringa per il numero di volte specificato. Nell'ambiente del servizio app, se si specifica di ripetere la stringa zero volte, il risultato è `NULL` . In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL il risultato è una stringa vuota.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Sostituisci funzione**. Tutte le chiamate a `REPLICATE` Function vengono sostituite con una chiamata a `REPLICATE_VARCHAR` o a una `REPLICATE_NVARCHAR` funzione definita dall'utente in base al tipo di parametri passati (creati nel database utente sotto il nome dello schema `s2ss` ) per emulare il comportamento di SAP ASE.
- Per usare il comportamento predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL/Azure, selezionare **Sostituisci funzione**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Replace (funzione)|
|Optimistic|Replace (funzione)|
|Full|Replace (funzione)|

### <a name="trim-ltrim-rtrim-function"></a>Funzione TRIM (LTRIM, RTRIM)

Questa impostazione specifica se sostituire le chiamate a `TRIM` `LTRIM` e `RTRIM` funzioni con le funzioni di sintassi di SAP ASE equivalenti oppure per mantenendo la sintassi corrente. Per questa particolare impostazione sono presenti le opzioni seguenti:

- **Replace (funzione)**
- **Mantieni sintassi corrente**

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Replace (funzione)|
|Optimistic|Replace (funzione)|
|Full|Replace (funzione)|

### <a name="substring-function"></a>SUBSTRING - funzione

Nell'ambiente del servizio app, la funzione `SUBSTRING(expression, start, length)` restituisce `NULL` se viene specificato un valore iniziale maggiore del numero di caratteri nell'espressione o se la lunghezza è uguale a zero. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL l'espressione equivalente restituisce una stringa vuota.

- Per usare il comportamento dell'ambiente del servizio app, selezionare **Sostituisci funzione**. Tutte le chiamate a `SUBSTRING` Function vengono sostituite con una chiamata a `SUBSTRING_VARCHAR` o o a una `SUBSTRING_NVARCHAR` `SUBSTRING_VARBINARY` funzione definita dall'utente in base al tipo di parametri passati (creati nel database utente sotto il nome dello schema `s2ss` ) per EMULARE il comportamento di SAP ASE.
- Per usare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamento SQL di/Azure, selezionare **Mantieni sintassi corrente**.

Quando si seleziona una modalità di conversione nella casella **modalità** , SSMA applica l'impostazione seguente:

|Modalità|Valore|
|-|-|
|Predefinito|Mantieni sintassi corrente|
|Optimistic|Mantieni sintassi corrente|
|Full|Replace (funzione)|

## <a name="tables-section"></a>Sezione Tables

### <a name="add-primary-key"></a>Aggiungi chiave primaria

Crea una nuova chiave primaria nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella o SQL di Azure se una tabella di SAP ASE non ha una chiave primaria o un indice univoco.

|Modalità|Valore|
|-|-|
|Predefinito|No|
|Optimistic|No|
|Full|Sì|

> [!NOTE]
> Quando si è connessi a SQL di Azure, è **Sì** per impostazione predefinita.

## <a name="see-also"></a>Vedere anche

[Guida di riferimento all'interfaccia utente (SybaseToSQL)](../../ssma/sybase/user-interface-reference-sybasetosql.md)
