---
title: Attivare/disattivare un punto di interruzione
description: Informazioni su come attivare/disattivare un punto di interruzione per evidenziare l'istruzione Transact-SQL associata e per eseguire varie azioni sull'istruzione, ad esempio la modifica.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c477ab89-a1cd-4f2c-aa7c-40525041100f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0de50770b4be70a79ebddffd3cd1a983dc2524ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339356"
---
# <a name="toggle-a-breakpoint"></a>Attivare/disattivare un punto di interruzione

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'azione di definizione di un punto di interruzione in un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] è denominata impostazione/rimozione di un punto di interruzione.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Punti di interruzione

Una volta impostato, il punto di interruzione è rappresentato da un'icona nella barra grigia a sinistra dell'istruzione. L'icona è nota come glifo del punto di interruzione. [!INCLUDE[tsql](../../includes/tsql-md.md)] I punti di interruzione vengono applicati a un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] completa. Quando un punto di interruzione viene attivato, il debugger evidenzia l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] associata.  
  
 Se sono presenti più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] su una riga, è possibile impostare/rimuovere un punto di interruzione per ogni istruzione. Facendo clic nella barra grigia a sinistra della finestra, è possibile impostare/rimuovere un punto di interruzione nella prima istruzione della riga. Per impostare/rimuovere un punto di interruzione in un'istruzione successiva, è possibile evidenziare qualsiasi parte dell'istruzione o spostare il cursore all'interno dell'istruzione e quindi premere F9 o scegliere **Imposta/Rimuovi punto di interruzione** dal menu **Debug** . Se sono presenti più punti di interruzione su una riga, nella barra grigia a sinistra sarà presente il glifo di un solo punto di interruzione.  
  
 Dopo aver attivato o disattivato un punto di interruzione, è possibile eseguirvi diverse operazioni, ad esempio modificarne le proprietà oppure disabilitarlo temporaneamente. Per altre informazioni, vedere [Punti di interruzione Transact-SQL](./transact-sql-breakpoints.md).  
  
## <a name="toggle-a-breakpoint"></a>Attivare/disattivare un punto di interruzione  
 **Per attivare o disattivare un punto di interruzione in un'istruzione Transact-SQL**  
  
1.  Fare clic sulla barra grigia a sinistra dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
2.  In alternativa, evidenziare qualsiasi parte dell'istruzione o spostare il cursore all'interno dell'istruzione, quindi effettuare una delle azioni seguenti:  
  
    -   Premere F9.  
  
    -   Scegliere **Imposta/Rimuovi punto di interruzione** dal menu **Debug**.  
  
