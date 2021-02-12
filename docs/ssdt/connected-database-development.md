---
title: Sviluppo del database connesso
description: Scoprire in che modo SQL Server Data Tools può interagire con i database connessi. Informazioni su come esplorare entità, progettare tabelle, modificare script ed eseguire altre attività.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 21f7f959-7b8e-4335-8681-bebcd957692c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 46dd6a4df85b57cbd18817debf55f95d6cf982e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077171"
---
# <a name="connected-database-development"></a>Sviluppo del database connesso

Questa sezione descrive le funzionalità fornite da SQL Server Data Tools per la progettazione di un database connesso e l'esecuzione di query su tale database.  
  
Se si usa Esplora oggetti di SQL Server in Visual Studio, gli sviluppatori possono ora creare, modificare e sfogliare oggetti di database posizionati in un server di database locale, quale SQL Server 2008 o Microsoft SQL Server 2012 oppure ospitati in SQL Azure. Possono inoltre facilmente clonare un database di produzione esistente in un'istanza di test, effettuare in esso un'operazione di sviluppo aggiuntiva e pubblicare di nuovo le modifiche nel database di produzione.  
  
> [!NOTE]  
> Negli argomenti in cui sono descritte le procedure inclusi in questa sezione è contenuta una serie di attività che può essere completata in una sequenza.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|---------|---------------|  
|[Procedura: Connettersi a un database e visualizzare gli oggetti esistenti](../ssdt/how-to-connect-to-a-database-and-browse-existing-objects.md)|Connettersi a un database e sfogliare le relative entità.|  
|[Procedura: Creare oggetti di database tramite Progettazione tabelle](../ssdt/how-to-create-database-objects-using-table-designer.md)|Utilizzare la nuova Progettazione tabelle per progettare tabelle e gestire le relative relazioni.|  
|[Procedura: Aggiornare un database connesso con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md)|Aggiornare un database connesso senza scrivere script ALTER.|  
|[Finestra di dialogo Filtra e ordina](../ssdt/filter-and-sort-dialog-box.md)|Specificare quali dati devono essere visualizzati nella Vista dati.|  
|[Procedura: Creare nuovi oggetti di database tramite query](../ssdt/how-to-create-new-database-objects-using-queries.md)|Usare l'Editor Transact\-SQL per modificare ed eseguire script Transact\-SQL.|  
|[Procedura: Modificare una tabella esistente tramite query](../ssdt/how-to-edit-an-existing-table-using-queries.md)|Scrivere script Transact\-SQL per modificare la definizione di una tabella o per popolare i dati.|  
|[Procedura: Visualizzare e modificare dati in una tabella](../ssdt/how-to-view-and-edit-data-in-a-table.md)|Utilizzare l'Editor dati per visualizzare o immettere dati in una tabella.|  
|[Procedura: Eliminare oggetti e risolvere le dipendenze](../ssdt/how-to-delete-objects-and-resolve-dependencies.md)|Rinominare o eliminare entità del database e consentire a SQL Server Data Tools di risolvere automaticamente le dipendenze degli oggetti.|  
|[Procedura: Clonare un database esistente](../ssdt/how-to-clone-an-existing-database.md)|Creare un database di sviluppo da un database di produzione.|  
|[Estrarre, pubblicare e registrare file con estensione dacpac](../ssdt/extract-publish-and-register-dacpac-files.md)|Viene mostrato come estrarre e pubblicare file con estensione dacpac.|  
  
## <a name="related-sections"></a>Sezioni correlate

[Gestire tabelle e relazioni e correggere errori](../ssdt/manage-tables-relationships-and-fix-errors.md)