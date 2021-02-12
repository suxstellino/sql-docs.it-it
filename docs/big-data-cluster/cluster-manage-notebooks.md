---
title: Gestire i cluster Big Data con notebook Jupyter e Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Gestione dei cluster Big Data con notebook Jupyter e Azure Data Studio nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 34592d9a6cb3db8bde5a2a25098314fd9ace1208
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039481"
---
# <a name="manage-big-data-clusters-bdc-the-cluster-with-notebooks"></a>Gestire i cluster Big Data con i notebook

Questa pagina è un indice dei notebook per i cluster Big Data di SQL Server. I notebook eseguibili (con estensione ipynb) sono progettati per facilitare la gestione dei cluster Big Data in SQL Server 2019.

Ogni notebook è progettato per cercare le proprie dipendenze. Un comando **Run all cells** (Esegui tutte le celle) viene eseguito correttamente oppure genera un'eccezione con un hint con collegamento ipertestuale a un altro notebook per risolvere la dipendenza mancante. Seguire il collegamento ipertestuale dell'hint al notebook successivo, scegliere **Run all cells** (Esegui tutte le celle) e, al termine, tornare al notebook originale e scegliere di nuovo **Run all cells** (Esegui tutte le celle).

Se vengono installate tutte le dipendenze, ma il comando **Run all cells** (Esegui tutte le celle) non viene eseguito, ogni notebook analizzerà i risultati e, dove possibile, produrrà un hint con collegamento ipertestuale a un altro notebook per facilitare la risoluzione del problema.


## <a name="installing-and-uninstalling-utilities-on-big-data-cluster-bdc"></a>Installazione e disinstallazione delle utilità nel cluster Big Data

Questa sezione contiene un set di notebook utili per l'installazione e la disinstallazione degli strumenti da riga di comando e dei pacchetti necessari per gestire i cluster Big Data di SQL Server.

|Nome |Descrizione |
|---|---|---|---|
|SOP010 - Aggiornare un cluster Big Data|Usare questo notebook per aggiornare un cluster Big Data con azdata. |
|SOP012 - Installare unixodbc per Mac|Usare questo notebook quando si verificano errori durante l'uso di Brew per installare ODBC per SQL Server.|
|SOP036 - Installare l'interfaccia della riga di comando di kubectl|Usare questo notebook per installare l'interfaccia della riga di comando di kubectl indipendentemente dal sistema operativo.|
|SOP037 - Disinstallare l'interfaccia della riga di comando di kubectl|Usare questo notebook per disinstallare l'interfaccia della riga di comando di kubectl indipendentemente dal sistema operativo.|
|SOP038 - Installare l'interfaccia della riga di comando di Azure|Usare questo notebook per installare l'interfaccia della riga di comando di Azure indipendentemente dal sistema operativo.|
|SOP039 - Disinstallare l'interfaccia della riga di comando di Azure|Usare questo notebook per disinstallare l'interfaccia della riga di comando di Azure indipendentemente dal sistema operativo.|
|SOP040 - Aggiornare pip nella sandbox Python ADS|Usare questo notebook per aggiornare pip nella sandbox Python ADS.|
|SOP054 - Installare l'interfaccia della riga di comando di azdata|Usare questo notebook per installare l'interfaccia della riga di comando di azdata indipendentemente dal sistema operativo.|
|SOP055 - Disinstallare l'interfaccia della riga di comando di azdata|Usare questo notebook per disinstallare l'interfaccia della riga di comando di azdata indipendentemente dal sistema operativo.|
|SOP059 - Installare il modulo Python Kubernetes|Usare questo notebook per installare i moduli Kubernetes con Python.|
|SOP060 - Disinstallare il modulo kubernetes|Usare questo notebook per disinstallare i moduli Kubernetes con Python.|
|SOP061 - Eliminare un cluster Big Data|Usare questo notebook per eliminare un cluster Big Data.|
|SOP062 - Installare i moduli ipython-sql e pyodbc|Usare questo notebook per installare i moduli ipython-sql e pyodbc.|
|SOP063 - Installare l'interfaccia della riga di comando di azdata (tramite la gestione pacchetti)|Usare questo notebook per installare l'interfaccia della riga di comando di azdata (tramite la gestione pacchetti).|
|SOP064 - Disinstallare l'interfaccia della riga di comando di azdata (tramite la gestione pacchetti)|Usare questo notebook per disinstallare l'interfaccia della riga di comando di azdata (tramite la gestione pacchetti).|
|SOP069 - Installare ODBC per SQL Server|Usare questo notebook per installare il driver ODBC poiché alcuni sottocomandi in azdata richiedono il driver ODBC di SQL Server.|


## <a name="managing-certificates-on-big-data-clusters-bdc"></a>Gestione dei certificati nel cluster Big Data

Un set di notebook per eseguire un notebook per la gestione dei certificati nei cluster di Big Data.

