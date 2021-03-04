---
title: Attacco SQL injection | Microsoft Docs
description: Informazioni sul funzionamento degli attacchi SQL injection. Attenuare questo tipo di attacchi convalidando l'input ed esaminando il codice per SQL injection in SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: security, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074033472f37787c0bcf1da6f261a5f835f0cbec
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838977"
---
# <a name="sql-injection"></a>Attacchi SQL injection
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In un attacco SQL injection, il malware viene inserito in stringhe che vengono successivamente passate a un'istanza di SQL Server per l'analisi e l'esecuzione. Per la prevenzione degli attacchi di questo tipo è necessario esaminare tutte le procedure che creano istruzioni SQL perché SQL Server esegue tutte le query sintatticamente valide che riceve. Anche i dati con parametri possono essere modificati da un utente malintenzionato abile e determinato.  
  
## <a name="how-sql-injection-works"></a>Come funziona un attacco SQL injection  
 La forma principale di un attacco intrusivo nel codice SQL consiste nell'inserimento diretto di codice in variabili di input utente concatenate a comandi SQL ed eseguite. Una forma meno diretta di attacco consiste nell'inserimento di malware in stringhe destinate all'archiviazione in una tabella o come metadati. Quando le stringhe archiviate vengono successivamente concatenate in un comando SQL dinamico, il codice dannoso viene eseguito.  
  
 Il processo di intrusione termina prematuramente una stringa di testo e aggiunge un nuovo comando. Poiché prima che venga eseguito è possibile che al comando inserito vengano aggiunte ulteriori stringhe, l'utente malintenzionato termina la stringa inserita con un contrassegno di commento "--". Al momento del'esecuzione, il testo successivo al segno di commento viene ignorato.  
  
 Nello script seguente viene illustrata una semplice intrusione nel codice SQL. Nello script viene compilata una query SQL tramite concatenazione di stringhe hardcoded a una stringa immessa dall'utente:  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 L'utente riceve la richiesta di immettere il nome di una città. Se l'utente immette `Redmond`, la query assemblata dallo script sarà simile alla seguente:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 Si supponga, tuttavia, che l'utente immetta la stringa seguente:  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 In questo caso, la query assemblata dallo script sarà la seguente:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 Il punto e virgola (;) indica la fine di una query e l'inizio di un'altra. Il doppio trattino (--) indica che il resto della riga corrente è un commento che deve essere ignorato. Se il codice modificato è sintatticamente corretto, verrà eseguito dal server. Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elabora questa istruzione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seleziona prima tutti i record in `OrdersTable` in cui `ShipCity` è `Redmond`, quindi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimuove `OrdersTable`.  
  
 Se il codice SQL inserito è sintatticamente corretto, le manomissioni non possono essere rilevate a livello di programmazione. È pertanto necessario convalidare tutti gli input utente e rivedere attentamente il codice per l'esecuzione dei comandi SQL generati nel server in uso. Nelle sezioni seguenti di questo argomento vengono illustrate le procedure consigliate relative al codice.  
  
## <a name="validate-all-input"></a>Convalidare tutti gli input  
 Convalidare sempre gli input utente testando tipo, lunghezza, formato e intervallo. Durante l'implementazione di precauzioni contro input dannosi, valutare gli scenari di distribuzione e l'architettura dell'applicazione in uso. È importante ricordare che i programmi destinati all'esecuzione in un ambiente sicuro possono essere copiati in un ambiente non sicuro. I suggerimenti seguenti vanno considerati procedure consigliate:  
  
-   Non basarsi su presupposti relativi a dimensioni, tipo o contenuto dei dati ricevuti dall'applicazione. È ad esempio possibile valutare:  
  
    -   Il comportamento dell'applicazione se un utente immette volontariamente o per errore un file MPEG da 10 MB nel punto in cui è previsto un codice postale.  
  
    -   Il comportamento dell'applicazione se un'istruzione `DROP TABLE` viene incorporata in un campo di testo.  
  
-   Testare le dimensioni e il tipo di dati dell'input e imporre limiti appropriati. In questo modo è possibile impedire intenzionali sovraccarichi del buffer.  
  
-   Testare il contenuto delle variabili stringa e accettare solo i valori previsti. Rifiutare voci contenenti dati binari, sequenze di escape e caratteri di commento. Ciò consente di evitare l'attacco intrusivo nel codice script e può fornire protezione da alcuni exploit basati sul sovraccarico del buffer.  
  
