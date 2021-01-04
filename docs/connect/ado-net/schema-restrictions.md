---
title: Restrizioni schema
description: Descrizione delle restrizioni dello schema che possono essere usate con GetSchema quando si usa il provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051325"
---
# <a name="schema-restrictions"></a>Restrizioni schema

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il secondo parametro facoltativo del metodo **GetSchema** è rappresentato dalle restrizioni usate per limitare la quantità di informazioni sullo schema restituite e viene passato al metodo **GetSchema** come matrice di stringhe. La posizione nella matrice determina i valori che è possibile passare ed equivale al numero della restrizione.  
  
Ad esempio, nella tabella seguente sono descritte le restrizioni supportate dalla raccolta di schemi "Tables" usando il provider di dati Microsoft SqlClient per SQL Server. Restrizioni aggiuntive per le raccolte di schemi di SQL Server vengono indicate alla fine di questo argomento.  

|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|TABLE_CATALOG|1|  
|Proprietario|@Owner|TABLE_SCHEMA|2|  
|Tabella|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>Specifica dei valori di restrizione  

Per usare una delle restrizioni della raccolta di schemi "Tables", è sufficiente creare una matrice di stringa con quattro elementi, quindi posizionare un valore nell'elemento che corrisponde al numero della restrizione. Ad esempio, per limitare le tabelle restituite dal metodo **GetSchema** solo a quelle incluse nello schema "Sales", impostare il secondo elemento della matrice su "Sales" prima di passarlo al metodo **GetSchema**.  
  
> [!NOTE]
> - Le raccolte di restrizioni per `SqlClient` hanno una colonna `ParameterName` aggiuntiva. La colonna di restrizione predefinita è ancora disponibile per garantire la compatibilità con le versioni precedenti, ma attualmente è ignorata. Per ridurre il rischio di attacchi SQL injection quando si specificano i valori di restrizione, si consiglia di usare query con parametri invece di sostituire le stringhe.  
> - Il numero di elementi nella matrice deve essere inferiore o uguale al numero di restrizioni supportato per la raccolta di schemi specificato, in caso contrario verrà generato un tipo <xref:System.ArgumentException>. Il numero di restrizioni può essere inferiore al numero massimo consentito. Le restrizioni mancanti verranno considerate null (senza restrizioni).  
  
È possibile eseguire una query nel provider di dati Microsoft SqlClient per SQL Server per determinare l'elenco delle restrizioni supportate chiamando il metodo **GetSchema** con il nome della raccolta di schemi delle restrizioni, ovvero "Restrictions". In questo modo verrà restituito un oggetto <xref:System.Data.DataTable> con un elenco dei nomi delle raccolte, i nomi delle restrizioni, i valori di restrizione predefiniti e i numeri delle restrizioni.  
  
### <a name="example"></a>Esempio  

Negli esempi seguenti viene illustrato come usare il metodo <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> del provider di dati Microsoft SqlClient per la classe <xref:Microsoft.Data.SqlClient.SqlConnection> di SQL Server per recuperare informazioni sullo schema relative a tutte le tabelle contenute nel database di esempio **AdventureWorks** e per limitare le informazioni restituite alle sole tabelle incluse nello schema "Sales":  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>Restrizioni per gli schemi di SQL Server  

 Nelle tabelle seguenti sono incluse le restrizioni per le raccolte di schemi di SQL Server.  
  
### <a name="users"></a>Utenti  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>Database  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Nome|@Name|Nome|1|  
  
### <a name="tables"></a>Tabelle  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|TABLE_CATALOG|1|  
|Proprietario|@Owner|TABLE_SCHEMA|2|  
|Tabella|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>Colonne  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|TABLE_CATALOG|1|  
|Proprietario|@Owner|TABLE_SCHEMA|2|  
|Tabella|@Table|TABLE_NAME|3|  
|Colonna|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|TABLE_CATALOG|1|  
|Proprietario|@Owner|TABLE_SCHEMA|2|  
|Tabella|@Table|TABLE_NAME|3|  
|Colonna|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>Viste  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|TABLE_CATALOG|1|  
|Proprietario|@Owner|TABLE_SCHEMA|2|  
|Tabella|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|VIEW_CATALOG|1|  
|Proprietario|@Owner|VIEW_SCHEMA|2|  
|Tabella|@Table|VIEW_NAME|3|  
|Colonna|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|SPECIFIC_CATALOG|1|  
|Proprietario|@Owner|SPECIFIC_SCHEMA|2|  
|Nome|@Name|SPECIFIC_NAME|3|  
|Parametro|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>Procedure  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|SPECIFIC_CATALOG|1|  
|Proprietario|@Owner|SPECIFIC_SCHEMA|2|  
|Nome|@Name|SPECIFIC_NAME|3|  
|Tipo|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|db_name()|1|  
|Proprietario|@Owner|user_name()|2|  
|Tabella|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|Colonna|@Column|c.name|5|  
  
### <a name="indexes"></a>Indici  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|db_name()|1|  
|Proprietario|@Owner|user_name()|2|  
|Tabella|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|Nome della restrizione|Nome parametro|Impostazione predefinita della restrizione|Numero della restrizione|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catalogo|@Catalog|CONSTRAINT_CATALOG|1|  
|Proprietario|@Owner|CONSTRAINT_SCHEMA|2|  
|Tabella|@Table|TABLE_NAME|3|  
|Nome|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>Vedere anche

- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
