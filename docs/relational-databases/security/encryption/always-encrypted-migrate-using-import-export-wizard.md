---
description: Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server
title: Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234dabee531f85b4e3f8a687067dee9f91c96b33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345494"
---
# <a name="migrate-data-to-or-from-columns-using-always-encrypted-with-sql-server-import-and-export-wizard"></a>Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Lo strumento [Importazione/Esportazione guidata SQL Server](../../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) consente di copiare dati da un'origine a una destinazione. Questo documento descrive come usare l'Importazione/Esportazione guidata SQL Server se un'origine e/o una destinazione è un database di SQL Server che contiene colonne protette con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

## <a name="migration-scenarios"></a>Scenari di migrazione
Con l'Importazione/Esportazione guidata SQL Server è possibile implementare gli scenari seguenti per la migrazione di dati da e verso colonne crittografate.

### <a name="encrypt-plaintext-data-on-migration"></a>Crittografare i dati in testo non crittografato durante la migrazione
Se l'origine dati contiene dati in testo non crittografato e la destinazione è un database di SQL Server contenente colonne crittografate, è possibile usare l'Importazione/Esportazione guidata SQL Server per recuperare i dati in testo non crittografato dall'origine, crittografarli e quindi copiarli nelle colonne crittografate del database di destinazione. Per questo scenario di migrazione, l'origine dati può essere qualsiasi archivio dati supportato dall'Importazione/Esportazione guidata SQL Server. Ad esempio, un file, un database di SQL Server o un database in un altro sistema di database.

Per assicurarsi che l'Importazione/Esportazione guidata SQL Server possa crittografare i dati, è necessario abilitare Always Encrypted per la connessione al database di destinazione, oltre che avere accesso alle chiavi che proteggono i dati nelle colonne del database di destinazione. Per altre informazioni, vedere [Abilitare e disabilitare Always Encrypted per una connessione di database](#enable-and-disable-always-encrypted-for-a-database-connection) e [Autorizzazioni per crittografare o decrittografare i dati durante la migrazione](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="decrypt-encrypted-data-on-migration"></a>Decrittografare i dati crittografati durante la migrazione
Se si sta eseguendo la migrazione di dati archiviati in colonne di database crittografate in un database di SQL Server, è possibile configurare l'Importazione/Esportazione guidata SQL Server in modo da decrittografare i dati e quindi copiarli in una destinazione, che può essere qualsiasi archivio dati di SQL Server L'Importazione/Esportazione guidata supporta, ad esempio, un file, un database di SQL Server o un database in un altro sistema di database.

Per assicurarsi che l'Importazione/Esportazione guidata SQL Server possa decrittografare i dati, è necessario abilitare Always Encrypted per la connessione al database di origine, oltre che avere accesso alle chiavi che proteggono i dati nelle colonne del database di origine. Per altre informazioni, vedere [Abilitare e disabilitare Always Encrypted per una connessione di database](#enable-and-disable-always-encrypted-for-a-database-connection) e [Autorizzazioni per crittografare o decrittografare i dati durante la migrazione](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="re-encrypt-data-on-migration"></a>Crittografare nuovamente i dati durante la migrazione
Se si stanno copiando i dati dalle colonne crittografate di un database di SQL Server di origine alle colonne crittografate nello stesso o in un altro database di SQL Server, è possibile configurare l'Importazione/Esportazione guidata SQL Server in modo da decrittografare i dati dopo averli recuperati dall'origine e quindi crittografarli di nuovo prima di inserirli nelle colonne crittografate del database di destinazione. Usare questo metodo se lo schema delle colonne di destinazione, ad esempio i tipi di dati, i tipi di crittografia e le chiavi di crittografia delle colonne, è diverso dallo schema delle colonne di origine.

Per assicurarsi che l'Importazione/Esportazione guidata SQL Server possa crittografare e decrittografare i dati, è necessario abilitare Always Encrypted per la connessione al database di destinazione e per la connessione al database di destinazione, oltre che avere accesso alle chiavi che proteggono i dati nelle colonne di entrambi i database. Per altre informazioni, vedere [Abilitare e disabilitare Always Encrypted per una connessione di database](#enable-and-disable-always-encrypted-for-a-database-connection) e [Autorizzazioni per crittografare o decrittografare i dati durante la migrazione](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="keep-data-encrypted-during-migration"></a>Mantenere i dati crittografati durante la migrazione
Se si stanno copiando i dati dalle colonne crittografate di un database di SQL Server di origine alle colonne crittografate nello stesso o in un altro database di SQL Server e le colonne di destinazione usano **esattamente** lo stesso schema delle colonne di destinazione (inclusi tipi di dati, tipi di crittografia e chiavi di crittografia delle colonne), è possibile configurare l'Importazione/Esportazione guidata SQL Server in modo da recuperare il testo crittografato dalle colonne di origine e inserire i dati crittografati nelle colonne crittografate del database di SQL Server di destinazione. 

