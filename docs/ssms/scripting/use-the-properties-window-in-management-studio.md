---
title: Utilizzo della finestra Proprietà in Management Studio
description: Informazioni su come usare la finestra Proprietà per visualizzare informazioni su un elemento di SQL Server Management Studio, ad esempio una connessione, e sugli oggetti di database.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing properties
- Properties window [SQL Server Management Studio]
- complex properties [SQL Server Management Studio]
ms.assetid: 903d4aca-f57c-43d9-a893-702eceaa7004
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2bd8af54c0178ac13f27dcb3ae23fc8e72b712a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346083"
---
# <a name="use-the-properties-window-in-management-studio"></a>Utilizzo della finestra Proprietà in Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Nella finestra Proprietà è indicato lo stato di un elemento in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ad esempio una connessione o un operatore Showplan, e vengono riportate informazioni su oggetti di database come ad esempio tabelle, visualizzazioni e finestre di progettazione.  
  
 È possibile utilizzare la finestra Proprietà per visualizzare le proprietà della connessione corrente. In questa finestra molte proprietà solo di sola lettura ma è possibile modificarle altrove in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La proprietà Database di una query, ad esempio, è di sola lettura nella finestra Proprietà ma può essere modificata sulla barra degli strumenti.  
  
### <a name="to-view-properties-using-the-properties-window"></a>Per visualizzare le proprietà utilizzando la finestra Proprietà  
  
1.  Se la finestra Proprietà non è visibile, scegliere **Finestra Proprietà** dal menu **Visualizza** oppure premere F4.  
  
2.  Impostare lo stato attivo sull'oggetto che si desidera visualizzare.  
  
3.  Cercare una proprietà specifica nella finestra Proprietà.  
  
### <a name="to-view-connection-properties-of-a-query-window"></a>Per visualizzare le proprietà di connessione di una finestra di query  
  
1.  Se la finestra Proprietà non è visibile, scegliere **Finestra Proprietà** dal menu **Visualizza** oppure premere F4.  
  
2.  Nella finestra Proprietà è possibile vedere tutte le proprietà di connessione.  
  
### <a name="to-view-the-properties-of-a-showplan-operator"></a>Per visualizzare le proprietà di un operatore Showplan  
  
1.  Scegliere **Includi piano di esecuzione effettivo** dal menu **Query**.  
  
2.  Nell'editor di query SQL digitare ed eseguire una query.  
  
3.  Se la finestra Proprietà non è visibile, scegliere **Finestra Proprietà** dal menu **Visualizza** oppure premere F4.  
  
4.  Nella scheda **Piano di esecuzione** dell'editor di query SQL fare clic sulle icone degli operatori per visualizzare informazioni sugli operatori nella finestra Proprietà.  
  
## <a name="see-also"></a>Vedere anche  
 [Finestra Proprietà &#40;Management Studio&#41;](../properties-window-management-studio.md)  
  
