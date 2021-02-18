---
title: Panoramica della configurazione di cluster di SQL Server Big Data
titleSuffix: SQL Server big data clusters
description: Panoramica della configurazione di cluster di Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343986"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>Configurare un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
La gestione della configurazione consente agli amministratori di garantire che il cluster di Big data sia sempre pronto per le proprie esigenze di carico di lavoro. Con questa funzionalità, gli amministratori del cluster possono modificare o ottimizzare varie parti del cluster di Big data in fase di distribuzione o post-distribuzione e ottenere informazioni più approfondite sulle configurazioni in esecuzione nel Catalogo dati business. 

Gestione configurazione consente a un amministratore di abilitare SQL Agent, definire le risorse di base per i processi Spark dell'organizzazione o vedere quali impostazioni sono configurabili in ogni ambito. In fase di distribuzione, è possibile configurare le configurazioni tramite il `bdc.json` file di distribuzione e dopo la distribuzione tramite l'interfaccia della riga di comando di azdata.

## <a name="configuration-scopes"></a>Ambiti di configurazione
La configurazione di cluster di Big data prevede tre livelli di ambito: `cluster` , `service` e `resource` . Anche la gerarchia delle impostazioni segue in questo ordine, dal più alto al più basso. I componenti BDC accetteranno il valore dell'impostazione definito nell'ambito più basso. Se l'impostazione non è definita in un ambito specificato, erediterà il valore dall'ambito padre superiore.

Ad esempio, è possibile definire il numero predefinito di core che il driver Spark userà nel pool di archiviazione e nelle `Sparkhead` risorse. Per definire il numero predefinito di core, è possibile eseguire una delle azioni seguenti:

- Specificare un valore di core predefinito nell'ambito del servizio `Spark`

- Specificare un valore di core predefinito nell'ambito della risorsa `storage-0` e `sparkhead`

Nel primo scenario tutte le risorse con ambito inferiore del servizio Spark (pool di archiviazione e `Sparkhead` ) *erediteranno* il numero predefinito di core dal valore predefinito del servizio Spark.

Nel secondo scenario ogni risorsa userà il valore definito nel rispettivo ambito.

Se il numero predefinito di core viene configurato sia nell'ambito del servizio sia in quello delle risorse, il valore con ambito risorsa eseguirà l'override del valore con ambito servizio, poiché si tratta dell'ambito **configurato dell'utente** più basso per l'impostazione specificata.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni specifiche sulla configurazione, vedere gli articoli appropriati:

- [Personalizzare la distribuzione di cluster di Big Data](deployment-custom-configuration.md)
- [Configurare i cluster di Big data dopo la distribuzione](configure-bdc-postdeployment.md)
- [Configurare cluster di Big Data-versione CU8 e versioni precedenti](configure-bdc-pre-configuration.md)

Riferimento: 
- [Proprietà di configurazione di SQL Server Big Data cluster](reference-config-bdc-overview.md)
- [Proprietà di configurazione di Apache Spark e Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Proprietà di configurazione dell'istanza di SQL Server Master-versione pre-CU9](reference-config-master-instance.md)
- [INTERFACCIA della riga di comando azdata](../azdata/reference/reference-azdata.md)