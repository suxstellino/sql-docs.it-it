---
title: Informazioni sul supporto della crittografia
description: Informazioni su come verificare se JDBC Driver usa la crittografia TLS per proteggere le connessioni a un database SQL.
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 073f3b9e-8edd-4815-88ea-de0655d0325e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 84c50dc2f31b0497b42fd811e2e7ac3bea7fd47c
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177018"
---
# <a name="understanding-encryption-support"></a>Informazioni sul supporto della crittografia

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Se durante la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'applicazione richiede la crittografia e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata per il supporto della crittografia TLS, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] avvia l'handshake TLS. L'handshake consente al server e al client di negoziare la crittografia e gli algoritmi di crittografia da utilizzare per proteggere i dati. Al termine dell'handshake TLS, il client e il server possono inviare in modo protetto i dati crittografati. Durante l'handshake TLS, il server invia il proprio certificato chiave pubblica al client. L'autorità emittente di un certificato chiave pubblica è nota come Autorità di certificazione (CA). Il client ha la responsabilità di convalidare che l'autorità di certificazione è una attendibilità del client.

Se l'applicazione non richiede la crittografia, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] non forza il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporto della crittografia TLS. Se l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza non è configurata in modo da forzare la crittografia TLS, viene stabilita una connessione senza crittografia. Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata per la forzatura della crittografia TLS, il driver abilita questo tipo di crittografia automaticamente durante l'esecuzione in un ambiente Java Virtual Machine (JVM) correttamente configurato. In caso contrario, la connessione viene terminata e il driver genera un errore.

> [!NOTE]
> Perché una connessione TLS riesca, assicurarsi che il valore passato a **serverName** corrisponda esattamente al nome comune o al nome DNS nel nome soggetto alternativo (SAN, Subject Alternate Name) nel certificato del server.
>
> Per altre informazioni su come configurare TLS per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [abilitare le connessioni crittografate al motore di database](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="remarks"></a>Commenti

Per consentire alle applicazioni di utilizzare la crittografia TLS, in [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] sono state introdotte le proprietà di connessione seguenti a partire dalla versione 1,2: **Encrypt**, **TrustServerCertificate**, **trustStore**, **trustStorePassword** e **hostNameInCertificate**. Per altre informazioni, vedere [Impostazione delle proprietà di connessione](setting-the-connection-properties.md).

La tabella seguente offre un riepilogo del comportamento della versione di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] per i possibili scenari di connessione TLS. In ogni scenario viene usato un set di proprietà di connessione TLS diverso. La tabella include i valori seguenti:

- **blank**: "la proprietà non esiste nella stringa di connessione"
- **value**: la proprietà esiste nella stringa di connessione e il relativo valore è valido
- **any**: "non importa se la proprietà esiste nella stringa di connessione o se il relativo valore è valido"

> [!NOTE]
> Lo stesso comportamento si applica all'autenticazione utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e all'autenticazione integrata di Windows.

