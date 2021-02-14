---
title: Introduzione con SSMA per SAP ASE (SybaseToSQL) | Microsoft Docs
description: Informazioni sul processo di installazione di SQL Server Migration Assistant (SSMA) per SAP ASE e acquisire familiarità con l'interfaccia utente di SSMA.
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c4098516-f0fc-4690-97bb-3766dfd43156
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2ed1698af612a3bd960b7c16011dc4f95e33f61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078862"
---
# <a name="getting-started-with-ssma-for-sap-ase-sybasetosql"></a>Introduzione con SSMA per SAP ASE (SybaseToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per SAP ASE consente di convertire rapidamente gli schemi di database SAP Adaptive Server Enterprise (ASE) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o negli schemi del database SQL di Azure, caricare gli schemi risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure ed eseguire la migrazione dei dati da SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure.  
  
In questo argomento viene illustrato il processo di installazione e quindi viene illustrato come acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-and-licensing-ssma"></a>Installazione e gestione delle licenze SSMA  
Per usare SSMA, è prima necessario installare il programma client SSMA in un computer in grado di accedere sia all'istanza di origine di SAP ASE sia all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Per usare la migrazione dei dati sul lato server, è necessario installare il pacchetto di estensione e almeno uno dei provider SAP ASE (OLE DB o ADO.NET) nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti supportano la migrazione dei dati e l'emulazione delle funzioni di sistema di SAP ASE. Per le istruzioni di installazione, vedere [installazione di SSMA per SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md).  
  
Per avviare SSMA, fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per Sybase**, quindi selezionare **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per Sybase**. La prima volta che si avvia SSMA, viene visualizzata una finestra di dialogo di gestione delle licenze. È necessario concedere in licenza SSMA utilizzando un Windows Live ID prima di poter utilizzare SSMA. Le istruzioni per la gestione delle licenze sono incluse nelle istruzioni di installazione nell'argomento [installazione di SSMA per Sybase Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md) .  
  
## <a name="ssma-for-sap-ase-user-interface"></a>SSMA per l'interfaccia utente di SAP ASE  
Dopo aver installato e concesso in licenza SSMA, è possibile usare SSMA per eseguire la migrazione dei database SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Consente di acquisire familiarità con l'interfaccia utente di SSMA prima di iniziare. Il diagramma seguente illustra l'interfaccia utente per SSMA, tra cui Esplora metadati, metadati, barre degli strumenti, riquadro di output e riquadro elenco errori:  
  
