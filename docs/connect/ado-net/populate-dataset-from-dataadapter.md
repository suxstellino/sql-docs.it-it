---
title: Popolare un DataSet da un DataAdapter
description: Informazioni su come popolare DataSet da DataAdapter in ADO.NET, che fornisce un modello di programmazione relazionale coerente indipendente dall'origine dati.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e6c50bf7255dc77edfd0b93e03dedeec83ed4c4d
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771503"
---
# <a name="populate-a-dataset-from-a-dataadapter"></a>Popolare un DataSet da un DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La classe <xref:System.Data.DataSet> di ADO.NET è una rappresentazione di dati residente in memoria che fornisce un modello di programmazione relazionale coerente indipendente dall'origine dati. Il `DataSet` rappresenta un set completo di dati che include tabelle, vincoli e relazioni tra tabelle. Poiché il `DataSet` è indipendente dall'origine dati, un `DataSet` può includere dati locali dell'applicazione nonché dati di più origini dati. L'interazione con le origini dati esistenti è controllata tramite `DataAdapter`.

La proprietà `SelectCommand` di `DataAdapter` è un oggetto `Command` che recupera i dati dall'origine dati. Le proprietà `InsertCommand`, `UpdateCommand`e `DeleteCommand` di `DataAdapter` sono oggetti `Command` che gestiscono gli aggiornamenti ai dati nell'origine dati in base alle modifiche apportate nel `DataSet`. Queste proprietà sono illustrate più dettagliatamente in [Aggiornare origini dati con DataAdapter](update-data-sources-with-dataadapters.md).

Il metodo `Fill` di `DataAdapter` viene usato per popolare un oggetto `DataSet` con i risultati dell'oggetto `SelectCommand` di `DataAdapter`. `Fill` accetta come argomenti un oggetto `DataSet` da popolare e un oggetto `DataTable` o il nome dell'oggetto `DataTable` da popolare con le righe restituite da `SelectCommand`.

> [!NOTE]
> L'utilizzo di `DataAdapter` per recuperare un'intera tabella richiede del tempo, soprattutto se la tabella contiene molte righe. L'accesso al database, l'individuazione e l'elaborazione dei dati e il successivo trasferimento dei dati al client tramite rete sono infatti processi lunghi. Se viene eseguito il pull dell'intera tabella nel client, vengono anche bloccate tutte le righe sul server. Per migliorare le prestazioni, è possibile usare la clausola `WHERE` in modo da ridurre sensibilmente il numero di righe restituite al client. È anche possibile ridurre la quantità di dati restituiti al client elencando in modo esplicito solo le colonne necessarie nell'istruzione `SELECT` . Un'altra soluzione alternativa efficace consiste nel recuperare le righe in batch, ad esempio diverse centinaia alla volta, e recuperare il batch successivo solo quando il client ha terminato con quello corrente.

Nel metodo `Fill` viene usato in modo implicito l'oggetto `DataReader` per restituire i nomi e i tipi delle colonne usate per creare le tabelle nel `DataSet`, nonché i dati per compilare le righe delle tabelle nel `DataSet`. Le tabelle e le colonne vengono create solo se non esistono già. In caso contrario, nel metodo `Fill` viene usato lo schema del `DataSet` esistente. I tipi di colonne vengono creati come tipi .NET Framework in base alle tabelle contenute in [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md). Le chiavi primarie vengono create solo se esistono nell'origine dati e `DataAdapter` **.** `MissingSchemaAction` è impostato su `MissingSchemaAction` **.** `AddWithKey`. Se `Fill` rileva la presenza di una chiave primaria per una tabella, sovrascriverà i dati presenti nel `DataSet` con quelli prelevati dall'origine dati per le righe in cui i valori della colonna della chiave primaria corrispondono a quelli della riga restituita dall'origine dati. Se non vengono rilevate chiavi primarie , i dati vengono aggiunti alle tabelle nell'oggetto `DataSet`. `Fill` usa qualsiasi mapping esistente quando si popola `DataSet`. Vedere [Mapping di DataAdapter, DataTable e DataColumn](dataadapter-datatable-datacolumn-mappings.md).

> [!NOTE]
> Se `SelectCommand` restituisce i risultati di un OUTER JOIN, mediante `DataAdapter` non viene impostato un valore di `PrimaryKey` per l'oggetto `DataTable`risultante. Per assicurarsi che le righe duplicate vengano risolte correttamente, sarà necessario definire `PrimaryKey` in modo autonomo.

Nell'esempio di codice seguente viene creata un'istanza di un tipo <xref:Microsoft.Data.SqlClient.SqlDataAdapter> che usa un tipo <xref:Microsoft.Data.SqlClient.SqlConnection> nel database `Northwind` di Microsoft SQL Server e viene compilato un tipo <xref:System.Data.DataTable> in un `DataSet` con l'elenco dei clienti. L'istruzione SQL e gli argomenti <xref:Microsoft.Data.SqlClient.SqlConnection> passati al costruttore <xref:Microsoft.Data.SqlClient.SqlDataAdapter> vengono usati per creare la proprietà <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> del tipo <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="example"></a>Esempio

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

