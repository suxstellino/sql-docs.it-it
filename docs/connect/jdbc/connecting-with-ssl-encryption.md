---
description: Trovare esempi di come connettersi usando la crittografia TLS nell'applicazione Java usando il driver JDBC per SQL Server.
title: Connessione tramite la crittografia
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ec91fa8a-ab7e-4c1e-a05a-d7951ddf33b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbefb54c4af48706d04e4081d8e3a6c25ec8e21f
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177048"
---
# <a name="connecting-with-encryption"></a>Connessione tramite la crittografia

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Gli esempi in questo articolo descrivono come usare le proprietà della stringa di connessione che consentono alle applicazioni di usare la crittografia TLS (Transport Layer Security) in un'applicazione Java. Per altre informazioni sulle nuove proprietà della stringa di connessione, ad esempio **encrypt**, **trustServerCertificate**, **trustStore**, **trustStorePassword** e **hostNameInCertificate**, vedere [Impostazione delle proprietà delle connessioni](setting-the-connection-properties.md).

## <a name="configuring-the-connection"></a>Configurazione della connessione

Se la proprietà **encrypt** è impostata su **true** e la proprietà **trustServerCertificate** è impostata su **true**, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] non convalida il certificato TLS di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa impostazione è comune per consentire le connessioni in ambienti di test, ad esempio se l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza dispone solo di un certificato autofirmato.

L'esempio di codice seguente illustra come impostare la proprietà **trustServerCertificate** in una stringa di connessione:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true;trustServerCertificate=true";
```

Se la proprietà **encrypt** è impostata su **true** e la proprietà **trustServerCertificate** è impostata su **false**, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] convaliderà il certificato TLS di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La convalida del certificato del server fa parte dell'handshake TLS e assicura che il server a cui si esegue la connessione sia quello corretto. Per la convalida del certificato del server è necessario che in fase di connessione venga fornito il materiale relativo all'attendibilità usando in modo esplicito le proprietà di connessione **trustStore** e **trustStorePassword** oppure usando in modo implicito l'archivio di scopi predefinito di JVM (Java Virtual Machine) sottostante.

La proprietà **trustStore** specifica il percorso (incluso il nome file) del file trustStore, che contiene l'elenco di certificati considerati attendibili dal client. La proprietà **trustStorePassword** specifica la password usata per verificare l'integrità dei dati del file trustStore. Per ulteriori informazioni sull'utilizzo dell'archivio di attendibilità predefinito di JVM, vedere la pagina relativa alla [configurazione del client per la crittografia](configuring-the-client-for-ssl-encryption.md).

L'esempio di codice seguente illustra come impostare le proprietà **trustStore** e **trustStorePassword** in una stringa di connessione:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true; trustServerCertificate=false;" +
     "trustStore=storeName;trustStorePassword=storePassword";
```

Il driver JDBC fornisce un'altra proprietà, **hostNameInCertificate**, che specifica il nome host del server. Il valore di questa proprietà deve corrispondere alla proprietà relativa al soggetto del certificato.

L'esempio di codice seguente illustra come usare la proprietà **hostNameInCertificate** in una stringa di connessione:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true; trustServerCertificate=false;" +
     "trustStore=storeName;trustStorePassword=storePassword" +
     "hostNameInCertificate=hostName";
```

> [!NOTE]
> In alternativa, è possibile impostare il valore delle proprietà di connessione usando i metodi **setter** appropriati offerti dalla classe [SQLServerDataSource](reference/sqlserverdatasource-class.md).

Se la proprietà **Encrypt** è **true** e la proprietà **TrustServerCertificate** è **false** e il nome del server nella stringa di connessione non corrisponde al nome del server nel certificato TLS, verrà generato l'errore seguente: `The driver couldn't establish a secure connection to SQL Server by using Secure Sockets Layer (SSL) encryption. Error: "java.security.cert.CertificateException: Failed to validate the server name in a certificate during Secure Sockets Layer (SSL) initialization."` . Con la versione 7,2 e superiore, il driver supporta i criteri di ricerca con caratteri jolly nell'etichetta a sinistra del nome del server nel certificato TLS.

## <a name="see-also"></a>Vedere anche

[Uso della crittografia](using-ssl-encryption.md)  
[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)  
