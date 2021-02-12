---
title: Aggiornare un progetto di test precedente contenente unit test del database
description: Informazioni su come aggiornare i progetti di test di Visual Studio 2010 che contengono unit test del database. Scoprire come usare SQL Server Data Tools con tali progetti.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 42782ff3-e8cf-4c9d-8dac-a95b236edfc4
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2830890ecb1fc9983ce4ccfac61874d4e529f07b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066856"
---
# <a name="upgrade-an-older-test-project-containing-database-unit-tests"></a>Aggiornare un progetto di test precedente contenente unit test del database

È possibile aggiornare un progetto di test precedente creato in Visual Studio 2010 contenente gli unit test del database in modo da usare i nuovi strumenti e runtime per unit test del database SQL Server Data Tools. Dopo aver aggiornato un progetto precedente è possibile aggiungere unit test di SQL Server al progetto. Per altre informazioni, vedere [Creazione e definizione di unit test di SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md).  
  
> [!TIP]  
> Se si usa Visual Studio 2010, dopo aver aggiunto gli unit test di SQL Server a un progetto dei test, non è possibile aggiungere unit test usando il vecchio modello di unit test del database. Se viene utilizzato, è necessario convertire nuovamente il progetto per poter eseguire correttamente i test.  
  
Se un progetto di test del database è stato creato in una versione precedente a Visual Studio 2010, fare riferimento all'argomento [Procedura: Aggiornare Unit test da versioni precedenti di Visual Studio](/previous-versions/visualstudio/visual-studio-2010/dd193412(v=vs.100)) per aggiornare il progetto di database a Visual Studio 2010, prima di aggiornare il progetto a SQL Server Data Tools.  
  
### <a name="initiating-an-upgrade"></a>Avvio di un aggiornamento  
  
-   È possibile avviare un aggiornamento del progetto dal menu di scelta rapida di un progetto di test.  
  
    In alcuni casi, SQL Server Data Tools visualizza una finestra di dialogo da cui è possibile avviare l'aggiornamento di un progetto di test.  
  
-   L'aggiornamento del progetto rimuove il riferimento dell'assembly al precedente framework di test del database e aggiunge un riferimento al nuovo framework e un assembly dell'adattatore. Anche il file app.config viene aggiornato.  
  
    > [!NOTE]  
    > Se il progetto di test include i file di codice per DatabaseSetup e SQLDatabaseSetup, l'aggiornamento del progetto a SQL Server Data Tools esclude il file DatabaseSetup dalla compilazione. È possibile rimuovere il file DatabaseSetup se viene escluso dalla compilazione.  
  
-   Dopo la conversione, gli unit test del database creati con il modello precedente utilizzeranno i tipi presenti nell'assembly dell'adattatore per accedere al nuovo framework. L'utilizzo dell'assembly dell'adattatore fa si che la procedura di aggiornamento non modifichi il codice e gli script di test. Se si aggiunge uno unit test di SQL Server al progetto, il nuovo test farà riferimento al nuovo framework direttamente e non tramite l'adattatore. È possibile scegliere di aggiornare manualmente il codice per utilizzare il nuovo framework per congruenza con i nuovi test, ma non è obbligatorio.  
  
## <a name="see-also"></a>Vedere anche  
[Verifica del codice di database tramite unit test di SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
