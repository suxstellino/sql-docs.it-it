---
title: Estensione Istanza gestita di SQL di Azure
description: Usare Azure Data Studio con Istanza gestita di SQL di Azure
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: alanyu, maghan, sstein
ms.custom: ''
ms.date: 10/07/2019
ms.openlocfilehash: 0c450da20d1e63e654201ccfcaf650dbebb57b4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052109"
---
# <a name="azure-sql-managed-instance-dashboard-for-azure-data-studio-preview"></a>Dashboard di Istanza gestita di SQL di Azure per Azure Data Studio (anteprima)

L'estensione Istanza gestita di SQL di Azure fornisce un dashboard per l'uso di [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance-index) in [Azure Data Studio](https://github.com/Microsoft/azuredatastudio). L'estensione offre le funzionalità seguenti:

- Mostra le proprietà di Istanza gestita di SQL, tra cui vCore e spazio di archiviazione usato
- Monitora l'utilizzo della CPU e dello spazio di archiviazione per le due ore precedenti
- Mostra gli avvisi di configurazione e le raccomandazioni di ottimizzazione
- Mostra lo stato delle repliche di database
- Mostra i log degli errori filtrati

## <a name="install"></a>Installazione

È possibile installare la versione ufficiale di questa estensione. Seguire i passaggi descritti nella [documentazione di Azure Data Studio](./add-extensions.md).
Nel riquadro **Estensioni** cercare "Istanza gestita" e installare l'estensione. Dopo l'installazione, si riceverà una notifica automatica degli aggiornamenti futuri dell'estensione.

Con l'estensione installata, in Azure Data Studio sarà disponibile una scheda **Istanza gestita**. La scheda include informazioni specifiche sull'istanza gestita.

## <a name="properties"></a>Proprietà

L'estensione mostra le caratteristiche tecniche dell'istanza gestita e l'utilizzo di alcune risorse.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-5.png" alt-text="Proprietà di Istanza gestita":::

Il riquadro superiore mostra i dettagli seguenti:

- **Proprietà**. Informazioni di base sull'istanza gestita, inclusi Vcore, memoria e spazio di archiviazione disponibili. Sono inoltre presenti dati su livello di servizio attuale, generazione dell'hardware e caratteristiche di I/O, come velocità effettiva di scrittura del log dell'istanza o velocità effettiva di I/O file.
- **Archiviazione SSD locale**. Al livello di servizio Utilizzo generico, i file **TempDB** vengono archiviati localmente. Al livello di servizio Business Critical _tutti_ i file di database vengono inseriti nell'archiviazione SSD locale. In questa sezione è possibile visualizzare la quantità di spazio nella risorsa di archiviazione locale usato dall'istanza gestita.
- **Archiviazione Premium di Azure**. Se si ha il livello di servizio Utilizzo generico, i file di database di sistema e dell'utente vengono inseriti nell'archiviazione Premium di Azure. In questa sezione è possibile visualizzare la quantità di dati usati, il numero di file e lo spazio di archiviazione disponibile. Al livello di servizio Business Critical questa sezione è vuota.
- **Utilizzo delle risorse**. Mostra la percentuale di spazio di archiviazione e CPU usati dall'istanza gestita nelle due ore precedenti. In questo modo, è possibile aumentare le dimensioni dell'istanza, se si avvicina al limite.

## <a name="recommendations"></a>Consigli

Nel secondo riquadro nella scheda **Istanza gestita** compaiono suggerimenti e avvisi che consentono di ottimizzare l'istanza gestita.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-6.png" alt-text=" Raccomandazioni per Istanza gestita":::

Potrebbero essere visualizzate raccomandazioni simili alle seguenti:

- **Raggiungimento del limite dello spazio di archiviazione**. Eliminare i dati non necessari o aumentare le dimensioni di archiviazione dell'istanza. I database che raggiungono il limite di archiviazione potrebbero non riuscire a elaborare le query di lettura.
- **Raggiungimento del limite di velocità effettiva dell'istanza**. Indica che il caricamento avviene in prossimità del limite del livello di servizio: 22 MB/s per il livello Utilizzo generico o 48 MB/s per il livello Business Critical. Tenere presente che l'istanza gestita limiterà il caricamento per assicurare l'esecuzione dei backup.
- **Utilizzo elevato della memoria**. Una bassa permanenza presunta delle pagine o la presenza di numerose statistiche di attesa `PAGEIOLATCH` potrebbero indicare che l'istanza sta eliminando pagine dalla memoria e tentando continuamente di caricare più pagine dal disco.
- **Limiti dei file di log**. Se i file di log raggiungono i [limiti di I/O al livello di servizio Utilizzo generico](/azure/sql-database/sql-database-managed-instance-resource-limits#file-io-characteristics-in-general-purpose-tier), potrebbe essere necessario aumentare le dimensioni del file di log per ottenere prestazioni migliori.
- **Limiti dei file di dati**. Se i file di dati raggiungono i [limiti di I/O al livello di servizio Utilizzo generico](/azure/sql-database/sql-database-managed-instance-resource-limits#file-io-characteristics-in-general-purpose-tier), potrebbe essere necessario aumentare le dimensioni dei file per ottenere prestazioni migliori. Questo problema potrebbe causare un utilizzo elevato della memoria e rallentare i backup.
- **Problemi di disponibilità**. La presenza di un numero elevato di file di log virtuali può influire sulle prestazioni. Se si verifica un errore di processo, questi problemi potrebbero comportare tempi più lunghi per il recupero del database al livello di servizio Utilizzo generico.

Rivedere regolarmente queste raccomandazioni, analizzare le cause radice e intraprendere azioni correttive. L'estensione Istanza gestita di SQL offre script che è possibile eseguire per risolvere alcuni dei problemi segnalati.

## <a name="replicas"></a>Repliche

Il terzo riquadro nella scheda **Istanza gestita** mostra lo stato delle repliche di database nell'istanza gestita.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-7.png" alt-text="Repliche di Istanza gestita":::

Al livello di servizio Utilizzo generico, ogni database ha una sola replica (primaria). In un'istanza Business Critical, ogni database ha una replica primaria e tre repliche secondarie, una delle quali è usata per i carichi di lavoro di sola lettura. Nel riquadro delle **repliche** è possibile monitorare il processo di sincronizzazione e verificare che tutte le repliche secondarie siano sincronizzate con la replica primaria.

## <a name="logs"></a>Log

Il quarto riquadro di **Istanza gestita** mostra le voci del log degli errori di SQL più recenti e rilevanti.

:::image type="content" source="media/azure-sql-managed-instance-extension/azure-sql-managed-instance-extension-tab-8.png" alt-text="Voci del log di Istanza gestita":::

Anche se l'istanza gestita genera un numero elevato di voci di log, la maggior parte di esse sono informazioni interne o di sistema. Inoltre, alcune voci di log mostrano i nomi di database fisici (valori `GUID`) anziché i nomi di database logici effettivi.

L'estensione Istanza gestita di SQL esclude le voci di log non necessarie in base al [metodo Dimitri Furman](https://techcommunity.microsoft.com/t5/DataCAT/Azure-SQL-DB-Managed-Instance-sp-readmierrorlog/ba-p/305506). Inoltre, l'estensione visualizza i nomi dei file logici effettivi anziché i nomi fisici.

## <a name="reporting-problems"></a>Segnalazione di problemi

Se si verificano problemi con l'estensione Istanza gestita di SQL, segnalare il problema nel [progetto GitHub dell'estensione](https://github.com/JocaPC/AzureDataStudio-Managed-Instance/issues).

## <a name="code-of-conduct"></a>Codice di comportamento

Questo progetto ha adottato il [Codice di comportamento di Microsoft per l'open source](https://opensource.microsoft.com/codeofconduct/).

Per altre informazioni, vedere le [Domande frequenti sul codice di comportamento](https://opensource.microsoft.com/codeofconduct/faq/) o scrivere a [opencode@microsoft.com](mailto:opencode@microsoft.com) per domande aggiuntive o commenti.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, visitare il [progetto GitHub](https://github.com/JocaPC/AzureDataStudio-Managed-Instance/).
