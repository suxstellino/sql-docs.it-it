---
title: Configurare Always Encrypted tramite SSMS
description: Descrive le attività per la configurazione e la gestione di database Always Encrypted con SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 10/31/2019
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
ms.openlocfilehash: fc8018153ca9f3ee87db9d9e19b4a696537e9e38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345411"
---
# <a name="configure-always-encrypted-using-sql-server-management-studio"></a>Configure Always Encrypted using SQL Server Management Studio (Configurare Always Encrypted usando SQL Server Management Studio)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive le operazioni necessarie per la configurazione di Always Encrypted e la gestione dei database che usano Always Encrypted con [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="security-considerations-when-using-ssms-to-configure-always-encrypted"></a>Considerazioni sulla sicurezza quando si usa SSMS per configurare Always Encrypted

Quando viene usato per configurare Always Encrypted, SSMS gestisce sia le chiavi che i dati sensibili di Always Encrypted in modo tale che vengano visualizzati in testo non crittografato all'interno del processo di SSMS. È quindi importante eseguire SSMS in un computer protetto. Se il database è ospitato in SQL Server, verificare che SSMS venga eseguito in un computer diverso da quello che ospita l'istanza di SQL Server. Poiché l'obiettivo principale di Always Encrypted è garantire la sicurezza dei dati sensibili crittografati anche se il sistema di database viene compromesso, eseguire uno script di PowerShell che elabora le chiavi o i dati sensibili nel computer SQL Server può ridurre o annullare i vantaggi della funzionalità. Per altre indicazioni, vedere [Considerazioni sulla sicurezza per la gestione delle chiavi](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

SQL Server Management Studio non supporta la separazione dei ruoli tra quelli che gestiscono il database (DBA) e quelli che gestiscono i segreti di crittografia e hanno accesso ai dati in testo normale (amministratori della sicurezza e/o di applicazioni). Se l'organizzazione applica la separazione dei ruoli, è necessario usare PowerShell per configurare Always Encrypted. Per altre informazioni, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) e [Configurare Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md). 

## <a name="always-encrypted-tasks-using-ssms"></a>Attività di Always Encrypted tramite SSMS

- [Effettuare il provisioning di chiavi Always Encrypted con SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Ruotare chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted](always-encrypted-wizard.md)
- [Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati](configure-always-encrypted-using-dacpac.md)
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Esportare e importare database usando Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Eseguire il backup e il ripristino di database usando Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Panoramica della gestione delle chiavi per Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurare Always Encrypted tramite PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)
