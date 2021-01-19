---
title: Mascheramento dati dinamici | Microsoft Docs
description: Informazioni sulla maschera dati dinamica che limita l'esposizione dei dati sensibili, nascondendoli agli utenti senza privilegi. La funzione può semplificare notevolmente la sicurezza in SQL Server.
ms.date: 05/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
ms.assetid: a62f4ff9-2953-42ca-b7d8-1f8f527c4d66
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33fb069efc355ebaa8892a743b926f927d8a435e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171283"
---
# <a name="dynamic-data-masking"></a>Dynamic Data Masking
[!INCLUDE [SQL Server 2016 ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

![Maschera dati dinamica](../../relational-databases/security/media/dynamic-data-masking.png)

La maschera dati dinamica (DDM) limita l'esposizione dei dati sensibili nascondendoli agli utenti senza privilegi. Può essere usata per semplificare notevolmente la progettazione e la codifica della sicurezza nell'applicazione.  

La maschera dati dinamica è utile per impedire l'accesso non autorizzato ai dati sensibili consentendo agli utenti di specificare la quantità di dati sensibili da visualizzare, con un impatto minimo sul livello dell'applicazione. È possibile configurare DDM in campi di database designati per nascondere i dati sensibili nei set di risultati delle query. Con DDM i dati nel database non vengono modificati. La maschera dati dinamica è semplice da usare con le applicazioni esistenti, poiché vengono applicate le regole per la maschera nei risultati della query. Molte applicazioni sono in grado di mascherare i dati sensibili senza modificare le query esistenti.

* I criteri di mascheramento dei dati centrali operano direttamente sui campi sensibili del database.
* Designare gli utenti con privilegi o ruoli che hanno accesso ai dati sensibili.
* Le funzionalità DDM offrono funzioni di mascheramento completo e parziale, oltre a una maschera casuale per dati numerici.
* Semplici comandi [!INCLUDE[tsql_md](../../includes/tsql-md.md)] definiscono e gestiscono le maschere.

Lo scopo della maschera dati dinamica consiste nel limitare l'esposizione dei dati sensibili, impedendo la visualizzazione dei dati agli utenti che non dovrebbero averne accesso. La maschera dati dinamica non mira a impedire agli utenti del database di connettersi direttamente al database ed eseguire query complete che espongano parti dei dati sensibili. La maschera dati dinamica è complementare ad altre funzionalità di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad esempio il controllo, la crittografia e la sicurezza a livello di riga, ed è consigliabile usare questa funzionalità insieme alle altre per ottimizzare la protezione dei dati sensibili nel database.  
  
Il mascheramento dei dati dinamici è disponibile in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]e viene configurato usando i comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] . Per altre informazioni sulla configurazione di una maschera dati dinamica tramite il portale di Azure, vedere [Maschera dati dinamica del database SQL (portale di Azure)](/azure/azure-sql/database/dynamic-data-masking-overview).  
  
## <a name="defining-a-dynamic-data-mask"></a>Definizione di una maschera dati dinamica
 Una regola per la maschera può essere definita in una colonna all'interno di una tabella, al fine di nascondere i dati in tale colonna. Sono disponibili quattro tipi di maschere.  
  
|Funzione|Descrizione|Esempi|  
|--------------|-----------------|--------------|  
|Predefinito|Maschera completa in base al tipo di dati dei campi designati.<br /><br /> Per il tipo di dati stringa usare XXXX o X minori se la dimensione del campo è inferiore a 4 caratteri  (**char**, **nchar**,  **varchar**, **nvarchar**, **text**, **ntext**).  <br /><br /> Per il tipo di dati numerici, usare il valore zero (**bigint**, **bit**, **decimal**, **int**, **money**, **numeric**, **smallint**, **smallmoney**, **tinyint**, **float**, **real**).<br /><br /> Per i tipi di dati data e ora, usare 01.01.1900 00:00:00.0000000 (**date**, **datetime2**, **datetime**, **datetimeoffset**, **smalldatetime**, **time**).<br /><br />Per i tipi di dati binati, usare un singolo byte di valore 0 ASCII (**binary**, **varbinary**, **image**).|Esempio di sintassi di definizione della colonna: `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL`<br /><br /> Esempio di sintassi di alter: `ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')`|  
|Email|Metodo di maschera che espone la prima lettera di un indirizzo di posta elettronica e il suffisso costante ".com", sotto forma di un indirizzo di posta elettronica. `aXXX@XXXX.com`.|Esempio di sintassi di definizione: `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL`<br /><br /> Esempio di sintassi di alter: `ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')`|  
|Random (Casuale)|Una funzione di maschera casuale per l'uso in qualsiasi tipo numerico al fine di mascherare il valore originale con un valore casuale in un intervallo specificato.|Esempio di sintassi di definizione: `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')`<br /><br /> Esempio di sintassi di alter: `ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')`|  
|Stringa personalizzata|Metodo di maschera che espone la prima e l'ultima lettera e aggiunge al centro una stringa di riempimento personalizzata. `prefix,[padding],suffix`<br /><br /> Nota: se il valore originale è troppo breve per completare l'intera maschera, parte del prefisso o del suffisso non sarà esposta.|Esempio di sintassi di definizione: `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL`<br /><br /> Esempio di sintassi di alter: `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')`<br /><br /> Esempio aggiuntivo:<br /><br /> `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')`|  
  