-   Quando si utilizzano documenti XML, convalidare tutti i dati in base al relativo schema man mano che vengono immessi.  
  
-   Non compilare mai istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] direttamente dall'input utente.  
  
-   Utilizzare stored procedure per la convalida dell'input utente.  
  
-   In ambienti multilivello è necessario convalidare tutti i dati prima di consentirne l'inserimento in aree considerate attendibili. I dati che non superano il processo di convalida devono essere rifiutati e un errore deve essere restituito al livello precedente.  
  
-   Implementare più livelli di convalida. Le precauzioni possono risultare inefficaci nella difesa da utenti malintenzionati determinati. Una procedura consigliata consiste nel convalidare l'input nell'interfaccia utente e in tutti i successivi punti in cui l'input oltrepassa il limite di un'area attendibile.   
    Ad esempio, la convalida dei dati in un'applicazione sul lato client può impedire i semplici attacchi intrusivi nel codice script. Se, tuttavia, al livello successivo si presuppone che il relativo input sia già stato convalidato, l'utente malintenzionato in grado di ignorare un client potrà accedere in modo illimitato a un sistema.  
  
-   Non eseguire mai la concatenazione di input utente non convalidato. La concatenazione delle stringhe è il punto di ingresso principale per l'attacco intrusivo nel codice script.  
  
-   Non accettare le stringhe seguenti nei campi da cui è possibile costruire i nomi di file: AUX, CLOCK$, da COM1 a COM8, CON, CONFIG$, da LPT1 a LPT8, NUL e PRN.  
  
 Quando possibile, rifiutare l'input contenente i caratteri seguenti.  
  
