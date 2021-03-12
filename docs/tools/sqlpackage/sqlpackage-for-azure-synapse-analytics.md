---
title: SqlPackage per Azure sinapsi Analytics
description: Suggerimenti per l'uso di SqlPackage negli scenari di analisi delle sinapsi di Azure
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 661230edf4deea3d62ceef7d8b400cdb951330a5
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622921"
---
# <a name="sqlpackage-for-azure-synapse-analytics"></a>SqlPackage per Azure sinapsi Analytics

Questo articolo evidenzia le funzionalità di SqlPackage.exe che sono incentrate sui database di analisi delle sinapsi di Azure.

## <a name="extract"></a>Extract
Per [estrarre](sqlpackage-extract.md) uno schema nell'archivio BLOB di Azure, sono necessarie le proprietà seguenti:
- /p: AzureStorageBlobEndpoint
- /p: AzureStorageContainer
- /p: AzureStorageKey

L'accesso per l'estrazione è autorizzato tramite una chiave dell'account di archiviazione.  Un parametro aggiuntivo è facoltativo, che imposta il percorso radice di archiviazione all'interno del contenitore:
- /p: AzureStorageRootPath

Senza questa proprietà, il percorso predefinito è `servername/databasename/timestamp/` .

## <a name="publish"></a>Pubblica
Per [pubblicare](sqlpackage-publish.md) uno schema da un dacpac nell'archivio BLOB di Azure, sono necessarie le proprietà seguenti:
- /p: AzureStorageBlobEndpoint
- /p: AzureStorageContainer
- /p: AzureStorageRootPath
- /p: AzureStorageKey o/p: AzureSharedAccessSignatureToken

L'accesso per la pubblicazione può essere autorizzato tramite una chiave dell'account di archiviazione o un token di firma di accesso condiviso (SAS).

## <a name="next-steps"></a>Passaggi successivi
- Altre informazioni sull' [estrazione](sqlpackage-extract.md)
- Altre informazioni sulla [pubblicazione](sqlpackage-publish.md)
- Altre informazioni sull' [archiviazione BLOB di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
- Altre informazioni sulle [chiavi dell'account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)