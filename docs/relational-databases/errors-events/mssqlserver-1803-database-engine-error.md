---
description: MSSQLSERVER_1803
title: MSSQLSERVER_1803 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1803 (Database Engine error)
ms.assetid: d4315390-82f1-4c4c-8d1b-1a4989537cca
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d351cbcab58df191f3de68d119c8a0d008f5be22
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196553"
---
# <a name="mssqlserver_1803"></a>MSSQLSERVER_1803
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1803|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|NO_SPACE|  
|Testo del messaggio|Istruzione CREATE DATABASE non riuscita. Per contenere una copia del database modello, il file primario deve essere di almeno %d MB.|  
  
## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un database eseguendo una copia del database modello. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rinomina quindi la copia e aumenta le dimensioni del nuovo database fino a quelle richieste. In questo caso l'utente ha tentato di creare un database di dimensioni inferiori rispetto al database modello. L'operazione non è riuscita perché le dimensioni del file di dati primario sono minori rispetto a quelle del database modello.  
  
## <a name="user-action"></a>Azione dell'utente  
Creare il database aumentando le dimensioni dei file di database. Ridurre quindi il database se si desidera utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o l'istruzione DBCC SHRINKDATABASE.  
  
