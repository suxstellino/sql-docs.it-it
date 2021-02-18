---
description: Riconciliazione di un diagramma di database con un database modificato (Visual Database Tools)
title: Riconciliare un diagramma di database con un database modificato
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- updating diagram to match database
- reconciling database diagrams
- diagrams [SQL Server], reconciling changes
- updating database to match diagram
- database diagrams [SQL Server], reconciling changes
ms.assetid: eda8dea2-eedd-43a7-85aa-92bd97783b5f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: b8ee9cf516085b10f221caf86e0850fdccaf8c85
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352870"
---
# <a name="reconcile-a-database-diagram-with-a-modified-database-visual-database-tools"></a>Riconciliazione di un diagramma di database con un database modificato (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Il diagramma di database viene salvato per aggiornare il database in modo che corrisponda al diagramma. Tuttavia, se il database è stato aggiornato da altri utenti dopo l'apertura del diagramma, le modifiche apportate potranno avere effetto sul diagramma e viceversa.  
  
Salvando il diagramma, le differenze tra il database e il diagramma verranno riconciliate sovrascrivendo le modifiche apportate da altri utenti, al fine di garantire la corrispondenza tra il database e il diagramma.  
  
### <a name="to-update-a-database-to-match-your-diagram"></a>Per aggiornare un database in modo che corrisponda al diagramma  
  
1.  Salvare il diagramma del database.  
  
    Se si salva il diagramma per la prima volta, specificare un nome per il diagramma nella finestra di dialogo **Salva nuovo diagramma di database** e scegliere **OK**.  
  
2.  Nella finestra di dialogo **Salva** è contenuto l'elenco delle tabelle su cui influirà il salvataggio del diagramma. Scegliere **Sì** per continuare.  
  
3.  Nella finestra di dialogo **Rilevate modifiche al database** vengono elencati gli oggetti modificati che verranno cambiati per assicurare la corrispondenza con il diagramma. Scegliere **Sì** per salvare il diagramma e accettare l'elenco di modifiche.  
  
    > [!NOTE]  
    > Se il diagramma contiene tabelle e colonne eliminate dal database, salvando il diagramma verranno create nuovamente nel database solo le corrispondenti definizioni. Con questa operazione non verranno ripristinati i dati contenuti in tali oggetti prima dell'eliminazione.  
  
### <a name="to-update-your-diagram-to-match-a-modified-database"></a>Per aggiornare il diagramma in modo che corrisponda a un database modificato  
  
1.  Chiudere il diagramma senza salvare le modifiche.  
  
2.  Fare clic con il pulsante destro del mouse sul diagramma in Esplora oggetti.  
  
3.  Scegliere **Aggiorna** dal menu di scelta rapida.  
  
4.  Aprire nuovamente il diagramma.  
  
## <a name="see-also"></a>Vedere anche  
[Utilizzare diagrammi di database &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
