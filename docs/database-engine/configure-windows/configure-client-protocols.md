---
title: Configurazione di protocolli client | Microsoft Docs
description: Informazioni sui diversi modi di configurare i protocolli usati dalle applicazioni client in SQL Server. I protocolli supportati includono TCP/IP, Named Pipes e Shared Memory.
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- default protocols
- network protocols [SQL Server], client configuration
- TCP/IP [SQL Server], client protocols
- disabling client protocols
- ordering protocols [SQL Server]
- protocols [SQL Server], order for client computers
- configure client protocols
- client protocols [SQL Server]
- protocols [SQL Server], client configuration
- default protocols, client
ms.assetid: 3dfa2702-ba65-43b4-a777-6727846e133a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6651a3c0d5985e3fa848c3fd02cc0971cb87db0c
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783304"
---
# <a name="configure-client-protocols"></a>configurazione di protocolli client
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come configurare i protocolli client utilizzati dalle applicazioni client in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta le comunicazioni client con il protocollo di rete TCP/IP e il protocollo Named Pipes. Se il client si connette a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] nello stesso computer, è inoltre disponibile il protocollo Shared Memory. La selezione del protocollo può essere in genere eseguita in tre modi.  
  
-   Configurare tutte le applicazioni client in modo da utilizzare lo stesso protocollo di rete impostando l'ordine dei protocolli in Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Configurare una singola applicazione client in modo da utilizzare un protocollo di rete diverso tramite la creazione di un alias. Per altre informazioni, vedere [Creazione o eliminazione di un alias server per l'utilizzo da parte di un client &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
-   Alcune applicazioni client, ad esempio sqlcmd.exe, possono specificare il protocollo nella stringa di connessione. Per altre informazioni, vedere [Connessione al Motore di database tramite sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md).  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
  
###  <a name="to-enable-or-disable-a-client-protocol"></a><a name="EnableDisable"></a> Per attivare o disabilitare un protocollo client  
  
1.  In Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espandere **Configurazione SQL Server Native Client**, fare clic con il pulsante destro del mouse su **Protocolli client** e quindi scegliere **Proprietà**.  
  
2.  Fare clic su un protocollo nella casella **Protocolli disabilitati** e quindi su **Abilita** per abilitare il protocollo.  
  
3.  Fare clic su un protocollo nella casella **Protocolli abilitati** e quindi su **Disabilita** per disabilitare il protocollo.  
  
###  <a name="to-change-the-default-protocol-or-the-protocol-order-for-client-computers"></a><a name="ChangeDefault"></a> Per cambiare il protocollo predefinito o modificare l'ordine dei protocolli per i computer client  
  
1.  In Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espandere **Configurazione SQL Server Native Client**, fare clic con il pulsante destro del mouse su **Protocolli client** e quindi scegliere **Proprietà**.  
  
2.  Nella casella **Protocolli abilitati** fare clic su **Sposta su** o **Sposta giù** per modificare l'ordine in cui i protocolli vengono usati durante un tentativo di connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il protocollo visualizzato in alto nella casella **Protocolli abilitati** è il protocollo predefinito.  
  
    > [!IMPORTANT]  
    >  Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea voci del Registro di sistema relative alle configurazioni dell'alias server e alla libreria di rete client predefinita. Tuttavia, le applicazioni non installano le librerie di rete client di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o i protocolli di rete. Le librerie di rete client di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono installate durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i protocolli di rete nell'ambito dell'installazione di Windows, oppure accedendo alla sezione **Reti** del **Pannello di controllo**. È possibile che un particolare protocollo di rete non sia disponibile tramite l'installazione di Windows. Per ulteriori informazioni sull'installazione di protocolli di rete specifici, vedere la documentazione del produttore.  
  
###  <a name="to-configure-a-client-to-use-tcpip"></a><a name="Configure"></a> Per configurare un client per l'utilizzo di TCP/IP  
  
1.  In Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espandere **Configurazione SQL Server Native Client**, fare clic con il pulsante destro del mouse su **Protocolli client** e quindi scegliere **Proprietà**.  
  
2.  Nella casella **Protocolli abilitati** fare clic sulle frecce SU e GIÙ per modificare l'ordine di utilizzo dei protocolli durante un tentativo di connessione a SQL Server. Il protocollo visualizzato in alto nella casella **Protocolli abilitati** è il protocollo predefinito.  
  
 Il protocollo Shared Memory viene abilitato separatamente selezionando la casella **Abilita protocollo Shared Memory**.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare l'opzione di configurazione del server remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md)  
  