## <a name="permissions"></a>Autorizzazioni  
 Non sono necessarie autorizzazioni per creare una tabella con una maschera dati dinamica. Sono sufficienti le autorizzazioni standard per schemi **CREATE TABLE** e **ALTER** .  
  
 L'aggiunta, la sostituzione o la rimozione della maschera da una colonna richiede le autorizzazioni **ALTER ANY MASK** e **ALTER** sulla tabella. È opportuno concedere l'autorizzazione **ALTER ANY MASK** a un responsabile della sicurezza.  
  
 Gli utenti con autorizzazione **SELECT** su una tabella possono visualizzare i dati in essa contenuti. Le colonne definite con maschera visualizzano i dati mascherati. Concedere l'autorizzazione **UNMASK** a un utente per consentire di recuperare i dati senza maschera dalle colonne su cui è definita la maschera.  
  
 L'autorizzazione **CONTROL** sul database include l'autorizzazione **ALTER ANY MASK** e **UNMASK** .  
  
## <a name="best-practices-and-common-use-cases"></a>Procedure consigliate e casi d'uso comuni  
  
-   La creazione di una maschera in una colonna non ne impedisce l'aggiornamento. Pertanto, anche se gli utenti ricevono dati mascherati quando eseguono una query sulla colonna con maschera, gli stessi utenti possono aggiornare i dati se dispongono di autorizzazioni di scrittura. Tuttavia, usare un criterio di controllo di accesso appropriato per limitare le autorizzazioni all'aggiornamento.  
  
-   Usando `SELECT INTO` o `INSERT INTO` per copiare i dati da una colonna con maschera in un'altra tabella vengono restituiti i dati mascherati nella tabella di destinazione.  
  
-   La maschera dati dinamica viene applicata quando si esegue l'importazione e l'esportazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un database contenente colonne con maschera genera un file di dati esportato con dati mascherati, supponendo che venga esportato da un utente senza privilegi **UNMASK**, e il database importato contiene dati mascherati in modo statico.  
  
## <a name="querying-for-masked-columns"></a>Esecuzione di query per le colonne con maschera  
 Usare la vista **sys.masked_columns** per eseguire query per tabelle e colonne con funzione di maschera applicata. Questa vista viene ereditata dalla vista **sys.columns** , che restituisce tutte le colonne della vista **sys.columns** , in aggiunta alle colonne **is_masked** e **masking_function** , che indicano se la colonna è nascosta e, in tal caso, quale funzione di maschera viene definita. Questa vista mostra solo le colonne su cui è applicata una funzione di maschera.  
  
```sql 
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.[object_id] = tbl.[object_id]  
WHERE is_masked = 1;  
```  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile definire una regola per la maschera per i tipi di colonna seguenti:  
  
-   Colonne crittografate (Crittografia sempre attiva)  
  
-   FILESTREAM  
  
-   COLUMN_SET o una colonna di tipo sparse che fa parte di un set di colonne.  
  
-   Una maschera non può essere configurata su una colonna calcolata, ma se la colonna calcolata dipende da una colonna dotata di MASCHERA, la colonna calcolata restituirà dati mascherati.  
  
-   Una colonna con la maschera dati non può essere una chiave per un indice FULLTEXT.  
  
 Per gli utenti senza autorizzazione **UNMASK** , le istruzioni **READTEXT**, **UPDATETEXT** e **WRITETEXT** deprecate non funzionano correttamente in una colonna configurata per la maschera dati dinamica. 
 
 Poiché l'aggiunta di una maschera di dati dinamici viene implementata come modifica dello schema nella tabella sottostante, l'operazione non può essere eseguita in una colonna con dipendenze. Per aggirare questa limitazione, è possibile rimuovere la dipendenza, aggiungere la maschera di dati dinamici e quindi ricreare la dipendenza. Ad esempio, se la dipendenza è dovuta a un indice che dipende dalla colonna, è possibile eliminare l'indice, aggiungere la maschera e quindi ricreare l'indice dipendente.
 

## <a name="security-note-bypassing-masking-using-inference-or-brute-force-techniques"></a>Nota sulla sicurezza: ignorare il mascheramento usando tecniche di attacchi di forza bruta o inferenza

