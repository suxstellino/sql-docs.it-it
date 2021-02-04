---
description: Metodo getTypeInfo (SQLServerDatabaseMetaData)
title: Metodo getTypeInfo (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43145d029b4095ab05cd47526deface99dc81eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155646"
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>Metodo getTypeInfo (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una descrizione di tutti i tipi SQL standard supportati dal database corrente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getTypeInfo viene specificato dal metodo getTypeInfo nell'interfaccia java.sql.DatabaseMetaData.  
  
 Il set di risultati restituito dal metodo getTypeInfo conterrà le informazioni seguenti:  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|TYPE_NAME|**Stringa**|Nome del tipo di dati.|  
|DATA_TYPE|**short**|Tipo di dati SQL da java.sql.Types.|  
|PRECISION|**int**|Numero totale di cifre significative.|  
|LITERAL_PREFIX|**Stringa**|Uno o più caratteri che precedono il nome di una costante.|  
|LITERAL_SUFFIX|**Stringa**|Uno o più caratteri che seguono il nome di una costante.|  
|CREATE_PARAMS|**Stringa**|Descrizione dei parametri di creazione per il tipo di dati.|  
|NULLABLE|**short**|Indica se la colonna può contenere un valore Null. Può essere uno dei valori seguenti:<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|Indica se il tipo di dati supporta la distinzione tra maiuscole e minuscole. "**true**" se il tipo fa distinzione tra maiuscole e minuscole. In caso contrario, "**false**".|  
|RICERCABILE|**short**|Indica se la colonna può essere utilizzata in una clausola WHERE SQL. Può essere uno dei valori seguenti:<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|Indica il segno del tipo di dati. "**true**" se il tipo è senza segno. In caso contrario, "**false**".|  
|FIXED_PREC_SCALE|**boolean**|Indica se il tipo di dati può essere un valore money. "**true**" se il tipo di dati è money. In caso contrario, "**false**".|  
|AUTO_INCREMENT|**boolean**|Indica se il tipo di dati può essere incrementato automaticamente. "**true**" se il tipo può essere incrementato automaticamente. In caso contrario, "**false**".|  
|LOCAL_TYPE_NAME|**Stringa**|Nome localizzato del tipo di dati.|  
|MINIMUM_SCALE|**short**|Numero massimo di cifre a destra del separatore decimale.|  
|MAXIMUM_SCALE|**short**|Numero minimo di cifre a destra del separatore decimale.|  
|SQL_DATA_TYPE|**int**|Non supportato dal driver JDBC.|  
|SQL_DATETIME_SUB|**int**|Non supportato dal driver JDBC.|  
|NUM_PREC_RADIX|**int**|Numero di bit o di cifre per il calcolo del numero massimo che una colonna può contenere.|  
|INTERVAL_PRECISION|**smallint**|Valore di precisione iniziale dell'intervallo.|  
|USERTYPE|**smallint**|Valore **usertype** della tabella **systypes**. Per ulteriori informazioni, vedere la documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
> [!NOTE]  
>  Per altre informazioni sui dati restituiti dal metodo getTypeInfo, vedere "sp_datatype_info (Transact-SQL)" nella documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come usare il metodo getTypeInfo per restituire informazioni sui tipi di dati usati in un database di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] (o versioni successive).  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
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
  
  
