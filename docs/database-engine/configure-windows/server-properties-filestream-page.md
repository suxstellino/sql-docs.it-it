---
title: Proprietà - SQL Server (pagina FILESTREAM) | Microsoft Docs
description: Acquisire familiarità con le impostazioni FILESTREAM in SQL Server. Informazioni su come attivare FILESTREAM e vedere come configurare l'accesso client remoto e altre proprietà.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.filestream.f1
helpviewer_keywords:
- FILESTREAM [SQL Server], properties page
ms.assetid: 8a8d38d3-e97a-4b09-a40b-659b2e3a5c47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bde5d28db431bb99e1721ee3984496dd10e6cf1c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764808"
---
# <a name="server-properties---filestream-page"></a>Proprietà server - pagina FILESTREAM
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa pagina per abilitare FILESTREAM per l'installazione corrente di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="ui-element-list"></a>Elenco di elementi dell'interfaccia utente  
 **Abilita FILESTREAM per l'accesso Transact-SQL**  
 Selezionare questa opzione per abilitare FILESTREAM per l'accesso [!INCLUDE[tsql](../../includes/tsql-md.md)] . È necessario che questo controllo sia selezionato affinché le altre opzioni di controllo siano disponibili.  
  
 **Abilita FILESTREAM per l'accesso tramite il flusso di I/O dei file**  
 Selezionare questa opzione per abilitare l'accesso tramite flusso Win32 per FILESTREAM.  
  
 **Nome condivisione di Windows**  
 Utilizzare questo controllo per immettere il nome della condivisione di Windows in cui verranno archiviati i dati FILESTREAM.  
  
 **Consenti ai client remoti l'accesso tramite flusso ai dati FILESTREAM**  
 Selezionare questo controllo per consentire ai client remoti di accedere ai dati FILESTREAM nel server corrente.  
  
## <a name="see-also"></a>Vedere anche  
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
