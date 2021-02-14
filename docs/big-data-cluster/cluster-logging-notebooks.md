---
title: Raccolta e analisi dei log con i notebook Jupyter e Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Registrazione del cluster con i notebook Jupyter e Azure Data Studio in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8609fbe12e6218d328528dd5577bf1a40167162
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047781"
---
# <a name="gathering-and-analyzing-logs-in-the-cluster-with-notebooks"></a>Raccolta e analisi dei log nel cluster con i notebook

Questa pagina è un indice dei notebook per i cluster Big Data di SQL Server. I notebook eseguibili (con estensione ipynb) sono progettati per facilitare la registrazione dei cluster Big Data in SQL Server 2019.

Ogni notebook è progettato per cercare le proprie dipendenze. Un comando **Run all cells** (Esegui tutte le celle) viene eseguito correttamente oppure genera un'eccezione con un hint con collegamento ipertestuale a un altro notebook per risolvere la dipendenza mancante. Seguire il collegamento ipertestuale dell'hint al notebook successivo, scegliere **Run all cells** (Esegui tutte le celle) e, al termine, tornare al notebook originale e scegliere di nuovo **Run all cells** (Esegui tutte le celle).

Se vengono installate tutte le dipendenze, ma il comando **Run all cells** (Esegui tutte le celle) non viene eseguito, ogni notebook analizzerà i risultati e, dove possibile, produrrà un hint con collegamento ipertestuale a un altro notebook per facilitare la risoluzione del problema.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Raccolta di log da cluster Big Data

Questa sezione contiene un set di notebook utili per ottenere i log da un cluster Big Data di SQL Server.

| Nome | Descrizione |
|--|--|
| TSG001 - Eseguire azdata copy-logs | Usare l'interfaccia della riga di comando di azdata per copiare i dati nei cluster Big Data. |
| TSG061 - Ottenere la parte finale di tutti i log dei contenitori per i pod nello spazio dei nomi BDC | Ottenere tutti i log dei contenitori per i pod dal cluster Big Data nello spazio dei nomi. |
| TSG062 - Ottenere la parte finale di tutti i log dei contenitori precedenti per i pod nello spazio dei nomi BDC | Ottenere tutti i log dei contenitori precedenti per i pod dal cluster Big Data nello spazio dei nomi. |
| TSG083 - Eseguire il dump cluster-info kubectl | Usare l'interfaccia della riga di comando di kubectl per eseguire il dump delle informazioni relative al cluster Big Data. |
| TSG084 - Errore interno di Query Processor | Uso della query DMV per ottenere altre informazioni sull'errore interno di Query Processor. |
| TSG091 - Ottenere i log dell'interfaccia della riga di comando azdata | Ottenere i log di azdata dal computer locale. |



## <a name="analyse-logs-from-big-data-clusters-bdc"></a>Analizzare i log da cluster Big Data

Set di notebook per raccogliere e analizzare i log da un cluster Big Data di SQL Server.  Il processo di analisi suggerirà i notebook successivi da eseguire per i problemi noti rilevati nei log.

|Nome|Descrizione |
|---|---|
|TSG030 - File errorlog SQL Server|Ottenere i file errorlog di SQL Server, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi. |
|TSG031 - Log PolyBase SQL Server|Ottenere i log PolyBase di SQL Server, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG034 - Log Livy|Ottenere i log Livy di SQL Server, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG035 - Log cronologia Spark|Ottenere i log della cronologia Spark di SQL Server, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG036 - Log controller|Ottenere le ultime 'n' ore dei log del controller, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG046 - Log gateway Knox|Knox restituisce un errore 500 al client e rimuove i dettagli (lo stack) che puntano alla causa del problema sottostante. Usare quindi questo notebook per ottenere i log Knox dal cluster. Ottenere i log del gateway Knox, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG073 - Log InfluxDB|Ottenere i log InfluxDB, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG076 - Log ricerca elastica|Ottenere i log della ricerca elastica, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG077 - Log Kibana|Ottenere i log Kibana, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG088 - Log datanode Hadoop|Ottenere i log datanode Hadoop, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG090 - Log nodemanager Yarn|Ottenere i log nodemanager Yarn, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG092 - Parte finale del log del supervisore per tutti i contenitori in BDC|Ottenere la parte finale del log Supervisord per tutti i contenitori nel cluster Big Data, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG093 - Parte finale del log dell'agente per tutti i contenitori in BDC|Ottenere la parte finale del log dell'agente per tutti i contenitori nel cluster Big Data, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG094 - Log Grafana|Ottenere i log Grafana, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG095 - Log namenode Hadoop|Ottenere i log namenode Hadoop, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|
|TSG096 - Log Zookeeper|Ottenere i log Zookeeper, analizzare le voci dei log e suggerire altre guide alla risoluzione dei problemi.|

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
