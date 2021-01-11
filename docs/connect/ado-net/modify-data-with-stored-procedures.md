---
title: Modificare i dati con stored procedure
description: Viene descritto come usare i parametri di input e di output della stored procedure per inserire una riga in un database, restituendo un nuovo valore Identity.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744407"
---
# <a name="modify-data-with-stored-procedures"></a>Modificare i dati con stored procedure

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le stored procedure possono accettare dati come parametri di input e possono restituire dati come parametri di output, set di risultati o valori restituiti. L'esempio seguente illustra il modo in cui il provider di dati Microsoft SqlClient per SQL Server invia e riceve i parametri di input, di output e i valori restituiti. Nell'esempio viene inserito un nuovo record in una tabella in cui la colonna chiave primaria è una colonna Identity.

> [!NOTE]
> Se si usano le stored procedure per modificare o eliminare i dati usando una classe <xref:Microsoft.Data.SqlClient.SqlDataAdapter>, assicurarsi di non usare **SET NOCOUNT ON** nella definizione della stored procedure. Con tale comando il totale restituito delle righe interessate è pari a zero e tale situazione viene interpretata da `DataAdapter` come un conflitto di concorrenza. In questo caso verrà generata un'eccezione <xref:System.Data.DBConcurrencyException>.

## <a name="example"></a>Esempio

Nell'esempio viene usata la stored procedure seguente per inserire nel database **Northwind** una nuova categoria nella tabella **Categories**. La stored procedure accetta il valore nella colonna **CategoryName** come parametro di input e usa la funzione **SCOPE_IDENTITY()** per recuperare il nuovo valore nel campo Identity, **CategoryID**, e restituirlo come parametro di output. L'istruzione RETURN usa la funzione **\@\@ROWCOUNT** per restituire il numero delle righe inserite.

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

Nell'esempio di codice seguente viene usata la stored procedure `InsertCategory` sopra indicata come origine per <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> di <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Il parametro di output `@Identity` e il valore restituito verranno riflessi in <xref:System.Data.DataSet> dopo l'inserimento del record nel database quando viene chiamato il metodo `Update` di <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Nel codice viene inoltre recuperato il valore restituito:

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](retrieving-modifying-data.md)
- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Esecuzione di un comando](execute-command.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
