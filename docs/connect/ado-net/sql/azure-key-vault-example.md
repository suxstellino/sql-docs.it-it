---
description: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted
title: Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 6638144576d56a0225f03021f7cf8226643a45ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771583"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Questo esempio illustra l'uso del provider Azure Key Vault per l'accesso alle colonne crittografate.

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - Per usare la funzionalità Always Encrypted senza enclavi sicuri per l'applicazione .NET Standard, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClientSqlClient**. La versione di .NET Standard supportata è 2.0 o successiva. 
>
> - Per usare la funzionalità Always Encrypted in Linux e macOS, è necessaria la versione 2.1.0 o successiva di **Microsoft.Data.SqlClient**.

## <a name="see-also"></a>Vedi anche

- [Esempio che illustra l'uso del provider Azure Key Vault con Always Encrypted con enclave sicuri](azure-key-vault-enclave-example.md)
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso di Always Encrypted con il provider di dati Microsoft .NET per SQL Server](sqlclient-support-always-encrypted.md)
