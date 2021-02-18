---
title: Esecuzione di file script Transact-SQL mediante sqlcmd
description: Informazioni su come usare sqlcmd per eseguire un file di script Transact-SQL. Può contenere istruzioni Transact-SQL, comandi sqlcmd e variabili di scripting.
ms.custom: seo-lt-2019
ms.date: 07/15/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transact sql scripts
ms.assetid: 90067eb8-ca3e-44e8-bb1a-bf7d1a359423
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edfa425e7cceba9264ae640239039105f6b366cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346661"
---
# <a name="sqlcmd---run-transact-sql-script-files"></a>sqlcmd - Eseguire file script Transact-SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Usare **sqlcmd** per eseguire un file di script Transact-SQL. Un file script Transact-SQL è un file di testo che può contenere una combinazione di istruzioni Transact-SQL, comandi **sqlcmd** e variabili di scripting.  

## <a name="create-a-script-file"></a>Creare un file script  
 Per creare un file script Transact-SQL semplice in Blocco note, seguire questa procedura:  
  
1.  Fare clic su **Start**, scegliere **Tutti i programmi**, **Accessori** e quindi fare clic su **Blocco note**.  
  
2.  Copiare e incollare il codice Transact-SQL seguente in Blocco note:  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT p.FirstName + ' ' + p.LastName AS 'Employee Name',  
    a.AddressLine1, a.AddressLine2 , a.City, a.PostalCode   
    FROM Person.Person AS p   
       INNER JOIN HumanResources.Employee AS e   
            ON p.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.BusinessEntityAddress bea   
            ON bea.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.Address AS a   
            ON a.AddressID = bea.AddressID;  
    GO  
    ```  
  
3.  Salvare il file con il nome **myScript.sql** nell'unità C.  
  
## <a name="run-the-script-file"></a>Eseguire il file di script  
  
1.  Aprire una finestra del prompt dei comandi.  
  
2.  Nella finestra del prompt dei comandi digitare **sqlcmd -S myServer\instanceName -i C:\myScript.sql**  
  
3.  Premere INVIO.  
  
 Nella finestra del prompt dei comandi verrà visualizzato un elenco di nomi e indirizzi di dipendenti di [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] .  

## <a name="save-the-output-to-a-text-file"></a>Salvare l'output in un file di testo
  
1.  Aprire una finestra del prompt dei comandi.  
  
2.  Nella finestra del prompt dei comandi digitare **sqlcmd -S myServer\instanceName -i C:\myScript.sql -o C:\EmpAdds.txt**  
  
3.  Premere INVIO.  
  
 Nella finestra del prompt dei comandi non verrà restituito alcun output. L'output verrà invece inviato al file EmpAdds.txt. È possibile verificare l'output aprendo il file EmpAdds.txt.  
  
## <a name="see-also"></a>Vedere anche  
 [Avvio dell'utilità sqlcmd](./sqlcmd-start-the-utility.md)   
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)  
  
