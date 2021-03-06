---
description: catalog.check_schema_version
title: catalog.check_schema_version | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa47e5107551ec2d4ded0832ff52922ffb4bfd55
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247526"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Determina se lo schema del catalogo SSISDB e i file binari (assembly ISServerExec e SQLCLR) di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sono compatibili.  
  
 ISServerExec.exc registra un messaggio di errore quando lo schema e i file binari sono incompatibili.  
  
 La versione dello schema SSISDB viene incrementata quando lo schema cambia durante l'applicazione di patch e aggiornamenti. È consigliabile eseguire questa stored procedure dopo il ripristino di un backup di SSISDB per assicurarsi che lo schema e i file binari siano compatibili.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @use32bitruntime= ] *use32bitruntime*  
 Quando il parametro è impostato su **1**, viene chiamata la versione a 32 bit di dtexec. *use32bitruntime* è di tipo **int**.  
  
 
## <a name="return-code-value"></a>Valore del codice restituito 
Restituisce 0 per l'esito positivo. 

## <a name="result-set"></a>Set di risultati  

Viene restituita una tabella con il formato seguente:

| Nome colonna | Tipo di dati | Descrizione |
|---|---|---|
| SERVER_BUILD | **decimal** | SQL Server versione. Ad esempio, un server che esegue SQL Server 2014 è `14.0.3335.7` . |
| SCHEMA_VERSION | **tinyint** | SQL Server numero di versione. Ad esempio, SQL Server 2017 e 2019 sono `6` `7` rispettivamente e.|
| SCHEMA_BUILD | **string** | Compilazione dello schema. |
| ASSEMBLY_BUILD | **string** | Compilazione assembly. |
| SHARED_COMPONENT_VERSION | **string** | Versione del componente condiviso. | 

## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria l'autorizzazioni seguente:  
  
-   Appartenenza al ruolo del database **ssis_admin**.  
  
  