> [!NOTE]
> Con il codice illustrato in questo esempio, l'oggetto `Connection`non viene aperto e chiuso in modo esplicito. Il metodo `Fill` apre in modo implicito l'oggetto `Connection` usato da `DataAdapter` se rileva che la connessione non è già aperta. Se la connessione è stata aperta da `Fill` , esso provvederà anche a chiuderla una volta terminato `Fill` . Questa procedura consente di semplificare il codice quando si esegue una singola operazione come `Fill` o `Update`. Tuttavia, se si eseguono più operazioni che richiedono una connessione aperta, per migliorare le prestazioni dell'applicazione è possibile chiamare in modo esplicito il metodo `Open` dell'oggetto `Connection`, eseguire le operazioni sull'origine dati, quindi chiamare il metodo `Close` dell'oggetto `Connection`. È necessario cercare di tenere aperte le connessioni con l'origine dati per un intervallo di tempo minimo, in modo da liberare le risorse che devono essere usate da altre applicazioni client.

## <a name="multiple-result-sets"></a>Più set di risultati

Se l'oggetto `DataAdapter` rileva più set di risultati, vengono create più tabelle nel `DataSet`. Alle tabelle viene assegnato il nome predefinito incrementale Table *N*, che inizia con "Table" per Table0. Se il nome di una tabella viene passato come argomento al metodo `Fill` , alle tabelle viene assegnato il nome predefinito incrementale TableName *N*, che inizia con "TableName" per TableName0.  
  
## <a name="populate-a-dataset-from-multiple-dataadapters"></a>Popolare un oggetto DataSet da più DataAdapter  

È possibile usare qualsiasi numero di oggetti `DataAdapter` con `DataSet`. Ogni oggetto `DataAdapter` può essere usato per compilare uno o più oggetti `DataTable` e risolvere gli aggiornamenti fino all'origine dati pertinente. Gli oggetti`DataRelation` e `Constraint` possono essere aggiunti all'oggetto `DataSet` localmente, consentendo di creare relazioni tra dati provenienti da origini dati diverse. Un `DataSet` , ad esempio, può contenere dati di un database Microsoft SQL Server, un database IBM DB2 esposto tramite OLE DB e un'origine dati che crea flussi XML. Uno o più oggetti `DataAdapter` possono gestire le comunicazioni con ciascuna origine dati.  
  
### <a name="example"></a>Esempio  

Nell'esempio di codice seguente vengono compilati un elenco di clienti dal database `Northwind` in Microsoft SQL Server e un elenco di ordini dal database `Northwind` archiviato in Microsoft Access. Viene creata una relazione tra le tabelle compilate tramite `DataRelation`e viene quindi visualizzato l'elenco di clienti con i relativi ordini.

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="sql-server-decimal-type"></a>Tipo decimal di SQL Server

Per impostazione predefinita, `DataSet` archivia i dati usando tipi di dati .NET. Per la maggior parte delle applicazioni, questi tipi consentono di rappresentare in modo adeguato le informazioni delle origini dei dati. Tuttavia, questo tipo di rappresentazione può generare un problema quando il tipo di dati nell'origine dati ha valore numeric o decimal di SQL Server. Il tipo di dati `decimal` di .NET può contenere fino a 28 cifre significative, mentre il tipo di dati `decimal` di SQL Server ne può contenere 38. Se, durante un'operazione `SqlDataAdapter` , `Fill` determina che la precisione di un campo `decimal` di SQL Server è maggiore di 28 caratteri, la riga corrente non viene aggiunta all'oggetto `DataTable`. Viene invece generato un evento `FillError` , che consente di determinare se si è verificata una perdita di precisione e quindi di rispondere in modo appropriato. Per altre informazioni sull'evento `FillError`, vedere [Gestire eventi DataAdapter](handle-dataadapter-events.md). Per ottenere il valore `decimal` di SQL Server, è anche possibile usare un oggetto <xref:Microsoft.Data.SqlClient.SqlDataReader> e chiamare il metodo <xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> .

ADO.NET fornisce anche un supporto migliorato per <xref:System.Data.SqlTypes> nel tipo `DataSet`. Per altre informazioni, vedere [SqlTypes and the DataSet](./sql/sqltypes-dataset.md).

## <a name="see-also"></a>Vedere anche

- [DataAdapter e DataReader](dataadapters-datareaders.md)
- [Mapping dei tipi di dati in ADO.NET](data-type-mappings-ado-net.md)
- [MARS (Multiple Active Result Sets)](./sql/multiple-active-result-sets-mars.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
