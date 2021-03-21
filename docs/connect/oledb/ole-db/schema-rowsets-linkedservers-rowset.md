---
title: Set di righe LINKEDSERVERS (OLE DB) | Microsoft Docs
description: Il set di righe LINKEDSERVERS enumera le origini dati dell'organizzazione che possono partecipare alle query distribuite di OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- LINKEDSERVERS rowset
- enumerating data sources [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52c70c4034e67e1a6300cbdeb860862100cc0235
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754131"
---
# <a name="schema-rowsets---linkedservers-rowset"></a>Set di righe dello schema - Set di righe LINKEDSERVERS
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il set di righe **LINKEDSERVERS** enumera le origini dati dell'organizzazione che possono partecipare nelle query distribuite di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Il set di righe **LINKEDSERVERS** contiene le colonne seguenti.  
  
|Nome colonna|Indicatore del tipo|Descrizione|  
|-----------------|--------------------|-----------------|  
|SVR_NAME|DBTYPE_WSTR|Nome di un server collegato.|  
|SVR_PRODUCT|DBTYPE_WSTR|Produttore o altro nome che identifica il tipo di archivio dati rappresentato dal nome del server collegato.|  
|SVR_PROVIDERNAME|DBTYPE_WSTR|Nome descrittivo del provider OLE DB impiegato per utilizzare dati dal server.|  
|SVR_DATASOURCE|DBTYPE_WSTR|Stringa OLE DB DBPROP_INIT_DATASOURCE utilizzata per acquisire un'origine dati dal provider.|  
|SVR_PROVIDERSTRING|DBTYPE_WSTR|Valore OLE DB DBPROP_INIT_PROVIDERSTRING utilizzato per acquisire un'origine dati dal provider.|  
|SVR_LOCATION|DBTYPE_WSTR|Stringa OLE DB DBPROP_INIT_LOCATION utilizzata per acquisire un'origine dati dal provider.|  
  
 Il set di righe viene ordinato su SRV_NAME e una singola restrizione è supportata su SRV_NAME.  
  
## <a name="see-also"></a>Vedere anche  
 [Supporto del set di righe dello schema &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowset-support-ole-db.md)  
  
  
