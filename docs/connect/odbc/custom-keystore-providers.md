---
title: Provider di archivi chiavi personalizzati
description: Informazioni su come implementare un provider di archivi chiavi personalizzati da usare insieme a Microsoft ODBC Driver for SQL Server e la funzionalità Always Encrypted.
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 90eadc72e631ee59b0773dc47fe1199668484b65
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673135"
---
# <a name="custom-keystore-providers"></a>Provider di archivi chiavi personalizzati

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Panoramica

Per accedere ai dati archiviati nelle colonne crittografate, la funzionalità di crittografia di colonna di SQL Server 2016 richiede che le chiavi di crittografia della colonna crittografate (ECEKs) archiviate nel server vengano recuperate dal client e quindi decrittografate in chiavi di crittografia della colonna (chiavi CEK). Le chiavi ECEK vengono crittografate dalle chiavi master della colonna (CMK) e la sicurezza della chiave CMK è importante per la sicurezza della crittografia della colonna. La chiave CMK deve quindi essere archiviata in una posizione sicura. Lo scopo di un provider di archivi chiavi di crittografia della colonna è offrire un'interfaccia per consentire al driver ODBC di accedere a tali chiavi CMK archiviate in modo sicuro. Per gli utenti con una propria archiviazione sicura, l'interfaccia del provider dell'archivio chiavi personalizzato offre un framework per l'implementazione dell'accesso all'archiviazione sicura della chiave CMK per il driver ODBC, che può quindi essere usato per eseguire la crittografia e la decrittografia della chiave CEK.

