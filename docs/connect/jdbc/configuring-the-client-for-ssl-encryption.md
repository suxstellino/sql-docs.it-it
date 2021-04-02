---
title: Configurazione del client per la crittografia
description: Informazioni sulla crittografia lato client e l'attendibilità dei certificati per garantire la sicurezza dei client utilizzando Microsoft JDBC driver per SQL Server.
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ae34cd1f-3569-4759-80c7-7c9b33b3e9eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cc0d713f3e7b2f1cb508d1a6f366d7c1a97617e9
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106176957"
---
# <a name="configuring-the-client-for-encryption"></a>Configurazione del client per la crittografia

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Il [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] client di o deve verificare che il server sia il server corretto e che il certificato venga emesso da un'autorità di certificazione considerata attendibile dal client. Per convalidare il certificato del server, è necessario fornire il materiale attendibile al momento della connessione. Inoltre, l'emittente del certificato del server deve essere un'autorità di certificazione considerata attendibile dal client.

Questo articolo descrive innanzitutto come fornire il materiale attendibile nel computer client. Viene quindi descritto come importare un certificato del server nell'archivio di attendibilità del computer client quando l'istanza del certificato di Transport Layer Security (TLS) del SQL Server viene rilasciata da un'autorità di certificazione privata.

Per altre informazioni sulla convalida del certificato del server, vedere la sezione Convalida del certificato TLS del server in [Informazioni sul supporto della crittografia](../../connect/jdbc/understanding-ssl-support.md).

## <a name="configuring-the-client-trust-store"></a>Configurazione dell'archivio di attendibilità del client

La convalida del certificato del server richiede il materiale attendibile al momento della connessione utilizzando in modo esplicito le proprietà di connessione **trustStore** e **trustStorePassword** oppure utilizzando in modo implicito l'archivio di attendibilità predefinito del Java Virtual Machine sottostante (JVM). Per ulteriori informazioni su come impostare le proprietà **trustStore** e **trustStorePassword** in una stringa di connessione, vedere [Connecting with Encryption](connecting-with-ssl-encryption.md).

Se la proprietà **trustStore** non è specificata o è impostata su Null, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa il provider di sicurezza di JVM sottostante, ovvero Java Secure Socket Extension (SunJSSE). Il provider SunJSSE fornisce un oggetto TrustManager predefinito, usato per convalidare i certificati X.509 restituiti da SQL Server rispetto al materiale relativo all'attendibilità fornito in un archivio di attendibilità.

TrustManager prova a individuare il file trustStore predefinito nell'ordine di ricerca seguente:

- Se è definita la proprietà di sistema "javax.net.ssl.trustStore", TrustManager prova a individuare il file trustStore predefinito usando il nome file specificato dalla proprietà di sistema.
- Se la proprietà di sistema "javax. NET. SSL. trustStore" non è stata specificata e il file " \<java-home> /lib/security/jssecacerts" esiste, viene utilizzato tale file.
- Se il file " \<java-home> /lib/security/cacerts" esiste, viene utilizzato tale file.

Per ulteriori informazioni, vedere la documentazione relativa all'interfaccia TrustManager SUNX509 nel sito Web Sun Microsystems.

Java Runtime Environment consente di impostare le proprietà di sistema trustStore e trustStorePassword come indicato di seguito:

```java
java -Djavax.net.ssl.trustStore=C:\MyCertificates\storeName
java -Djavax.net.ssl.trustStorePassword=storePassword
```

In questo caso, in qualsiasi applicazione in esecuzione in questo ambiente JVM queste impostazioni vengono utilizzate come predefinite. Per eseguire l'override delle impostazioni predefinite nell'applicazione, è necessario impostare le proprietà di connessione **trustStore** e **trustStorePassword** nella stringa di connessione o nel metodo setter appropriato della classe [SQLServerDataSource](reference/sqlserverdatasource-class.md) .

Inoltre, è possibile configurare e gestire i file dell'archivio di attendibilità predefiniti, ad esempio " \<java-home> /lib/security/jssecacerts" e " \<java-home> /lib/security/cacerts". A tale scopo, utilizzare l'utilità "keytool" JAVA installata con JRE (Java Runtime Environment). Per ulteriori informazioni sull'utilità "strumento", vedere [la documentazione relativa agli strumenti per il sito Web Oracle](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html).

### <a name="importing-the-server-certificate-to-trust-store"></a>Importazione del certificato del server nell'archivio di attendibilità

Durante l'handshake TLS, il server invia il proprio certificato chiave pubblica al client. L'autorità emittente di un certificato chiave pubblica è nota come Autorità di certificazione (CA). Il client deve assicurarsi che l'autorità di certificazione sia una attendibilità del client. Questa garanzia viene eseguita conoscendo in anticipo la chiave pubblica di autorità di certificazione attendibili. In genere, con JVM viene fornito un set predefinito di Autorità di certificazione attendibili.

Se l'istanza del certificato TLS di SQL Server viene emessa da un'Autorità di certificazione privata, è necessario aggiungere il certificato dell'Autorità di certificazione all'elenco di certificati attendibili nell'archivio di attendibilità del computer client.

A tale scopo, utilizzare l'utilità "keytool" JAVA installata con JRE (Java Runtime Environment). Nel prompt dei comandi seguente viene illustrato come utilizzare l'utilità "keytool" per importare un certificato da un file:

```java
keytool -import -v -trustcacerts -alias myServer -file caCert.cer -keystore truststore.ks
```

Nell'esempio viene utilizzato un file denominato "caCert.cer" come file di certificato. È necessario ottenere questo file di certificato dal server. Nei passaggi seguenti viene illustrato come esportare il certificato del server in un file:

1. Selezionare Start, quindi Esegui e digitare MMC. MMC è l'acronimo di Microsoft Management Console.
2. In MMC aprire Certificati.
3. Espandere Personale, quindi Certificati.
4. Fare clic con il pulsante destro del mouse sul certificato del server, quindi scegliere Tutte le attività\Esporta.
5. Selezionare Avanti per spostarsi oltre la finestra di dialogo iniziale dell'esportazione guidata certificati.
6. Verificare che `No, do not export the private key` sia selezionato, quindi fare clic su Avanti.
7. Assicurarsi di selezionare il formato binario codificato DER X. 509 (. CER) o con codifica base 64 X. 509 (. CER), quindi fare clic su Avanti.
8. Immettere un nome per il file di esportazione.
9. Selezionare Avanti, quindi fare clic su fine per esportare il certificato.

## <a name="see-also"></a>Vedere anche

[Uso della crittografia](using-ssl-encryption.md)  
[Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)  
