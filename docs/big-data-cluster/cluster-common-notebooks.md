---
title: Scenario comune di utilizzo di cluster Big Data con i notebook Jupyter e Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Scenario comune di utilizzo di cluster Big Data con i notebook Jupyter e Azure Data Studio in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cbf034873b690262ff6acb395ed8eca5d09ae79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039511"
---
# <a name="common-notebooks-for-sql-server-big-data-clusters"></a>Notebook comuni per i cluster Big Data di SQL Server

Questo articolo elenca i notebook per i cluster Big Data di SQL Server. I notebook eseguibili (con estensione ipynb) sono progettati per facilitare la visualizzazione degli scenari comuni dei cluster Big Data in SQL Server 2019.

Ogni notebook è progettato per cercare le proprie dipendenze. Un'opzione **Run all cells** (Esegui tutte le celle) viene eseguita correttamente oppure genera un'eccezione con un *hint* con collegamento ipertestuale a un altro notebook per risolvere la dipendenza mancante. Seguire il collegamento ipertestuale dell'hint al notebook successivo, scegliere **Run all cells** (Esegui tutte le celle) e, al termine, tornare al notebook originale e scegliere di nuovo **Run all cells** (Esegui tutte le celle).

Se vengono installate tutte le dipendenze, ma il comando **Run all cells** (Esegui tutte le celle) non viene eseguito, ogni notebook analizzerà i risultati e, dove possibile, produrrà un hint con collegamento ipertestuale a un altro notebook per facilitare la risoluzione del problema.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Raccolta di log da cluster Big Data

I notebook di questa sezione vengono usati come prerequisiti per altri notebook, ad esempio per l'accesso e la disconnessione di un cluster.

