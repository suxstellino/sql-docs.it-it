---
description: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted
title: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 48c9fbbe5afbc5115aa52dbb13d258e6595b6efc
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464862"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Questo esempio illustra l'uso del provider Azure Key Vault per l'accesso alle colonne crittografate.

## <a name="azurekeyvaultprovider-v20"></a>AzureKeyVaultProvider v 2.0 +

[!code-csharp [Azure Key Vault Provider 2.0 Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample_2_0.cs#1)]

## <a name="azurekeyvaultprovider-v1x"></a>AzureKeyVaultProvider V1. x

[!code-csharp [Azure Key Vault Provider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
>
> - Per usare la funzionalità Always Encrypted senza enclavi sicuri per l'applicazione .NET Standard, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClientSqlClient**. La versione di .NET Standard supportata è 2.0 o successiva.
>
> - Per usare la funzionalità Always Encrypted in Linux e macOS, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClient**.

## <a name="see-also"></a>Vedi anche

- [Esempio che illustra l'uso del provider di Azure Key Vault con Always Encrypted abilitato con enclave sicure](azure-key-vault-enclave-example.md)
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](sqlclient-support-always-encrypted.md)
