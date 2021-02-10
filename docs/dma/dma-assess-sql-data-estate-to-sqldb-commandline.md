---
title: 'DMACMD: valutazione della disponibilità SQL Server per la migrazione ad Azure SQL'
titleSuffix: Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant strumento da riga di comando (DMACMD) per valutare un SQL Server di dati per la migrazione ad Azure SQL
ms.date: 10/02/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: ''
ms.openlocfilehash: 8688fa61a1ccaede3bd370e1d19ccd66531f5739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060936"
---
# <a name="dmacmd-assess-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql"></a>DMACMD: consente di valutare la conformità di un SQL Server dati di migrazione ad Azure SQL 

Con molte organizzazioni che tentano di eseguire la migrazione ad Azure, è fondamentale valutare le istanze di SQL Server locali esistenti e identificare la destinazione SQL di Azure appropriata, ovvero il database SQL di Azure, Istanza gestita SQL di Azure o SQL Server in macchine virtuali di Azure. 

[Data Migration Assistant (DMA)](dma-overview.md) consente di valutare un'istanza SQL Server per una specifica destinazione SQL di Azure e di misurare la conformità dei database SQL Server che eseguono la migrazione ad Azure SQL. Caricare i risultati della valutazione DMA nell'hub Azure Migrate per una visualizzazione centralizzata della conformità dell'intero patrimonio di dati. 

Questo articolo illustra come eseguire valutazioni su larga scala e caricare i risultati in Azure Migrate hub usando l'interfaccia della riga di comando DMA (DMACMD). In alternativa, è possibile usare l' [interfaccia utente grafica DMA](dma-assess-sql-data-estate-to-sqldb.md) per eseguire la valutazione.

Per altre informazioni, vedere il video di Channel9 seguente:

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-to-Assess-Readiness-of-SQL-Server-Data-Estate-Migrating-to-Azure-SQL/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prerequisiti 

Per usare DMACMD per eseguire una valutazione e caricare i risultati in Azure Migrate Hub, è necessario quanto segue: 

