---
title: Connettersi a un database e visualizzare gli oggetti esistenti
description: Informazioni su come usare Esplora oggetti di SQL Server in Visual Studio per connettersi a istanze di SQL Server locali e non locali.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.connectionpicker.f1
ms.assetid: 9b331800-3806-4459-ac58-88cdc98124d3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b2a8c7ae0bbd50598d6c476c0a065786e6efcddb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018421"
---
# <a name="how-to-connect-to-a-database-and-browse-existing-objects"></a>Procedura: Connettersi a un database e visualizzare gli oggetti esistenti

Un'attività molto comune per sviluppatori e amministratori di database è connettersi a un database attivo, progettare o sfogliare il relativo schema ed eseguire una query sugli oggetti. In Esplora oggetti di SQL Server in Visual Studio è ora disponibile un nodo **SQL Server** dedicato in cui tutte le istanze di SQL Server connesse e i relativi database sono raggruppati in una gerarchia analoga a quella di SSMS. Le istanze di SQL Server connesse possono essere locali, quale l'esecuzione di SQL Server 2008, o istanze di SQL Azure ospitate.  
  
Nella procedura seguente si presuppone che si dispone già di un database di esempio AdventureWorks installato. Usare [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) per individuare e installare database di esempio per diverse versioni di SQL Server. Se si preferisce, è possibile seguire anche i passaggi e utilizzare un database esistente nel server.  
  
### <a name="to-connect-to-a-database-instance"></a>Per connettersi a un'istanza del database  
  
1.  In Visual Studio verificare che **Esplora oggetti di SQL Server** sia aperto. In caso contrario, fare clic sul menu **Visualizza** e selezionare **Esplora oggetti di SQL Server**.  
  
2.  Fare clic con il pulsante destro del mouse sul nodo **SQL Server** in **Esplora oggetti di SQL Server** e selezionare **Aggiungi SQL Server**.  
  
3.  Nella finestra di dialogo **Connetti al server** immettere il **Nome server** dell'istanza del server a cui si desidera eseguire la connessione, le credenziali e fare clic su **Connetti**.  
  
4.  In **Esplora oggetti di SQL Server** espandere il nodo **Database** nell'istanza del server. Verranno visualizzati tutti i database che si trovano in questa istanza del server aggiunta in questo nodo **Database** .  
  
5.  Espandere il nodo **AdventureWorks** (o un altro database). Si noterà che tutte le entità del database sono organizzate in una gerarchia simile a SQL Server Management Studio.  
  
