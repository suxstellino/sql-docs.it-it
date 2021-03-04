---
description: Sviluppare applicazioni usando Always Encrypted con enclave sicuri
title: Sviluppare applicazioni usando Always Encrypted con enclave sicuri | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 14bfbdd1a9c6bd176c89513674c5680388ef1452
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838926"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Sviluppare applicazioni usando Always Encrypted con enclave sicuri
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclave sicuri](always-encrypted-enclaves.md) estende [Always Encrypted](always-encrypted-database-engine.md) per abilitare funzionalità più avanzate delle query dell'applicazione su colonne di database sensibili crittografate. Sfrutta le tecnologie basate sugli enclave sicuri per consentire all'executor della query in [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] di delegare i calcoli nelle colonne crittografate a un enclave sicuro all'interno del processo di [!INCLUDE[ssde-md](../../../includes/ssde-md.md)].

## <a name="prerequisites"></a>Prerequisiti

- L'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o il database e il server in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] devono essere configurati correttamente per supportare le enclave e l'attestazione. Per altre informazioni, vedere [Configurare l'enclave sicura e l'attestazione](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- È necessario ottenere un URL di attestazione per l'ambiente dall'amministratore del servizio di attestazione.

  - Se si usa [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] e il servizio Sorveglianza host (HGS), vedere [Determinare e condividere l'URL di attestazione HGS](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Se si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] e il servizio Attestazione di Microsoft Azure, vedere [Determinare l'URL di attestazione per i criteri di attestazione](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- L'applicazione deve usare una versione del driver del client SQL che supporta le enclave sicure. Per altri dettagli, vedere le sezioni seguenti.

- È necessario configurare un protocollo di attestazione e un URL di attestazione per una connessione di database. I dettagli relativi alla modalità di configurazione del protocollo di attestazione e dell'URL di attestazione dipendono dal driver client in uso.

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>Driver client per Always Encrypted con enclave sicure

Per sviluppare applicazioni usando Always Encrypted con enclave sicuri, è necessaria una versione del driver client di SQL che supporti enclave sicuri. Il driver client svolge il ruolo chiave seguente:

- Prima di inviare una query che usa un enclave sicuro a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per l'esecuzione, il driver avvia l'attestazione dell'enclave per verificare che l'enclave sicuro sia attendibile e possa essere usato senza problemi per elaborare i dati sensibili. Per altre informazioni sull'attestazione, vedere [Attestazione degli enclave sicuri](always-encrypted-enclaves.md#secure-enclave-attestation).
- Una volta completata l'attestazione, il driver client stabilisce una sessione sicura con l'enclave negoziando un segreto condiviso.
- Il driver usa il segreto condiviso per crittografare le chiavi di crittografia di colonna necessarie all'enclave per elaborare la query e invia le chiavi a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], che le inoltra all'enclave sicuro che decrittografa le chiavi. 
- Il driver invia infine la query per l'esecuzione, che attiva i calcoli all'interno dell'enclave sicuro.

## <a name="next-steps"></a>Passaggi successivi

I driver client seguenti supportano Always Encrypted con enclave sicuri:

- Provider di dati Microsoft .NET per SQL Server in .NET Framework 4.6 o versione successiva e .NET Core 2.1 o versione successiva. 
    - Per altre informazioni, vedere [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md).
    - Per un'esercitazione dettagliata, vedere [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicuri](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md).
    - Vedere anche [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted abilitato con enclave sicure](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md).
- Microsoft ODBC Driver for SQL Server, versione 17.4 o successiva. 
    - Per altre informazioni, vedere [Using Always Encrypted with the Windows ODBC Driver](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) (Uso di Always Encrypted with the con il driver ODBC di Windows). 
    - Per informazioni su come abilitare i calcoli dell'enclave per una connessione di database usando ODBC, vedere la sezione [Abilitare Always Encrypted con enclave sicure](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves).
- Microsoft JDBC Driver per SQL Server, versione 8.2 o successiva.
    - Per altre informazioni, vedere [Uso di Always Encrypted con enclave sicure con il driver JDBC](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md).
- Provider di dati .NET Framework per SQL Server in .NET Framework 4.7.2 o versione successiva. 
    - Per altre informazioni, vedere [Uso di Always Encrypted con il provider di dati .NET Framework per SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md).
    - Per un'esercitazione dettagliata, vedere [Esercitazione: Sviluppare un'applicazione .NET Framework usando Always Encrypted con enclave sicuri](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Vedere anche

- [Risolvere i problemi comuni per Always Encrypted con enclave sicure](always-encrypted-enclaves-troubleshooting.md)