---
title: Mapping dei dati dei parametri CLR | Microsoft Docs
description: In questo articolo vengono elencati Microsoft SQL Server tipi di dati, equivalenti in CLR per SQL Server e gli equivalenti CLR nativi nell'.NET Framework.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- SqlBinary data type
- SqlInt16 data type
- SqlMoney data type
- SqlString data type
- SqlSingle data type
- data types [CLR integration]
- SqlInt64 data type
- SqlDateTime data type
- SqlXml data type
- SqlBoolean data type
- SqlDecimal data type
- SqlBytes data type
- mapping data types [CLR integration]
- SqlChars data type
- SqlInt32 data type
ms.assetid: 89b43ee9-b9ad-4281-a4bf-c7c8d116daa2
author: rothja
ms.author: jroth
ms.openlocfilehash: bffc04b0015e6d9634aae15fff4e4312d9cc213b
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981009"
---
# <a name="mapping-clr-parameter-data"></a>Mapping dei dati dei parametri CLR
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Nella tabella seguente sono elencati i [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di dati, gli equivalenti nella Common Language Runtime (CLR) per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nello spazio dei nomi **System. Data. SqlTypes** e i relativi equivalenti CLR nativi nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework.  
  
|**Tipo di dati di SQL Server**|Tipo (in System.Data.SqlTypes o Microsoft.SqlServer.Types)|**Tipo di dati CLR (.NET Framework)**|  
|-|-|-|  
|**bigint**|**SqlInt64**|**Int64, Nullable\<Int64>**|  
|**binary**|**SqlBytes, SqlBinary**|**Byte []**|  
|**bit**|**SqlBoolean**|**Booleano, Nullable\<Boolean>**|  
|**char**|nessuno|nessuno|  
|**cursor**|nessuno|nessuno|  
|**date**|**SqlDateTime**|**DateTime, Nullable\<DateTime>**|  
|**datetime**|**SqlDateTime**|**DateTime, Nullable\<DateTime>**|  
|**datetime2**|nessuno|**DateTime, Nullable\<DateTime>**|  
|**DATETIMEOFFSET**|**Nessuno**|**DateTimeOffset, Nullable\<DateTimeOffset>**|  
|**decimal**|**SqlDecimal**|**Decimale, Nullable\<Decimal>**|  
|**float**|**SqlDouble**|**Double, Nullable\<Double>**|  
|**geography**|**SqlGeography**<br /><br /> **SqlGeography** è definito in Microsoft.SqlServer.Types.dll, installato con SQL Server e può essere scaricato dal [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|nessuno|  
|**geometry**|**SqlGeometry**<br /><br /> **SqlGeometry** è definito in Microsoft.SqlServer.Types.dll, installato con SQL Server e può essere scaricato dal [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|nessuno|  
|**hierarchyid**|**SqlHierarchyId**<br /><br /> **SqlHierarchyId** è definito in Microsoft.SqlServer.Types.dll, installato con SQL Server e può essere scaricato dal [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [Feature Pack](https://www.microsoft.com/download/details.aspx?id=100430).|nessuno|  
|**image**|nessuno|nessuno|  
|**int**|**SqlInt32**|**Int32, Nullable\<Int32>**|  
|**money**|**SqlMoney**|**Decimale, Nullable\<Decimal>**|  
|**nchar**|**SqlChars, SqlString**|**String, Char[]**|  
|**ntext**|nessuno|nessuno|  
|**numeric**|**SqlDecimal**|**Decimale, Nullable\<Decimal>**|  
|**nvarchar**|**SqlChars, SqlString**<br /><br /> **SqlChars** rappresenta una corrispondenza migliore per il trasferimento e l'accesso ai dati e **SqlString** rappresenta una corrispondenza migliore per l'esecuzione di operazioni di stringa.|**String, Char[]**|  
|**nvarchar (1), nchar (1)**|**SqlChars, SqlString**|**Char, String, Char [], Nullable\<char>**|  
|**real**|**SqlSingle** (l'intervallo di **SqlSingle**, tuttavia, è maggiore di **Real**)|**Singolo, Nullable\<Single>**|  
|**rowversion**|nessuno|**Byte []**|  
|**smallint**|**SqlInt16**|**Int16, Nullable\<Int16>**|  
|**smallmoney**|**SqlMoney**|**Decimale, Nullable\<Decimal>**|  
|**sql_variant**|nessuno|**Object**|  
|**tabella**|nessuno|nessuno|  
|**text**|nessuno|nessuno|  
|**time**|nessuno|**TimeSpan, Nullable\<TimeSpan>**|  
|**timestamp**|nessuno|nessuno|  
|**tinyint**|**SqlByte**|**Byte, Nullable\<Byte>**|  
|**uniqueidentifier**|**SqlGuid**|**GUID, Nullable\<Guid>**|  
|**Tipo definito dall'utente (UDT)**|nessuno|La stessa classe associata al tipo definito dall'utente (UDT) nello stesso assembly o un assembly dipendente.|  
|**varbinary**|**SqlBytes, SqlBinary**|**Byte []**|  
|**varbinary(1), binary(1)**|**SqlBytes, SqlBinary**|**byte, byte [], Nullable\<byte>**|  
|**varchar**|nessuno|nessuno|  
|**xml**|**SqlXml**|nessuno|  
  
## <a name="automatic-data-type-conversion-with-out-parameters"></a>Conversione automatica dei tipi di dati con parametri Out  
 Un metodo CLR può restituire informazioni al codice o al programma chiamante contrassegnando un parametro di input con il modificatore **out** (Microsoft Visual C#) o **\<Out()> ByRef** (Microsoft Visual Basic) se il parametro di input è un tipo di dati CLR nel sistema. lo spazio dei nomi **Data. SqlTypes** e il programma chiamante specificano il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo di dati equivalente come parametro di input. una conversione del tipo viene eseguita automaticamente quando il metodo CLR restituisce il tipo di dati.  
  
 Il stored procedure CLR seguente, ad esempio, include un parametro di input del tipo di dati CLR **SqlInt32** contrassegnato con **out** (C#) o **\<Out()> ByRef** (Visual Basic):  
  
```csharp  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void PriceSum(out SqlInt32 value)  
{ ... }  
```  
  
```vb  
\<Microsoft.SqlServer.Server.SqlProcedure> _  
Public Shared Sub PriceSum( \<Out()> ByRef value As SqlInt32)  
...  
End Sub  
```  
  
 Dopo che l'assembly è stato compilato e creato nel database, il stored procedure viene creato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'istruzione Transact-SQL seguente, che specifica un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo di dati **int** come parametro di output:  
  
```  
CREATE PROCEDURE PriceSum (@sum int OUTPUT)  
AS EXTERNAL NAME TestStoredProc.StoredProcedures.PriceSum  
```  
  
 Quando viene chiamato il stored procedure CLR, il tipo di dati **SqlInt32** viene convertito automaticamente in un tipo di dati **int** e restituito al programma chiamante.  
  
 Non tutti i tipi di dati CLR possono essere convertiti automaticamente nei tipi dei dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] equivalenti tramite un parametro out. Nella seguente tabella vengono descritte queste eccezioni.  
  
|**Tipo di dati CLR (SQL Server)**|**Tipo di dati di SQL Server**|  
|-|-|  
|**Decimale**|SMALLMONEY|  
|**SqlMoney**|SMALLMONEY|  
|**Decimale**|money|  
|**DateTime**|smalldatetime|  
|**SQLDateTime**|smalldatetime|  
  
## <a name="change-history"></a>Cronologia modifiche  
  
|Contenuto aggiornato|  
|---------------------|  
|Aggiunta dei tipi **SqlGeography**, **SqlGeometry** e **SqlHierarchyId** alla tabella di mapping.|  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati di SQL Server in .NET Framework](../../relational-databases/clr-integration-database-objects-types-net-framework/sql-server-data-types-in-the-net-framework.md)  
  
  
