---
description: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted abilitato con enclave sicure
title: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted abilitato con enclave sicure | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-daenge
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 01c8e698c5fd5a63701900c98e76935f4f796616
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835688"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted-enabled-with-secure-enclaves"></a>Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted abilitato con enclave sicure

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Questo esempio illustra l'uso del provider Azure Key Vault per l'accesso alle colonne crittografate.

[!code-csharp [Azure Key Vault Provider with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample.cs#1)]

> [!NOTE]
> - Per usare Always Encrypted con enclavi sicuri per l'applicazione .NET Standard, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClientSqlClient**. La versione di .NET Standard supportata è 2.1 o successiva. 
>
> - Per usare Always Encrypted con enclavi sicuri in Linux e macOS, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClient**.

## <a name="see-also"></a>Vedi anche

- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](sqlclient-support-always-encrypted.md)
