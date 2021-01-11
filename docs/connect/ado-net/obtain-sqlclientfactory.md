---
title: Ottenere un'istanza di SqlClientFactory
description: Informazioni su come ottenere un'istanza di SqlClientFactory dalla classe DbProviderFactories per usare origini dati specifiche in .NET.
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771763"
---
# <a name="obtain-a-sqlclientfactory"></a>Ottenere un'istanza di SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il processo di recupero di un oggetto <xref:System.Data.Common.DbProviderFactory> implica il passaggio delle informazioni su un provider di dati alla classe <xref:System.Data.Common.DbProviderFactories>. Sulla base di queste informazioni, il metodo <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> crea una factory del provider fortemente tipizzata. Ad esempio, per creare un’istanza di <xref:Microsoft.Data.SqlClient.SqlClientFactory>, è possibile passare a `GetFactory` una stringa con il nome del provider specificato come "**Microsoft.Data.SqlClient**".

L'altro overload di `GetFactory` accetta un oggetto <xref:System.Data.DataRow>. Dopo aver creato la factory del provider, è quindi possibile usarne i metodi per creare altri oggetti. I metodi di un oggetto `SqlClientFactory` includono <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A>e <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.

## <a name="register-sqlclientfactory"></a>Registrare un’istanza di SqlClientFactory

Per recuperare l'oggetto <xref:Microsoft.Data.SqlClient.SqlClientFactory> dalla classe <xref:System.Data.Common.DbProviderFactories> in .NET Framework, è necessario registrarlo in un file **App.config** o **web.config**. Nel frammento di file di configurazione seguente sono illustrati la sintassi e il formato di <xref:Microsoft.Data.SqlClient>.  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

L'attributo **invariant** identifica il provider di dati sottostante. La sintassi di denominazione in tre parti viene inoltre usata durante la creazione di una nuova factory e per l'identificazione del provider in un file di configurazione dell'applicazione in modo da consentire il recupero del nome del provider, unitamente alla stringa di connessione associata, in fase di esecuzione.  

> [!NOTE]  
> In .NET Core, poiché non esiste alcun supporto della GAC o della configurazione globale, l'oggetto <xref:Microsoft.Data.SqlClient.SqlClientFactory> deve essere registrato chiamando il metodo <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A> nel progetto.

Nell'esempio seguente viene illustrato come usare <xref:Microsoft.Data.SqlClient.SqlClientFactory> in un'applicazione .NET Core.

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>Vedi anche

- [Oggetti DbProviderFactory](dbproviderfactories.md)
- [Stringhe di connessione](connection-strings.md)
- [Uso delle classi di configurazione](/previous-versions/aspnet/ms228063(v=vs.100))
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
