---
title: Rinominare l'istanza del cluster di failover
description: Questo articolo descrive come rinominare un'istanza di SQL Server che fa parte di un cluster di failover, un'operazione diversa dalla ridenominazione di un'istanza autonoma.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: a407ab5bc7f01a91a1b20a9627a61441e62ddcc9
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642829"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>Ridenominare un'istanza del cluster di failover di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Se un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fa parte di un cluster di failover, il processo di ridenominazione del server virtuale è diverso da quello previsto per un'istanza autonoma. Per altre informazioni, vedere [Rinominare un computer che ospita un'istanza autonoma di SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md).  
  
 Il nome del server virtuale è sempre uguale al nome di rete SQL, ovvero al nome di rete del server virtuale SQL. È possibile modificare il nome del server virtuale, ma non è possibile modificare il nome dell'istanza. Ad esempio, è possibile modificare il nome di un server virtuale denominato VS1\instance1 in un altro nome quale SQL35\instance1, ma la parte relativa al nome dell'istanza, in questo caso instance1, rimarrà invariata.  
  
 Prima di iniziare il processo di ridenominazione, tenere presenti le considerazioni seguenti.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non supporta la ridenominazione di server coinvolti nella replica, eccetto nel caso in cui venga utilizzata il log shipping con la replica. È possibile rinominare il server secondario nel log shipping in caso di perdita definitiva del server primario. Per altre informazioni, vedere [Log shipping e replica &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Se si rinomina un server virtuale configurato per l'utilizzo del mirroring del database, è necessario disabilitare il mirroring del database prima di eseguire la ridenominazione e quindi riabilitarlo con il nome del nuovo server virtuale. I metadati per il mirroring del database non verranno aggiornati automaticamente in modo da riflettere il nome del nuovo server virtuale.  
  
### <a name="to-rename-a-virtual-server"></a>Per rinominare un server virtuale  
  
1.  In Amministrazione cluster, impostare il nuovo nome come nome di rete SQL.  
  
2.  Portare la risorsa del nome di rete offline. In questo modo, la risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e anche le altre risorse dipendenti saranno offline.  
  
3.  Portare di nuovo online la risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="verify-the-renaming-operation"></a>Verifica dell'operazione di ridenominazione  
 Dopo avere rinominato un server virtuale, tutte le connessioni che utilizzavano il nome precedente dovranno utilizzare il nuovo nome per la connessione.  
  
 Per verificare che la ridenominazione sia stata completata, selezionare informazioni da **@@servername** o **sys.servers**. La funzione **@@servername** restituirà il nome del nuovo server virtuale, che verrà visualizzato nella tabella **sys.servers**. Per verificare che il processo di failover funzioni correttamente con il nome del nuovo server virtuale, tentare di eseguire il failover della risorsa di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] negli altri nodi.  
  
 Per le connessioni da qualsiasi nodo del cluster, è possibile utilizzare quasi immediatamente il nuovo nome. Per le connessioni che utilizzano il nuovo nome da un computer client, non è tuttavia possibile connettersi al server fino a quando il nuovo nome non è visibile nel computer client. La quantità di tempo necessaria per la propagazione del nuovo nome nella rete può variare da pochi secondi ad alcuni minuti, a seconda della configurazione della rete. Il tempo necessario prima che il nome precedente non sia più visibile nella rete potrebbe essere più lungo.  
  
 Per ridurre al minimo il ritardo nella propagazione di un'operazione di ridenominazione all'interno della rete, eseguire la procedura seguente:  
  
#### <a name="to-minimize-network-propagation-delay"></a>Per ridurre al minimo il ritardo nella propagazione  
  
1.  Eseguire i comandi seguenti a un prompt dei comandi nel nodo server:  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>Ulteriori considerazioni dopo la ridenominazione dell'operazione  
 Dopo la ridenominazione del nome di rete del cluster di failover, è necessario verificare e applicare le istruzioni riportate di seguito per abilitare tutti gli scenari in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent e [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 **Servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent:** verificare ed eseguire le azioni aggiuntive riportate di seguito per il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent:  
  
-   Correggere le impostazioni del Registro di sistema se SQL Agent è configurato per l'inoltro eventi. Per altre informazioni, vedere [Designazione di un server di inoltro eventi &#40;SQL Server Management Studio&#41;](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md).  
  
-   Correggere i nomi delle istanze del server master (MSX) e dei server di destinazione (TSX) quando si ridenominano computer o nomi di rete cluster. Per altre informazioni, vedere gli argomenti seguenti:  
  
    -   [Escludere più server di destinazione da un server master](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [Creazione di un ambiente multiserver](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   Riconfigurare il log shipping in modo da utilizzare il nome aggiornato del server nei log di backup e ripristino. Per altre informazioni, vedere gli argomenti seguenti:  
  
    -   [Configurare il log shipping &#40;SQL Server&#41;](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [Rimuovere il log shipping &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   Aggiornare i passaggi del processo che dipendono dal nome del server. Per altre informazioni, vedere [Gestire passaggi di processo](../../../ssms/agent/manage-job-steps.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Rinominare un computer che ospita un'istanza autonoma di SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