Il mascheramento dei dati dinamici è progettato per semplificare lo sviluppo di applicazioni, limitando l'esposizione dei dati in un set di query predefinito usato dall'applicazione. Nonostante possa essere utile anche per prevenire l'esposizione accidentale dei dati sensibili durante l'accesso diretto a un database di protezione, è importante notare che gli utenti senza privilegi con autorizzazioni per le query ad hoc possono applicare le tecniche per ottenere l'accesso ai dati effettivi. Se è necessario concedere l'accesso ad hoc, usare il Controllo per monitorare tutte le attività del database e attenuare questo scenario.
 
Ad esempio, si consideri un'entità di database con privilegi sufficienti per eseguire query ad hoc sul database che provi a trovare i dati sottostanti e infine a dedurre i valori effettivi. Si presupponga di aver definito una maschera nella colonna `[Employee].[Salary]` e che l'utente si connetta direttamente al database e inizi a ipotizzare i valori, deducendo infine il valore `[Salary]` di un insieme di dipendenti:
 

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

>    |  ID | Nome| Stipendio |   
>    | ----- | ---------- | ------ | 
>    |  62543 | Valeria Dal Monte | 0 | 
>    |  91245 | Giorgio Cavaglieri | 0 |  

Ciò dimostra che il mascheramento dati dinamici non deve essere usato come misura isolata per garantire la sicurezza dei dati sensibili dagli utenti che eseguono query ad hoc nel database. È appropriato per impedire l'esposizione accidentale dei dati sensibili, ma non protegge da potenziali attacchi dannosi mirati a dedurre i dati sottostanti.
 
È importante gestire correttamente le autorizzazioni per il database e seguire sempre il principio di autorizzazioni minime necessarie. Ricordare anche di abilitare il Controllo per tenere traccia di tutte le attività eseguite sul database.

  
## <a name="examples"></a>Esempi  
  
### <a name="creating-a-dynamic-data-mask"></a>Creazione di una maschera dati dinamica  
 L'esempio seguente illustra la creazione di una tabella con tre tipi diversi di maschera dati dinamica. Nell'esempio la tabella viene compilata ed è possibile selezionare la visualizzazione dei risultati.  
  
```sql
CREATE TABLE Membership  
  (MemberID int IDENTITY PRIMARY KEY,  
   FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)') NULL,  
   LastName varchar(100) NOT NULL,  
   Phone varchar(12) MASKED WITH (FUNCTION = 'default()') NULL,  
   Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL);  
  
INSERT Membership (FirstName, LastName, Phone, Email) VALUES   
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com'),  
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co'),  
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net');  
SELECT * FROM Membership;  
```  
  
 Viene creato un nuovo utente a cui viene concessa autorizzazione **SELECT** per la tabella. Le query eseguite come `TestUser` possono visualizzare i dati mascherati.  
  
```sql 
CREATE USER TestUser WITHOUT LOGIN;  
GRANT SELECT ON Membership TO TestUser;  
  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;  
```  
  
 Il risultato illustra le maschere modificando i dati da  
  
 `1    Roberto     Tamburello    555.123.4567    RTamburello@contoso.com`  
  
 into  
  
 `1    RXXXXXXX    Tamburello    xxxx            RXXX@XXXX.com`  
  
### <a name="adding-or-editing-a-mask-on-an-existing-column"></a>Aggiunta o modifica di una maschera su una colonna esistente  
 Usare l'istruzione **ALTER TABLE** per aggiungere una maschera a una colonna esistente nella tabella o per modificare la maschera su tale colonna.  
L'esempio seguente aggiunge una funzione della maschera alla colonna `LastName`:  
  
```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName ADD MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');  
```  
  
 L'esempio seguente modifica una funzione della maschera sulla colonna `LastName` :  

```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName varchar(100) MASKED WITH (FUNCTION = 'default()');  
```  
  
### <a name="granting-permissions-to-view-unmasked-data"></a>Concessione di autorizzazioni per visualizzare i dati senza maschera  
 La concessione dell'autorizzazione **UNMASK** consente a `TestUser` di visualizzare i dati senza maschera.  
  
```sql
GRANT UNMASK TO TestUser;  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;   
  
-- Removing the UNMASK permission  
REVOKE UNMASK TO TestUser;  
```  
  
### <a name="dropping-a-dynamic-data-mask"></a>Eliminazione di una maschera dati dinamica  
 L'istruzione seguente elimina la maschera della colonna `LastName` creata nell'esempio precedente:  
  
```sql  
ALTER TABLE Membership   
ALTER COLUMN LastName DROP MASKED;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
 [sys.masked_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-masked-columns-transact-sql.md)   
 [Maschera dati dinamica del database SQL (portale di Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)
