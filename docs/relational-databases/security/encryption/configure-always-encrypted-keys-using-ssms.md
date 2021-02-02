---
title: Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio | Microsoft Docs
description: Informazioni su come effettuare il provisioning di chiavi master della colonna e di chiavi di crittografia della colonna per Always Encrypted con SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f31a17d3f8bf2285bc41c7462deec4e53008efd1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237078"
---
# <a name="provision-always-encrypted-keys-using-sql-server-management-studio"></a>Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive la procedura necessaria per effettuare il provisioning di chiavi master della colonna e di chiavi di crittografia della colonna per Always Encrypted con [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Per una panoramica sulla gestione delle chiavi Always Encrypted, incluse indicazioni sulle procedure consigliate e importanti considerazioni sulla sicurezza, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="provisioncmk"></a>
## <a name="provision-column-master-keys-with-the-new-column-master-key-dialog"></a>Effettuare il provisioning delle chiavi master di colonna con la finestra di dialogo Nuova chiave master della colonna

La finestra di dialogo **Nuova chiave master della colonna** consente di generare una chiave master della colonna o selezionare una chiave esistente in un archivio chiavi e di creare i metadati della chiave master della colonna per la chiave selezionata o creata nel database.

1.  In **Esplora oggetti** passare alla cartella **Sicurezza > Chiavi con crittografia sempre attiva** del database.
2.  Fare clic con il pulsante destro del mouse sulla cartella **Chiavi master della colonna** e selezionare **Nuova chiave master della colonna**. 
3.  Nella finestra di dialogo **Nuova chiave master della colonna** immettere il nome dell'oggetto metadati della chiave master della colonna.
4.  Selezionare un archivio chiavi:
    - **Archivio certificati - Utente corrente**: indica il percorso dell'archivio certificati Utente corrente nell'archivio certificati di Windows, ovvero nell'archivio personale. 
    - **Archivio certificati - Computer locale**: indica il percorso dell'archivio certificati Computer locale nell'archivio certificati di Windows. 
    - **Azure Key Vault**: è necessario accedere ad Azure facendo clic su **Accedi**. Dopo aver eseguito l'accesso, sarà possibile selezionare uno degli abbonamenti di Azure e un insieme di credenziali delle chiavi.
    - **Provider dell'archivio chiavi (KSP)** : indica un archivio chiavi che è accessibile tramite un provider dell'archivio chiavi (KSP) che implementa l'API CNG (Cryptography Next Generation). In genere, questo tipo di archivio è un modulo di protezione hardware (HSM). Dopo aver selezionato questa opzione, è necessario selezionare un provider dell'archivio chiavi. Viene selezionato per impostazione predefinita il **provider dell'archivio chiavi del software Microsoft** . Per usare una chiave master della colonna archiviata in un HSM, selezionare un provider dell'archivio chiavi per il dispositivo, che deve essere installato e configurato nel computer prima di aprire la finestra di dialogo.
    -   **Provider del servizio di crittografia (CSP)** : un archivio chiavi accessibile attraverso un provider del servizio di crittografia (CSP) che implementa l'API di crittografia (CAPI). In genere, questo archivio è un modulo di protezione hardware (HSM). Dopo aver selezionato questa opzione, è necessario selezionare un provider del servizio di crittografia.  Per usare una chiave master della colonna archiviata in un HSM, selezionare un provider del servizio di crittografia per il dispositivo, che deve essere installato e configurato nel computer prima di aprire la finestra di dialogo.
    
    > [!NOTE]
    > Poiché CAPI è un'API deprecata, il provider del servizio di crittografia è disabilitato per impostazione predefinita. È possibile abilitarlo creando il valore DWORD CAPI Provider Enabled sotto la chiave **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** nel Registro di sistema di Windows e impostando il valore su 1. È consigliabile usare CNG anziché CAPI, a meno che l'archivio chiavi non supporti CNG.
   
    Per altre informazioni sugli archivi di chiavi sopra indicati, vedere [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

5. Se si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] e l'istanza di SQL Server è stata configurata con un enclave sicuro, è possibile selezionare la casella di controllo **Consenti calcoli enclave** per abilitare la chiave per l'enclave. Per informazioni dettagliate, vedere [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md). 

    > [!NOTE]
    > La casella di controllo **Consenti calcoli enclave** non viene visualizzata se l'istanza di SQL Server non è stata correttamente configurata con un enclave sicuro.

6.  Selezionare una chiave esistente nell'archivio chiavi oppure fare clic sul pulsante **Genera chiave** o **Genera certificato** per creare una chiave nell'archivio chiavi. 
7.  Fare clic su **OK** e la nuova chiave verrà visualizzata nell'elenco. 

Dopo aver completato la finestra di dialogo, SQL Server Management Studio crea i metadati per la chiave master della colonna nel database. Nella finestra di dialogo viene generata e rilasciata un'istruzione [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) .

