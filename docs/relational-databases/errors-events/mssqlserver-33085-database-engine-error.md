---
description: MSSQLSERVER_33085
title: MSSQLSERVER_33085 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33085 (Database Engine error)
ms.assetid: c27b8d1d-668a-4ba8-8b61-25a5ebbc5485
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8e36e50b129b61bbe4fbf025c1febc35d6e9a5ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208405"
---
# <a name="mssqlserver_33085"></a>MSSQLSERVER_33085
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|33085|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SEC_CRYPTOPROVE_METHOD_CANNOT_FOUND|  
|Testo del messaggio|Impossibile trovare uno o più metodi nella libreria del provider del servizio di crittografia '%.* ls.'|  
  
## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è stato in grado di usare il provider del servizio di crittografia elencato nel messaggio di errore. Il provider del servizio di crittografia non supporta un metodo obbligatorio. Lo stato dell'errore indica il metodo non trovato.  
  
|State|Descrizione|  
|---------|---------------|  
|1|SqlCryptInitializeProvider|  
|2|SqlCryptFreeProvider|  
|3|SqlCryptOpenSession|  
|4|SqlCryptCloseSession|  
|5|SqlCryptGetProviderInfo|  
|6|SqlCryptGetNextAlgorithmId|  
|7|SqlCryptGetAlgorithmInfo|  
|8|SqlCryptCreateKey|  
|9|SqlCryptDropKey|  
|10|SqlCryptGetNextKeyId|  
|11|SqlCryptGetKeyInfoByKeyId|  
|12|SqlCryptGetKeyInfoByThumb|  
|13|SqlCryptGetKeyInfoByName|  
|14|SqlCryptExportKey|  
|15|SqlCryptImportKey|  
|16|SqlCryptEncrypt|  
|17|SqlCryptDecrypt|  
  
## <a name="user-action"></a>Azione dell'utente  
Per ulteriori informazioni, contattare il provider del servizio di crittografia.  
  
