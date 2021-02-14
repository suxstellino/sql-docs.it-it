---
description: Configurare le funzionalità compatibili di SQL Server con Stretch Database
title: Configurare le funzionalità compatibili di SQL Server
ms.date: 03/14/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c8121ede-1aec-459b-b7b0-1408bb3e62fb
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c1f2389ea412b1cdc542f87ee25f3a95c34fa256
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080212"
---
# <a name="configure-compatible-sql-server-features-with-stretch-database"></a>Configurare le funzionalità compatibili di SQL Server con Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Per configurare le seguenti funzionalità di SQL Server per il funzionamento con Stretch Database sono sufficienti alcuni semplici passaggi.
-   Always On
-   Always Encrypted
-   Transparent Data Encryption (TDE)
-   Tabelle temporali

## <a name="configure-always-on-with-stretch-database"></a>Configurare Always On con Stretch Database
Se si usa Always On con Stretch Database, è necessario verificare che la chiave master del database sia disponibile nelle repliche secondarie. Stretch Database usa la chiave master del database per proteggere le credenziali che usa per la connessione al database remoto.

Dopo aver configurato il gruppo di disponibilità Always On, eseguire la stored procedure **sp_control_dbmasterkey_password** in ogni replica secondaria e specificare la password per il database abilitato per l'estensione. Per altre informazioni ed esempi, vedere [sp_control_dbmasterkey_password](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md). 

## <a name="configure-always-encrypted-with-stretch-database"></a>Configurare Always Encrypted con Stretch Database
Se si vuole usare Always Encrypted insieme a Stretch Database, è necessario configurare la crittografia nelle colonne selezionate prima di abilitare Stretch Database nella tabella.

Se la funzionalità Stretch Database è già abilitata nella tabella e si vogliono usare colonne Always Encrypted, è necessario effettuare le operazioni seguenti.
1.   Disabilitare Stretch Database nella tabella e recuperare i dati remoti da Azure. Per altre informazioni, vedere [Disabilitare Stretch Database e ripristinare i dati remoti](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).
2.   Configurare Always Encrypted nelle colonne selezionate.
3. Abilitare nuovamente Stretch Database nella tabella. Per ulteriori informazioni, vedere [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

## <a name="configure-transparent-data-encryption-tde-with-stretch-database"></a>Configurare Transparent Data Encryption (TDE) con Stretch Database

Se la crittografia TDE è abilitata nel database locale, non verrà abilitata automaticamente nell'endpoint remoto di Estensione database. È necessario ricordarsi di abilitare la crittografia TDE nell'endpoint remoto dopo avere abilitato l'estensione nel database.

## <a name="configure-temporal-tables-with-stretch-database"></a>Configurare le tabelle temporali con Stretch Database
Se si usano le tabelle temporali, è possibile abilitare Stretch Database nella tabella di cronologia, ma non nella tabella corrente.
-   Per istruzioni sull'uso delle tabelle temporali con Stretch Database, vedere [Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).
-   Per filtrare le righe di cui eseguire la migrazione dalla tabella di cronologia usando una finestra temporale scorrevole, vedere [Selezionare le righe di cui eseguire la migrazione tramite una funzione di filtro](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).
-   Se la tabella è ottimizzata per la memoria, non è possibile abilitare Stretch Database nella tabella di cronologia temporale. Le tabelle ottimizzate per la memoria non sono supportate.