::: moniker range=">=sql-server-ver15"

Se si sta configurando una chiave master della colonna abilitata per l'enclave, SSMS usa la chiave anche per firmare i metadati. 

::: moniker-end

### <a name="permissions-for-provisioning-a-column-master-key"></a>Autorizzazioni per il provisioning di una chiave master della colonna

È necessaria l'autorizzazione *ALTER ANY COLUMN MASTER KEY* nel database perché nella finestra di dialogo venga creata una chiave master della colonna. Per usare la finestra di dialogo per la creazione di una nuova chiave master della colonna o per usare una chiave esistente in un archivio chiavi, è possibile che siano necessarie le autorizzazioni per l'archivio chiavi e/o per la chiave:
- **Archivio certificati - Computer locale**: è necessario avere accesso in lettura al certificato usato come chiave master della colonna o essere l'amministratore del computer.
- **Azure Key Vault** - Sono necessarie le autorizzazioni *get* e *list* per selezionare e usare una chiave e l'autorizzazione *create* per creare una nuova chiave. Per configurare una chiave master della colonna abilitata per l'enclave, è necessario anche l'autorizzazione *sign* per generare una firma dei metadati della chiave.
- **Provider dell'archivio chiavi (CNG)** : l'autorizzazione e le credenziali obbligatorie potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione KSP.
- **Provider del servizio di crittografia (CAPI)** : potrebbero essere richieste l'autorizzazione e le credenziali obbligatorie quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione CSP.

Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="provisioncek"></a> 
## <a name="provision-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Effettuare il provisioning di chiavi di crittografia di colonna con la finestra di dialogo Nuova chiave di crittografia della colonna

La finestra di dialogo **Nuova chiave di crittografia della colonna** consente di generare una chiave di crittografia della colonna, crittografarla con una chiave master della colonna e creare i metadati della chiave di crittografia della colonna nel database.

1.  In **Esplora oggetti** passare alla cartella **Sicurezza/Chiavi con crittografia sempre attiva** del database.
2.  Fare clic con il pulsante destro del mouse sulla cartella **Chiavi di crittografia della colonna** e selezionare **Nuova chiave di crittografia della colonna**. 
3.  Nella finestra di dialogo **Nuova chiave di crittografia della colonna** immettere il nome dell'oggetto metadati della chiave di crittografia della colonna.
4.  Selezionare un oggetto metadati che rappresenta la chiave master della colonna nel database.
5.  Fare clic su **OK**. 

Dopo aver completato la finestra di dialogo, SQL Server Management Studio genera una nuova chiave di crittografia della colonna e consente di recuperare i metadati per la chiave master della colonna selezionata dal database. SSMS userà quindi i metadati della chiave master della colonna per contattare l'archivio chiavi contenente la chiave master della colonna e crittografare la chiave di crittografia della colonna. SSMS creerà infine i dati dei metadati per la nuova crittografia di colonna nel database generando ed emettendo un'istruzione [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md).

### <a name="permissions-for-provisioning-a-column-encryption-key"></a>Autorizzazioni per il provisioning di una chiave di crittografia della colonna

Sono necessarie le autorizzazioni di database *ALTER ANY COLUMN ENCRYPTION KEY* e *VIEW ANY COLUMN MASTER KEY DEFINITION* per creare nella finestra di dialogo i metadati della chiave di crittografia della colonna e accedere ai metadati della chiave master della colonna.
Per accedere a un archivio chiavi e usare la chiave master della colonna, è possibile richiedere le autorizzazioni per l'archivio chiavi e/o la chiave:
- **Archivio certificati - Computer locale**: è necessario avere accesso in lettura al certificato usato come chiave master della colonna o essere l'amministratore del computer.
- **Azure Key Vault**: sono necessarie le autorizzazioni *get*, *unwrapKey*, *wrapKey*, *sign* e *verify* per l'insieme di credenziali contenente la chiave master della colonna.
- **Provider dell'archivio chiavi (CNG)** : l'autorizzazione e le credenziali obbligatorie potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione KSP.
- **Provider del servizio di crittografia (CAPI)** : potrebbero essere richieste l'autorizzazione e le credenziali obbligatorie quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione CSP.

Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="provision-always-encrypted-keys-using-the-always-encrypted-wizard"></a>Effettuare il provisioning di chiavi Always Encrypted con la relativa procedura guidata

Lo strumento [Procedura guidata Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) consente di eseguire la crittografia, la decrittografia e una nuova crittografia delle colonne di database selezionate. Può usare chiavi già configurate ma, al tempo stesso, consente anche di generare una nuova chiave master della colonna e una nuova crittografia di colonna. 

## <a name="next-steps"></a>Passaggi successivi
- [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
- [Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati](configure-always-encrypted-using-dacpac.md)
- [Ruotare chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)
- [Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Configurare Always Encrypted usando SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
