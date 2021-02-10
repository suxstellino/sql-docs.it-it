---
description: Utilizzo dei progetti SSMA (DB2ToSQL)
title: Utilizzo dei progetti SSMA (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 07abef8a-28e8-4a66-927c-c9a5b8c938ef
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 67c772195ea4b2a2ab29c08219f77d04a5d6833c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069316"
---
# <a name="working-with-ssma-projects-db2tosql"></a>Utilizzo dei progetti SSMA (DB2ToSQL)
Per eseguire la migrazione dei database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , creare innanzitutto un progetto SSMA. Il progetto è un file che contiene le informazioni seguenti:  
  
-   Metadati sui database DB2 a cui si vuole eseguire la migrazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Metadati sull'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che riceveranno gli oggetti e i dati migrati.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] informazioni di connessione.  
  
-   Impostazioni del progetto.  
  
Quando si apre un progetto, questo viene disconnesso da DB2 e da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Che consente di lavorare offline. Per informazioni sulla riconnessione a, vedere la pagina relativa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [alla connessione a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Revisione delle impostazioni predefinite del progetto  
SSMA contiene diverse impostazioni per la conversione e il caricamento di oggetti di database, la migrazione dei dati e la sincronizzazione di SSMA con DB2 e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le impostazioni predefinite sono appropriate per molti utenti. Tuttavia, prima di creare un nuovo progetto SSMA, è necessario rivedere le impostazioni. Se lo si desidera, è possibile modificare le impostazioni predefinite che verranno utilizzate per tutti i nuovi progetti.  
  
**Per esaminare le impostazioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Selezionare il tipo di progetto nell'elenco a discesa della **versione di destinazione della migrazione** per cui sono necessarie le impostazioni da visualizzare o modificare e quindi fare clic sulla scheda **generale** .  
  
3.  Nel riquadro sinistro fare clic su **conversione**.  
  
4.  Nel riquadro destro esaminare e modificare le impostazioni secondo necessità. Per ulteriori informazioni su queste impostazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;DB2ToSQL ](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
5.  Ripetere i passaggi 1-3 per la migrazione, la sincronizzazione, il caricamento di oggetti di sistema, l'interfaccia utente grafica e le pagine di mapping dei tipi.  
  
    -   Per informazioni sulle impostazioni di migrazione, vedere [Impostazioni progetto &#40;migrazione&#41; &#40;&#41;DB2ToSQL ](../../ssma/db2/project-settings-migration-db2tosql.md).  
  
    -   Per informazioni sulle impostazioni degli oggetti di sistema, vedere [Impostazioni progetto&#40;caricamento di oggetti di sistema&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-loading-system-objects-db2tosql.md).  
  
    -   Per informazioni sulle impostazioni per la sincronizzazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [impostazioni progetto&#40;sincronizzazione&#41; &#40;&#41;DB2ToSQL ](../../ssma/db2/project-settings-synchronization-db2tosql.md).  
  
    -   Per informazioni sulle impostazioni GUI, vedere [Impostazioni progetto &#40;gui&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md).  
  
    -   Per informazioni sulle impostazioni di mapping dei tipi di dati, vedere [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
## <a name="creating-new-projects"></a>Creazione di nuovi progetti  
Per eseguire la migrazione dei dati da database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario innanzitutto creare un progetto.  
  
**Per creare un progetto**  
  
1.  Scegliere **Nuovo progetto** dal menu **File**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
2.  Nella casella **nome** immettere un nome per il progetto.  
  
3.  Nella casella **percorso** immettere o selezionare una cartella per il progetto, quindi fare clic su **OK**.  
  
4.  Nell'elenco **a discesa migrazione per** selezionare la versione di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata per la migrazione. Le opzioni disponibili sono:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Database SQL di Azure  
  
## <a name="customizing-project-settings"></a>Personalizzazione delle impostazioni di progetto  
Oltre a definire le impostazioni di progetto predefinite che si applicano a tutti i nuovi progetti SSMA, è possibile personalizzare le impostazioni per ogni progetto. Per ulteriori informazioni, vedere [impostazione delle opzioni del progetto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md) e sezioni correlate.  
  
Quando si personalizzano i mapping dei tipi di dati tra i database di origine e di destinazione, è possibile definire i mapping a livello di progetto, database o oggetto. Per ulteriori informazioni, vedere [mapping di tipi di dati DB2 e SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
## <a name="saving-projects"></a>Salvataggio di progetti  
Quando si salva un progetto, SSMA mantiene le impostazioni del progetto e, facoltativamente, i metadati del database, nel file di progetto.  
  
**Per salvare un progetto**  
  
-   Scegliere **Salva progetto** dal menu **file** .  
  
    Se gli schemi del progetto sono stati modificati o non sono stati convertiti, SSMA richiede di caricare e salvare i metadati. Il caricamento e il salvataggio dei metadati consentiranno di lavorare offline. Consente inoltre di inviare un file di progetto completo ad altre persone, ad esempio il personale del supporto tecnico. Se viene richiesto di salvare i metadati, procedere come segue:  
  
    1.  Per ogni schema che mostra lo stato dei **metadati mancanti**, selezionare la casella di controllo accanto al nome del database.  
  
        Il salvataggio dei metadati potrebbe richiedere diversi minuti. Se non si desidera salvare ancora i metadati, non selezionare alcuna casella di controllo.  
  
    2.  Fare clic sul pulsante **Salva**.  
  
        SSMA analizzerà gli schemi DB2 e salverà i metadati nel file di progetto.  
  
## <a name="opening-projects"></a>Apertura di progetti  
Quando si apre un progetto, questo viene disconnesso da DB2 e da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Che consente di lavorare offline. Per aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per eseguire la migrazione dei dati, è necessario riconnettersi a DB2 e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Per aprire un progetto**  
  
1.  Usare una delle procedure seguenti:  
  
    -   Scegliere **progetti recenti** dal menu **file** , quindi fare clic sul progetto che si desidera aprire.  
  
    -   Nel menu **file** selezionare **Apri progetto**, individuare il file di progetto. o2ssproj, selezionare il file e quindi fare clic su **Apri**.  
  
2.  Per riconnettersi a DB2, scegliere **Riconnetti a DB2** dal menu **file** .  
  
3.  Per riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , scegliere **Riconnetti a SQL Server** dal menu **file** .  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [connettersi al database DB2](./connecting-to-db2-database-db2tosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
[Connessione al database DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
[Connessione a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
