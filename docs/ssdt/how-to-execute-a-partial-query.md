---
title: Eseguire una query parziale
description: Informazioni su come eseguire il debug di una sezione di una query complessa. Usare l'editor Transact-SQL per evidenziare un segmento di script specifico ed eseguirlo come singola query.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: af04ab37-6cbb-4185-9382-e5922fa5b1df
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 25ad063fc90539647a48bac2702e0cd2b80cd53b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018201"
---
# <a name="how-to-execute-a-partial-query"></a>Procedura: Eseguire una query parziale

L'Editor Transact\-SQL consente di evidenziare un segmento specifico dello script ed eseguirlo come una singola query. In questo modo è più semplice eseguire il debug di sezioni di query complesse.  
  
> [!WARNING]  
> Nella procedura seguente vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
## <a name="to-partially-execute-a-query"></a>Per eseguire parzialmente una query  
  
1. In **Esplora oggetti di SQL Server** fare doppio clic su **PerishableFruits** in **Viste** per aprirlo nell'Editor Transact\-SQL.  
  
2. Evidenziare il segmento `SELECT p.Id, p.Name FROM dbo.Product p` nel codice, fare clic con il pulsante destro del mouse e scegliere **Esegui query**.  
  
3. Si noti che tutte le righe con i campi specificati nella tabella `Products` vengono restituite nel riquadro dei **risultati**.