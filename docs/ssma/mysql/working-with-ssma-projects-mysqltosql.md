---
description: Utilizzo dei progetti SSMA (MySQLToSQL)
title: Utilizzo dei progetti SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Working with SSMA projects, create new project
- Working with SSMA projects, customize settings
- Working with SSMA projects, Open project
- Working with SSMA projects, Save project
ms.assetid: 9e4394e9-f177-41d9-839e-5d53a9c9b840
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92fb1bec694ca29ffe0591eb03012e1ed2ab3f2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064370"
---
# <a name="working-with-ssma-projects-mysqltosql"></a>Utilizzo dei progetti SSMA (MySQLToSQL)
Per eseguire la migrazione di database MySQL a SQL Server o SQL Azure, è necessario creare prima un progetto SSMA. Il progetto è un file che contiene le informazioni seguenti:  
  
-   Metadati sui database MySQL di cui si vuole eseguire la migrazione SQL Server o SQL Azure.  
  
-   Metadati sull'istanza di destinazione di SQL Server o SQL Azure che riceveranno gli oggetti e i dati migrati.  
  
-   SQL Server o SQL Azure le informazioni di connessione.  
  
-   Impostazioni del progetto.  
  
Quando si apre un progetto, questo viene disconnesso da MySQL e SQL Server o SQL Azure. Che consente di lavorare offline. Per ulteriori informazioni sulla riconnessione a SQL Server, vedere la pagina relativa [alla connessione a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="reviewing-default-project-settings"></a>Revisione delle impostazioni predefinite del progetto  
SSMA contiene diverse impostazioni per la conversione e il caricamento del database, la migrazione dei dati e la sincronizzazione di SSMA con MySQL e SQL Server o SQL Azure. Le impostazioni predefinite sono appropriate per molti utenti. Tuttavia, prima di creare un nuovo progetto SSMA, è necessario rivedere le impostazioni. Se necessario, è possibile modificare le impostazioni predefinite che verranno usate per tutti i nuovi progetti.  
  
##### <a name="to-review-default-project-settings"></a>Per esaminare le impostazioni predefinite del progetto  
  
1.  Scegliere **Impostazioni progetto predefinite** dal menu **strumenti** .  
  
2.  Selezionare il tipo di progetto nell'elenco a discesa della **versione di destinazione della migrazione** per cui visualizzare o modificare le impostazioni e quindi fare clic sulla scheda **generale** .  
  
3.  Nel riquadro sinistro fare clic su **conversione**.  
  
4.  Nel riquadro destro esaminare e modificare le impostazioni secondo necessità. Per ulteriori informazioni su queste impostazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;MySQLToSQL ](../../ssma/mysql/project-settings-conversion-mysqltosql.md) .  
  
5.  Ripetere i passaggi 1-3 per le pagine migrazione, sincronizzazione, SQL Azure, GUI e mapping dei tipi.  
  
