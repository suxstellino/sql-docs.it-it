---
description: Viste (catalogo di Integration Services)
title: Viste (catalogo di Integration Services) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- views [Integration Services]
ms.assetid: d0294d43-4852-46dc-9afa-d0c19ea9aa03
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b4e067dc38f6bd511237ab05491feda4307106c4
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835989"
---
# <a name="views-integration-services-catalog"></a>Viste (catalogo di Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  In questa sezione vengono descritte le viste [!INCLUDE[tsql](../../includes/tsql-md.md)] disponibili per l'amministrazione di progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] distribuiti in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Eseguire una query sulle viste di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] per controllare oggetti, impostazioni e dati operativi archiviati nel catalogo **SSISDB**.  
  
 Il nome predefinito del catalogo è SSISDB. Tra gli oggetti archiviati nel catalogo sono inclusi progetti, pacchetti, parametri, ambienti e cronologia operativa.  
  
 È possibile utilizzare le viste del database e le stored procedure direttamente oppure scrivere codice personalizzato con cui chiamare l'API gestita. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e l'API gestita eseguono query sulle viste e chiamano le stored procedure descritte in questa sezione per eseguire molte delle rispettive attività.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [catalog.catalog_properties &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)  
 Vengono visualizzate le proprietà del catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.effective_object_permissions &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md)  
 Vengono visualizzate le autorizzazioni valide per l'entità corrente per tutti gli oggetti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_variables &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
 Vengono visualizzati i dettagli delle variabili di ambiente di tutti gli ambienti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environments &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
 Vengono visualizzati i dettagli relativi all'ambiente di tutti gli ambienti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Negli ambienti sono contenute variabili che possono fare riferimento ai progetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.execution_parameter_values &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
 Vengono visualizzati i valori effettivi del parametro utilizzati dai pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] durante un'istanza di esecuzione.  
  
 [catalog.executions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
 Vengono visualizzate le istanze di esecuzione del pacchetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. I pacchetti eseguiti con l'attività Esegui pacchetto vengono eseguiti nella stessa istanza di esecuzione del pacchetto padre.  
  
 [catalog.explicit_object_permissions &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md)  
 Vengono visualizzate solo le autorizzazioni assegnate in modo esplicito all'utente.  
  
 [catalog.extended_operation_info &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
 Vengono visualizzate le informazioni estese per tutte le operazioni nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.folders &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md)  
 Vengono visualizzate le cartelle nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_parameters &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
 Vengono visualizzati i parametri per tutti i pacchetti e progetti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_versions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
 Vengono visualizzate le versioni di oggetti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. In questa versione sono supportate solo le versioni di progetto in questa vista.  
  
 [catalog.operation_messages &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
 Vengono visualizzati i messaggi registrati durante le operazioni nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.operations &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
 Vengono visualizzati i dettagli di tutte le operazioni nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.packages &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
 Vengono visualizzati i dettagli di tutti i pacchetti visualizzati nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_references &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
 Vengono visualizzati i riferimenti all'ambiente per tutti i progetti nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.projects &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
 Vengono visualizzati i dettagli per tutti i progetti visualizzati nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.validations &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md)  
 Vengono visualizzati i dettagli di tutte le convalide del progetto e del pacchetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
[catalog.master_properties &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)  
Consente di visualizzare le proprietà di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Master.

[catalog.worker_agents &#40;database SSISDB&#41;](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)  
Visualizza le informazioni su [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker.  
