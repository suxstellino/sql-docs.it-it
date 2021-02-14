---
title: Installare SQL Server tramite un file di configurazione | Microsoft Docs
description: È possibile usare il programma di installazione di SQL Server per generare un file di configurazione per la distribuzione di SQL Server in tutta l'organizzazione con una configurazione uniforme.
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: a832153a-6775-4bed-83f0-55790766d885
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b5b38250065d1c7474d5f500284a082b421332c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348347"
---
# <a name="install-sql-server-using-a-configuration-file"></a>Installare SQL Server tramite un file di configurazione

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il programma di installazione consente di generare un file di configurazione in base all'impostazione predefinita del sistema e ai dati di input inseriti in fase di esecuzione. È possibile utilizzare il file di configurazione per distribuire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in tutta l'organizzazione con la stessa configurazione, nonché standardizzare le installazioni manuali nell'organizzazione creando un file batch che consente di avviare Setup.exe. 
 
Questo articolo viene aggiornato per le versioni di SQL Server 2016 e successive. Per le versioni precedenti di SQL Server, vedere [Installare SQL Server 2014 tramite un file di configurazione](/previous-versions/sql/2014/database-engine/install-windows/install-sql-server-using-a-configuration-file?view=sql-server-2014&preserve-view=true).
 
Il programma di installazione supporta l'utilizzo del file di configurazione solo tramite il prompt dei comandi. L'ordine di elaborazione dei parametri durante l'utilizzo del file di configurazione viene indicato di seguito:  
  
-  Il file di configurazione sovrascrive le impostazioni predefinite in un pacchetto.  
  
-   I valori della riga di comando sovrascrivono quelli presenti nel file di configurazione.  
  
 Il file di configurazione può essere utilizzato per tenere traccia dei parametri e dei valori per ogni installazione e consente pertanto di verificare e controllare le installazioni. 
  
## <a name="configuration-file-structure"></a>Struttura dei file di configurazione  
 Il file ConfigurationFile.ini è un file di testo con parametri (coppia nome/valore) e commenti descrittivi. 
  
 Di seguito è riportato un esempio di un file ConfigurationFile.ini:  
  
```  
; Microsoft SQL Server Configuration file  
[OPTIONS]  
; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE.  
; This is a required parameter.  
ACTION="Install"  
; Specifies features to install, uninstall, or upgrade.  
; The list of top-level features include SQL, AS, RS, IS, and Tools.  
; The SQL feature will install the database engine, replication, and full-text.  
; The Tools feature will install Management Tools, Books online,   
; SQL Server Data Tools, and other shared components.  
FEATURES=SQL,Tools  
```  
  
#### <a name="how-to-generate-a-configuration-file"></a>Modalità di generazione di un file di configurazione  
  
1. Inserire il supporto di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella cartella radice fare doppio clic sul file Setup.exe. Per eseguire l'installazione da una condivisione di rete, individuare la cartella radice nella condivisione, quindi fare doppio clic sul file Setup.exe. 
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express Edition non crea automaticamente un file di configurazione. Il comando seguente avvia l'installazione e crea un file di configurazione. 
    >   
    >  SETUP.exe /UIMODE=Normal /ACTION=INSTALL  
  
2. Seguire la procedura guidata nella pagina **Inizio installazione** . Il percorso del file di configurazione viene specificato nella pagina **Inizio installazione** nella sezione relativa al percorso del file di configurazione. Per altre informazioni su come installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Installare SQL Server dall'Installazione guidata &#40;programma di installazione&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). 
  
3. Annullare l'installazione senza completarla per generare il file INI. 
  
    > [!NOTE]  
    >  L'infrastruttura del programma di installazione scrive tutti i parametri appropriati per le azioni eseguite, ad eccezione delle informazioni riservate come le password. Anche il parametro /IAcceptSQLServerLicenseTerms non viene scritto nel file di configurazione, di conseguenza è necessario apportare una modifica al file oppure fornire un valore al prompt dei comandi. Per altre informazioni, vedere [Installazione di SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md). Viene inoltre incluso un valore per i parametri booleani per cui non viene in genere fornito alcun valore attraverso il prompt dei comandi. 
  
## <a name="using-the-configuration-file-to-install-ssnoversion"></a>Uso del file di configurazione per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Il file di configurazione può essere utilizzato solo nelle installazioni da riga di comando. 
  
> [!NOTE]  
> Se è necessario apportare modifiche al file di configurazione, è consigliabile crearne una copia e utilizzare quest'ultima. 
  
### <a name="how-to-use-a-configuration-file-to-install-a-stand-alone-ssnoversion-instance"></a>Modalità di utilizzo di un file di configurazione per installare un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Eseguire l'installazione tramite il prompt dei comandi e specificare il file ConfigurationFile.ini utilizzando il parametro *ConfigurationFile* . 
  
