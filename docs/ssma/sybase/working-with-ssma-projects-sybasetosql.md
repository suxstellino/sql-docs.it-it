---
description: Uso dei progetti SSMA (SybaseToSQL)
title: Utilizzo dei progetti SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 11091d95-c488-48c3-891a-743cac94ac93
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b0276efc988eca1674410c31a2f83379ee1e875b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064980"
---
# <a name="working-with-ssma-projects-sybasetosql"></a>Uso dei progetti SSMA (SybaseToSQL)
Per eseguire la migrazione di database di Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, creare innanzitutto un progetto SSMA. Il progetto è un file che contiene i metadati relativi ai database dell'ambiente del servizio app di cui si desidera eseguire la migrazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, i metadati relativi all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure che riceveranno gli oggetti e i dati migrati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure SQL Azure le informazioni di connessione e le impostazioni di progetto.  
  
Quando si apre un progetto, questo viene disconnesso da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. In questo modo è possibile lavorare offline. È possibile riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Per altre informazioni, vedere [connessione a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  /  [connessione al database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Revisione delle impostazioni predefinite del progetto  
SSMA contiene diverse opzioni per la conversione e il caricamento di oggetti di database, la migrazione dei dati e la sincronizzazione di SSMA con ASE e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Le impostazioni predefinite per queste opzioni sono appropriate per molti utenti. Tuttavia, prima di creare un nuovo progetto SSMA, è necessario rivedere le opzioni e, se si desidera, modificare i valori predefiniti che verranno utilizzati per tutti i nuovi progetti.  
  
**Per esaminare le impostazioni predefinite del progetto**  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Selezionare il tipo di progetto nell'elenco a discesa della **versione di destinazione della migrazione** per cui sono necessarie le impostazioni da visualizzare o modificare e quindi fare clic sulla scheda **generale** .  
  
3.  Nel riquadro sinistro fare clic su **conversione**.  
  
4.  Nel riquadro di destra esaminare le opzioni, modificando le opzioni in modo necessario. Per altre informazioni su queste opzioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;SybaseToSQL ](../../ssma/sybase/project-settings-conversion-sybasetosql.md).  
  
5.  Ripetere i passaggi 1-3 per la migrazione, la SQL Azure, il caricamento di oggetti, l'interfaccia utente grafica e le pagine di mapping dei tipi.  
  
    -   Per informazioni sulle opzioni di migrazione, vedere [Impostazioni progetto &#40;migrazione&#41; &#40;&#41;SybaseToSQL ](../../ssma/sybase/project-settings-migration-sybasetosql.md).  
  
    -   Per informazioni sulle opzioni per il caricamento di oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [impostazioni progetto &#40;sincronizzazione&#41; &#40;&#41;SybaseToSQL ](../../ssma/sybase/project-settings-synchronization-sybasetosql.md).  
  
    -   Per altre informazioni sulle opzioni GUI, vedere [Impostazioni progetto &#40;gui&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md).  
  
    -   Per ulteriori informazioni sulle impostazioni di mapping dei tipi di dati, fare clic su [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md).  
  
    -   Per altre informazioni sulle opzioni di SQL Azure, vedere [Impostazioni progetto &#40;database SQL di Azure &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md).  
  
    > [!NOTE]  
    > Le impostazioni SQL Azure verranno visualizzate solo quando si seleziona la **migrazione per SQL Azure** durante la creazione di un progetto.  
  
## <a name="creating-new-projects"></a>Creazione di nuovi progetti  
Per eseguire la migrazione dei dati dai database dell'ambiente del servizio app a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è necessario creare prima un progetto.  
  
**Per creare un progetto**  
  
1.  Scegliere **Nuovo progetto** dal menu **File**.  
  
    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
2.  Nella casella **nome** immettere un nome per il progetto.  
  
3.  Nella casella **percorso** immettere o selezionare una cartella per il progetto.  
  
4.  Nell'elenco **a discesa migrazione per** selezionare la versione di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata per la migrazione. Le opzioni disponibili sono:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Database SQL di Azure  
  
E quindi fare clic su **OK**.  
  
## <a name="customizing-project-settings"></a>Personalizzazione delle impostazioni di progetto  
Oltre a definire le impostazioni di progetto predefinite che si applicano a tutti i nuovi progetti SSMA, è possibile personalizzare le impostazioni per ogni progetto. Per ulteriori informazioni, vedere [impostazione delle opzioni del progetto &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).  
  
Quando si personalizzano i mapping dei tipi di dati tra i database di origine e di destinazione, è possibile definire i mapping a livello di progetto, database o oggetto. Per altre informazioni sul mapping dei tipi, vedere [mapping dei tipi di dati di Sybase ASE e SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
## <a name="saving-projects"></a>Salvataggio di progetti  
Quando si salva un progetto, SSMA mantiene le impostazioni del progetto e, facoltativamente, i metadati del database, nel file di progetto.  
  
**Per salvare un progetto**  
  
-   Scegliere **Salva progetto** dal menu **file** .  
  
    Se i database all'interno del progetto sono stati modificati o non sono stati convertiti, SSMA richiede di salvare i metadati nel progetto. Il salvataggio dei metadati consente di lavorare offline e di inviare un file di progetto completo ad altre persone, incluso il personale del supporto tecnico. Se viene richiesto di salvare i metadati, procedere come segue:  
  
    1.  Per ogni database che mostra lo stato dei **metadati mancanti**, selezionare la casella di controllo accanto al nome del database.  
  
        Il salvataggio dei metadati potrebbe richiedere diversi minuti. Se non si desidera salvare i metadati in questa fase, non selezionare alcuna casella di controllo.  
  
    2.  Fare clic sul pulsante **Salva**.  
  
        SSMA analizzerà gli schemi dell'ambiente del servizio app Sybase e salverà i metadati nel file di progetto.  
  
## <a name="opening-projects"></a>Apertura di progetti  
Quando si apre un progetto, questo viene disconnesso da ASE e da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. In questo modo è possibile lavorare offline. Per aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Per eseguire la migrazione dei dati, è necessario riconnettersi a ASE e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
**Per aprire un progetto**  
  
1.  Usare una delle procedure seguenti:  
  
    -   Scegliere **progetti recenti** dal menu **file** , quindi selezionare il progetto che si desidera aprire.  
  
    -   Nel menu **file** selezionare **Apri progetto**, individuare il file di progetto. s2ssproj, selezionare il file e quindi fare clic su **Apri**.  
  
2.  Per riconnettersi all'ambiente del servizio app, scegliere **Riconnetti a Sybase** dal menu **file** .  
  
3.  Per riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure scegliere **Riconnetti per SQL Server** Riconnetti   /  **a SQL Azure** nel menu file.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [connettersi a Sybase ASE](connecting-to-sybase-ase-sybasetosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione dei database Sybase ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Connessione a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)  
[Connessione a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  
[Connessione al database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)  
  
