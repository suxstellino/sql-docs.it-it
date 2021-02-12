---
title: Connessione al database SQL di Azure (AccessToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un'istanza di destinazione del database SQL di Azure per eseguire la migrazione dei database di Access. SSMA ottiene i metadati sui database nel database SQL di Azure.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instance of SQL Azure
- metadata, refreshing
- refreshing metadata
- SQL Azure
- SQL Azure, connecting
- SQL Azure, connecting to
- SQL Azure, reconnecting
- SQL Azure, synchronizing metadata
ms.assetid: 1ba0d113-dc05-4431-8689-e14a8821bafd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4f31b2e46312e89830d977ca0ce61ad07572c875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076272"
---
# <a name="connecting-to-azure-sql-database-accesstosql"></a>Connessione al database SQL di Azure (AccessToSQL)

Per eseguire la migrazione dei database di Access a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , è necessario connettersi all'istanza di destinazione di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti i database nell'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e Visualizza i metadati del database in **Esplora metadati del database SQL di Azure**. SSMA archivia informazioni sull'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] a cui si è connessi, ma non archivia le password.

La connessione [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e si migrano i dati.

I metadati relativi all'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] non vengono sincronizzati automaticamente. Per aggiornare i metadati in **Esplora metadati del database SQL di Azure**, è invece necessario aggiornare manualmente i [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadati. Per ulteriori informazioni, vedere la sezione relativa alla sincronizzazione dei metadati del database SQL di Azure più avanti in questo argomento.

## <a name="required-azure-sql-database-permissions"></a>Autorizzazioni necessarie per il database SQL di Azure

L'account utilizzato per la connessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] richiede autorizzazioni diverse a seconda delle azioni eseguite dall'account:

- Per convertire gli oggetti di accesso in [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi, per aggiornare [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] i metadati da o per salvare la sintassi convertita in script, l'account deve disporre dell'autorizzazione per accedere all'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Per caricare oggetti di database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , l'account deve essere un membro del ruolo del database **db_ddladmin** .

- Per eseguire la migrazione dei dati a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , l'account deve essere un membro del ruolo del database **db_owner** .

## <a name="establishing-an-azure-sql-database-connection"></a>Creazione di una connessione al database SQL di Azure

Prima di convertire gli oggetti di database di Access nella [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] sintassi, è necessario stabilire una connessione all'istanza di in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] cui si desidera eseguire la migrazione del database o dei database di Access.

Quando si definiscono le proprietà di connessione, è inoltre necessario specificare il database in cui verrà eseguita la migrazione degli oggetti e dei dati. È possibile personalizzare questo mapping a livello di schema di accesso dopo la connessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Per ulteriori informazioni, vedere [mapping dei database di Access a SQL Server schemi](mapping-source-and-target-databases-accesstosql.md).
  
> [!IMPORTANT]
> Prima di provare a connettersi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , assicurarsi che l'indirizzo IP sia consentito tramite il [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] Firewall.
  
Per connettersi al [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]:

1. Scegliere **Connetti a SQL Azure** dal menu **file** . questa opzione è abilitata dopo la creazione di un progetto.
   Se in precedenza si è connessi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , il nome del comando verrà **riconnesso a SQL Azure**.

2. Nella finestra di dialogo connessione immettere o selezionare il nome del server [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Immettere, selezionare o **esplorare** il nome del database.

4. Immettere o selezionare **username**.

5. Immettere la **password**.

6. SSMA consiglia la connessione crittografata a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Fare clic su **Connect** (Connetti).
  
Se non sono presenti database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , è possibile creare il primo database usando l'opzione **Crea database di Azure** visualizzata facendo clic sul pulsante **Sfoglia** .

## <a name="synchronizing-azure-sql-database-metadata"></a>Sincronizzazione dei metadati del database SQL di Azure

I metadati relativi ai database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] non vengono aggiornati automaticamente. I metadati in **Esplora metadati del database SQL di Azure** sono uno snapshot dei metadati alla prima connessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o all'ultima volta che sono stati aggiornati manualmente i metadati. È possibile aggiornare manualmente i metadati per tutti i database o per qualsiasi singolo database o oggetto di database. Per sincronizzare i metadati:

1. Assicurarsi di essere connessi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. In **Esplora metadati del database SQL di Azure** selezionare la casella di controllo accanto allo schema del database o del database che si desidera aggiornare.
   Ad esempio, per aggiornare i metadati per tutti i database, selezionare la casella accanto a **database**.

3. Fare clic con il pulsante destro del mouse su **database** o sul database singolo o sullo schema del database, quindi scegliere **Sincronizza con database**.

## <a name="refreshing-azure-sql-database-metadata"></a>Aggiornamento dei metadati del database SQL di Azure

Se [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] gli schemi cambiano dopo la connessione, è possibile aggiornare i metadati dal server.

Per aggiornare i [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadati:

- In **Esplora metadati del database SQL di Azure** fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Aggiorna da database**.

## <a name="reconnecting-to-azure-sql-database"></a>Riconnessione al database SQL di Azure

La connessione [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e si migrano i dati.

La procedura per la riconnessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] è identica alla procedura per stabilire una connessione.

## <a name="next-steps"></a>Passaggi successivi

Il passaggio successivo della migrazione dipende dalle esigenze del progetto:

- Per personalizzare il mapping tra gli schemi di accesso e il database SQL di Azure, vedere [mapping dei database di Access a SQL Server schemi](mapping-source-and-target-databases-accesstosql.md).
- Per personalizzare le opzioni di configurazione per i progetti, vedere [impostazione delle opzioni del progetto](setting-conversion-and-migration-options-accesstosql.md).
- Per personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere [mapping di tipi di dati di origine e di destinazione](mapping-source-and-target-data-types-accesstosql.md).
- Se non è necessario eseguire alcuna di queste attività, è possibile convertire le definizioni degli oggetti di database di Access in SQL Azure definizioni degli oggetti. Per ulteriori informazioni, vedere [conversione di database di Access](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Vedere anche

[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
