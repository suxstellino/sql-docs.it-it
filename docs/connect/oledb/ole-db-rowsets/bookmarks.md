---
title: Segnalibri di righe (OLE DB Driver) | Microsoft Docs
description: Informazioni su come i segnalibri consentono ai consumer di tornare rapidamente a una riga accedendo in modo casuale alle righe in base al valore del segnalibro in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- bookmarks [OLE DB]
- OLE DB Driver for SQL Server, bookmarks
- rowsets [OLE DB], bookmarks
- OLE DB rowsets, bookmarks
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca8688c797f304ae99f0382a3f2f242a0cc21e8e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753121"
---
# <a name="bookmarks"></a>Segnalibri
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  I segnalibri consentono ai consumer di tornare rapidamente su una riga. Grazie ai segnalibri, essi possono accedere in modo casuale alle righe in base al valore del segnalibro. La colonna del segnalibro è la colonna 0 nel set di righe. Il consumer imposta il valore del campo dwFlag della struttura di associazione su DBCOLUMNSINFO_ISBOOKMARK per indicare che la colonna viene utilizzata come segnalibro. Imposta inoltre la proprietà del set di righe DBPROP_BOOKMARKS su VARIANT_TRUE, consentendo alla colonna 0 di essere presente nel set di righe. Viene quindi usato il metodo **IRowsetLocate::GetRowsAt** per recuperare le righe, a partire da quella specificata in corrispondenza di un offset da un segnalibro.  
  
## <a name="see-also"></a>Vedere anche  
 [Set di righe](../../oledb/ole-db-rowsets/rowsets.md)  
  
  
