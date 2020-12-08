---
title: Indicazioni relative agli indici columnstore - Ottimizzazione guidata motore di database
description: Informazioni su come Ottimizzazione guidata motore di database può analizzare il carico di lavoro e consigliare gli indici rowstore e columnstore per il database in SQL Server.
ms.custom: seo-dt-2019
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, columnstore index
- Database Engine Tuning Advisor, columnstore and rowstore indexes
ms.assetid: 9fba1139-82cb-4244-a41f-4337a7d0c132
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 441b0c37a39efaf02ff00d64b893896cd3fe8f6f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505375"
---
# <a name="columnstore-index-recommendations-in-database-engine-tuning-advisor-dta"></a>Indicazioni relative agli indici columnstore in Ottimizzazione guidata motore di database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 
  I carichi di lavoro di data warehousing e analisi possono trarre vantaggio dagli [indici columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md), nonché dagli indici rowstore tradizionali. La scelta della combinazione di indici rowstore e columnstore da compilare per il database dipende dal carico di lavoro dell'applicazione. In SQL Server 2016, [Ottimizzazione guidata motore di database](../../relational-databases/performance/database-engine-tuning-advisor.md) può analizzare il carico di lavoro e consigliare una combinazione di indici rowstore e columnstore appropriata per il database. 
  
 Questa funzionalità è disponibile con SQL Server Management Studio **16.4** o versione successiva. 
  
## <a name="how-to-enable-columnstore-index-recommendations-in-database-engine-tuning-advisor-gui"></a>Come abilitare le indicazioni relative agli indici columnstore nell'interfaccia utente dell'Ottimizzazione guidata motore di database

  
  1. Avviare Ottimizzazione guidata motore di database e aprire una nuova sessione di ottimizzazione.
  
  2. Selezionare uno o più database e il carico di lavoro per l'ottimizzazione nel riquadro **Generale**.
  
  3. Nel riquadro Opzioni di ottimizzazione selezionare la casella di controllo **Consiglia indici columnstore** (vedere la figura seguente).
  ![Opzioni di ottimizzazione per gli indici columnstore nell'Ottimizzazione guidata motore di database](../../relational-databases/performance/media/dta-columnstore-indexes-tuning-option.gif)
 
  4. Selezionare altre opzioni di ottimizzazione e fare clic sul pulsante **Avvia analisi**.
  
  5. Al termine di ottimizzazione, visualizzare tutti i consigli, inclusi eventuali indici columnstore, nel riquadro **Indicazioni** riquadro (vedere la figura seguente).      
  ![Indicazioni per gli indici columnstore nell'Ottimizzazione guidata motore di database](../../relational-databases/performance/media/dta-columnstore-index-recommendation.gif)
  
  6. Fare clic sul collegamento ipertestuale **Definizione** per visualizzare l'istruzione DDL (Data Definition Language) SQL che consente di creare l'indice consigliato. Per impostazione predefinita, l'Ottimizzazione guidata motore di database usa il suffisso **col** nel nome degli indici columnstore per facilitare l'identificazione degli columnstore (vedere la figura seguente).
  ![Definizione dell'indice nell'Ottimizzazione guidata motore di database](../../relational-databases/performance/media/dta-columnstore-index-definition.gif) 
  
  
  ## <a name="how-to-enable-columnstore-index-recommendations-in-dtaexe-utility"></a>Come abilitare le indicazioni relative agli indici columnstore nell'utilità dta.exe

Per abilitare le indicazioni relative agli indici columnstore quando si usa l'utilità della riga di comando dta.exe, usare il parametro della riga di comando **-fc**.

Per altre informazioni sull'utilità della riga di comando dta.exe, vedere [Utilità dta](../../tools/dta/dta-utility.md).

## <a name="see-also"></a>Vedere anche
[Guida agli indici columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)       
[Ottimizzazione guidata motore di database](../../relational-databases/performance/database-engine-tuning-advisor.md)      
[Esercitazione: Ottimizzazione guidata motore di database](../../tools/dta/tutorial-database-engine-tuning-advisor.md)



  

