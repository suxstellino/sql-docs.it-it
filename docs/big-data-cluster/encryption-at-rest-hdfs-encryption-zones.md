---
title: Guida all'uso delle zone di crittografia HDFS in cluster Big Data di SQL Server
titleSuffix: SQL Server big data clusters
description: Questo articolo illustra come usare la funzionalità delle zone di crittografia HDFS di cluster Big Data di SQL Server
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bfcc348595a645eca3b1150ae39e7c777f4e192a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046761"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guida all'uso delle zone di crittografia HDFS in cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa guida illustra come usare le funzionalità di crittografia dei dati inattivi di cluster Big Data di SQL Server per crittografare le cartelle HDFS usando le zone di crittografia.

Si noti che esiste già una zona di crittografia predefinita montata in __```/securelake```__ che può essere usata immediatamente. È stata creata con una chiave a 256 bit generata dal sistema denominata __securelakekey__. Questa chiave può essere usata per creare zone di crittografia aggiuntive.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Cluster Big Data di SQL Server CU8+](release-notes-big-data-cluster.md) e integrazione di Active Directory.
- Utente con privilegi amministrativi.

## <a name="login-into-the-name-node"></a>Accedere al nodo nomi

Seguire le [istruzioni di connessione ad Active Directory ](active-directory-connect.md) per eseguire l'accesso al cluster. Accedere al namenode (nmnode-0-0) per eseguire i comandi per chiave e zone di crittografia.

   ```console
   kubectl exec -it -c hadoop -n <cluster_namespace> nmnode-0-0 -- /bin/bash
   ```

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Creare una zona di crittografia usando la chiave gestita e fornita dal sistema

1. Creare una cartella HDFS

   ```console
   hdfs dfs -mkdir -p /user/zone/folder
   ```

1. Eseguire il comando di creazione della zona di crittografia per crittografare la cartella usando la chiave __securelakekey__.

   ```console
   hdfs crypto -createZone -keyName securelakekey -path /user/zone/folder
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Creare una chiave e una zona di crittografia nuove

1. Usare il modello seguente per creare una chiave a 256 bit.

   ```console
   kinit hdfs
   hadoop key create mydatalakekey -size 256
   ```

1. Creare e crittografare un nuovo percorso HDFS usando la chiave utente.

   ```console
   hdfs dfs -mkdir -p /user/mydatalake
   hdfs crypto -createZone -keyName mydatalakekey -path /user/mydatalake
   ```
