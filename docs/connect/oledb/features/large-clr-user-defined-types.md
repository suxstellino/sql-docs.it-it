---
title: Tipi CLR definiti dall'utente di grandi dimensioni | Microsoft Docs
description: Informazioni sulle restrizioni di dimensione e sul comportamento correlato per i tipi definiti dall'utente in Common Language Runtime per versioni diverse di SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- large CLR user-defined types
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5b1256b7511fcbf62e976e3734349a8941a38cce
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742521"
---
# <a name="large-clr-user-defined-types"></a>Tipi CLR definiti dall'utente di grandi dimensioni
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In SQL Server 2005 i tipi definiti dall'utente in CLR (Common Language Runtime) sono limitati a dimensioni di 8.000 byte. Questa restrizione è stata eliminata in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] e versioni successive. In questa versione i tipi CLR definiti dall'utente vengono considerati simili ai tipi LOB. Tipi definiti dall'utente minori o uguali a 8.000 byte, pertanto, hanno lo stesso comportamento che in SQL Server 2005, ma sono supportati dati definiti dall'utente di dimensioni maggiori, che vengono indicate come illimitate ("unlimited").  
  
 Per altre informazioni, vedere [Tipi CLR definiti dall'utente di grandi dimensioni &#40;OLE DB&#41;](../../oledb/ole-db/large-clr-user-defined-types-ole-db.md).  
  
## <a name="use-cases"></a>Modalità di utilizzo comuni   
  
 Per OLE DB, il supporto per i tipi definiti dall'utente di grandi dimensioni include la possibilità di eseguire il flusso dei valori di tali tipi al e dal server usando l'associazione ISequentialStream.  
  
 Tipi definiti dall'utente minori o uguali a 8.000 byte hanno lo stesso comportamento che in SQL Server 2005. Per OLE DB, è comunque possibile trasmettere tipi definiti dall'utente di piccole dimensioni usando l'associazione ISequentialStream.  
  
 Il codice nativo dovrà talvolta riconoscere il contenuto dei tipi CLR definiti dall'utente, ma non dovrà creare istanze di oggetti gestiti. In tal caso, è possibile utilizzare serializzazione personalizzata per convertire i valori dei tipi definiti dall'utente nel server in un formato noto per i client.  
  
 Per le applicazioni che dispongono di codice di accesso ai dati, è possibile sfruttare il comportamento dei tipi CLR definiti dall'utente nel client recuperando tali tipi tramite API native e creandone istanze tramite l'interoperabilità C++ CLI in applicazioni in modalità mista.  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
  
  
