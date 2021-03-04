---
title: Panoramica dell'anteprima del percorso sinapsi di Azure
description: Il percorso delle sinapsi di Azure è uno strumento per eseguire la migrazione di una data warehouse ad Azure sinapsi Analytics.
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: Azure Synapse Pathway
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: d7289d2bfe099dad7bbc91ccd5060797f7aad997
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839230"
---
# <a name="azure-synapse-pathway-preview"></a>Anteprima del percorso della sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Poiché i clienti considerano la modernizzazione dei sistemi data warehouse, uno dei blocchi critici che affrontano è la traduzione del codice SQL. Il codice esistente viene scritto e ottimizzato per il sistema corrente, ma deve essere ottimizzato per il nuovo sistema a cui viene eseguita la migrazione.

Le organizzazioni in tutto il mondo vogliono modernizzare la propria piattaforma di analisi per sfruttare i vantaggi di costo totale di proprietà (TCO) e innovazione. Tuttavia, i clienti hanno investito migliaia di ore lavorative, milioni di dollari e decine di migliaia di righe di codice per la loro data warehouse.
 
Per tradurre questo codice SQL critico, i clienti devono riscrivere manualmente il codice SQL esistente o investire grandi quantità di budget per una procedura esterna per riscrivere o convertire il codice.

> [!IMPORTANT]
> Il percorso delle sinapsi di Azure è attualmente disponibile in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate.
 
> Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Il **percorso delle sinapsi di Azure** consente di eseguire l'aggiornamento a una piattaforma di data warehouse moderna automatizzando la conversione del codice del data warehouse esistente. Si tratta di uno strumento gratuito, intuitivo e facile da usare che consente di automatizzare la conversione del codice e di velocizzare la migrazione ad Azure sinapsi Analytics.

 ![Panoramica del percorso della sinapsi di Azure.](./media/pathway-overview/synapse-pathway-overview.png) 

Il percorso sinapsi converte le istruzioni DDL (Data Definition Language) e DML (Data Manipulation Language) in un linguaggio conforme a T-SQL compatibile con SQL sinapsi di Azure.

## <a name="behind-the-scenes"></a>Dietro le quinte

Per altre informazioni sul percorso delle sinapsi di Azure, vedere [dietro le quinte](synapse-pathway-behind-the-scenes.md) per un processo dettagliato sul funzionamento del percorso sinapsi di Azure.

## <a name="get-azure-synapse-pathway"></a>Ottenere il percorso della sinapsi di Azure

Per installare il percorso sinapsi, controllare il [download del percorso sinapsi di Azure](synapse-pathway-download.md) per i prerequisiti e il collegamento per scaricare la versione più recente.

## <a name="supported-sources"></a>Origini supportate

Il percorso delle sinapsi di Azure supporta la conversione del codice di database, schemi e tabelle per le origini seguenti:
- **IBM Netezza** 
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>Domande frequenti

Per altre informazioni sul percorso delle sinapsi di Azure, vedere la [pagina delle domande frequenti](pathway-faq.md)

## <a name="next-steps"></a>Passaggi successivi

- [Eseguire la prima traduzione con un percorso di sinapsi di Azure](synapse-pathway-assessment.md)
- Blog di annuncio-annuncio [del percorso delle sinapsi di Azure: sovralimentare il data warehouse migrazione-Microsoft Tech Community](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)


