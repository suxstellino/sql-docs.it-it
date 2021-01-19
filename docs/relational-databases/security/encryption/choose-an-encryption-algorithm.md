---
title: Scegliere un algoritmo di crittografia | Microsoft Docs
description: Usare queste linee guida per scegliere un algoritmo di crittografia per proteggere un'istanza di SQL Server che supporta diversi algoritmi comuni.
ms.custom: ''
ms.date: 08/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], algorithms
- encryption [SQL Server], algorithms
- security [SQL Server], encryption
- algorithms [SQL Server encryption]
ms.assetid: 8227028c-a9c9-489d-bd27-fbf8242634ae
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fb31365f95cddc23739db02df9ba4ad9db0345e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171813"
---
# <a name="choose-an-encryption-algorithm"></a>Scelta di un algoritmo di crittografia
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La crittografia è una delle molte difese in profondità che gli amministratori possono utilizzare per proteggere un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Gli algoritmi di crittografia definiscono trasformazioni dei dati che non possono essere facilmente invertite da utenti non autorizzati. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] consente ad amministratori e sviluppatori di scegliere tra diversi algoritmi, tra cui DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 a 128 bit, DESX, AES a 128 bit, AES a 192 bit e AES a 256 bit.  
  
> [!NOTE]  
>  A partire da [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], tutti gli algoritmi diversi da AES_128, AES_192 e AES_256 sono deprecati. Per usare algoritmi meno recenti (sconsigliato), è necessario impostare il database sul livello di compatibilità del database 120 o su uno inferiore.  
  
 Nessun algoritmo è ideale in tutte le situazioni e la valutazione dei vantaggi relativi a ogni algoritmo esula dall'ambito della documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Sono comunque validi i principi generali seguenti:  
  
-   La crittografia avanzata utilizza in genere una quantità di risorse della CPU maggiore rispetto alla crittografia vulnerabile.  
  
-   Le chiavi lunghe consentono in genere una crittografia più avanzata rispetto a quelle brevi.  
  
-   La crittografia asimmetrica è più lenta della crittografia simmetrica.  
  
-   Le password lunghe e complesse sono più avanzate rispetto alle password brevi.  

-   La crittografia simmetrica è in genere consigliata quando la chiave viene archiviata solo in locale, la crittografia asimmetrica è consigliata quando le chiavi devono essere condivise in rete.
  
-   Se si crittografano molti dati, è necessario crittografarli tramite una chiave simmetrica e crittografare la chiave simmetrica con una asimmetrica.  
  
-   I dati crittografati non possono essere compressi, ma i dati compressi possono essere crittografati. Se si utilizza la compressione, è necessario comprimere i dati prima di crittografarli.  
  
> [!IMPORTANT]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] e versioni successive il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
>   
>  L'utilizzo ripetuto della stessa funzione KEY_GUID RC4 o RC4_128 su blocchi di dati diversi produrrà la stessa chiave RC4 perché [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non fornisce automaticamente un valore salt. L'utilizzo ripetuto della stessa chiave RC4 è un errore noto che comporta una crittografia molto debole. Per questo motivo le parole chiave RC4 e RC4_128 sono deprecate. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]  
  
 Per altre informazioni sugli algoritmi e sulla tecnologia di crittografia, vedere [Concetti chiave sulla sicurezza](/previous-versions/aa720225(v=vs.71)) nella Guida per gli sviluppatori di .NET Framework in MSDN.  
  
 **Chiarimento relativo agli algoritmi DES:**  
  
-   La crittografia DESX è stata menzionata erroneamente. Le chiavi simmetriche create con ALGORITHM = DESX utilizzano in realtà la crittografia TRIPLE DES con una chiave a 192 bit. L'algoritmo DESX non è disponibile. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Le chiavi simmetriche create con ALGORITHM = TRIPLE_DES_3KEY utilizzano TRIPLE DES con una chiave a 192 bit.  
  
-   Le chiavi simmetriche create con ALGORITHM = TRIPLE_DES utilizzano TRIPLE DES con una chiave a 128 bit.  
  
## <a name="related-tasks"></a>Attività correlate  
  
| Attività | Type |
| ---- | ---- |
|Crittografia tramite una chiave simmetrica.|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Crittografia tramite una chiave asimmetrica.|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|Crittografia tramite certificato.|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)|  
|Crittografia dei file di database mediante Transparent Data Encryption.|[Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)|  
|Come crittografare una colonna di una tabella.|[Crittografia di una colonna di dati](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Crittografia di SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Gerarchia di crittografia](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
