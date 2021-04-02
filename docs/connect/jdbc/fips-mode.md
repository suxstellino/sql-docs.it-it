---
description: Informazioni su come utilizzare il driver JDBC per SQL Server in modalità FIPS per assicurare la conformità dell'applicazione FIPS 140.
title: Modalità FIPS in JDBC
ms.custom: ''
ms.date: 03/21/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: genemi
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f11527e1b6e5917ed7b74624dc5e92ed39c27047
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177002"
---
# <a name="fips-mode"></a>Modalità FIPS

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC Driver per SQL Server supporta l'esecuzione in JVM (Java Virtual Machine) configurate per essere *conformi a FIPS 140*.

## <a name="prerequisites"></a>Prerequisiti

- JVM configurata per FIPS
- Certificato TLS/SSL appropriato
- File di criteri appropriati
- Parametri di configurazione appropriati

## <a name="fips-configured-jvm"></a>JVM configurata per FIPS

In genere, le applicazioni possono configurare il `java.security` file per l'utilizzo di provider di crittografia conformi a FIPS. Per informazioni sulla configurazione della conformità FIPS 140, vedere la documentazione specifica per la JVM.

Per informazioni sui moduli approvati per la configurazione FIPS, vedere [Moduli convalidati nel Programma di convalida moduli di crittografia](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/Validated-Modules).

Per configurare una JVM con FIPS, i fornitori possono eseguire altri passaggi.

## <a name="appropriate-tls-certificate"></a>Certificato TLS appropriato

Per connettersi a SQL Server in modalità FIPS, è necessario un certificato TLS/SSL valido. Installarlo o importarlo nell'archivio chiavi Java nel computer client (JVM) in cui è abilitato FIPS.

### <a name="importing-tls-certificate-in-java-keystore"></a>Importazione del certificato TLS nell'archivio chiavi Java

Per FIPS, è molto probabile che sia necessario importare il certificato (con estensione cert) in formato PKCS o in un formato specifico del provider.
Usare il frammento di codice seguente per importare il certificato TLS/SSL e archiviarlo in una directory di lavoro con il formato di archivio chiavi appropriato. _TRUST\_STORE\_PASSWORD_ è la password per l'archivio chiavi Java.

```java
public void saveGenericKeyStore(
        String provider,
        String trustStoreType,
        String certName,
        String certPath
        ) throws KeyStoreException, CertificateException,
            NoSuchAlgorithmException, NoSuchProviderException,
            IOException
{
    KeyStore ks = KeyStore.getInstance(trustStoreType, provider);
    FileOutputStream os = new FileOutputStream("./MyTrustStore_" + trustStoreType);
    ks.load(null, null);
    ks.setCertificateEntry(certName, getCertificate(certPath));
    ks.store(os, TRUST_STORE_PASSWORD.toCharArray());
    os.flush();
    os.close();
}

private Certificate getCertificate(String pathName)
        throws FileNotFoundException, CertificateException
{
    FileInputStream fis = new FileInputStream(pathName);
    CertificateFactory cf = CertificateFactory.getInstance("X.509");
    return cf.generateCertificate(fis);
}
```

Nell'esempio seguente viene importato un certificato TLS/SSL di Azure in formato PKCS12 con il provider BouncyCastle. Il certificato viene importato nella directory di lavoro denominata _MyTrustStore\_PKCS12_ usando il frammento di codice seguente:

`saveGenericKeyStore(BCFIPS, PKCS12, "SQLAzure SSL Certificate Name", "SQLAzure.cer");`

## <a name="appropriate-policy-files"></a>File di criteri appropriati

Per alcuni provider FIPS sono necessari JAR di criteri senza restrizioni. In questi casi, per Sun/Oracle, scaricare Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files per [JRE 8](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) o [JRE 7](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html).

## <a name="appropriate-configuration-parameters"></a>Parametri di configurazione appropriati

Per eseguire JDBC Driver in modalità conforme a FIPS, configurare le proprietà di connessione come indicato nella tabella riportata di seguito.

### <a name="properties"></a>Proprietà

|Proprietà|Type|Predefinito|Descrizione|Note|
|---|---|---|---|---|
|`encrypt`|booleano ["true/false"]|"false"|Per la JVM abilitata per FIPS, la proprietà encrypt deve essere **true**||
|`TrustServerCertificate`|booleano ["true/false"]|"false"|Per FIPS, l'utente deve convalidare la catena di certificati. Pertanto l'utente deve usare il valore **"false"** per questa proprietà. ||
|`trustStore`|string|Null|Il percorso del file dell'archivio chiavi Java in cui è stato importato il certificato. Se il certificato viene installato nel sistema, non è necessario passare alcun elemento. Il driver usa i file cacerts o jssecacerts.||
|`trustStorePassword`|string|Null|Password utilizzata per verificare l'integrità dei dati del file trustStore.||
|`fips`|booleano ["true/false"]|"false"|Per la JVM abilitata per FIPS, questa proprietà deve essere **true**.|Aggiunta nella versione 6.1.4 (versione stabile 6.2.2)|
|`fipsProvider`|string|Null|Provider FIPS configurato nella JVM. Ad esempio, BCFIPS o SunPKCS11-NSS |Aggiunta nella versione 6.1.2 (versione stabile 6.2.2), deprecata nella versione 6.4.0. Vedere i dettagli [qui](https://github.com/Microsoft/mssql-jdbc/pull/460).|
|`trustStoreType`|string|JKS|Per la modalità FIPS impostare il tipo di archivio attendibilità su PKCS12 o sul tipo definito dal provider FIPS |Aggiunta nella versione 6.1.2 (versione stabile 6.2.2)|
