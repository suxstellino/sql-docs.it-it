---
description: DECLARE CURSOR (Transact-SQL)
title: DECLARE CURSOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5fcd8a9ca3239d368b25f7444c11c311e4b1d306
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102485"
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Definisce gli attributi di un cursore del server [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio lo scorrimento e la query utilizzata per compilare il set di risultati su cui agisce il cursore. L'istruzione `DECLARE CURSOR` supporta sia la sintassi basata sullo standard ISO che una sintassi che usi un set di estensioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *cursor_name*  
 Nome del cursore server [!INCLUDE[tsql](../../includes/tsql-md.md)] definito. *cursor_name* deve essere conforme alle regole per gli identificatori.  
  
 INSENSITIVE  
 Definisce un cursore che crea una copia temporanea dei dati utilizzati dal cursore. La risposta alle richieste indirizzate al cursore viene formulata tramite questa tabella temporanea in **tempdb**. Le modifiche alle tabelle di base non vengono pertanto riportate nei dati restituiti dalle operazioni di recupero eseguite in questo cursore, in cui non è consentito apportare modifiche. Nella sintassi ISO, se viene omessa la parola chiave `INSENSITIVE`, le operazioni di eliminazione e aggiornamento di cui è stato eseguito il commit nelle tabelle sottostanti da parte di qualsiasi utente si riflettono nelle operazioni di recupero successive.  
  
 SCROLL  
 Specifica che tutte le opzioni di recupero (`FIRST`, `LAST`, `PRIOR`, `NEXT`, `RELATIVE`, `ABSOLUTE`) sono disponibili. Se l'opzione `SCROLL` non viene specificata in un'istruzione `DECLARE CURSOR` ISO, l'unica opzione di recupero supportata è `NEXT`. Non è possibile specificare l'opzione `SCROLL` se è specificata l'opzione `FAST_FORWARD`. Se `SCROLL` non è specificato, è disponibile solo l'opzione di recupero `NEXT` e il cursore diventa `FORWARD_ONLY`.
  
 *select_statement*  
 Istruzione `SELECT` standard che definisce il set di risultati del cursore. Le parole chiave `FOR BROWSE` e `INTO` non sono consentite all'interno dell'argomento *select_statement* della dichiarazione di un cursore.  
  
 Se le clausole nell'argomento *select_statement* sono in conflitto con la funzionalità del tipo di cursore richiesto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] converte il cursore in modo implicito in un altro tipo.  
  
 READ ONLY  
 Impedisce gli aggiornamenti eseguiti tramite il cursore. Non è possibile fare riferimento al cursore in una clausola `WHERE CURRENT OF` di un'istruzione `UPDATE` o `DELETE`. Questa opzione è prioritaria rispetto alla funzionalità di aggiornamento predefinita di un cursore.  
  
 UPDATE [OF *column_name* [**,**...*n*]]  
 Definisce le colonne aggiornabili nel cursore. Se è specificato l'argomento OF <column_name> [, <... n>], è possibile apportare modifiche solo nelle colonne elencate. Se si specifica `UPDATE` senza un elenco di colonne, è possibile aggiornare tutte le colonne.  
  
*cursor_name*  
Nome del cursore server [!INCLUDE[tsql](../../includes/tsql-md.md)] definito. *cursor_name* deve essere conforme alle regole per gli identificatori.  
  
LOCAL  
Specifica che l'ambito del cursore è locale rispetto al batch, alla stored procedure o al trigger in cui il cursore è stato creato. Il nome del cursore è valido solo in tale ambito. È possibile fare riferimento al cursore tramite variabili di cursore locali nel batch, nella stored procedure o nel trigger oppure tramite un parametro `OUTPUT` di stored procedure. Un parametro `OUTPUT` consente di passare il cursore locale al batch, alla stored procedure o al trigger chiamante, che può quindi assegnare il parametro a una variabile cursore per fare riferimento al cursore dopo l'esecuzione della stored procedure. Il cursore viene deallocato in modo implicito al termine del batch, della stored procedure o del trigger, a meno che non venga passato a un parametro `OUTPUT`. In tal caso, il cursore viene deallocato quando l'ultima variabile che vi fa riferimento viene deallocata o risulta esterna all'ambito di validità.  
  
GLOBAL  
Specifica che l'ambito del cursore è globale rispetto alla connessione. È possibile fare riferimento al nome del cursore in qualsiasi stored procedure o batch eseguiti tramite la connessione. Il cursore viene deallocato solo in modo implicito in fase di disconnessione.  
  
> [!NOTE]  
>  Se gli argomenti `GLOBAL` e `LOCAL` vengono entrambi omessi, il valore predefinito dipende dall'impostazione dell'opzione di database **default to local cursor**.  
  
FORWARD_ONLY  
Specifica che il cursore può solo spostarsi in avanti e scorrere dalla prima all'ultima riga. `FETCH NEXT` è l'unica opzione di recupero supportata. Tutte le istruzioni INSERT, UPDATE e DELETE eseguite dall'utente corrente (o di cui è stato eseguito il commit da altri utenti) che interessano le righe del set di risultati sono visibili nel momento in cui le righe vengono recuperate. Poiché lo scorrimento all'indietro del cursore non è consentito, tuttavia, le modifiche apportate alle righe nel database dopo il recupero delle righe stesse non sono visibili tramite il cursore. I cursori forward-only sono dinamici per impostazione predefinita, vale a dire che tutte le modifiche vengono rilevate durante l'elaborazione della riga corrente. Questo consente maggiore rapidità di apertura del cursore e la visualizzazione nel set di risultati degli aggiornamenti apportati alle tabelle sottostanti. I cursori forward-only non supportano lo scorrimento all'indietro, ma le applicazioni possono tornare all'inizio del set di risultati chiudendo e riaprendo il cursore. Se si specifica l'opzione `FORWARD_ONLY` senza alcuna delle parole chiave `STATIC`, `KEYSET` o `DYNAMIC`, il cursore fungerà da cursore dinamico. Se si omettono sia `FORWARD_ONLY` che `SCROLL`, l'impostazione predefinita è `FORWARD_ONLY`, a meno che non sia specificata la parola chiave `STATIC`, `KEYSET` o `DYNAMIC`. L'impostazione predefinita dei cursori `STATIC`, `KEYSET` e `DYNAMIC` è `SCROLL`. A differenza delle API di database, ad esempio ODBC e ADO, l'opzione `FORWARD_ONLY` è supportata con i cursori `STATIC`, `KEYSET` e `DYNAMIC` [!INCLUDE[tsql](../../includes/tsql-md.md)].  
   
 STATIC  
Specifica che il cursore deve sempre visualizzare il set di risultati com'era quando il cursore è stato aperto la prima volta e creare una copia temporanea dei dati che deve usare. Tutte le richieste al cursore ricevono risposta da questa tabella temporanea in **tempdb**. Di conseguenza gli inserimenti, gli aggiornamenti e le eliminazioni eseguiti nelle tabelle di base non si riflettono nei dati restituiti dalle operazioni di recupero eseguite in questo cursore e le modifiche apportate all'appartenenza, all'ordine o ai valori del set di risultati dopo l'apertura del cursore non vengono rilevate da quest'ultimo. I cursori statici possono rilevare le proprie operazioni di aggiornamento, eliminazione e inserimento, anche se tale rilevamento non è obbligatorio per cursori di questo tipo. Si supponga, ad esempio, che un cursore statico recuperi una riga e che un'altra applicazione aggiorni in seguito tale riga. Se l'applicazione recupera nuovamente la riga dal cursore statico, i valori che rileva sono invariati, nonostante le modifiche apportate dall'altra applicazione. Sono supportati tutti i tipi di scorrimento. 
  
KEYSET  
Specifica che all'apertura del cursore l'appartenenza e l'ordine delle righe nel cursore sono fissi. Il set di chiavi che identifica le righe in modo univoco è incluso in una tabella di **tempdb** denominata **keyset**. Questo cursore offre funzionalità intermedie di cursore statico e cursore dinamico, per quanto riguarda la facoltà di quest'ultimo di rilevare le modifiche. Come un cursore statico, non sempre rileva le modifiche all'appartenenza e all'ordine del set di risultati. Come un cursore dinamico, rileva le modifiche ai valori delle righe nel set di risultati. I cursori gestiti da keyset vengono controllati da un set di identificatori univoci (chiavi), noti come keyset. Le chiavi sono costituite da un set di colonne che identificano in modo univoco le righe del set di risultati. Il keyset corrisponde al set di valori di chiave di tutte le righe restituite dall'istruzione della query. Con i cursori gestiti da keyset viene generata e salvata una chiave per ogni riga nel cursore e tale chiave viene archiviata nella workstation client o nel server. Quando si accede a ogni riga, la chiave archiviata viene usata per recuperare i valori correnti dei dati dall'origine dati. In un cursore gestito da keyset, quando il set di chiavi è completamente popolato, l'appartenenza del set di risultati è bloccata. Le aggiunte o gli aggiornamenti successivi che interessano l'appartenenza faranno parte del set di risultati solo quando verrà riaperto. Le modifiche ai valori dei dati, apportate dal proprietario del keyset o da altri processi, sono visibili mentre l'utente scorre il set di risultati:
-  Se una riga viene eliminata, un tentativo di recuperarla restituisce un valore di `@@FETCH_STATUS` pari a -2, poiché la riga eliminata viene visualizzata come gap nel set di risultati. La chiave per la riga è presente nel keyset, ma la riga non esiste più nel set di risultati. 
-  Gli inserimenti eseguiti all'esterno del cursore da altri processi sono visibili solo se il cursore viene chiuso e riaperto. Gli inserimenti eseguiti dall'interno del cursore sono visibili alla fine del set di risultati.
-  Le operazioni di aggiornamento di valori di chiave dall'esterno del cursore sono simili a un'operazione di eliminazione della riga precedente seguita da un'operazione di inserimento della nuova riga. La riga con i nuovi valori non è visibile e i tentativi di recuperare la riga con i valori precedenti restituiranno il valore -2 per la funzione `@@FETCH_STATUS`. I nuovi valori sono visibili se l'aggiornamento viene effettuato tramite il cursore con l'aggiunta della clausola `WHERE CURRENT OF`. 

> [!NOTE]  
> Se la query fa riferimento ad almeno una tabella priva di indice univoco, il cursore keyset viene convertito in cursore statico.  
  
DYNAMIC  
Definisce un cursore che riflette tutte le modifiche apportate ai dati delle righe nel relativo set di risultati quando l'utente scorre il cursore e recupera un nuovo record, indipendentemente dal fatto che le modifiche vengano eseguite dall'interno del cursore o da altri utenti all'esterno del cursore stesso. Di conseguenza tutte le istruzioni di inserimento, aggiornamento ed eliminazione eseguite da tutti gli utenti sono visibili tramite il cursore. I valori dei dati, l'ordine e l'appartenenza delle righe possono cambiare a ogni operazione di recupero. L'opzione di recupero `ABSOLUTE` non è supportata con i cursori dinamici. Gli aggiornamenti eseguiti all'esterno del cursore risultano visibili solo dopo l'operazione di commit, a meno che il livello di isolamento della transazione non sia impostato su `UNCOMMITTED`. Si supponga, ad esempio, che un cursore dinamico recuperi due righe e un'altra applicazione aggiorni in seguito una di queste righe ed elimini l'altra. Se a questo punto il cursore dinamico recupera tali righe, non troverà la riga eliminata, ma visualizzerà i nuovi valori per la riga aggiornata. 
  
FAST_FORWARD  
Specifica un cursore `FORWARD_ONLY`, `READ_ONLY` con le ottimizzazioni delle prestazioni abilitate. Non è possibile specificare l'opzione `FAST_FORWARD` se è specificata l'opzione `SCROLL` o `FOR_UPDATE`. Questo tipo di cursore non consente modifiche ai dati dall'interno del cursore.  
  
> [!NOTE]  
> Le opzioni `FAST_FORWARD` e `FORWARD_ONLY` possono essere usate nella stessa istruzione `DECLARE CURSOR`.  
  
READ_ONLY  
Impedisce gli aggiornamenti eseguiti tramite il cursore. Non è possibile fare riferimento al cursore in una clausola `WHERE CURRENT OF` di un'istruzione `UPDATE` o `DELETE`. Questa opzione è prioritaria rispetto alla funzionalità di aggiornamento predefinita di un cursore.  
  
SCROLL_LOCKS  
Specifica che gli aggiornamenti o le eliminazioni posizionate eseguite tramite il cursore avranno esito positivo. Durante la lettura nel cursore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] blocca le righe in modo che siano disponibili per modifiche successive. Non è possibile specificare l'opzione `SCROLL_LOCKS` se è specificata l'opzione `FAST_FORWARD` o `STATIC`.  
  
