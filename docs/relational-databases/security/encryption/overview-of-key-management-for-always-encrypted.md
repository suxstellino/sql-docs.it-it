---
title: Panoramica della gestione delle chiavi per Always Encrypted | Microsoft Docs
description: 'Informazioni su come gestire i due tipi di chiavi crittografiche usati da Always Encrypted per proteggere i dati in SQL Server: chiave di crittografia della colonna e chiave master della colonna.'
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 07a305b1-4110-42f0-b7aa-28a4e32e912a
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234f6233e2f6b024e6ad74a82f403e3df7db9a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340182"
---
# <a name="overview-of-key-management-for-always-encrypted"></a>Panoramica della gestione delle chiavi per Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]


Per proteggere i dati,[Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) usa due tipi di chiavi crittografiche: una per crittografare i dati e un'altra per crittografare la chiave che crittografa i dati. La chiave di crittografia della colonna crittografa i dati, la chiave master della colonna crittografa la chiave di crittografia della colonna. Questo articolo offre una panoramica dettagliata della gestione di queste chiavi di crittografia.  

Quando si parla di chiavi e di gestione delle chiavi Always Encrypted, è importante comprendere la differenza tra le chiavi crittografiche vere e proprie e gli oggetti metadati che *descrivono* le chiavi. I termini **chiave di crittografia della colonna** e **chiave master della colonna** vengono usati per fare riferimento alle chiavi crittografiche vere e proprie, mentre i termini **metadati della chiave di crittografia della colonna** e **metadati della chiave master della colonna** vengono usati per fare riferimento alle *descrizioni* delle chiavi Always Encrypted nel database.

