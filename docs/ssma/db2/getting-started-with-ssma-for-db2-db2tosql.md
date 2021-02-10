---
description: Introduzione con SSMA per DB2 (DB2ToSQL)
title: Introduzione con SSMA per DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 48ca32fc-1830-4d1f-add7-480ba5ad02e8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1829101a62a3f2602e32b6e4d3535292a90a140d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069516"
---
# <a name="getting-started-with-ssma-for-db2-db2tosql"></a>Introduzione con SSMA per DB2 (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per DB2 consente di convertire rapidamente gli schemi di database DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi, caricare gli schemi risultanti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed eseguire la migrazione dei dati da DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
In questo argomento viene illustrato il processo di installazione e quindi viene illustrato come acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-ssma"></a>Installazione di SSMA  
Per utilizzare SSMA, è necessario innanzitutto installare il programma client SSMA in un computer in grado di accedere sia al database DB2 di origine sia all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Provider DB2 OLEDB nel computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti supportano la migrazione dei dati e l'emulazione delle funzioni di sistema DB2. Per le istruzioni di installazione, vedere [installazione di SSMA per DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md).  
  
Per avviare SSMA, fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per db2**, quindi fare clic su **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant per DB2**.  
  
## <a name="ssma-for-db2-user-interface"></a>Interfaccia utente di SSMA per DB2  
Dopo l'installazione di SSMA, è possibile usare SSMA per eseguire la migrazione dei database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Consente di acquisire familiarità con l'interfaccia utente di SSMA prima di iniziare. Il diagramma seguente illustra l'interfaccia utente per SSMA, tra cui Esplora metadati, metadati, barre degli strumenti, riquadro di output e riquadro elenco errori:  
  
![Interfaccia utente di SSMA](../../ssma/db2/media/ssma_db2_ui.png "Interfaccia utente di SSMA")  
  
Per avviare una migrazione, è necessario innanzitutto creare un nuovo progetto. Quindi, ci si connette a un database DB2. Una volta completata la connessione, gli schemi DB2 verranno visualizzati in DB2 Metadata Explorer. È quindi possibile fare clic con il pulsante destro del mouse su oggetti in DB2 Metadata Explorer per eseguire attività quali la creazione di report per la valutazione delle conversioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È anche possibile eseguire queste attività usando le barre degli strumenti e i menu.  
  
È inoltre necessario connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Una volta completata la connessione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Esplora metadati verrà visualizzata una gerarchia di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dopo la conversione degli schemi DB2 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in schemi, selezionare gli schemi convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, quindi sincronizzare gli schemi con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Dopo aver sincronizzato gli schemi convertiti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile tornare a DB2 Metadata Explorer ed eseguire la migrazione dei dati da schemi DB2 nei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database di.  
  
Per ulteriori informazioni su queste attività e su come eseguirle, vedere [migrazione di database DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Esplora metadati  
SSMA contiene due Esplora metadati per esplorare ed eseguire azioni su DB2 e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
#### <a name="db2-metadata-explorer"></a>Esplora metadati DB2  
DB2 Metadata Explorer Mostra informazioni sugli schemi DB2. Con Esplora metadati DB2 è possibile eseguire le attività seguenti:  
  
-   Esplorare gli oggetti in ogni schema.  
  
-   Selezionare oggetti per la conversione, quindi convertire gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi. Per ulteriori informazioni, vedere [conversione di schemi DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
-   Selezionare tabelle per la migrazione dei dati, quindi eseguire la migrazione dei dati da tali tabelle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [migrazione di database DB2 a SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Esplora metadati SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati Mostra informazioni su un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando ci si connette a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA recupera i metadati relativi a tale istanza e li archivia nel file di progetto.  
  
È possibile utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati per selezionare oggetti di database DB2 convertiti, quindi sincronizzare tali oggetti con l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="metadata"></a>Metadati  
A destra di ogni Esplora metadati sono presenti schede che descrivono l'oggetto selezionato. Se ad esempio si seleziona una tabella in DB2 Metadata Explorer, verranno visualizzate sei schede: **tabella**, **SQL**, **mapping dei tipi, report**, **Proprietà** e **dati**. La scheda **report** contiene informazioni solo dopo la creazione di un report che contiene l'oggetto selezionato. Se si seleziona una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati, verranno visualizzate tre schede: **tabella**, **SQL** e **dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. Tuttavia, è possibile modificare i metadati seguenti:  
  
-   In DB2 Metadata Explorer è possibile modificare le routine e i mapping dei tipi. Per convertire le procedure modificate e i mapping dei tipi, apportare modifiche prima di convertire gli schemi.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati è possibile modificare l'oggetto [!INCLUDE[tsql](../../includes/tsql-md.md)] per le stored procedure. Per visualizzare queste modifiche in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , apportare queste modifiche prima di caricare gli schemi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le modifiche apportate in Esplora metadati vengono riflesse nei metadati del progetto, non nei database di origine o di destinazione.  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA dispone di due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
#### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene i pulsanti per l'utilizzo di progetti, la connessione a DB2 e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi pulsanti sono simili ai comandi del menu **file** .  
  
#### <a name="migration-toolbar"></a>Barra degli strumenti migrazione  
La tabella seguente illustra i comandi della barra degli strumenti di migrazione:  
  
|Pulsante|Funzione|  
|------|--------|  
|**Creazione di report**|Converte gli oggetti DB2 selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, quindi crea un report che mostra l'esito positivo della conversione.<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in DB2 Metadata Explorer.|  
|**Converti schema**|Converte gli oggetti DB2 selezionati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti.<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in DB2 Metadata Explorer.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Prima di eseguire questo comando, è necessario convertire gli schemi DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schemi, quindi caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Questo comando è disabilitato, a meno che non vengano selezionati oggetti in DB2 Metadata Explorer.|  
|**Stop**|Arresta il processo corrente.|  
  
### <a name="menus"></a>Menu  
La tabella seguente illustra i menu SSMA.  
  
|Menu|Descrizione|  
|----|-----------|  
|**File**|Contiene i comandi per l'utilizzo di progetti, la connessione a DB2 e la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Modifica**|Contiene i comandi per trovare e utilizzare il testo nelle pagine dei dettagli, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] la copia dal riquadro dettagli SQL. Contiene anche l'opzione **Gestisci segnalibri** , in cui sarà possibile visualizzare un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il comando **Sincronizza Esplora metadati** . Che sincronizza gli oggetti tra Esplora metadati DB2 e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati. Contiene anche i comandi per visualizzare e nascondere i riquadri di **output** e **Elenco errori** e i **layout** delle opzioni per gestire i layout.|  
|**Strumenti**|Contiene i comandi per creare report ed eseguire la migrazione di oggetti e dati. Consente inoltre di accedere alle **Impostazioni globali** e alle finestre di dialogo **delle impostazioni del progetto** .|  
|**?**|Consente di accedere alla guida di SSMA e alla finestra **di dialogo informazioni su** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** include i comandi per abilitare o disabilitare la visibilità del riquadro di output e del riquadro elenco errori:  
  
-   Il riquadro output Mostra i messaggi di stato di SSMA durante la conversione dell'oggetto, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori Mostra messaggi di errore, di avviso e informativi in un elenco ordinabile.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati DB2 in SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
[Informazioni di riferimento sull'interfaccia utente &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
