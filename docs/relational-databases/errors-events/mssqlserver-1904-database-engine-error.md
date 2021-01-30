---
description: MSSQLSERVER_1904
title: MSSQLSERVER_1904 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1904 (Database Engine error)
ms.assetid: 2a35d57d-74e2-45a2-8f67-3f2e51d69712
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 198fe8af27532157397b32875882a072f26f6f4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196475"
---
# <a name="mssqlserver_1904"></a>MSSQLSERVER_1904
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1904|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|KEYCOUNT|  
|Testo del messaggio|Per l'oggetto %S_MSG '%.*ls' nella tabella '%.\*ls' sono presenti %d nomi di colonna nell'elenco chiavi %S_MSG. Il limite massimo per l'elenco delle colonne chiave per indici o statistiche è di %d.|  
  
## <a name="explanation"></a>Spiegazione  
Il numero di colonne chiave nell'elenco per le statistiche o l'indice specificato supera il numero massimo di colonne consentite.  
  
## <a name="user-action"></a>Azione dell'utente  
Modificare l'elenco delle colonne chiave in modo da non includere un numero di colonne superiore a quello specificato.  
  
Per gli indici non cluster, utilizzare la clausola INCLUDE nell'istruzione CREATE INDEX per aggiungere colonne all'indice come colonne non chiave. In questo modo si evita di superare la limitazione relativa alla dimensione di indice corrente che consente al massimo 16 colonne chiave. Per altre informazioni, vedere [Creare indici con colonne incluse](~/relational-databases/indexes/create-indexes-with-included-columns.md).  
  
## <a name="see-also"></a>Vedere anche  
[CREATE INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](~/t-sql/statements/create-statistics-transact-sql.md)  
  