| encrypt | trustServerCertificate | hostNameInCertificate | trustStore | trustStorePassword | Comportamento |
| ------- | ---------------------- | --------------------- | ---------- | ------------------ | -------- |
| false o blank | any | any | any | any | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]Non forza il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporto della crittografia TLS. Se il server dispone di un certificato autofirmato, tramite il driver viene avviato lo scambio del certificato TLS. Il certificato TLS non verrà convalidato e verranno crittografate solo le credenziali (nel pacchetto di accesso).<br /><br /> Se il server richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS. Il certificato TLS non verrà convalidato, ma l'intera comunicazione verrà crittografata. |
| true | true | any | any | any | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS. Se la proprietà **TrustServerCertificate** è impostata su "true", il certificato TLS non verrà convalidato dal driver.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | vuoto | vuoto | vuoto | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> La proprietà **serverName** specificata nell'URL di connessione viene usata dal driver per convalidare il certificato TLS del server e le regole di ricerca della factory del gestore di attendibilità vengono usate per determinare l'archivio certificati da usare.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | Valore | vuoto | vuoto | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver convalida il valore del soggetto del certificato TLS tramite il valore specificato per la proprietà **hostNameInCertificate**.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | vuoto | Valore | Valore | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStore** per cercare il file trustStore del certificato e il valore della proprietà **trustStorePassword** per controllare l'integrità del file trustStore.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | vuoto | vuoto | Valore | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStorePassword** per controllare l'integrità del file trustStore predefinito.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | vuoto | Valore | vuoto | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStore** per cercare la posizione del file trustStore.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | Valore | vuoto | Valore | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStorePassword** per controllare l'integrità del file trustStore predefinito. Inoltre, il driver utilizzerà il valore della proprietà **hostNameInCertificate** per convalidare il certificato TLS.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | Valore | Valore | vuoto | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStore** per cercare la posizione del file trustStore. Inoltre, il driver utilizzerà il valore della proprietà **hostNameInCertificate** per convalidare il certificato TLS.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |
| true | false o blank | Valore | Valore | Valore | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] richiede di usare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Se il server supporta la crittografia o richiede il supporto della crittografia TLS da parte del client, tramite il driver viene avviato lo scambio del certificato TLS.<br /><br /> Il driver usa il valore della proprietà **trustStore** per cercare il file trustStore del certificato e il valore della proprietà **trustStorePassword** per controllare l'integrità del file trustStore. Inoltre, il driver utilizzerà il valore della proprietà **hostNameInCertificate** per convalidare il certificato TLS.<br /><br /> Se il server non è configurato per supportare la crittografia, il driver genererà un errore e terminerà la connessione. |

Se la proprietà encrypt è impostata su **true**, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] usa il provider di sicurezza JSSE predefinito di JVM per negoziare la crittografia TLS con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile che il provider di sicurezza predefinito non supporti tutte le funzionalità necessarie per negoziare la crittografia TLS. Tale provider può, ad esempio, non supportare la dimensione della chiave pubblica RSA usata nel certificato TLS di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In questo caso, è possibile che venga generato un errore dal provider di sicurezza predefinito, a causa del quale la connessione viene terminata dal driver JDBC. Per risolvere questo problema, è possibile usare una delle opzioni seguenti:

- Configurare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con un certificato del server con una chiave pubblica RSA di dimensione inferiore
- Configurare JVM per l'uso di un provider di sicurezza JSSE diverso nel file delle proprietà di sicurezza "\<java-home>/lib/security/java.security"
- Utilizzare una versione di JVM diversa.

## <a name="validating-server-tls-certificate"></a>Convalida del certificato TLS del server

Durante l'handshake TLS, il server invia il proprio certificato chiave pubblica al client. Il driver o il client JDBC deve verificare che il certificato del server sia emesso da un'autorità di certificazione considerata attendibile dal client. Il driver richiede che il certificato del server soddisfi le condizioni seguenti:

- Il certificato è stato emesso da un'Autorità di certificazione attendibile.
- Il certificato deve essere emesso per l'autenticazione del server.
- Il certificato non è scaduto.
- Il nome comune nell'oggetto o un nome DNS nel nome alternativo del soggetto (SAN, Subject Alternate Name) del certificato corrisponde esattamente al valore **serverName** specificato nella stringa di connessione o, se specificato, al valore della proprietà **hostNameInCertificate**.
- Un nome DNS può includere caratteri jolly. La versione precedente 7,2, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] non supporta la corrispondenza con caratteri jolly. Ovvero, abc.com non corrisponderà a \* . com, ma \* . com corrisponderà a \* . com. Con la versione 7,2 e versioni più rilevate, è supportata la corrispondenza dei certificati standard.

## <a name="see-also"></a>Vedi anche

[Uso della crittografia](using-ssl-encryption.md) 
 [Protezione delle applicazioni del driver JDBC](securing-jdbc-driver-applications.md)
