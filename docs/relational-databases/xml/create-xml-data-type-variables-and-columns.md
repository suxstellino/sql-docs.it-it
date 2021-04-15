---
title: Creare variabili e colonne con tipo di dati XML | Microsoft Docs
description: Informazioni su come creare colonne e variabili del tipo di dati XML in SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- xml data type [SQL Server], variables
- xml data type [SQL Server], columns
ms.assetid: 8994ab6e-5519-4ba2-97a1-fac8af6f72db
author: rothja
ms.author: jroth
ms.openlocfilehash: cd1d16b7b5fe813087715ec754ae4b53e3b47d7a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488892"
---
# <a name="create-xml-data-type-variables-and-columns"></a>Creazione di variabili e colonne con tipo di dati XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Il tipo di dati **xml** è un tipo di dati predefinito di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed è simile per alcuni aspetti ad altri tipi predefiniti, quali **int** e **varchar**. Come per gli altri tipi predefiniti, è possibile usare il tipo di dati **xml** come tipo di colonna quando si crea una tabella, come tipo di variabile, tipo di parametro, tipo restituito dalla funzione oppure in [CAST e CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
## <a name="creating-columns-and-variables"></a>Creazione di colonne e variabili  
 Per creare una colonna di tipo `xml` come parte di una tabella, usare un'istruzione `CREATE TABLE` come illustrato nell'esempio seguente:  
  
```  
CREATE TABLE T1(Col1 int primary key, Col2 xml)   
```  
  
 È possibile usare una `DECLARE statement` per creare una variabile del tipo `xml` , come illustrato nell'esempio seguente.  
  
```  
DECLARE @x xml   
```  
  
 Creare una variabile `xml` tipizzata specificando una raccolta di XML Schema, come mostrato nell'esempio seguente.  
  
```  
DECLARE @x xml (Sales.StoreSurveySchemaCollection)  
```  
  
 Per passare un parametro di tipo `xml` a una stored procedure, usare un'istruzione `CREATE PROCEDURE` come illustrato nell'esempio seguente.  
  
```  
CREATE PROCEDURE SampleProc(@XmlDoc xml) AS ...   
```  
  
 È possibile utilizzare XQuery per eseguire query su istanze XML archiviate in colonne, parametri o variabili. È inoltre possibile utilizzare il linguaggio XML DML (Data Manipulation Language) per l'applicazione di aggiornamenti alle istanze XML. Poiché al momento dello sviluppo lo standard XQuery non prevedeva istruzioni DML per il linguaggio XQuery, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduce in XQuery le estensioni del [linguaggio XML DML (Data Modification Language)](../../t-sql/xml/xml-data-modification-language-xml-dml.md) . Queste estensioni consentono l'esecuzione di operazioni di inserimento, aggiornamento ed eliminazione.  
  
## <a name="assigning-defaults"></a>Assegnazione di valori predefiniti  
 All'interno di una tabella è possibile assegnare un'istanza XML predefinita a una colonna di tipo **xml** . È possibile fornire dati XML predefiniti in una delle due modalità: usando una costante XML o un cast esplicito al tipo **xml** .  
  
 Per fornire dati XML predefiniti come costante XML, utilizzare la sintassi come illustrato nell'esempio seguente. Si noti che la stringa è di tipo CAST implicito al tipo **xml** .  
  
```  
CREATE TABLE T (XmlColumn xml default N'<element1/><element2/>')  
```  
  
 Per fornire i dati XML predefiniti come un `CAST` esplicito a `xml`, utilizzare la sintassi come illustrato nell'esempio seguente.  
  
