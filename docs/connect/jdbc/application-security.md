---
description: Informazioni sulle autorizzazioni per la sicurezza delle applicazioni e i criteri Java quando si sviluppa un'applicazione mediante il driver JDBC.
title: Sicurezza delle applicazioni
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 940879b4-aa0f-41ce-a369-6cfc0e78e01d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f4785c39dc1d3fa9921a7191c9c3d338c7a655c2
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177017"
---
# <a name="application-security"></a>Sicurezza delle applicazioni

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Quando si utilizza [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] , è importante prendere le precauzioni necessarie per garantire la sicurezza dell'applicazione. Le sezioni seguenti forniscono informazioni sui passaggi che è possibile eseguire per proteggere l'applicazione.

## <a name="using-java-policy-permissions"></a>Uso di autorizzazioni basate sui criteri Java

Quando si utilizza [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] , è importante specificare le autorizzazioni dei criteri Java necessarie richieste dal driver JDBC. Il Java Runtime Environment (JRE) fornisce un modello di sicurezza completo. Tale modello può essere utilizzato in fase di esecuzione per determinare se un thread ha accesso a una risorsa. L'accesso può essere gestito grazie ai file dei criteri di sicurezza, I file dei criteri vengono gestiti dal deployer e dal ruolo sysadmin per il contenitore. Le autorizzazioni elencate in questo articolo sono quelle che interessano il funzionamento del driver JDBC.

Di seguito è riportato un esempio di autorizzazione tipica disponibile nel file dei criteri:

```config
// Example policy file entry.
grant [signedBy <signer>,] [codeBase <code source>] {
   permission  <class>  [<name> [, <action list>]];
};
```

 La codebase seguente deve essere limitata alla codebase del driver JDBC per assicurarsi di concedere il numero minimo di privilegi.

```config
grant codeBase "file:/install_dir/lib/-" {

// Grant access to data source.
permission java.util.PropertyPermission "java.naming.*", "read,write";

// Specify which hosts can be connected to.
permission java.net.socketPermission "host:port", "connect";

// Logger permission to take advantage of logging.
permission java.util.logging.LoggingPermission;

// Grant listen/connect/accept permissions to the driver if
// connecting to a named instance as the client driver.
// This connects to a udp service and listens for a response.
permission java.net.SocketPermission "*", "listen, connect, accept";
};
```

> [!NOTE]
> Il codice "file:/install_dir/lib/-" fa riferimento alla directory di installazione del driver JDBC.

## <a name="protecting-server-communication"></a>Protezione delle comunicazioni con il server

Quando si utilizza il driver JDBC per comunicare con un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database, è importante proteggere il canale di comunicazione. È possibile proteggere il canale utilizzando Internet Protocol Security (IPSEC) o Transport Layer Security (TLS), noto in precedenza come Secure Sockets Layer (SSL), oppure è possibile utilizzare entrambi.

Il supporto TLS può essere utilizzato per fornire un livello aggiuntivo di protezione oltre a IPSEC. Per altre informazioni sull'uso di TLS, vedere [Uso della crittografia](using-ssl-encryption.md).

## <a name="see-also"></a>Vedere anche

[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)
