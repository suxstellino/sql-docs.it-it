---
title: Usare sparklyr da RStudio
titleSuffix: SQL Server big data clusters
description: Informazioni su come usare sparklyr in un cluster Big data di SQL Server per connettersi a Spark tramite l'interfaccia R.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 8a15336a22f564dd22870de8843732cbb583d2f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043536"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>Usare sparklyr in un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Sparklyr offre un'interfaccia R per Apache Spark e costituisce quindi lo strumento più usato dagli sviluppatori R per usare Spark. Questo articolo descrive come usare sparklyr in un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] tramite RStudio.

## <a name="prerequisites"></a>Prerequisites

- [Distribuire un cluster Big Data di SQL Server 2019](quickstart-big-data-cluster-deploy.md).

### <a name="install-rstudio-desktop"></a>Installare RStudio Desktop

Installare e configurare **RStudio Desktop** seguendo questa procedura:

1. Se è in esecuzione un client Windows, [scaricare e installare R 3.4.4](https://cran.rstudio.com/bin/windows/base/old/3.4.4).

1. [Scaricare e installare RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

1. Al termine dell'installazione, eseguire i comandi seguenti all'interno di RStudio Desktop per installare i pacchetti necessari:

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>Connettersi a Spark in un cluster Big Data

È possibile usare sparklyr per connettere un client al cluster Big Data usando Livio e il gateway HDFS/Spark. 

In RStudio creare uno script R e connettersi a Spark come nell'esempio seguente:

> [!TIP]
> Per i valori `<AZDATA_USERNAME>` e `<AZDATA_PASSWORD>`, usare il nome utente e la password impostati durante la distribuzione del cluster Big Data.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Per i valori `<IP>` e `<PORT>`, vedere la documentazione relativa alla [connessione a un cluster Big Data](connect-to-big-data-cluster.md).

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>Eseguire query sparklyr

Dopo aver effettuato la connessione a Spark, è possibile eseguire sparklyr. Nell'esempio seguente viene eseguita una query sul set di dati Iris usando sparklyr:

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>Calcoli R distribuiti

Sparklyr offre la possibilità di [distribuire calcoli R](https://spark.rstudio.com/guides/distributed-r/) tramite [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object).

I cluster Big Data usano connessioni Livio ed è quindi necessario impostare `packages = FALSE` nella chiamata a **spark_apply**. Per altre informazioni, vedere la [sezione su Livy](https://spark.rstudio.com/guides/distributed-r/#livy) della documentazione di sparklyr relativa ai calcoli R distribuiti. Con questa impostazione, nel codice R passato a **spark_apply** è possibile usare solo i pacchetti R già installati nel cluster Spark. Nell'esempio seguente viene illustrata questa funzionalità:

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
