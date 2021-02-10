---
description: Migrazione guidata (AccessToSQL)
title: Migrazione guidata (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration Wizard dialog box
- Migration Wizard, adding Access databases
- Migration Wizard, Connect to SQL Azure
- Migration Wizard, Connect to SQL Server
- Migration Wizard, Link Tables
- Migration Wizard, Migration status
- Migration Wizard, New Project
- Migration Wizard, Selecting objects to migrate
ms.assetid: 5bab5914-b2ae-4795-8cf5-83e42d64bef2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f5f97b55b4d357c2caa2314c993806c61f51e07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066676"
---
# <a name="migration-wizard-accesstosql"></a>Migrazione guidata (AccessToSQL)
La migrazione guidata consente di eseguire la migrazione di uno o più database dall'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Utilizzando la procedura guidata, si creerà un progetto, si aggiungeranno database al progetto, si selezionano gli oggetti di cui eseguire la migrazione e ci si connetterà a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Sarà inoltre possibile convertire, caricare ed eseguire la migrazione di schemi e dati di accesso. Facoltativamente, è possibile collegare le tabelle di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure tabelle.  
  
La maggior parte delle pagine della migrazione guidata contiene le stesse opzioni delle finestre di dialogo SSMA esistenti. Pertanto, le pagine della procedura guidata sono descritte di seguito, quindi vengono forniti collegamenti per poter ottenere ulteriori informazioni sulle singole opzioni. Se una pagina contiene opzioni univoche, queste sono documentate qui.  
  
## <a name="starting-the-migration-wizard"></a>Avvio della migrazione guidata  
Per impostazione predefinita, la migrazione guidata viene visualizzata all'avvio di SSMA. È inoltre possibile avviare la procedura guidata dal menu **file** selezionando **migrazione guidata**.  
  
## <a name="welcome-page"></a>Home page  
Nella pagina iniziale è stata introdotta la procedura guidata di migrazione e viene fornita l'opzione seguente per avviare la procedura guidata.  
  
**Avvia la procedura guidata all'avvio.**  
Per impostazione predefinita, SSMA avvierà la migrazione guidata quando si avvia SSMA. Per evitare l'avvio automatico della procedura guidata, deselezionare questa casella di controllo.  
  
## <a name="create-new-project-page"></a>Pagina Crea nuovo progetto  
La pagina Crea nuovo progetto consente di immettere il nome del file di progetto, il percorso e il tipo di progetto di migrazione (la versione di destinazione SQL Server utilizzata per la migrazione). Per ulteriori informazioni, vedere [nuovo progetto (SSMA)](./new-project-ssma-accesstosql.md)  
  
## <a name="add-access-databases-page"></a>Pagina Aggiungi database di accesso  
Nella pagina Aggiungi database di accesso è possibile aggiungere uno o più database di Access al progetto. È possibile aggiungere singoli database facendo clic su **Aggiungi database**, quindi selezionando i database dalla finestra **Apri** . In alternativa, è possibile trovare i database usando il pulsante **trova database** . Per altre informazioni, vedere i seguenti argomenti:  
  
-   [Aggiunta e rimozione di file di database di Access](adding-and-removing-access-database-files-accesstosql.md)  
  
-   [Procedura guidata per la ricerca di database (selezione percorsi)](./find-databases-wizard-select-locations-accesstosql.md)  
  
-   [Procedura guidata per la ricerca di database (selezione file)](./find-databases-wizard-select-files-accesstosql.md)  
  
-   [Procedura guidata per la ricerca di database (verifica selezione)](./find-databases-wizard-verify-selection-accesstosql.md)  
  
## <a name="select-objects-to-migrate-page"></a>Pagina Seleziona oggetti da migrare  
Nella pagina Seleziona oggetti da migrare è possibile selezionare gli oggetti da convertire. È possibile selezionare tutti gli oggetti, i gruppi di oggetti o singoli oggetti.  
  
**Per selezionare gli oggetti**  
  
1.  Espandere **Access-metabase**, quindi espandere **database**.  
  
2.  Effettuare una o più delle seguenti operazioni:  
  
    -   Per convertire tutti i database, selezionare la casella di controllo accanto a **database**.  
  
    -   Per convertire o omettere i singoli database, selezionare o deselezionare la casella di controllo accanto al nome del database.  
  
    -   Per convertire o omettere le query, espandere il database, quindi selezionare o deselezionare la casella di controllo **query** .  
  
    -   Per convertire o omettere singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare o deselezionare la casella di controllo accanto alla tabella.  
  
Se si dispone di molti oggetti, è possibile utilizzare le opzioni **avanzate di selezione oggetti** nel riquadro destro per filtrare gli oggetti di database di Access. Se, ad esempio, si selezionano **tabelle** nel riquadro sinistro, sarà possibile filtrare l'elenco delle tabelle immettendo le stringhe nella casella **filtro** . È quindi possibile selezionare o deselezionare le tabelle filtrate per la migrazione utilizzando i pulsanti nella parte superiore del riquadro.  
  
Per ulteriori informazioni sui filtri, vedere la sezione Opzioni di [selezione avanzata degli oggetti (SSMA Common)](../sybase/advanced-object-selection-sybasetosql.md).  
  
## <a name="connect-to-sql-server-page"></a>Connetti a SQL Server pagina  
Nella pagina Connetti a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] specificare le proprietà di connessione e quindi connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [Connect to SQL Server](connect-to-sql-server-accesstosql.md).
  
> [!IMPORTANT]  
> Non appena la connessione ha esito positivo, si verificherà la pagina delle **tabelle dei collegamenti** in cui è possibile collegare le tabelle. Fare clic su **Avanti** per avviare la migrazione.  
  
## <a name="connect-to-sql-azure-page"></a>Connetti a SQL Azure pagina  
Nella pagina Connetti a SQL Azure è possibile specificare le proprietà di connessione e quindi connettersi a SQL Azure. Per creare un nuovo database di Azure, è possibile usare l'opzione **Crea database di Azure** che viene visualizzata facendo clic sul pulsante **Sfoglia** . Per ulteriori informazioni, vedere [Connect to SQL Azure](connect-to-azure-sql-db-accesstosql.md)  
  
> [!IMPORTANT]  
> Non appena la connessione ha esito positivo, si verificherà la pagina delle **tabelle dei collegamenti** in cui è possibile collegare le tabelle. Fare clic sul pulsante **Avanti** nella pagina collegamenti per avviare la migrazione.  
  
## <a name="link-tables-page"></a>Pagina delle tabelle di collegamento  
Nella pagina tabelle dei collegamenti è possibile collegare le tabelle di accesso originali alle tabelle migrate [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Le tabelle di collegamento modificano il database di Access in modo che le query, i form, i report e le pagine di accesso ai dati usino i dati nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database di o SQL di Azure anziché i dati nel database di Access.  
  
**Tabelle di collegamento**  
Selezionare la casella di controllo **tabelle di collegamento** per collegare le tabelle di accesso alle tabelle migrate [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Per avviare la migrazione, fare clic sul pulsante **Avanti** .  
  
## <a name="migration-status-page"></a>Pagina stato migrazione  
Nella pagina stato migrazione viene visualizzato lo stato di avanzamento della conversione degli schemi di accesso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure schemi, il caricamento degli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, quindi la migrazione dei dati.  
  
Per ulteriori informazioni su questa pagina, vedere [Convert, Load e migrate](./convert-load-and-migrate-accesstosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Introduzione con SQL Server Migration Assistant per Access &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Guida di riferimento all'interfaccia utente (accesso)](./user-interface-reference-accesstosql.md)  
