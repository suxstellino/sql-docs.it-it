---
title: Guida alla migrazione da SAP ASE a SQL Server
description: 'Questa guida illustra come eseguire la migrazione dei database di SAP ASE a Microsoft SQL Server usando la migrazione SQL Server per SAP ASE (SSMA per SAP ASE). '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: fb297b72e1eeb3d614a00a9ef574f0097f9d0f75
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603796"
---
# <a name="migration-guide-sap-ase-to-sql-server"></a>Guida alla migrazione: SAP ASE per SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

Questa guida illustra come eseguire la migrazione dei database SAP ASE a SQL Server usando SQL Server Migration Assistant per SAP ASE.

Per altri scenari, vedere la [Guida alla migrazione del database](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Prerequisiti 

Per eseguire la migrazione del database SAP SE a SQL Server, è necessario:

- Verificare che l'ambiente di origine sia supportato. 
- [SQL Server Migration Assistant per SAP Adaptive Server Enterprise (in precedenza SAP Sybase ASE)](https://www.microsoft.com/download/details.aspx?id=54256). 

## <a name="pre-migration"></a>Pre-migrazione

Una volta soddisfatti i prerequisiti, si è pronti per individuare la topologia dell'ambiente e valutare la fattibilità della migrazione.

### <a name="assess"></a>Valutare

Usare [SQL Server Migration Assistant (SSMA) per SAP Adaptive Server Enterprise (formalmente SAP Sybase ASE)](https://www.microsoft.com/download/details.aspx?id=54256) per esaminare gli oggetti e i dati di database, valutare i database per la migrazione, migrare gli oggetti di database Sybase in SQL Server e quindi migrare i dati in SQL Server. Per ulteriori informazioni, vedere [SQL Server Migration Assistant per Sybase (SybaseToSQL)](../../../ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql.md).

Per creare una valutazione, seguire questa procedura: 

1. Aprire **SSMA per Sybase**. 
1. Selezionare **File** e quindi scegliere **Nuovo progetto**. 
1. Specificare un nome di progetto, un percorso in cui salvare il progetto, quindi selezionare SQL Server come destinazione della migrazione dall'elenco a discesa. Selezionare **OK**.
1. Immettere i valori per i dettagli della connessione SAP nella finestra di dialogo **Connetti a Sybase** . 
1. Fare clic con il pulsante destro del mouse sul database SAP di cui si desidera eseguire la migrazione, quindi scegliere **Crea report**. Verrà generato un report HTML.
1. Leggere il report HTML per esaminare le statistiche di conversione e gli eventuali errori o avvisi. È anche possibile aprire il report in Excel per ottenere un inventario degli oggetti di SAP ASE e lo sforzo necessario per eseguire le conversioni dello schema. La posizione predefinita del report è la cartella report all'interno di SSMAProjects.

   Ad esempio: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 


### <a name="validate-type-mappings"></a>Convalida mapping dei tipi

Prima di eseguire la conversione dello schema, convalidare i mapping del tipo di dati predefiniti o modificarli in base ai requisiti. Questa operazione può essere eseguita passando al menu **strumenti** e scegliendo **Impostazioni progetto** oppure è possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **Esplora metadati di SAP ASE**.


### <a name="convert-schema"></a>Converti schema

Per convertire lo schema, seguire questa procedura:

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**. 
1. Selezionare **Connetti a SQL Server** nella barra di spostamento in alto a linee e specificare SQL Server dettagli. È possibile scegliere di connettersi a un database esistente o specificare un nuovo nome, nel qual caso verrà creato un database nel server di destinazione.
1. Fare clic con il pulsante destro del mouse sullo schema SAP ASE in **Sybase Metadata Explorer** e scegliere **Converti schema**. In alternativa, è possibile selezionare **Converti schema** dalla barra di spostamento in alto a linee. 
1. Confrontare ed esaminare la struttura dello schema per identificare i potenziali problemi. 

   Dopo la conversione dello schema è possibile salvare il progetto localmente per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **File**. In questo modo è possibile valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di poter pubblicare lo schema in SQL Server.

Per altre informazioni, vedere [Convert schema](../../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md) .


## <a name="migrate"></a>Migrate 

Una volta soddisfatti i prerequisiti necessari e aver completato le attività associate alla fase di **pre-migrazione** , si è pronti per eseguire la migrazione dello schema e dei dati.

Per pubblicare lo schema ed eseguire la migrazione dei dati, attenersi alla procedura seguente: 

1. Fare clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e scegliere **Sincronizza con database**.  Questa azione pubblica lo schema di SAP ASE nell'istanza di SQL Server.
1. Fare clic con il pulsante destro del mouse sullo schema SAP ASE in **SAP ASE Metadata Explorer** e scegliere **Migrate data**.  In alternativa, è possibile selezionare **migrare i dati** dalla barra di spostamento in alto a linea.  
1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**: 
1. Convalidare la migrazione esaminando i dati e lo schema nell'istanza SQL Server usando SQL Server Management Studio (SSMS).


## <a name="post-migration"></a>Post-migrazione 

Dopo aver completato la fase di **migrazione** , è necessario eseguire una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza usavano l'origine devono iniziare a usare la destinazione. Per ottenere questo risultato, in alcuni casi sarà necessario apportare modifiche alle applicazioni.

### <a name="perform-tests"></a>Eseguire test

L'approccio di test per la migrazione del database consiste nell'eseguire le attività seguenti:

1. **Sviluppare i test di convalida**. Per testare la migrazione del database, è necessario usare le query SQL. È necessario creare le query di convalida da eseguire sia sul database di origine che su quello di destinazione. Le query di convalida devono essere estese all'ambito definito.

2. **Configurare l'ambiente di test**. L'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.

3. **Eseguire i test di convalida**. Eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.

4. **Eseguire test delle prestazioni**. Eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.

### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati e verificare la completezza, nonché per risolvere i problemi di prestazioni del carico di lavoro.

> [!NOTE]
> Per ulteriori dettagli su questi problemi e su passaggi specifici per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="migration-assets"></a>Risorse per la migrazione

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti, che sono state sviluppate a supporto di un progetto di migrazione del mondo reale.

| **Titolo/collegamento**                                                                                                        | **Descrizione**                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Guida all'ottimizzazione per i dati e l'app mainframe ricompilati in .NET & SQL Server](https://aka.ms/dmj-wp-mainframe-optimize) | Questa guida offre consigli sull'ottimizzazione per l'esecuzione di ricerche di punti su SQL Server da .NET nel modo più efficiente possibile. I clienti che desiderano eseguire la migrazione da database mainframe a SQL Server potrebbero voler eseguire la migrazione di modelli di progettazione ottimizzati per mainframe esistenti, soprattutto quando si usano strumenti di terze parti, ad esempio il compilatore Raincode, per eseguire automaticamente la migrazione del codice mainframe (ad esempio COBOL/JCL) a T-SQL e C# .NET. |

> [!NOTE]
> Queste risorse sono state sviluppate come parte del programma Data Migration Jumpstart (DM Jumpstart), sponsorizzato dal team di progettazione del gruppo di dati di Azure. Il core charter DM JumpStart è quello di sbloccare e accelerare la modernizzazione complessa e competere con le opportunità di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure. Se si ritiene che la propria organizzazione sia interessata a partecipare al programma DM JumpStart, contattare il team dell'account e richiedere l'invio di una candidatura.

## <a name="next-steps"></a>Passaggi successivi

- Per una panoramica della Guida alla migrazione del database di Azure e delle informazioni in esso contenute, vedere il video [su come usare la guida alla migrazione del database](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/).

- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per supportare diversi scenari di migrazione di database e dati, nonché per le attività speciali, vedere l'articolo [servizio e strumenti per la migrazione dei dati](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

- Per altre guide alla migrazione, vedere [Migrazione dei database](https://datamigration.microsoft.com/). 

Per i video, vedere: 
- [Panoramica del percorso di migrazione e degli strumenti e dei servizi consigliati per eseguire la valutazione e la migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
