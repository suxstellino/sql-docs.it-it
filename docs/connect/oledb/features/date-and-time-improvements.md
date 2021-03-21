---
title: Miglioramenti relativi a data e ora | Microsoft Docs
description: Informazioni sul supporto di OLE DB Driver per SQL Server per i tipi di dati date e time aggiunti in SQL Server 2008.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca548ab01e4379b6cea92b5764820383abaf926b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742861"
---
# <a name="date-and-time-improvements"></a>Miglioramenti relativi a data e ora
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questo argomento descrive il supporto di OLE DB Driver per SQL Server per i tipi di dati di data e ora aggiunti in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
 Per altre informazioni sui miglioramenti relativi a data/ora, vedere [Miglioramenti relativi a data e ora &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md).  
  
 Per informazioni sulle applicazioni di esempio in cui viene illustrata questa funzionalità, vedere la [pagina relativa agli esempi di programmazione dati di SQL Server](https://msftdpprodsamples.codeplex.com/).  
  
## <a name="usage"></a>Uso  
 Nelle sezioni seguenti vengono descritte le diverse modalità di utilizzo dei nuovi tipi di data e ora.  
  
### <a name="use-date-as-a-distinct-data-type"></a>Utilizzare il tipo date come tipo di dati distinto  
 A partire da [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], il supporto migliorato per i tipi di data/ora consente un uso più efficiente del tipo DBTYPE_DBDATE OLE DB.  
  
### <a name="use-time-as-a-distinct-data-type"></a>Utilizzare il tipo time come tipo di dati distinto  
 OLE DB include già un tipo di dati che contiene solo l'ora, ovvero DBTYPE_DBTIME, che garantisce precisione pari a 1 secondo.
  
 Il nuovo tipo di dati time di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ha una precisione frazionaria dei secondi pari a 100 nanosecondi. Ciò richiede un nuovo tipo in OLE DB Driver per SQL Server: DBTYPE_DBTIME2. Le applicazioni esistenti scritte per utilizzare le ore senza secondi frazionari possono utilizzare colonne time(0). Il tipo OLE DB DBTYPE_TIME esistente e i relativi struct funzioneranno correttamente, a meno che le applicazioni non si basino sul tipo restituito nei metadati.  
  
### <a name="use-time-as-a-distinct-data-type-with-extended-fractional-seconds-precision"></a>Utilizzare il tipo time come tipo di dati distinto con precisione frazionaria dei secondi estesa  
 Alcune applicazioni, ad esempio le applicazioni di controllo dei processi e di produzione, richiedono la possibilità di gestire i dati di tipo time con una precisione pari a fino 100 nanosecondi. A questo scopo, in OLE DB è disponibile il nuovo tipo DBTYPE_DBTIME2.  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision"></a>Utilizzare il tipo datetime con precisione frazionaria dei secondi estesa  
 OLE DB definisce già un tipo con una precisione pari fino a 1 nanosecondo. Questo tipo, tuttavia, è già utilizzato dalle applicazioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esistenti, che garantiscono una probabile precisione pari solo a 1/300 di secondo. Il nuovo tipo **datetime2(3)** non è direttamente compatibile con il tipo datetime esistente. Se vi è un rischio che tale tipo di dati influisca negativamente sul comportamento dell'applicazione, le applicazioni devono utilizzare un nuovo flag DBCOLUMN per determinare il tipo di server effettivo.    
  
### <a name="use-datetime-with-extended-fractional-seconds-precision-and-timezone"></a>Utilizzare il tipo datetime con precisione frazionaria dei secondi estesa e fuso orario  
 Alcune applicazioni richiedono valori datetime con informazioni sul fuso orario. Questa necessità è supportata dal nuovo tipo DBTYPE_DBTIMESTAMPOFFSET.
  
### <a name="use-datetimedatetimedatetimeoffset-data-with-client-side-conversions-consistent-with-existing-conversions"></a>Utilizzare dati di tipo date/time/datetime/datetimeoffset con conversioni sul lato client coerenti con le conversioni esistenti  
 Le conversioni sono estese in modo coerente per includere conversioni tra tutti i tipi di data e ora introdotti in [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
