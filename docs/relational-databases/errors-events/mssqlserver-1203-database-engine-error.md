---
description: MSSQLSERVER_1203
title: MSSQLSERVER_1203 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1203 (Database Engine error)
ms.assetid: 33a35f00-98c8-46c6-b432-544b326b6117
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 43cbe733bebcc6a3050d64e15750d49393b637fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197185"
---
# <a name="mssqlserver_1203"></a>MSSQLSERVER_1203
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1203|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LK_NOT|  
|Testo del messaggio|Il processo con ID %d ha tentato di sbloccare una risorsa di cui non è proprietario: %.*ls. Ripetere la transazione. L'errore potrebbe essere dovuto a una condizione basata sul tempo. Se il problema persiste, contattare l'amministratore del database.|  
  
## <a name="explanation"></a>Spiegazione  
Questo errore si verifica quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è impegnato in attività diverse dalle normali operazioni di pulizia dei dati temporanei post-elaborazione e tenta di sbloccare una pagina specifica che in realtà è già sbloccata.  
  
### <a name="possible-causes"></a>Possibili cause  
La causa sottostante di questo errore può essere correlata a problemi strutturali nel database interessato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestisce l'acquisizione e il rilascio di pagine per mantenere il controllo della concorrenza nell'ambiente multiutente. Per il mantenimento di questo meccanismo vengono utilizzate diverse strutture di blocco interne che identificano la pagina e il tipo di blocco presente. I blocchi vengono acquisiti per consentire l'elaborazione delle pagine interessate e rilasciati al termine dell'elaborazione.  
  
## <a name="user-action"></a>Azione dell'utente  
Eseguire DBCC CHECKDB sul database cui appartiene l'oggetto. Se DBCC CHECKDB non restituisce errori, provare a ristabilire la connessione e a eseguire il comando.  
  
> [!IMPORTANT]  
> Se l'esecuzione di DBCC CHECKDB con una delle clausole REPAIR non consente di correggere il problema relativo all'indice oppure non si è certi dell'effetto prodotto sui dati dall'esecuzione di DBCC CHECKDB con una clausola REPAIR, contattare il personale del supporto tecnico.  
  
