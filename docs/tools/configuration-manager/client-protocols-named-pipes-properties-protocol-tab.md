---
title: Protocolli client - Proprietà - Named pipe (scheda Protocollo)
description: Informazioni su come visualizzare o modificare la descrizione della pipe predefinita in Gestione configurazione SQL Server e su come connettersi a una pipe differente.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server], connecting to
- Named Pipes [SQL Server], default pipe
- client protocols [SQL Server]
ms.assetid: 30fbae62-2f2e-4d36-9c6e-3444fff68781
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: afcc04a5b48014843c5aedcf2c3d62fc9993fd39
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349859"
---
# <a name="client-protocols---named-pipes-properties-protocol-tab"></a>Protocolli client - Proprietà - Named pipe (scheda Protocollo)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  In Gestione configurazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usare la scheda **Protocollo** della finestra di dialogo **Proprietà - Named Pipes** per visualizzare o modificare la descrizione della pipe predefinita. Per connettersi a una pipe diversa, digitarne il nome nella casella **Pipe predefinita** . Per ulteriori informazioni sulle stringhe di connessione, vedere [Creating a Valid Connection String Using Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130)).  
  
## <a name="options"></a>Opzioni  
 **Pipe predefinita**  
 Specifica la pipe predefinita usata dalla libreria di rete Named Pipes per tentare la connessione all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in ascolto su: `\\.\pipe\sql\query`  
  
 Per connettersi alla pipe predefinita, immettere `sql\query`  
  
 **Enabled**  
 I valori possibili sono **Sì** e **No**.  
  
## <a name="see-also"></a>Vedere anche  
 [Scelta di un protocollo di rete](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
