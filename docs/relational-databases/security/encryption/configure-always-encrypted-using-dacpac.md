---
description: Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati
title: Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7a42972d4a0cd5e6b456d72ccdba6786ad66c734
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237105"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Un [pacchetto di applicazione livello dati (DAC)](../../data-tier-applications/data-tier-applications.md), noto anche come DACPAC, è un'unità portabile della distribuzione di database SQL Server che definisce tutti gli oggetti SQL Server, incluse le tabelle e le colonne delle tabelle. Quando si pubblica un pacchetto DACPAC in un database (ovvero, quando si aggiorna un database usando un pacchetto DACPAC), lo schema del database di destinazione viene aggiornato in modo da corrispondere allo schema del pacchetto DACPAC. È possibile pubblicare un pacchetto DACPAC seguendo la [procedura guidata Aggiorna applicazione di livello dati](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard) in SQL Server Management Studio, [PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) o [SqlPackage](../../../tools/sqlpackage/sqlpackage-publish.md).

In questo articolo vengono illustrate alcune considerazioni speciali per l'aggiornamento di un database quando il pacchetto DACPAC o/e il database di destinazione contiene colonne protette con [Always Encrypted](always-encrypted-database-engine.md). Se lo schema di crittografia per una colonna nel pacchetto DACPAC differisce dallo schema di crittografia per una colonna esistente nel database di destinazione, la pubblicazione di DACPAC comporta la crittografia, la decrittografia o la nuova crittografia dei dati archiviati nella colonna. Vedere la tabella seguente per i dettagli.

| Condizione|Azione|
|:---|:---|
|La colonna è crittografata nel file DACPAC e non è crittografata nel database.| I dati contenuti nella colonna verranno crittografati.|
|La colonna non è crittografata nel file DACPAC ed è crittografata nel database.| I dati presenti nella colonna verranno decrittografati ovvero la crittografia verrà rimossa dalla colonna.|
| La colonna è crittografata sia nel file DACPAC che nel database, ma la colonna nel file DACPAC usa un tipo diverso di crittografia e/o una chiave di crittografia della colonna diversa rispetto alla colonna corrispondente nel database.|I dati presenti nella colonna verranno decrittografati e di nuovo crittografati in modo da corrispondere alla configurazione di crittografia nel file DACPAC.|

La distribuzione di un pacchetto DAC può comportare anche la creazione o la rimozione di oggetti metadati per le chiavi master della colonna o le chiavi di crittografia della colonna per Always Encrypted.

## <a name="performance-considerations"></a>Considerazioni sulle prestazioni
Per eseguire operazioni di crittografia, lo strumento usato per distribuire un pacchetto DACPAC deve spostare i dati fuori dal database. Lo strumento crea una o più nuove tabelle con la configurazione di crittografia desiderata nel database, carica tutti i dati dalle tabelle originali, esegue le operazioni di crittografia richieste, carica i dati nelle nuove tabelle e quindi scambia le tabelle originali con le nuove tabelle. L'esecuzione di operazioni di crittografia può richiedere molto tempo. Durante questo periodo, il database non è disponibile per la scrittura di transazioni. 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Se si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] e l'istanza di SQL Server è configurata con un'enclave sicura, è possibile eseguire le operazioni di crittografia sul posto, senza trasferire i dati all'esterno del database. Vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md). La crittografia sul posto non è disponibile per le distribuzioni di pacchetti DACPAC.

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>Autorizzazioni per la pubblicazione di un pacchetto DAC se è configurata la funzionalità Always Encrypted

Per pubblicare un pacchetto DAC con Always Encrypted configurato nel file DACPAC o nel database di destinazione, potrebbero essere necessarie alcune o tutte le autorizzazioni indicate di seguito, in base alle differenze tra lo schema nel file DACPAC e lo schema del database di destinazione.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Se l'operazione di aggiornamento attiva un'operazione di crittografia dei dati, è necessario essere in grado di accedere alle chiavi master della colonna configurate per le colonne interessate:

- **Archivio certificati - Computer locale**: è necessario avere accesso in lettura al certificato usato come chiave master della colonna o essere l'amministratore del computer.
- **Azure Key Vault**: sono necessarie le autorizzazioni *create*, *get*, *unwrapKey*, *wrapKey*, *sign* e *verify* per l'insieme di credenziali contenente la chiave master della colonna.
- **Provider dell'archivio chiavi (CNG)** : l'autorizzazione e le credenziali obbligatorie potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione KSP.
- **Provider del servizio di crittografia (CAPI)** : potrebbero essere richieste l'autorizzazione e le credenziali obbligatorie quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione CSP.

Per altre informazioni, vedere [Creare e archiviare chiavi master della colonna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md). 

 
## <a name="next-steps"></a>Passaggi successivi
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>Vedere anche  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configurare Always Encrypted usando SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
 - [Configurare la crittografia delle colonne usando Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md)
 
