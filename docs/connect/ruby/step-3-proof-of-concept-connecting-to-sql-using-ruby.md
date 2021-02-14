---
title: 'Passaggio 3: Modello di verifica per la connessione a SQL tramite Ruby'
description: Il passaggio 3 è un modello di verifica che illustra come è possibile connettersi a SQL Server usando Ruby. Gli esempi di base illustrano la selezione e l'inserimento dei dati.
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cac20b18-0a6d-4243-bbda-a5d1b9476441
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93c40d3c64f2a32cd9f318dc236c4c350cfddc28
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058116"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-ruby"></a>Passaggio 3: Modello di verifica per la connessione a SQL tramite Ruby

Questo esempio deve essere considerato solo un modello di verifica.  Il codice di esempio è semplificato per maggiore chiarezza e non rappresenta necessariamente le procedure consigliate da Microsoft.  
  
## <a name="step-1--connect"></a>Passaggio 1:  Connessione  
  
La funzione [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) viene usata per connettersi al database SQL.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
```  
  
## <a name="step-2--execute-a-query"></a>Passaggio 2:  Eseguire una query  
  
Copiare e incollare codice seguente in un file vuoto. Denominarlo test.rb. Quindi eseguirlo immettendo il comando seguente dal prompt dei comandi:  
  
```ruby
    ruby test.rb  
```
  
Nel codice di esempio, la funzione [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) viene utilizzata per recuperare un set di risultati da una query sul database SQL. Questa funzione accetta una query e restituisce un set di risultati. Il set di risultati è iterato usando [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds).  
  
```ruby 
    require 'tiny_tds'    
    print 'test'       
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")  
    results.each do |row|  
    puts row  
    end  
```  
  
## <a name="step-3--insert-a-row"></a>Passaggio 3:  Inserire una riga  
  
Questo esempio illustra come eseguire un'istruzione [INSERT](../../t-sql/statements/insert-transact-sql.md) in modo sicuro e come passare i parametri che proteggono l'applicazione da attacchi [SQL injection](../../relational-databases/tables/primary-and-foreign-key-constraints.md).    
  
Per usare TinyTDS con Azure, si consiglia di eseguire diverse istruzioni `SET` per modificare la modalità di gestione di informazioni specifiche della sessione corrente. Le istruzioni consigliate `SET`vengono fornite nell’esempio di codice Ad esempio, `SET ANSI_NULL_DFLT_ON` consentirà a nuove colonne create di autorizzare valori nulli anche se lo stato di supporto di valori nulli della colonna non è indicato in modo esplicito.  
  
Per allinearlo con il formato [datetime](../../t-sql/data-types/datetime-transact-sql.md) di Microsoft SQL Server, usare la funzione [strftime](https://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) per eseguire il cast nel formato datetime corrispondente.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SET ANSI_NULLS ON")  
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")  
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")  
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")  
    results = client.execute("SET ANSI_PADDING ON")  
    results = client.execute("SET QUOTED_IDENTIFIER ON")  
    results = client.execute("SET ANSI_WARNINGS ON")  
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")  
    require 'date'  
    t = Time.now  
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")  
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)  
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")  
    results.each do |row|  
    puts row  
    end  
```
