---
title: Gestione dei certificati (Gestione configurazione SQL Server)
description: Informazioni su come installare i certificati in diverse configurazioni di SQL Server. Gli esempi includono istanze singole, cluster di failover e gruppi di disponibilità Always On.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: d4ad86b8ce46e39f0143f72badca4926bc0d50b2
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978625"
---
# <a name="certificate-management-sql-server-configuration-manager"></a>Gestione dei certificati (Gestione configurazione SQL Server)

[!INCLUDE [sql-windows-only](../../includes/applies-to-version/sql-windows-only.md)]

Questo argomento descrive come distribuire e gestire i certificati in una topologia con gruppi di disponibilità o cluster di failover Always On di SQL Server.

I certificati SSL/TLS sono molto usati per proteggere l'accesso a SQL Server. Con le versioni precedenti di SQL Server le organizzazioni con grandi quantità di SQL Server dovevano compiere sforzi considerevoli per gestire la propria infrastruttura di certificati per SQL Server, spesso sviluppando script ed eseguendo comandi manuali. In SQL Server 2019 la gestione dei certificati è integrata in Gestione configurazione SQL Server e questo semplifica varie attività comuni, ad esempio: 

* Visualizzazione e convalida di certificati installati in un'istanza di SQL Server. 
* Identificazione dei certificati prossimi alla scadenza. 
* Distribuzione dei certificati ai computer appartenenti a gruppi di disponibilità Always On dal nodo che contiene la replica primaria. 
* Distribuzione dei certificati ai computer appartenenti a un'istanza del cluster di failover Always On dal nodo attivo.

> [!NOTE]
> È possibile usare la gestione dei certificati in Gestione configurazione SQL Server con versioni precedenti di SQL Server, a partire da SQL Server 2008.

##  <a name="to-install-a-certificate-for-a-single-sql-server-instance"></a><a name="provision-single-server-cert"></a> Per installare un certificato per una singola istanza di SQL Server  

::: moniker range=">=sql-server-ver15"
1. Nel riquadro della console di Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server**.  

2. Fare clic con il pulsante destro del mouse su **Protocolli per** *&lt;nome istanza&gt;* e quindi scegliere **Proprietà**.  

3. Scegliere la scheda **Certificato** e quindi selezionare **Importa**.  

4. Selezionare **Sfoglia** e quindi selezionare il file del certificato.  

5. Selezionare **Avanti** per convalidare il certificato. Se non ci sono errori, selezionare **Avanti** per importare il certificato nell'istanza locale.  
::: moniker-end

::: moniker range="<= sql-server-2017"
1. Nel riquadro della console di Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server**.  

2. Fare clic con il pulsante destro del mouse su **Protocolli per** *&lt;nome istanza&gt;* e quindi scegliere **Proprietà**.  

3. Selezionare un certificato dal menu a discesa **Certificato** e quindi selezionare **Applica**.  

4. Selezionare **OK**. 
::: moniker-end

##  <a name="to-install-a-certificate-in-a-failover-cluster-instance-configuration"></a><a name="provision-failover-cluster-cert"></a> Per installare un certificato in una configurazione di un'istanza del cluster di failover  
  
1. Nel riquadro della console di Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server**.
  
2. Fare clic con il pulsante destro del mouse su **Protocolli per** *&lt;nome istanza&gt;* e quindi scegliere **Proprietà**. 

3. Scegliere la scheda **Certificato** e quindi selezionare **Importa**.

4. Selezionare il tipo di certificato e indicare se l'importazione riguarda solo il nodo corrente oppure ogni singolo nodo del cluster.

5. Se l'installazione riguarda un nodo singolo, scegliere **Sfoglia** e selezionare il file del certificato. Andare al passaggio 8.

6. Se l'installazione del certificato riguarda ogni nodo, selezionare **Avanti** per visualizzare l'elenco dei nodi dei possibili proprietari. I proprietari possibili per l'istanza del cluster di failover corrente sono preselezionati.

7. Scegliere **Avanti** per selezionare il certificato da importare.

8. Immettere la password quando richiesto. Cercare eventuali avvisi o errori dopo la convalida.

9. Selezionare **Avanti** per importare i certificati selezionati.

> [!NOTE]
> Completare questi passaggi nel nodo attivo dell'istanza del cluster di failover Always On. L'utente deve avere autorizzazioni amministratore per tutti i nodi del cluster.

##  <a name="to-install-a-certificate-in-an-always-on-availability-group-configuration"></a><a name="provision-availability-group-cert"></a>Per installare un certificato in una configurazione del gruppo di disponibilità Always On  
  
1. Nel riquadro della console di Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server**.
  
2. Fare clic con il pulsante destro del mouse su **Protocolli per** *&lt;nome istanza&gt;* e quindi scegliere **Proprietà**.  
  
3. Scegliere la scheda **Certificato** e quindi selezionare **Importa**.  
  
4. Scegliere il tipo di certificato e **Avanti** per selezionare dall'elenco dei gruppi di disponibilità noti.  

5. Selezionare **Avanti** per scegliere i certificati per ogni nodo di replica. Il nome di file dei certificati deve corrispondere al nome NetBIOS dei nodi.

6. Selezionare **Avanti** per importare il certificato in ogni nodo.


> [!NOTE]
> Completare questi passaggi dal nodo che contiene la replica primaria del gruppo di disponibilità. L'utente deve avere autorizzazioni amministratore per tutti i nodi del cluster.