### <a name="how-to-use-a-configuration-file-to-prepare-and-complete-an-image-of-a-stand-alone-ssnoversion-instance-sysprep"></a>Modalità di utilizzo di un file di configurazione per preparare e completare un'immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SysPrep)  
  
1. Per preparare una o più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e configurarle nello stesso computer. 
  
    - Eseguire **Preparazione immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina **Avanzate** di Centro installazione e acquisire il file di configurazione della preparazione immagine. 
  
    - Utilizzare lo stesso file di configurazione della preparazione immagine come un modello per preparare più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
    - Eseguire **Completamento immagine di un'istanza autonoma predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina **Avanzate** di Centro installazione per configurare le istanze predisposte nel computer. 
  
2. Per preparare un'immagine del sistema operativo comprendente un'istanza predisposta non configurata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando lo strumento SysPrep di Windows. 
  
    -   Eseguire **Preparazione immagine di un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina Avanzate di Centro installazione e acquisire il file di configurazione della preparazione immagine. 
  
    -   Eseguire **Completamento immagine di un'istanza autonoma predisposta di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** nella pagina **Avanzate** di Centro installazione, ma annullarlo nella pagina **Inizio completamento** dopo avere acquisito il file di configurazione del completamento. 
  
    -   Il file di configurazione del completamento immagine può essere archiviato con l'immagine Windows per rendere automatica la configurazione delle istanze predisposte. 
  
### <a name="how-to-install-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Modalità di installazione di un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il file di configurazione  
  
1. Opzione di installazione integrata (creazione di un cluster di failover a nodo singolo in un nodo e utilizzo di AddNode per i nodi aggiuntivi):  
  
    -   Eseguire l'opzione per l'installazione del cluster di failover e acquisire il file di configurazione in cui sono elencate tutte le impostazioni di installazione. 
  
    -   Eseguire l'installazione del cluster di failover da riga di comando specificando il parametro *ConfigurationFile* . 
  
    -   In un nodo da aggiungere eseguire AddNode per acquisire il file ConfigurationFile.ini applicabile al cluster di failover esistente. 
  
    -   Eseguire AddNode dalla riga di comando in tutti i nodi aggiuntivi per cui verrà creato il join del cluster di failover, specificando lo stesso file di configurazione utilizzando il parametro ConfigurationFile. 
  
2. Opzione di installazione avanzata (preparazione del cluster di failover in tutti i nodi del cluster ed esecuzione della funzionalità di completamento nel nodo proprietario del disco condiviso al termine della preparazione):  
  
    -   Eseguire **Prepara** in uno dei nodi e acquisire il file ConfigurationFile.ini. 
  
    -   Specificare lo stesso file ConfigurationFile.ini al programma di installazione in tutti i nodi che verranno preparati per il cluster di failover. 
  
    -   Dopo che i nodi sono stati preparati, eseguire un'operazione di completamento del cluster di failover nel nodo proprietario del disco condiviso e acquisire il file ConfigurationFile.ini. 
  
    -   A questo punto è possibile specificare il file ConfigurationFile.ini per completare il cluster di failover. 
  
### <a name="how-to-add-or-remove-a-node-to-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Modalità di aggiunta o rimozione di un nodo a un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il file di configurazione  
  
-   Se si dispone di un file di configurazione utilizzato in precedenza per aggiungere un nodo a un cluster di failover o rimuoverlo, è possibile riutilizzarlo per aggiungere o rimuovere altri nodi. 
  
### <a name="how-to-upgrade-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Modalità di aggiornamento di un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il file di configurazione  
  
1. Eseguire l'aggiornamento nel nodo passivo e acquisire il file ConfigurationFile.ini. A questo scopo è possibile eseguire l'aggiornamento effettivo oppure uscire senza avere effettuato questa operazione. 
  
2. In tutti i nodi aggiuntivi da aggiornare specificare il file ConfigurationFile.ini per completare il processo. 
  
## <a name="sample-syntax"></a>Sintassi di esempio  
 Di seguito vengono riportati alcuni esempi sull'utilizzo del file di configurazione:  
  
-   Per specificare il file di configurazione al prompt dei comandi:  
  
```  
Setup.exe /ConfigurationFile=MyConfigurationFile.INI  
```  
  
-   Per specificare le password al prompt dei comandi anziché nel file di configurazione:  
  
```  
Setup.exe /SQLSVCPASSWORD="************" /AGTSVCPASSWORD="************" /ASSVCPASSWORD="************" /ISSVCPASSWORD="************" /RSSVCPASSWORD="************" /ConfigurationFile=MyConfigurationFile.INI  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Installare SQL Server dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md)   
 [Installazione del cluster di failover di SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)   
 [Aggiornare un'istanza del cluster di failover di SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)  
  