|Nome |Descrizione |
|---|---|---|---|
|CER001 - Generare un certificato CA radice|Generare un certificato CA radice. Può essere utile usare un certificato CA radice per tutti i cluster non di produzione in ogni ambiente, in quanto questa tecnica riduce il numero di certificati CA radice che devono essere caricati nei client che si connettono a questi cluster. |
|CER002 - Scaricare il certificato CA radice esistente|Usare questo notebook per scaricare un certificato CA radice generato da un cluster.|
|CER003 - Caricare il certificato CA radice esistente|CER003 - Caricare il certificato CA radice esistente.|
|CER004 - Scaricare e caricare il certificato CA radice esistente|Scaricare e caricare il certificato CA radice esistente. |
|CER010 - Installare il certificato CA radice generato|Questo notebook copia localmente (da un cluster Big Data) il certificato CA radice generato che è stato installato con **CER001 - Generare un certificato CA radice** o **CER003 - Caricare il certificato CA radice esistente** e quindi installa il certificato CA radice nell'archivio certificati locale del computer.|
|CER020 - Creare un certificato per il proxy di gestione|Questo notebook crea un certificato per l'endpoint del proxy di gestione.|
|CER021 - Creare un certificato Knox|Questo notebook crea un certificato per l'endpoint del gateway Knox.|
|CER022 - Creare un certificato App Proxy|Questo notebook crea un certificato per l'endpoint del proxy di distribuzione app.|
|CER023 - Creare un certificato master|Questo notebook crea un certificato per l'endpoint master.|
|CER030 - Firmare il certificato del proxy di gestione con la CA generata|Questo notebook firma il certificato creato usando **CER020 - Creare un certificato per il proxy di gestione** con il certificato CA radice generato, creato usando **CER001 - Generare un certificato CA radice** o **CER003 - Caricare un certificato CA radice esistente**|
|CER031 - Firmare il certificato Knox con la CA generata|Questo notebook firma il certificato creato usando **CER021 - Creare un certificato Knox** con il certificato CA radice generato, creato usando **CER001 - Generare un certificato CA radice** o **CER003 - Caricare un certificato CA radice esistente**|
|CER032 - Firmare il certificato App-Proxy con la CA generata|Questo notebook firma il certificato creato usando **CER022 - Creare un certificato App Proxy** con il certificato CA radice generato, creato usando **CER001 - Generare un certificato CA radice** o **CER003 - Caricare un certificato CA radice esistente**.|
|CER033 - Firmare il certificato master con la CA generata|Questo notebook firma il certificato creato usando **CER023 - Creare un certificato master** con il certificato CA radice generato, creato usando **CER001 - Generare un certificato CA radice** o **CER003 - Caricare un certificato CA radice esistente**.|
|CER040 - Installare un certificato del proxy di gestione firmato|Questo notebook viene installato nel cluster Big Data il certificato firmato usando **CER030 - Firmare il certificato del proxy di gestione con la CA generata**.|
|CER041 - Installare il certificato Knox firmato|Questo notebook installa nel cluster Big Data il certificato firmato usando **CER031 - Firmare il certificato Knox con la CA generata**.|
|CER042 - Installare il certificato App-Proxy firmato|Questo notebook installa nel cluster Big Data il certificato firmato usando **CER032 - Firmare il certificato App-Proxy con la CA generata**.|
|CER043 - Installare il certificato Controller firmato|Questo notebook installa nel cluster Big Data il certificato firmato usando **CER034 - Firmare il certificato Controller con la CA radice del cluster**. Si noti che alla fine del notebook il pod controller e tutti i pod che usano PolyBase (i pod del pool master e del pool di calcolo) verranno riavviati per caricare i nuovi certificati.|
|CER050 - Attendere che il cluster Big Data torni integro|Il notebook resterà in attesa fino a quando il cluster Big Data non tornerà a uno stato integro, dopo che il pod Controller e i pod che usano PolyBase sono stati riavviati per caricare i nuovi certificati.|
|CER100 - Configurare il cluster con certificati autofirmati|Questo notebook genera una nuova autorità di certificazione radice nel cluster Big Data e crea nuovi certificati per ogni endpoint (gli endpoint sono: Gestione, Gateway, App-Proxy e Controller). Firmare ogni nuovo certificato con la nuova CA radice generata, ad eccezione del certificato Controller, firmato con la CA radice del cluster esistente, quindi installare ogni certificato nel cluster Big Data. Scaricare la nuova CA radice generata nell'archivio certificati Autorità di certificazione radice disponibile nell'elenco locale del computer. Tutti i certificati autofirmati generati verranno archiviati nel pod Controller in test_cert_store_root.|
|CER101 - Configurare il cluster con certificati autofirmati usando la CA radice esistente|Questo notebook userà una CA radice generata esistente nel cluster Big Data (caricata con **CER003**) e creerà nuovi certificati per ogni endpoint (Gestione, Gateway, App-Proxy e Controller), quindi firmerà ogni nuovo certificato con la nuova CA radice generata, ad eccezione del certificato Controller (firmato con la CA radice del cluster esistente), e installerà ogni certificato nel cluster Big Data. Tutti i certificati autofirmati generati verranno archiviati nel pod Controller in test_cert_store_root. Al termine di questo notebook, tutti gli accessi https:// al cluster Big Data da questo computer (e tutti i computer che installano la nuova CA radice) verranno visualizzati come protetti. Il capitolo dello strumento di esecuzione del notebook garantisce che i processi cron creati (OPR003) per eseguire App-Deploy installeranno la CA radice del cluster per consentire di ottenere in modo sicuro i token JWT e il file swagger.json.|

## <a name="backup-and-restore-from-big-data-cluster-bdc"></a>Eseguire il backup e il ripristino dal cluster Big Data

Questa sezione contiene un set di notebook utili per eseguire il backup e il ripristino delle operazioni per i cluster Big Data di SQL Server.

| Nome | Descrizione |
|--|--|
| SOP008 - Eseguire il backup dei file HDFS in Azure Data Lake Store Gen2 con distcp | Questa procedura operativa standard (SOP, Standard Operating Procedure) consente di eseguire il backup dei dati dal file system HDFS del cluster Big Data di origine di SQL Server 2019 nell'account Azure Data Lake Store Gen2 specificato. Assicurarsi che l'account Azure Data Lake Store Gen2 sia configurato con lo "spazio dei nomi gerarchico" abilitato. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
