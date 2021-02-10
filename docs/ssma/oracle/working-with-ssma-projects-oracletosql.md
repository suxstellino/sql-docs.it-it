---
title: Utilizzo dei progetti SSMA (OracleToSQL) | Microsoft Docs
description: Informazioni su come creare un progetto SSMA contenente metadati per i database Oracle da migrare e SQL Server, insieme alle impostazioni e alle informazioni di connessione.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Customizing Project Settings
ms.assetid: ee5d94c0-c7a6-4779-bd32-729bdaf61e1b
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 32a35ace86beea46d79f8eff1ea9a3ba094903fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073435"
---
# <a name="working-with-ssma-projects-oracletosql"></a>Uso dei progetti SSMA (OracleToSQL)
Per eseguire la migrazione di database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , creare innanzitutto un progetto SSMA. Il progetto è un file che contiene le informazioni seguenti:  
  
-   Metadati sui database Oracle di cui si desidera eseguire la migrazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Metadati sull'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che riceveranno gli oggetti e i dati migrati.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] informazioni di connessione.  
  
-   Impostazioni del progetto.  
  
Quando si apre un progetto, questo viene disconnesso da Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Che consente di lavorare offline. Per informazioni sulla riconnessione a, vedere la pagina relativa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [alla connessione a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Revisione delle impostazioni predefinite del progetto  
SSMA contiene diverse impostazioni per la conversione e il caricamento di oggetti di database, la migrazione dei dati e la sincronizzazione di SSMA con Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le impostazioni predefinite sono appropriate per molti utenti. Tuttavia, prima di creare un nuovo progetto SSMA, è necessario rivedere le impostazioni. Se lo si desidera, è possibile modificare le impostazioni predefinite che verranno utilizzate per tutti i nuovi progetti.  
  
**Per esaminare le impostazioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Selezionare il tipo di progetto nell'elenco a discesa della **versione di destinazione della migrazione** per cui sono necessarie le impostazioni da visualizzare o modificare e quindi fare clic sulla scheda **generale** .  
  
3.  Nel riquadro sinistro fare clic su **conversione**.  
  
4.  Nel riquadro destro esaminare e modificare le impostazioni secondo necessità. Per ulteriori informazioni su queste impostazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-conversion-oracletosql.md).  
  
5.  Ripetere i passaggi 1-3 per la migrazione, la sincronizzazione, il caricamento di oggetti di sistema, l'interfaccia utente grafica e le pagine di mapping dei tipi.  
  
    -   Per informazioni sulle impostazioni di migrazione, vedere [Impostazioni progetto &#40;migrazione&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-migration-oracletosql.md).  
  
    -   Per informazioni sulle impostazioni degli oggetti di sistema, vedere [Impostazioni progetto&#40;caricamento di oggetti di sistema&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-loading-system-objects-oracletosql.md).  
  
    -   Per informazioni sulle impostazioni per la sincronizzazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [impostazioni progetto&#40;sincronizzazione&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-synchronization-oracletosql.md).  
  
    -   Per informazioni sulle impostazioni GUI, vedere [Impostazioni progetto &#40;gui&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-gui-oracletosql.md).  
  
    -   Per informazioni sulle impostazioni di mapping dei tipi di dati, vedere [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
## <a name="creating-new-projects"></a>Creazione di nuovi progetti  
Per eseguire la migrazione dei dati da database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario innanzitutto creare un progetto di.  
  
**Per creare un progetto**  
  
1.  Scegliere **Nuovo progetto** dal menu **File**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
2.  Nella casella **nome** immettere un nome per il progetto.  
  
3.  Nella casella **percorso** immettere o selezionare una cartella per il progetto, quindi fare clic su **OK**.  
  
4.  Nell'elenco **a discesa migrazione per** selezionare la versione di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata per la migrazione. Le opzioni disponibili sono:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Database SQL di Azure  
  
## <a name="customizing-project-settings"></a>Personalizzazione delle impostazioni di progetto  
Oltre a definire le impostazioni di progetto predefinite che si applicano a tutti i nuovi progetti SSMA, è possibile personalizzare le impostazioni per ogni progetto. Per ulteriori informazioni, vedere [impostazione delle opzioni del progetto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md).  
  
Quando si personalizzano i mapping dei tipi di dati tra i database di origine e di destinazione, è possibile definire i mapping a livello di progetto, database o oggetto. Per ulteriori informazioni, vedere [mapping di tipi di dati Oracle e SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
## <a name="saving-projects"></a>Salvataggio di progetti  
Quando si salva un progetto, SSMA mantiene le impostazioni del progetto e, facoltativamente, i metadati del database, nel file di progetto.  
  
**Per salvare un progetto**  
  
-   Scegliere **Salva progetto** dal menu **file** .  
  
    Se gli schemi del progetto sono stati modificati o non sono stati convertiti, SSMA richiede di caricare e salvare i metadati. Il caricamento e il salvataggio dei metadati consentiranno di lavorare offline. Consente inoltre di inviare un file di progetto completo ad altre persone, ad esempio il personale del supporto tecnico. Se viene richiesto di salvare i metadati, procedere come segue:  
  
    1.  Per ogni schema che mostra lo stato dei **metadati mancanti**, selezionare la casella di controllo accanto al nome del database.  
  
        Il salvataggio dei metadati potrebbe richiedere diversi minuti. Se non si desidera salvare ancora i metadati, non selezionare alcuna casella di controllo.  
  
    2.  Fare clic sul pulsante **Salva**.  
  
        SSMA analizzerà gli schemi Oracle e salverà i metadati nel file di progetto.  
  
## <a name="opening-projects"></a>Apertura di progetti  
Quando si apre un progetto, questo viene disconnesso da Oracle e da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Che consente di lavorare offline. Per aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per eseguire la migrazione dei dati, è necessario riconnettersi a Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Per aprire un progetto**  
  
1.  Usare una delle procedure seguenti:  
  
    -   Scegliere **progetti recenti** dal menu **file** , quindi fare clic sul progetto che si desidera aprire.  
  
    -   Nel menu **file** selezionare **Apri progetto**, individuare il file di progetto. o2ssproj, selezionare il file e quindi fare clic su **Apri**.  
  
2.  Per riconnettersi a Oracle, scegliere **Riconnetti a Oracle** dal menu **file** .  
  
3.  Per riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , scegliere **Riconnetti a SQL Server** dal menu **file** .  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [connettersi a Oracle database (OracleToSQL)](./connecting-to-oracle-database-oracletosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
[Connessione a Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md)  
[Connessione a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
