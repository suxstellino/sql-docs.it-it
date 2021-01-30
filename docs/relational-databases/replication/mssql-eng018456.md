---
description: MSSQL_ENG018456
title: MSSQL_ENG018456 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018456 error
ms.assetid: 3daa8144-d81f-445a-b6c3-4bb3e9fd1526
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0afdbb3340cb0162da95c654d3d72026256c3a60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205629"
---
# <a name="mssql_eng018456"></a>MSSQL_ENG018456
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|18456|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Accesso non riuscito per l'utente '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Spiegazione  
 L'errore MSSQL_ENG018456 viene generato quando un tentativo di accesso non riesce. Se il messaggio di errore include l'account **distributor_admin** (Accesso non riuscito per l'utente 'distributor_admin'.) il problema riguarda un account utilizzato dalla replica. La replica crea un server remoto, **repl_distributor**, che consente la comunicazione tra il server di distribuzione e quello di pubblicazione. L'account di accesso **distributor_admin** è associato a questo server remoto e deve avere una password valida.  
  
## <a name="user-action"></a>Azione dell'utente  
 Verificare di avere specificato la password per questo account. Per altre informazioni, vedere [Sicurezza del database di distribuzione](../../relational-databases/replication/security/secure-the-distributor.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
