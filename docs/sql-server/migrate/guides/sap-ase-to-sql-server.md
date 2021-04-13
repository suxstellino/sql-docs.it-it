---
title: 'Guida alla migrazione da SAP ASE a SQL Server:'
description: Questa guida illustra come eseguire la migrazione dei database SAP ASE a Microsoft SQL Server usando SQL Server Migration Assistant per SAP ASE (SSMA per SAP ASE).
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 7c413bb99520e30882e1746763d803d57006e6bb
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210867"
---
# <a name="migration-guide-sap-ase-to-sql-server"></a>Guida alla migrazione: SAP ASE per SQL Server

[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

Questa guida illustra come eseguire la migrazione dei database SAP ASE a SQL Server usando SQL Server Migration Assistant per SAP ASE (SSMA per SAP ASE).

Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://docs.microsoft.com/data-migration).

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare la migrazione del database SAP ASE a SQL Server:

- Verificare che l'ambiente di origine sia supportato.
- Ottenere [SQL Server Migration Assistant per SAP Adaptive Server Enterprise (in precedenza SAP Sybase ASE)](https://www.microsoft.com/download/details.aspx?id=54256).
- Ottenere connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione.

## <a name="pre-migration"></a>Pre-migrazione

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare la topologia dell'ambiente e valutare la fattibilità della migrazione.

### <a name="assess"></a>Valutare

Con SSMA per SAP ASE è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, migrare gli oggetti di database di Sybase a SQL Server e quindi migrare i dati SQL Server. Per ulteriori informazioni, vedere [SQL Server Migration Assistant per Sybase (SybaseToSQL)](../../../ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql.md).

Per creare una valutazione:

1. Aprire [SSMA per SAP ASE](https://www.microsoft.com/download/details.aspx?id=54256).
1. Scegliere **Nuovo progetto** dal menu **File**.
1. Immettere un nome di progetto e un percorso per salvare il progetto. Selezionare quindi **SQL Server** come destinazione della migrazione dall'elenco a discesa e fare clic su **OK**.
1. Nella finestra di dialogo **Connetti a Sybase** immettere i valori per i dettagli della connessione SAP.
1. Fare clic con il pulsante destro del mouse sul database SAP di cui si desidera eseguire la migrazione e quindi scegliere **Crea report** per generare un report HTML.
1. Leggere il report HTML per esaminare le statistiche di conversione e gli eventuali errori o avvisi. È anche possibile aprire il report in Excel per ottenere un inventario degli oggetti di SAP ASE e lo sforzo necessario per eseguire le conversioni dello schema. Il percorso predefinito per il report si trova nella cartella report in SSMAProjects, come illustrato di seguito:

    `drive:\<username>\Documents\SSMAProjects\MySAPMigration\report\report_<date>`.

### <a name="validate-the-type-mappings"></a>Convalidare i mapping dei tipi

Prima di eseguire una conversione dello schema, convalidare i mapping del tipo di dati predefiniti o modificarli in base ai requisiti. È possibile passare al menu **strumenti** e selezionare **Impostazioni progetto** oppure è possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **Esplora metadati di SAP ASE**.

### <a name="convert-the-schema"></a>Convertire lo schema

Per convertire lo schema:

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**.
1. Selezionare la scheda **Connetti a SQL Server** e immettere SQL Server dettagli. È possibile scegliere di connettersi a un database esistente o immettere un nuovo nome, nel qual caso verrà creato un database nel server di destinazione.
1. Fare clic con il pulsante destro del mouse sul database o sull'oggetto di cui si vuole eseguire la migrazione in **Esplora metadati di SAP ASE** e selezionare **Migrate data**. In alternativa, è possibile selezionare la scheda **Migrate data** . Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare le caselle di controllo accanto alle tabelle. Per omettere i dati dalle singole tabelle, deselezionare le caselle di controllo.
1. Confrontare ed esaminare la struttura dello schema per identificare i potenziali problemi.

   Al termine della conversione dello schema, è possibile salvare il progetto localmente per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **file** . Questo passaggio consente di valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di pubblicare lo schema in SQL Server.

Per altre informazioni, vedere [Convert schema](../../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).

## <a name="migrate"></a>Migrate

Una volta soddisfatti i prerequisiti necessari e aver completato le attività associate alla fase di *pre-migrazione* , si è pronti per eseguire la migrazione dello schema e dei dati.

Per pubblicare lo schema ed eseguire la migrazione dei dati:

1. Pubblicare lo schema facendo clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e selezionando **Sincronizza con database**. Questa azione pubblica lo schema di SAP ASE nell'istanza di SQL Server.
1. Migrare i dati facendo clic con il pulsante destro del mouse sul database o sull'oggetto di cui si vuole eseguire la migrazione in **Esplora metadati di SAP ASE** e selezionando **Migrate data** In alternativa, è possibile selezionare la scheda **Migrate data** . Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare le caselle di controllo accanto alle tabelle. Per omettere i dati dalle singole tabelle, deselezionare le caselle di controllo.
1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**.
1. Connettersi all'istanza di SQL Server utilizzando [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)e convalidare la migrazione riesaminando i dati e lo schema.

## <a name="post-migration"></a>Post-migrazione

Dopo aver completato la fase di *migrazione* , è necessario completare una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo aver eseguito la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza utilizzano l'origine devono iniziare a utilizzare la destinazione. Per eseguire questa attività, in alcuni casi è necessario apportare modifiche alle applicazioni.

### <a name="perform-tests"></a>Eseguire test

L'approccio di test per la migrazione del database prevede le attività seguenti:

1. **Sviluppare i test di convalida**: per testare la migrazione del database, è necessario usare query SQL. È necessario creare le query di convalida da eseguire sia sul database di origine che su quello di destinazione. Le query di convalida devono coprire l'ambito definito.
1. **Configurare un ambiente di test**: l'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.
1. **Eseguire i test di convalida**: eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.
1. **Eseguire test delle prestazioni**: eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.

### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati, verificare la completezza e risolvere i problemi relativi alle prestazioni con il carico di lavoro.

> [!Note]
> Per ulteriori informazioni su questi problemi e sui passaggi per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md).

## <a name="migration-assets"></a>Risorse per la migrazione

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere la seguente risorsa. È stata sviluppata in supporto di un impegno di progetto di migrazione reale.

| Titolo                                                                                                       | Descrizione                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Guida all'ottimizzazione per i dati e l'app mainframe ricompilati in .NET & SQL Server](https://aka.ms/dmj-wp-mainframe-optimize) | Questa guida offre consigli sull'ottimizzazione per l'esecuzione di ricerche di punti su SQL Server da .NET nel modo più efficiente possibile. I clienti che desiderano eseguire la migrazione da database mainframe a SQL Server potrebbero voler eseguire la migrazione di modelli di progettazione ottimizzati per mainframe esistenti, specialmente quando usano strumenti di terze parti, ad esempio il compilatore Raincode, per eseguire automaticamente la migrazione del codice mainframe (ad esempio COBOL/JCL) a T-SQL e C# .NET. |

> [!NOTE]
> Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

- Per una panoramica della Guida alla migrazione del database di Azure e delle informazioni in esso contenute, vedere il video [su come usare la guida alla migrazione del database](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/).
- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati e attività speciali, vedere [Servizi e strumenti per la migrazione dei dati](https://docs.microsoft.com/azure/dms/dms-tools-matrix).
- Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://datamigration.microsoft.com/).
- Per un video sulla migrazione, vedere [Panoramica del percorso di migrazione e gli strumenti e i servizi consigliati per eseguire la valutazione e la migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
