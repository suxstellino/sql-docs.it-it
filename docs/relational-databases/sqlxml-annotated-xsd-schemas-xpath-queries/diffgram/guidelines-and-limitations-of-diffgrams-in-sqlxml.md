---
description: Linee guida e limitazioni per i Diffgram in SQLXML
title: Linee guida e limitazioni per i Diffgram in SQLXML
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: cf8689c4-2a63-4d05-b202-21b5ff187d7f
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 737cbcef2cead359c4f4cfbab0a9d06e58405826
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491860"
---
# <a name="guidelines-and-limitations-of-diffgrams-in-sqlxml"></a>Linee guida e limitazioni per i Diffgram in SQLXML
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Quando si utilizzano Diffgram con SQLXML 4.0, tenere presenti le considerazioni seguenti:  
  
-   I tipi BLOB (Binary Large Object) come **text/ntext** e images non devono essere usati nel blocco in quando si usano i DiffGram, perché verranno inclusi per l'uso nel controllo della **\<diffgr:before>** concorrenza. Ciò può provocare problemi con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a causa delle limitazioni applicate al confronto per i tipi BLOB. Ad esempio, la parola chiave LIKE viene usata nella clausola WHERE per il confronto tra colonne del **tipo di** dati text. Tuttavia, i confronti avranno esito negativo nel caso di tipi BLOB in cui le dimensioni dei dati sono maggiori di 8.000.  
  
-   I caratteri speciali **nei dati ntext** possono causare problemi con SQLXML 4.0 a causa delle limitazioni relative al confronto per i tipi BLOB. Ad esempio, l'uso di "[Serializable]" nel blocco di un DiffGram quando viene usato nel controllo della concorrenza di una colonna di tipo ntext avrà esito negativo con la descrizione dell'errore **\<diffgr:before>** SQLOLEDB  seguente:  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
  
