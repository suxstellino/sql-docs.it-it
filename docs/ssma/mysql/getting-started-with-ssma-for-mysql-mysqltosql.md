---
title: Introduzione con SSMA per MySQL (MySQLToSQL) | Microsoft Docs
description: Informazioni sul processo di installazione di SQL Server Migration Assistant (SSMA) per MySQL e acquisire familiarità con l'interfaccia utente di SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Getting started, MySQL metadata explorer
- Getting started, SQL Server or SQL Azure metadata explorer
- Getting started,Installing and licensing
ms.assetid: 8ebfa061-be6f-4a07-923f-8dc832a82f70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9847b7a709fe64f3fcb7fca92ce1f8a4003843e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070256"
---
# <a name="getting-started-with-ssma-for-mysql-mysqltosql"></a>Introduzione a SSMA per MySQL (MySQLToSQL)
SQL Server Migration Assistant (SSMA) per MySQL consente di convertire rapidamente gli schemi di database MySQL in SQL Server o negli schemi del database SQL di Azure, caricare gli schemi risultanti in SQL Server o nel database SQL di Azure ed eseguire la migrazione dei dati da MySQL a SQL Server o al database SQL di Azure.  
  
In questo argomento viene illustrato il processo di installazione e quindi viene illustrato come acquisire familiarità con l'interfaccia utente di SSMA.  
  
## <a name="installing-ssma"></a>Installazione di SSMA  
Per usare SSMA, è prima necessario installare il programma client di SSMA in un computer in grado di accedere sia al database MySQL di origine sia all'istanza di destinazione di SQL Server o al database SQL di Azure. Installare quindi i provider MySQL (MySQL ODBC 5,1 driver (Trusted)) nel computer che esegue il programma client SSMA. Per le istruzioni di installazione, vedere [installazione di SSMA per MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
  
Per avviare SSMA, fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **SQL Server Migration Assistant per MySQL** e quindi fare clic su **SQL Server Migration Assistant per MySQL**.  
  
## <a name="ssma-for-mysql-user-interface"></a>SSMA per l'interfaccia utente di MySQL  
Dopo aver installato e concesso in licenza SSMA, è possibile usare SSMA per eseguire la migrazione di database MySQL a SQL Server o al database SQL di Azure. Consente di acquisire familiarità con l'interfaccia utente di SSMA prima di iniziare. Il diagramma seguente illustra l'interfaccia utente per SSMA, tra cui Esplora metadati, metadati, barre degli strumenti, riquadro di output e riquadro elenco errori:  
  
