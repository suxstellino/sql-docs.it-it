---
title: Uso di Always Encrypted con il driver ODBC
description: Informazioni su come sviluppare applicazioni ODBC usando Always Encrypted e Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: ab1b5b73ad1bd6ba02baa5ee31bb4be4b42bb63f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199007"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Uso di Always Encrypted con ODBC Driver for SQL Server
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>Applicabile a

- ODBC Driver 13.1 for SQL Server
- ODBC Driver 17 for SQL Server

### <a name="introduction"></a>Introduzione

In questo articolo vengono fornite informazioni su come sviluppare applicazioni ODBC utilizzando [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con le enclavi sicure](../../relational-databases/security/encryption/always-encrypted-enclaves.md) e il [driver ODBC per SQL Server](microsoft-odbc-driver-for-sql-server.md).

Always Encrypted consente alle applicazioni client di eseguire la crittografia dei dati sensibili senza mai rivelare i dati o le chiavi di crittografia a SQL Server o al database SQL di Azure. Di tutto ciò si occupa un driver abilitato per Always Encrypted, come ODBC Driver for SQL Server, che esegue in modo trasparente la crittografia e la decrittografia dei dati sensibili nell'applicazione client. Il driver determina automaticamente i parametri di query corrispondenti alle colonne di database con dati sensibili (protette mediante Always Encrypted) e crittografa i valori di tali parametri prima di passare i dati a SQL Server o al database SQL di Azure. Analogamente, il driver esegue in modo trasparente la decrittografia dei dati, recuperati dalle colonne di database crittografate nei risultati delle query. *Always Encrypted con enclave sicuri* estende la funzionalità esistente per abilitare funzionalità più avanzate per i dati sensibili, mantenendo i dati riservati.

