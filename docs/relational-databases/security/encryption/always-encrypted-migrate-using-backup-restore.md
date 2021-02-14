---
description: Eseguire il backup e il ripristino di database con Always Encrypted
title: Eseguire il backup e il ripristino di database con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b5f491846b51b1d70b36c73029ad96706cb1b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345478"
---
# <a name="backup-and-restore-databases-using-always-encrypted"></a>Eseguire il backup e il ripristino di database con Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Questo articolo descrive come eseguire il backup e il ripristino di un database contenente colonne protette con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Quando si esegue il backup di un database, il file di backup risultante contiene dati crittografati archiviati in colonne crittografate e tutti i metadati per le chiavi Always Encrypted.

Quando si ripristina un database, vengono ripristinati anche tutti i dati crittografati e tutti i metadati per le chiavi Always Encrypted. 

Se il database viene ripristinato in un altro server o con un nome diverso, non è necessario compiere alcuna operazione particolare per consentire all'applicazione di eseguire query sui dati crittografati nel database di destinazione, poiché le chiavi dei due database sono uguali.

Per informazioni dettagliate su come eseguire il backup e il ripristino di un database, vedere:
- [Backup Overview (SQL Server)](../../backup-restore/backup-overview-sql-server.md)
- [Ripristinare un database in un'istanza gestita](/azure/sql-database/sql-database-managed-instance-get-started-restore)

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Vedere anche
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Esportare e importare database usando Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Eseguire la migrazione di dati da o verso colonne usando Always Encrypted con l'Importazione/Esportazione guidata SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Caricamento bulk di dati crittografati in colonne tramite Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)