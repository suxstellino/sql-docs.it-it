---
description: Connessione al database SQL di Azure (SybaseToSQL)
title: Connessione al database SQL di Azure (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9e77e4b0-40c0-455c-8431-ca5d43849aa7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9407f646eeded6a24fcaf166492abe4792b9b609
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057066"
---
# <a name="connecting-to-azure-sql-database-sybasetosql"></a>Connessione al database SQL di Azure (SybaseToSQL)

Per eseguire la migrazione dei database Sybase a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , è necessario connettersi all'istanza di destinazione di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti i database nell'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e Visualizza i metadati del database in **Esplora metadati del database SQL di Azure**. SSMA archivia le informazioni dell'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] a cui si è connessi, ma non archivia le password.

La connessione [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e si migrano i dati.

I metadati relativi all'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] non vengono sincronizzati automaticamente. Per aggiornare i metadati in **Esplora metadati del database SQL di Azure**, è invece necessario aggiornare manualmente i [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadati. Per ulteriori informazioni, vedere la sezione relativa alla sincronizzazione dei metadati del database SQL di Azure più avanti in questo argomento.

## <a name="required-azure-sql-database-permissions"></a>Autorizzazioni necessarie per il database SQL di Azure

L'account utilizzato per la connessione a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] richiede autorizzazioni diverse a seconda delle azioni eseguite dall'account:

- Per convertire gli oggetti dell'ambiente del servizio app in [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi, per aggiornare i metadati da [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o per salvare la sintassi convertita in script, l'account deve disporre dell'autorizzazione per accedere all'istanza di [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Per caricare oggetti di database in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , l'account deve essere un membro del ruolo del database **db_ddladmin** .

- Per eseguire la migrazione dei dati a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , l'account deve essere un membro del ruolo del database **db_owner** .

- Per eseguire il codice generato da SSMA, è necessario che l'account disponga `EXECUTE` delle autorizzazioni per tutte le funzioni definite dall'utente nello schema **ssma_syb** del database di destinazione. Queste funzioni forniscono funzionalità equivalenti delle funzioni di sistema dell'ambiente del servizio app e vengono usate dagli oggetti convertiti.

## <a name="establishing-an-azure-sql-database-connection"></a>Creazione di una connessione al database SQL di Azure

Prima di convertire gli oggetti di database Sybase in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] sintassi, è necessario stabilire una connessione all'istanza di in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] cui si desidera eseguire la migrazione del database o dei database Sybase.

Quando si definiscono le proprietà di connessione, è inoltre necessario specificare il database in cui verrà eseguita la migrazione degli oggetti e dei dati. È possibile personalizzare questo mapping a livello di schema Sybase dopo la connessione al database SQL di Azure. Per altre informazioni, vedere mapping di schemi dell'ambiente del servizio app [Sybase a schemi SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).

> [!IMPORTANT]
> Prima di provare a connettersi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , assicurarsi che l'indirizzo IP sia consentito tramite il [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] Firewall.

Per connettersi al [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]:

1. Scegliere **Connetti al database SQL di Azure** dal menu **file** . questa opzione è abilitata dopo la creazione di un progetto.
   Se si è precedentemente connessi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , il nome del comando verrà **riconnesso al database SQL di Azure**.

2. Nella finestra di dialogo connessione immettere o selezionare il nome del server [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Immettere, selezionare o **esplorare** il nome del database.

4. Immettere o selezionare **username**.

5. Immettere la **password**.

6. SSMA consiglia la connessione crittografata a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Fare clic su **Connect** (Connetti).

## <a name="synchronizing-azure-sql-database-metadata"></a>Sincronizzazione dei metadati del database SQL di Azure

I metadati relativi ai [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] database non vengono aggiornati automaticamente. I metadati in Esplora metadati del database SQL di Azure sono uno snapshot dei metadati alla prima connessione al database SQL di Azure o all'ultima volta che sono stati aggiornati manualmente i metadati. È possibile aggiornare manualmente i metadati per tutti i database o per qualsiasi singolo database o oggetto di database. Per sincronizzare i metadati:

1. Assicurarsi di essere connessi a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. In **Esplora metadati del database SQL di Azure** selezionare la casella di controllo accanto allo schema del database o del database che si desidera aggiornare.
   Ad esempio, per aggiornare i metadati per tutti i database, selezionare la casella accanto a **database**.

3. Fare clic con il pulsante destro del mouse su **database** o sul database singolo o sullo schema del database, quindi scegliere **Sincronizza con database**.

## <a name="next-step"></a>passaggio successivo

Il passaggio successivo della migrazione dipende dalle esigenze del progetto:

- Per personalizzare il mapping tra schemi e database di Sybase, vedere mapping degli schemi dell'ambiente del servizio app [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] [Sybase a schemi SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).
- Per personalizzare le opzioni di configurazione per i progetti, vedere [impostazione delle opzioni del progetto &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).
- Per personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere Mapping dei tipi di [dati di Sybase ASE e SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).
- Se non è necessario eseguire alcuna di queste attività, è possibile convertire le definizioni degli oggetti di database Sybase in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] definizioni di oggetti. Per altre informazioni, vedere [conversione di oggetti di database Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).

## <a name="see-also"></a>Vedere anche

[Migrazione dei database Sybase ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
