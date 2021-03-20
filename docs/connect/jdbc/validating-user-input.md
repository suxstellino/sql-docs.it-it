---
description: Informazioni sul motivo per cui la convalida dell'input utente è fondamentale per proteggere l'applicazione da attacchi SQL injection.
title: Convalida dell'input utente
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8aa867b0-e6f0-49eb-93d3-817ae2ed8f77
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 00aa7d14354ca29859806611550540674a798e48
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673935"
---
# <a name="validating-user-input"></a>Convalida dell'input utente

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Quando si costruisce un'applicazione che accede ai dati, è necessario presupporre che ogni input utente possa essere dannoso fino a quando non viene dimostrato il contrario. In caso contrario, si espone l'applicazione a potenziali attacchi. Uno tipo di attacco che può verificarsi è detto attacco intrusivo nel codice SQL e in questo tipo di attacco il codice dannoso viene aggiunto a stringhe che successivamente vengono passate a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per essere analizzate ed eseguite. Per evitare questo tipo di attacco, è necessario utilizzare stored procedure con parametri, dove possibile, e convalidare sempre l'input utente.

La convalida dell'input utente nel codice client è importante per evitare di eseguire round trip al server non necessari. È altrettanto importante convalidare i parametri delle stored procedure nel server per intercettare l'input non valido e che ignora la convalida sul lato client.

Per altre informazioni su SQL injection e su come evitarlo, vedere [SQL injection](../../relational-databases/security/sql-injection.md). Per ulteriori informazioni sulla convalida dei parametri di stored procedure, vedere [stored procedure](../../relational-databases/stored-procedures/stored-procedures-database-engine.md) e articoli correlati.

## <a name="see-also"></a>Vedere anche

[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)
