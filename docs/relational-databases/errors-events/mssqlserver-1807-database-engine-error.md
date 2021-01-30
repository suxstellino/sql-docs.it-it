---
description: MSSQLSERVER_1807
title: MSSQLSERVER_1807 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1807 (Database Engine error)
ms.assetid: 13c1b240-098b-4d9e-89aa-21599548e074
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c5a754d957291e3cb30c56e6c9b97b4201d7e45a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196544"
---
# <a name="mssqlserver_1807"></a>MSSQLSERVER_1807
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1807|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|CANNOT_EX_LOCK|  
|Testo del messaggio|Impossibile ottenere il blocco esclusivo sul database '%.*ls'. Ripetere l'operazione in un secondo momento.|  
  
## <a name="explanation"></a>Spiegazione  
Non è possibile ottenere l'accesso esclusivo al database necessario per l'esecuzione di un'operazione.  
  
## <a name="user-action"></a>Azione dell'utente  
Disconnettere tutte le connessioni al database oppure eseguire nuovamente la query in un secondo momento.  
  
