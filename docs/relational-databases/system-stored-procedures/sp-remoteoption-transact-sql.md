---
description: sp_remoteoption (Transact-SQL)
title: sp_remoteoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_remoteoption_TSQL
- sp_remoteoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_remoteoption
ms.assetid: c9a7309b-eab7-4192-a414-e282581af4e5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aff4a43e54c6bbb8e6c406d6b48452fc84915e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185648"
---
# <a name="sp_remoteoption-transact-sql"></a>sp_remoteoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza o modifica le opzioni di un account di accesso remoto definito nel server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)]La stored procedure sp_remoteoption non modifica alcuna opzione e restituisce un messaggio di errore. È supportata solo per motivi di compatibilità con le versioni precedenti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_remoteoption [ [ @remoteserver = ] 'remoteserver' ]   
     [ , [ @loginame = ] 'loginame' ]   
     [ , [ @remotename = ] 'remotename' ]   
     [ , [ @optname = ] 'optname' ]   
     [ , [ @optvalue = ] 'optvalue' ]  
```  
  
## <a name="remarks"></a>Osservazioni  
 Questa stored procedure restituisce il messaggio di errore seguente:  
  
 `The trusted option in remote login mapping is no longer supported.`  
  
## <a name="see-also"></a>Vedere anche  
 [Server collegati &#40;Motore di database&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)  
  
  
