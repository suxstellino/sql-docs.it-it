---
title: Domande frequenti sull'anteprima del percorso sinapsi di Azure
description: Domande frequenti sul percorso delle sinapsi di Azure
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 8352fb6a70c54ede61d544a147f970237404c9f5
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186328"
---
# <a name="azure-synapse-pathway-preview-faq"></a>Domande frequenti sull'anteprima del percorso sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

In questa guida sono disponibili le domande più frequenti sull'anteprima dei percorsi delle sinapsi di Azure.

## <a name="general"></a>Generale

### <a name="q-what-is-azure-synapse-pathway"></a>Q. Che cos'è il percorso sinapsi di Azure?

R. Il percorso della sinapsi di Azure è uno strumento di conversione del codice che consente di eseguire l'aggiornamento a una piattaforma di data warehouse moderna automatizzando la conversione del codice del data warehouse esistente per renderlo conforme con SQL sinapsi di Azure basato su T-SQL.

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>Q. Come è possibile scaricare il percorso sinapsi di Azure?

R. Il percorso sinapsi è disponibile per il download dall' [area download Microsoft](https://aka.ms/synapse-pathway-download).

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>Q. Quanto costa il percorso della sinapsi di Azure?

R. Non vi sono costi associati al download e all'esecuzione delle traduzioni del codice con il percorso sinapsi.

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>Q. Quali coppie di origine/destinazione è attualmente supportata da Azure sinapsi?

R. In questa versione di anteprima del percorso sinapsi, i data warehouse seguenti sono inclusi come origini:
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>Q. Cosa è incluso nell'ambito della conversione del codice?

R. Il percorso sinapsi supporta la conversione del codice di tabelle, schemi, viste e stored procedure.

### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>Q. È anche possibile analizzare l'ambiente e fornire un report di valutazione di tutti gli oggetti che devono essere convertiti/tradotti?

R. In questa versione di anteprima del percorso sinapsi sarà necessario fornire il collegamento agli script DDL/DML che devono essere tradotti. Il percorso sinapsi non analizza l'ambiente corrente per identificare gli oggetti che devono essere tradotti.

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>Q. Quali informazioni acquisisce il percorso della sinapsi di Azure sull'istanza di data warehouse corrente?

R. Poiché è possibile eseguire il percorso sinapsi nell'ambiente locale, non vengono acquisiti dati, ad eccezione del nome e dell'organizzazione, che è necessario quando si Scarica il file MSI dall'area download Microsoft.

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>Q. Dove è possibile generare I problemi riscontrati nel percorso delle sinapsi di Azure?

R. Il percorso delle sinapsi di Azure è attualmente in fase di **Anteprima**.   Il supporto per il percorso sinapsi è disponibile tramite il canale di supporto Microsoft. È possibile aumentare il ticket nei portali portale di Azure o standard (in genere il supporto locale).


> [!NOTE] 
> Come per qualsiasi altro servizio di Azure, tutti i servizi in anteprima sono idonei per il supporto, solo senza contratti di servizio.

<!-- ### Troubleshooting and optimization

#### Q. Why do I see slow performance while running the code conversion?

#### Q. Translation of errors or unexpected results? -->

## <a name="next-steps"></a>Passaggi successivi

[Scarica il percorso delle sinapsi di Azure](synapse-pathway-download.md)