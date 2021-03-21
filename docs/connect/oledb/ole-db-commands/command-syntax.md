---
title: Sintassi dei comandi (OLE DB Driver) | Microsoft Docs
description: Informazioni sulla sintassi dei comandi riconosciuta da OLE DB Driver per SQL Server e su come eseguire una stored procedure di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- commands [OLE DB]
- OLE DB Driver for SQL Server, stored procedures
- stored procedures [OLE DB], command syntax
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a8210c1131d928da03bb6b580a5f6218cbfd97af
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751921"
---
# <a name="command-syntax"></a>Sintassi dei comandi
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server riconosce la sintassi dei comandi specificata dalla macro DBGUID_SQL. Per OLE DB Driver per SQL Server l'identificatore indica che una combinazione di ODBC SQL, ISO e [!INCLUDE[tsql](../../../includes/tsql-md.md)] è una sintassi valida. L'istruzione SQL seguente, ad esempio, utilizza una sequenza di escape ODBC SQL per specificare la funzione per i valori stringa LCASE:  
  
```  
SELECT customerid={fn LCASE(CustomerID)} FROM Customers  
```  
  
 LCASE restituisce una stringa di caratteri, convertendo tutti i caratteri maiuscoli nei rispettivi equivalenti minuscoli. Poiché la funzione per i valori stringa ISO LOWER esegue la stessa operazione, l'istruzione SQL seguente è un equivalente ISO dell'istruzione ODBC presentata nell'esempio precedente:  
  
```  
SELECT customerid=LOWER(CustomerID) FROM Customers  
```  
  
 Il driver OLE DB per SQL Server elabora correttamente entrambe le forme dell'istruzione se specificate come testo per un comando.  
  
## <a name="stored-procedures"></a>Stored procedure  
 Quando si esegue una stored procedure di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzando un comando del driver OLE DB per SQL Server, utilizzare la sequenza di escape ODBC CALL nel testo del comando. Il driver OLE DB per SQL Server utilizza quindi il meccanismo di chiamata a stored procedure remote di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ottimizzare l'elaborazione del comando. L'istruzione ODBC SQL seguente, ad esempio, rappresenta il testo del comando preferito rispetto alla forma [!INCLUDE[tsql](../../../includes/tsql-md.md)]:  
  
-   ODBC SQL  
  
    ```  
    {call SalesByCategory('Produce', '1995')}  
    ```  
  
-   Transact-SQL  
  
    ```  
    EXECUTE SalesByCategory 'Produce', '1995'  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Comandi](../../oledb/ole-db-commands/commands.md)  
  
  
