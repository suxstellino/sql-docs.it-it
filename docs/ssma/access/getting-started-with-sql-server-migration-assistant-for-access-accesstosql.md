---
title: Introduzione a SQL Server Migration Assistant per l'accesso | Microsoft Docs
description: Introduzione all'uso di SSMA per convertire gli oggetti di database di Access in SQL Server o oggetti di database SQL di Azure, caricare gli oggetti risultanti ed eseguire la migrazione dei dati.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- error list pane
- getting started
- menus
- metadata explorers
- output pane
- toolbars
- user interface
- user interface overview
ms.assetid: 462a731f-08f1-44e1-9eeb-4deac6d2f6c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e05bc860ec1cc9f3703a1d1c17441c4e1ad3de15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044621"
---
# <a name="getting-started-with-sql-server-migration-assistant-for-access-accesstosql"></a>Introduzione a SQL Server Migration Assistant per Access (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per l'accesso consente di convertire rapidamente oggetti di database di Access in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti di database SQL di Azure, caricare gli oggetti risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure ed eseguire la migrazione dei dati dall'accesso al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure. Se necessario, è anche possibile collegare le tabelle di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o alle tabelle del database SQL di Azure in modo da poter continuare a usare le applicazioni front-end di accesso esistenti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il database SQL di Azure.  
  
