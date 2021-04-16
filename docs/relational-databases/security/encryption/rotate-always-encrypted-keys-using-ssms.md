---
title: Ruotare chiavi Always Encrypted con SQL Server Management Studio | Microsoft Docs
description: Informazioni sulla rotazione delle chiavi master della colonna Always Encrypted e delle chiavi di crittografia della colonna con SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
- sql13.SWB.COLUMNMASTERKEY.CLEANUP.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9137726ecd04a56f5b5e55fd6e8dcf33407c9e86
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561046"
---
# <a name="rotate-always-encrypted-keys-using-sql-server-management-studio"></a>Ruotare chiavi Always Encrypted con SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive le attività necessarie per ruotare chiavi master della colonna Always Encrypted e chiavi di crittografia della colonna con [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Per una panoramica sulla gestione delle chiavi Always Encrypted, incluse indicazioni sulle procedure consigliate e importanti considerazioni sulla sicurezza, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

> [!NOTE]
> L'uso delle chiavi master della colonna archiviate in un [HSM](https://docs.microsoft.com/azure/key-vault/managed-hsm/overview) gestito in Azure Key Vault richiede SSMS 18.9 o versione successiva.

<a name="rotatecmk"></a>
## <a name="rotate-column-master-keys"></a>Ruotare le chiavi master della colonna 

La rotazione di una chiave master della colonna è il processo di sostituzione di una chiave master della colonna esistente con la nuova chiave. Potrebbe essere necessario ruotare una chiave se questa è stata compromessa oppure per conformità ai criteri e alle normative dell'organizzazione che impongono la rotazione a intervalli regolari delle chiavi crittografiche. La rotazione delle chiavi master della colonna interessa la decrittografia delle chiavi di crittografia della colonna che sono protette dalla chiave master corrente della colonna, la loro successiva crittografia tramite la chiave master nuova della colonna e l'aggiornamento dei metadati per entrambi i tipi di chiavi. 

### <a name="step-1-provision-a-new-column-master-key"></a>Passaggio 1: Eseguire il provisioning di una nuova chiave master della colonna

Seguire la procedura descritta in [Effettuare il provisioning delle chiavi master di colonna con la finestra di dialogo Nuova chiave master della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog).

### <a name="step-2-encrypt-column-encryption-keys-with-the-new-column-master-key"></a>Passaggio 2: Eseguire la crittografia delle chiavi di crittografia della colonna con la nuova chiave master della colonna

Una chiave master della colonna protegge in genere uno o più chiavi di crittografia di colonna. Una chiave master della colonna protegge in genere una o più chiavi di crittografia della colonna. Ognuna di queste chiavi contiene un valore crittografato e archiviato nel database, che è il risultato della crittografia della chiave di crittografia con la chiave master della colonna.
In questo passaggio è necessario crittografare tutte le chiavi di crittografia della colonna protette con la chiave master della colonna, eseguendo la rotazione con la nuova chiave master della colonna e archiviando il nuovo valore crittografato nel database. Di conseguenza, ogni chiave di crittografia della colonna interessata dalla rotazione avrà due valori crittografati: un valore crittografato con la chiave master della colonna esistente e un nuovo valore crittografato con la nuova chiave master della colonna.

1.  In **Esplora oggetti** passare alla cartella **Sicurezza>Chiavi Always Encrypted>Chiavi master della colonna** e individuare la chiave master della colonna per la rotazione.
2.  Fare clic con il pulsante destro del mouse sulla chiave master della colonna e selezionare **Ruota**.
3.  Nella finestra **Rotazione della chiave master della colonna** selezionare il nome della nuova chiave master della colonna creata nel passaggio 1 nel campo **Destinazione** .
4.  Consultare l'elenco delle chiavi di crittografia della colonna protette dalle chiavi master della colonna esistenti. Queste chiavi saranno coinvolte nella rotazione.
5.  Fare clic su **OK**.

SQL Server Management Studio ottiene i metadati delle chiavi di crittografia della colonna protette con la chiave master della colonna precedente e i metadati delle chiavi master della colonna vecchie e nuove. Quindi, SSMS usa i metadati della chiave master della colonna per accedere all'archivio dati contenente la chiave master della colonna precedente e decrittografare le chiavi di crittografia della colonna. Successivamente, SSMS accede all'archivio chiavi contenente la nuova chiave master della colonna per produrre un nuovo set di valori crittografati delle chiavi di crittografia della colonna e quindi aggiunge i nuovi valori ai metadati generando ed emettendo istruzioni [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) .

> [!NOTE]
> Verificare che ogni chiave di crittografia della colonna, crittografata con la chiave master della colonna precedente, non sia crittografata con altre chiavi master della colonna. In altre parole, ogni chiave di crittografia della colonna interessata dalla rotazione deve avere un solo valore crittografato nel database. Se una chiave di crittografia della colonna interessata contiene più di un valore crittografato, è necessario rimuovere il valore prima di procedere con la rotazione (vedere il *passaggio 4* per la rimozione di un valore crittografato da una chiave di crittografia della colonna).