|Carattere di input|Significato in Transact-SQL|  
|---------------------|------------------------------|  
|**;**|Delimitatore di query|  
|**'**|Delimitatore di stringhe di dati di tipo carattere|  
|**--**|Delimitatore di commento a riga singola. Il testo **--** che segue fino alla fine della riga non viene valutato dal server.|  
|**/\**_ ... _*\*/**|Delimitatori di commento. Il testo compreso fra **/\* *_ e _* \*/** non viene valutato dal server.|  
|**xp_**|Usato all'inizio del nome delle stored procedure estese di catalogo come `xp_cmdshell`.|  
  
### <a name="use-type-safe-sql-parameters"></a>Utilizzo di parametri SQL type safe  
 La raccolta **Parameters** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa il controllo del tipo e la convalida della lunghezza. Se si usa la raccolta **Parameters** , l'input viene interpretato come valore letterale e non come codice eseguibile. Un ulteriore vantaggio dell'utilizzo della raccolta **Parameters** consiste nella possibilità di applicare controlli sul tipo e sulla lunghezza. I valori non compresi nell'intervallo generano un'eccezione. Nel frammento di codice seguente viene illustrato l'utilizzo della raccolta **Parameters** :  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 In questo esempio il parametro `@au_id` viene interpretato come valore letterale e non come codice eseguibile. Vengono controllati il tipo e la lunghezza del valore. Se il valore di `@au_id` non è conforme ai vincoli di tipo e lunghezza specificati, viene generata un'eccezione.  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>Utilizzo di input con parametri nelle stored procedure  
 Le stored procedure sono vulnerabili all'attacco intrusivo nel codice SQL se utilizzano input non filtrato. Ad esempio, il codice seguente è vulnerabile:  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 Se si utilizzano stored procedure è opportuno utilizzare come input solo parametri.  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>Utilizzo dell'insieme Parameters nelle istruzioni SQL dinamiche  
 Se non si possono usare stored procedure, è comunque possibile usare parametri, come mostrato nell'esempio di codice seguente:  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>Filtraggio dell'input  
 È anche possibile filtrare l'input per proteggersi da attacchi intrusivi nel codice SQL tramite la rimozione di caratteri di escape. Dato il numero elevato di caratteri che potrebbero causare problemi, non è tuttavia possibile considerare affidabile questo metodo di difesa. Nell'esempio seguente viene eseguita la ricerca del delimitatore di stringhe di caratteri.  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>Clausole LIKE  
 Si noti che se si usa una clausola `LIKE` sarà comunque necessario usare caratteri di escape per i caratteri jolly:  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>Revisione del codice per attacchi intrusivi nel codice SQL  
 È necessario rivedere tutto il codice che chiama `EXECUTE`, `EXEC`o `sp_executesql`. È possibile utilizzare query simili alla seguente per identificare procedure contenenti queste istruzioni. Questa query verifica la presenza di 1, 2, 3 o 4 spazi dopo le parole `EXECUTE` o `EXEC`.  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>Wrapping di parametri con QUOTENAME() e REPLACE()  
 In ogni stored procedure selezionata verificare che tutte le variabili usate in Transact-SQL dinamico vengano gestite correttamente. Il wrapping dei dati provenienti dai parametri di input della stored procedure o letti da una tabella deve essere eseguito in QUOTENAME() o REPLACE(). Ricordare che il valore di @variable passato a QUOTENAME() è di tipo sysname e presenta una lunghezza massima di 128 caratteri.  
  
|Variabile \@|Wrapper consigliato|  
|---------------|-------------------------|  
|Nome di un'entità a sicurezza diretta|`QUOTENAME(@variable)`|  
|Stringa di ≤ 128 caratteri|`QUOTENAME(@variable, '''')`|  
|Stringa di > 128 caratteri|`REPLACE(@variable,'''', '''''')`|  
  
 Quando si utilizza questa tecnica, è possibile rivedere un'istruzione SET nel modo seguente:  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>Attacco intrusivo consentito dal troncamento dei dati  
 Qualsiasi [!INCLUDE[tsql](../../includes/tsql-md.md)] dinamico assegnato a una variabile verrà troncato se risulta maggiore del buffer allocato per quella variabile. Un utente malintenzionato in grado di imporre il troncamento delle istruzioni passando stringhe inaspettatamente lunghe a una stored procedure può modificare il risultato. La stored procedure creata dallo script seguente, ad esempio, è vulnerabile agli attacchi intrusivi consentiti dal troncamento.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 Passando 154 caratteri in un buffer di 128 caratteri, un utente malintenzionato può impostare una nuova password per sa senza conoscere quella vecchia.  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 Per questo motivo, è consigliabile usare un buffer di grandi dimensioni per una variabile di comando oppure eseguire direttamente [!INCLUDE[tsql](../../includes/tsql-md.md)] dinamico all'interno di un'istruzione `EXECUTE` .  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>Troncamento se vengono usati QUOTENAME (@variable, '''') e REPLACE()  
 Le stringhe restituite da QUOTENAME() e REPLACE() verranno troncate senza avviso se superano lo spazio allocato. La stored procedure creata nell'esempio seguente illustra le conseguenze.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Di conseguenza, l'istruzione seguente imposterà le password di tutti gli utenti sul valore passato nel codice precedente  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 È possibile imporre il troncamento delle stringhe superando lo spazio allocato del buffer quando si utilizza REPLACE(). La stored procedure creata nell'esempio seguente illustra le conseguenze.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Come con QUOTENAME(), il troncamento delle stringhe da parte di REPLACE() può essere evitato dichiarando variabili temporanee sufficientemente lunghe per tutti i casi. Se possibile, è necessario chiamare QUOTENAME() o REPLACE() direttamente all'interno di [!INCLUDE[tsql](../../includes/tsql-md.md)]dinamico. In caso contrario, è possibile calcolare la dimensione richiesta del buffer nel modo seguente. Per `@outbuffer = QUOTENAME(@input)`, la dimensione di `@outbuffer` deve essere `2*(len(@input)+1)`. Quando si usa `REPLACE()` e le virgolette doppie, come nell'esempio precedente, un buffer di `2*len(@input)` è sufficiente.  
  
 Il calcolo seguente tratta tutti i casi:  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>Troncamento quando viene usato QUOTENAME(@variable, ']')  
 Può verificarsi il troncamento quando il nome di un'entità a sicurezza diretta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene passato a istruzioni che utilizzano la forma `QUOTENAME(@variable, ']')`. Nell'esempio riportato di seguito viene illustrata questa situazione.  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 Quando si concatenano valori di tipo sysname, è consigliabile usare variabili temporanee di lunghezza sufficiente per contenere 128 caratteri per valore. Se possibile, chiamare `QUOTENAME()` direttamente all'interno di [!INCLUDE[tsql](../../includes/tsql-md.md)]dinamico. In caso contrario, è possibile calcolare la dimensione richiesta del buffer, come illustrato nella sezione precedente.  
  
## <a name="see-also"></a>Vedere anche  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [Sicurezza di SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