```  
CREATE TABLE T (XmlColumn xml   
                  default CAST(N'<element1/><element2/>' AS xml))  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta anche i vincoli NULL e NOT NULL sulle colonne di tipo **xml** . Ad esempio:  
  
```  
CREATE TABLE T (XmlColumn xml NOT NULL)  
```  
  
## <a name="specifying-constraints"></a>Specifica dei vincoli  
 Durante la creazione di colonne di tipo **xml** è possibile definire vincoli a livello di colonna o di tabella. Utilizzare i vincoli nelle situazioni seguenti:  
  
-   Le regole business non possono essere espresse negli elementi XML Schema. Se ad esempio l'indirizzo di recapito di un fiorista deve essere entro 80 Km dal negozio, tale condizione può essere espressa come vincolo sulla colonna XML. Il vincolo può includere metodi per il tipo di dati **xml** .  
  
-   Il vincolo coinvolge altre colonne XML o non XML nella tabella. Un esempio è l'applicazione dell'ID di un cliente (`/Customer/@CustId`) trovato in un'istanza XML per la corrispondenza con il valore in una colonna relazionale CustomerID.  
  
 È possibile specificare vincoli per le colonne di tipi di dati **xml** tipizzate o non tipizzate. Nella specifica dei vincoli non è tuttavia possibile usare i [metodi con tipo di dati XML](../../t-sql/xml/xml-data-type-methods.md) . Si noti anche che il tipo di dati **xml** non supporta i vincoli di colonna e tabella seguenti:  
  
-   PRIMARY KEY/ FOREIGN KEY  
  
-   UNIQUE  
  
-   COLLATE  
  
     XML fornisce una codifica specifica. Le regole di confronto sono applicabili unicamente ai tipi stringa. Il tipo di dati **xml** non è di tipo stringa. Fornisce tuttavia la rappresentazione di stringa e consente il cast a e da i tipi di dati stringa.  
  
-   RULE  
  
 Un'alternativa all'uso di vincoli è la creazione di un wrapper, una funzione definita dall'utente per eseguire il wrapping del metodo con tipo di dati **xml** e specificare la funzione definita dall'utente nel vincolo CHECK, come illustrato nell'esempio seguente.  
  
 Nell'esempio seguente, il vincolo in `Col2` specifica che tutte le istanze XML archiviate nella colonna devono avere un elemento `<ProductDescription>` contenente un attributo `ProductID` . Il vincolo viene imposto da questa funzione definita dall'utente:  
  
```  
CREATE FUNCTION my_udf(@var xml) returns bit  
AS BEGIN   
RETURN @var.exist('/ProductDescription/@ProductID')  
END  
GO  
```  
  
 Si noti che il metodo `exist()` con tipo di dati `xml` restituisce `1` se l'elemento `<ProductDescription>` nell'istanza contiene l'attributo `ProductID` . In caso contrario, viene restituito `0`.  
  
 A questo punto è possibile creare una colonna con vincoli a livello di colonna come illustrato di seguito:  
  
```  
CREATE TABLE T (  
    Col1 int primary key,   
    Col2 xml check(dbo.my_udf(Col2)=1))  
GO  
```  
  
 L'inserimento seguente ha esito positivo:  
  
```  
INSERT INTO T values(1,'<ProductDescription ProductID="1" />')  
```  
  
 L'inserimento seguente ha invece esito negativo, a causa del vincolo:  
  
```  
INSERT INTO T values(1,'<Product />')  
```  
  
## <a name="same-or-different-table"></a>Utilizzo di un'unica tabella o di più tabelle  
 È possibile creare una colonna con tipo di dati **xml** in una tabella che contiene altre colonne relazionali oppure in una tabella separata con una relazione di chiave esterna con una tabella principale.  
  
 Creare una colonna con tipo di dati **xml** nella stessa tabella quando si verifica una delle condizioni seguenti:  
  
-   L'applicazione esegue operazioni di recupero dei dati sulla colonna XML e non richiede un indice XML su tale colonna.  
  
-   Si vuole compilare un indice XML sulla colonna con tipo di dati **xml** e la chiave primaria della tabella principale coincide con la chiave di clustering. Per altre informazioni, vedere [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
 Creare la colonna con tipo di dati **xml** in una tabella separata se si verificano le condizioni seguenti:  
  
-   Si vuole compilare un indice XML sulla colonna con tipo di dati **xml** , ma la chiave primaria della tabella principale è diversa dalla chiave di clustering, la tabella principale non ha una chiave primaria, oppure la tabella principale è un heap, ovvero è priva di chiave di clustering. Questa situazione può verificarsi quando si utilizza una tabella principale esistente.  
  
-   Si desidera evitare che l'analisi della tabella venga rallentata a causa della presenza della colonna XML, che utilizza spazio sia che venga archiviata all'interno delle righe che all'esterno delle righe.  
  
  