|Nome |Descrizione |
|---|---|
|SOP005 - Accesso con az|Usare l'interfaccia della riga di comando az per l'accesso ad Azure. |
|SOP006 - Disconnessione con az|Usare l'interfaccia della riga di comando az per la disconnessione di Azure.|
|SOP007 - Informazioni sulla versione (azdata, bdc, kubernetes)|Definire le funzioni di supporto usate nel notebook per informazioni sul controllo delle versioni.|
|SOP011 - Impostare il contesto di configurazione di kubernetes|Impostare la configurazione di kubernetes da usare. |
|SOP028 - Accesso azdata|Usare l'interfaccia della riga di comando azdata per accedere a un cluster Big Data. |
|SOP033 - Disconnessione azdata|Usare l'interfaccia della riga di comando azdata per la disconnessione di un cluster Big Data. |
|SOP034 - Attendere che il cluster Big Data torni integro|Blocca fino a quando il cluster Big Data non è integro o il timeout specificato scade. Il parametro min_pod_count indica che il controllo integrità non verrà superato fino a quando non esiste il numero minimo di pod nel cluster. Se i pod esistenti oltre questo limite non sono integri, il cluster non è integro.|
|OPR001 - Creare app-deploy|Usare questo notebook per creare un'app in un cluster Big Data. |
|OPR002 - Eseguire app-deploy|Usare questo notebook per eseguire un'app in un cluster Big Data. |
|OPR003 - Creare un processo cron|Usare questo notebook per creare un processo cron in un cluster Big Data. |
|OPR004 - Sospendere il processo cron|Usare questo notebook per sospendere un processo cron in un cluster Big Data. |
|OPR005 - Riprendere il processo cron|Usare questo notebook per riprendere un processo cron in un cluster Big Data. |
|OPR006 - Eliminare un processo cron|Usare questo notebook per eliminare un processo cron in un cluster Big Data. |
|OPR007 - Eliminare app-deploy|Usare questo notebook per eliminare un'app in un cluster Big Data. |
|OPR100 - Distribuire e pianificare i notebook|Usare questo notebook per distribuire un elenco di notebook in un pod App-Deploy, eseguire App-Deploy in una pianificazione usando un processo cron Kubernetes e installare un dashboard Grafana per visualizzare i risultati.|
|OPR600 - Monitorare l'infrastruttura (Kubernetes)|Usare questo notebook per monitorare l'infrastruttura.|
|OPR700 - Creare il dashboard Grafana per le applicazioni App-Deploy|Usare questo notebook per creare un dashboard in Grafana per monitorare i risultati di App-Deploy e generare avvisi in caso di errore Canary o Application.|
|OPR900 - Risolvere i problemi di esecuzione di app-deploy|Usare questo notebook per eseguire lo script di app-deploy direttamente nel contenitore (usando l'esecuzione kubectl). In questo modo è possibile ottenere più informazioni di debug per la risoluzione dei problemi, soprattutto in relazione ai problemi della fase di avvio dello script.|
|OPR901 - Risolvere i problemi del processo cron|Usare questo notebook per eseguire lo script del processo cron direttamente nel contenitore (usando l'esecuzione kubectl). In questo modo è possibile ottenere più informazioni di debug per la risoluzione dei problemi.|


## <a name="analyze-logs-from-big-data-clusters-bdc"></a>Analizzare i log dei cluster Big Data

Set di notebook di esempio che illustrano gli scenari di cluster Big Data di SQL Server.

|Nome |Descrizione |
|---|---|
|SAM001a - Pool di archiviazione query del pool master di SQL Server (1 di 3) - Caricare dati di esempio|In questa esercitazione in tre parti caricare i dati nel pool di archiviazione (HDFS) usando azdata, convertirli in Parquet (usando Spark) e nella terza parte eseguire una query sui dati usando il pool master (SQL Server). |
|SAM001b - Pool di archiviazione query del pool master di SQL Server (2 di 3) - Convertire i dati in Parquet|In questa seconda parte di un'esercitazione in tre parti usare Spark per convertire un file con estensione csv in un file Parquet.|
|SAM001c - Pool di archiviazione query del pool master di SQL Server (3 di 3) - Eseguire query in HDFS da SQL Server|In questa terza parte dell'esercitazione relativa al pool di archiviazione verrà illustrato come creare una tabella esterna che punta ai dati di HDFS in un cluster Big Data e unire questi dati con dati di valore elevato nell'istanza master.|
|SAM002 - Pool di archiviazione (2 di 2) - Eseguire query in HDFS|In questa seconda parte dell'esercitazione relativa al pool di archiviazione verrà illustrato come creare una tabella esterna che punta ai dati di HDFS in un cluster Big Data e unire questi dati con dati di valore elevato nell'istanza master|
|SAM003 - Esempio di pool di dati|In questa esercitazione si apprenderà come creare un'origine del pool di dati e una tabella esterna nel pool di dati e quindi inserire i dati nelle tabelle del pool di dati e caricare i dati da una tabella del pool di dati in un'altra. Unire i dati nella tabella del pool di dati con altre tabelle del pool di dati, eseguendo anche il troncamento delle tabelle e la pulizia. |
|SAM004 - Virtualizzare i dati da MongoDB|Per eseguire query sui dati da un'origine dati esterna MongoDB, è necessario creare tabelle esterne per fare riferimento a tali dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.|
|SAM005 - Virtualizzare i dati da Oracle|Per eseguire query sui dati da un'origine dati esterna Oracle, è necessario creare tabelle esterne per fare riferimento a tali dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.|
|SAM006 - Virtualizzare i dati da SQL Server|Per eseguire query virtuali sui dati da un'altra origine dati SQL Server, è necessario creare tabelle esterne per fare riferimento a tali dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.|
|SAM007 - Virtualizzare i dati da Teradata|Per eseguire query sui dati da un'origine dati esterna Teradata, è necessario creare tabelle esterne per fare riferimento a tali dati esterni. In questa sezione è disponibile codice di esempio per creare queste tabelle esterne.|
|SAM008 - Spark con azdata|Comandi di azdata e kubectl da usare nelle sessioni Spark.|
|SAM009 - HDFS con azdata|Comandi di azdata e kubectl da usare in HDFS.|
|SAM010 - App con azdata|Comandi di azdata e kubectl da usare in App Deploy. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