-   Per informazioni sulle impostazioni di migrazione, vedere [Impostazioni progetto &#40;migrazione&#41; &#40;&#41;MySQLToSQL ](../../ssma/mysql/project-settings-migration-mysqltosql.md).  
  
-   Per informazioni sulle impostazioni per la sincronizzazione SQL Server, vedere [Impostazioni progetto &#40;sincronizzazione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md).  
  
-   Per informazioni sulle impostazioni GUI, vedere [Impostazioni progetto (GUI) (SSMA comune)](../sybase/project-settings-gui-sybasetosql.md).  
  
-   Per informazioni sulle impostazioni di mapping dei tipi di dati, vedere [Impostazioni progetto &#40;mapping dei tipi&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
-   Per informazioni sulle impostazioni di SQL Azure, vedere [Impostazioni progetto &#40;database SQL di Azure&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md).  
  
> [!NOTE]  
> Le impostazioni SQL Azure verranno visualizzate solo quando si seleziona la **migrazione per SQL Azure** durante la creazione di un progetto.  
  
## <a name="creating-new-projects"></a>Creazione di nuovi progetti  
Per eseguire la migrazione dei dati da database MySQL a SQL Server o SQL Azure, è necessario creare un progetto.  
  
##### <a name="to-create-a-new-project"></a>Per creare un nuovo progetto  
  
1.  Scegliere **nuovo progetto** dal menu **file** . Verrà visualizzata la finestra di dialogo **Nuovo progetto** . Scegliere **Nuovo progetto** dal menu **File**. Verrà visualizzata la finestra di dialogo **Nuovo progetto** .  
  
2.  Nella casella **nome** immettere un nome per il progetto.  
  
3.  Nella casella **percorso** immettere o selezionare una cartella per il progetto.  
  
4.  Nell'elenco **a discesa migrazione per** selezionare la versione di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata per la migrazione. Le opzioni disponibili sono:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   Database SQL di Azure  
  
E quindi fare clic su **OK**  
  
SSMA crea il file di progetto.  
  
## <a name="customizing-project-settings"></a>Personalizzazione delle impostazioni di progetto  
Oltre a definire le impostazioni di progetto predefinite applicabili a tutti i nuovi progetti SSMA, è anche possibile personalizzare le impostazioni per ogni progetto. Per ulteriori informazioni, vedere [impostazione delle opzioni del progetto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).  
  
Quando si personalizzano i mapping dei tipi di dati tra i database di origine e di destinazione, è possibile definire i mapping a livello di progetto, database o oggetto. Per altre informazioni, vedere [mapping di tipi di dati MySQL e SQL Server &#40;&#41;MySQLToSQL ](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).  
  
## <a name="saving-projects"></a>Salvataggio di progetti  
La funzionalità di salvataggio dei progetti consente all'utente di salvare essenzialmente le impostazioni del progetto e, facoltativamente, i metadati del database nel file di progetto SSMA.  
  
##### <a name="to-save-a-project"></a>Per salvare un progetto  
  
-   Scegliere **Salva** progetto dal menu **file** .  
  
Se i database all'interno del progetto sono stati modificati o non sono stati convertiti, SSMA richiede di caricare e salvare i metadati. Il caricamento e il salvataggio dei metadati consentono di lavorare offline. Consente inoltre di inviare un file di progetto completo ad altre persone, ad esempio il personale del supporto tecnico. Se viene richiesto di salvare i metadati, procedere come segue:  
  
1.  Per ogni database che mostra lo stato dei **metadati mancanti**, selezionare la casella di controllo accanto al nome del database. Il salvataggio dei metadati potrebbe richiedere diversi minuti. Se non si desidera salvare i metadati in questa fase, non selezionare alcuna casella di controllo.  
  
2.  Fare clic su **Salva**.  
  
SSMA analizzerà gli schemi MySQL e salverà i metadati nel file di progetto.  
  
## <a name="opening-projects"></a>Apertura di progetti  
Quando si apre un progetto, questo viene disconnesso da MySQL e da SQL Server o SQL Azure. In questo modo è possibile lavorare offline. Per aggiornare i metadati, caricare oggetti di database in SQL Server o SQL Azure. Per eseguire la migrazione dei dati, è necessario riconnettersi a SQL Server o SQL Azure.  
  
##### <a name="to-open-a-project"></a>Per aprire un progetto  
  
1.  Usare una delle procedure seguenti:  
  
    1.  Scegliere **progetti recenti** dal menu **file** .  
  
    2.  Selezionare il progetto che si desidera aprire.  
  
    3.  Nel menu **file** selezionare **Apri progetto**, individuare il file di progetto. m2ssproj, selezionare il file e quindi fare clic su **Apri**.  
  
2.  Per riconnettersi a MySQL, scegliere **Riconnetti a MySQL** dal menu **file** .  
  
3.  Per riconnettersi a SQL Server, scegliere **Riconnetti a SQL Server** dal menu **file** .  
  
4.  Per riconnettersi a SQL Azure, scegliere **Riconnetti a SQL Azure** dal menu **file** .  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione è la [connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Connessione a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
[Connessione al database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
