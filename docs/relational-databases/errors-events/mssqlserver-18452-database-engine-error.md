---
description: MSSQLSERVER_18452
title: MSSQLSERVER_18452 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18456 (Database Engine error)
- 18452 (Database Engine error)
ms.assetid: 21da332c-e81d-4dee-a9d2-95598911b3be
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6bdfef25fcb7c159c22355d173182cc077d9b3a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196532"
---
# <a name="mssqlserver_18452"></a>MSSQLSERVER_18452
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|18452|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LOGON_INVALID_CONNECT|  
|Testo del messaggio|Accesso non riuscito per l'utente '%.*ls'. L'accesso è un accesso di SQL Server e non può essere usato con l'autenticazione di Windows %.\*ls|  
  
## <a name="explanation"></a>Spiegazione  
L'utente ha tentato di effettuare l'accesso con credenziali che non è possibile convalidare. Le cause possibili sono:  
  
-   L'account di accesso può essere un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ma il server accetta solo l'autenticazione di Windows.  
  
-   Si sta tentando di effettuare la connessione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ma l'account di accesso utilizzato non esiste in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L'account di accesso può utilizzare l'autenticazione di Windows ma è un'entità di Windows non riconosciuta. Un'entità di Windows non riconosciuta indica che l'account di accesso non può essere verificato da Windows. La causa potrebbe essere la provenienza dell'account di accesso di Windows da un dominio non attendibile.  
  
Problemi simili possono provocare l'errore 18456 meno specifico.  
  
## <a name="user-action"></a>Azione dell'utente  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], verificare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia configurato in modalità Autenticazione mista.  
  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare l'esistenza dell'account di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Se si sta tentando di effettuare la connessione mediante l'autenticazione di Windows, verificare di essere connessi al dominio corretto.  
  
