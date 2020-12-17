---
title: Sviluppo di database orientato ai progetti utilizzando gli strumenti della riga di comando
description: Visualizzare le risorse disponibili per gli strumenti da riga di comando forniti da SQL Server Data Tools per l'uso di file con estensione dacpac, ad esempio SqlPackage.exe e dbSqlPackage.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9a26def9-8fbd-43e4-9e57-414840b73ed8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: f84638d0ded57fcc1312258422de522aec997b39
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559243"
---
# <a name="project-oriented-database-development-using-command-line-tools"></a>Sviluppo di database orientato ai progetti utilizzando gli strumenti della riga di comando

SQL Server Data Tools offre strumenti della riga di comando che consentono numerosi scenari di sviluppo di database orientato ai progetti.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-|-|  
|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)|In questo argomento viene descritta l'utilità SQLPackage.exe che consente di eseguire le attività seguenti:<br /><br />- Estrazione di un file con estensione dacpac da un database di SQL Server attivo.<br />- Pubblicazione di un file con estensione dacpac in un database di SQL Server attivo per aggiornare in modo incrementale lo schema del database attivo affinché corrisponda al file con estensione dacpac.<br />- Confronto di un file con estensione dacpac con un database di SQL Server attivo e generazione di uno script Transact\-SQL di aggiornamento incrementale senza l'aggiornamento del database attivo.<br />- Confronto di due file con estensione dacpac per la generazione di uno script Transact\-SQL di aggiornamento incrementale.<br />- Generazione di un report XML in cui vengono riepilogate le modifiche dell'aggiornamento incrementale che si verificherebbero in caso di aggiornamento incrementale del database.|  
|[Uso di MSDeploy con il provider dbSqlPackage](../ssdt/using-msdeploy-with-dbsqlpackage-provider.md)|Questo argomento descrive il provider dello [Strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=231798) denominato dbSqlPackage incluso in SSDT, che interagisce con lo Strumento di distribuzione Web (MSDeploy.exe) di Microsoft Internet Information Services (IIS), usato per le attività seguenti:<br /><br />- Estrazione di un file con estensione dacpac da un database di SQL Server o un database SQL di Azure remoto o locale.<br />- Pubblicazione di un file con estensione dacpac in un database di SQL Server o un database SQL di Azure remoto o locale per l'aggiornamento incrementale.<br />- Pubblicazione da un database di SQL Server locale in un database di SQL Server o un database SQL di Azure remoto per l'aggiornamento  incrementale del database remoto.<br />- Confronto di un file con estensione dacpac con un database di SQL Server o un database SQL di Azure remoto o locale per generare uno script Transact\-SQL di aggiornamento incrementale senza l'aggiornamento del database attivo.<br />- Generazione di un report XML in cui vengono riepilogate le modifiche dell'aggiornamento incrementale che si verificherebbero in caso di aggiornamento incrementale del database.|  
  
## <a name="related-sections"></a>Sezioni correlate  
[Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md)  
  
