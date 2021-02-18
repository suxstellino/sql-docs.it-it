---
title: Proprietà Alias (scheda Alias)
description: Usare la scheda Alias della finestra di dialogo Proprietà per configurare un alias e usare così un nome alternativo per la connessione a un'istanza di SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2d1498e2-129c-4ce7-88e5-408e4037243c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 510ee3ceb24f2e61e414282903fd8b9dee667632
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349942"
---
# <a name="ltaliasgt-properties-alias-tab"></a>Proprietà &lt;Alias&gt; (scheda Alias)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Un alias rappresenta un nome alternativo che può essere utilizzato per stabilire una connessione. L'alias incapsula gli elementi necessari di una stringa di connessione e li espone con un nome scelto dall'utente. Usare la pagina **Alias** nella finestra di dialogo **Proprietà \<**Alias name**>** per visualizzare o specificare gli elementi della stringa di connessione di un alias.

## <a name="options"></a>Opzioni

**Nome alias**

Nome o alias che si desidera utilizzare per fare riferimento alla connessione.  

**Nome pipe** / **Numero porta**  

Elementi aggiuntivi della stringa di connessione. Il nome di questa casella varia a seconda del protocollo selezionato. Per esempi, vedere gli argomenti elencati di seguito.  

**Protocollo**

Protocollo utilizzato per la connessione.

**Server**

Nome dell'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si esegue la connessione.  

## <a name="see-also"></a>Vedere anche

- [Creazione di una stringa di connessione valida mediante il protocollo di memoria condivisa](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)
- [Creazione di una stringa di connessione valida con TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)
- [Creazione di una stringa di connessione valida tramite named pipe](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))