- Le ***chiavi di crittografia della colonna*** sono chiavi di crittografia contenuto usate per crittografare i dati. Come suggerito dal nome, le chiavi di crittografia della colonna sono usate per crittografare i dati nelle colonne del database. Per crittografare una o più colonne è possibile usare la stessa chiave di crittografia della colonna oppure più chiavi, a seconda dei requisiti dell'applicazione. Le chiavi di crittografia della colonna sono a loro volta crittografate. Nel database sono archiviati solo i valori crittografati delle chiavi di crittografia della colonna, all'interno dei metadati delle chiavi di crittografia della colonna stesse. I metadati delle chiavi di crittografia della colonna sono archiviati nelle viste del catalogo [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) e [sys.column_encryption_key_values (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) . Le chiavi di crittografia della colonna usate con l'algoritmo AES-256 sono lunghe 256 bit.


- Le ***chiavi master della colonna*** proteggono le chiavi usate per crittografare le chiavi di crittografia della colonna. Le chiavi master della colonna devono essere archiviate in un archivio chiavi attendibile, ad esempio l'archivio certificati Windows, l'insieme di credenziali delle chiavi di Azure o un modulo di sicurezza hardware. Il database contiene solo i metadati relativi alle chiavi master della colonna (il tipo di archivio chiavi e la posizione). I metadati delle chiavi master della colonna sono archiviati nella vista del catalogo [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md) .  

È importante notare che i metadati delle chiavi nel sistema del database non contengono chiavi master della colonna o chiavi di crittografia della colonna sotto forma di testo non crittografato. Il database contiene solo informazioni sul tipo e sulla posizione delle chiavi master della colonna e i valori crittografati delle chiavi di crittografia della colonna. Ciò significa che le chiavi in testo non crittografato non sono mai esposte al sistema di database. Questo garantisce che i dati protetti con Always Encrypted sono al sicuro anche se il sistema di database viene compromesso. Per assicurarsi che il sistema del database non possa accedere alle chiavi in testo non crittografato, eseguire gli strumenti di gestione delle chiavi in un computer diverso da quello che ospita il database. Per i dettagli, vedere la sezione [Considerazioni sulla sicurezza per la gestione delle chiavi](#security-considerations-for-key-management) più avanti.

Poiché il database contiene solo dati crittografati (all'interno di colonne protette con Always Encrypted) e non può accedere alle chiavi in testo non crittografato, non può decrittografare i dati. Ciò significa che una query sulle colonne Always Encrypted restituirà semplicemente valori crittografati. Le applicazioni client che devono crittografare o decrittografare i dati protetti, quindi, devono essere in grado di accedere alla chiave master della colonna e alle chiavi di crittografia della colonna corrispondenti. Per informazioni dettagliate, vedere [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md).



## <a name="key-management-tasks"></a>Attività di gestione delle chiavi

Il processo di gestione delle chiavi può essere suddiviso nelle attività di alto livello seguenti:

- **Provisioning della chiave** : creazione delle chiavi fisiche in un archivio chiavi attendibile, ad esempio nell'archivio certificati Windows, nell'insieme di credenziali delle chiavi di Azure o in un modulo di sicurezza hardware, crittografia delle chiavi di crittografia della colonna con le chiavi master della colonna e creazione dei metadati per entrambi i tipi di chiavi nel database.

- **Rotazione delle chiavi** : sostituzione periodica di una chiave esistente con una nuova. Può essere necessario ruotare una chiave se questa è stata compromessa oppure per conformità ai criteri e alle normative dell'organizzazione che impongono la rotazione delle chiavi crittografiche. 


## <a name="key-management-roles"></a><a name="KeyManagementRoles"></a> Ruoli di gestione delle chiavi

Per la gestione delle chiavi Always Encrypted esistono due ruoli utente distinti: gli amministratori della sicurezza e gli amministratori del database:

- **Amministratore della sicurezza** : genera le chiavi di crittografia della colonna e le chiavi master della colonna, oltre agli archivi delle chiavi contenenti le chiavi master della colonna. Per eseguire queste attività, un amministratore della sicurezza deve essere in grado di accedere alle chiavi e all'archivio delle chiavi, ma non ha bisogno di accedere al database.
- **Amministratore del database**: gestisce i metadati relativi alle chiavi nel database. Per eseguire attività di gestione delle chiavi, un amministratore del database deve essere in grado di gestire i metadati delle chiavi nel database, ma non ha bisogno di accedere alle chiavi o all'archivio delle chiavi che contiene le chiavi master della colonna.

Considerando i ruoli sopra descritti, le attività di gestione delle chiavi per Always Encrypted possono essere eseguite in due modi diversi: *con la separazione dei ruoli* e *senza la separazione dei ruoli*. A seconda delle esigenze dell'organizzazione è possibile selezionare il processo di gestione delle chiavi che meglio si adatta alle proprie esigenze.

## <a name="managing-keys-with-role-separation"></a>Gestione delle chiavi con la separazione dei ruoli
Se le chiavi Always Encrypted vengono gestite con la separazione dei ruoli, il ruolo di amministratore della sicurezza e quello di amministratore del database all'interno dell'organizzazione vengono assunti da persone diverse. Un processo di gestione delle chiavi con separazione dei ruoli garantisce che gli amministratori del database non abbiano accesso alle chiavi o agli archivi che le contengono e che gli amministratori della sicurezza non abbiano accesso al database, che contiene dati sensibili. La gestione delle chiavi con la separazione dei ruoli è consigliata se l'obiettivo è di assicurarsi che gli amministratori del database all'interno dell'organizzazione non possano accedere ai dati sensibili. 

**Nota:** gli amministratori della sicurezza generano e usano le chiavi di testo non crittografato. Non devono quindi mai eseguire le loro attività da un computer che ospiti un sistema di database o a cui possa accedere un amministratore del database o qualsiasi altro utente che possa rappresentare un potenziale antagonista. 

## <a name="managing-keys-without-role-separation"></a>Gestione delle chiavi senza separazione dei ruoli
Se le chiavi Always Encrypted vengono gestite senza separazione dei ruoli, un unico utente può assumere entrambi i ruoli di amministratore della sicurezza e di amministratore del database. Questo implica che la persona deve essere in grado di raggiungere e gestire sia le chiavi e gli archivi delle chiavi che i metadati delle chiavi stesse. La gestione delle chiavi senza separazione dei ruoli è consigliata per le organizzazioni che usano il modello DevOps oppure se il database è ospitato nel cloud e l'obiettivo principale consiste nel limitare l'accesso ai dati sensibili agli amministratori del cloud escludendo gli amministratori di database locali.



## <a name="tools-for-managing-always-encrypted-keys"></a>Strumenti per la gestione delle chiavi Always Encrypted

Le chiavi Always Encrypted possono essere gestite tramite [SQL Server Management Studio (SSMS)](../../../ssms/sql-server-management-studio-ssms.md) e [PowerShell](../../../powershell/sql-server-powershell.md):

- **SQL Server Management Studio (SSMS)** : mette a disposizione finestre di dialogo e procedure guidate che consentono di combinare attività legate all'accesso all'archivio delle chiavi e all'accesso al database. SSMS quindi non supporta la separazione dei ruoli ma semplifica la configurazione delle chiavi. Per altre informazioni sulla gestione delle chiavi con SSMS, vedere:
    - [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
    - [Ruotare chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)

- **SQL Server PowerShell**: include cmdlet per la gestione delle chiavi Always Encrypted con e senza la separazione dei ruoli. Per altre informazioni, vedere:
    - [Configurare le chiavi Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)
    - [Ruotare le chiavi Always Encrypted con PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)


## <a name="security-considerations-for-key-management"></a>Considerazioni sulla sicurezza per la gestione delle chiavi

L'obiettivo principale di Always Encrypted è di garantire la sicurezza dei dati sensibili archiviati in un database anche se il sistema di database o il suo ambiente host viene compromesso. Ecco alcuni esempi di attacchi alla sicurezza in cui Always Encrypted consente di evitare perdite di dati sensibili:

- Un utente del database con privilegi elevati malintenzionato, ad esempio un amministratore di database, esegue query su colonne di dati sensibili.
- Un amministratore non autorizzato di un computer che ospita un'istanza di SQL Server analizza la memoria di un processo SQL Server o file di dump di processi SQL Server.
- Un operatore di centro dati malintenzionato esegue una query su un database di clienti, esamina i file di dump di SQL Server o esamina la memoria di un computer che ospita dati sui clienti nel cloud.
- Malware in esecuzione in un computer che ospita il database.

Per garantire che Always Encrypted sia efficace nella prevenzione di questi tipi di attacchi, il processo di gestione delle chiavi deve verificare le chiavi master della colonna e le chiavi di crittografia della colonna e deve controllare che le credenziali dell'archivio contenente le chiavi master della colonna non vengano mai rivelate a un potenziale utente malintenzionato. Ecco alcune indicazioni da seguire:

- Non generare mai le chiavi master della colonna o le chiavi di crittografia della colonna nel computer che ospita il database. Generare le chiavi in un computer separato dedicato alla gestione delle chiavi o nel computer che ospita le applicazioni che dovrebbero comunque avere accesso alle chiavi. Ciò significa che **non si devono mai eseguire strumenti usati per generare le chiavi nel computer che ospita il database** perché se riuscisse ad accedere a un computer usato per il provisioning o la gestione delle chiavi Always Encrypted, un utente malintenzionato potrebbe ottenere le chiavi, anche se queste compaiono nella memoria dello strumento solo per un breve periodo di tempo.
- Per assicurarsi che il processo di gestione delle chiavi non riveli inavvertitamente le chiavi master della colonna o le chiavi di crittografia della colonna, è fondamentale identificare potenziali antagonisti e possibili minacce alla sicurezza prima di definire e implementare un processo di gestione delle chiavi. Se ad esempio l'obiettivo è di verificare che gli amministratori del database non abbiano accesso ai dati sensibili, un amministratore del database non può essere incaricato della generazione delle chiavi. Un amministratore del database, tuttavia, *può* gestire i metadati delle chiavi nel database, poiché i metadati non contengono le chiavi sotto forma di testo non crittografato.

## <a name="next-steps"></a>Passaggi successivi
- [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
- [Creare e archiviare chiavi master della colonna per Always Encrypted](create-and-store-column-master-keys-always-encrypted.md)
- [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Esercitazione guidata su Always Encrypted (insieme di credenziali delle chiavi di Azure)](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)
- [Esercitazione guidata su Always Encrypted (archivio certificati Windows)](/azure/azure-sql/database/always-encrypted-certificate-store-configure)