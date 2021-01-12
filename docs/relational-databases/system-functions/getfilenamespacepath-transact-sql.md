---
description: GetFileNamespacePath (Transact-SQL)
title: GetFileNamespacePath (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- GetFileNamespacePath
- GetFileNamespacePath_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetFileNamespacePath function
ms.assetid: b393ecef-baa8-4d05-a268-b2f309fce89a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ecda7d44603636ff12eef955dd83e7659cb9dc7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097488"
---
# <a name="getfilenamespacepath-transact-sql"></a>GetFileNamespacePath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il percorso UNC per un file o una directory in una tabella FileTable.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
<column-name>.GetFileNamespacePath(is_full_path, @option)  
```  
  
## <a name="arguments"></a>Argomenti  
 *Nome colonna*  
 Nome della colonna **FILE_STREAM** varbinary (max) in una tabella FileTable.  
  
 Il valore del *nome della colonna* deve essere un nome di colonna valido. Non può essere un'espressione o un valore convertito o di cui sia stato eseguito il cast da una colonna di un altro tipo di dati.  
  
 *is_full_path*  
 Espressione Integer che specifica se restituire un percorso relativo o assoluto. *is_full_path* possibile avere uno dei valori seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Restituisce il percorso relativo all'interno della directory a livello di database.<br /><br /> Si tratta del valore predefinito.|  
|**1**|Restituisce il percorso UNC completo, che inizia con `\\computer_name`.|  
  
 *\@Opzione*  
 Espressione Integer che definisce la formattazione del componente server del percorso. l' *\@ opzione* può avere uno dei valori seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**0**|Restituisce il nome del server convertito in formato NetBIOS, ad esempio:<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Si tratta del valore predefinito.|  
|**1**|Restituisce il nome del server senza conversione, ad esempio:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Restituisce il percorso completo del server, ad esempio:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Tipo restituito  
 **nvarchar(max)**  
  
 Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fa parte di un cluster di failover, il nome computer restituito come parte di questo percorso è il nome host virtuale per l'istanza cluster.  
  
 Quando il database appartiene a un gruppo di disponibilità Always On, la funzione **FileTableRootPath** restituisce il nome della rete virtuale (VNN) anziché il nome del computer.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Il percorso restituito dalla funzione **GetFileNamespacePath** è una directory logica o un percorso di file nel formato seguente:  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\...`  
  
 Questo percorso logico non corrisponde direttamente a un percorso NTFS fisico. Viene convertito nel percorso fisico dal driver di filtro file system di FILESTREAM e dall'agente FILESTREAM. La separazione tra percorso logico e percorso fisico consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di riorganizzare i dati internamente senza incidere sulla validità del percorso.  
  
## <a name="best-practices"></a>Procedure consigliate  
 Per mantenere il codice e le applicazioni indipendenti dal database e dal computer correnti, evitare di scrivere codice basato su percorsi di file assoluti. Ottenere invece il percorso completo di un file in fase di esecuzione usando le funzioni **FileTableRootPath** e **GetFileNamespacePath** insieme, come illustrato nell'esempio seguente. Per impostazione predefinita, la funzione **GetFileNamespacePath** restituisce il percorso relativo del file all'interno del percorso radice per il database.  
  
```sql  
USE MyDocumentDatabase;  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
  
@fullPath = varchar(1000);  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath() FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="examples"></a>Esempi  
 Negli esempi seguenti viene illustrato come chiamare la funzione **GetFileNamespacePath** per ottenere il percorso UNC per un file o una directory in una tabella FileTable.  
  
```  
-- returns the relative path of the form "\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath() AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
  
-- returns "\\MyServer\MSSQLSERVER\MyDocumentDatabase\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath(1, Null) AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Usare directory e percorsi in FileTable](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
