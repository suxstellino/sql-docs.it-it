---
title: Uso della clausola OUTPUT con OLE DB nel driver OLE DB per SQL Server | Microsoft Docs
description: Informazioni sull'uso della clausola OUTPUT in un comando INSERT, UPDATE, DELETE o MERGE in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e2c1f942067f3ef5297f6bad67390f3da3bb88e8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745971"
---
# <a name="using-the-output-clause-with-ole-db-in-ole-db-driver-for-sql-server"></a>Uso della clausola OUTPUT con OLE DB nel driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Se si utilizza una clausola OUTPUT in un comando INSERT, UPDATE, DELETE o MERGE, il numero di righe interessate non risulta disponibile. L'applicazione deve contare il numero di righe nel set di righe restituito dalla clausola OUTPUT.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione di un driver OLE DB per applicazione SQL Server](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md) 
  
  
