---
description: Gestione connessione SQL Server Compact Edition
title: Gestione connessione SQL Server Compact Edition | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlmobileconnection.connection.f1
- sql13.dts.designer.sqlmobileconnection.all.f1
helpviewer_keywords:
- SQL Server Compact, connection manager
- connections [Integration Services], SQL Server Compact
- connection managers [Integration Services], SQL Server Compact
ms.assetid: ba627d4d-41f4-49fc-a921-f534cde67770
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae61fcfcec740ee26a7fc2a57c0e03b0141e6e10
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425943"
---
# <a name="sql-server-compact-edition-connection-manager"></a>Gestione connessione SQL Server Compact Edition

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Una gestione connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact consente la connessione tra un pacchetto e un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact. La destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact inclusa in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa questa gestione connessione per caricare dati in una tabella di un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
> [!NOTE]  
>  In un computer a 64 bit è necessario eseguire pacchetti che si connettono a origini dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact in modalità a 32 bit. Il provider [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact usato in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per la connessione a origini dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact è disponibile solo nella versione a 32 bit.  
  
## <a name="configuration-the-sql-server-compact-edition-connection-manager"></a>Configurazione della gestione connessione SQL Server Compact Edition  
 Quando si aggiunge una gestione connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact a un pacchetto, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea una gestione connessione che in fase di esecuzione verrà risolta in una connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, imposta le proprietà della gestione connessione e quindi la aggiunge alla raccolta **Connections** del pacchetto.  
  
 La proprietà **ConnectionManagerType** della gestione connessione viene impostata su **SQLMOBILE**.  
  
 Per configurare la gestione connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, procedere nel modo seguente:  
  
-   Immettere una stringa di connessione che specifica il percorso del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
-   Se il database è protetto da password, specificare una password.  
  
-   Specificare il server in cui è archiviato il database.  
  
-   Indicare se la connessione creata dalla gestione connessione deve essere mantenuta in fase di esecuzione.  
  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per informazioni sulla configurazione di una gestione connessione a livello di programmazione, vedere l'articolo relativo a <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> e [Aggiunta di connessioni a livello di programmazione](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="sql-server-compact-edition-connection-manager-editor-connection-page"></a>Editor gestione connessione SQL Server Compact Edition (pagina Connessione)
  Utilizzare la finestra di dialogo **Editor gestione connessione SQL Server Compact Edition** per specificare le proprietà di connessione a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Per ulteriori informazioni sulla gestione connessione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition, vedere [Editor gestione connessione SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md).  
  
### <a name="options"></a>Opzioni  
 **Percorso e nome di file del database**  
 Immettere il percorso e il nome del file per il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 **Sfoglia**  
 Consente di individuare il file di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact desiderato usando la finestra di dialogo **Select SQL Server Compact Edition database** (Seleziona database di SQL Compact Edition).  
  
 **Password del database**  
 Consente di immettere la password per il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
## <a name="sql-server-compact-edition-connection-manager-editor-all-page"></a>Editor gestione connessione SQL Server Compact Edition (pagina Tutte)
  Utilizzare la finestra di dialogo **Editor gestione connessione SQL Server Compact Edition** per specificare le proprietà di connessione a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Per ulteriori informazioni sulla gestione connessione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition, vedere [Editor gestione connessione SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md).  
  
### <a name="options"></a>Opzioni  
 **AutoShrink Threshold**  
 Consente di specificare la quantità di spazio libero in percentuale consentito nel database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact al raggiungimento della quale verrà avviata l'operazione di compattazione automatica.  
  
 **Default Lock Escalation**  
 Consente di specificare il numero di blocchi di database acquisiti dal database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact prima di tentare l'escalation.  
  
 **Default Lock Timeout**  
 Consente di specificare l'intervallo di tempo predefinito (in millisecondi) per cui una transazione deve rimanere in attesa di un blocco.  
  
 **Flush Interval**  
 Consente di specificare l'intervallo di tempo (in secondi) dopo il quale le transazioni completate devono essere scaricate su disco.  
  
 **Locale Identifier**  
 Consente di specificare l'ID delle impostazioni locali (LCID) del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 **Max Buffer Size**  
 Consente di specificare la quantità massima di memoria in KB usata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact al raggiungimento della quale viene eseguito lo scaricamento dei dati su disco.  
  
 **Max Database Size**  
 Consente di specificare le dimensioni massime in MB del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 **Modalità**  
 Consente di specificare la modalità file in cui aprire il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact. Il valore predefinito di questa proprietà è **Read Write**.  
  
 Nella tabella seguente sono descritti i quattro valori disponibili per la proprietà Mode.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**Sola lettura**|Imposta l'accesso di sola lettura al database.|  
|**Read Write**|Imposta l'autorizzazione di lettura/scrittura per il database.|  
|**Exclusive**|Imposta l'accesso esclusivo al database.|  
|**Shared Read**|Specifica che altri utenti possono accedere contemporaneamente in lettura al database.|  
  
 **Persist Security Info**  
 Consente di specificare se le informazioni di sicurezza vengono restituite all'interno della stringa di connessione. Il valore predefinito dell'opzione è **False**.  
  
 **Temp File Directory**  
 Consente di specificare il percorso del file di database temporaneo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 **Origine dati**  
 Consente di specificare il nome del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 **Password**  
 Consente di immettere la password per il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
