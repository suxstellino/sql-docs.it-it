---
title: Supporto UTF-16 nel driver OLE DB per SQL Server| Microsoft Docs
description: Informazioni sul supporto UTF-16 in OLE DB Driver per SQL Server e sull'aggiunta di un punto di codice surrogato elevato al buffer.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07be222f6afb7bd2463c68cf0a540f14471dc81d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751091"
---
# <a name="utf-16-support-in-ole-db-driver-for-sql-server"></a>Supporto UTF-16 nel driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  A partire da [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], se si specifica un buffer a lunghezza fissa quando si esegue l'associazione di un risultato della colonna o di un parametro di output e se il carattere **wchar** scritto nel buffer prima del carattere di terminazione è un punto di codice surrogato elevato in una coppia di surrogati, nonché se il carattere **wchar** successivo è un punto di codice surrogato basso, nel driver OLE DB per SQL Server il punto di codice surrogato elevato non viene aggiunto al buffer.  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
  
  
