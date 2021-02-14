---
title: Connettersi in modalità Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come connettersi a cluster Big Data di SQL Server in un dominio di Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 809a91c59973bee1602f335ed37fbef5931bbfeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039911"
---
# <a name="connect-big-data-clusters-2019-active-directory-mode"></a>Connettersi a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: Modalità Active Directory

Questo articolo descrive come connettersi agli endpoint di un cluster Big Data di SQL Server in modalità Active Directory. Per eseguire le attività descritte in questo articolo è necessario disporre di un cluster Big Data di SQL Server distribuito in modalità Active Directory. Se non è disponibile un cluster, vedere [Distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deploy.md).

## <a name="overview"></a>Panoramica

Accedere all'istanza master di SQL Server tramite l'autenticazione di Active Directory.

Per verificare le connessioni di Active Directory all'istanza di SQL Server, connettersi all'istanza master di SQL Server con `sqlcmd`. Gli account di accesso vengono creati automaticamente per i gruppi specificati in fase di distribuzione (`clusterUsers` e `clusterAdmins`).

Se si usa Linux, eseguire prima `kinit` come utente di Active Directory, quindi eseguire `sqlcmd`. Se si usa Windows, è sufficiente accedere come utente desiderato da un **computer client aggiunto al dominio**.

## <a name="connect-to-master-instance-from-linuxmac"></a>Connettersi all'istanza master da Linux/Mac

```bash
kinit <username>@<domain name>
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="connect-to-master-instance-from-windows"></a>Connettersi all'istanza master da Windows

```cmd
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="log-in-to-sql-server-master-instance-using-azure-data-studio-or-ssms"></a>Accedere all'istanza master di SQL Server tramite Azure Data Studio o SSMS

Da un client aggiunto a un dominio è possibile aprire SSMS o Azure Data Studio e connettersi all'istanza master. Si tratta di un'esperienza identica a quella di connessione a qualsiasi istanza di SQL Server tramite l'autenticazione di Active Directory.

Da SSMS:

![Finestra di dialogo per la connessione a SQL Server in SSMS](./media/deploy-active-directory/image23.png)

Da Azure Data Studio:

![Finestra di dialogo di Azure Data Studio per la connessione a SQL Server](./media/deploy-active-directory/image24.png)}

## <a name="log-in-to-controller-with-ad-authentication"></a>Accedere al controller tramite l'autenticazione di Active Directory

### <a name="connect-to-controller-with-ad-authentication-from-linuxmac"></a>Connettersi al controller tramite l'autenticazione di Active Directory da Linux/Mac

Sono disponibili due opzioni per connettersi all'endpoint controller tramite [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] e l'autenticazione di Active Directory. È possibile usare il parametro *--endpoint/-e*:

```bash
kinit <username>@<domain name>
azdata login -e https://<controller DNS name>:30080 --auth ad
```

In alternativa, è possibile connettersi usando il parametro *--namespace/-n*, che corrisponde al nome del cluster Big Data:

```bash
kinit <username>@<domain name>
azdata login -n <clusterName> --auth ad
```

### <a name="connect-to-controller-with-ad-authentication-from-windows"></a>Connettersi al controller tramite l'autenticazione di Active Directory da Windows

```cmd
azdata login -e https://<controller DNS name>:30080 --auth ad
```

## <a name="use-ad-authentication-to-knox-gateway-webhdfs"></a>Usare l'autenticazione di Active Directory per il gateway Knox (webHDFS)

È anche possibile emettere comandi HDFS usando curl attraverso l'endpoint del gateway Knox. A questo scopo, è necessaria l'autenticazione di Active Directory per Knox. Il comando curl seguente effettua una chiamata REST webHDFS tramite il gateway Knox per creare una directory denominata `products`

```bash
curl -k -v --negotiate -u : https://<Gateway DNS name>:30443/gateway/default/webhdfs/v1/products?op=MKDIRS -X PUT
```

## <a name="next-steps"></a>Passaggi successivi

[Risolvere i problemi di integrazione di Active Directory di cluster Big Data di SQL Server](troubleshoot-active-directory.md)

[Concetto: distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in modalità Active Directory](active-directory-deployment-background.md)
