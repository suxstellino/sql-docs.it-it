---
description: 'Esercitazione: Uso del tipo di dati hierarchyid'
title: 'Esercitazione: Uso del tipo di dati hierarchyid | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
helpviewer_keywords:
- tutorials [hierarchyid]
- hierarchyid [Database Engine], tutorial
ms.assetid: 5a7f7cfd-7faf-439f-8085-8fd6bf7db355
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8aae425506ebc8a2a17aadb2fa9c2d4485fe8f85
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97426740"
---
# <a name="tutorial-using-the-hierarchyid-data-type"></a>Esercitazione: Uso del tipo di dati hierarchyid
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Questa esercitazione è destinata agli utenti che hanno esperienza nell'uso di [!INCLUDE[tsql](../../includes/tsql-md.md)], ma non hanno familiarità con il tipo di dati **hierarchyid**.  
  
## <a name="what-you-will-learn"></a>Lezioni dell'esercitazione  
L'esercitazione è suddivisa in due lezioni:  
  
[Lezione 1: Conversione di una tabella in una struttura gerarchica](../../relational-databases/tables/lesson-1-converting-a-table-to-a-hierarchical-structure.md)  
In questa lezione si prende in considerazione una tabella Employee esistente strutturata come gerarchia padre/figlio, quindi si spostano i dati nella nuova tabella che rappresenta la gerarchia usando il tipo di dati **hierarchyid** . Questa lezione richiede il database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
[Lezione 2: Creazione e gestione di dati in una tabella gerarchica](../../relational-databases/tables/lesson-2-creating-and-managing-data-in-a-hierarchical-table.md)  
In questa lezione si crea una tabella usando il tipo di dati **hierarchyid** per rappresentare la struttura della gerarchia. Pertanto, si modificano i dati della tabella utilizzando i metodi gerarchici.  
  
## <a name="requirements"></a>Requisiti  
È necessario che nel sistema siano installati i componenti seguenti:  
  
-   Qualsiasi edizione di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versione successiva.  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] Express.  
  
-   Internet Explorer 6 o versioni successive.  
  
## <a name="see-also"></a>Vedi anche  
[Esercitazione: Introduzione al motore di database](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
[Esercitazione: Scrittura di istruzioni Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md)  
[Guida di riferimento ai metodi per il tipo di dati hierarchyid](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
[Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
  
  