OPTIMISTIC  
Specifica che gli aggiornamenti o le eliminazioni posizionate eseguite tramite il cursore non avranno esito positivo se la riga ha subito un aggiornamento dopo la lettura nel cursore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non blocca le righe man mano che vengono lette nel cursore. Per determinare se la riga è stata modificata dopo la lettura nel cursore, vengono usati confronti tra i valori della colonna **timestamp** oppure un valore di checksum se la tabella non include una colonna **timestamp**. Se la riga è stata modificata, i tentativi di eseguire un aggiornamento o un'eliminazione posizionata hanno esito negativo. Non è possibile specificare l'opzione `OPTIMISTIC` se è specificata l'opzione `FAST_FORWARD`.  
  
 TYPE_WARNING  
 Specifica che deve essere inviato un messaggio di avviso al client quando il cursore viene convertito in modo implicito dal tipo richiesto in un altro tipo.  
  
 *select_statement*  
 Istruzione SELECT standard che definisce il set di risultati del cursore. Le parole chiave `COMPUTE`, `COMPUTE BY`, `FOR BROWSE` e `INTO` non sono consentite all'interno dell'argomento *select_statement* della dichiarazione di un cursore.  
  
> [!NOTE]  
> Nella dichiarazione di un cursore è possibile usare un hint per la query. Se tuttavia si usa anche la clausola `FOR UPDATE OF`, specificare `OPTION (<query_hint>)` subito dopo.  
  
