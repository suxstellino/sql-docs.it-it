---
description: Stretch Database
title: Stretch Database
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database
ms.assetid: ce6db775-21a5-40bc-95a1-f560376d4ee2
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c2d2acc7ba0ac0f57038b3a72f62e6b67383b1bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077412"
---
# <a name="stretch-database"></a>Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Stretch Database esegue la migrazione dei dati non attivi in modo trasparente e sicuro al cloud di Microsoft Azure.  
  
 Per iniziare subito a usare Stretch Database, vedere [Introduzione all'esecuzione della procedura guidata Abilitare il database per la funzionalità Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md).  
  
## <a name="what-are-the-benefits-of-stretch-database"></a>Vantaggi di Stretch Database  
 Stretch Database offre i vantaggi seguenti:  
  
 **Fornisce disponibilità conveniente per i dati ad accesso sporadico**  
 I dati transazionali usati di frequente e raramente vengono estesi dinamicamente da SQL Server a Microsoft Azure con SQL Server Stretch Database. A differenza della tipica archiviazione dei dati ad accesso sporadico, i dati sono sempre online e disponibili per eseguire query. È possibile fornire sequenze temporali di durata maggiore per la conservazione dei dati senza ridurre lo spazio per le tabelle di grandi dimensioni come la cronologia degli ordini cliente. È possibile trarre vantaggio dal basso costo di Azure invece di aggiornare la costosa archiviazione locale. Scegliere il piano tariffario e configurare le impostazioni nel portale di Azure per mantenere il controllo su prezzo e costi. Passare a un piano superiore o inferiore secondo le esigenze. Per informazioni dettagliate, vedere [Prezzi di SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
 **Non richiede modifiche a query o applicazioni**  
 Consente di accedere direttamente ai dati di SQL Server a prescindere che si trovino in locale o che siano stati estesi al cloud.  Si impostano i criteri che determinano dove vengono archiviati i dati e SQL Server gestisce lo spostamento dei dati in background. L'intera tabella è sempre online e disponibile per le query. Stretch Database non richiede modifiche alle query o alle applicazioni esistenti: il percorso dei dati è completamente disponibile per l'applicazione.  
  
 **Semplifica la manutenzione dei dati locali**  
 Consente di ridurre la manutenzione e l’archiviazione locale dei dati. I backup dei dati in locale vengono eseguiti più velocemente e terminano all'interno della finestra di manutenzione. Esegue in automatico il backup della parte dei dati nel cloud. Le esigenze di archiviazione locale vengono notevolmente ridotte. L’archiviazione di Azure può consentire un risparmio dell’80% rispetto all’aggiunta di unità SSD locali.  
  
 **I dati rimangono al sicuro anche durante la migrazione**  
 Consente di estendere in massima tranquillità e in modo sicure le applicazioni più importanti nel cloud. La funzionalità Always Encrypted di SQL Server offre la crittografia dei dati in transito. La sicurezza a livello di riga e altre funzionalità di sicurezza avanzate di SQL Server sono utilizzabili anche con Stretch Database per proteggere i dati.  
  
## <a name="what-does-stretch-database-do"></a>Funzionalità di Stretch Database  
 Dopo l'abilitazione di Stretch Database per un'istanza di SQL Server e di un database e dopo la selezione di almeno una tabella, Stretch Database avvia automaticamente la migrazione dei dati ad accesso sporadico in Azure.  
  
-   Se i dati ad accesso sporadico vengono archiviati in una tabella separata, è possibile eseguire la migrazione dell'intera tabella.  
  
-   Se la tabella contiene dati usati più di frequente e dati usati meno di frequente, è possibile specificare una funzione di filtro per selezionare le righe di cui eseguire la migrazione.

**Non è necessario modificare le query e le applicazioni client esistenti.** Si continua ad avere accesso trasparente ai dati locali e remoti, anche durante la migrazione dei dati. Potrebbe essere riscontrata una leggera latenza per le query remote, che si verifica solo quando si esegue una query dei dati ad accesso sporadico.

**Stretch Database assicura che nessun dato vada perso** se si verifica un errore durante la migrazione. È disponibile anche una logica di ripetizione dei tentativi per gestire i problemi di connessione che possono verificarsi durante la migrazione. Una vista a gestione dinamica fornisce lo stato della migrazione.

**È possibile sospendere la migrazione dei dati** per risolvere i problemi nel server locale o per ottimizzare la larghezza di banda di rete disponibile.  
  
 ![Panoramica di Stretch Database](../../sql-server/stretch-database/media/stretch-overview.png "Panoramica di Stretch Database")  
  
## <a name="is-stretch-database-for-you"></a>Esigenze soddisfatte da Stretch Database  
 Se è possibile fare le seguenti affermazioni, Stretch Database può soddisfare i requisiti e risolvere i problemi specifici.  
  
|Responsabili delle decisioni|DBA|  
|--------------------------------|---------------------|  
|È necessario mantenere i dati transazionali per molto tempo.|Le dimensioni delle tabelle sono fuori controllo.|  
|Alcune volte è necessario eseguire query dei dati ad accesso sporadico.|Gli utenti sostengono che desiderano accedere ai dati ad accesso sporadico, che usano solo raramente.|  
|Si dispone di applicazioni, incluse applicazioni meno recenti, che non si intende aggiornare.|È necessario continuare ad aggiungere spazio di archiviazione.|  
|Si desidera trovare un modo per risparmiare denaro sull'archiviazione.|Non è possibile eseguire il backup o il ripristino di tabelle di dimensioni così grandi secondo quanto previsto dal contratto di servizio.|  
  
## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Tipi di database e tabelle candidati per Stretch Database  
 Stretch Database è destinato a database transazionali con grandi quantità di dati cronologici, in genere archiviati in un numero limitato di tabelle. Queste tabelle possono contenere più di un miliardo di righe.  
  
 Se si usa la funzionalità della tabella temporale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], usare Stretch Database per migrare tutta o una parte della tabella di cronologia associata per un'archiviazione conveniente in Azure. Per altre informazioni, vedere [Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).  
  
 Per identificare i database e le tabelle candidati per Stretch Database, usare l'ottimizzazione guidata Stretch Database di SQL Server 2016 Upgrade Advisor. Per altre informazioni, vedere [Identificare i database e le tabelle per Estensione database eseguendo Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md). Per altre informazioni sui possibili problemi di blocco, vedere [Limitazioni di Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  

## <a name="test-drive-stretch-database"></a>Test Drive di Stretch Database  
 **Test drive di Stretch Database con il database di esempio AdventureWorks.** Per ottenere il database di esempio AdventureWorks, è necessario scaricare almeno il file di database e il file di script ed esempi da [qui](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks). Dopo aver ripristinato il database di esempio in un'istanza di SQL Server 2016, decomprimere il file di esempi e aprire il file degli esempi di Estensione database dalla cartella Stretch DB. Eseguire gli script in questo file per controllare lo spazio usato dai dati prima e dopo aver abilitato Stretch Database per tenere traccia dell'avanzamento della migrazione dei dati e per verificare se è possibile continuare a eseguire query sui dati esistenti e inserire nuovi dati durante e dopo la migrazione dei dati.  
  
## <a name="next-step"></a>Passaggio successivo  
 **Identificare database e tabelle candidati per Stretch Database.** Scaricare Data Migration Assistant ed eseguire una valutazione per identificare i database e le tabelle candidati per lo stretch database. Per altre informazioni, vedere [Identificare i database e le tabelle per Estensione database eseguendo Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
  
