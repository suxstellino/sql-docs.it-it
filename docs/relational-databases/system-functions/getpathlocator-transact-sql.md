---
description: GetPathLocator (Transact-SQL)
title: GetPathLocator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- GetPathLocator
- GetPathLocator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetPathLocator function
ms.assetid: 78b7e220-445b-4fdf-811b-7253f4f2b058
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fbdf65d45374fe9c85adaade284ac6a8aab65dbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207460"
---
# <a name="getpathlocator-transact-sql"></a>GetPathLocator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il valore dell'ID localizzatore del percorso per la directory o il file specificato in una tabella FileTable.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
GetPathLocator(filenamespace_path)  
```  
  
## <a name="arguments"></a>Argomenti  
 *filenamespace_path*  
 Percorso dello spazio dei nomi nella tabella FileTable. Il percorso dello spazio dei nomi è di tipo **nvarchar (max)**.  
  
 Quando il database appartiene a un gruppo di disponibilità Always On, la funzione **GetPathLocator** accetta il nome della rete virtuale (VNN) o il nome del computer.  
  
## <a name="return-type"></a>Tipo restituito  
 **hierarchyid**  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Per altre informazioni, vedere [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="examples"></a>Esempio  
 È possibile utilizzare la funzione **GetPathLocator** quando si esegue la migrazione di file da un file server a una tabella FileTable. In questo scenario si desidera spostare i file nella tabella FileTable, quindi sostituire il percorso UNC originale per ogni file con il percorso UNC della tabella FileTable. Per un esempio completo, vedere [caricare file in tabelle FileTable](../../relational-databases/blob/load-files-into-filetables.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Usare directory e percorsi in FileTable](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
