---
title: Esplorare l'interfaccia SSMA per Oracle
description: Informazioni sul processo di SQL Server Migration Assistant (SSMA) per Oracle e acquisire familiarità con l'interfaccia utente di SSMA.
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
ms.openlocfilehash: 87e4f35fbb9b7a8979a501260e180a63ec31bddc
ms.sourcegitcommit: 3bb5ea67dc0d369b921f1bee4ffd4317aba2253c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107720450"
---
# <a name="explore-ssma-for-oracle-interface"></a>Esplorare l'interfaccia SSMA per Oracle
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per Oracle consente di convertire rapidamente gli schemi di database Oracle in schemi, caricare gli schemi risultanti in ed eseguire la migrazione dei dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Questo argomento presenta il processo di installazione e quindi consente di acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-ssma"></a>Installazione di SSMA  
Per usare SSMA, è innanzitutto necessario installare il programma client SSMA in un computer in grado di accedere sia al database Oracle di origine che all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi necessario installare un pacchetto di estensione e almeno uno dei provider Oracle (OLE DB o ) nel computer che [!INCLUDE[vstecado](../../includes/vstecado_md.md)] esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti supportano la migrazione dei dati e l'emulazione delle funzioni di sistema Oracle. Per istruzioni sull'installazione, vedere [Installazione di SSMA per Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Per avviare SSMA, fare clic sul pulsante **Start,** scegliere Tutti i **programmi,** **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per Oracle** e quindi fare clic su Migration Assistant per **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA per Oracle Interfaccia utente  
Dopo aver installato SSMA, è possibile usare SSMA per eseguire la migrazione di database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Consente di acquisire familiarità con l'interfaccia utente di SSMA prima di iniziare. Il diagramma seguente illustra l'interfaccia utente per SSMA, inclusi esplora metadati, metadati, barre degli strumenti, riquadro di output e riquadro elenco errori:  
  
![SSMA per l'interfaccia utente di Oracle](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA per l'interfaccia utente di Oracle")  
  
Per avviare una migrazione, è prima necessario creare un nuovo progetto. Quindi, ci si connette a un database Oracle. Al termine della connessione, gli schemi Oracle verranno visualizzati in Oracle Metadata Explorer. È quindi possibile fare clic con il pulsante destro del mouse sugli oggetti in Oracle Metadata Explorer per eseguire attività quali la creazione di report che valutano le conversioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È anche possibile eseguire queste attività usando le barre degli strumenti e i menu.  
  
È anche necessario connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Al termine di una connessione, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati verrà visualizzata una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gerarchia di database. Dopo aver convertito gli schemi Oracle in schemi, selezionare gli schemi convertiti in Esplora metadati e quindi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sincronizzare gli schemi con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Dopo aver sincronizzato gli schemi convertiti con , è possibile tornare a Esplora metadati Oracle ed eseguire la migrazione dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dati dagli schemi Oracle ai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
Per altre informazioni su queste attività e su come eseguirle, vedere [Migrating Oracle Databases to SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Visualizzatori metadati  
SSMA contiene due strumenti di esplorazione dei metadati per esplorare ed eseguire azioni su Oracle e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i database.  
  
#### <a name="oracle-metadata-explorer"></a>Oracle Metadata Explorer  
Oracle Metadata Explorer visualizza informazioni sugli schemi Oracle. Tramite Oracle Metadata Explorer è possibile eseguire le attività seguenti:  
  
-   Esplorare gli oggetti in ogni schema.  
  
-   Selezionare gli oggetti per la conversione e quindi convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi. Per altre informazioni, vedere [Conversione di schemi Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Selezionare le tabelle per la migrazione dei dati e quindi eseguire la migrazione dei dati da tali tabelle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Migrating Oracle Data into SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>SQL Server Metadata Explorer  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati visualizza informazioni su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando ci si connette a un'istanza di , SSMA recupera i metadati relativi a tale istanza e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] li archivia nel file di progetto.  
  
È possibile utilizzare Esplora metadati per selezionare gli oggetti di database Oracle convertiti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e quindi sincronizzare tali oggetti con l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Per altre informazioni, vedere [Caricamento di oggetti di database convertiti in SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Metadati  
A destra di ogni visualizzatore metadati sono presenti schede che descrivono l'oggetto selezionato. Ad esempio, se si seleziona una tabella in Oracle Metadata Explorer, verranno visualizzate sei schede: **Tabella**, **SQL**, Mapping **dei tipi, Report**, Proprietà e **Dati**.  La **scheda Report** contiene informazioni solo dopo aver creato un report contenente l'oggetto selezionato. Se si seleziona una tabella in Esplora metadati, verranno visualizzate tre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schede: **Tabella**, **SQL** e **Dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. È tuttavia possibile modificare i metadati seguenti:  
  
-   In Oracle Metadata Explorer è possibile modificare le procedure e i mapping dei tipi. Per convertire le procedure modificate e i mapping dei tipi, apportare modifiche prima di convertire gli schemi.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati è possibile modificare per le stored [!INCLUDE[tsql](../../includes/tsql-md.md)] procedure. Per visualizzare queste modifiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , apportare queste modifiche prima di caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le modifiche apportate in Esplora metadati vengono riflesse nei metadati del progetto, non nei database di origine o di destinazione.  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA ha due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
#### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene pulsanti per l'uso dei progetti, la connessione a Oracle e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi pulsanti sono simili ai comandi del menu **File.**  
  
#### <a name="migration-toolbar"></a>Barra degli strumenti di migrazione  
La tabella seguente mostra i comandi della barra degli strumenti di migrazione:  
  
|Pulsante|Funzione|  
|------|--------|  
|**Creazione di report**|Converte gli oggetti Oracle selezionati in sintassi e quindi crea un report che mostra la riuscita [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] della conversione.<br /><br />Questo comando è disabilitato a meno che non vengano selezionati oggetti in Oracle Metadata Explorer.|  
|**Convertire lo schema**|Converte gli oggetti Oracle selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti .<br /><br />Questo comando è disabilitato a meno che non siano selezionati oggetti in Esplora metadati Oracle.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Prima di eseguire questo comando, è necessario convertire gli schemi Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi e quindi caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Questo comando è disabilitato a meno che non siano selezionati oggetti in Esplora metadati Oracle.|  
|**Stop**|Arresta il processo corrente.|  
  
### <a name="menus"></a>Menu  
La tabella seguente mostra i menu di SSMA.  
  
|Menu|Descrizione|  
|----|-----------|  
|**File**|Contiene comandi per l'utilizzo di progetti, la connessione a Oracle e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Modifica**|Contiene i comandi per la ricerca e l'uso del testo nelle pagine dei dettagli, ad esempio la [!INCLUDE[tsql](../../includes/tsql-md.md)] copia dal riquadro dei dettagli SQL. Contiene anche **l'opzione Gestisci segnalibri,** in cui sarà possibile visualizzare un elenco di segnalibri esistenti. È possibile usare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il **comando Synchronize Metadata Explorers.** In questo modo gli oggetti vengono sincronizzati tra Oracle Metadata Explorer e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Metadata Explorer. Contiene anche comandi per visualizzare e nascondere i **riquadri Output** ed **Elenco** errori e un'opzione **Layout per** gestire i layout.|  
|**Strumenti**|Contiene comandi per creare report ed eseguire la migrazione di oggetti e dati. Fornisce anche l'accesso alle **finestre di dialogo Impostazioni globali** e **Impostazioni** progetto.|  
|**Tester**|Contiene i comandi per la creazione e l'uso di test case, repository e sistema di gestione dei backup.|  
|**?**|Fornisce l'accesso alla Guida di SSMA e alla **finestra di** dialogo Informazioni su .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** fornisce comandi per attivare o disattivare la visibilità del riquadro Output e del riquadro Elenco errori:  
  
-   Il riquadro di output mostra i messaggi di stato da SSMA durante la conversione di oggetti, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori mostra messaggi informativi, di avviso e di errore in un elenco ordinabile.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati Oracle in SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Interfaccia utente riferimento &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
