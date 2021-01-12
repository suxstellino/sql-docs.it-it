---
description: Viste dell'applicazione livello dati-dbo.sysdac_instances
title: dbo.sysdac_instances (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysdac_instances_TSQL
- sysdac_instances
- sysdac_instances_TSQL
- dbo.sysdac_instances
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.sysdac_instances
- sysdac_instances
ms.assetid: 28285f3d-3889-439f-8b24-3bdef08e46b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5178fdb50b8a959471c8be973080d1416ba10f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092666"
---
# <a name="data-tier-application-views---dbosysdac_instances"></a>Viste dell'applicazione livello dati-dbo.sysdac_instances
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di visualizzare una riga per ogni istanza di applicazione livello dati distribuita in un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. sysdac_instances appartiene allo schema dbo nel database msdb. Nella tabella seguente vengono descritte le colonne nella vista sysdac_instances.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|Identificatore dell'istanza di applicazione livello dati.|  
|nome_istanza|**sysname**|Nome dell'istanza di applicazione livello dati specificata alla distribuzione dell'applicazione livello dati.|  
|type_name|**sysname**|Nome dell'applicazione livello dati specificata alla creazione del pacchetto di applicazione livello dati.|  
|type_version|**nvarchar (64)**|Versione dell'applicazione livello dati specificata alla creazione del pacchetto di applicazione livello dati.|  
|description|**nvarchar(4000)**|Descrizione dell'applicazione livello dati scritta alla creazione del pacchetto di applicazione livello dati.|  
|type_stream|**varbinary(max)**|Flusso di bit contenente una rappresentazione codificata degli oggetti logici, quali tabelle e viste, contenuti nell'applicazione livello dati.|  
|date_created|**datetime**|Data e ora di creazione dell'istanza di applicazione livello dati.|  
|created_by|**sysname**|Account di accesso utilizzato per la creazione dell'istanza di applicazione livello dati.|  
|database_name|**sysname**|Nome del database creato per l'istanza di applicazione livello dati.|  
  
## <a name="remarks"></a>Osservazioni  
 Un'applicazione livello dati include un tipo di applicazione livello dati, ovvero una definizione degli oggetti di livello dati logici utilizzati da un'applicazione, quali tabelle e viste. Un pacchetto DAC è un file utilizzato per la distribuzione di DAC. Il pacchetto di applicazione livello dati contiene una rappresentazione di tutti gli oggetti logici contenuti nel tipo di applicazione livello dati. Il pacchetto di applicazione livello dati può essere utilizzato per la distribuzione di una o più copie, o istanze, dell'applicazione livello dati in un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Ogni istanza di applicazione livello dati distribuita dallo stesso pacchetto di applicazione livello dati condivide lo stesso tipo, ma è associata a un nome di istanza e a un identificatore univoci.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per visualizzare tutte le colonne è necessaria l'appartenenza al ruolo predefinito del server sysadmin. I membri del ruolo public possono visualizzare le colonne instance_name, description e type_version.  
  
## <a name="see-also"></a>Vedere anche  
 [Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Viste dell'applicazione livello dati &#40;&#41;Transact-SQL ]()  
  
