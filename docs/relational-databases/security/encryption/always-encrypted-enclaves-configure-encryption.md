---
description: Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri
title: Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri | Microsoft Docs
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
ms.openlocfilehash: 67b74e36ff5b2872e619a4b26fabdd05428e6a16
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534840"
---
# <a name="configure-column-encryption-in-place-using-always-encrypted-with-secure-enclaves"></a>Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclave sicure](always-encrypted-enclaves.md) supporta operazioni di crittografia sulle colonne di database sul posto, all'interno di un enclave sicura nel [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. La crittografia sul posto elimina la necessità di spostare i dati per tali operazioni all'esterno del database, rendendo le operazioni di crittografia più veloci e affidabili. 

> [!NOTE]
> Nonostante i vantaggi della crittografia sul posto a livello di prestazioni, le operazioni di crittografia su tabelle di grandi dimensioni possono richiedere molto tempo e utilizzare un numero significativo di risorse, influendo in modo potenzialmente negativo sulle prestazioni e sulla disponibilità delle applicazioni.

La crittografia sul posto consente anche di attivare operazioni di crittografia usando l'istruzione [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md), operazione impossibile senza un'enclave.

## <a name="prerequisites"></a>Prerequisiti
Le operazioni di crittografia supportate e i requisiti per le chiavi di crittografia di colonna, usati per le operazioni, sono:
- Crittografia di una colonna di testo non crittografato. La chiave di crittografia di colonna usata per crittografare la colonna deve essere abilitata per l'enclave.
- Nuova crittografia di una colonna crittografata usando un nuovo tipo di crittografia e/o una nuova chiave di crittografia di colonna. Sia la chiave di crittografia di colonna corrente che la nuova chiave di crittografia di colonna (se diversa da quella corrente) devono essere abilitate per l'enclave.
- Decrittografia di una colonna crittografata: la chiave di crittografia di colonna, che protegge la colonna, deve essere abilitata per l'enclave.

Per informazioni su come assicurarsi che le chiavi di crittografia di colonna siano abilitate per l'enclave, vedere [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md).

È anche necessario assicurarsi che l'ambiente soddisfi i [prerequisiti generali per l'esecuzione di istruzioni usando enclave sicure](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).

Un utente o un'applicazione che attiva operazioni di crittografia deve avere le autorizzazioni per apportare modifiche allo schema nella tabella contenente le colonne interessate e per accedere alle chiavi master delle colonne coinvolte nelle operazioni e ai metadati delle chiavi pertinenti nel database.

È possibile attivare la crittografia sul posto solo usando [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) da SQL Server Management Studio o dall'applicazione personalizzata. Vedere [Configurare la crittografia delle colonne sul posto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md).

> [!NOTE]
> La [procedura guidata Always Encrypted](always-encrypted-wizard.md) e il cmdlet [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption) non supportano attualmente la crittografia sul posto e scaricano sempre i dati per le operazioni di crittografia, anche se la configurazione soddisfa i requisiti precedenti. 

## <a name="next-steps"></a>Passaggi successivi
- [Configurare la crittografia delle colonne sul posto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Creare e usare indici in una colonna usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Vedere anche  
- [Risolvere i problemi comuni per Always Encrypted con enclave sicure](always-encrypted-enclaves-troubleshooting.md)