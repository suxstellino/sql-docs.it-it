---
title: Parametri DataAdapter
description: Informazioni sulle proprietà di DbDataAdapter che restituiscono i dati da un'origine dati e gestiscono le modifiche apportate all'origine dati.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 90985f155392a78da408ffbda3e1d2c0b12d4648
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771603"
---
# <a name="dataadapter-parameters"></a>Parametri DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:System.Data.Common.DbDataAdapter> dispone di quattro proprietà che consentono di recuperare e aggiornare i dati dell'origine dati. La proprietà <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> restituisce i dati dall'origine dati, mentre le proprietà <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> e <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> vengono usate per gestire le modifiche nell'origine dati.

> [!NOTE]
> La proprietà `SelectCommand` deve essere impostata prima di chiamare il metodo `Fill` di `DataAdapter`. È necessario impostare la proprietà `InsertCommand`, `UpdateCommand` o `DeleteCommand` prima di chiamare il metodo `Update` di `DataAdapter` a seconda delle modifiche apportate ai dati in <xref:System.Data.DataTable>, Se ad esempio sono state aggiunte righe, è necessario impostare la proprietà `InsertCommand` prima di chiamare `Update`. Quando `Update` elabora una riga inserita, aggiornata o eliminata, `DataAdapter` usa la rispettiva proprietà `Command` per l'operazione. Le informazioni correnti sulla riga modificata vengono passate all'oggetto `Command` mediante la raccolta `Parameters`.

Quando si aggiorna una riga nell'origine dati, si chiama l'istruzione UPDATE, che usa un identificatore univoco per identificare la riga della tabella da aggiornare. In genere, il valore dell'identificatore univoco corrisponde a quello del campo di una chiave primaria. Nell'istruzione UPDATE vengono usati i parametri che contengono sia l'identificatore univoco che le colonne e i valori da aggiornare, come illustrato nell'istruzione Transact-SQL seguente.

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> La sintassi per i segnaposto dei parametri varia in base all'origine dati. In questo esempio vengono mostrati i segnaposto per un'origine dati SQL Server.

In questo esempio il campo `CompanyName` viene aggiornato con il valore del parametro `@CompanyName` nella riga in cui `CustomerID` è uguale al valore del parametro `@CustomerID`. Le informazioni della riga modificata vengono recuperate dai parametri usando la proprietà <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A> dell'oggetto <xref:Microsoft.Data.SqlClient.SqlParameter>. Di seguito sono riportati i parametri della precedente istruzione UPDATE di esempio. Nel codice si presuppone che la variabile `adapter` rappresenti un oggetto <xref:Microsoft.Data.SqlClient.SqlDataAdapter> valido.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

Il metodo `Add` della raccolta `Parameters` accetta il nome del parametro, il tipo di dati, le dimensioni (se applicabili al tipo) e il nome dell'oggetto <xref:System.Data.Common.DbParameter.SourceColumn%2A> da `DataTable`. Notare che la proprietà <xref:System.Data.Common.DbParameter.SourceVersion%2A> del parametro `@CustomerID` è impostata su `Original`. Questo valore assicura che l'aggiornamento della riga esistente nell'origine dati venga eseguito se il valore della colonna o delle colonne identificative è stato cambiato nell'oggetto <xref:System.Data.DataRow> modificato. In questo caso il valore `Original` della riga corrisponde al valore corrente nell'origine dati e il valore `Current` della riga contiene il valore aggiornato. `SourceVersion` non è impostato per il parametro `@CompanyName`, pertanto verrà usato il valore di riga `Current` predefinito.

> [!NOTE]
> Per entrambe le operazioni `Fill` dei metodi `DataAdapter` e `Get` di `DataReader`, il tipo .NET viene dedotto dal tipo restituito dal provider di dati Microsoft SqlClient per SQL Server. I tipi .NET dedotti e i metodi delle funzioni di accesso per i tipi di dati Microsoft SQL Server sono descritti in [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md).

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn e Parameter.SourceVersion

È possibile passare `SourceColumn` e `SourceVersion` come argomenti del costruttore `Parameter` o impostarli come proprietà di un oggetto `Parameter` esistente. `SourceColumn` è il nome dell'oggetto <xref:System.Data.DataColumn> derivato da <xref:System.Data.DataRow> in cui viene recuperato il valore di `Parameter`. `SourceVersion` specifica la versione di `DataRow` usata da `DataAdapter` per recuperare il valore.

Nella tabella seguente sono elencati i valori di enumerazione <xref:System.Data.DataRowVersion> disponibili per l'uso con `SourceVersion`.

|Enumerazione DataRowVersion|Descrizione|  
|--------------------------------|-----------------|  
|`Current`|Il parametro usa il valore corrente della colonna. Questo è il valore predefinito.|  
|`Default`|Il parametro usa il valore `DefaultValue` della colonna.|  
|`Original`|Il parametro usa il valore originale della colonna.|  
|`Proposed`|Il parametro usa un valore proposto.|  

Nell'esempio di codice `SqlClient` della sezione successiva viene definito un parametro per un oggetto <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> in cui la colonna `CustomerID` viene usata come `SourceColumn` per due parametri: `@CustomerID` (`SET CustomerID = @CustomerID`) e `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). Il parametro `@CustomerID` viene usato per aggiornare la colonna **CustomerID** in base al valore corrente di `DataRow`. Di conseguenza, viene usato `CustomerID` `SourceColumn` in cui il valore di `SourceVersion` è uguale a `Current`. Il parametro `@OldCustomerID` viene usato per identificare la riga corrente nell'origine dati. Poiché il valore della colonna corrispondente viene individuato nella versione `Original` della riga, verrà usato lo stesso oggetto `SourceColumn` (`CustomerID`) con `SourceVersion``Original`.

## <a name="work-with-sqlclient-parameters"></a>Usare i parametri SqlClient

Nell'esempio seguente viene illustrato come creare un oggetto <xref:Microsoft.Data.SqlClient.SqlDataAdapter> e impostare <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> su <xref:System.Data.MissingSchemaAction.AddWithKey> per recuperare informazioni aggiuntive sullo schema dal database. Vengono impostate le proprietà <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> e <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> e i relativi oggetti <xref:Microsoft.Data.SqlClient.SqlParameter> corrispondenti vengono aggiunti alla raccolta <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A>. Il metodo restituisce un oggetto `SqlDataAdapter`.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>Vedere anche

- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Comandi e parametri](commands-parameters.md)
- [Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md)
- [Modificare i dati con stored procedure](modify-data-with-stored-procedures.md)
- [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