![SSMA per l'interfaccia utente grafica di MySQL](../../ssma/mysql/media/ssmaformysqlgui.gif "SSMA per l'interfaccia utente grafica di MySQL")  
  
Per avviare una migrazione, è necessario:  
  
1.  Creare un nuovo progetto.  
  
2.  Connettersi a un database MySQL.  
  
3.  Una volta completata la connessione, gli schemi MySQL verranno visualizzati in MySQL Metadata Explorer. Fare clic con il pulsante destro del mouse su oggetti in MySQL Metadata Explorer per eseguire attività quali la creazione di report per la valutazione delle conversioni in SQL Server/database SQL di Azure.  
  
È anche possibile eseguire queste attività usando le barre degli strumenti e i menu.  
  
È inoltre necessario connettersi a un'istanza di SQL Server. Una volta stabilita la connessione, viene visualizzata una gerarchia di database SQL Server in SQL Server Esplora metadati. Dopo la conversione degli schemi di MySQL in SQL Server schemi, selezionare gli schemi convertiti in SQL Server Esplora metadati, quindi sincronizzare gli schemi con SQL Server.  
  
È necessario connettersi al database SQL di Azure se è stato selezionato database SQL di Azure dalla finestra di dialogo Esegui migrazione all'elenco a discesa in nuovo progetto. Una volta completata la connessione, viene visualizzata una gerarchia di database SQL di Azure in Esplora metadati del database SQL di Azure. Dopo la conversione degli schemi MySQL negli schemi del database SQL di Azure, selezionare gli schemi convertiti in Esplora metadati del database SQL di Azure e quindi sincronizzare gli schemi con il database SQL di Azure.  
  
Dopo aver sincronizzato gli schemi convertiti con SQL Server o il database SQL di Azure, è possibile tornare a MySQL Metadata Explorer ed eseguire la migrazione dei dati da schemi MySQL in SQL Server o database SQL di Azure.  
  
Per altre informazioni su queste attività e su come eseguirle, vedere [migrazione di database MySQL a SQL Server-database SQL di Azure &#40;&#41;MySQLToSql ](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md).  
  
Le sezioni seguenti descrivono le funzionalità dell'interfaccia utente di SSMA.  
  
### <a name="metadata-explorers"></a>Esplora metadati  
SSMA contiene due Esplora metadati per esplorare ed eseguire azioni sui database MySQL e SQL Server.  
  
### <a name="mysql-metadata-explorer"></a>Esplora metadati MySQL  
MySQL Metadata Explorer Mostra informazioni sugli schemi MySQL. Con MySQL Metadata Explorer è possibile eseguire le attività seguenti:  
  
-   Esplorare gli oggetti in ogni schema.  
  
-   Selezionare oggetti per la conversione, quindi convertire gli oggetti in SQL Server sintassi. Per altre informazioni, vedere [conversione di database MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
-   Selezionare tabelle per la migrazione dei dati, quindi eseguire la migrazione dei dati da tali tabelle a SQL Server. Per altre informazioni, vedere [migrazione di dati MySQL in SQL Server-database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server o Esplora metadati del database SQL di Azure  
SQL Server o Esplora metadati del database SQL di Azure Mostra informazioni su un'istanza di SQL Server o sul database SQL di Azure. Quando ci si connette a un'istanza di SQL Server o al database SQL di Azure, SSMA recupera i metadati relativi a tale istanza e li archivia nel file di progetto.  
  
È possibile usare questa finestra di esplorazione dei metadati per selezionare gli oggetti di database MySQL convertiti e quindi sincronizzare tali oggetti con l'istanza di SQL Server o il database SQL di Azure.  
  
Per altre informazioni, vedere [sincronizzazione (MySQL per SQL Server/database SQL di Azure)](./loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
### <a name="metadata"></a>Metadati  
A destra di ogni Esplora metadati sono presenti schede che descrivono l'oggetto selezionato. Ad esempio, se si seleziona una tabella in MySQL Metadata Explorer, verranno visualizzate nove schede: **Table**, **SQL**, **Type mapping**, **Data**, **Settings**, **CharSet mapping**, **SQL modes**, **Properties** e **report**. La scheda **report** contiene informazioni solo dopo la creazione di un report che contiene l'oggetto selezionato. Se si seleziona una tabella in SQL Server Esplora metadati, verranno visualizzate tre schede: **tabella**, **SQL** e **dati**.  
  
La maggior parte delle impostazioni dei metadati è di sola lettura. Tuttavia, è possibile modificare i metadati seguenti:  
  
-   In MySQL Metadata Explorer è possibile modificare i mapping dei tipi, il mapping del set di caratteri e le modalità SQL. Per convertire i mapping dei tipi modificati o il mapping del set di caratteri o le modalità SQL, apportare modifiche prima di convertire gli schemi.  
  
-   In SQL Server Metadata Explorer è possibile modificare le proprietà della tabella e dell'indice nella scheda tabella. Per visualizzare queste modifiche in SQL Server, apportare queste modifiche prima di caricare gli schemi in SQL Server.  
  
Le modifiche apportate in Esplora metadati vengono riflesse nei metadati del progetto, non nei database di origine o di destinazione.  
  
### <a name="toolbars"></a>Barre degli strumenti  
SSMA dispone di due barre degli strumenti: una barra degli strumenti del progetto e una barra degli strumenti di migrazione.  
  
### <a name="the-project-toolbar"></a>Barra degli strumenti del progetto  
La barra degli strumenti del progetto contiene i pulsanti per l'uso di progetti, la connessione a MySQL e la connessione a SQL Server o al database SQL di Azure. Questi pulsanti sono simili ai comandi del menu **file** .  
  
### <a name="migration-toolbar"></a>Barra degli strumenti migrazione  
La tabella seguente illustra i comandi della barra degli strumenti di migrazione:  
  
|||  
|-|-|  
|**Button**|**Funzione**|  
|**Creazione di report**|Converte gli oggetti MySQL selezionati in SQL Server o oggetti di database SQL di Azure e quindi crea un report che mostra l'esito positivo della conversione.<br /><br />Questo comando è disabilitato, a meno che gli oggetti non siano selezionati in MySQL Metadata Explorer.|  
|**Converti schema**|Converte gli oggetti MySQL selezionati in SQL Server o oggetti di database SQL di Azure.<br /><br />Questo comando è disabilitato, a meno che gli oggetti non siano selezionati in MySQL Metadata Explorer.|  
|**Eseguire la migrazione dei dati**|Esegue la migrazione dei dati dal database MySQL a SQL Server o al database SQL di Azure. Prima di eseguire questo comando, è necessario convertire gli schemi MySQL in SQL Server o negli schemi del database SQL di Azure e quindi caricare gli oggetti in SQL Server o nel database SQL di Azure.<br /><br />Questo comando è disabilitato, a meno che gli oggetti non siano selezionati in MySQL Metadata Explorer.|  
|**Stop**|Arresta il processo corrente.|  
  
### <a name="menus"></a>Menu  
La tabella seguente illustra i menu SSMA.  
  
|||  
|-|-|  
|**Menu**|**Descrizione**|  
|**File**|Contiene i comandi per l'uso di progetti, la connessione a MySQL e la connessione a SQL Server o al database SQL di Azure.|  
|**Modifica**|Contiene i comandi per trovare e utilizzare il testo nelle pagine dei dettagli. Per aprire la finestra di dialogo **Gestisci segnalibri** , nel menu Modifica fare clic su Gestisci segnalibri. Nella finestra di dialogo viene visualizzato un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.|  
|**Visualizzazione**|Contiene il comando **Sincronizza Esplora metadati** . Che sincronizza gli oggetti tra Esplora metadati MySQL e SQL Server o Esplora metadati del database SQL di Azure. Contiene anche i comandi per visualizzare e nascondere i riquadri di **output** e **Elenco errori** e i **layout** delle opzioni da gestire con i layout.|  
|**Strumenti**|Contiene i comandi per la creazione di report, la conversione dello schema, l'aggiornamento dal database, la migrazione di oggetti e dati e il salvataggio come script. Consente inoltre di accedere alle impostazioni **globali,** alle impostazioni predefinite del progetto e alle finestre di dialogo **delle impostazioni del progetto** .|  
|**?**|Consente di accedere alla guida di SSMA e alla finestra **di dialogo informazioni su** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Riquadro di output e riquadro Elenco errori  
Il menu **Visualizza** include i comandi per abilitare o disabilitare la visibilità del riquadro di output e del riquadro elenco errori:  
  
-   Il riquadro output Mostra i messaggi di stato di SSMA durante la conversione dell'oggetto, la sincronizzazione degli oggetti e la migrazione dei dati.  
  
-   Il riquadro Elenco errori Mostra messaggi di errore, di avviso e informativi in un elenco ordinabile.  
  
## <a name="see-also"></a>Vedere anche  
[Informazioni di riferimento sull'interfaccia utente &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
[Migrazione dei dati MySQL in SQL Server-database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
