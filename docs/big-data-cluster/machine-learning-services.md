---
title: Machine Learning Services (Python, R)
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come eseguire script Python e R nell'istanza master di cluster Big Data di SQL Server con Machine Learning Services.
author: dphansen
ms.author: davidph
ms.date: 11/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: af0611396dcd50fcbea41e96ecc9d651b56a91d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047529"
---
# <a name="run-python-and-r-scripts-with-machine-learning-services-on-sql-server-big-data-clusters"></a>Eseguire script Python e R con Machine Learning Services in cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

È possibile eseguire script Python e R nell'istanza master di [cluster Big Data di SQL Server](big-data-cluster-overview.md) con [Machine Learning Services](../machine-learning/index.yml).

> [!NOTE]
> È anche possibile eseguire codice Java nell'istanza master dei cluster Big Data di SQL Server l'[estensione per il linguaggio Java](../language-extensions/java-overview.md). Con la procedura seguente verranno abilitate anche le [estensioni del linguaggio di SQL Server](../language-extensions/language-extensions-overview.md).

## <a name="enable-machine-learning-services"></a>Abilitare Machine Learning Services

Machine Learning Services viene installato nei cluster Big Data per impostazione predefinita e non richiede un'installazione separata.

Per abilitare Machine Learning Services, eseguire questa istruzione sull'istanza master:

```sql
EXEC sp_configure 'external scripts enabled', 1
RECONFIGURE WITH OVERRIDE
GO
```

È ora possibile eseguire script Python e R nell'istanza master di cluster Big Data. Per eseguire il primo script, vedere le guide di avvio rapido in [Passaggi successivi](#next-steps).

>[!NOTE]
>Non è possibile impostare l'impostazione di configurazione in una connessione del listener del gruppo di disponibilità. Se i cluster Big Data vengono distribuiti con disponibilità elevata, impostare `external scripts enabled` in ogni replica. Vedere [Abilitare in un cluster con disponibilità elevata](#enable-on-cluster-with-high-availability).

## <a name="enable-on-cluster-with-high-availability"></a>Abilitare in un cluster con disponibilità elevata

Quando si [distribuisce un cluster Big Data di SQL Server con disponibilità elevata](deployment-high-availability.md), la distribuzione crea un gruppo di disponibilità per l'istanza master. Per abilitare Machine Learning Services, impostare `external scripts enabled` in ogni istanza del gruppo di disponibilità. Per un cluster Big Data è necessario eseguire `sp_configure` in ogni replica dell'istanza master di SQL Server

Nella sezione seguente viene descritto come abilitare gli script esterni in ogni istanza.

### <a name="create-an-external-load-balancer-for-each-instance"></a>Creare un servizio di bilanciamento del carico esterno per ogni istanza

Per ogni replica nel gruppo di disponibilità, creare un servizio di bilanciamento del carico per consentire la connessione all'istanza. 

`kubectl expose pod <pod-name> --port=<connection port number> --name=<load-balancer-name> --type=LoadBalancer -n <kubernetes namespace>`

Gli esempi in questo articolo usano i valori seguenti:

- `<pod-name>`: `master-#`
- `<connection port number>`: `1533`
- `<load-balancer-name>`: `mymaster-#`
- `<kubernetes namespace>`: `mssql-cluster`

Aggiornare lo script seguente per l'ambiente ed eseguire i comandi:

```bash
kubectl expose pod master-0 --port=1533 --name=mymaster-0 --type=LoadBalancer -n mssql-cluster 
kubectl expose pod master-1 --port=1533 --name=mymaster-1 --type=LoadBalancer -n mssql-cluster
kubectl expose pod master-2 --port=1533 --name=mymaster-2 --type=LoadBalancer -n mssql-cluster 
```

`kubectl` restituisce l'output seguente.

```bash
service/mymaster-0 exposed
service/mymaster-1 exposed
service/mymaster-2 exposed
```

Ogni servizio di bilanciamento del carico è un endpoint della replica master.

### <a name="enable-script-execution-on-each-replica"></a>Abilitare l'esecuzione di script in ogni replica

1. Ottenere l'indirizzo IP per l'endpoint della replica master.

   Il comando seguente restituisce l'indirizzo IP esterno per l'endpoint di replica. 

   `kubectl get services <load-balancer-name> -n <kubernetes namespace>`

   Per ottenere l'indirizzo IP esterno per ogni replica in questo scenario, eseguire i comandi seguenti:

   ```bash
   kubectl get services mymaster-0 -n mssql-cluster
   kubectl get services mymaster-1 -n mssql-cluster
   kubectl get services mymaster-2 -n mssql-cluster
   ```

   >[!NOTE]
   > Potrebbe essere necessario un po' di tempo prima che l'indirizzo IP esterno sia disponibile. Eseguire periodicamente lo script precedente finché ogni endpoint non restituisce un indirizzo IP esterno.

1. Connettersi all'endpoint della replica master e abilitare l'esecuzione dello script.

    Eseguire l'istruzione seguente:

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    GO
    ```

   Ad esempio, è possibile eseguire il comando precedente con `sqlcmd`. Nell'esempio seguente viene eseguita la connessione all'endpoint della replica master e viene abilitata l'esecuzione dello script. Aggiornare i valori nello script in base all'ambiente specifico.

   ```bash
   sqlcmd -S <IP address>,1533 -U <user name> -P <password> -Q "EXEC sp_configure 'external scripts enabled', 1; RECONFIGURE WITH OVERRIDE;"
   ```

   Ripetere il passaggio per ogni replica.

### <a name="demonstration"></a>Dimostrazione

L'immagine seguente illustra questo processo.

[![Demo](media/machine-learning-services/example-kube-enable-scripts.png "Dimostrazione della funzionalità di abilitazione in Kubernetes")](media/machine-learning-services/example-kube-enable-scripts.png#lightbox)

È ora possibile eseguire script Python e R nell'istanza master di cluster Big Data. Per eseguire il primo script, vedere le guide di avvio rapido in [Passaggi successivi](#next-steps).

### <a name="delete-the-master-replica-endpoints"></a>Eliminare gli endpoint della replica master

Nel cluster Kubernetes eliminare l'endpoint per ogni replica. L'endpoint viene esposto in Kubernetes come servizio di bilanciamento del carico.

Il comando seguente elimina il servizio di bilanciamento del carico.

`kubectl delete svc <load-balancer-name> -n mssql-cluster`

Per gli esempi di questo articolo, eseguire i comandi seguenti.

```bash
kubectl delete svc mymaster-0 -n mssql-cluster
kubectl delete svc mymaster-1 -n mssql-cluster
kubectl delete svc mymaster-2 -n mssql-cluster
```

## <a name="next-steps"></a>Passaggi successivi

+ [Eseguire script Python semplici](../machine-learning/tutorials/quickstart-python-create-script.md?toc=/sql/toc.json)
+ [Eseguire il training e assegnare un punteggio a un modello predittivo in Python](../machine-learning/tutorials/quickstart-python-train-score-model.md?toc=/sql/toc.json)
+ [Eseguire script R semplici](../machine-learning/tutorials/quickstart-r-create-script.md?toc=/sql/toc.json)
+ [Eseguire il training e assegnare un punteggio a un modello predittivo in R](../machine-learning/tutorials/quickstart-r-train-score-model.md?toc=/sql/toc.json)
