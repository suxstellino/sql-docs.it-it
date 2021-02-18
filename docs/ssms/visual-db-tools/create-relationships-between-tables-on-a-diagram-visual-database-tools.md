---
description: Creazione di relazioni tra tabelle in un diagramma (Visual Database Tools)
title: Creare relazioni tra tabelle in un diagramma
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- diagrams [SQL Server], designing
ms.assetid: 28e9630c-dff4-46cc-bb0e-fe77998b6ac2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 5dd6604f4fb82a502402d74cb15c0a0744f82e0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350459"
---
# <a name="create-relationships-between-tables-on-a-diagram-visual-database-tools"></a>Creazione di relazioni tra tabelle in un diagramma (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È possibile creare relazioni tra le colonne in diverse tabelle in Progettazione diagrammi di database trascinando le colonne all'interno delle tabelle.  
  
### <a name="to-create-a-relationship-graphically"></a>Per creare una relazione graficamente  
  
1.  In Progettazione database fare clic sul selettore delle righe per una o più colonne di database che si desidera correlare a una colonna in un'altra tabella.  
  
2.  Trascinare la colonna o le colonne selezionate nella tabella correlata.  
  
3.  Verranno visualizzate due finestre di dialogo: **Relazione chiavi esterne** e **Tabelle e colonne**, quest'ultima visualizzata in primo piano.  
  
4.  Il **nome relazione** è assegnato dal sistema nel formato FK_ *localtable*\_*foreigntable*. È possibile modificare questo valore.  
  
5.  Verificare che in **Tabella di chiave primaria** sia specificata la tabella corretta.  
  
6.  Nella griglia sono indicate le colonne locali e le colonne esterne corrispondenti. È possibile aggiungere o rimuovere le colonne di tabella o modificare i mapping.  
  
7.  Scegliere **OK**.  
  
    Verrà visualizzata la finestra di dialogo **Relazione chiavi esterne** . **Relazione selezionata** visualizza la relazione creata.  
  
8.  Modificare le proprietà della relazione nella griglia.  
  
9. Scegliere **OK** per creare la relazione.  
  
    Progettazione database mostra una relazione tra le colonne selezionate.  
  
## <a name="see-also"></a>Vedere anche  
[Finestra di dialogo Tabelle e colonne &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/tables-and-columns-dialog-box-visual-database-tools.md)  
[Utilizzo dei vincoli](../../relational-databases/tables/unique-constraints-and-check-constraints.md)  
[Utilizzo di tabelle in diagrammi di database &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-tables-in-database-diagram-visual-database-tools.md)  
