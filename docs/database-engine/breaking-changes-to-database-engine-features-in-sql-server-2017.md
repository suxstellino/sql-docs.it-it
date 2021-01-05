---
title: 'Motore di database: Modifiche che causano interruzioni | Microsoft Docs'
titleSuffix: SQL Server 2017
description: Informazioni sulle modifiche che potrebbero comportare l'interruzione di applicazioni, script o funzionalità basati su versioni precedenti di SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- breaking changes 2017 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: affc62db57e4ae08e1886af0fef4ab537bcda86b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97665887"
---
# <a name="breaking-changes-to-database-engine-features-in-sssqlv14-md"></a>Modifiche di rilievo apportate alle funzionalità del motore di database in [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]


  Questo argomento descrive le modifiche che causano interruzioni introdotte nel [!INCLUDE[ssDE](../includes/ssde-md.md)] di [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]. Tali modifiche potrebbero interrompere il funzionamento di applicazioni, funzionalità o script basati su versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. È possibile che questi problemi si verifichino quando viene effettuato un aggiornamento.  
  
## <a name="breaking-changes-in-sssqlv14-md-ssde"></a>Modifiche che causano interruzioni nel [!INCLUDE[ssDE](../includes/ssde-md.md)] di [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]  
  
-  CLR usa la Sicurezza dall'accesso di codice (CAS, Code Access Security) in .NET Framework, non più supportata come limite di sicurezza. A partire da [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)][!INCLUDE[ssDE](../includes/ssde-md.md)], viene introdotta un'opzione `sp_configure` denominata `clr strict security` per migliorare la sicurezza degli assembly CLR. CLR strict security è abilitata per impostazione predefinita e considera gli assembly CLR `SAFE` e `EXTERNAL_ACCESS` come se fossero contrassegnati `UNSAFE`. È possibile disabilitare l'opzione `clr strict security` per la compatibilità con le versioni precedenti, ma questa operazione è sconsigliata. Quando `clr strict security` è disabilitata, un assembly CLR creato con `PERMISSION_SET = SAFE` potrebbe essere in grado di accedere alle risorse di sistema esterne, chiamare codice non gestito e acquisire privilegi **sysadmin**. Dopo l'abilitazione di strict security, tutti gli assembly non firmati non verranno caricati. Inoltre, se il database dispone di assembly `SAFE` o `EXTERNAL_ACCESS`, le istruzioni `RESTORE` o `ATTACH DATABASE` possono essere completate ma gli assembly potrebbero non essere caricati.   
  Per caricare gli assembly, è necessario modificare oppure eliminare e ricreare ogni assembly in modo che sia firmato con un certificato o una chiave asimmetrica con un account di accesso corrispondente con l'autorizzazione `UNSAFE ASSEMBLY` nel server. Per altre informazioni, vedere [CLR strict security](../database-engine/configure-windows/clr-strict-security.md). 
  
-  Gli algoritmi MD2, MD4, MD5, SHA e SHA1 sono deprecati in [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]. Fino a [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], un certificato autofirmato viene creato con SHA1. A partire da [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], un certificato autofirmato viene creato usando SHA2_256.

## <a name="previous-versions"></a><a name="previous-versions"></a> Versioni precedenti  

- [Modifiche di rilievo apportate alle funzionalità del Motore di database in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)

- [Modifiche di rilievo apportate alle funzionalità del motore di database in SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?view=sql-server-2014&preserve-view=true#SQL14)

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Documentazione archiviata per versioni molto vecchie di SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>Vedere anche  
 [Funzionalità del motore di database deprecate in SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Funzionalità del motore di database non più usate in SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Compatibilità con le versioni precedenti del motore di database di SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
