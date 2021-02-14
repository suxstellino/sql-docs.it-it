---
title: Istanza server principale (Configurazione guidata sicurezza mirroring del database)
description: Descrizione della pagina "Istanza server principale" della "Configurazione guidata sicurezza mirroring del database" in SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.principalsrvr.f1
ms.assetid: 58af27d7-c5dd-4669-be6b-b472bc2c8ef4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 258014c74dac92e85d3efa5d4078f5bc3b1bbe47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337880"
---
# <a name="principal-server-instance-configure-database-mirroring-security-wizard"></a>Istanza server principale (Configurazione guidata sicurezza mirroring del database)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa pagina per specificare le informazioni relative all'istanza del server del database principale. Il database principale è la copia del database che avvia la sessione di mirroring. Dopo l'avvio della sessione il database principale è la copia del database che accetta le modifiche dell'utente. Quando si verifica un failover, i ruoli principale e mirror vengono invertiti, quindi il database principale iniziale potrebbe non rimanere tale.  
  
 **Per configurare il mirroring del database tramite SQL Server Management Studio**  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opzioni  
 **Istanza server principale**  
 Poiché il mirroring del database in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] viene sempre configurato dal server principale, l'istanza corrente del server è sempre l'istanza del server principale.  
  
 **Listener Port** (Porta listener)  
 Il funzionamento di questa opzione dipende dalla presenza dell'endpoint di mirroring per l'istanza del server corrente, come illustrato di seguito:  
  
-   Se non esiste una porta di attesa per l'istanza del server, nella casella di testo **Porta** verrà visualizzato il numero di porta 5022. È possibile utilizzare qualsiasi numero di porta disponibile, ad esempio 7022.  
  
-   Se l'endpoint di mirroring esiste già, viene visualizzato il numero di porta ricevuto dall'endpoint. Se è necessario modificare la porta, utilizzare un comando ALTER ENDPOINT. Per altre informazioni, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
> [!NOTE]  
>  È necessario un numero di porta.  
  
 **Nome endpoint**  
 Se l'endpoint di mirroring esiste per l'istanza del server, viene visualizzato il nome dell'endpoint. Se l'endpoint non esiste, è possibile specificare il nome dell'endpoint.  
  
 **Crittografa i dati inviati tramite questo endpoint**  
 La crittografia è abilitata per impostazione predefinita. Quando è abilitata, la crittografia non solo è supportata, ma è anche necessaria e utilizza i valori predefiniti per tutte le opzioni di crittografia. Per altre informazioni, vedere [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Per disabilitare la crittografia, deselezionare la casella di controllo. Per abilitare di nuovo la crittografia, selezionare la casella di controllo.  
  
## <a name="see-also"></a>Vedere anche  
 [Endpoint del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Proprietà del database &#40;Pagina Mirroring&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Creare un endpoint del mirroring del database per l'autenticazione Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
