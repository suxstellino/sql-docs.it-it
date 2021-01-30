---
description: MSSQL_ENG002601
title: MSSQL_ENG002601 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG002601 error
ms.assetid: 657c3ae6-9e4b-4c60-becc-4caf7435c1dc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8e507b7fc3bce13d775ab313dba6cd1af7da68ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198994"
---
# <a name="mssql_eng002601"></a>MSSQL_ENG002601
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|2601|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico|N/D|  
|Testo del messaggio|Non è possibile inserire la riga di chiave duplicata nell'oggetto '%.*ls!' con indice univoco '%.\*ls'.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore generale può essere generato indipendentemente dal fatto che un database venga o meno replicato. Nei database replicati l'errore viene di solito generato in quanto le chiavi primarie non sono state gestite correttamente nella topologia. In un ambiente distribuito è fondamentale garantire che lo stesso valore non venga inserito in una colonna chiave primaria o in qualsiasi altra colonna univoca in più di un nodo. Le cause possibile includono:  
  
-   Le operazioni di inserimento e aggiornamento su una riga vengono eseguite in più di un nodo. Sebbene la replica di tipo merge e le sottoscrizioni aggiornabili per la replica transazionale consentano il rilevamento e la risoluzione dei conflitti, è preferibile inserire o aggiornare una determinata riga in un unico nodo. La replica transazionale di tipo peer-to-peer non fornisce funzioni di rilevamento e risoluzione dei conflitti e richiede il partizionamento degli inserimenti e degli aggiornamenti.  
  
-   È stata inserita una riga in un Sottoscrittore che dovrebbe essere di sola lettura. I Sottoscrittori delle pubblicazioni snapshot devono essere considerati come di sola lettura, analogamente ai Sottoscrittori delle pubblicazioni transazionali a meno che non vengano utilizzate sottoscrizioni aggiornabili o una replica transazione peer-to-peer.  
  
-   Viene utilizzata una tabella con una colonna Identity, ma la colonna non è gestita in modo appropriato.  
  
-   Nella replica di tipo merge questo errore può verificarsi anche durante un inserimento nella tabella di sistema **MSmerge_contents**. L'errore generato è simile all'errore "Impossibile inserire la riga di chiave duplicata nell'oggetto 'MSmerge_contents' con indice univoco 'ucl1SycContents'".  
  
## <a name="user-action"></a>Azione dell'utente  
 L'azione richiesta dipende dal motivo per il quale è stato generato l'errore:  
  
-   Le operazioni di inserimento e aggiornamento su una riga vengono eseguite in più di un nodo.  
  
     Indipendentemente dal tipo di replica utilizzato, è consigliabile partizionare inserimenti e aggiornamenti quando possibile, in modo da ridurre l'elaborazione richiesta per il rilevamento e la risoluzione dei conflitti. Per la replica transazionale peer-to-peer, è richiesto il partizionamento di inserimenti e aggiornamenti. Per altre informazioni, vedere [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
-   È stata inserita una riga in un Sottoscrittore che dovrebbe essere di sola lettura.  
  
     Non inserire o aggiornare righe nel Sottoscrittore a meno che non si stia utilizzando la replica di tipo merge, la replica transazionale con sottoscrizioni aggiornabili o la replica transazionale peer-to-peer.  
  
-   Viene utilizzata una tabella con una colonna Identity, ma la colonna non è gestita in modo appropriato.  
  
     Per la replica di tipo merge e la replica transazionale con sottoscrizioni aggiornabili, le colonne Identity devono essere gestite automaticamente dalla replica. Per la replica transazionale peer-to-peer, devono invece essere gestite manualmente. Per altre informazioni, vedere [Replicare colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
-   L'errore si verifica durante un inserimento nella tabella di sistema **MSmerge_contents**.  
  
     Tale errore può verificarsi a causa di un valore non corretto per la proprietà del filtro join **join_unique_key**. Tale proprietà deve essere impostata su TRUE solo se la colonna unita in join nella tabella padre è univoca. Se la proprietà è impostata su TRUE ma la colonna non è univoca, viene generato l'errore. Per ulteriori informazioni sull'impostazione di questa proprietà, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
