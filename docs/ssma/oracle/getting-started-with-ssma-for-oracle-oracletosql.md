---
title: Introduzione con SSMA per Oracle (OracleToSQL) | Microsoft Docs
description: Informazioni sul processo di installazione di SQL Server Migration Assistant (SSMA) per Oracle e acquisire familiarità con l'interfaccia utente di SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA for Oracle, Metadata Explorers
- SSMA for Oracle, Toolbars
ms.assetid: df79664c-972e-4bef-865a-ce609789fee7
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ce9d4908113daddf51707b4b7a80c4085defc228
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076722"
---
# <a name="getting-started-with-ssma-for-oracle-oracletosql"></a>Introduzione a SSMA per Oracle (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per Oracle consente di convertire rapidamente gli schemi di database Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi, caricare gli schemi risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed eseguire la migrazione dei dati da Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
In questo argomento viene illustrato il processo di installazione e quindi viene illustrato come acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-ssma"></a>Installazione di SSMA  
Per utilizzare SSMA, è necessario innanzitutto installare il programma client SSMA in un computer in grado di accedere sia al database Oracle di origine che all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi necessario installare un pacchetto di estensione e almeno uno dei provider Oracle (OLE DB o [!INCLUDE[vstecado](../../includes/vstecado_md.md)] ) nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti supportano la migrazione dei dati e l'emulazione di funzioni di sistema Oracle. Per le istruzioni di installazione, vedere [installazione di SSMA per Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Per avviare SSMA, fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per oracle** e quindi fare clic su **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA per l'interfaccia utente di Oracle  
Dopo l'installazione di SSMA, è possibile utilizzare SSMA per eseguire la migrazione dei database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Consente di acquisire familiarità con l'interfaccia utente di SSMA prima di iniziare. Il diagramma seguente illustra l'interfaccia utente per SSMA, tra cui Esplora metadati, metadati, barre degli strumenti, riquadro di output e riquadro elenco errori:  
  
![SSMA per l'interfaccia utente di Oracle](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA per l'interfaccia utente di Oracle")  
  
Per avviare una migrazione, è necessario innanzitutto creare un nuovo progetto. Quindi, si esegue la connessione a un database Oracle. Una volta completata la connessione, gli schemi Oracle verranno visualizzati in Oracle Metadata Explorer. È quindi possibile fare clic con il pulsante destro del mouse su oggetti in Oracle Metadata Explorer per eseguire attività quali la creazione di report per la valutazione delle conversioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È anche possibile eseguire queste attività usando le barre degli strumenti e i menu.  
  
È inoltre necessario connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Una volta completata la connessione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Esplora metadati verrà visualizzata una gerarchia di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dopo la conversione degli schemi Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in schemi, selezionare gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, quindi sincronizzare gli schemi con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Dopo aver sincronizzato gli schemi convertiti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile tornare a Oracle Metadata Explorer ed eseguire la migrazione dei dati da schemi Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
Per ulteriori informazioni su queste attività e su come eseguirle, vedere [migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Esplora metadati  
SSMA contiene due Esplora metadati per esplorare ed eseguire azioni su Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
#### <a name="oracle-metadata-explorer"></a>Esplora metadati Oracle  
In Oracle Metadata Explorer vengono visualizzate informazioni sugli schemi Oracle. Utilizzando Esplora metadati Oracle, è possibile eseguire le attività seguenti:  
  
-   Esplorare gli oggetti in ogni schema.  
  
-   Selezionare oggetti per la conversione, quindi convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi. Per ulteriori informazioni, vedere la pagina relativa alla [conversione di schemi Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Selezionare tabelle per la migrazione dei dati, quindi eseguire la migrazione dei dati da tali tabelle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [migrazione di dati Oracle in SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Esplora metadati SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati Mostra informazioni su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA recupera i metadati relativi a tale istanza e li archivia nel file di progetto.  
  
È possibile utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati per selezionare oggetti di database Oracle convertiti, quindi sincronizzare tali oggetti con l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Per ulteriori informazioni, vedere [caricamento di oggetti di database convertiti in SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Metadati  
A destra di ogni Esplora metadati sono presenti schede che descrivono l'oggetto selezionato. Se ad esempio si seleziona una tabella in Oracle Metadata Explorer, verranno visualizzate sei schede: **tabella**, **SQL**, **mapping dei tipi, report**, **Proprietà** e **dati**. La scheda **report** contiene informazioni solo dopo la creazione di un report che contiene l'oggetto selezionato. Se si seleziona una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, verranno visualizzate tre schede: **tabella**, **SQL** e **dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. Tuttavia, è possibile modificare i metadati seguenti:  
  
-   In Oracle Metadata Explorer è possibile modificare le routine e i mapping dei tipi. Per convertire le procedure modificate e i mapping dei tipi, apportare modifiche prima di convertire gli schemi.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati è possibile modificare l'oggetto [!INCLUDE[tsql](../../includes/tsql-md.md)] per le stored procedure. Per visualizzare queste modifiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , apportare queste modifiche prima di caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le modifiche apportate in Esplora metadati vengono riflesse nei metadati del progetto, non nei database di origine o di destinazione.  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA dispone di due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
#### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene pulsanti per l'utilizzo di progetti, la connessione a Oracle e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi pulsanti sono simili ai comandi del menu **file** .  
  
#### <a name="migration-toolbar"></a>Barra degli strumenti migrazione  
La tabella seguente illustra i comandi della barra degli strumenti di migrazione:  
  
|Pulsante|Funzione|  
|------|--------|  
|**Creazione di report**|Converte gli oggetti Oracle selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, quindi crea un report che mostra l'esito positivo della conversione.<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in Esplora metadati Oracle.|  
|**Converti schema**|Converte gli oggetti Oracle selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti.<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in Esplora metadati Oracle.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Prima di eseguire questo comando, è necessario convertire gli schemi Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi, quindi caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in Esplora metadati Oracle.|  
|**Stop**|Arresta il processo corrente.|  
  
### <a name="menus"></a>Menu  
La tabella seguente illustra i menu SSMA.  
  
|Menu|Descrizione|  
|----|-----------|  
|**File**|Contiene i comandi per l'utilizzo di progetti, la connessione a Oracle e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Modifica**|Contiene i comandi per trovare e utilizzare il testo nelle pagine dei dettagli, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] la copia dal riquadro dettagli SQL. Contiene anche l'opzione **Gestisci segnalibri** , in cui sarà possibile visualizzare un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il comando **Sincronizza Esplora metadati** . Che sincronizza gli oggetti tra Esplora metadati Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati. Contiene anche i comandi per visualizzare e nascondere i riquadri di **output** e **Elenco errori** e i **layout** delle opzioni per gestire i layout.|  
|**Strumenti**|Contiene i comandi per creare report ed eseguire la migrazione di oggetti e dati. Consente inoltre di accedere alle **Impostazioni globali** e alle finestre di dialogo **delle impostazioni del progetto** .|  
|**Tester**|Contiene i comandi per la creazione e l'utilizzo di test case, repository e sistemi di gestione di backup.|  
|**?**|Consente di accedere alla guida di SSMA e alla finestra **di dialogo informazioni su** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** include i comandi per abilitare o disabilitare la visibilità del riquadro di output e del riquadro elenco errori:  
  
-   Il riquadro output Mostra i messaggi di stato di SSMA durante la conversione dell'oggetto, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori Mostra messaggi di errore, di avviso e informativi in un elenco ordinabile.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati Oracle in SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Informazioni di riferimento sull'interfaccia utente &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
