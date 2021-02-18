---
description: Creare o modificare un gruppo di server (SQL Server Management Studio)
title: Creare o modificare un gruppo di server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editgroup.f1
- sql13.swb.newgroup.f1
helpviewer_keywords:
- Registered Servers [SQL Server], server groups
- server groups [SQL Server]
- groups [SQL Server], server
ms.assetid: d4a942bd-2dd1-42db-ad0e-e9a9ae5b856d
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 81212d0e55c322467abcb47ca96770bc1ae0f5ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350521"
---
# <a name="create-or-edit-a-server-group-sql-server-management-studio"></a>Creare o modificare un gruppo di server (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In questo argomento viene illustrato come organizzare i server in Server registrati in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] creando gruppi di server e inserendoli all'interno dei gruppi. È possibile creare gruppi di server in Server registrati in qualsiasi momento oppure quando si esegue la registrazione dei server.  

## <a name="SSMSProcedure"></a>

### <a name="to-create-a-server-group-in-registered-servers"></a>Per creare un gruppo di server in Server registrati  

1. In Server registrati fare clic sul tipo di server sulla barra degli strumenti Server registrati. Se Server registrati non è visibile, scegliere **Server registrati** dal menu **Visualizza** .  

2. Fare clic con il pulsante destro del mouse su un server o su un gruppo di server, scegliere **Nuovo**, quindi **Gruppo di server**.  

3. Nella finestra di dialogo **Nuovo gruppo di server** digitare un nome univoco per il gruppo di server nella casella di riepilogo **Nome gruppo** . Il nome del gruppo di server deve essere univoco per la posizione corrente nell'albero Server registrati.

4. Nella casella di riepilogo **Descrizione gruppo** è possibile specificare facoltativamente un nome descrittivo per il gruppo di server, ad esempio "Server finanziari America Latina".  

5. Nella casella **Selezionare un percorso per il nuovo gruppo di server** fare clic su un percorso, quindi su **Salva**.  

   > [!NOTE]
   >  È anche possibile creare un nuovo gruppo di server nell'ambito della registrazione di un server facendo clic su **Nuovo gruppo** e inserendo le informazioni nella finestra di dialogo **Nuovo gruppo** .  

## <a name="see-also"></a>Vedere anche

[Registrazione di server](./register-servers.md)