---
title: Distribuire un cluster kubeadm a nodo singolo
titleSuffix: SQL Server Big Data Clusters
description: Usare uno script di distribuzione Bash per distribuire un cluster Big Data di SQL Server 2019 in un cluster kubeadm a nodo singolo.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 63e1e3776c62033280287e55c288791901a2d15f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046934"
---
# <a name="deploy-with-a-bash-script-to-a-single-node-kubeadm-cluster"></a>Eseguire la distribuzione con uno script Bash in un cluster kubeadm a nodo singolo

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In questa esercitazione si userà uno script di distribuzione Bash di esempio per distribuire un cluster Kubernetes a nodo singolo usando kubeadm e un cluster Big Data di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- Una macchina fisica o virtuale **server** Vanilla Ubuntu 18.04 o 16.04. Tutte le dipendenze sono configurate dallo script e lo script viene eseguito dalla macchina virtuale.

  > [!NOTE]
  > L'uso di macchine virtuali Linux di Azure non è ancora supportato.

- La macchina virtuale deve avere almeno 8 CPU, 64 GB di RAM e 100 GB di spazio su disco. Dopo aver eseguito il pull di tutte le immagini Docker del cluster Big Data, rimarranno 50 GB per i dati e i log da distribuire su tutti i componenti.

- Usare i comandi seguenti per aggiornare i pacchetti esistenti e garantire così che l'immagine del sistema operativo sia aggiornata.

   ``` bash
   sudo apt update && sudo apt upgrade -y
   sudo systemctl reboot
   ```

## <a name="recommended-virtual-machine-settings"></a>Impostazioni della macchina virtuale consigliate

1. Usare la configurazione di memoria statica per la macchina virtuale. Nelle installazioni di Hyper-V, ad esempio, non usare l'allocazione dinamica della memoria, ma allocare invece i 64 GB (o superiori) consigliati.

1. Usare la funzionalità checkpoint o snapshot nell'hypervisor per poter eseguire il rollback della macchina virtuale a uno stato pulito.


## <a name="instructions-to-deploy-sql-server-big-data-cluster"></a>Istruzioni per distribuire un cluster Big Data di SQL Server

1. Scaricare lo script nella macchina virtuale che si prevede di usare per la distribuzione.

   ```bash
   curl --output setup-bdc.sh https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/setup-bdc.sh
   ```

2. Rendere lo script eseguibile con il comando seguente.

   ```bash
   chmod +x setup-bdc.sh
   ```

3. Eseguire lo script (assicurarsi che sia in esecuzione con privilegi *sudo*)

   ```bash
   sudo ./setup-bdc.sh
   ```

   Quando richiesto, specificare la password da usare per gli endpoint esterni seguenti: controller, istanza master di SQL Server e gateway. La password deve essere sufficientemente complessa rispetto alle regole esistenti per le password di SQL Server. Per impostazione predefinita, il nome utente del controller è *admin*.

4. Configurare un alias per lo strumento **azdata**.

   ```bash
   source ~/.bashrc
   ```

5. Aggiornare l'installazione degli alias per azdata.

   ```bash
   azdata --version
   ```

## <a name="cleanup"></a>Pulizia

Lo script [cleanup-bdc.sh](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/cleanup-bdc.sh) viene fornito per consentire di reimpostare rapidamente l'ambiente, se necessario. È consigliabile tuttavia usare una macchina virtuale per le attività di test e la funzionalità snapshot nell'hypervisor per eseguire il rollback della macchina virtuale a uno stato pulito.

## <a name="next-steps"></a>Passaggi successivi

Per informazioni introduttive sull'uso dei cluster Big Data, vedere [Esercitazione: Caricare dati di esempio in un cluster Big Data di SQL Server](tutorial-load-sample-data.md).
