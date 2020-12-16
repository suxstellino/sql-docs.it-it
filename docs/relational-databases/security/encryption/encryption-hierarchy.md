---
title: Gerarchia di crittografia | Microsoft Docs
description: Informazioni sulla gerarchia di crittografia e sull'infrastruttura di gestione delle chiavi in SQL Server. Archiviare le chiavi in un modulo Extensible Key Management.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], hierarchies
- cryptography [SQL Server], hierarchies
- encryption keys [SQL Server]
- security [SQL Server], encryption
- hierarchies [SQL Server], encryption
ms.assetid: 96c276d5-1bba-4e95-b678-10f059f1fbcf
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c47cb3825c18b518c547e7c88c3c89e0c2f3c23a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477542"
---
# <a name="encryption-hierarchy"></a>Gerarchia di crittografia
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crittografa i dati in base ad una particolare gerarchia di crittografia e infrastruttura di gestione delle chiavi. Ciascun livello crittografa il livello sottostante utilizzando una combinazione di certificati, chiavi asimmetriche e chiavi simmetriche. Le chiavi asimmetriche e simmetriche possono essere archiviate al di fuori di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un modulo EKM (Extensible Key Management).  
  
 Nella figura seguente viene indicato come nella gerarchia di crittografia ogni livello crittografi il livello sottostante e vengono visualizzate le configurazioni di crittografia più comuni. L'accesso all'inizio della gerarchia è in genere protetto da una password.  
  
 ![Visualizza alcune combinazioni di crittografia in uno stack.](../../../relational-databases/security/encryption/media/encryption-hierarchy-stack.gif "Visualizza alcune combinazioni di crittografia in uno stack.")  
  
 Tenere presenti i concetti seguenti:  
  
-   Per ottenere prestazioni ottimali, crittografare i dati utilizzando chiavi simmetriche anziché certificati o chiavi asimmetriche.  
  
-   Le chiavi master del database sono protette dalla chiave master del servizio. La chiave master del servizio viene creata dal programma di installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e viene crittografata tramite l'API di protezione dati di Windows (DPAPI).  
  
-   Sono possibili altre gerarchie di crittografia che includono livelli aggiuntivi.  
  
-   Un modulo EKM contiene chiavi simmetriche o asimmetriche al di fuori di SQL Server.  
  
-   Transparent Data Encryption (TDE) deve utilizzare una chiave simmetrica denominata chiave di crittografia del database, che è protetta da un certificato protetto a sua volta dalla chiave master del database master o da una chiave asimmetrica archiviata in un modulo EKM.  
  
-   La chiave master del servizio e tutte le chiavi master del database sono chiavi simmetriche.  
  
 Le stesse informazioni vengono illustrate in modo diverso nella figura seguente.  
  
 ![Visualizza alcune combinazioni di crittografia in una ruota.](../../../relational-databases/security/encryption/media/encryption-hierarchy-wheel.gif "Visualizza alcune combinazioni di crittografia in una ruota.")  
  
 In questo diagramma vengono illustrati i concetti aggiuntivi seguenti:  
  
-   In questa illustrazione le frecce indicano gerarchie di crittografia comuni.  
  
-   Le chiavi simmetriche e asimmetriche in EKM possono proteggere l'accesso alle chiavi simmetriche e asimmetriche archiviate in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La linea punteggiata associata a EKM indica che le chiavi in EKM potrebbero sostituire le chiavi simmetriche e asimmetriche archiviate in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="encryption-mechanisms"></a>Meccanismi di crittografia  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comprende i meccanismi di crittografia seguenti:  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] funzioni  
  
-   Chiavi asimmetriche  
  
-   Chiavi simmetriche  
  
-   Certificati  
  
-   Transparent Data Encryption  
  
### <a name="transact-sql-functions"></a>Funzioni Transact-SQL  
 Singoli elementi possono essere crittografati mentre vengono inseriti o aggiornati con le funzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] . Per altre informazioni, vedere [ENCRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbypassphrase-transact-sql.md) e [DECRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbypassphrase-transact-sql.md).  
  
