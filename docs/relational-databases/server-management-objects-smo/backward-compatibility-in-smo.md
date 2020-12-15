---
description: Compatibilità con le versioni precedenti in SMO
title: Compatibilità con le versioni precedenti in SMO | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: 2f986436-aaf2-4eaf-9809-df849d97d4fb
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a6e36a0966f631138768c2c4b17ab7444df76aea
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463072"
---
# <a name="backward-compatibility-in-smo"></a>Compatibilità con le versioni precedenti in SMO
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Le applicazioni SMO create utilizzando le versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere comunque ricompilate in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]tramite SMO.  
  
## <a name="migrating-smo-applications"></a>Migrazione di applicazioni SMO  
 È necessario che i riferimenti alle DLL SMO nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengano rimossi e che vengano inclusi invece i riferimenti alle nuove DLL SMO disponibili in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
 Vengono utilizzati in genere i riferimenti agli elementi seguenti:  
  
-   Microsoft.SqlServer.ConnectionInfo  
  
-   Microsoft.SqlServer.Smo  
  
-   Microsoft.SqlServer.Management.Sdk.Sfc  
  
 Tali file sono necessari per le classi di connessione, le classi di utilità SMO e le classi della libreria Microsoft Foundation Class.  
  
> [!NOTE]  
>  Poiché SmoEnum.dll è stato rimosso, è necessario che i riferimenti a tale file vengono rimossi dal progetto [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SMO.  
  
 Lo spazio dei nomi è stato modificato ed è pertanto necessario utilizzare gli elementi seguenti:  
  
##### <a name="for-visual-c"></a>Per Visual C#  
  
```  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Common;  
```  
  
##### <a name="for-visual-basic"></a>Per Visual Basic  
  
```  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Common  
```  
  
 Se nel codice viene utilizzata la funzionalità URN, ad esempio **Server.GetSqlSmoObject(Urn)**, è necessario collegarsi allo spazio dei nomi Microsoft.SqlServer.Management.Sdk.Sfc.  
  
 Se nel codice viene utilizzato direttamente l'oggetto Transfer, è necessario collegarsi allo spazio dei nomi Microsoft.SqlServer.Management.SmoExtended.  
  
 Potrebbe necessario modificare il codice quando si esegue la relativa migrazione, poiché numerose caratteristiche di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] sono deprecate in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Per ulteriori informazioni sulle funzionalità deprecate, vedere [deprecated motore di database features in SQL Server 2016](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md) nella [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] documentazione online di.  
  
  
