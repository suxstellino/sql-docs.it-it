---
description: 'Creazione di un set di righe con ICommand:: Execute (provider OLE DB Native Client)'
title: 'Creare un set di righe con ICommand:: Execute (provider di OLE DB di Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], creating
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets, creating
- Execute method
ms.assetid: 9b530b7d-8165-49d4-a978-5ced17c6705e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d0c77af4281fe53b4a02fca0e09ae6060c292cf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740801"
---
# <a name="creating-rowsets-with-icommandexecute-in-sql-server-native-client"></a>Creazione di set di righe con ICommand:: Execute in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per i set di righe creati tramite il metodo **ICommand::Execute** le proprietà desiderate nel set di righe risultante possono determinare restrizioni per il testo del comando. Si tratta di un fattore particolarmente critico per i consumer che supportano testo del comando dinamico.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non è [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in grado di utilizzare i cursori per supportare i risultati di più set di righe generati da molti comandi. Se un consumer richiede un set di righe per cui è necessario il supporto del cursore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si verifica un errore se il testo del comando genera più di un singolo set di righe come risultato. Per altre informazioni, vedere [Comandi che generano risultati con più set di righe](../../relational-databases/native-client-ole-db-commands/commands-generating-multiple-rowset-results.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]I set di righe del provider OLE DB di Native Client scorrevoli sono supportati dai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cursori. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impone limitazioni sui cursori che sono sensibili alle modifiche apportate dagli altri utenti del database. In particolare, le righe in alcuni cursori non possono essere ordinate e il tentativo di creare un set di righe tramite un comando che contiene una clausola SQL ORDER BY può non riuscire. Per altre informazioni, vedere [Set di righe e cursori SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Set di righe](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
