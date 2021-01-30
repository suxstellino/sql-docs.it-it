---
description: Limitazioni dei parametri delle stored procedure
title: Limitazioni del parametro della stored procedure | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], stored procedures
ms.assetid: 8b804bcf-4cce-4e6f-aa45-00bab9ef9921
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c92ff03bf5cf8899706966169c9b3c770f64f57c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188960"
---
# <a name="stored-procedure-parameter-limitations"></a>Limitazioni dei parametri delle stored procedure
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Utilizzare invece il driver ODBC fornito da Oracle.  
  
 Quando si eseguono stored procedure Oracle che utilizzano 10 o più parametri di output, la chiamata stored procedure avrà esito negativo, causando un errore di violazione di accesso o di ActiveX Data Objects (ADO). Questo problema può verificarsi quando si utilizza Microsoft ODBC driver for Oracle con le versioni 8.0.4.0.0 e 8.0.4.0.4 del software client Oracle.  
  
 Per risolvere il problema, è necessario aggiornare il software client Oracle alla versione 8.0.4.2.0 o successiva. Per ulteriori informazioni sulle [patch](../../odbc/microsoft/oracle-software-patches.md), contattare Oracle Corporation.  
  
> [!NOTE]  
>  Questo problema non si verifica con la versione iniziale della versione del software client Oracle 8.0.3.0.0.