- La [versione più recente di Data Migration Assistant (DMA)](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
- [Progetto Azure migrate](dma-assess-sql-data-estate-to-sqldb.md#create-a-project-and-add-a-tool). 
- Accesso del ruolo Collaboratore alla risorsa del progetto Azure Migrate.

## <a name="use-dmacmd"></a>Usare DMACMD

Usare un file XML come input per eseguire valutazioni su larga scala usando l'interfaccia della riga di comando (DMACMD.exe). 

Usare il comando di esempio seguente per passare un file XML a DMACMD e avviare la valutazione:

```console
C:\Program Files\Microsoft Data Migration Assistant\DmaCmd.exe /Action=Assess /AssessmentConfiguration= C:\Demo\ScaleAssessment\Assess-for-AzureSQLMI.xml
```

Il contenuto dell'esempio `Assess-for-AzureSQLMI.xml` definisce gli elementi per valutare SQL Server istanze per una destinazione SQL istanza gestita: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AssessmentConfiguration xmlns="http://microsoft.com/schemas/SqlServer/Advisor/AssessmentConfiguration">
   <AssessmentName>Scale-Assessment-for-AzureSQLManagedInstance</AssessmentName>
   <AssessmentSourcePlatform>SqlOnPrem</AssessmentSourcePlatform>
   <AssessmentTargetPlatform>ManagedSqlServer</AssessmentTargetPlatform>
   <AssessmentDatabases>
      <AssessmentDatabase>Server=ServerName\SQL2017;Integrated Security=true</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=AdventureWorks2016</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=TestDB</AssessmentDatabase>
   </AssessmentDatabases>
   <AssessmentResultDma>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.dma</AssessmentResultDma>
   <AssessmentResultJson>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.json</AssessmentResultJson>
   <AssessmentResultCsv>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.csv</AssessmentResultCsv>
   <AssessmentOverwriteResult>true</AssessmentOverwriteResult>
   <AssessmentEvaluateCompatibilityIssues>true</AssessmentEvaluateCompatibilityIssues>
   <AssessmentEvaluateFeatureParity>true</AssessmentEvaluateFeatureParity>
   <AzureCloudEnvironment>Azure</AzureCloudEnvironment>
   <SubscriptionId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx</SubscriptionId>
   <AzureMigrateProjectName>Scale-Assessment-for-AzureSQLMI</AzureMigrateProjectName>
   <ResourceGroupName>Resource-Group-Name</ResourceGroupName>
   <AzureAuthenticationInteractiveAuthentication>true</AzureAuthenticationInteractiveAuthentication>
   <AzureAuthenticationTenantId>xxxxxxxx-xxxx-xxxxxxxx</AzureAuthenticationTenantId>
   <EnableAssessmentUploadToAzureMigrate>true</EnableAssessmentUploadToAzureMigrate>
</AssessmentConfiguration>
```



## <a name="xml-elements"></a>Elementi XML 

Gli elementi XML passati a DMACMD sono definiti nella tabella seguente: 


|**Elemento XML** |**Definition**  |
|---------|---------|
|`AssessmentName`|Nome della valutazione|
|`AssessmentSourcePlatform`|Piattaforma SQL Server di origine. Il valore predefinito è `SqlOnPrem`.|
|`AssessmentTargetPlatform`|Piattaforma SQL Server di destinazione.  </br> `AzureSqlDatabase` è per una destinazione del database SQL di Azure. </br> `ManagedSqlServer` è per una destinazione di Istanza gestita SQL di Azure. </br></br>L'esempio **valutazione-for-AzureSQLMI** valuta una destinazione SQL istanza gestita.|
|`AssessmentDatabases`|Se è necessario valutare tutti i database in un'istanza di, specificare solo il nome dell'istanza. in caso contrario, elencare i database specifici in ogni riga. </br></br>L'esempio **valutazione-for-AzureSQLMI** valuta tutti i database nell'istanza `Servername\SQL2017` e due database specifici nell'istanza `Servername\SQL2016` .  |
|`AssessmentResultDma` </br> `AssessmentResultJson` </br> `AssessmentResultCsv` | Specifica il formato del file di risultati. `.DMA``.JSON`rispettivamente,, e `.CSV` . Fare doppio clic `.DMA` per aprire nell'interfaccia utente DMA. <br> `AssessmentResultDma` è necessario per caricare i risultati della valutazione nell'hub Azure Migrate.  |
|`AssessmentOverwriteResult`| Indica se sovrascrivere un file di risultati di valutazione esistente con lo stesso percorso di `AssessmentResultJson` `AssessmentResultDma` o `AssessmentResultCsv` .|
|`AssessmentEvaluateCompatibilityIssues` </br> `AssessmentEvaluateFeatureParity` |Eseguire la valutazione per valutare i problemi di compatibilità e i problemi di parità delle funzionalità.|
|`AzureCloudEnvironment`|Ambiente cloud di Azure a cui connettersi, il valore predefinito è il cloud pubblico di Azure. </br></br> Valori supportati: </br>`Azure (default)`, `AzureChina`, `AzureGermany`, `AzureUSGovernment`.|
|`SubscriptionId`|ID sottoscrizione di Azure.|
|`AzureMigrateProjectName`|Azure Migrate nome del progetto in cui caricare i risultati della valutazione.|
|`ResourceGroupName`|Azure Migrate nome del gruppo di risorse.|
|`AzureAuthenticationInteractiveAuthentication`|Impostare su `true` per visualizzare la finestra di autenticazione.|
|`AzureAuthenticationTenantId`|ID del tenant di Azure Active Directory. </br></br>Ottenere questo dal pannello **Panoramica** di Azure Active Directory nel [portale di Azure](https://portal.azure.com). |
|`EnableAssessmentUploadToAzureMigrate`| Impostare su `true` per caricare e pubblicare i risultati della valutazione nell'hub Azure migrate.|
|   |   |


## <a name="results"></a>Risultati 

DMACMD restituisce uno stato quando viene completato correttamente. 


Di seguito è riportato un esempio di output dei risultati: 

```console
Assessment finished for project: Scale-Assessment-for-AzureSQLManagedInstance
DATABASES:
Succeeded             : 4
Failed                : 0
SERVER INSTANCES:
Succeeded             : 2
Failed                : 0
CSV result file       : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.csv
JSON result file      : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.json
--------------------------------------------------------------------------------
```

Visualizza i risultati caricati in [Azure migrate](dma-assess-sql-data-estate-to-sqldb.md#view-target-readiness-assessment-results) per una visualizzazione centralizzata dell'intera proprietà dei dati. . 

## <a name="best-practices"></a>Procedure consigliate 

Quando si usa DMACMD, tenere presenti le seguenti procedure consigliate: 

- Raggruppare in modo logico le istanze di SQL Server e i database in base all'applicazione, anziché valutare tutte le istanze di SQL Server nell'intera proprietà dei dati. 
- Creare un progetto di Azure Migrate separato per ogni destinazione SQL di Azure per evitare di sovrascrivere i risultati. 
- Il tempo necessario per eseguire una valutazione dipende dal numero di oggetti di database. Se possibile, evitare di eseguire valutazioni sul sistema di produzione e di eseguire l'offload in una macchina virtuale o in un server di gestione temporanea, in particolare per i database con un numero elevato di oggetti. 


## <a name="see-also"></a>Vedi anche

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: impostazioni di configurazione](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedure consigliate](../dma/dma-bestpractices.md)

