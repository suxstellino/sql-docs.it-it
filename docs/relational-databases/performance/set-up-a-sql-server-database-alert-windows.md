---
title: Impostare un avviso del database di SQL Server (Windows) | Microsoft Docs
description: Informazioni su come creare un avviso da generare quando un contatore di Monitor di sistema raggiunge un valore soglia. In risposta, Monitor di sistema può avviare un'applicazione.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
ms.assetid: 65d2c5c1-921f-4eff-9ef7-149170ab61e8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60e50e5680aebd9db384a8f4ace56ee5687d640a
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504993"
---
# <a name="set-up-a-sql-server-database-alert-windows"></a>Impostare un avviso del database di SQL Server (Windows)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  È possibile usare Monitoraggio di sistema per creare un avviso da generare quando un contatore di Monitoraggio di sistema raggiunge un valore soglia. In risposta all'avviso, Monitoraggio di sistema può avviare un'applicazione, ad esempio un'applicazione personalizzata programmata per gestire la condizione dell'avviso. Ad esempio, è possibile creare un avviso che viene generato quando il numero di deadlock supera un valore specifico. 
  
 È anche possibile impostare gli avvisi tramite Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per altre informazioni, vedere [Avvisi](../../ssms/agent/alerts.md).  
  
## <a name="set-up-a-sql-server-database-alert"></a>Impostare un avviso del database di SQL Server  
  
1. Nell'albero di navigazione della finestra **Prestazioni** espandere **Avvisi e registri di prestazioni**.  
  
2. Fare clic con il pulsante destro del mouse su **Avvisi** e selezionare **Impostazioni nuovo avviso**.
  
3. Nella finestra di dialogo **Impostazioni nuovo avviso** digitare un nome per il nuovo avviso e selezionare **OK**.  
  
4. Nella scheda **Generale** della finestra di dialogo relativa al nuovo avviso inserire un **Commento**. Selezionare **Aggiungi** per aggiungere un contatore all'avviso.  
  
     Tutti gli avvisi devono avere almeno un contatore.  
  
5. Nella finestra di dialogo **Aggiungi contatori** selezionare un oggetto SQL Server dall'elenco **Oggetto prestazioni**. Selezionare un contatore da **Seleziona i contatori dall'elenco**.  
  
6. Per aggiungere il contatore all'avviso, selezionare **Aggiungi**. È possibile continuare ad aggiungere contatori oppure selezionare **Chiudi** per tornare alla finestra di dialogo del nuovo avviso.  
  
7. Nella finestra di dialogo del nuovo avviso selezionare **Over** (Superiore a) o **Under** (Inferiore a) nell'elenco **Avvisa quando il valore è**. In seguito immettere un valore soglia in **Limite**.  
  
     L'avviso viene generato quando il valore del contatore è superiore o inferiore al valore soglia, a seconda che sia stata selezionata l'opzione **Over** (Superiore a) o **Under** (Inferiore a).  
  
8. Nelle caselle **Campiona dati ogni** impostare la frequenza di campionamento.  
  
9. Nella scheda **Azione** impostare le azioni in modo che vengano eseguite ogni volta che viene generato l'avviso.  
  
10. Nella scheda **Pianificazione** impostare l'avvio e l'arresto pianificati per l'analisi dell'avviso.  
  
## <a name="see-also"></a>Vedere anche  
 [Creare un avviso del database di SQL Server](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)  
  
  
