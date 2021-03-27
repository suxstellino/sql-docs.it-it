---
title: Miglioramenti di data e ora in OLE DB
description: In questi articoli viene descritto il modo in cui OLE DB Driver per SQL Server supporta nuovi tipi date e time.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB]
- OLE DB, date/time improvements
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 89a52e5eafcf8d4ae9729a5410778f117dcd0e01
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633310"
---
# <a name="date-and-time-improvements-in-ole-db"></a>Miglioramenti di data e ora in OLE DB

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

In [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] sono stati introdotti nuovi tipi di dati di data e ora. Questa sezione illustra come questi nuovi tipi vengono esposti come estensioni in OLE DB Driver per SQL Server. Per una panoramica del supporto di OLE DB Driver per SQL Server per i nuovi tipi di dati di data e ora, vedere [Miglioramenti relativi a data e ora](../features/date-and-time-improvements.md). Per un esempio, vedere [Usare le funzionalità avanzate di data e ora &#40;OLE DB&#41;](../ole-db-how-to/use-enhanced-date-and-time-features-ole-db.md).

Per altre informazioni generali sui tipi di dati di data e ora, vedere [datetime &#40;Transact-SQL&#41;](../../../t-sql/data-types/datetime-transact-sql.md).

## <a name="in-this-section"></a>Contenuto della sezione

[Supporto dei tipi di dati per i miglioramenti relativi a data e ora OLE DB](data-type-support-for-ole-db-date-and-time-improvements.md)  
Vengono fornite informazioni sui tipi OLE DB (OLE DB Driver per SQL Server) che supportano i tipi di dati di data e ora di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

[Metadati &#40;OLE DB&#41;](metadata-parameter-and-rowset.md)  
Contiene informazioni sulla struttura DBBINDING, su **ICommandWithParameters::GetParameterInfo**, **ICommandWithParameters::SetParameterInfo**, **IColumnsRowset::GetColumnsRowset** e **IColumnsInfo::GetColumnInfo**. Vengono inoltre fornite informazioni sugli aggiornamenti ai set di righe dello schema OLE DB.

[Associazioni e conversioni &#40;OLE DB&#41;](conversions-ole-db.md)  
Vengono illustrate le regole per la conversione fra server e client per tipi di dati sia nuovi che esistenti.

[Modifiche apportate alla copia bulk per i tipi di data e ora migliorati &#40;OLE DB&#41;](bulk-copy-changes-for-enhanced-date-and-time-types-ole-db.md)  
Vengono descritti i miglioramenti apportati ai tipi di data/ora per supportare le operazioni di copia bulk.

[Supporto dell'API OLE DB per i miglioramenti relativi a data e ora](ole-db-api-support-for-date-and-time-enhancements.md)  
Vengono descritte le API OLE DB che supportano le caratteristiche avanzate dei tipi di dati date/time.

[Possibilità di confronto per IRowsetFind](comparability-for-irowsetfind.md)  
Descrive i tipi date/time e **IRowsetFind**.

## <a name="see-also"></a>Vedi anche

[Driver OLE DB per programmazione con SQL Server](../ole-db/oledb-driver-for-sql-server-programming.md)