### <a name="step-3-configure-your-applications-with-the-new-column-master-key"></a>Passaggio 3: Configurare le applicazioni con la nuova chiave master della colonna

In questo passaggio è necessario verificare che tutte le applicazioni client in uso che eseguono query nelle colonne del database protette dalla chiave master della colonna in fase di rotazione siano in grado di accedere alla nuova chiave master della colonna, ad esempio le colonne del database crittografate con una chiave di crittografia della colonna crittografata con la chiave master della colonna. Questo passaggio dipende dal tipo di archivio chiavi in cui si trova la nuova chiave master della colonna. Ad esempio:

- Se la nuova chiave master della colonna è un certificato archiviato nell'archivio certificati di Windows, è necessario distribuire il certificato nella posizione dell'archivio dei certificati (*Utente corrente* o *Computer locale*) e nella posizione specificata nel percorso della chiave master della colonna nel database. L'applicazione deve poter accedere al certificato:
  - Se il certificato viene archiviato nel percorso dell'archivio certificati *Utente corrente*, deve essere importato nell'archivio Utente corrente dell'identità Windows dell'applicazione (utente).
  - Se il certificato viene archiviato nel percorso dell'archivio certificati *Computer locale*, l'identità Windows dell'applicazione deve disporre dell'autorizzazione per accedere al certificato.
- Se la nuova chiave master della colonna viene archiviata nell'insieme di credenziali delle chiavi di Microsoft Azure, deve essere implementata per l'autenticazione in Azure e deve essere autorizzata ad accedere alla chiave.

Per informazioni dettagliate, vedere [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!NOTE]
> A questo punto della rotazione, sia la nuova chiave master della colonna che quella precedente sono valide e possono essere usate per accedere ai dati.

### <a name="step-4-clean-up-column-encryption-key-values-encrypted-with-the-old-column-master-key"></a>Passaggio 4: Eseguire la pulizia dei valori della chiave di crittografia della colonna crittografati con la chiave master della colonna precedente

Dopo aver configurato tutte le applicazioni per l'uso con la nuova chiave master della colonna, rimuovere dal database i valori delle chiavi di crittografia della colonna, crittografate con la chiave master della colonna *precedente* . La rimozione dei valori precedenti predispone il sistema per la rotazione successiva. Tenere presente che ogni chiave di crittografia della colonna, protetta con una chiave master della colonna da ruotare, deve avere esattamente un valore crittografato.

