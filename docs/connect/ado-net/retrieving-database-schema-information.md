---
title: Recupero di informazioni dello schema del database
description: Informazioni sull'uso del provider di dati Microsoft SqlClient per SQL Server per recuperare informazioni sullo schema del database.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051330"
---
# <a name="retrieving-database-schema-information"></a>Recupero di informazioni dello schema del database

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il recupero di informazioni sullo schema da un database viene eseguito tramite il processo di individuazione dello schema. L'individuazione dello schema consente alle applicazioni di richiedere ai provider gestiti di trovare e restituire informazioni sullo schema del database, note anche come *metadati* di un determinato database. Nella raccolta di schemi vengono esposti vari elementi dello schema del database, quali tabelle, colonne e stored procedure. Ogni raccolta di schemi contiene una varietà di informazioni sullo schema specifiche del provider usato.

Il provider di dati Microsoft SqlClient per SQL Server implementa il metodo **GetSchema** nella classe **SqlConnection** e le informazioni sullo schema restituite dal metodo **GetSchema** vengono visualizzate sotto forma di <xref:System.Data.DataTable>. **GetSchema** è un metodo in overload che fornisce parametri facoltativi per specificare la raccolta di schemi da restituire e per limitare la quantità di informazioni restituite. Il provider di dati SqlClient fornisce anche un metodo **GetSchemaTable** che restituisce un oggetto DataTable che descrive i metadati della colonna di **SqlDataReader**.

## <a name="in-this-section"></a>Contenuto della sezione

[Raccolte di schemi e GetSchema](getschema-and-schema-collections.md)  
Descrizione del metodo **GetSchema** e di come usarlo per recuperare e limitare le informazioni sullo schema da un database.

[Restrizioni schema](schema-restrictions.md)  
Descrizione delle restrizioni per lo schema che è possibile usare con **GetSchema**. 

[Raccolte di schemi comuni](common-schema-collections.md)  
Descrizione di tutte le raccolte di schemi comuni supportate da tutti i provider gestiti .NET.  
  
[Raccolte di schemi di SQL Server](sql-server-schema-collections.md)  
Descrizione delle raccolte di schemi aggiuntive supportate dal provider di dati Microsoft SqlClient per SQL Server. 

## <a name="reference"></a>Informazioni di riferimento

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
Descrizione del metodo **GetSchema** della classe <xref:System.Data.Common.DbConnection>.

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
Descrizione del metodo **GetSchema** della classe <xref:Microsoft.Data.SqlClient.SqlConnection>.

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
Descrizione del metodo **GetSchemaTable** della classe <xref:System.Data.Common.DbDataReader>. 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
Descrizione del metodo **GetSchemaTable** della classe <xref:Microsoft.Data.SqlClient.SqlDataReader>.

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
