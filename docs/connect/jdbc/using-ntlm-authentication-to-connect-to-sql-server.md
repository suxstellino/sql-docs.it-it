---
title: Uso dell'autenticazione NTLM per la connessione a SQL Server
description: Informazioni su come stabilire una connessione al database SQL usando l'autenticazione NTLM con JDBC Driver.
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: lilgreenbird
ms.author: v-susanh
manager: kenvh
ms.openlocfilehash: eac4f363cd289695b92bfbdc02bcfe9b167f77d2
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177082"
---
# <a name="using-ntlm-authentication-to-connect-to-sql-server"></a>Uso dell'autenticazione NTLM per la connessione a SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] consente a un'applicazione di usare la proprietà di connessione **authenticationScheme** per specificare una connessione a un database tramite l'autenticazione NTLM v2.

Per l'autenticazione NTLM vengono usate anche le proprietà seguenti:

- **domain = domainName** (facoltativo)
- **user = userName**
- **password = password**
- **integratedSecurity = true**

Ad eccezione di **domain**, le altre proprietà sono obbligatorie. Nel caso in cui non siano presenti, il driver genera un errore quando viene usata la proprietà authenticationScheme di **NTLM**.

Per altre informazioni sulle proprietà di connessione, vedere [Impostazione delle proprietà delle connessioni](setting-the-connection-properties.md). Per altre informazioni sul protocollo di autenticazione NTLM di Microsoft, vedere [Microsoft NTLM](/windows/desktop/SecAuthN/microsoft-ntlm).

## <a name="remarks"></a>Osservazioni

Vedere [Sicurezza di rete: livello di autenticazione di LAN Manager](/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level) per la descrizione delle impostazioni di SQL Server, che controllano il comportamento dell'autenticazione NTLM.

## <a name="logging"></a>Registrazione

È stato aggiunto un nuovo logger per supportare l'autenticazione NTLM: com.microsoft.sqlserver.jdbc.internals.NTLMAuthentication. Per altre informazioni, vedere [Creazione di tracce](tracing-driver-operation.md).

## <a name="datasource"></a>DataSource

Quando si usa un'origine dati per creare connessioni, le proprietà NTLM possono essere impostate a livello di codice usando **setAuthenticationScheme**, **setDomain** e, facoltativamente, **setServerSpn**.

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(1433); // change if necessary
ds.setIntegratedSecurity(true);
ds.setAuthenticationScheme("NTLM");
ds.setDomain("<domainName>");
ds.setUser("<userName>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setServerSpn("<serverSpn");

try (Connection c = ds.getConnection(); Statement s = c.createStatement();
        ResultSet rs = s.executeQuery("select auth_scheme from sys.dm_exec_connections where session_id=@@spid")) {
    while (rs.next()) {
        System.out.println("Authentication Scheme: " + rs.getString(1));
    }
}
```

## <a name="service-principal-names"></a>Nomi dell'entità servizio

Un nome dell'entità servizio (SPN) è il nome con cui un client identifica in modo univoco un'istanza di un servizio.

È possibile specificare il nome dell'entità servizio usando la proprietà di connessione **serverSpn** o lasciare che sia il driver a crearlo automaticamente (impostazione predefinita). Questa proprietà usa il formato: "MSSQLSvc/fqdn:porta\@AREADIAUTENTICAZIONE", dove fqdn è il nome di dominio completo, porta è il numero di porta e AREADIAUTENTICAZIONE è l'area di autenticazione di SQL Server in lettere maiuscole. La parte dell'area di autenticazione di questa proprietà è facoltativa perché l'area di autenticazione predefinita è la stessa di quella del server.

Ad esempio, il nome dell'entità servizio potrebbe essere simile al seguente: "MSSQLSvc/some-server.zzz.corp.contoso.com:1433"

Per ulteriori informazioni sui nomi dell'entità servizio (SPN), vedere:

- [Supporto del nome dell'entità servizio (SPN) nelle connessioni client](../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)

> [!NOTE]
> L'attributo di connessione serverSpn è supportato solo da Microsoft JDBC Driver 4.2 e versioni successive.

> Per le versioni precedenti alla 6.2 del driver JDBC, è necessario impostare **serverSpn** in modo esplicito. A partire dalla versione 6.2, il driver può compilare **serverSpn** per impostazione predefinita, sebbene sia possibile usare **serverSpn** in modo esplicito.

## <a name="security-risks"></a>Rischi per la sicurezza

Il protocollo NTLM è un protocollo di autenticazione obsoleto con diverse vulnerabilità, che rappresentano un rischio per la sicurezza. Si basa su uno schema crittografico debole ed è vulnerabile agli attacchi. NTLM è stato sostituito da Kerberos, che è molto più sicuro e consigliato. L'autenticazione NTLM deve essere usata solo in un ambiente attendibile sicuro oppure quando non è possibile usare Kerberos.

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] supporta solo NTLM v2, che offre alcuni miglioramenti per la sicurezza rispetto al protocollo v1 originale. È inoltre consigliabile abilitare la protezione estesa oppure utilizzare la crittografia TLS per una maggiore sicurezza.

Per altre informazioni su come abilitare la protezione estesa, vedere:

- [Connessione al motore di database mediante la protezione estesa](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)

Per ulteriori informazioni sulla connessione con la crittografia, vedere:

- [Connessione tramite la crittografia](connecting-with-ssl-encryption.md)

> [!NOTE]
> Per la versione 7.4, l'abilitazione di **entrambe**, protezione estesa e crittografia, non è supportata.

## <a name="see-also"></a>Vedere anche

[Connessione a SQL Server con il driver JDBC](connecting-to-sql-server-with-the-jdbc-driver.md)
