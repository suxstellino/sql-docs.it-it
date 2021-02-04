---
description: Metodo getFunctionColumns (SQLServerDatabaseMetaData)
title: Metodo getFunctionColumns (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e2b0e0f7-717c-48e6-bcd2-a325d938a833
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03a5bbe04c9fdf511659a2abef7e6e4135e87370
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175757"
---
# <a name="getfunctioncolumns-method-sqlserverdatabasemetadata"></a>Metodo getFunctionColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una descrizione dei parametri e del tipo restituito delle funzioni utente o di sistema del catalogo specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public ResultSet getFunctionColumns(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern  
                       java.lang.String columnNamePattern)  
```  
  
#### <a name="parameters"></a>Parametri  
 *catalog*  
  
 Valore **String** contenente il nome del catalogo. Se è una stringa vuota (""), il risultato include le funzioni senza un catalogo. Se è **null**, il nome del catalogo non viene usato per la ricerca.  
  
 *schemaPattern*  
  
 Valore **String** contenente il modello del nome dello schema. Se è una stringa vuota (""), il risultato include le funzioni senza uno schema. Se è **null**, il nome dello schema non viene usato per la ricerca.  
  
 *functionNamePattern*  
  
 Valore **String** che contiene il nome di una funzione.  
  
 *columnNamePattern*  
  
 Valore **String** che contiene il nome di un parametro.  
  
## <a name="return-value"></a>Valore restituito  
 Oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Eccezioni  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Osservazioni  
 Questo metodo getFunctionColumns viene specificato dal metodo getFunctionColumns nell'interfaccia java.sql.DatabaseMetaData.  
  
 Restituisce solo le funzioni e i parametri corrispondenti allo schema, al nome della funzione e al nome del parametro specificati all'interno del catalogo specificato.  
  
 Ogni riga nel set di risultati include le colonne seguenti per una descrizione del parametro, una descrizione della colonna o un tipo restituito:  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**Stringa**|Nome del database contenente la funzione.|  
|FUNCTION_SCHEM|**Stringa**|Schema della funzione.|  
|FUNCTION_NAME|**Stringa**|Nome della funzione.|  
|COLUMN_NAME|**Stringa**|Nome di un parametro o di una colonna.|  
|COLUMN_TYPE|**short**|**Tipo della colonna. Può essere uno dei valori seguenti:**<br /><br /> functionColumnUnknown (0): Tipo sconosciuto.<br /><br /> functionColumnIn (1): parametro di input.<br /><br /> functionColumnInOut (2): parametro di input/output.<br /><br /> functionColumnOut (3): parametro di output.<br /><br /> functionReturn (4): valore restituito della funzione.<br /><br /> functionColumnResult (5): un parametro o una colonna è una colonna nel set di risultati.|  
|DATA_TYPE|**smallint**|Tipo di dati SQL da Java.sql.Types.|  
|TYPE_NAME|**Stringa**|Nome del tipo di dati.|  
|PRECISION|**int**|Numero totale di cifre significative.|  
|LENGTH|**int**|Lunghezza dei dati in byte.|  
|SCALE|**short**|Numero di cifre a destra del separatore decimale.|  
|RADIX|**short**|Base per i tipi numerici.|  
|NULLABLE|**short**|Indica se il parametro o il valore restituito può contenere un valore **null**.<br /><br /> **Può essere uno dei valori seguenti:**<br /><br /> functionNoNulls (0): il valore NULL non è consentito.<br /><br /> functionNullable (1): il valore NULL è consentito.<br /><br /> functionNullableUnknown (2): Sconosciuto.|  
|REMARKS|**Stringa**|Commenti su una colonna o un parametro.|  
|COLUMN_DEF|**Stringa**|Valore predefinito della colonna.<br /><br /> **Nota:** Queste informazioni sono disponibili con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e sono specifiche del driver JDBC.|  
|SQL_DATA_TYPE|**smallint**|Questa colonna corrisponde alla colonna **DATA_TYPE**, tranne che per i tipi di dati **datetime** e ISO **interval**.<br /><br /> **Nota:** Queste informazioni sono disponibili con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e sono specifiche del driver JDBC.|  
|SQL_DATETIME_SUB|**smallint**|Sottocodice **datetime** ISO **interval** se il valore di **SQL_DATA_TYPE** è **SQL_DATETIME** o **SQL_INTERVAL**. Per i tipi di dati diversi da **datetime** e **ISO interval**, questa colonna è NULL.<br /><br /> **Nota:** Queste informazioni sono disponibili con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e sono specifiche del driver JDBC.|  
|CHAR_OCTET_LENGTH|**int**|Lunghezza massima dei parametri o delle colonne di tipo carattere o binario. Per gli altri tipi di dati il valore è NULL.|  
|ORDINAL_POSITION|**int**|Per i parametri di input e di output, rappresenta la posizione a partire da 1.<br /><br /> Per le colonne del set di risultati, è la posizione della colonna nel set di risultati a partire da 1.<br /><br /> Per il valore restituito, è 0.|  
|IS_NULLABLE|**Stringa**|Determina se un parametro o una colonna ammette i valori Null.<br /><br /> Può essere uno dei valori seguenti:<br /><br /> **YES**: il parametro o la colonna può includere valori NULL.<br /><br /> **NO**: il parametro o la colonna non può includere valori NULL.<br /><br /> Stringa vuota (""): Sconosciuto.|  
|SS_TYPE_CATALOG_NAME|**Stringa**|Nome del catalogo contenente il tipo definito dall'utente (UDT).|  
|SS_TYPE_SCHEMA_NAME|**Stringa**|Nome dello schema contenente il tipo definito dall'utente (UDT).|  
|SS_UDT_CATALOG_NAME|**Stringa**|Tipo definito dall'utente (UDT) del nome completo.|  
|SS_UDT_SCHEMA_NAME|**Stringa**|Nome del catalogo in cui viene definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome del catalogo, questa variabile contiene una stringa vuota.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**Stringa**|Nome dello schema in cui viene definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome dello schema, viene visualizzata una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**Stringa**|Nome di una raccolta di XML Schema. Se non è possibile trovare il nome, viene visualizzata una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**Stringa**|Nome del catalogo contenente il tipo definito dall'utente (UDT).|  
|SS_XML_SCHEMACOLLECTION_NAME|**Stringa**|Nome dello schema contenente il tipo definito dall'utente (UDT).|  
|SS_DATA_TYPE|**tinyint**|Tipo di dati di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usato in stored procedure estese.<br /><br /> **Nota** Per altre informazioni sui tipi di dati restituiti da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vedere "Tipi di dati (Transact-SQL)" nella documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