Se le clausole nell'argomento *select_statement* sono in conflitto con la funzionalità del tipo di cursore richiesto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] converte il cursore in modo implicito in un altro tipo. Per ulteriori informazioni, vedere l'argomento relativo alla conversione implicita dei cursori.  
  
FOR UPDATE [OF *column_name* [**,**...*n*]]  
Definisce le colonne aggiornabili nel cursore. Se si specifica `OF <column_name> [, <... n>]`, è possibile apportare modifiche solo nelle colonne elencate. Se l'istruzione `UPDATE` viene specificata senza un elenco di colonne, è possibile aggiornare tutte le colonne, a meno che non sia stata specificata l'opzione di concorrenza `READ_ONLY`.  
  
## <a name="remarks"></a>Commenti  
`DECLARE CURSOR` definisce gli attributi di un cursore server [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio il comportamento dello scorrimento e la query usata per compilare il set di risultati su cui il cursore opera. L'istruzione `OPEN` popola il set di risultati e l'istruzione `FETCH` restituisce una riga di questo set. L'istruzione `CLOSE` rilascia il set di risultati corrente associato al cursore. L'istruzione `DEALLOCATE` rilascia le risorse usate dal cursore.  
  
La prima forma dell'istruzione `DECLARE CURSOR` dichiara il funzionamento del cursore tramite la sintassi ISO. La seconda forma dell'istruzione `DECLARE CURSOR` usa estensioni di [!INCLUDE[tsql](../../includes/tsql-md.md)] che consentono di definire cursori in base allo stesso tipo di cursore usato nelle funzioni di cursore delle API di database ODBC o ADO.  
  
Non è possibile combinare le due forme dell'istruzione. Se si specifica la parola chiave `SCROLL` o `INSENSITIVE` prima della parola chiave `CURSOR`, non è possibile specificare parole chiave tra le parole chiave `CURSOR` e `FOR <select_statement>`. Se si specifica una parola chiave tra le parole chiave `CURSOR` e `FOR <select_statement>`, non è possibile specificare `SCROLL` o `INSENSITIVE` prima della parola chiave `CURSOR`.  
  
Se un'istruzione `DECLARE CURSOR` che usa la sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] non specifica `READ_ONLY`, `OPTIMISTIC` o `SCROLL_LOCKS`, il valore predefinito viene stabilito come segue:  
  
-   Se l'istruzione `SELECT` non supporta aggiornamenti (autorizzazioni insufficienti, accesso a tabelle remote che non supportano aggiornamenti e così via), il cursore viene impostato come `READ_ONLY`.  
  
-   L'impostazione predefinita dei cursori `STATIC` e `FAST_FORWARD` è `READ_ONLY`.  
  
-   L'impostazione predefinita dei cursori `DYNAMIC` e `KEYSET` è `OPTIMISTIC`.  
  
È possibile fare riferimento a nomi di cursore solo tramite altre istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Non è possibile fare riferimento ai nomi di cursore con funzioni API del database. Dopo la dichiarazione del cursore, ad esempio, non è possibile fare riferimento al nome del cursore tramite le funzioni o i metodi di OLE DB, ODBC o ADO. Le righe del cursore non possono essere recuperate tramite le funzioni o i metodi di recupero API, ma solo utilizzando istruzioni FETCH [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Dopo la dichiarazione di un cursore è possibile eseguire le stored procedure di sistema seguenti per determinare le caratteristiche del cursore.  
  
|Stored procedure di sistema|Descrizione|  
|------------------------------|-----------------|  
|**sp_cursor_list**|Restituisce l'elenco dei cursori visibili nella connessione e gli attributi corrispondenti.|  
|**sp_describe_cursor**|Descrive gli attributi di un cursore, ad esempio se si tratta di un cursore forward-only o scorrevole.|  
|**sp_describe_cursor_columns**|Descrive gli attributi delle colonne nel set di risultati del cursore.|  
|**sp_describe_cursor_tables**|Descrive le tabelle di base a cui ha avuto accesso il cursore.|  
  
 È possibile usare variabili come parte dell'istruzione *select_statement* che dichiara un cursore. Dopo la dichiarazione di un cursore i valori delle variabili di cursore non cambiano.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per l'istruzione `DECLARE CURSOR` vengono assegnate per impostazione predefinita a qualsiasi utente che disponga delle autorizzazioni per l'istruzione `SELECT` nelle viste, nelle tabelle e nelle colonne usate nel cursore.
 
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni

Non è possibile usare cursori o trigger in una tabella con un indice columnstore cluster. Questa restrizione non si applica agli indici columnstore non cluster. In una tabella con un indice columnstore non cluster è possibile usare cursori e trigger. 
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-simple-cursor-and-syntax"></a>R. Utilizzo di una semplice sintassi per la definizione di un cursore  

Il set di risultati generato all'apertura del cursore include tutte le righe e le colonne della tabella. Questo cursore può essere aggiornato e tutti gli aggiornamenti e le eliminazioni sono rappresentati nei recuperi eseguiti su questo cursore. `FETCH NEXT` è l'unica operazione di recupero disponibile perché l'opzione `SCROLL` non è stata specificata.  
 
```sql  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. Utilizzo di cursori annidati per la generazione di report  
 Nell'esempio seguente viene illustrato in che modo è possibile nidificare i cursori per generare report complessi. Il cursore interno viene dichiarato per ogni fornitore.  
  
```sql  
SET NOCOUNT ON;  
  
DECLARE @vendor_id INT, @vendor_name NVARCHAR(50),  
    @message VARCHAR(80), @product NVARCHAR(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Cursori &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
