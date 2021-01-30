---
description: HOST_NAME (Transact-SQL)
title: HOST_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HOST_NAME_TSQL
- HOST_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- HOST_NAME function
- workstation names [SQL Server]
ms.assetid: 4b8b0705-c083-4b07-b954-c83ee73b2ebb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2cb8c8cc0b9945ba3ebc4b4c49ca9872a50469fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203574"
---
# <a name="host_name-transact-sql"></a>HOST_NAME (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Restituisce il nome della workstation.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
HOST_NAME ()  
```  

## <a name="return-types"></a>Tipi restituiti
 **nvarchar(128)**  
  
## <a name="remarks"></a>Osservazioni  
 Se il parametro per una funzione di sistema è facoltativo, vengono utilizzati il database, il computer host, l'utente del server o l'utente del database correnti. Le funzioni predefinite devono essere sempre seguite da parentesi.  
  
 È possibile utilizzare funzioni di sistema nell'elenco di selezione, nella clausola WHERE e in tutti i casi in cui è consentita un'espressione.  
  
> [!IMPORTANT]  
>  L'applicazione client fornisce il nome della workstation e può indicare dati non accurati. Non considerare HOST_NAME una caratteristica di sicurezza.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creata una tabella che utilizza `HOST_NAME()` in una definizione `DEFAULT` per registrare il nome di workstation dei computer in cui vengono inserite righe in una tabella di registrazione degli ordini.  
  
```sql  
CREATE TABLE Orders  
   (OrderID     INT        PRIMARY KEY,  
    CustomerID  NCHAR(5)   REFERENCES Customers(CustomerID),  
    Workstation NCHAR(30)  NOT NULL DEFAULT HOST_NAME(),  
    OrderDate   DATETIME   NOT NULL,  
    ShipDate    DATETIME   NULL,  
    ShipperID   INT        NULL REFERENCES Shippers(ShipperID));  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
