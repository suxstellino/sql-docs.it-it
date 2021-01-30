---
title: MSSQLSERVER_4104 | Microsoft Docs
description: Non è stato possibile eseguire il mapping dell'identificatore in più parti specificato a un'entità esistente. Vedere la descrizione dell'errore e delle possibili soluzioni.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4104 (Database Engine error)
ms.assetid: 52dc32d8-97ad-4ef0-834d-2e68f215d007
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9f28d2e4cf04427887584cab6ee27de80024eae7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185114"
---
# <a name="mssqlserver_4104"></a>MSSQLSERVER_4104
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|4104|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|ALG_MULTI_ID_BAD|  
|Testo del messaggio|Impossibile associare l'identificatore in più parti "%. * ls".|  
  
## <a name="explanation"></a>Spiegazione  
Il nome di un'entità in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene definito *identificatore*. Gli identificatori vengono utilizzati quando si fa riferimento a entità, ad esempio, quando si specificano nomi di colonne e tabelle in una query. Un identificatore in più parti contiene uno o più qualificatori usati come prefisso. Un identificatore di tabella può essere preceduto, ad esempio, da qualificatori come il nome del database e dello schema in cui è contenuta la tabella. Un identificatore di colonna può essere preceduto da qualificatori come un nome o un alias della tabella.  
  
L'errore 4104 indica che non è stato possibile eseguire il mapping dell'identificatore in più parti specificato a un'entità esistente. Questo errore può essere restituito nelle condizioni seguenti:  
  
-   Il qualificatore fornito come prefisso per un nome di colonna non corrisponde ad alcun alias o nome di tabella utilizzato nella query.  
  
    Nell'istruzione seguente viene, ad esempio utilizzato un alias di tabella (`Dept`) come prefisso di colonna. Nella clausola FROM non viene tuttavia fatto riferimento a tale alias di tabella.  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department;  
    ```  
  
    Nelle istruzioni seguenti un identificatore di colonna in più parti `TableB.KeyCol` viene specificato nella clausola WHERE come parte di una condizione JOIN tra due tabelle. A `TableB`, tuttavia, non viene fatto riferimento in modo esplicito nella query.  
  
    ```  
    DELETE FROM TableA WHERE TableA.KeyCol = TableB.KeyCol;  
    ```  
  
    ```  
    SELECT 'X' FROM TableA WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   Un nome di alias per la tabella viene specificato nella clausola FROM, ma il qualificatore fornito per una colonna è il nome della tabella. Nell'istruzione seguente, ad esempio, viene utilizzato il nome di tabella `Department` come prefisso di colonna. Nella clausola FROM è stato tuttavia specificato l'alias `Dept` per la tabella a cui viene fatto riferimento.  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department AS Dept;  
    ```  
  
    Quando viene utilizzato un alias, il nome della tabella non può essere utilizzato in altre posizioni all'interno dell'istruzione.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di determinare se l'identificatore in più parti fa riferimento a una colonna preceduta da una tabella o a una proprietà di un tipo di dati CLR definito dall'utente (UTD) preceduto da una colonna. Ciò accade in quanto alle proprietà delle colonne definite dall'utente viene fatto riferimento utilizzando come separatore il punto (.) tra il nome della colonna e il nome della proprietà, così come un nome di colonna è preceduto da un nome di tabella. Nell'esempio seguente vengono create due tabelle, ovvero `a` e `b`. La tabella `b` contiene la colonna `a` che usa il tipo di dati CLR definito dall'utente `dbo.myudt2`. L'istruzione SELECT contiene un identificatore in più parti `a.c2`.  
  
    ```  
    CREATE TABLE a (c2 int);   
    GO  
    ```  
  
    ```  
    CREATE TABLE b (a dbo.myudt2);   
    GO  
    ```  
  
    ```  
    SELECT a.c2 FROM a, b;   
    ```  
  
    Presupponendo che il tipo UDT `myudt2` non disponga di una proprietà denominata `c2`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di determinare se l'identificatore `a.c2` fa riferimento alla colonna `c2` nella tabella `a` o alla colonna `a`, proprietà `c2` nella tabella `b`.  
  
## <a name="user-action"></a>Azione dell'utente  
  
-   Mettere in corrispondenza i prefissi di colonna con in nomi di tabella o gli alias specificati nella clausola FROM della query. Se un alias è definito per un nome di tabella nella clausola FROM, è possibile utilizzare l'alias solo come qualificatore per le colonne associate alla tabella.  
  
    Le istruzioni appena descritte che fanno riferimento alla tabella `HumanResources.Department` possono essere corrette nel modo seguente:  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department AS Dept;  
    GO  
    ```  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department;  
    GO  
    ```  
  
-   Assicurarsi che tutte le tabelle vengano specificate nella query e che le condizioni JOIN tra le tabelle vengano specificate correttamente. L'istruzione DELETE può essere corretta nel modo seguente:  
  
    ```  
    DELETE FROM dbo.TableA  
    WHERE TableA.KeyCol = (SELECT TableB.KeyCol   
                            FROM TableB   
                            WHERE TableA.KeyCol = TableB.KeyCol);  
    GO  
    ```  
  
    L'istruzione SELECT precedente relativa a `TableA` può essere corretta nel modo seguente:  
  
    ```  
    SELECT 'X' FROM TableA, TableB WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
    o  
  
    ```  
    SELECT 'X' FROM TableA INNER JOIN TableB ON TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   Utilizzare nomi univoci chiaramente definiti per gli identificatori per facilitare la lettura e la gestione del codice e ridurre inoltre il rischio di creare riferimenti ambigui a più entità.  
  
## <a name="see-also"></a>Vedere anche  
[MSSQLSERVER_107](~/relational-databases/errors-events/mssqlserver-107-database-engine-error.md)  
[Identificatori del database](~/relational-databases/databases/database-identifiers.md)  
  