### <a name="certificates"></a>Certificati  
 Un certificato a chiave pubblica, normalmente chiamato semplicemente un certificato, è un'istruzione firmata digitalmente che associa il valore di una chiave pubblica all'identità di una persona, un dispositivo o un servizio che detiene la corrispondente chiave privata. I certificati vengono rilasciati e firmati da un'autorità di certificazione (CA). L'entità che riceve un certificato da un'autorità di certificazione è il soggetto del certificato. Normalmente, i certificati contengono le informazioni seguenti.  
  
-   Chiave pubblica del soggetto.  
  
-   Informazioni di identificazione del soggetto, come ad esempio il nome e l'indirizzo di posta elettronica.  
  
-   Periodo di validità. È il periodo di tempo durante il quale il certificato è considerato valido.  
  
     Un certificato è valido solo per il periodo di tempo specificato nel certificato; e ogni certificato contiene le date **Valido dal** e **Valido fino al** . Queste date definiscono i limiti del periodo di validità. Quando il periodo di validità di un certificato viene superato, il soggetto del certificato scaduto deve richiedere un nuovo certificato.  
  
-   Informazioni di identificazione dell'emittente  
  
-   Firma digitale dell'emittente.  
  
     Questa firma attesta la validità dell'associazione tra la chiave pubblica e le informazioni di identificazione del soggetto. (L'apposizione di una firma digitale a informazioni comporta la trasformazione delle informazioni, e di alcune informazioni segrete detenute dal mittente, in un tag chiamato firma.)  
  
 Il vantaggio principale dei certificati è quello di evitare che gli host debbano mantenere un set di password per singoli soggetti. L'host non deve fare altro che stabilire una relazione di trust con l'emittente di un certificato, che successivamente può firmare un numero illimitato di certificati.  
  
 Quando un host, come ad esempio un server Web sicuro, designa un emittente come autorità radice affidabile, l'host implicitamente considera affidabili i criteri che l'emittente ha utilizzato per stabilire le associazioni relative ai certificati emessi. In effetti, l'host fa affidamento all'emittente per la verifica dell'identità del soggetto del certificato. L'host designa un emittente come autorità radice affidabile collocando il certificato autofirmato dell'emittente, contenente la chiave pubblica dell'emittente, nell'archivio certificati delle autorità di certificazione affidabili del computer host. Le autorità di certificazione intermedie o subordinate vengono considerate affidabili solo se dispongono di un percorso di certificazione valido ottenuto da un'autorità di certificazione radice affidabile.  
  
 L'emittente può revocare un certificato prima che scada. La revoca di un certificato annulla l'associazione tra la chiave pubblica e l'identità dichiarata nel certificato. Ciascun emittente mantiene un elenco di revoche di certificati che può essere utilizzato da programmi per la verifica della validità di un particolare certificato.  
  
 I certificati autofirmati creati da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sono conformi allo standard X.509 e supportano i campi X.509 v1.  
  
### <a name="asymmetric-keys"></a>Chiavi asimmetriche  
 Una chiave asimmetrica è costituita da una chiave privata e dalla corrispondente chiave pubblica. Ogni chiave può decrittografare dati crittografati dall'altra chiave. La crittografia e la decrittografia asimmetrica richiedono un uso intensivo delle risorse ma offrono un livello di sicurezza superiore a quello della crittografia simmetrica. Una chiave asimmetrica può essere utilizzata per crittografare una chiave simmetrica da memorizzare in un database.  
  
### <a name="symmetric-keys"></a>Chiavi simmetriche  
 Una chiave simmetrica è una chiave che viene utilizzata sia per la crittografia che per decrittografia. La crittografia e la decrittografia tramite una chiave simmetrica sono veloci e sono quindi adatte per essere utilizzate in modo ricorrente per informazioni riservate contenute in database.  
  
### <a name="transparent-data-encryption"></a>Transparent Data Encryption  
 Transparent Data Encryption è un caso speciale di crittografia che utilizza una chiave simmetrica. Transparent Data Encryption crittografa un intero database utilizzando una chiave simmetrica denominata chiave di crittografia del database. La chiave di crittografia del database è protetta da altri certificati o chiavi, a loro volta protetti dalla chiave master del database o da una chiave asimmetrica archiviata in un modulo EKM. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
## <a name="related-content"></a>Contenuto correlato  
 [Sicurezza di SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Gerarchia delle autorizzazioni &#40;Motore di database&#41;](../../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Entità a protezione diretta](../../../relational-databases/security/securables.md)  
  
  
