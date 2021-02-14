---
title: 'Configurazione guidata sicurezza: Istanza server mirror'
description: Descrive la pagina "Istanza server mirror" della "Configurazione guidata sicurezza mirroring del database" in SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.mirrorsrvr.f1
ms.assetid: 53223432-615e-440f-904d-925d33ec2144
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 783e7eabf5af38b22c4438e92909a5f70f8c4a4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343315"
---
# <a name="configure-database-mirrroing-security-wizard-mirror-server-instance"></a>Configurazione guidata sicurezza mirroring del database: Istanza server mirror
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa pagina per specificare le informazioni relative all'istanza del server del database mirror.  
  
> [!IMPORTANT]  
>  L'istanza del server mirror deve essere in esecuzione nella stessa edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Standard o Enterprise, come l'istanza del server principale. È inoltre consigliabile che vengano eseguite in sistemi simili in grado di gestire carichi di lavoro identici.  
  
 **Per configurare il mirroring del database tramite SQL Server Management Studio**  
  
-   [Stabilire una sessione di mirroring del database tramite autenticazione di Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opzioni  
 **Istanza server mirror**  
 Se nella pagina **Mirroring** della finestra di dialogo **Proprietà database** è già stata specificata un'istanza del server mirror, tale istanza viene visualizzata. Per altre informazioni, vedere [Proprietà del database &#40;Pagina Mirroring&#41;](../../relational-databases/databases/database-properties-mirroring-page.md).  
  
 In caso contrario, immettere il nome dell'istanza del server mirror. Si noti che l'istanza del server mirror non può essere la stessa istanza del server principale.  
  
 **Connettere**  
 Se non è stata specificata un'istanza del server mirror, fare clic su **Connetti**. Verrà visualizzata la finestra di dialogo **Connetti al server** tramite la quale è possibile specificare l'istanza del server e stabilire una connessione.  
  
 Se è stata specificata l'istanza, ma non è disponibile una connessione con autorizzazioni sufficienti per il controllo dell'esistenza dell'endpoint, fare clic su **Connetti**. Verrà visualizzata la finestra di dialogo **Connetti al server** in cui l'istanza del server risulterà selezionata automaticamente e non sarà possibile modificarla. Specificare un account di dominio dotato di autorizzazioni sufficienti e connettersi all'istanza del server.  
  
> [!NOTE]  
>  Quando viene stabilita la connessione all'istanza del server, la Configurazione guidata sicurezza mirroring del database usa le credenziali della finestra di dialogo **Connetti al server** . Queste credenziali sono diverse da quelle di una sessione di mirroring, che utilizza le credenziali dell'account di avvio con il quale l'istanza del server è in esecuzione come servizio.  
  
 **Listener Port** (Porta listener)  
 Il funzionamento di questa opzione dipende dalla presenza dell'endpoint di mirroring per l'istanza del server corrente, come illustrato di seguito:  
  
-   Se non esiste una porta di attesa per l'istanza del server, nella casella di testo **Porta** verrà visualizzato il numero di porta 5022. È possibile utilizzare qualsiasi numero di porta disponibile, ad esempio 7022.  
  
-   Se esiste già l'endpoint del mirroring, verrà visualizzato il numero di porta dell'endpoint. Se è necessario modificare la porta, utilizzare un comando ALTER ENDPOINT. Per altre informazioni, vedere [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
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
  
  