![SSMA per l'interfaccia utente di SAP ASE](../../ssma/sybase/media/ssmaforsybaseuserinterface.jpg "SSMA per l'interfaccia utente di SAP ASE")  
  
Per avviare una migrazione, è necessario innanzitutto creare un nuovo progetto. Connettersi quindi a SAP ASE. Una volta completata la connessione, verrà visualizzata una gerarchia di database SAP ASE in Sybase Metadata Explorer. È quindi possibile fare clic con il pulsante destro del mouse su oggetti in Sybase Metadata Explorer per eseguire attività come la creazione di report che valutano le conversioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure. È anche possibile eseguire queste attività tramite le barre degli strumenti e i menu.  
  
È anche necessario connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Una volta stabilita la connessione, viene visualizzata una gerarchia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o database SQL di Azure in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati. Dopo la conversione degli schemi di SAP ASE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o negli schemi del database SQL di Azure, selezionare gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati, quindi caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.  
  
Dopo aver caricato gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure, è possibile tornare a Sybase Metadata Explorer ed eseguire la migrazione dei dati dai database di SAP ASE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nei database SQL di Azure.  
  
Per altre informazioni su queste attività e su come eseguirle, vedere [migrazione di database SAP ASE a SQL Server-database SQL di Azure &#40;&#41;SybaseToSQL ](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md).  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Esplora metadati  
SSMA contiene due Esplora metadati per esplorare ed eseguire azioni su database SAP ASE e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
#### <a name="sybase-metadata-explorer"></a>Esplora metadati Sybase  
Sybase Metadata Explorer Mostra informazioni sui database nell'istanza di origine di SAP ASE.  
  
Con Sybase Metadata Explorer è possibile eseguire le attività seguenti:  
  
-   Esplorare le tabelle in ogni database.  
  
-   Selezionare gli oggetti per la conversione, quindi convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nella sintassi del database SQL di Azure. Per altre informazioni, vedere [conversione di oggetti di database SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
-   Selezionare gli oggetti per la migrazione dei dati, quindi eseguire la migrazione dei dati da tali oggetti a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Per altre informazioni, vedere [migrazione dei dati di SAP ASE in SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
#### <a name="sql-server-or-sql-azure-metadata-explorer"></a>SQL Server o SQL Azure Esplora metadati  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in alternativa, SQL Azure Esplora metadati Mostra informazioni su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o database SQL di Azure. Quando ci si connette a un'istanza di o a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure, SSMA recupera i metadati relativi a tale istanza e li archivia nel file di progetto.  
  
È possibile usare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati per selezionare gli oggetti di database SAP ASE convertiti e quindi caricare (sincronizzare) tali oggetti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.  
  
Per ulteriori informazioni, vedere [caricamento di oggetti di database convertiti in SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
### <a name="metadata"></a>Metadati  
A destra di ogni Esplora metadati sono presenti schede che descrivono l'oggetto selezionato. Se ad esempio si seleziona una tabella in Sybase Metadata Explorer, verranno visualizzate sei schede: **tabella**, **SQL**, **mapping dei tipi**, **dati**, **Proprietà** e **report**. La scheda **report** contiene informazioni solo dopo la creazione di un report contenente l'oggetto selezionato. Se si seleziona una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati, verranno visualizzate tre schede: **tabella**, **SQL** e **dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. Tuttavia, è possibile modificare i metadati seguenti:  
  
-   In Sybase Metadata Explorer è possibile modificare le routine e i mapping dei tipi. Apportare queste modifiche prima di convertire gli schemi.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati è possibile modificare per le [!INCLUDE[tsql](../../includes/tsql-md.md)] stored procedure. Apportare queste modifiche prima di caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le modifiche apportate in Esplora metadati vengono riflesse nei metadati del progetto, non nei database di origine o di destinazione.  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA dispone di due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
#### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene i pulsanti per l'uso di progetti, la connessione a SAP ASE e la connessione al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure o. Questi pulsanti sono simili ai comandi del menu **file** .  
  
#### <a name="the-migration-toolbar"></a>Barra degli strumenti migrazione  
La barra degli strumenti di migrazione contiene i comandi seguenti:  
  
|Pulsante|Funzione|  
|----------|------------|  
|**Creazione di report**|Converte gli oggetti di SAP ASE selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, quindi crea un report che mostra l'esito positivo della conversione.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Sybase Metadata Explorer.|  
|**Converti schema**|Converte gli oggetti di SAP ASE selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti di database SQL di Azure o.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Sybase Metadata Explorer.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Prima di eseguire questo comando, è necessario convertire gli schemi di SAP ASE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o negli schemi del database SQL di Azure e quindi caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Sybase Metadata Explorer.|  
|**Stop**|Arresta il processo corrente, ad esempio la conversione di oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la sintassi del database SQL di Azure.|  
  
### <a name="menus"></a>Menu  
SSMA contiene i menu seguenti:  
  
|Menu|Descrizione|  
|--------|---------------|  
|**File**|Contiene i comandi per l'uso di progetti, la connessione a SAP ASE e la connessione al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure o.|  
|**Modifica**|Contiene i comandi per trovare e utilizzare il testo nelle pagine dei dettagli, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] la copia dal riquadro dettagli SQL. Contiene anche l'opzione **Gestisci segnalibri** , in cui è possibile visualizzare un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il comando **Sincronizza Esplora metadati** . Questa operazione Sincronizza gli oggetti tra la finestra di esplorazione dei metadati di Sybase e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati. Contiene anche i comandi per visualizzare e nascondere i riquadri di **output** e **Elenco errori** e i **layout** delle opzioni per gestire i layout.|  
|**Strumenti**|Contiene i comandi per la creazione di report, l'esportazione e la migrazione di oggetti e dati. Consente inoltre di accedere alle **Impostazioni globali** e alle finestre di dialogo **delle impostazioni del progetto** .|  
|**Tester**|Contiene i comandi per creare test case, visualizzare i risultati dei test e i comandi per la gestione dei backup del database.|  
|**?**|Consente di accedere alla guida di SSMA e alla finestra **di dialogo informazioni su** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** include i comandi per abilitare o disabilitare la visibilità del riquadro di output e del riquadro elenco errori:  
  
-   Il riquadro output Mostra i messaggi di stato di SSMA durante la conversione dell'oggetto, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori Mostra messaggi di errore, di avviso e informativi in un elenco che è possibile ordinare.  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database SAP ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Informazioni di riferimento sull'interfaccia utente &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
