---
title: Configurare gruppi con scalabilità orizzontale PolyBase in Windows | Microsoft Docs
description: Configurare un gruppo con scalabilità orizzontale di PolyBase per creare un cluster di istanze di SQL Server. In questo modo è possibile migliorare le prestazioni delle query per set di dati di grandi dimensioni da origini esterne.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: tutorial
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 99129e6b52be36b1eb729bfda6056ec1010e7198
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351867"
---
# <a name="configure-polybase-scale-out-groups-on-windows"></a>Configurare gruppi con scalabilità orizzontale PolyBase in Windows

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Questo articolo descrive come configurare un [gruppo con scalabilità orizzontale PolyBase](polybase-scale-out-groups.md) in Windows. Questa funzionalità consente di creare un cluster di istanze di SQL Server per elaborare set di dati di grandi dimensioni da origini dati esterne, ad esempio Hadoop o Archiviazione BLOB di Azure, in una soluzione di scalabilità orizzontale che consente di migliorare le prestazioni delle query.

## <a name="prerequisites"></a>Prerequisiti
  
- Più di un computer nello stesso dominio  
  
- Un account utente di dominio per l'esecuzione dei servizi PolyBase  
  
## <a name="process-overview"></a>Panoramica del processo

I passaggi seguenti riepilogano il processo per la creazione di un gruppo con scalabilità orizzontale PolyBase. La prossima sezione offre una procedura guidata più dettagliata di ogni passaggio.
  
1. Installare la stessa versione di SQL Server con PolyBase su N computer.
  
2. Selezionare un'istanza di SQL Server come nodo head. 
  
3. Aggiungere le istanze rimanenti di SQL Server come nodi di calcolo usando [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

4. Monitorare i nodi nel gruppo usando [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).

5. Facoltativa. Rimuovere un nodo di calcolo usando [sp_polybase_leave_group &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-leave-group.md).

## <a name="example-walk-through"></a>Procedura dettagliata di esempio

Di seguito vengono illustrati i passaggi per configurare un gruppo di PolyBase con:  
  
1. Due macchine nel dominio *PQTH4A* . I nomi computer sono:  
  
   - PQTH4A-CMP01  
  
   - PQTH4A-CMP02  
  
2. Account di dominio: *PQTH4A\PolyBaseUser*  

## <a name="install-sql-server-with-polybase-on-all-machines"></a>Installare SQL Server con PolyBase in tutti i computer

1. Eseguire setup.exe.
  
2. Nella pagina Selezione funzionalità scegliere **Servizio query PolyBase per i dati esterni**.
  
3. Nella pagina Configurazione server usare l'**account di dominio** PQTH4A\PolyBaseUser per il motore di ricerca PolyBase di SQL Server e SQL Server PolyBase Data Movement Service.
  
4. Nella pagina di configurazione di PolyBase selezionare l'opzione **Usare questa istanza di SQL Server come parte del gruppo PolyBase con scalabilità orizzontale**. Verrà aperto il firewall per consentire le connessioni in ingresso ai servizi di PolyBase. Se il nodo head è un'istanza denominata, è necessario aggiungere manualmente la porta di SQL Server a Windows Firewall nel nodo head e anche avviare SQL Browser nel nodo head.
  
5. Al termine dell'installazione, eseguire **services.msc**. Verificare che SQL Server, il motore PolyBase e PolyBase Data Movement Service siano in esecuzione.
  
   ![Servizi PolyBase](../../relational-databases/polybase/media/polybase-services.png "Servizi PolyBase")  
  
## <a name="select-one-sql-server-as-head-node"></a>Selezionare un'istanza di SQL Server come nodo head  
  
Al termine dell'installazione, entrambi i computer possono fungere da nodi head del gruppo di PolyBase. In questo esempio, si sceglierà "MSSQLSERVER" in PQTH4A-CMP01 come nodo head.
  
## <a name="add-other-sql-server-instances-as-compute-nodes"></a>Aggiungere altre istanze di SQL Server come nodi di calcolo  
  
1. Connettersi a SQL Server su PQTH4A-CMP02.
  
2. Eseguire la stored procedure [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

   ```sql
   -- Enter head node details:
   -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';
   ```  

3. Eseguire services.msc sul nodo di calcolo, PQTH4A-CMP02.
  
4. Arrestare il motore PolyBase e riavviare PolyBase Data Movement Service.

> [!NOTE] 
> Quando il servizio del motore PolyBase viene riavviato o arrestato nel nodo head, i servizi Data Movement Service (DMS) vengono arrestati non appena il canale di comunicazione viene chiuso tra DMS e il servizio motore Polybase (DW). Se il motore DW viene riavviato più di due volte, il servizio DMS passa a un periodo di attesa di 90 minuti e deve attendere 90 minuti per il successivo tentativo di avvio automatico. In questa situazione è necessario avviare manualmente il servizio in tutti i nodi.

## <a name="optional-remove-a-compute-node"></a>Facoltativo: Rimuovere un nodo di calcolo  
  
1. Connettersi al nodo di calcolo PQTH4A-CMP02 per SQL Server.
  
2. Eseguire la stored procedure sp_polybase_leave_group.
  
    ```sql  
    EXEC sp_polybase_leave_group;  
    ```  
  
3. Eseguire services.msc sul nodo di calcolo da rimuovere, PQTH4A-CMP02.
  
4. Avviare il motore PolyBase. Riavviare PolyBase Data Movement Service.
  
5. Verificare che il nodo sia stato rimosso eseguendo la DMV sys.dm_exec_compute_nodes su PQTH4A-CMP01. A questo punto, PQTH4A-CMP02 funzionerà come un nodo head autonomo  
  
## <a name="next-steps"></a>Passaggi successivi  

Per la risoluzione dei problemi, vedere [PolyBase troubleshooting with dynamic management views](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130)).
  
Per altre informazioni su PolyBase, vedere la [panoramica di PolyBase](../../relational-databases/polybase/polybase-guide.md).