Questo argomento introduce il processo di installazione e consente di acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-ssma"></a>Installazione di SSMA  
Per usare SSMA, è prima necessario installare il programma client di SSMA in un computer in grado di accedere ai database di cui si vuole eseguire la migrazione e all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Per le istruzioni di installazione, vedere [installazione di SQL Server Migration Assistant per l'accesso &#40;&#41;AccessToSQL ](../../ssma/access/installing-sql-server-migration-assistant-for-access-accesstosql.md).  
  
Per avviare SSMA, fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **SQL Server Migration Assistant per accesso** e quindi selezionare **SQL Server Migration Assistant per l'accesso**.  
  
## <a name="using-ssma"></a>Uso di SSMA  
Dopo l'installazione di SSMA, consente di acquisire familiarità con l'interfaccia utente di SSMA prima di utilizzare lo strumento per eseguire la migrazione dei database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Il diagramma seguente illustra l'interfaccia utente di SSMA, tra cui le finestre di esplorazione dei metadati, i metadati, le barre degli strumenti, il riquadro di output e l'elenco errori.  
  
![SSMA per l'interfaccia utente grafica di Access](../../ssma/access/media/ssmaforaccessgui.gif "SSMA per l'interfaccia utente grafica di Access")  
  
Per avviare una migrazione, creare un nuovo progetto e quindi aggiungere i database di Access per accedere a Esplora metadati. È quindi possibile fare clic con il pulsante destro del mouse su oggetti in Esplora metadati di Access per eseguire attività quali:
- Esportazione di un inventario di oggetti di database di Access in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.
- Creazione di report per la valutazione delle conversioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.
- Conversione degli schemi di accesso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o negli schemi del database SQL di Azure.

È anche possibile eseguire queste attività usando le barre degli strumenti e i menu.  
  
È inoltre necessario connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Una volta completata la connessione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Esplora metadati viene visualizzata una gerarchia di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dopo la conversione degli schemi di accesso agli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi, è possibile selezionare gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, quindi caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Se è stato selezionato database SQL di Azure dalla finestra di dialogo Esegui migrazione a elenco a discesa in nuovo progetto, è necessario connettersi al database SQL di Azure. Una volta completata la connessione, viene visualizzata una gerarchia di database SQL di Azure in Esplora metadati del database SQL di Azure. Dopo la conversione degli schemi di accesso agli schemi del database SQL di Azure, è possibile selezionare gli schemi convertiti in Esplora metadati del database SQL di Azure e quindi caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Dopo aver caricato gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure, è possibile tornare ad accedere a Esplora metadati ed eseguire la migrazione dei dati dai database di Access nei database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o del database SQL di Azure. Se necessario, è anche possibile collegare le tabelle di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o alle tabelle del database SQL di Azure.  
  
Per ulteriori informazioni su queste attività e su come eseguirle, vedere gli argomenti seguenti:  
  
-   [Preparazione dei database di Access per la migrazione](preparing-access-databases-for-migration-accesstosql.md)  
  
-   [Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
-   [Collegamento di applicazioni di accesso a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Esplora metadati  
SSMA contiene due Esplora metadati che è possibile usare per esplorare ed eseguire azioni per l'accesso e i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database di database SQL di Azure.  
  
#### <a name="access-metadata-explorer"></a>Accedi a Visualizzatore metadati  
Esplora metadati di Access Mostra informazioni sui database di Access aggiunti al progetto. Quando si aggiunge un database di Access, SSMA recupera i metadati relativi a tale database, ossia i metadati disponibili in Esplora metadati di Access.  
  
È possibile utilizzare Esplora metadati di Access per eseguire le attività seguenti:  
  
-   Esplorare le tabelle in ogni database di Access.  
  
-   Selezionare gli oggetti per la conversione e convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi. Per ulteriori informazioni, vedere [conversione di oggetti di database di Access](converting-access-database-objects-accesstosql.md).  
  
-   Selezionare gli oggetti per la migrazione dei dati ed eseguire la migrazione dei dati da tali oggetti a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [migrazione dei dati di accesso in SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
-   Collegare e scollegare l'accesso e le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabelle.  
  
#### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server o Esplora metadati del database SQL di Azure  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in alternativa, Esplora metadati del database SQL di Azure Mostra informazioni su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un database SQL di Azure. Quando ci si connette a un'istanza di o a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure, SSMA recupera i metadati relativi a tale istanza e li archivia nel file di progetto.  
  
È possibile usare l'SQL Server o Esplora metadati del database SQL di Azure per selezionare gli oggetti di database di Access convertiti e caricare (sincronizzare) tali oggetti nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.  
  
Per ulteriori informazioni, vedere [caricamento di oggetti di database convertiti in SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md).  
  
### <a name="metadata"></a>Metadati  
A destra di ogni Esplora metadati sono presenti schede che descrivono l'oggetto selezionato. Se ad esempio si seleziona una tabella in Esplora metadati di Access, verranno visualizzate quattro schede: **tabella**, **mapping dei tipi**, **proprietà** e **dati**. Se si seleziona una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, verranno visualizzate tre schede: **tabella**, **SQL** e **dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. Tuttavia, è possibile modificare i metadati seguenti:  
  
-   In Esplora metadati di Access è possibile modificare i mapping dei tipi. Assicurarsi di apportare queste modifiche prima di creare report o convertire gli schemi.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati è possibile modificare le proprietà delle tabelle e degli indici nella scheda **tabella** . apportare queste modifiche prima di caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [conversione di oggetti di database di Access](converting-access-database-objects-accesstosql.md).  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA dispone di due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
#### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene pulsanti per l'utilizzo di progetti, l'aggiunta di file di database di Access e la connessione al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure o. Questi pulsanti sono simili ai comandi del menu **file** .  
  
#### <a name="the-migration-toolbar"></a>Barra degli strumenti migrazione  
La barra degli strumenti di migrazione contiene i comandi seguenti:  
  
|Pulsante|Funzione|  
|----------|------------|  
|**Convertire, caricare ed eseguire la migrazione**|Converte i database di Access, carica gli oggetti convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure ed esegue la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure, il tutto in un unico passaggio.|  
|**Creazione di report**|Converte lo schema di accesso selezionato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la sintassi del database SQL di Azure, quindi crea un report che mostra l'esito positivo della conversione.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Esplora metadati di Access.|  
|**Converti schema**|Converte lo schema di accesso selezionato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o negli schemi del database SQL di Azure.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Esplora metadati di Access.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Prima di eseguire questo comando, è necessario convertire gli schemi di accesso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o gli schemi del database SQL di Azure e quindi caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure.<br /><br />Questo comando è disponibile solo quando gli oggetti sono selezionati in Esplora metadati di Access.|  
|**Stop**|Arresta il processo corrente, ad esempio la conversione di oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la sintassi del database SQL di Azure.|  
  
### <a name="menus"></a>Menu  
SSMA contiene i menu seguenti:  
  
|Menu|Descrizione|  
|--------|---------------|  
|**File**|Contiene i comandi per la migrazione guidata, l'utilizzo di progetti, l'aggiunta e la rimozione di file di database di Access e la connessione al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure.|  
|**Modifica**|Contiene i comandi per trovare e utilizzare il testo nelle pagine dei dettagli, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] la copia dal riquadro dettagli SQL. Per aprire la finestra di dialogo **Gestisci segnalibri** , scegliere Gestisci segnalibri dal menu Modifica. Nella finestra di dialogo viene visualizzato un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il comando **Sincronizza Esplora metadati** . Questo sincronizza gli oggetti tra Esplora metadati di accesso e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati del database SQL di Azure. Contiene anche i comandi per visualizzare e nascondere i riquadri di **output** e **Elenco errori** e i **layout** delle opzioni da gestire con i layout.|  
|**Strumenti**|Contiene i comandi per la creazione di report, l'esportazione di dati, la migrazione di oggetti e dati, la connessione di tabelle e l'accesso alle finestre di dialogo delle impostazioni globali e progetto.|  
|**?**|Consente di accedere alla guida di SSMA e alla finestra **di dialogo informazioni su** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** include i comandi per abilitare o disabilitare la visibilità del riquadro di output e del riquadro elenco errori:  
  
-   Il riquadro output Mostra i messaggi di stato di SSMA durante la conversione dell'oggetto, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori Mostra messaggi di errore, di avviso e informativi in un elenco che è possibile ordinare.  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