Un altro motivo per cui è necessario pulire il valore precedente prima di archiviare o rimuovere la chiave master della colonna precedente riguarda le prestazioni: quando si eseguono query su una colonna crittografata, un driver client abilitato per la crittografia sempre attiva potrebbe tentare di decrittografare due valori, il valore precedente e quello nuovo. Il driver non riconosce quale delle due chiavi master della colonna sia valida nell'ambiente dell'applicazione, quindi recupera entrambi i valori dal server. Se la decrittografia di uno dei valori non va a buon fine in ragione del fatto che il valore è protetto con una chiave master della colonna non disponibile (ad esempio, la chiave master della colonna precedente rimossa dall'archivio), il driver tenterà di decrittografare un altro valore usando la nuova chiave master della colonna.

> [!WARNING]
> Se si rimuove il valore di una chiave di crittografia della colonna prima che la relativa chiave master della colonna diventi disponibile per un'applicazione, l'applicazione non sarà più in grado di decrittografare la colonna del database.

1.  In **Esplora oggetti** passare alla cartella **Sicurezza>Chiavi con crittografia sempre attiva** e individuare la chiave master della colonna esistente da sostituire.
2.  Fare clic con il pulsante destro del mouse sulla chiave master della colonna esistente e selezionare **Pulizia**.
3.  Verificare l'elenco dei valori delle chiavi di crittografia della colonna da rimuovere.
4.  Fare clic su **OK**.

SQL Server Management Studio rilascerà istruzioni [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) per eliminare i valori crittografati delle chiavi di crittografia della colonna crittografati con la chiave master della colonna precedente.

### <a name="step-5-delete-metadata-for-your-old-column-master-key"></a>Passaggio 5: Eliminare i metadati per la chiave master della colonna precedente

Se si sceglie di rimuovere la definizione della chiave master della colonna precedente dal database, eseguire la procedura descritta di seguito.

1. In **Esplora oggetti** passare alla cartella **Sicurezza>Chiavi con crittografia sempre attiva>Chiavi master della colonna** e individuare la chiave master della colonna precedente da rimuovere dal database.
2. Fare clic con il pulsante destro del mouse sulla chiave master della colonna precedente e selezionare **Elimina**. In questo modo viene generata e rilasciata un'istruzione [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) per rimuovere i metadati della colonna chiave master.
3. Fare clic su **OK**.

> [!NOTE]
> Si consiglia di non eliminare definitivamente la chiave master precedente della colonna dopo la rotazione. È opportuno invece conservare la chiave master precedente della colonna nell'archivio chiavi corrente o archiviarla in un altro posto sicuro. Se si ripristina il database da un file di backup creato prima della configurazione della nuova chiave master della colonna, sarà necessaria la chiave precedente per accedere ai dati.

### <a name="permissions-for-rotating-column-master-key"></a>Autorizzazioni per la rotazione della chiave master della colonna

La rotazione di una chiave master della colonna richiede le seguenti autorizzazioni di database:

- **ALTER ANY COLUMN MASTER KEY**: richiesta per creare i metadati per la nuova chiave master della colonna ed eliminare i metadati per la chiave master della colonna precedente.
- **ALTER ANY COLUMN ENCRYPTION KEY**: richiesta per modificare i metadati della chiave di crittografia della colonna (aggiunta di nuovi valori crittografati).

Sono necessarie anche le autorizzazioni per l'archivio chiavi per poter accedere sia alla chiave master della colonna precedente che alla nuova chiave master della colonna nei rispettivi archivi chiavi. Per informazioni dettagliate sulle autorizzazioni dell'archivio chiavi necessarie per le operazioni di gestione delle chiavi, vedere Creare e archiviare chiavi master della colonna [per Always Encrypted](create-and-store-column-master-keys-always-encrypted.md) e trovare una sezione pertinente per l'archivio chiavi.

<a name="rotatecek"></a> 
## <a name="rotate-column-encryption-keys"></a>Ruotare le chiavi di crittografia della colonna

La rotazione di una chiave di crittografia della colonna comporta la decrittografia dei dati in tutte le colonne crittografate con la chiave da ruotare e la nuova crittografia dei dati che usano la nuova chiave di crittografia della colonna.

>[!NOTE]
> La rotazione di una chiave di crittografia della colonna può richiedere molto tempo, se le tabelle contenenti le colonne crittografate con la chiave da ruotare sono di grandi dimensioni. Durante la nuova crittografia dei dati, le applicazioni non possono scrivere nelle tabelle interessate. Pertanto, l'organizzazione deve pianificare con molta attenzione una rotazione della chiave di crittografia della colonna.
Per ruotare una chiave di crittografia della colonna, usare la procedura guidata di Always Encrypted.

1.  Aprire la procedura guidata per il database: fare clic con il pulsante destro del mouse sul database, scegliere **Attività**, quindi fare clic su **Crittografa colonne**.
2.  Leggere la pagina **Introduzione** , quindi fare clic su **Avanti**.
3.  Nella pagina **Selezione colonne** espandere le tabelle e individuare tutte le colonne da sostituire che attualmente vengono crittografate con la chiave di crittografia della colonna precedente.
4.  Per ogni colonna crittografata con la chiave di crittografia precedente, impostare **Chiave di crittografia** su una nuova chiave generata automaticamente. **Nota:** in alternativa è possibile creare una nuova chiave di crittografia della colonna prima di eseguire la procedura guidata. Vedere [Effettuare il provisioning delle chiavi di crittografia di colonna con la finestra di dialogo Nuova chiave di crittografia della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).
5.  Nella pagina **Configurazione chiave master** , selezionare un percorso per archiviare la nuova chiave, selezionare un'origine della chiave master e quindi fare clic su **Avanti**. **Nota:** se si usa una chiave di crittografia della colonna già esistente (non una generata automaticamente), non deve essere eseguita alcuna azione in questa pagina.
6.  Nella pagina **Convalida** scegliere se eseguire lo script immediatamente o creare uno script di PowerShell, quindi fare clic su **Avanti**.
7.  Nella pagina **Riepilogo** esaminare le opzioni selezionate, quindi fare clic su **Fine** e chiudere la procedura guidata quando viene completata.
8.  In **Esplora oggetti** passare alla cartella **Sicurezza/Chiavi con crittografia sempre attiva/Chiavi di crittografia della colonna** e individuare la chiave di crittografia della colonna precedente da rimuovere dal database. Fare clic sulla chiave con il pulsante destro del mouse e selezionare **Elimina**.

### <a name="permissions-for-rotating-column-encryption-keys"></a>Autorizzazioni per la rotazione delle chiavi di crittografia della colonna

La rotazione di una chiave master della colonna richiede le autorizzazioni di database seguenti: **ALTER ANY COLUMN MASTER KEY**: richiesta se si usa una nuova chiave di crittografia della colonna generata automaticamente. Verranno generati anche una nuova chiave master della colonna e i relativi metadati.
**ALTER ANY COLUMN ENCRYPTION KEY**: richiesta per aggiungere metadati per la nuova chiave di crittografia della colonna.

Sono necessarie anche le autorizzazioni dell'archivio chiavi per poter accedere alle chiavi master della colonna sia per la chiave di crittografia nuova che per quella precedente. Per informazioni dettagliate sulle autorizzazioni dell'archivio chiavi necessarie per le operazioni di gestione delle chiavi, vedere Creare e archiviare chiavi master della colonna [per Always Encrypted](create-and-store-column-master-keys-always-encrypted.md) e trovare una sezione pertinente per l'archivio chiavi.

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configurare Always Encrypted usando SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurare Always Encrypted tramite PowerShell](configure-always-encrypted-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
