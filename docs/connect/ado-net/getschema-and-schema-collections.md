---
title: GetSchema e raccolte di schemi
description: Descrizione dell'uso del metodo GetSchema per recuperare e limitare le informazioni sullo schema da un database.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051336"
---
# <a name="get-schema-and-schema-collections"></a>GetSchema e raccolte di schemi

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le classi **SqlConnection** nel provider di dati Microsoft SqlClient per SQL Server implementano un metodo **GetSchema** che viene usato per recuperare informazioni sullo schema del database attualmente connesso. Inoltre, le informazioni sullo schema vengono restituite dal metodo **GetSchema** sotto forma di <xref:System.Data.DataTable>. **GetSchema** è un metodo in overload che fornisce parametri facoltativi per specificare la raccolta di schemi da restituire e per limitare la quantità di informazioni restituite.

## <a name="specifying-the-schema-collections"></a>Specifica delle raccolte di schemi

Il primo parametro facoltativo del metodo **GetSchema** è il nome della raccolta specificato come stringa. Sono disponibili due tipi di raccolte di schemi: raccolte di schemi comuni a tutti i provider e raccolte di schemi specifici, ovvero schemi specifici per ciascun provider.  

È possibile eseguire una query nel provider di dati Microsoft SqlClient per SQL Server per determinare l'elenco delle raccolte di schemi supportate chiamando il metodo **GetSchema** senza argomenti oppure con il nome della raccolta di schemi "MetaDataCollections". In questo modo verrà restituito un oggetto <xref:System.Data.DataTable> con un elenco delle raccolte di schemi supportati, il numero delle restrizioni supportate da ciascuna raccolta e il numero di parti identificatore usate.  

### <a name="retrieving-schema-collections-example"></a>Esempio di recupero delle raccolte di schemi

Gli esempi seguenti illustrano come usare il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> del provider di dati Microsoft SqlClient per la classe <xref:Microsoft.Data.SqlClient.SqlConnection> di SQL Server per recuperare informazioni sullo schema relative a tutte le tabelle contenute nel database di esempio **AdventureWorks**:  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>Vedere anche

- [Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
