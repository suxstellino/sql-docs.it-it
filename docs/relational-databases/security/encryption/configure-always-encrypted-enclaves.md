---
title: Configurare e usare Always Encrypted con enclave sicuri | Microsoft Docs
description: Informazioni su come configurare e usare Always Encrypted con enclave sicure in SQL Server e nel database SQL di Azure per ottenere funzionalità avanzate per i dati sensibili.
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
ms.openlocfilehash: 481493a50fdefc22f6eb4d77feb13cfc4848388d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534650"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>Configurare e usare Always Encrypted con enclave sicuri 

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclave sicuri](always-encrypted-enclaves.md) estende la funzionalità [Always Encrypted](always-encrypted-database-engine.md) esistente per abilitare funzionalità più avanzate per i dati sensibili, mantenendo i dati riservati. Questo articolo elenca le attività comuni per la configurazione e l'uso della funzionalità.

Per esercitazioni che illustrano come iniziare rapidamente a usare Always Encrypted con enclave sicure, vedere:

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="set-up-the-secure-enclave-and-attestation"></a>Configurare l'enclave sicura e l'attestazione

Prima di poter usare Always Encrypted con enclave sicure, è necessario configurare l'ambiente in modo da assicurare che l'enclave sicura sia disponibile per il database. È anche necessario configurare l'[attestazione dell'enclave](always-encrypted-enclaves.md#secure-enclave-attestation). 

Il processo di configurazione dell'ambiente varia a seconda che si usi [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] o [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="set-up-the-secure-enclave-and-attestation-in-ssnoversion-md"></a>Configurare l'enclave sicura e l'attestazione in [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Per informazioni dettagliate, vedere gli articoli seguenti:
- [Pianificare l'attestazione del servizio Sorveglianza host](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [Distribuire il servizio Sorveglianza host per [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)
- [Registrare il computer nel servizio Sorveglianza host](./always-encrypted-enclaves-host-guardian-service-register.md)
- [Configurare l'enclave sicuro in SQL Server](always-encrypted-enclaves-configure-enclave-type.md)

### <a name="set-up-the-secure-enclave-and-attestation-in-sssdsfull"></a>Configurare l'enclave sicura e l'attestazione in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

Per informazioni dettagliate, vedere gli articoli seguenti:
- [Pianificare le enclave e l'attestazione di Intel SGX in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-plan)
- [Abilitare Intel SGX per il database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-enable-sgx)
- [Configurare l'attestazione di Azure per il server logico del database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-configure-attestation)

## <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Gestire le chiavi per Always Encrypted con enclave sicuri
Per informazioni dettagliate, vedere gli articoli seguenti:
- [Gestire le chiavi per Always Encrypted con enclave sicuri - Panoramica](always-encrypted-enclaves-manage-keys.md)
- [Effettuare il provisioning delle chiavi abilitate per l'enclave](always-encrypted-enclaves-provision-keys.md)
- [Ruotare le chiavi abilitate per l'enclave](always-encrypted-enclaves-rotate-keys.md)

## <a name="configure-columns-with-always-encrypted-with-secure-enclaves"></a>Configurare le colonne con Always Encrypted con enclave sicuri
Per informazioni dettagliate, vedere gli articoli seguenti:
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri - Panoramica](always-encrypted-enclaves-configure-encryption.md)
- [Configurare la crittografia delle colonne sul posto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md)

## <a name="run-transact-sql-statements-using-secure-enclaves"></a>Eseguire istruzioni Transact-SQL con enclave sicure
Per informazioni dettagliate, vedere gli articoli seguenti:
- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)
- [Risolvere i problemi comuni per Always Encrypted con enclave sicure](always-encrypted-enclaves-troubleshooting.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>Creare e usare indici sulle colonne abilitate per l'enclave
Per informazioni dettagliate, vedere gli articoli seguenti:
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md)
  
## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Sviluppare applicazioni usando Always Encrypted con enclave sicuri
Per informazioni dettagliate, vedere gli articoli seguenti:
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)
