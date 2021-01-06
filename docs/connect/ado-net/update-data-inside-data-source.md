---
title: Aggiornamento di dati in un'origine dati
description: Viene descritto come eseguire i comandi o le stored procedure che modificano i dati in un database.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771483"
---
# <a name="updating-data-in-a-data-source"></a>Aggiornamento di dati in un'origine dati

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le istruzioni SQL che modificano i dati, ad esempio INSERT, UPDATE o DELETE, non restituiscono righe. Analogamente, molte stored procedure eseguono un'operazione ma non restituiscono righe. Per eseguire comandi che non restituiscono righe, creare un oggetto **Command** con il comando SQL appropriato e un oggetto **Connection** con tutti i valori di **Parameters** necessari. Eseguire il comando con il metodo <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlCommand>.

> [!NOTE]
> Il metodo **ExecuteNonQuery** restituisce un valore intero che rappresenta il numero di righe interessate dall'istruzione o dalla stored procedure eseguita. Se si eseguono più istruzioni, il valore restituito sarà la somma dei record interessati da ognuna delle istruzioni eseguite.

## <a name="example"></a>Esempio

Nell'esempio di codice seguente viene eseguita un'istruzione INSERT per inserire un record in un database usando **ExecuteNonQuery**.
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

Nell'esempio di codice seguente viene eseguita la stored procedure creata dal codice di esempio in [Esecuzione di operazioni nel catalogo](perform-catalog-operations.md). La stored procedure non restituisce righe, quindi viene usato il metodo **ExecuteNonQuery**, ma riceve un parametro di input e restituisce un parametro di output e un valore restituito.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>Vedere anche

- [Uso di comandi per modificare i dati](use-commands-to-modify-data.md)
- [Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md)
- [Comandi e parametri](commands-parameters.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