Ogni provider dell'archivio chiavi contiene e gestisce una o più chiavi CMK, che sono identificate da percorsi delle chiavi (stringhe di un formato definito dal provider) La chiave CMK, insieme all'algoritmo di crittografia (anch'esso una stringa definita dal provider), può essere usata per eseguire la crittografia di una chiave CEK e la decrittografia di una chiave ECEK. L'algoritmo, insieme alla chiave ECEK e al nome del provider, viene archiviato nei metadati di crittografia del database. Per altre informazioni, vedere [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) e [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md). Le due operazioni fondamentali di gestione delle chiavi sono quindi:

```cpp
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

dove `CEKeystoreProvider_name` viene usato per identificare il provider dell'archivio chiavi di crittografia della colonna specifico (CEKeystoreProvider) e gli altri argomenti vengono usati da CEKeystoreProvider per crittografare/decrittografare la chiave (E)CEK. Il nome e il percorso della chiave vengono specificati dai metadati CMK, mentre l'algoritmo e il valore della chiave ECEK sono specificati dai metadati CEK. Possono essere presenti più provider dell'archivio chiavi oltre ai provider predefiniti. Quando si esegue un'operazione che richiede la chiave CEK, il driver usa i metadati CMK per trovare il provider dell'archivio chiavi appropriato in base al nome ed esegue l'operazione di decrittografia che può essere espressa come segue:

```cpp
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Benché il driver non debba crittografare le chiavi CEK, può essere necessario uno strumento di gestione delle chiavi per implementare operazioni come la creazione e la rotazione di chiavi CMK. Per questo è necessario eseguire l'operazione inversa:

```cpp
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>Interfaccia CEKeyStoreProvider

In questo documento viene descritta in dettaglio l'interfaccia CEKeyStoreProvider. Microsoft ODBC Driver for SQL Server può usare un provider dell'archivio chiavi che implementa questa interfaccia. Gli implementatori di CEKeyStoreProvider possono usare questa guida per sviluppare provider di archivi chiavi personalizzati utilizzabili dal driver.

Una libreria del provider dell'archivio chiavi ("libreria del provider") è una libreria di collegamento dinamico che può essere caricata dal driver ODBC e contiene uno o più provider dell'archivio chiavi. Il simbolo `CEKeystoreProvider` deve essere esportato da una libreria di provider ed essere l'indirizzo di una matrice con terminazione Null di puntatori alle strutture `CEKeystoreProvider`, una per ogni provider dell'archivio chiavi all'interno della libreria.

Una struttura `CEKeystoreProvider` definisce i punti di ingresso di un singolo provider dell'archivio chiavi:

```cpp
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Nome campo|Descrizione|
|:--|:--|
|`Name`|Nome del provider dell'archivio chiavi. Non deve corrispondere a qualsiasi altro provider dell'archivio chiavi caricato in precedenza dal driver o presente in questa libreria. Stringa di caratteri "wide"* con terminazione Null.|
|`Init`|Funzione di inizializzazione. Se non è necessaria una funzione di inizializzazione, questo campo può essere Null.|
|`Read`|Funzione di lettura del provider. Può essere Null se non è obbligatoria.|
|`Write`|Funzione di lettura del provider. Obbligatoria se Read non è Null. Può essere Null se non è obbligatoria.|
|`DecryptCEK`|Funzione di decrittografia della chiave ECEK. Questa funzione è il motivo dell'esistenza di un provider dell'archivio chiavi e non deve essere Null.|
|`EncryptCEK`|Funzione di crittografia della chiave CEK. Il driver non chiama questa funzione, ma viene offerta per consentire l'accesso a livello di codice alla creazione della chiave ECEK da parte degli strumenti di gestione delle chiavi. Può essere Null se non è obbligatoria.|
|`Free`|Funzione di terminazione. Può essere Null se non è obbligatoria.|

Ad eccezione di Free, le funzioni in questa interfaccia hanno tutti una coppia di parametri, **ctx** e **OnError**. Il primo identifica il contesto in cui viene chiamata la funzione, mentre il secondo viene usato per la segnalazione degli errori. Per altre informazioni, vedere [Contesti](#context-association) e [Gestione degli errori](#error-handling) di seguito.

```cpp
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```

Nome del segnaposto per una funzione di inizializzazione definita dal provider. Il driver chiama questa funzione una sola volta, dopo che un provider è stato caricato ma prima di dover eseguire per la prima volta richieste di decrittografia della chiave ECEK o Read()/Write(). Usare questa funzione per eseguire tutte le operazioni di inizializzazione necessarie.

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto dell'operazione.|
|`onError`|[Input] Funzione di segnalazione degli errori.|
|`Return Value`|Restituisce un valore diverso da zero per indicare l'esito positivo o zero per indicare un errore.|

```cpp
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Nome del segnaposto per una funzione di comunicazione definita dal provider. Il driver chiama questa funzione quando l'applicazione richiede la lettura dei dati da un provider (scritto in precedenza) utilizzando l'attributo di connessione SQL_COPT_SS_CEKEYSTOREDATA, consentendo all'applicazione di leggere dati arbitrari dal provider. Per altre informazioni, vedere [Comunicazione con i provider di archivi chiavi](using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto dell'operazione.|
|`onError`|[Input] Funzione di segnalazione degli errori.|
|`data`|[Output] Puntatore a un buffer in cui il provider scrive i dati che devono essere letti dall'applicazione. Il buffer corrisponde al campo dati della struttura CEKEYSTOREDATA.|
|`len`|Inout Puntatore a un valore di lunghezza; al momento dell'input, questo valore è la lunghezza massima del buffer di dati e il provider non deve scrivere più di `*len` byte. Al momento della restituzione, il provider deve essere aggiornato `*len` con il numero di byte scritti.|
|`Return Value`|Restituisce un valore diverso da zero per indicare l'esito positivo o zero per indicare un errore.|

```cpp
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```

Nome del segnaposto per una funzione di comunicazione definita dal provider. Il driver chiama questa funzione quando l'applicazione richiede la scrittura di dati in un provider tramite l'attributo di connessione SQL_COPT_SS_CEKEYSTOREDATA, consentendo all'applicazione di scrivere dati arbitrari nel provider. Per altre informazioni, vedere [Comunicazione con i provider di archivi chiavi](using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto dell'operazione.|
|`onError`|[Input] Funzione di segnalazione degli errori.|
|`data`|[Input] Puntatore a un buffer contenente i dati per la lettura da parte del provider. Il buffer corrisponde al campo dati della struttura CEKEYSTOREDATA. Il provider non deve leggere un numero di byte superiore al valore len da questo buffer.|
|`len`|[Input] Numero di byte disponibili nei dati. Questo valore corrisponde al campo DataSize della struttura CEKEYSTOREDATA.|
|`Return Value`|Restituisce un valore diverso da zero per indicare l'esito positivo o zero per indicare un errore.|

```cpp
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```

Nome del segnaposto per una funzione di decrittografia della chiave ECEK definita dal provider. Il driver chiama questa funzione per decrittografare una chiave ECEK crittografata da una chiave CMK associata a questo provider in una chiave CEK.

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto dell'operazione.|
|`onError`|[Input] Funzione di segnalazione degli errori.|
|`keyPath`|[Input] Valore dell'attributo dei metadati [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) per la chiave CMK a cui viene fatto riferimento dalla chiave ECEK specificata. Stringa di caratteri "wide"* con terminazione Null. Questo valore consente di identificare una chiave CMK gestita da questo provider.|
|`alg`|[Input] Valore dell'attributo dei metadati [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) per la chiave ECEK specificata. Stringa di caratteri "wide"* con terminazione Null. Questo valore consente di identificare l'algoritmo di crittografia usato per crittografare la chiave ECEK specificata.|
|`ecek`|[Input] Puntatore alla chiave ECEK da decrittografare.|
|`ecekLen`|[Input] Lunghezza della chiave ECEK.|
|`cekOut`|[Output] Il provider deve allocare memoria per la chiave ECEK decrittografata e scrivere il relativo indirizzo nel puntatore a cui punta cekOut. Deve essere possibile liberare questo blocco di memoria usando la funzione [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) o Free (Linux/macOS). Se non è stata allocata memoria a causa di un errore o per altri motivi, il provider deve impostare *cekOut su un puntatore Null.|
|`cekLen`|[Output] Il provider deve scrivere nell'indirizzo a cui punta cekLen la lunghezza della chiave ECEK decrittografata scritta in **cekOut.|
|`Return Value`|Restituisce un valore diverso da zero per indicare l'esito positivo o zero per indicare un errore.|

```cpp
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```

Nome del segnaposto per una funzione di crittografia della chiave CEK definita dal provider. Il driver non chiama questa funzione e non ne espone la funzionalità tramite il driver ODBC, ma viene offerta per consentire l'accesso a livello di codice alla creazione della chiave ECEK da parte degli strumenti di gestione delle chiavi.

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto dell'operazione.|
|`onError`|[Input] Funzione di segnalazione degli errori.|
|`keyPath`|[Input] Valore dell'attributo dei metadati [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) per la chiave CMK a cui viene fatto riferimento dalla chiave ECEK specificata. Stringa di caratteri "wide"* con terminazione Null. Questo valore consente di identificare una chiave CMK gestita da questo provider.|
|`alg`|[Input] Valore dell'attributo dei metadati [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) per la chiave ECEK specificata. Stringa di caratteri "wide"* con terminazione Null. Questo valore consente di identificare l'algoritmo di crittografia usato per crittografare la chiave ECEK specificata.|
|`cek`|[Input] Puntatore alla chiave CEK da crittografare.|
|`cekLen`|[Input] Lunghezza della chiave CEK.|
|`ecekOut`|[Output] Il provider deve allocare memoria per la chiave CEK crittografata e scrivere il relativo indirizzo nel puntatore a cui punta ecekOut. Deve essere possibile liberare questo blocco di memoria usando la funzione [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) o Free (Linux/macOS). Se non è stata allocata memoria a causa di un errore o per altri motivi, il provider deve impostare *ecekOut su un puntatore Null.|
|`ecekLen`|[Output] Il provider deve scrivere nell'indirizzo a cui punta ecekLen la lunghezza della chiave ECEK crittografata scritta in **ecekOut.|
|`Return Value`|Restituisce un valore diverso da zero per indicare l'esito positivo o zero per indicare un errore.|

```cpp
void (*Free)();
```

Nome del segnaposto per una funzione di terminazione definita dal provider. Il driver può chiamare questa funzione alla chiusura normale del processo.

> [!NOTE]
> *Le stringhe di caratteri "wide" sono caratteri a 2 byte (UTF-16) per il modo in cui vengono archiviate da SQL Server.*

### <a name="error-handling"></a>Gestione degli errori

Poiché durante l'elaborazione di un provider possono verificarsi errori, viene offerto un meccanismo che consente di segnalare gli errori al driver con maggiori dettagli rispetto a un valore booleano di esito positivo o negativo. Molte delle funzioni hanno una coppia di parametri, **ctx** e **onError**, usati insieme a questo scopo in aggiunta al valore di esito positivo o negativo restituito.

Il parametro **ctx** identifica il contesto in cui si verifica un'operazione del provider.

Il parametro **OnError** punta a una funzione di segnalazione degli errori, con il prototipo seguente:

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Argomento|Descrizione|
|:--|:--|
|`ctx`|[Input] Contesto in cui segnalare l'errore.|
|`msg`|[Input] Messaggio di errore da segnalare. Stringa di caratteri "wide" con terminazione Null. Per consentire le informazioni con parametri, questa stringa può contenere sequenze di formattazione di inserimento nel formato accettato dalla funzione [FormatMessage](/windows/desktop/api/winbase/nf-winbase-formatmessage) . La funzionalità estesa può essere specificata da questo parametro, come descritto di seguito.|
|...|Input Parametri Variadic aggiuntivi per adattare gli identificatori di formato in msg, a seconda dei casi.|

Per segnalare quando si è verificato un errore, il provider chiama OnError, specificando il parametro di contesto passato nella funzione del provider dal driver e un messaggio di errore con parametri aggiuntivi facoltativi da formattare. Il provider può chiamare questa funzione più volte per inviare più messaggi di errore consecutivamente in una chiamata di funzione del provider. Ad esempio:

```cpp
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```

Il `msg` parametro è in genere una stringa di caratteri wide, ma sono disponibili altre estensioni:

Usando uno dei valori predefiniti speciali con la macro IDS_MSG, si possono usare i messaggi di errore generici già esistenti e presenti in un modulo localizzato nel driver. Se, ad esempio, un provider non riesce ad allocare memoria, è possibile usare il messaggio `IDS_S1_001` "Impossibile allocare memoria":

`onError(ctx, IDS_MSG(IDS_S1_001));`

Affinché l'errore venga riconosciuto dal driver, la funzione del provider deve restituire un errore. Quando si verifica un errore nel contesto di un'operazione ODBC, gli errori inviati diventeranno accessibili nell'handle di connessione o di istruzione tramite il meccanismo di diagnostica ODBC standard (`SQLError`, `SQLGetDiagRec`e `SQLGetDiagField`).

### <a name="context-association"></a>Associazione del contesto

La struttura di `CEKEYSTORECONTEXT`, oltre a offrire il contesto per il callback dell'errore, può essere usata anche per determinare il contesto ODBC in cui viene eseguita un'operazione del provider. Questo contesto consente a un provider di associare i dati a ognuno di questi contesti, ad esempio per implementare la configurazione per connessione. A questo scopo, la struttura contiene tre puntatori opachi che corrispondono al contesto di ambiente, connessione e istruzione:

```cpp
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```

|Campo|Descrizione|
|:--|:--|
|`envCtx`|Contesto dell'ambiente.|
|`dbcCtx`|Contesto della connessione.|
|`stmtCtx`|Contesto dell'istruzione.|

Ognuno di questi contesti è un valore opaco che, sebbene non sia uguale all'handle ODBC corrispondente, può essere utilizzato come identificatore univoco per il punto di controllo: se handle *x* è associato al valore di contesto *y*, nessun altro handle di ambiente, connessione o istruzione presente contemporaneamente a *x* avrà il valore di contesto *y* e nessun altro valore di contesto verrà associato a handle *X*. Se l'operazione del provider eseguita non dispone di un contesto di handle specifico (ad esempio, le chiamate a SQLSetConnectAttr per caricare e configurare i provider, in cui non è presente alcun handle di istruzione), il valore di contesto corrispondente nella struttura è null.

## <a name="example"></a>Esempio

### <a name="keystore-provider"></a>Provider dell'archivio chiavi

Il codice seguente è un esempio di implementazione minima di provider dell'archivio chiavi.

```cpp
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/macOS: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>Applicazione ODBC

Il codice seguente è un'applicazione demo che usa il provider dell'archivio chiavi descritto sopra. Quando viene eseguito, verificare che la libreria del provider si trovi nella stessa directory del file binario dell'applicazione e che la stringa di connessione specifichi l'impostazione `ColumnEncryption=Enabled` o un DSN che la contenga.

```cpp
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/macOS: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>Vedere anche

[Using Always Encrypted with the ODBC Driver](using-always-encrypted-with-the-odbc-driver.md) (Utilizzo di Always Encrypted con il driver ODBC)
