---
title: Nuovo alias (scheda Alias)
description: Informazioni su come creare un alias, un nome alternativo per un'istanza di SQL Server usato per la connessione a tale istanza. Vedere esempi dei casi d'uso di un alias.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 785eb6fb-f67e-449d-b1c8-c38dfbb95ef6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d2a34ab643359f7afca304b64328638675f8a742
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349770"
---
# <a name="new-alias-alias-tab"></a>Nuovo alias (scheda Alias)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Un alias rappresenta un nome alternativo che può essere utilizzato per stabilire una connessione. L'alias incapsula gli elementi necessari di una stringa di connessione e li espone con un nome scelto dall'utente. La pagina **Alias** della finestra di dialogo **Nuovo alias** consente di specificare gli elementi della stringa di connessione per un alias. Per modificare la stringa di connessione di un alias esistente, vedere [Proprietà &#60;Alias&#62; &#40;scheda Alias&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md).  
  
 Non è necessario completare tutti i valori nella griglia **Proprietà** . Le combinazioni valide variano a seconda del protocollo selezionato. Per esempi di combinazioni valide, vedere gli argomenti elencati di seguito.  
  
 **Nome alias**  
 Nome o alias che si desidera utilizzare per fare riferimento alla connessione.  
  
 **Nome pipe** / **Numero porta**  
 Elementi aggiuntivi della stringa di connessione. Il nome di questa casella varia a seconda del protocollo selezionato.  
  
 **Protocollo**  
 Protocollo utilizzato per la connessione.  
  
 **Server**  
 Nome dell'istanza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si esegue la connessione.  
  
## <a name="when-to-use-an-alias"></a>Situazioni in cui utilizzare un alias  
 Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si connette a un'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante il protocollo **Shared Memory** e a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un altro computer mediante **TCP/IP** o **Named Pipes**. Creare un alias quando si utilizza TCP/IP o Named Pipes e si desidera fornire una stringa di connessione personalizzata o quando si desidera utilizzare un nome diverso da quello del server per la connessione.  
  
### <a name="examples"></a>Esempi  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in attesa sulla porta TCP/IP 1433 predefinita, pertanto si desidera creare una stringa di connessione con un numero di porta diverso.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in attesa sulla porta named pipe predefinita, pertanto si desidera creare una stringa di connessione con un nome di pipe diverso.  
  
-   Un'applicazione prevede di connettersi a un database nel server denominato `ACCT`, ma tale database è stato consolidato come istanza denominata `ACCT` nel server `CENTRAL`. Non è semplice modificare l'applicazione. Creare un alias denominato `ACCT`, con una stringa di connessione che punta a `CENTRAL\ACCT`.  
  
## <a name="creating-a-valid-connection-string"></a>Creazione di una stringa di connessione valida  
 Per una descrizione ed esempi di combinazioni valide di proprietà di alias, vedere gli argomenti seguenti:  
  
-   [Creazione di una stringa di connessione valida mediante il protocollo di memoria condivisa](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
-   [Creazione di una stringa di connessione valida con TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
-   [Creazione di una stringa di connessione valida tramite named pipe](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
