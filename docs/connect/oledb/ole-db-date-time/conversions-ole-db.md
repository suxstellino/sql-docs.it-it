---
title: Associazioni e conversioni (OLE DB) | Microsoft Docs
description: Informazioni su come OLE DB Driver per SQL Server esegue la conversione tra i valori datetime e datetimeoffset. Vi sono diverse regole di conversione generali.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB]
- bindings [OLE DB]
- OLE DB, bindings and conversions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e87bcae399abd2b563f9c347f6752f49568a523
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755341"
---
# <a name="conversions-ole-db"></a>Conversioni (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questa sezione descrive come eseguire la conversione tra i valori **datetime** e **datetimeoffset**. Le conversioni descritte in questa sezione sono già disponibili in OLE DB o costituiscono un'estensione coerente di OLE DB.  
  
 Il formato di valori letterali e stringhe per date e ore in OLE DB segue in genere lo standard ISO e non dipende dalle impostazioni locali del client. Un'eccezione è rappresentata da DBTYPE_DATE, che utilizza come standard l'automazione OLE. Tuttavia, poiché OLE DB Driver per SQL Server esegue conversioni tra tipi solo quando i dati vengono trasmessi al o dal client, un'applicazione non può forzare in alcun modo OLE DB Driver per SQL Server a eseguire conversioni tra DBTYPE_DATE e formati stringa. In caso contrario, le stringhe utilizzano i formati indicati di seguito. Il testo tra parentesi indica un elemento facoltativo.  
  
-   Il formato delle stringhe **datetime** e **datetimeoffset** è:  
  
     *aaaa*-*mm*-*gg*[ *hh*:*mm*:*ss*[.*9999999*][ ± *hh*:*mm*]]  
  
-   Il formato delle stringe **time** è:  
  
     *hh*:*mm*:*ss*[.*9999999*]  
  
-   Il formato delle stringhe **date** è:  
  
     *aaaa*-*mm*-*gg*  
  
> [!NOTE]  
>  Le versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client e SQLOLEDB implementano conversioni OLE se le conversioni standard non vengono eseguite correttamente. OLE DB Driver per SQL Server funziona in modo analogo a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Di conseguenza, alcune conversioni eseguite in OLE DB Driver per SQL Server differiscono dalla specifica OLE DB.  
  
 Le conversioni dalle stringhe consentono flessibilità nella larghezza degli spazi vuoti e dei campi. Per altre informazioni, vedere la sezione "Formati di dati: stringhe e valori letterali" in [Supporto dei tipi di dati per i miglioramenti relativi a data e ora OLE DB](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md).  
  
 Di seguito vengono fornite le regole di conversione generali:  
  
-   Quando una stringa viene convertita in un tipo di data/ora, la stringa viene prima analizzata come valore letterale ISO. Se l'operazione ha esito negativo, la stringa viene analizzata come valore letterale di data OLE, che include componenti per l'ora.  
  
-   Se l'ora non è presente ma il ricevitore può archiviare l'ora, questa viene impostata su zero. Se la data non è presente ma il ricevitore può archiviare la data, questa viene impostata sulla data corrente quando si utilizzano conversioni ISO e su 1899-12-30 quando si utilizzano conversioni OLE.  
  
-   Se nel tipo di dati utilizzato dal client non è presente il fuso orario, ma il server può archiviare il fuso orario, la data nel client viene considerata nel fuso orario utilizzato dal client stesso.  
  
-   Se nel server non è presente il fuso orario ma il client dispone di informazioni sul fuso orario, viene presupposto il fuso orario UTC. Questo comportamento differisce da quello del server.  
  
-   Se l'ora è presente ma il ricevitore non può archiviare l'ora, il componente per l'ora viene ignorato.  
  
-   Se la data è presente ma il ricevitore non può archiviare la data, il componente per la data viene ignorato.  
  
-   Se si verifica un troncamento di secondi o di secondi frazionari durante la conversione dal client al server, viene restituito DB_E_ERRORSOCCURRED e viene impostato lo stato DBSTATUS_E_DATAOVERFLOW.  
  
-   Se si verifica un troncamento di secondi o di secondi frazionari durante la conversione dal server al client, viene impostato lo stato DBSTATUS_S_TRUNCATED.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Conversioni eseguite da client a server](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md)  
 Vengono descritte le conversioni di data/ora eseguite tra un'applicazione client scritta con la specifica driver OLE DB per SQL Server e [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (o versione successiva).  
  
 [Conversioni eseguite da server a client](../../oledb/ole-db-date-time/conversions-performed-from-server-to-client.md)  
 Vengono descritte le conversioni di data/ora eseguite tra [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], o versione successiva, e un'applicazione client scritta con driver OLE DB per SQL Server.  
  
## <a name="see-also"></a>Vedere anche  
 [Miglioramenti relativi a data e ora &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
