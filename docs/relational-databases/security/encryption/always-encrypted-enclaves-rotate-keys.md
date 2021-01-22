---
description: Ruotare le chiavi abilitate per l'enclave
title: Eseguire la rotazione | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 8ef5e2e3cbf4e00619aeb4b0d1643f0d07100aad
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534304"
---
# <a name="rotate-enclave-enabled-keys"></a>Ruotare le chiavi abilitate per l'enclave

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In Always Encrypted la rotazione di una chiave è il processo di sostituzione di una chiave master della colonna esistente o di una chiave di crittografia di colonna con una nuova chiave. Questo articolo descrive i casi d'uso e le considerazioni sulla rotazione delle chiavi specifiche di [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md) quando la chiave iniziale e/o la chiave di destinazione (nuova) è una chiave abilitata per l'enclave. Per le linee guida generali e i processi di gestione delle chiavi Always Encrypted, vedere [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md). 

Potrebbe essere necessario ruotare una chiave per motivi di sicurezza o di conformità, ad esempio se una chiave è stata compromessa o i criteri dell'organizzazione richiedono di sostituire periodicamente le chiavi. Always Encrypted con la rotazione delle chiavi degli enclave sicure consente anche di abilitare o disabilitare la funzionalità dell'enclave sicuro lato server per le colonne crittografate.

- Quando si sostituisce una chiave non abilitata per l'enclave con una chiave abilitata per l'enclave, si rende disponibile la funzionalità dell'enclave sicura per eseguire query su colonne protette con la chiave. Per altre informazioni, vedere [Abilitare Always Encrypted con enclave sicure per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- Quando si sostituisce una chiave abilitata per l'enclave con una chiave non abilitata per l'enclave, si disabilita la funzionalità dell'enclave sicura per eseguire query su colonne protette con la chiave.

Se si ruota una chiave solo per motivi di sicurezza/conformità e non per abilitare o disabilitare i calcoli dell'enclave per le colonne, verificare che la chiave di destinazione abbia la stessa configurazione per le enclave della chiave di origine. Se ad esempio la chiave di origine è abilitata per l'enclave, anche la chiave di destinazione deve essere abilitata per l'enclave.

I passaggi seguenti includono collegamenti ad articoli dettagliati, a seconda dello scenario di rotazione:

1. Effettuare il provisioning di una nuova chiave (una chiave master di colonna o una chiave di crittografia di colonna).
    - Per effettuare il provisioning di una nuova chiave abilitata per l'enclave, vedere [Effettuare il provisioning delle chiavi abilitate per l'enclave](always-encrypted-enclaves-provision-keys.md).
    - Per effettuare il provisioning di una chiave non abilitata per l'enclave, vedere [Effettuare il provisioning di chiavi Always Encrypted usando SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md) ed [ con PowerShell](configure-always-encrypted-keys-using-powershell.md).
2. Sostituire una chiave esistente con la nuova chiave.
    - Se si ruota una chiave di crittografia di colonna e sia la chiave di origine che la chiave di destinazione sono abilitate per l'enclave, è possibile eseguire la rotazione (che implica una nuova crittografia dei dati) sul posto. Per altre informazioni, vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md).
    - Per la procedura dettagliata di rotazione delle chiavi, vedere [Ruotare le chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) e [Ruotare le chiavi Always Encrypted con PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Passaggi successivi

- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md)
- [Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)  

## <a name="see-also"></a>Vedere anche  
- [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md)