Per altre informazioni, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) e [Always Encrypted con le enclave sicure](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

### <a name="prerequisites"></a>Prerequisiti

Configurare Always Encrypted nel database. Ciò implica il provisioning di chiavi Always Encrypted e l'impostazione della crittografia per le colonne di database selezionate. Se non è presente un database in cui Always Encrypted è configurato, seguire le istruzioni fornite nel blog di [introduzione a Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). In particolare, il database deve contenere le definizioni dei metadati per una chiave master della colonna, una chiave di crittografia della colonna e una tabella contenente una o più colonne crittografate con tale chiave di crittografia.

Se si usa Always Encrypted con enclave sicure, vedere [Sviluppare applicazioni usando Always Encrypted con enclave sicure](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md) per altri prerequisiti.

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Abilitazione di Always Encrypted in un'applicazione ODBC

Il modo più semplice per abilitare la crittografia dei parametri e la decrittografia delle colonne crittografate dei set di risultati consiste nell'impostare il valore della parola chiave della stringa di connessione `ColumnEncryption` su **Abilitato**. Di seguito è riportato un esempio di stringa di connessione che abilita la Always Encrypted:

```
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Always Encrypted può anche essere abilitato nella configurazione DSN, usando la stessa chiave e lo stesso valore (di cui verrà eseguito l'override dall'impostazione della stringa di connessione, se presente) o a livello di codice con l'attributo di pre-connessione `SQL_COPT_SS_COLUMN_ENCRYPTION`. Impostandolo in questo modo esegue l'override del valore impostato nella stringa di connessione o DSN:

```
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Una volta abilitato per la connessione, il comportamento di Always Encrypted può essere regolato per singole query. Per altre informazioni, vedere [Controllo dell'impatto di Always Encrypted sulle prestazioni](#controlling-the-performance-impact-of-always-encrypted) più avanti.

Si noti che l'abilitazione di Always Encrypted non è sufficiente per l'esito positivo della crittografia o della decrittografia. È anche necessario assicurarsi che:

- L'applicazione abbia le autorizzazioni di database *VIEW ANY COLUMN MASTER KEY DEFINITION* e *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , necessarie per accedere ai metadati sulle chiavi Always Encrypted nel database. Per informazioni dettagliate, vedere [Autorizzazioni per il database](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- L'applicazione possa accedere alla chiave master della colonna che protegge le chiavi di crittografia della colonna per le colonne crittografate sottoposte a query. Questo dipende dal provider dell'archivio chiavi che archivia la chiave master della colonna. Per altre informazioni, vedere [Uso degli archivi delle chiavi master delle colonne](#working-with-column-master-key-stores).

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>Abilitare Always Encrypted con enclave sicuri

> [!NOTE]
> In Linux e macOS è necessario usare OpenSSL versione 1.0.1 o successiva per usare Always Encrypted con enclave sicure.

A partire dalla versione 17.4, il driver supporta Always Encrypted con enclavi sicuri. Per abilitare l'uso dell'enclave durante la connessione a un database, impostare la `ColumnEncryption` chiave DSN, la parola chiave della stringa di connessione o l'attributo di connessione sul valore seguente: `<attestation protocol>\<attestation URL>` , dove:

- `<attestation protocol>` : specifica un protocollo usato per l'attestazione dell'enclave.
  - Se si usa [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] e il servizio sorveglianza host (HGS), `<attestation protocol>` deve essere `VBS-HGS` .
  - Se si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e Microsoft Azure attestazione, `<attestation protocol>` deve essere `SGX-AAS` .

- `<attestation URL>` : specifica un URL di attestazione (un endpoint del servizio di attestazione). È necessario ottenere un URL di attestazione per l'ambiente dall'amministratore del servizio di attestazione.

  - Se si usa [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host (HGS), vedere [Determinare e condividere l'URL di attestazione HGS](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Se si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e il servizio Attestazione di Microsoft Azure, vedere [Determinare l'URL di attestazione per i criteri di attestazione](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).


Esempi di stringhe di connessione che abilitano i calcoli enclave per una connessione al database:

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] :
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

Se il server, il servizio di attestazione e le chiavi CMK e CEK con supporto degli enclave sono configurati correttamente per le colonne desiderate, ora è possibile eseguire query che usano l'enclave, ad esempio la crittografia sul posto e i calcoli avanzati, oltre alla funzionalità esistente offerta da Always Encrypted. Per altre informazioni, vedere [Configurare Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).


### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recupero e modifica di dati nelle colonne crittografate

Dopo aver abilitato Always Encrypted in una connessione è possibile usare le API ODBC standard. Le API ODBC possono recuperare o modificare i dati nelle colonne di database crittografate. Per altre informazioni vedere i seguenti documenti:

- [Codice di esempio ODBC](cpp-code-example-app-connect-access-sql-db.md)
- [Guida di riferimento per programmatori ODBC](../../odbc/reference/odbc-programmer-s-reference.md)

L'applicazione deve avere le autorizzazioni database necessarie ed essere in grado di accedere alla chiave master della colonna. Il driver crittografa quindi tutti i parametri di query destinati alle colonne crittografate. Il driver decrittografa anche i dati recuperati dalle colonne crittografate. Il driver esegue tutte le operazioni di crittografia e decrittografia senza alcuna assistenza da parte del codice sorgente. Il programma funziona come se le colonne non fossero crittografate.

Se Always Encrypted non è abilitato, le query con parametri destinati alle colonne crittografate avranno esito negativo. I dati possono essere comunque recuperati dalle colonne crittografate, a condizione che non presentino parametri destinati alle colonne crittografate. Il driver tuttavia non tenterà alcuna decrittografia e l'applicazione riceverà i dati binari crittografati, sotto forma di matrici di byte.

La tabella seguente riepiloga il comportamento delle query, a seconda che la funzionalità Always Encrypted sia abilitata o meno:

|Caratteristica della query | Always Encrypted è abilitato e l'applicazione può accedere alle chiavi e ai metadati delle chiavi|Always Encrypted è abilitato e l'applicazione non può accedere alle chiavi o ai metadati delle chiavi | Always Encrypted è disabilitato|
|:---|:---|:---|:---|
| Parametri destinati alle colonne crittografate. | I valori dei parametri vengono crittografati in modo trasparente. | Errore | Errore|
| Recupero di dati dalle colonne crittografate, senza parametri destinati alle colonne crittografate.| I risultati delle colonne vengono decrittografati in modo trasparente. L'applicazione riceve i valori di colonna in testo non crittografato. | Errore | I risultati delle colonne crittografate non vengono decrittografate. L'applicazione riceve i valori crittografati come matrici di byte.

Gli esempi seguenti illustrano il recupero e la modifica dei dati nelle colonne crittografate. Negli esempi si presuppone una tabella con lo schema seguente. Si noti che le colonne SSN e BirthDate nascita sono crittografate.

```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Esempio di inserimento dati

Questo esempio illustra come inserire una riga nella tabella Patients. Tenere presente quanto segue:

- Il codice di esempio non contiene alcun elemento specifico per la crittografia. Il driver rileva automaticamente e crittografa i valori dei parametri SSN e data, che puntano a colonne crittografate. In questo modo la crittografia diventa trasparente per l'applicazione.

- I valori inseriti nelle colonne di database, incluse quelle crittografate, vengono passati come parametri associati (vedere [Funzione SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)). Quando si inviano valori a colonne non crittografate, l'uso dei parametri è facoltativo, nonostante sia consigliabile per prevenire attacchi SQL injection. È invece necessario usare i parametri in presenza di valori destinati a colonne crittografate. Se i valori inseriti nelle colonne SSN o BirthDate sono stati passati come valori letterali incorporati nell'istruzione della query, la query avrà esito negativo perché il driver non prova a crittografare o a elaborare i valori letterali nelle query. Di conseguenza, il server li rifiuterà come incompatibili con le colonne crittografate.

- Il tipo SQL del parametro inserito nella colonna SSN è impostato su SQL_CHAR, che esegue il mapping al tipo di dati di SQL Server **char** (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Se il tipo del parametro è stato impostato su SQL_WCHAR, che esegue il mapping a **nchar**, la query avrà esito negativo perché Always Encrypted non supporta le conversioni sul lato server da valori nchar crittografati a valori char crittografati. Vedere [Guida di riferimento per programmatori ODBC - Appendice D: Tipi di dati](../../odbc/reference/appendixes/appendix-d-data-types.md) per informazioni sui mapping dei tipi di dati.

```
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Esempio di recupero di dati di testo non crittografato

L'esempio seguente illustra come filtrare i dati in base ai valori crittografati e recuperare i dati in testo non crittografato dalle colonne crittografate. Tenere presente quanto segue:

- Il valore usato nella clausola WHERE per filtrare la colonna SSN deve essere passato usando SQLBindParameter, in modo che il driver possa crittografarlo in modo trasparente prima dell'invio al server.

- Tutti i valori stampati dal programma saranno in testo non crittografato, perché il driver decrittografa in modo trasparente i dati recuperati dalle colonne SSN e BirthDate.

> [!NOTE]
> Le query possono eseguire confronti di uguaglianza nelle colonne crittografate solo se la crittografia è deterministica o se è attivato l'enclave sicuro. Per altre informazioni, vedere [Selezione della crittografia deterministica o casuale](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Esempio di recupero di dati di testo crittografato

Se Always Encrypted non è abilitato, una query può comunque recuperare dati dalle colonne crittografate, a condizione che non presenti parametri destinati alle colonne crittografate.

L'esempio seguente illustra come recuperare dati crittografati binari da colonne crittografate. Tenere presente quanto segue:

- Considerato che Always Encrypted non è abilitato nella stringa di connessione, la query restituirà i valori crittografati di SSN e BirthDate come matrici di byte (il programma converte i valori in stringhe).
- Una query che recupera dati dalle colonne crittografate con Always Encrypted disabilitato può avere parametri, a condizione che nessuno dei parametri sia destinato a una colonna crittografata. La query precedente filtra in base alla colonna LastName, non è crittografata nel database. Se la query avesse filtrato per SSN o data di nascita, avrebbe avuto esito negativo.


```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="moneysmallmoney-encryption"></a>Crittografia Money/SmallMoney

A partire dalla versione del driver 17,7 è possibile usare Always Encrypted con MONEY e SMALLMONEY. Tuttavia, esistono alcuni passaggi aggiuntivi da eseguire.
Quando si inseriscono colonne MONEY o SMALLMONEY crittografate, usare uno dei tipi C seguenti:
```
SQL_C_CHAR
SQL_C_WCHAR
SQL_C_SHORT
SQL_C_LONG
SQL_C_FLOAT
SQL_C_DOUBLE
SQL_C_BIT
SQL_C_TINYINT
SQL_C_SBIGINT
SQL_C_NUMERIC
```

e un tipo SQL di `SQL_NUMERIC` o `SQL_DOUBLE` (la precisione può andare persa quando si usa questo tipo).

##### <a name="binding-the-variable"></a>Binding della variabile

Quando si associa una variabile MONEY/SMALLMONEY in una colonna crittografata, è necessario impostare i campi del descrittore seguenti:

```
// n is the descriptor record of the MONEY/SMALLMONEY parameter
// the type is assumed to be SMALLMONEY if isSmallMoney is true and MONEY otherwise

SQLHANDLE ipd = 0;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, (SQLPOINTER)&ipd, SQL_IS_POINTER, NULL);
SQLSetDescField(ipd, n, SQL_CA_SS_SERVER_TYPE, isSmallMoney ? (SQLPOINTER)SQL_SS_TYPE_SMALLMONEY :
                                                              (SQLPOINTER)SQL_SS_TYPE_MONEY, SQL_IS_INTEGER);
                                                              
                                                              
// If the variable is bound as SQL_NUMERIC, additional descriptor fields have to be set
// var is SQL_NUMERIC_STRUCT containing the value to be inserted

SQLHDESC   hdesc = NULL;
SQLGetStmtAttr(hStmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
SQLSetDescField(hdesc, n, SQL_DESC_PRECISION, (SQLPOINTER)(var.precision), 0);
SQLSetDescField(hdesc, n, SQL_DESC_SCALE, (SQLPOINTER)(var.scale), 0);
SQLSetDescField(hdesc, n, SQL_DESC_DATA_PTR, &var, 0);
```


#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Come evitare i problemi comuni quando si eseguono query su colonne crittografate

Questa sezione descrive categorie di errori comuni che si verificano quando si eseguono query su colonne crittografate da applicazioni ODBC e contiene alcune linee guida su come correggere tali errori.

##### <a name="unsupported-data-type-conversion-errors"></a>Errori di conversione dei tipi di dati non supportati

Always Encrypted supporta alcune conversioni per i tipi di dati crittografati. Per l'elenco dettagliato delle conversioni dei tipi supportati, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Per evitare errori di conversione dei tipi di dati, assicurarsi di osservare quanto riportato di seguito quando si usa la funzione SQLBindParameter con parametri destinati alle colonne crittografate:

- Il tipo SQL del parametro è esattamente lo stesso del tipo della colonna di destinazione oppure è supportata la conversione dal tipo SQL al tipo della colonna.

- La precisione e la scala dei parametri destinati alle colonne dei tipi di dati `decimal` e `numeric` di SQL Server sono uguali a quelle configurate per la colonna di destinazione.

- La precisione dei parametri destinati alle colonne dei tipi di dati `datetime2`, `datetimeoffset` e `time` di SQL Server non è maggiore della precisione per la colonna di destinazione nelle query che modificano la colonna di destinazione.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errori causati dal passaggio di testo non crittografato anziché di valori crittografati

Qualsiasi valore destinato a una colonna crittografata deve essere crittografato prima di essere inviato al server. Il tentativo di inserire, modificare o filtrare in base a un valore di testo non crittografato in una colonna crittografata genererà un errore. Per evitare tali errori, verificare che:

- Always Encrypted sia abilitato (nel DSN, nella stringa di connessione, prima della connessione impostando l'attributo di connessione `SQL_COPT_SS_COLUMN_ENCRYPTION` per una connessione specifica o l'attributo di istruzione `SQL_SOPT_SS_COLUMN_ENCRYPTION` per un'istruzione specifica).

- Sia usato SQLBindParameter per inviare i dati destinati alle colonne crittografate. L'esempio seguente illustra una query che filtra in modo errato in base a un valore letterale o costante in una colonna crittografata (SSN), anziché passare il valore letterale come argomento a SQLBindParameter.

```
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Precauzioni quando si usano SQLSetPos e SQLMoreResults

#### <a name="sqlsetpos"></a>SQLSetPos

L'API `SQLSetPos` consente a un'applicazione aggiornare le righe in un set di risultati usando buffer che sono stati associati con SQLBindCol e in cui precedentemente sono stati recuperati dati di riga. A causa del comportamento di riempimento asimmetrico dei tipi crittografati a lunghezza fissa, è possibile modificare in modo imprevisto i dati di queste colonne mentre si eseguono aggiornamenti in altre colonne della riga. Con Always Encrypted, i valori dei caratteri a lunghezza fissa verranno riempiti se il valore è inferiore alle dimensioni del buffer.

Per mitigare questo comportamento, usare il flag `SQL_COLUMN_IGNORE` per ignorare le colonne che non verranno aggiornate come parte di `SQLBulkOperations` e quando si usa `SQLSetPos` per gli aggiornamenti basati sul cursore.  Tutte le colonne che non vengono modificate direttamente dall'applicazione devono essere ignorate, sia per le prestazioni sia per evitare il troncamento delle colonne associate a un buffer *inferiore* alle dimensioni effettive (DB). Per altre informazioni, vedere la [guida di riferimento della funzione SQLSetPos](../../odbc/reference/syntax/sqlsetpos-function.md).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults e SQLDescribeCol

I programmi applicativi possono chiamare [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md) per restituire metadati sulle colonne nelle istruzioni preparate.  Se Always Encrypted è abilitato la chiamata di `SQLMoreResults` *prima* della chiamata `SQLDescribeCol` fa sì che venga chiamato [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), che non restituisce correttamente i metadati in testo non crittografato per le colonne crittografate. Per evitare questo problema, chiamare `SQLDescribeCol` nelle istruzioni preparate *prima* di chiamare `SQLMoreResults`.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Controllo dell'impatto di Always Encrypted sulle prestazioni

Considerato che Always Encrypted è una tecnologia di crittografia lato client, la maggior parte del sovraccarico delle prestazioni si verifica sul lato client, non nel database. A parte il costo delle operazioni di crittografia e decrittografia, le altre origini di sovraccarico delle prestazioni sul lato client sono le seguenti:

- Round trip aggiuntivi al database per recuperare i metadati per i parametri di query.

- Chiamate a un archivio delle chiavi master delle colonne per accedere alla chiave master di una colonna.

Questa sezione descrive le ottimizzazioni delle prestazioni predefinite in ODBC Driver for SQL Server e come controllare l'impatto sulle prestazioni esercitato dai due fattori descritti.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controllo dei round trip per recuperare i metadati per i parametri di query

Se la funzionalità Always Encrypted è abilitata per una connessione, per impostazione predefinita il driver chiamerà [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) per ogni query con parametri, passando l'istruzione di query (senza i valori dei parametri) a SQL Server. Questa stored procedure analizza l'istruzione di query per verificare se sono presenti parametri da crittografare e, in tal caso, restituisce le informazioni relative alla crittografia per ogni parametro in modo da consentire al driver di crittografarli. Il comportamento descritto garantisce all'applicazione client un elevato livello di trasparenza: non è necessario che l'applicazione (e lo sviluppatore dell'applicazione) sappiano quali query accedono alle colonne crittografate, a condizione che i valori destinati alle colonne crittografate vengano passati al driver nei parametri.

A partire dalla versione 17.6, il driver memorizza nella cache anche i metadati di crittografia per le istruzioni preparate, migliorando le prestazioni evitando che le chiamate successive a `SQLExecute` richiedano un round trip aggiuntivo per recuperare i metadati di crittografia.

### <a name="per-statement-always-encrypted-behavior"></a>Comportamento di Always Encrypted in base alle istruzioni

Per controllare l'impatto sulle prestazioni del recupero dei metadati di crittografia per le query con parametri, è possibile modificare il comportamento di Always Encrypted per singole query se è stato abilitato per la connessione. In questo modo, è possibile assicurarsi che `sys.sp_describe_parameter_encryption` venga richiamato solo per le query con parametri destinati a colonne crittografate. Si noti, tuttavia, che in tal modo si riduce la trasparenza della crittografia. Se si esegue la crittografia di colonne aggiuntive nel database, potrebbe essere necessario modificare il codice dell'applicazione per allinearlo alle modifiche dello schema.

Per controllare il comportamento di Always Encrypted di un'istruzione, chiamare SQLSetStmtAttr per impostare l'attributo di istruzione `SQL_SOPT_SS_COLUMN_ENCRYPTION` su uno dei valori seguenti:

|valore|Descrizione|
|-|-|
|`SQL_CE_DISABLED` (0)|Always Encrypted è disabilitato per l'istruzione|
|`SQL_CE_RESULTSETONLY` (1)|Solo decrittografia. I set di risultati e i valori restituiti sono decrittografati e i parametri non sono crittografati|
|`SQL_CE_ENABLED` (3) | Always Encrypted è abilitato e usato sia per i parametri che per i risultati|

I nuovi handle di istruzione creati da una connessione con Always Encrypted abilitato vengono impostati su SQL_CE_ENABLED per impostazione predefinita. Quelli creati da una connessione con Always Encrypted disabilitato vengono impostati su SQL_CE_DISABLED per impostazione predefinita (e per questi handle non è possibile abilitare Always Encrypted).

Se la maggior parte delle query di un'applicazione client accede alle colonne crittografate, si consiglia quanto segue:

- Impostare la parola chiave della stringa di connessione `ColumnEncryption` su `Enabled`.

- Impostare l'attributo `SQL_SOPT_SS_COLUMN_ENCRYPTION` su `SQL_CE_DISABLED` nelle istruzioni che non accedono alle colonne crittografate. In questo modo si disabiliterà sia la chiamata a `sys.sp_describe_parameter_encryption` che i tentativi di decrittografare qualsiasi valore nel set di risultati.
    
- Impostare l'attributo `SQL_SOPT_SS_COLUMN_ENCRYPTION` su `SQL_CE_RESULTSETONLY` nelle istruzioni che non contengono parametri che richiedono la crittografia, ma recuperano dati dalle colonne crittografate. In questo modo si disabiliterà la chiamata `sys.sp_describe_parameter_encryption` e la crittografia dei parametri. I risultati contenenti colonne crittografate continueranno a essere decrittografati.

- Utilizzare le istruzioni preparate per le query che verranno eseguite più di una volta. Preparare la query con `SQLPrepare` e salvare l'handle di istruzione riusandolo con `SQLExecute` ogni volta che viene eseguito. Questo è l'approccio preferito per le prestazioni anche quando non sono presenti colonne crittografate e consente al driver di sfruttare i metadati memorizzati nella cache.

## <a name="always-encrypted-security-settings"></a>Impostazioni di sicurezza di Always Encrypted

### <a name="force-column-encryption"></a>Forzare la crittografia di colonna

Per applicare la crittografia di un parametro, impostare il campo del descrittore parametri di implementazione `SQL_CA_SS_FORCE_ENCRYPT` tramite una chiamata alla funzione SQLSetDescField. Un valore diverso da zero fa sì che il driver restituisca un errore quando non vengono restituiti metadati di crittografia per il parametro associato.

```
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Se SQL Server indica al driver che il parametro non deve essere crittografato, le query che usano il parametro avranno esito negativo. Si aumenta così la protezione contro attacchi alla sicurezza, come un'istanza di SQL Server compromessa che invia metadati di crittografia non corretti al client, con il conseguente possibile rischio di divulgazione dei dati.

### <a name="column-encryption-key-caching"></a>Memorizzazione nella cache delle chiavi di crittografia della colonna

Per ridurre il numero di chiamate a un archivio chiavi master della colonna per decrittografare le chiavi di crittografia della colonna, il driver memorizza nella cache le chiavi di crittografia della colonna di testo non crittografato. La cache delle chiavi di crittografia della colonna è globale per il driver e non è associata ad alcuna connessione. Dopo aver ricevuto la chiave ECEK dai metadati del database, il driver prova prima di tutto a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente al valore della chiave crittografata nella cache. Il driver chiama l'archivio chiavi contenente la chiave master della colonna solo se non riesce a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente nella cache.

> [!NOTE]
> In ODBC Driver for SQL Server le voci nella cache vengono rimosse dopo un timeout di due ore. Questo significa che per una determinata chiave ECEK, il driver contatta l'archivio chiavi una sola volta nel corso della durata dell'applicazione o ogni due ore, a seconda di quale sia il valore più basso.

A partire da ODBC Driver 17.1 for SQL Server, il timeout della cache delle chiavi di crittografia della colonna può essere regolato usando l'attributo di connessione `SQL_COPT_SS_CEKCACHETTL`, che specifica il numero di secondi in cui una chiave di crittografia della colonna rimarrà nella cache. A causa della natura globale della cache, questo attributo può essere modificato da qualsiasi handle di connessione valido per il driver. Quando il valore TTL della cache viene ridotto, vengono rimosse anche le chiavi di crittografia della colonna esistenti che potrebbero superare il nuovo valore TTL. Se è 0, nessuna chiave di crittografia della colonna viene memorizzata nella cache.

### <a name="trusted-key-paths"></a>Percorsi delle chiavi attendibili

A partire da ODBC Driver 17.1 for SQL Server, l'attributo di connessione `SQL_COPT_SS_TRUSTEDCMKPATHS` consente a un'applicazione di richiedere che le operazioni Always Encrypted usino solo un elenco specificato di chiavi di crittografia della colonna, identificate dai relativi percorsi delle chiavi. Per impostazione predefinita, questo attributo è NULL, che significa che il driver accetta qualsiasi percorso della chiave. Per usare questa funzionalità, impostare `SQL_COPT_SS_TRUSTEDCMKPATHS` in modo che punti a una stringa di caratteri "wide" delimitati da Null con terminazione Null che elenca i percorsi delle chiavi consentiti. La memoria a cui fa riferimento questo attributo deve rimanere valida durante le operazioni di crittografia o decrittografia usando l'handle di connessione in cui è impostato, in base al quale il driver controllerà se il percorso CMK come specificato dai metadati del server fa distinzione tra maiuscole e minuscole in questo elenco. Se il percorso CMK non è presente nell'elenco, l'operazione non riesce. L'applicazione può modificare il contenuto della memoria a cui punta questo attributo, per modificare l'elenco di chiavi di crittografia della colonna attendibili, senza impostare nuovamente l'attributo.

## <a name="working-with-column-master-key-stores"></a>Uso degli archivi delle chiavi master delle colonne

Per crittografare o decrittografare i dati, il driver deve ottenere una chiave di crittografia della colonna configurata per la colonna di destinazione. Le chiavi di crittografia di colonna vengono archiviate in formato crittografato (ECEK) nei metadati del database. Ogni chiave di crittografia della colonna ha una chiave master della colonna corrispondente che è stata usata per crittografarla. I [metadati del database](../../t-sql/statements/create-column-master-key-transact-sql.md) non archiviano la chiave di crittografia della colonna stessa; contengono solo il nome dell'archivio chiavi e le informazioni che l'archivio chiavi può usare per trovare la chiave di crittografia della colonna.

Per ottenere il valore di testo non crittografato di una chiave ECEK, il driver ottiene prima di tutto i metadati relativi sia alla chiave di crittografia della colonna che alla chiave master della colonna corrispondente e quindi usa queste informazioni per contattare l'archivio chiavi contenente la chiave master della colonna e richiede di decrittografare la chiave ECEK. Il driver comunica con un archivio chiavi usando un provider dell'archivio chiavi.

### <a name="built-in-keystore-providers"></a>Provider di archivi chiavi predefiniti

ODBC Driver for SQL Server viene fornito con i provider di archivi chiavi predefiniti seguenti:

| Nome | Descrizione | Nome del provider (metadati) |Disponibilità|
|:---|:---|:---|:---|
|Insieme di credenziali chiave di Azure |Archivia le chiavi master della colonna in un'istanza di Azure Key Vault | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Archivio certificati Windows|Archivia le chiavi master della colonna in locale nell'archivio chiavi Windows| `MSSQL_CERTIFICATE_STORE`|Windows|

- È necessario che l'utente (o l'amministratore del database) verifichi che il nome del provider, configurato nei metadati della chiave master della colonna, sia corretto e che il percorso della chiave master sia conforme al formato del percorso della chiave per il provider specificato. È consigliabile configurare le chiavi usando strumenti come SQL Server Management Studio, che genera automaticamente i nomi di provider e i percorsi di chiave validi quando viene eseguita l'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) .

- È necessario verificare che l'applicazione possa accedere alla chiave nell'archivio chiavi. Questa operazione comportare l'accesso da parte dell'applicazione alla chiave e/o all'archivio chiavi, a seconda dell'archivio, o l'esecuzione di altri passaggi di configurazione specifici dell'archivio chiavi. Ad esempio, per accedere a un'istanza di Azure Key Vault, è necessario fornire le credenziali corrette all'archivio chiavi.

### <a name="using-the-azure-key-vault-provider"></a>Uso di Azure Key Vault Provider

Azure Key Vault (AKV) rappresenta una scelta valida per archiviare e gestire le chiavi master delle colonne per Always Encrypted, soprattutto se le applicazioni sono ospitate in Azure. ODBC Driver for SQL Server in Linux, macOS e Windows include un provider di archivio chiavi master della colonna predefinito per Azure Key Vault. Per altre informazioni sulla configurazione di un'istanza di Azure Key Vault per Always Encrypted, vedere [Azure Key Vault - Procedura dettagliata](/archive/blogs/kv/azure-key-vault-step-by-step), [Introduzione a Key Vault](/azure/key-vault/general/overview) e [Creazione di chiavi master della colonna in Azure Key Vault](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

> [!NOTE]
> Il driver ODBC supporta solo l'autenticazione AKV direttamente con Azure Active Directory. Se si usa l'autenticazione di Azure Active Directory per AKV e la configurazione di Active Directory richiede l'autenticazione in un endpoint Active Directory Federation Services, l'autenticazione potrebbe non riuscire.
> In Linux e macOS, per la versione del driver 17.2 e versioni successive, `libcurl` è necessario per usare questo provider, ma non è una dipendenza esplicita poiché altre operazioni con il driver non lo richiedono. Se si verifica un errore relativo a `libcurl`, verificare che sia installato.

Il driver supporta l'autenticazione ad Azure Key Vault usando i seguenti tipi di credenziali:

- Nome utente/Password: con questo metodo, le credenziali sono rappresentate dal nome di un utente di Azure Active Directory e dalla relativa password.

- ID client/Segreto: con questo metodo, le credenziali sono rappresentate da un ID client dell'applicazione e un segreto dell'applicazione.

- Identità gestita (17.5.2 +) - Assegnata dal sistema o dall'utente. Per altre informazioni, vedere [Identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/).

- Azure Key Vault Interactive (17,7 + driver Windows): con questo metodo, le credenziali vengono autenticate tramite Azure Active Directory con ID di accesso.

Per consentire al driver di usare chiavi master della colonna archiviate in Azure Key Vault per la crittografia di colonna, usare le seguenti parole chiave costituite solo dalla stringa di connessione:

|Tipo di credenziali|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|Nome utente/password| `KeyVaultPassword`|Nome entità utente|Password|
|ID client/Segreto| `KeyVaultClientSecret`|ID client|Segreto|
|Identità gestita|`KeyVaultManagedIdentity`|ID oggetto (facoltativo, solo per assegnata dall'utente)|(non specificato)|
|AKV interattivo|`KeyVaultInteractive`|(non impostato)|(non impostato)|

#### <a name="example-connection-strings"></a>Esempi di stringhe di connessione

Le stringhe di connessione seguenti illustrano come eseguire l'autenticazione ad Azure Key Vault con i due tipi di credenziali:

**ID client/segreto**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Nome utente/password**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**Identità gestita (assegnata dal sistema)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**Identità gestita (assegnata dall'utente)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

**AKV interattivo**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultInteractive;UID=<userID>;PWD=<password>
```

Non sono necessarie altre modifiche all'applicazione ODBC per usare Azure Key Vault per l'archiviazione di chiavi master della colonna.

> [!NOTE]
> Il driver contiene un elenco di endpoint AKV attendibili. A partire dalla versione del driver 17.5.2, questo elenco è configurabile: impostare la proprietà `AKVTrustedEndpoints` nel driver o nella chiave del Registro di sistema ODBCINST.INI o ODBC.INI di DSN (Windows) oppure nella sezione del file `odbcinst.ini` o `odbc.ini` (Linux/macOS) in un elenco delimitato da punti e virgola. L'impostazione nel sistema DSN ha la precedenza su un'impostazione nel driver. Se il valore inizia con un punto e virgola, estende l'elenco predefinito. In caso contrario, sostituisce l'elenco predefinito. L'elenco predefinito (fino alla versione 17.5) è `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`. A partire da 17,7, l'elenco include anche `managedhsm.azure.net;managedhsm.azure.cn;managedhsm.usgovcloudapi.net;managedhsm.microsoftazure.de` .


### <a name="using-the-windows-certificate-store-provider"></a>Uso del provider per l'archivio certificati Windows

ODBC Driver for SQL Server in Windows include un provider di archivio chiavi master della colonna predefinito per l'archivio certificati Windows denominato `MSSQL_CERTIFICATE_STORE`. (Questo provider non è disponibile in macOS o Linux). Con questo provider, la chiave master della colonna viene archiviata in locale nel computer client e non è necessaria alcuna configurazione aggiuntiva da parte dell'applicazione per usarla con il driver. Tuttavia, l'applicazione deve avere accesso al certificato e alla relativa chiave privata nell'archivio. Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-custom-keystore-providers"></a>Uso di provider di archivi chiavi personalizzati

ODBC Driver for SQL Server supporta anche i provider di archivi chiavi personalizzati di terze parti tramite l'interfaccia CEKeystoreProvider. Questo consente a un'applicazione di caricare, eseguire query e configurare i provider di archivi chiavi in modo che possano essere usati dal driver per accedere alle colonne crittografate. Le applicazioni possono anche interagire direttamente con un provider dell'archivio chiavi per crittografare le chiavi di crittografia della colonna per l'archiviazione in SQL Server ed eseguire attività che vanno oltre l'accesso alle colonne crittografate con ODBC; per altre informazioni, vedere [Provider di archivi chiavi personalizzati](custom-keystore-providers.md).

Due attributi di connessione vengono usati per interagire con i provider di archivi chiavi personalizzati, ovvero:

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

Il primo viene usato per caricare ed enumerare i provider di archivi chiavi caricati, mentre il secondo consente le comunicazioni tra il provider e l'applicazione. Questi attributi di connessione possono essere usati in qualsiasi momento, prima o dopo aver stabilito una connessione, in quanto l'interazione tra applicazione e provider non comporta la comunicazione con SQL Server. Tuttavia, poiché il driver non è stato ancora caricato, l'impostazione e il recupero di questi attributi prima della connessione ne causerà l'elaborazione da parte di Gestione driver e potrebbe non restituire i risultati previsti.

#### <a name="loading-a-keystore-provider"></a>Caricamento di un provider dell'archivio chiavi

L'impostazione dell'attributo di connessione `SQL_COPT_SS_CEKEYSTOREPROVIDER` consente a un'applicazione client di caricare una libreria di provider, rendendo disponibili per l'uso i provider di archivi chiavi in essa contenuti.

```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argomento | Descrizione |
|:---|:---|
|`ConnectionHandle`|[Input] Handle di connessione. Deve essere un handle di connessione valido, ma i provider caricati tramite un handle di connessione sono accessibili da qualsiasi altro provider nello stesso processo.|
|`Attribute`|[Input] Attributo da impostare: costante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Input] Puntatore a una stringa di caratteri con terminazione Null che specifica il nome file della libreria di provider. Per SQLSetConnectAttrA, si tratta di una stringa ANSI (multibyte). Per SQLSetConnectAttrW, si tratta di una stringa Unicode (wchar_t).|
|`StringLength`|[Input] Lunghezza della stringa ValuePtr o SQL_NTS.|

Il driver tenta di caricare la libreria identificata dal parametro ValuePtr usando il meccanismo di caricamento dinamico della libreria definito dalla piattaforma (`dlopen()` in Linux e macOS, `LoadLibrary()` in Windows) e aggiunge tutti i provider in essa definiti all'elenco di provider noti al driver. È possibile che si verifichino gli errori seguenti:

| Errore | Descrizione |
|:--|:--|
|`CE203`|Non è stato possibile caricare la libreria dinamica.|
|`CE203`|Il simbolo "CEKeyStoreProvider" esportato non è stato trovato nella libreria.|
|`CE203`|Uno o più provider nella libreria sono già caricati.|

`SQLSetConnectAttr` restituisce i soliti valori di errore o di riuscita e sono disponibili informazioni aggiuntive per eventuali errori che si sono verificati tramite il meccanismo diagnostico standard ODBC.

> [!NOTE]
> Il programmatore dell'applicazione deve assicurarsi che tutti i provider personalizzati vengano caricati prima che una query che li richieda venga inviata tramite qualsiasi connessione. In caso contrario, viene generato l'errore seguente:

| Errore | Descrizione |
|:--|:--|
|`CE200`|Il provider dell'archivio chiavi %1 non è stato trovato. Assicurarsi che sia stata caricata la libreria del provider dell'archivio chiavi appropriata.|

> [!NOTE]
> I responsabili dell'implementazione dei provider di archivi chiavi devono evitare l'uso di `MSSQL` nel nome dei propri provider personalizzati. Questo termine è riservato esclusivamente per l'uso da parte di Microsoft e può causare conflitti con i provider predefiniti futuri. L'uso di questo termine nel nome di un provider personalizzato può causare un avviso ODBC.

#### <a name="getting-the-list-of-loaded-providers"></a>Recupero dell'elenco di provider caricati

Il recupero di questo attributo di connessione consente a un'applicazione client di determinare i provider di archivi chiavi attualmente caricati nel driver (incluse quelli predefiniti). Questa operazione può essere eseguita solo dopo la connessione.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argomento | Descrizione |
|:---|:---|
|`ConnectionHandle`|[Input] Handle di connessione. Deve essere un handle di connessione valido, ma i provider caricati tramite un handle di connessione sono accessibili da qualsiasi altro provider nello stesso processo.|
|`Attribute`|[Input] Attributo da recuperare: costante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Output] Puntatore alla memoria in cui restituire il successivo nome del provider caricato.|
|`BufferLength`|[Input] Lunghezza del buffer ValuePtr.|
|`StringLengthPtr`|[Output] Puntatore a un buffer in cui restituire il numero totale di byte (escluso il carattere di terminazione Null) disponibile da restituire in \*ValuePtr. Se ValuePtr è un puntatore Null, non viene restituita alcuna lunghezza. Se il valore dell'attributo è una stringa di caratteri e il numero di byte disponibili da restituire è maggiore di BufferLength meno la lunghezza del carattere di terminazione Null, i dati in \*ValuePtr vengono troncati a BufferLength meno la lunghezza del carattere di terminazione Null e la terminazione Null viene aggiunta dal driver.|

Per consentire il recupero dell'intero elenco, ogni operazione Get restituisce il nome del provider corrente e incrementa un contatore interno a quello successivo. Quando questo contatore raggiunge la fine dell'elenco, viene restituita una stringa vuota ("") e il contatore viene reimpostato; le operazioni Get successive quindi proseguono nuovamente dall'inizio dell'elenco.

### <a name="communicating-with-keystore-providers"></a>Comunicazione con i provider di archivi chiavi

L'attributo di connessione `SQL_COPT_SS_CEKEYSTOREDATA` consente a un'applicazione client di comunicare con i provider di archivi chiavi caricati per la configurazione di parametri aggiuntivi, materiale per la generazione di chiavi e così via. La comunicazione tra un'applicazione client e un provider segue un protocollo di richiesta-risposta semplice, basato su richieste Get e Set mediante questo attributo di connessione. La comunicazione viene avviata solo dall'applicazione client.

> [!NOTE]
> A causa della natura delle chiamate ODBC a cui risponde CEKeyStoreProvider (SQLGet/SetConnectAttr), l'interfaccia ODBC supporta solo l'impostazione dei dati alla risoluzione del contesto di connessione.

L'applicazione comunica con i provider di archivi chiavi tramite il driver tramite la struttura CEKeystoreData:

```
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| Argomento | Descrizione |
|:---|:---|
|`name`|[Input] Dopo Set, il nome del provider a cui vengono inviati i dati. Ignorato dopo Get. Stringa di caratteri "wide" con terminazione Null.|
|`dataSize`|[Input] Dimensioni della matrice di dati che segue la struttura.|
|`data`|[InOut] Dopo Set, i dati da inviare al provider. Può trattarsi di dati arbitrari. Il driver non fa alcun tentativo di interpretarli. Dopo Get, il buffer per ricevere i dati letti dal provider.|

#### <a name="writing-data-to-a-provider"></a>Scrittura di dati in un provider

Una chiamata a `SQLSetConnectAttr` con l'attributo `SQL_COPT_SS_CEKEYSTOREDATA` scrive un "pacchetto" di dati nel provider dell'archivio chiavi specificato.
```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argomento | Descrizione |
|:---|:---|
|`ConnectionHandle`| [Input] Handle di connessione. Deve essere un handle di connessione valido, ma i provider caricati tramite un handle di connessione sono accessibili da qualsiasi altro provider nello stesso processo.|
|`Attribute`|[Input] Attributo da impostare: costante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Input] Puntatore a una struttura CEKeystoreData. Il campo del nome della struttura identifica il provider a cui sono destinati i dati.|
|`StringLength`|[Input] Costante SQL_IS_POINTER|

Altre informazioni dettagliate sugli errori possono essere ottenute tramite [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

> [!NOTE]
> Il provider può usare l'handle di connessione per associare i dati scritti a una connessione specifica, se è opportuno. Questo è utile per implementare la configurazione in base alla connessione. È possibile anche ignorare il contesto della connessione e trattare i dati in modo identico indipendentemente dalla connessione usata per inviare i dati. Per altre informazioni, vedere [Associazione del contesto](custom-keystore-providers.md#context-association).

#### <a name="reading-data-from-a-provider"></a>Lettura di dati da un provider

Una chiamata a `SQLGetConnectAttr` con l'attributo `SQL_COPT_SS_CEKEYSTOREDATA` legge un "pacchetto" di dati dall'*ultimo provider in cui sono stati scritti dati*. Se non è presente, si verifica un errore nella sequenza della funzione. Si consiglia ai responsabili dell'implementazione di provider di archivi chiavi di supportare "scritture fittizie" di 0 byte come modo per selezionare il provider per le operazioni di lettura senza causare altri effetti collaterali, se è opportuno farlo.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argomento | Descrizione |
|:---|:---|
|`ConnectionHandle`|[Input] Handle di connessione. Deve essere un handle di connessione valido, ma i provider caricati tramite un handle di connessione sono accessibili da qualsiasi altro provider nello stesso processo.|
|`Attribute`|[Input] Attributo da recuperare: costante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Output] Puntatore a una struttura CEKeystoreData in cui vengono inseriti i dati letti dal provider.|
|`BufferLength`|[Input] Costante SQL_IS_POINTER|
|`StringLengthPtr`|[Output] Puntatore a un buffer in cui restituire BufferLength. Se *ValuePtr è un puntatore Null, non viene restituita alcuna lunghezza.|

Il chiamante deve garantire che venga allocato un buffer di lunghezza sufficiente dopo la struttura CEKEYSTOREDATA per il provider in cui scrivere. Al momento della restituzione, il relativo campo dataSize viene aggiornato con la lunghezza effettiva dei dati letti dal provider. Altre informazioni dettagliate sugli errori possono essere ottenute tramite [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

Questa interfaccia non pone requisiti aggiuntivi sul formato dei dati trasferiti tra un'applicazione e un provider dell'archivio chiavi. Ogni provider può definire il proprio protocollo/formato di dati, a seconda delle esigenze.

Per un esempio di implementazione di un proprio provider dell'archivio chiavi, vedere [Provider di archivi chiavi personalizzati](custom-keystore-providers.md).

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Limitazioni del driver ODBC quando si usa Always Encrypted

### <a name="asynchronous-operations"></a>Operazioni asincrone
Mentre il driver ODBC consentirà l'uso di [operazioni asincrone](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) con Always Encrypted, c'è un impatto sulle prestazioni delle operazioni quando Always Encrypted è abilitato. La chiamata a `sys.sp_describe_parameter_encryption` per determinare i metadati di crittografia per l'istruzione viene bloccata facendo sì che il driver attenda che il server restituisca i metadati prima di restituire `SQL_STILL_EXECUTING`.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Recuperare i dati in parti con SQLGetData
Prima di ODBC Driver 17 for SQL Server non era possibile recuperare i caratteri crittografati e le colonne di dati binari in parti con SQLGetData. Ora è possibile eseguire una sola chiamata a SQLGetData, con un buffer di lunghezza sufficiente per contenere i dati dell'intera colonna.

### <a name="send-data-in-parts-with-sqlputdata"></a>Inviare i dati in parti con SQLPutData
Prima di ODBC Driver 17.3 for SQL Server i dati per l'inserimento o il confronto non possono essere inviati in parti con SQLPutData. È possibile eseguire una sola chiamata a SQLPutData, con un buffer che contiene tutti i dati. Per l'inserimento di dati Long in colonne crittografate, usare l'API della copia bulk, descritta nella sezione successiva, con un file di dati di input.

### <a name="encrypted-money-and-smallmoney"></a>Smallmoney e money crittografati
I parametri non possono destinare le colonne crittografate **money** o **smallmoney**, in quanto non esiste un tipo di dati ODBC specifico che esegue il mapping a questi tipi. Si generano così errori di conflitto del tipo di operando.

## <a name="bulk-copy-of-encrypted-columns"></a>Copia bulk di colonne crittografate

A partire da ODBC Driver 17 for SQL Server, è possibile usare le [funzioni di copia bulk di SQL](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) e l'utilità **bcp** supportata con Always Encrypted. Sia il testo non crittografato (crittografato in inserimento e decrittografato in recupero) sia il testo crittografato (verbatim trasferito) possono essere inseriti e recuperati usando le API di copia bulk (bcp_&#42;) e l'utilità **bcp**.

- Per recuperare il testo crittografato nel formato varbinary(max) (ad esempio per il caricamento bulk in un database diverso), connettersi senza l'opzione `ColumnEncryption`, oppure impostarla su `Disabled`, ed eseguire l'operazione BCP OUT.

- Per inserire e recuperare testo non crittografato e consentire al driver di eseguire la crittografia e la decrittografia in modo trasparente, impostare semplicemente `ColumnEncryption` su `Enabled`. In caso contrario, la funzionalità dell'API BCP rimane invariata.

- Per inserire testo crittografato nel formato varbinary (max) (ad esempio il testo recuperato descritto in precedenza), impostare l'opzione `BCPMODIFYENCRYPTED` su TRUE ed eseguire l'operazione BCP IN. Affinché i dati risultanti possano essere decrittografati, verificare che il valore CEK della colonna di destinazione sia lo stesso dal quale è stato originariamente ottenuto il testo crittografato.

Quando si usa l'utilità **bcp**: Per controllare l'impostazione `ColumnEncryption` usare l'opzione -D e specificare un DSN che contiene il valore desiderato. Per inserire testo crittografato, assicurarsi che sia abilitata l'impostazione `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` dell'utente.

La tabella seguente fornisce un riepilogo delle azioni da eseguire quando si usa una colonna crittografata:

|<code>ColumnEncryption</code>|Direzione BCP|Descrizione|
|----------------|-------------|-----------|
|`Disabled`|OUT (al client)|Recupera testo crittografato. Il tipo di dati osservati è **varbinary(max)** .|
|`Enabled`|OUT (al client)|Recupera testo non crittografato. Il driver decrittograferà i dati della colonna.|
|`Disabled`|IN (al server)|Inserisce testo crittografato. Questa operazione serve per spostare in modo non trasparente i dati crittografati senza che sia necessario decrittografarli. L'operazione avrà esito negativo se l'opzione `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` non è impostata sull'utente o BCPMODIFYENCRYPTED non è impostata sull'handle di connessione. Per altre informazioni, vedere più avanti.|
|`Enabled`|IN (al server)|Inserisce testo non crittografato. Il driver crittograferà i dati della colonna.|

### <a name="the-bcpmodifyencrypted-option"></a>Opzione BCPMODIFYENCRYPTED

Per evitare il danneggiamento dei dati, il server in genere non consente l'inserimento di testo crittografato direttamente in una colonna crittografata e quindi i tentativi di eseguire questa operazione avranno esito negativo; tuttavia, per il caricamento bulk dei dati crittografati con l'API BCP, se si imposta l'opzione `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) su TRUE sarà possibile inserire direttamente testo crittografato riducendo il rischio di danneggiare i dati crittografati tramite impostazione dell'opzione `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` sull'account utente. Ciononostante, le chiavi devono corrispondere ai dati ed è consigliabile eseguire alcuni controlli di sola lettura dei dati inseriti dopo l'inserimento bulk e prima di poterli usare.

Per altre informazioni, vedere [Migrare dati sensibili protetti da Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="always-encrypted-api-summary"></a>Riepilogo dell'API Always Encrypted

### <a name="connection-string-keywords"></a>Parole chiave per le stringhe di connessione

|Nome|Descrizione|  
|----------|-----------------|  
|`ColumnEncryption`|I valori accettati sono `Enabled`/`Disabled`.<br>`Enabled`: abilita o disabilita la funzionalità Always Encrypted per la connessione.<br>`Disabled`: disabilita la funzionalità Always Encrypted per la connessione.<br>*protocollo di attestazione*,*URL di attestazione* : (versione 17,4 e successive) consente always Encrypted con l'enclave protetta usando il protocollo di attestazione specificato e l'URL di attestazione. <br><br>Il valore predefinito è `Disabled`.|
|`KeyStoreAuthentication` | Valori validi: `KeyVaultPassword` , `KeyVaultClientSecret` , `KeyVaultInteractive` |
|`KeyStorePrincipalId` | Quando `KeyStoreAuthentication` = `KeyVaultPassword`, impostare questo valore su un nome dell'entità utente di Azure Active Directory valido. <br>Quando `KeyStoreAuthetication` = `KeyVaultClientSecret`, impostare questo valore su un ID client dell'applicazione di Azure Active Directory valido. |
|`KeyStoreSecret` | Quando `KeyStoreAuthentication` = `KeyVaultPassword`,impostare questo valore sulla password per il nome utente corrispondente. <br>Quando `KeyStoreAuthentication` = `KeyVaultClientSecret`, impostare questo valore sul segreto dell'applicazione associato a un ID client dell'applicazione di Azure Active Directory valido. |


### <a name="connection-attributes"></a>Attributi di connessione

|Nome|Tipo|Descrizione|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|Pre-connessione|`SQL_COLUMN_ENCRYPTION_DISABLE` (0) -- Disabilita Always Encrypted <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1) -- Abilita Always Encrypted<br> puntatore al *protocollo di attestazione*, stringa dell'*URL di attestazione* --(versione 17,4 e successive) abilitata con l'enclave sicura|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|Post-connessione|[Set] Prova a caricare CEKeystoreProvider<br>[Get] Restituisce un nome CEKeystoreProvider|
|`SQL_COPT_SS_CEKEYSTOREDATA`|Post-connessione|[Set] Scrive dati in CEKeystoreProvider<br>[Get] Legge dati da CEKeystoreProvider|
|`SQL_COPT_SS_CEKCACHETTL`|Post-connessione|[Set] Imposta il valore TTL della cache delle chiavi di crittografia della colonna<br>[Get] Recupera il valore TTL corrente della cache delle chiavi di crittografia della colonna|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|Post-connessione|[Set] Imposta il puntatore ai percorsi CMK attendibili<br>[Get] Recupera il puntatore corrente ai percorsi CMK attendibili|

### <a name="statement-attributes"></a>Attributi di istruzione

|Nome|Descrizione|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0) -- Always Encrypted è disabilitato per l'istruzione <br>`SQL_CE_RESULTSETONLY` (1) -- Solo decrittografia. I set di risultati e i valori restituiti sono decrittografati e i parametri non sono crittografati <br>`SQL_CE_ENABLED` (3) -- Always Encrypted è abilitato e usato sia per i parametri che per i risultati|

### <a name="descriptor-fields"></a>Campi del descrittore

|Campo descrittore parametri di implementazione|Dimensioni/Tipo|Default Value|Descrizione|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|WORD (2 byte)|0|Quando 0 (impostazione predefinita): la decisione di crittografare questo parametro è determinata dalla disponibilità dei metadati di crittografia.<br><br>Quando diverso da zero: se sono disponibili metadati di crittografia per questo parametro, viene crittografato. In caso contrario, la richiesta ha esito negativo con l'errore [CE300] [Microsoft][ODBC Driver 17 for SQL Server]La crittografia obbligatoria è stata specificata per un parametro ma non sono stati forniti metadati di crittografia dal server.|

### <a name="bcp_control-options"></a>Opzioni bcp_control

|Nome opzione|Default Value|Descrizione|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Quando TRUE, consente di inserire valori varbinary(max) in una colonna crittografata. Quando FALSE, impedisce l'inserimento a meno che non vengano forniti i metadati di crittografia e il tipo corretti.|

## <a name="troubleshooting"></a>Risoluzione dei problemi

Quando si riscontrano difficoltà nell'uso di Always Encrypted, iniziare controllando i punti seguenti:

- La chiave CEK che esegue la crittografia della colonna desiderata è presente e accessibile sul server.

- La chiave CMK che esegue la crittografia di CEK dispone di metadati accessibili sul server ed è accessibile anche dal client.

- `ColumnEncryption` è abilitato nel DSN, nella stringa di connessione o nell'attributo di connessione e, se si usa l'enclave protetta, ha il formato corretto.

Inoltre, quando si usa l'enclave sicura, gli errori di attestazione identificano il passaggio del processo di attestazione in cui si è verificato l'errore, in base alla tabella seguente:

|Passaggio|Descrizione|
|----|-----------|
|0-99| Risposta di attestazione non valida o errore di verifica della firma. |
|100-199| Errore durante il recupero dei certificati dall'URL di attestazione. Verificare che `<attestation URL>/v2.0/signingCertificates` sia valido e accessibile. |
|200-299| Formato dell'identità dell'enclave inatteso o errato. |
|300-399| Errore durante la creazione del canale sicuro con enclave. |

## <a name="see-also"></a>Vedere anche

- [Always Encrypted (Motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md)
