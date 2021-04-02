---
description: Informazioni sul motivo per cui la convalida dell'input utente è fondamentale per proteggere l'applicazione da attacchi SQL injection.
title: Convalida dell'input utente
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8aa867b0-e6f0-49eb-93d3-817ae2ed8f77
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1683f99b6c9d188a45434304251168748af32582
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177047"
---
# <a name="validating-user-input"></a>Convalida dell'input utente

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Quando si costruisce un'applicazione che accede ai dati, è necessario presupporre che ogni input utente possa essere dannoso fino a quando non viene dimostrato il contrario. In caso contrario, si espone l'applicazione a potenziali attacchi. Un tipo di attacco che può verificarsi è denominato SQL injection. Questo attacco è il punto in cui il codice dannoso viene aggiunto alle stringhe che vengono passate a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per essere analizzate ed eseguite. Per evitare questo tipo di attacco, è necessario utilizzare stored procedure con parametri, dove possibile, e convalidare sempre l'input utente.

La convalida dell'input utente nel codice client è importante in modo da non sprecare i round trip al server. È ugualmente importante convalidare i parametri per le stored procedure nel server. In questo modo viene rilevato un input che ignora la convalida lato client.

Per altre informazioni su SQL injection e su come evitarlo, vedere [SQL injection](../../relational-databases/security/sql-injection.md). Per ulteriori informazioni sulla convalida dei parametri di stored procedure, vedere [stored procedure](../../relational-databases/stored-procedures/stored-procedures-database-engine.md) e articoli correlati.

## <a name="see-also"></a>Vedere anche

[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)
