---
title: SQL Server e chiavi di crittografia del database
description: Informazioni sulla chiave master del servizio e sulla chiave master del database usate dal motore di database di SQL Server per crittografare e proteggere i dati.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- keys [SQL Server], database encryption
ms.assetid: 15c0a5e8-9177-484c-ae75-8c552dc0dac0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 0ffb2adad7c1c5e74cc96e7baa5e4f37a2818674
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232158"
---
# <a name="sql-server-and-database-encryption-keys-database-engine"></a>Chiavi di crittografia del database e di SQL Server (Motore di database)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa le chiavi di crittografia per la protezione di dati, credenziali e informazioni di connessione archiviate in un database del server. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dispone di due tipi di chiavi: *simmetrica* e *asimmetrica*. Le chiavi simmetriche utilizzano la stessa password per crittografare e decrittografare i dati. Le chiavi asimmetriche usano una password per crittografare i dati (chiave *pubblica* ) e un'altra per decrittografare i dati (chiave *privata* ).  
  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le chiavi di crittografia sono costituite da una combinazione di chiavi pubbliche, private e simmetriche utilizzate per proteggere dati riservati. La chiave simmetrica viene creata durante l'inizializzazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando si avvia per la prima volta l'istanza [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La chiave è utilizzata da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per crittografare dati riservati archiviati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le chiavi pubblica e privata vengono create dal sistema operativo e sono utilizzate per proteggere la chiave simmetrica. Per ogni istanza [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che contiene dati riservati in un database viene creata una coppia di chiavi pubblica e privata.  
  
## <a name="applications-for-sql-server-and-database-keys"></a>Applicazioni per chiavi del SQL Server e del database  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] contiene due applicazioni principali per le chiavi: una *chiave master del servizio* (SMK) generata per un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e una *chiave master del database* (DMK) usata per un database.

### <a name="service-master-key"></a>Chiave master del servizio
  
 La chiave master del servizio è l'elemento radice della gerarchia di crittografia di SQL Server. La chiave master del servizio viene generata automaticamente la prima volta che viene avviata e usata l'istanza [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per crittografare la password, le credenziali o la chiave master del database di un server collegato in ogni database. La chiave SMK viene crittografata usando la chiave locale del computer che usa l'API Windows Data Protection (DPAPI). Il DPAPI usa una chiave derivata dalle credenziali Windows dell'account di servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e le credenziali del computer. La chiave master del servizio può essere decrittografata solo dall'account del servizio con cui è stata creata o da un'entità autorizzata ad accedere alle credenziali della macchina.

La chiave master del servizio può essere aperta solo dall'account di servizio Windows nel quale è stata creata oppure da un'entità con accesso sia al nome che alla password del'account di servizio.

 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] usa l'algoritmo di crittografia AES per proteggere la chiave master del servizio (SMK) e la chiave master del database (DMK). AES è un algoritmo di crittografia più recente rispetto a 3DES utilizzato nelle versioni precedenti. Dopo aver aggiornato un'istanza di [!INCLUDE[ssDE](../../../includes/ssde-md.md)] a [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] le chiavi SMK e DMK devono essere rigenerate per aggiornare le chiavi master ad AES. Per altre informazioni sulla rigenerazione della chiave SMK, vedere [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-service-master-key-transact-sql.md) e [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-master-key-transact-sql.md).

### <a name="database-master-key"></a>Chiave master del database
  
 La chiave master del database è una chiave simmetrica utilizzata per proteggere le chiavi private dei certificati e le chiavi asimmetriche presenti nel database. Si può utilizzare anche per crittografare dati, ma ha limitazioni di lunghezza che la rendono meno pratica per i dati rispetto all'utilizzo di una chiave simmetrica. Per abilitare la decrittografia automatica della chiave master del database, viene crittografata una copia della chiave usando la chiave SMK. Viene archiviata in entrambi i database dove è usata e nel database **master** di sistema.  
  
 La copia della DMK archiviata nel database **master** di sistema viene aggiornata automaticamente ogni qualvolta la DMK è modificata. È possibile modificare questa impostazione predefinita usando l'opzione **DROP ENCRYPTION BY SERVICE MASTER KEY** dell'istruzione **ALTER MASTER KEY** . Per aprire una chiave master non crittografata con la chiave master del servizio, è necessario usare l'istruzione **OPEN MASTER KEY** e una password.  
  
## <a name="managing-sql-server-and-database-keys"></a>Gestione delle chiavi del SQL Server e del database  
 La gestione delle chiavi di crittografia comprende la creazione di nuove chiavi di database, la creazione di un backup delle chiavi server e del database e informazioni su quando e come ripristinare, eliminare o modificare le chiavi.  
  
 Per la gestione delle chiavi simmetriche è possibile utilizzare gli strumenti inclusi in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per eseguire le operazioni seguenti:  
  
-   Backup di una copia delle chiavi del server e del database per poterle utilizzare per il recupero dell'installazione di un server di report o come parte di una migrazione pianificata.  
  
-   Ripristino di una chiave precedentemente salvata in un database. Consente a una nuova istanza del server l'accesso a dati esistenti che non erano stati crittografati in origine.  
  
-   Eliminazione dei dati crittografati in un database nel caso molto improbabile in cui non sia più possibile accedere a tali dati.  
  
-   Ricreazione delle chiavi e riesecuzione della crittografia dei dati nel caso molto improbabile in cui la chiave risulti compromessa. Come procedura di sicurezza consigliata, ricreare le chiavi periodicamente, ad esempio dopo qualche mese, per proteggere il server da attacchi informatici mirati alla decrittazione delle chiavi.  
  
-   Aggiunta o rimozione di un'istanza del server da una distribuzione del server con scalabilità orizzontale in cui più server condividono sia un unico database sia la chiave che consente la crittografia reversibile per quel database.  
  
## <a name="important-security-information"></a>Importanti informazioni relative alla sicurezza  
 L'accesso a oggetti protetti dalla chiave master del servizio richiede l'account di servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzato per creare la chiave o l'account del computer. Ovvero l'account del computer associato al sistema in cui è stata creata la chiave. È possibile modificare l'account del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]*o* l'account del computer senza perdere l'accesso alla chiave. Tuttavia, se si modificano entrambi, si perderà l'accesso alla chiave master del servizio. Se si verifica tale perdita senza disporre di uno di questi due elementi, non sarà possibile decrittografare i dati e gli oggetti crittografati usando la chiave originale.  
  
 Non è possibile ripristinare connessioni protette con la chiave master del servizio se non si dispone della stessa.  
  
 L'accesso a oggetti e dati protetti con la chiave master del database richiede solo la password utilizzata per proteggere la chiave.  
  
> [!CAUTION]  
>  Se si perde ogni accesso alle chiavi descritte in precedenza, si perderà l'accesso agli oggetti, alle connessioni e ai dati protetti da quelle chiavi. È possibile ripristinare la chiave master del servizio, come viene descritto nei collegamenti qui riportati, oppure è possibile ritornare al sistema di crittografa originale per recuperare l'accesso. Non è possibile recuperare l'accesso in altro modo.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Chiave master del servizio]()  
 Viene fornita una breve spiegazione della chiave master del servizio e delle procedure consigliate.  
  
 [Extensible Key Management &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
 Viene illustrata la modalità di utilizzo dei sistemi di gestione delle chiavi di terze parti con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="related-tasks"></a>Attività correlate  
 [Eseguire il backup della chiave master del servizio](../../../relational-databases/security/encryption/back-up-the-service-master-key.md)  
  
 [Ripristino della chiave master del servizio](../../../relational-databases/security/encryption/restore-the-service-master-key.md)  
  
 [Creazione della chiave master di un database](../../../relational-databases/security/encryption/create-a-database-master-key.md)  
  
 [Backup della chiave master di un database](../../../relational-databases/security/encryption/back-up-a-database-master-key.md)  
  
 [Ripristino di una chiave master del database](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
 [Creare chiavi simmetriche identiche su due server](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
 [Abilitare TDE in SQL Server con EKM](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
 [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
 [Crittografia di una colonna di dati](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)  
  
## <a name="related-content"></a>Contenuto correlato  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-service-master-key-transact-sql.md)  
  
 [Ripristino di una chiave master del database](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)   
 [Eliminare e ricreare chiavi di crittografia &#40;Gestione configurazione SSRS&#41;](../../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)   
 [Aggiungere e rimuovere le chiavi di crittografia per una distribuzione con scalabilità orizzontale &#40;Gestione configurazione SSRS&#41;](../../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)   
 [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
