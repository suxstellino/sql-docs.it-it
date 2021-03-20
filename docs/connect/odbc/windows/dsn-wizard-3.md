---
description: Informazioni su come definire elementi come il database predefinito nella creazione guidata origine dati per creare una nuova connessione ODBC a SQL Server.
title: Schermata 3 di Creazione guidata origine dati (driver ODBC per SQL Server)
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2263cfde2f48da1562e4a23015cb0e575a23cc4e
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673125"
---
# <a name="data-source-wizard-screen-3"></a>Creazione guidata origine dati - Schermata 3

Specificare il database predefinito, le varie opzioni ANSI che devono essere utilizzate dal driver e il nome di un server mirror.

## <a name="options"></a>Opzioni

### <a name="change-the-default-database-to"></a>Usa il seguente database predefinito

Indica il nome del database predefinito per qualsiasi connessione stabilita utilizzando l'origine dati corrente. Se questa casella viene deselezionata, le connessioni utilizzeranno il database predefinito definito per l'ID di accesso nel server. Se questa casella viene selezionata, verrà utilizzato il database specificato anziché il database predefinito definito per l'ID di accesso. Se la casella **Associa file di database** include il nome di un file primario, il database descritto dal file primario viene collegato come database usando il nome di database specificato nella casella **Usa il seguente database predefinito**.

L'utilizzo del database predefinito per l'ID di accesso è più efficiente rispetto all'impostazione di un database predefinito nell'origine dati ODBC.

### <a name="mirror-server"></a>Server mirror

Indica il nome del partner di failover del database da sottoporre a mirroring. Se nella casella **Usa il seguente database predefinito** non è specificato alcun nome di database o viene visualizzato il nome del database predefinito, l'opzione **Server mirror** non è attiva.

Facoltativamente, è possibile specificare un nome SPN per il server mirror. Tale nome verrà utilizzato per l'autenticazione reciproca tra client e server.

### <a name="attach-database-filename"></a>Associa file di database

Indica il nome del file primario di un database a cui è possibile collegarsi. Questo database viene associato e utilizzato come database predefinito per l'origine dati. Specificare il percorso completo e il nome del file primario. Il nome del database specificato nella casella **Usa il seguente database predefinito** viene usato come nome del database collegato.

### <a name="use-ansi-quoted-identifiers"></a>Usa identificatori delimitati ANSI

Specifica che `QUOTED_IDENTIFIERS` è impostato su on quando il driver ODBC per SQL Server si connette. Quando questa casella di controllo è selezionata, SQL Server applica le regole ANSI relative alle virgolette. Le virgolette doppie possono essere utilizzate solo per gli identificatori, ad esempio i nomi delle colonne e delle tabelle. Le stringhe di caratteri devono essere racchiuse tra virgolette singole:

```sql
SELECT "LastName"
FROM "Person.Contact"
WHERE "LastName" = 'O''Brien'
```

Se questa casella di controllo viene deselezionata, le applicazioni che utilizzano gli identificatori tra virgolette, ad esempio l'utilità Microsoft Query distribuita con Microsoft Excel, provocano errori quando generano istruzioni SQL con identificatori tra virgolette.

### <a name="use-ansi-nulls-paddings-and-warnings"></a>Usa avvisi, riempimenti e caratteri Null ANSI

Indica che le opzioni ANSI_NULLS, ANSI_WARNINGS e ANSI_PADDINGS devono essere impostate al momento della connessione del driver ODBC per SQL Server.

Se ANSI_NULLS è impostato, il server applica le regole ANSI relative al confronto delle colonne per individuare i valori NULL. La sintassi ANSI "IS NULL" o "IS NOT NULL" deve essere utilizzata per tutti i confronti di valori NULL. La sintassi Transact-SQL "= NULL" non è supportata.

Se ANSI_WARNINGS è impostato, SQL Server genera messaggi di avviso per le condizioni che violano le regole ANSI, ma non le regole di Transact-SQL. Esempi di tali errori sono il troncamento dei dati durante l'esecuzione di un'istruzione INSERT o UPDATE oppure il rilevamento di un valore Null durante una funzione di aggregazione.

Se l'opzione ANSI_PADDING è impostata, gli spazi vuoti finali nei valori **varchar** e gli zeri finali nei valori **varbinary** non vengono rimossi automaticamente.

### <a name="application-intent"></a>Finalità dell'applicazione

Dichiara il tipo di carico di lavoro dell'applicazione in caso di connessione a un server. I valori possibili sono **ReadOnly** e **ReadWrite**.

### <a name="multi-subnet-failover"></a>Failover su più subnet

Se l'applicazione si connette a un gruppo di disponibilità per il ripristino di emergenza a disponibilità elevata (gruppi di disponibilità AlwaysOn) in subnet diverse, l'abilitazione del **failover su più subnet** configura il driver ODBC per SQL Server per fornire un rilevamento e una connessione più veloci al server (attualmente) attivo.

### <a name="transparent-network-ip-resolution"></a>Risoluzione dell'IP di rete trasparente

Modifica il comportamento del **failover su più subnet** per consentire una riconnessione più veloce durante il failover. Per altre informazioni vedere [Uso della risoluzione dell'IP di rete trasparente](../using-transparent-network-ip-resolution.md).

### <a name="column-encryption"></a>Crittografia di colonna

Consente la decrittografia e la crittografia automatiche dei trasferimenti di dati da e verso le colonne crittografate con la funzionalità [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) disponibile in SQL Server 2016 e versioni successive.

### <a name="use-fmtonly-metadata-discovery"></a>Usare l'individuazione dei metadati FMTONLY

Usare il metodo di individuazione dei metadati SET FMTONLY legacy quando ci si connette a SQL Server 2012 o versione successiva. Abilitare questa opzione solo quando si utilizzano query non supportate da [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), ad esempio quelle che contengono tabelle temporanee.

### <a name="next"></a>Prossima

Consente di passare alla schermata successiva della procedura guidata.

### <a name="back"></a>Indietro

Consente di tornare alla schermata precedente della procedura guidata.

## <a name="next-steps"></a>Passaggi successivi

[Creazione guidata origine dati - Schermata 2](dsn-wizard-2.md)  
[Creazione guidata origine dati - Schermata 4](dsn-wizard-4.md)  
