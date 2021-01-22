---
description: Creare e usare indici in colonne usando Always Encrypted con enclave sicuri
title: Creare e usare indici in colonne usando Always Encrypted con enclave sicuri | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 4799cd725dce4eb8300717b8c89d601e9915f7d2
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534830"
---
# <a name="create-and-use-indexes-on-columns-using-always-encrypted-with-secure-enclaves"></a>Creare e usare indici in colonne usando Always Encrypted con enclave sicuri

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Questo articolo descrive come creare e usare gli indici nelle colonne crittografate usando chiavi di crittografia di colonna abilitate per l'enclave con [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md).

Always Encrypted con enclave sicuri supporta:
- Indici cluster e non cluster nelle colonne crittografate con la crittografia deterministica e le chiavi abilitate per l'enclave.
  - Tali indici sono ordinati in base al testo crittografato e non richiedono considerazioni particolari. È possibile gestirli e usarli allo stesso modo degli indici nelle colonne crittografate con la crittografia deterministica e le chiavi non abilitate per l'enclave (come con Always Encrypted). 
- Indici non cluster nelle colonne crittografate con la crittografia casuale e le chiavi abilitate per l'enclave.
  - I valori delle chiavi nella struttura dei dati dell'indice (albero B) vengono crittografati e ordinati in base ai relativi valori del testo non crittografato. Per altre informazioni, vedere [indici nelle colonne abilitate per l'enclave](always-encrypted-enclaves.md#indexes-on-enclave-enabled-columns).

> [!NOTE]
> Nel resto dell'articolo vengono illustrati gli indici non cluster nelle colonne crittografate con la crittografia casuale e le chiavi abilitate per l'enclave.

Poiché un indice su una colonna che usa la crittografia casuale e una chiave di crittografia di colonna abilitata per l'enclave contiene dati crittografati (testo crittografato) ordinati in base al testo non crittografato, il motore di SQL Server deve usare l'enclave per qualsiasi operazione che comporta la creazione, l'aggiornamento o la ricerca in un indice, tra cui:

- Creazione o ricompilazione di un indice.
- Inserimento, aggiornamento o eliminazione di una riga in una tabella (contenente una colonna indicizzata/crittografata), che attiva l'inserimento e/o la rimozione di una chiave di indice nell'indice.
- Esecuzione di comandi `DBCC` che comportano la verifica dell'integrità degli indici, ad esempio [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) oppure [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
- Ripristino del database (ad esempio, dopo il riavvio di SQL Server in seguito a un errore), se SQL Server deve annullare eventuali modifiche all'indice (altre informazioni di seguito).

Tutte le operazioni precedenti richiedono che l'enclave abbia la chiave di crittografia di colonna per la colonna indicizzata. La chiave è necessaria per decrittografare le chiavi di indice. In generale, l'enclave può ottenere una chiave di crittografia della colonna in due modi:
- Direttamente dall'applicazione client.
- Dalla cache delle chiavi di crittografia della colonna.

## <a name="invoke-indexing-operations-with-column-encryption-keys-provided-directly-by-the-client"></a>Richiamare operazioni di indicizzazione con chiavi di crittografia della colonna fornite direttamente dal client
Per il corretto funzionamento di questo metodo per richiamare le operazioni di indicizzazione, l'applicazione, incluso uno strumento come SQL Server Management Studio (SSMS), che esegue una query che attiva un'operazione su un indice deve:

- Connettersi al database con Always Encrypted e i calcoli dell'enclave abilitati per la connessione di database.
- L'applicazione deve avere accesso alla chiave master della colonna che protegge la chiave di crittografia della colonna per la colonna indicizzata.

Quando il motore di SQL Server analizza la query dell'applicazione e determina che sarà necessario aggiornare un indice su una colonna crittografata per eseguire la query, indica al driver client di fornire all'enclave la chiave di crittografia di colonna richiesta tramite un canale sicuro. Si tratta esattamente dello stesso meccanismo usato per fornire all'enclave le chiavi di crittografia di colonna per l'elaborazione di tutte le altre query che non usano indici, ad esempio la crittografia sul posto o le query che usano criteri di ricerca e confronti di intervalli.

Questo metodo è utile per garantire che la presenza di indici su colonne crittografate sia trasparente per le applicazioni già connesse al database con Always Encrypted e i calcoli dell'enclave abilitati. La connessione dell'applicazione può usare l'enclave per l'elaborazione delle query. Dopo aver creato un indice su una colonna, il driver all'interno dell'app fornirà in modo trasparente le chiavi di crittografia della colonna all'enclave per le operazioni di indicizzazione. La creazione di indici può aumentare il numero di query che richiedono all'applicazione di inviare all'enclave le chiavi di crittografia della colonna.

Per usare questo metodo, seguire le indicazioni generali per l'esecuzione di istruzioni usando un'enclave sicura in [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md).

Per istruzioni dettagliate su come usare questo metodo, vedere [Esercitazione: Creazione e uso di indici sulle colonne abilitate per l'enclave tramite la crittografia casuale](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).

## <a name="invoke-indexing-operations-using-cached-column-encryption-keys"></a>Richiamare operazioni di indicizzazione mediante chiavi di crittografia di colonna memorizzate nella cache

Una volta che un'applicazione client invia all'enclave una chiave di crittografia di colonna per l'elaborazione di qualsiasi query che richiede i calcoli dell'enclave, l'enclave memorizza la chiave di crittografia di colonna in una cache interna. Questa cache si trova all'interno dell'enclave e non è accessibile dall'esterno.

Se la stessa o un'altra applicazione client usata dallo stesso o da un altro utente attiva un'operazione su un indice senza specificare direttamente la crittografia della colonna richiesta, l'enclave cerca la chiave di crittografia di colonna nella cache. Di conseguenza, l'operazione sull'indice ha esito positivo, anche se l'applicazione client non ha fornito la chiave.

Per il corretto funzionamento di questo metodo di richiamo delle operazioni di indicizzazione, l'applicazione deve connettersi al database senza Always Encrypted abilitato per la connessione e la chiave di crittografia della colonna richiesta deve essere disponibile nella cache all'interno dell'enclave.

Questo metodo di chiamata delle operazioni è supportato solo per le query che non richiedono chiavi di crittografia di colonna per le altre operazioni non correlate agli indici. Ad esempio, un'applicazione che inserisce una riga tramite un'istruzione `INSERT` in una tabella che contiene una colonna crittografata deve connettersi al database con Always Encrypted abilitato nella stringa di connessione e deve avere accesso alle chiavi, indipendentemente dal fatto che la colonna crittografata contenga un indice o meno.

Questo metodo è utile per:
 - Assicurare che la presenza di indici sulle colonne abilitate per l'enclave con crittografia casuale sia trasparente per le applicazioni e gli utenti che non hanno accesso alle chiavi e ai dati come testo non crittografato. 
 - Garantire che la creazione di un indice in una colonna crittografata non rallenti le query esistenti. Se un'applicazione esegue una query su una tabella contenente colonne crittografate senza la necessità di avere accesso alle chiavi, l'applicazione può continuare a essere eseguita senza aver accesso alle chiavi dopo che un amministratore di database crea un indice. Ad esempio, si consideri un'applicazione che esegue la query seguente sulla tabella **Employees** che contiene colonne crittografate. L'amministratore di database non ha creato un indice su nessuna colonna crittografata.

   ```sql
   DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 1;
   GO
   ```

   Se l'applicazione invia la query tramite una connessione senza Always Encrypted e i calcoli dell'enclave abilitati, la query avrà esito positivo. La query non attiva alcun calcolo sulle colonne crittografate. Dopo che un amministratore di database crea un indice su colonne crittografate, la query attiva la rimozione dagli indici delle chiavi di indice. In questa situazione, l'enclave necessita delle chiavi di crittografia di colonna. L'applicazione sarà comunque in grado di continuare a eseguire questa query tramite la stessa connessione, purché il proprietario dei dati abbia fornito all'enclave le chiavi di crittografia della colonna.

 - Per ottenere la separazione dei ruoli durante la gestione degli indici, perché consente agli amministratori di database di creare e modificare gli indici su colonne crittografate senza dover accedere ai dati sensibili. 

> [!TIP] 
> [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md) consente di inviare facilmente all'enclave tutte le chiavi di crittografia di colonna abilitate per l'enclave usate per gli indici e di popolare la cache delle chiavi.

Per istruzioni dettagliate su come usare questo metodo, vedere [Esercitazione: Creazione e uso di indici sulle colonne abilitate per l'enclave tramite la crittografia casuale](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md). 

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)

## <a name="see-also"></a>Vedere anche  
- [Esercitazione: Creazione e uso di indici sulle colonne abilitate per l'enclave tramite la crittografia casuale](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).
- [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)
