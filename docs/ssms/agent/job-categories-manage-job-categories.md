---
description: Categorie di processi - Gestisci categorie di processi
title: Categorie di processi - Gestisci categorie di processi
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.categories.f1
helpviewer_keywords:
- Job Categories dialog box
ms.assetid: 38276438-40b1-43ce-9aae-6805be6d9332
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2dd854780c892da693b4f3950a88524a961a5fb2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402571"
---
# <a name="job-categories---manage-job-categories"></a>Categorie di processi - Gestisci categorie di processi
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilizzare la finestra di dialogo **Categorie di processi** per aggiungere o eliminare categorie di processi. Le categorie di processi predefinite non possono essere eliminate.  
  
## <a name="options"></a>Opzioni  
**Nome**  
Nome della categoria di processi.  
  
**Numero di processi nella categoria**  
Numero di processi definiti per la categoria corrente.  
  
**Visualizza processi**  
Consente di aprire la finestra di dialogo **Proprietà** relativa alla categoria selezionata in cui sono elencati tutti i processi attualmente definiti per quella categoria.  
  
**Aggiungere**  
Consente di aprire la finestra di dialogo **Nuova categoria di processi** per aggiungere una nuova categoria di processi  
  
**Elimina**  
Consente di eliminare la categoria di processi selezionata. Questa opzione è abilitata solo per le categorie di processi definite dall'utente.  
  
**Aggiorna**  
Esegue una query sul server per ottenere informazioni correnti.  
  
#### <a name="to-access-the-job-categories-dialog-box"></a>Per accedere alla finestra di dialogo Categorie di processi  
  
1.  In **Esplora oggetti** espandere **SQL Server Agent**, fare clic con il pulsante destro del mouse su **Processi** e scegliere **Gestisci categorie di processi**.  