Per questo scenario, è possibile usare qualsiasi provider di dati che supporti SQL Server per connettersi al database di SQL Server di origine o di destinazione. Se si usa un provider che supporta Always Encrypted per la connessione al database di destinazione, è necessario assicurarsi che Always Encrypted sia disabilitato per la connessione di database. Per altre informazioni, vedere [Abilitare e disabilitare Always Encrypted per una connessione di database](#enable-and-disable-always-encrypted-for-a-database-connection).

È anche necessario assicurarsi che l'entità di database (utente) usata dall'Importazione/Esportazione guidata SQL Server per connettersi al database di destinazione sia configurata con l'opzione `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` impostata su `ON`. Questa opzione disattiva i controlli sui metadati di crittografia nel server nelle operazioni di copia bulk, il che consente alla procedura guidata di inserire in blocco i dati crittografati nel database di destinazione senza decrittografarli. Per altre informazioni, vedere [Caricamento bulk di dati crittografati in colonne tramite Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="enable-and-disable-always-encrypted-for-a-database-connection"></a>Abilitare e disabilitare Always Encrypted per una connessione di database
Se lo scenario di migrazione richiede che l'Importazione/Esportazione guidata SQL Server sia in grado di crittografare e/o decrittografare i dati, è necessario configurare la connessione al database di SQL Server di origine e/o la connessione al database di SQL Server di destinazione usando un provider di dati che supporti Always Encrypted. È anche necessario abilitare Always Encrypted per la connessione al database di origine e/o di destinazione.

Se non è necessario che la procedura guidata crittografi o decrittografi i dati di una connessione, per tale connessione è possibile usare qualsiasi provider di dati.

I provider di dati seguenti nell'Importazione/Esportazione guidata SQL Server supportano Always Encrypted.

- Provider di dati .NET Framework per SQL Server
  - Verificare che il computer in cui viene eseguita la procedura guidata usi .NET Framework 4.6.1 o versione successiva.
  - Per abilitare Always Encrypted per una connessione, impostare `Column Encryption Setting` su `Enabled` nelle proprietà di connessione. Per disabilitare Always Encrypted, impostare `Column Encryption Setting` su `Disabled`. Per altre informazioni, vedere [Connettersi a SQL Server con il provider di dati .NET Framework per SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server) e [Abilitazione di Always Encrypted per le query dell'applicazione](develop-using-always-encrypted-with-net-framework-data-provider.md#enabling-always-encrypted-for-application-queries).
- Provider di dati .NET Framework per ODBC.
  - Installare Microsoft ODBC Driver 13.1 o versione successiva.
    - Per abilitare Always Encrypted per una connessione, impostare `Column Encryption` su `Enabled` nelle proprietà di connessione. Per disabilitare Always Encrypted, impostare `Column Encryption` su `Disabled`. Per altre informazioni, vedere [Connettersi a SQL Server con il driver ODBC per SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-odbc-driver-for-sql-server) e [Abilitazione di Always Encrypted in un'applicazione ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-in-an-odbc-application).

## <a name="permissions-for-encrypting-or-decrypting-data-during-migration"></a>Autorizzazioni per crittografare o decrittografare i dati durante la migrazione

Per crittografare o decrittografare i dati archiviati in un database di SQL Server di origine o destinazione, sono necessarie le autorizzazioni *VIEW ANY COLUMN MASTER KEY DEFINITION* e *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* nel database di origine.

È anche necessario accedere alle chiavi master di colonna configurate per le colonne in cui sono archiviati i dati da crittografare o decrittografare:

- **Archivio certificati - Computer locale**: è necessario avere accesso in lettura al certificato usato come chiave master della colonna o essere l'amministratore del computer.
- **Azure Key Vault**: sono necessarie le autorizzazioni _get_, _unwrapKey_ e _verify_ per l'insieme di credenziali contenente la chiave master della colonna.
- **Provider dell'archivio chiavi (CNG)**: l'autorizzazione e le credenziali necessarie; potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione KSP.
- **Provider del servizio di crittografia (CAPI)**: l'autorizzazione e le credenziali necessarie; potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione CSP.
Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](always-encrypted-database-engine.md)
- [Esportare e importare database usando Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Eseguire il backup e il ripristino di database usando Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Caricamento bulk di dati crittografati in colonne tramite Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)
