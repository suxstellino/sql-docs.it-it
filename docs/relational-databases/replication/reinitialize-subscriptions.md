---
description: Reinizializzare le sottoscrizioni
title: Reinizializzare le sottoscrizioni | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- initializing subscriptions [SQL Server replication], reinitializing
- subscriptions [SQL Server replication], reinitializing
- reinitializing subscriptions
ms.assetid: fb13712b-e7ad-4f1f-b605-4554bad0cb60
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 739d977502331d245b78fcd22776a25590c66bc8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479812"
---
# <a name="reinitialize-subscriptions"></a>Reinizializzare le sottoscrizioni
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La reinizializzazione di una sottoscrizione consiste nell'applicare un nuovo snapshot di uno o più articoli a uno o più Sottoscrittori. Nella replica transazionale e snapshot è possibile reinizializzare singoli articoli, mentre nella replica di tipo merge è necessario reinizializzare tutti gli articoli. In una topologia di replica transazionale peer-to-peer non è possibile reinizializzare i nodi. Per verificare se un nodo dispone di una nuova copia dei dati, ripristinare un backup nel nodo stesso. La reinizializzazione avviene per uno dei due motivi seguenti:  
  
-   Una sottoscrizione viene contrassegnata esplicitamente per la reinizializzazione.  
  
-   Viene eseguita un'azione che richiede la reinizializzazione, ad esempio la modifica di una proprietà. Per altre informazioni sulle azioni che richiedono la reinizializzazione, vedere [Modificare le proprietà di pubblicazioni e articoli](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
 In entrambi i casi, alla successiva esecuzione dell'agente di distribuzione o dell'agente di merge, al Sottoscrittore viene applicato lo snapshot più recente. Nella replica snapshot e transazionale, al momento della reinizializzazione le modifiche apportate nel Sottoscrittore non ancora sincronizzate con il server di pubblicazione vengono sovrascritte dall'applicazione del nuovo snapshot.  
  
 Nella replica di tipo merge è possibile scegliere di caricare dal Sottoscrittore tutte le modifiche apportate ai dati prima di applicare lo snapshot. Tutte le modifiche dello schema in sospeso vengono applicate dal server di pubblicazione al Sottoscrittore e tutti gli aggiornamenti eseguiti nel Sottoscrittore dopo l'ultima sincronizzazione vengono distribuiti al server di pubblicazione prima di applicare nuovamente lo snapshot. Questa funzionalità viene controllata dalle proprietà **upload_first** e **automatic_reinitialization_policy** . Per ulteriori informazioni, vedere [Reinitialize a Subscription](../../relational-databases/replication/reinitialize-a-subscription.md). Se si contrassegna una sottoscrizione per la reinizializzazione tramite SQL Server Management Studio o Monitoraggio replica, nella finestra di dialogo **Reinizializza sottoscrizioni** è possibile scegliere di caricare innanzitutto le modifiche.  
  
> [!IMPORTANT]  
>  Se si aggiunge, elimina o modifica un filtro con parametri in una pubblicazione di tipo merge, non sarà possibile caricare le modifiche in sospeso dal Sottoscrittore al server di pubblicazione durante la reinizializzazione. Per caricare le modifiche in sospeso, sincronizzare tutte le sottoscrizioni prima di modificare il filtro.  
  
 Se durante la creazione della sottoscrizione è stata disattivata l'applicazione dello snapshot iniziale al Sottoscrittore e quindi si contrassegna la sottoscrizione per la reinizializzazione, non verrà applicato uno snapshot. Per altre informazioni, vedere [Inizializzazione di una sottoscrizione transazionale senza uno snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 **Per reinizializzare una sottoscrizione**  
  
 Per reinizializzare tutti gli articoli di una sottoscrizione, utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], stored procedure oppure oggetti RMO (Replication Management Objects). Per reinizializzare singoli articoli di pubblicazioni snapshot e transazionali, è necessario utilizzare stored procedure. Per altre informazioni, vedere [Reinitialize a Subscription](../../relational-databases/replication/reinitialize-a-subscription.md).  
  
## <a name="see-also"></a>Vedi anche  
 [Inizializzare una sottoscrizione](../../relational-databases/replication/initialize-a-subscription.md)   
 [Scadenza e disattivazione delle sottoscrizioni](../../relational-databases/replication/subscription-expiration-and-deactivation.md)  
  
  
