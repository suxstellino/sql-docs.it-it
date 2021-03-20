---
title: Certificati SQL Server e chiavi simmetriche | Microsoft Docs
description: Informazioni su certificati e chiavi asimmetriche in SQL Server, inclusi i certificati generati esternamente o generati da SQL Server, gli strumenti e le attività correlate.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server], certificates and asymmetric keys
ms.assetid: 8519aa2f-f09c-4c1c-96b5-abc24811e60c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58c0cb870c6d9c91556f27b3827404f9b9469042
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739695"
---
# <a name="sql-server-certificates-and-asymmetric-keys"></a>Certificati SQL Server e chiavi simmetriche
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 La crittografia a chiave pubblica è un sistema di tutela della segretezza dei messaggi in cui un utente crea una chiave *pubblica* e una chiave *privata*. La chiave privata viene tenuta segreta, mentre la chiave pubblica può essere distribuita ad altri. Sebbene le chiavi siano collegate da una relazione matematica, non è possibile estrapolare facilmente la chiave privata utilizzando la chiave pubblica. La chiave pubblica può essere usata per crittografare dati che potranno essere decrittografati solo dalla chiave privata corrispondente. Questo approccio può essere usato per crittografare messaggi destinati al proprietario della chiave privata. In modo analogo il proprietario di una chiave privata può crittografare dati che possono essere decrittografati solo con la chiave pubblica. Questo approccio costituisce la base dei certificati digitali, in cui le informazioni presenti nel certificato vengono crittografate dal proprietario di una chiave privata, in modo da offrire una garanzia all'autore del contenuto. Poiché le chiavi di crittografia e decrittografia sono diverse, sono note come chiavi *asimmetriche*.
  
 Certificati e chiavi asimmetriche rappresentano entrambi una modalità di utilizzo della crittografia asimmetrica. I certificati vengono spesso utilizzati come contenitori delle chiavi asimmetriche perché possono contenere un maggior numero di informazioni, ad esempio date di scadenza e autorità emittenti. Non c'è differenza tra i due meccanismi per l'algoritmo di crittografia e nessuna differenza nel livello di protezione fornito a parità di lunghezza della chiave. In genere, si utilizza un certificato per crittografare gli altri tipi di chiavi di crittografia in un database o per firmare moduli di codice.  
  
 Certificati e chiavi asimmetriche sono in grado di decrittografare dati crittografati da altri. In genere, la crittografia asimmetrica viene utilizzata per crittografare una chiave simmetrica da archiviare in un database.  
  
 Al contrario di un certificato, una chiave pubblica non presenta un formato particolare e non può essere esportata in un file.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene caratteristiche che consentono di creare e gestire certificati e chiavi da utilizzare con il server e il database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per creare e gestire certificati e chiavi con le altre applicazioni o nel sistema operativo.  
  
## <a name="certificates"></a>Certificati  
 Un certificato è un oggetto di sicurezza provvisto di firma digitale all'interno del quale è presente una chiave pubblica (e facoltativamente una privata) per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile utilizzare certificati generati all'esterno o da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] I certificati sono conformi allo standard per certificati IETF X.509v3.  
  
 L'utilità dei certificati deriva dall'opzione che consente di esportare e importare le chiavi a file di certificato X.509. La sintassi per la creazione di certificati offre opzioni come l'impostazione di una data di scadenza.  
  
### <a name="using-a-certificate-in-sql-server"></a>Utilizzo di un certificato in SQL Server  
 È possibile utilizzare i certificati per proteggere connessioni, eseguire il mirroring del database, firmare pacchetti e altri oggetti o crittografare dati o connessioni. Nella tabella seguente sono elencate risorse aggiuntive per i certificati presenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|Viene descritto il comando per la creazione di certificati.|  
|[Identificazione dell'origine dei pacchetti con firme digitali](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md)|Vengono fornite informazioni sull'utilizzo di certificati per la firma di pacchetti software.|  
|[Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|Vengono fornite informazioni sull'utilizzo dei certificati con il mirroring del database.|  
  
## <a name="asymmetric-keys"></a>Chiavi asimmetriche  
 Le chiavi asimmetriche sono utilizzate per proteggere le chiavi simmetriche. È possibile utilizzarle anche per una crittografia limitata dei dati e per la firma digitale di oggetti di database. Una chiave asimmetrica consiste in una chiave privata e in una chiave pubblica corrispondente. Per altre informazioni sulle chiavi simmetriche, vedere [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md).  
  
 È possibile importare le chiavi asimmetriche da file di chiave con nome sicuro, ma non esportarle. Le chiavi asimmetriche, inoltre, non hanno opzioni di scadenza e non sono in grado di crittografare connessioni.  
  
### <a name="using-an-asymmetric-key-in-sql-server"></a>Utilizzo di una chiave asimmetrica in SQL Server  
 È possibile utilizzare chiavi asimmetriche per proteggere dati o firmare testo non crittografato. Nella tabella seguente sono elencate risorse aggiuntive per chiavi asimmetriche presenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)|Viene descritto il comando per la creazione di chiavi asimmetriche.|  
|[SIGNBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/signbyasymkey-transact-sql.md)|Vengono visualizzate le opzioni per la firma di oggetti.|  
  
## <a name="tools"></a>Strumenti  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] vengono forniti strumenti e utilità in grado di generare certificati e file di chiave con nome sicuro. Questi strumenti offrono maggiore flessibilità nel processo di generazione delle chiavi rispetto alla sintassi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È possibile utilizzare questi strumenti per creare chiavi RSA con lunghezze di chiave più complesse e importarle quindi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nella tabella seguente viene indicato dove è possibile trovare questi strumenti.  
  
| Strumento | Scopo |
| ---- | ------- |
|[New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)|Crea certificati autofirmati.|  
|[makecert](/windows/desktop/SecCrypto/makecert)|Vengono creati certificati. Deprecato a favore di **New-SelfSignedCertificate**.|  
|[sn](/dotnet/framework/tools/sn-exe-strong-name-tool)|Vengono creati nomi sicuri per le chiavi simmetriche.|  
  
## <a name="related-tasks"></a>Attività correlate  
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)   
 [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
  
