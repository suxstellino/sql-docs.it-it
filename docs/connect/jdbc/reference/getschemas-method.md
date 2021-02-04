---
description: Metodo getSchemas ()
title: Metodo getSchemas () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSchemas
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: adba0ee6-ff6d-4215-b646-62c735be3fe9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a20265dfd8080e75aae0ab5b293ae2e5651cd571
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162372"
---
# <a name="getschemas-method-"></a>Metodo getSchemas ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera i nomi di schema disponibili nel database corrente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.ResultSet getSchemas()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getSchemas viene specificato dal metodo getSchemas nell'interfaccia java.sql.DatabaseMetaData.  
  
 Il set di risultati restituito dal metodo getSchemas contiene le informazioni riportate di seguito:  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|TABLE_SCHEM|**Stringa**|Nome dello schema.|  
|TABLE_CATALOG|**Stringa**|Nome di catalogo per lo schema.|  
  
 I risultati vengono ordinati in base a TABLE_CATALOG e quindi in base a TABLE_SCHEM. La prima colonna di ogni riga è TABLE_SCHEM, mentre la seconda è TABLE_CATALOG.  
  
> [!NOTE]  
>  Per altre informazioni sui dati restituiti dal metodo getSchemas, vedere "sys.schemas (Transact-SQL)" nella documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come usare il metodo getSchemas per restituire informazioni sul catalogo e sui nomi di schema associati in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quando l'argomento della connessione specifica il database da usare.  
  
```  
public static void executeGetSchemas(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getSchemas();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Metodi di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
