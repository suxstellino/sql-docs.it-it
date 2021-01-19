---
description: Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti
title: Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 03a57949624897b2cd2e0552892803c21a408b52
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534810"
---
# <a name="enable-always-encrypted-with-secure-enclaves-for-existing-encrypted-columns"></a>Abilitare Always Encrypted con enclave sicuri per le colonne crittografate esistenti 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Questo articolo descrive come rendere disponibile la funzionalità di Always Encrypted con gli enclave sicuri e abilitare i calcoli dell'enclave per le colonne crittografate esistenti.  

Se sono presenti colonne crittografate con chiavi non abilitate per l'enclave, è possibile crittografare le colonne con chiavi abilitate per l'enclave. In questo modo sarà possibile usare l'enclave sicuro nelle query sulle colonne.

È possibile abilitare i calcoli dell'enclave per le colonne crittografate esistenti in diversi modi, a seconda di:

- **Ambito/granularità:** si vuole abilitare la funzionalità di enclave per un subset di colonne o per tutte le colonne protette con una chiave master della colonna specificata?
- **Dimensioni dei dati:** quali sono le dimensioni delle tabelle contenenti le colonne che si vuole abilitare per l'enclave?
- Si vuole modificare anche il tipo di crittografia per le colonne? Tenere presente che solo la crittografia casuale supporta i calcoli avanzati (criteri di ricerca, operatori di confronto). Se la colonna è crittografata con la crittografia deterministica, sarà necessario anche crittografarla di nuovo con la crittografia casuale per rendere disponibili i calcoli avanzati.

Le sezioni seguenti descrivono i tre approcci per l'abilitazione degli enclave per colonne esistenti.

## <a name="method-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>Metodo 1: Ruotare la chiave master della colonna per sostituirla con una chiave master della colonna abilitata per l'enclave
La sostituzione di una chiave master della colonna esistente (non abilitata per l'enclave) con una nuova chiave master della colonna abilitata per l'enclave rende effettivamente abilitate per l'enclave anche tutte le chiavi di crittografia di colonna (associate alla chiave master della colonna).

- Vantaggi:
  - Non richiede di crittografare di nuovo i dati, pertanto è in genere l'approccio più rapido. Si tratta di un approccio consigliato per le colonne contenenti grandi quantità di dati, che sono già abilitate per i calcoli avanzati e usano la crittografia deterministica.
  - Consente di abilitare la funzionalità di enclave per più colonne su larga scala. La sostituzione della chiave master della colonna con la chiave master della colonna abilitata per l'enclave abilita per l'enclave tutte le chiavi di crittografia di colonna e tutte le colonne crittografate associate alla chiave master della colonna originale.
  
- Svantaggi:
  - Non supporta il passaggio del tipo di crittografia da deterministico a casuale. Anche se rende disponibile la crittografia sul posto per le colonne crittografate con la crittografia deterministica, non abilita i calcoli avanzati. Sarà necessario crittografare di nuovo le colonne usando la crittografia casuale.
  - Non consente di convertire in modo selettivo alcune delle colonne associate a una determinata chiave master della colonna.
  - Introduce un sovraccarico di gestione delle chiavi. Sarà necessario creare una nuova chiave master della colonna e renderla disponibile per le applicazioni che eseguono query sulle colonne interessate.

Per informazioni su come ruotare la chiave master di una colonna, vedere [Ruotare le chiavi abilitate per l'enclave](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-2-rotate-the-column-master-key-and-re-encrypt-columns-using-randomized-encryption-in-place"></a>Metodo 2: Ruotare la chiave master della colonna e crittografare di nuovo le colonne usando la crittografia casuale sul posto
Questo metodo prevede l'esecuzione del metodo 1 come primo passaggio e quindi la nuova crittografia delle colonne. Le colonne usano inizialmente la crittografia deterministica e quindi vengono ricrittografate con la crittografia casuale per sbloccare le query avanzate.

- Vantaggi:
  - Crittografa nuovamente i dati sul posto. È un metodo consigliato se è necessario abilitare le query complesse per le colonne crittografate che usano attualmente la crittografia deterministica e contengono grandi quantità di dati. Il passaggio 1 (la rotazione della chiave master della colonna) sblocca la crittografia sul posto per le colonne che usano la crittografia deterministica e il passaggio 2 (nuova crittografia delle colonne) può essere eseguito sul posto.
  - Consente di abilitare la funzionalità di enclave per più colonne su larga scala.
  
- Svantaggi:
  - Non consente di convertire in modo selettivo alcune delle colonne associate a una determinata chiave master della colonna.
  - Introduce un sovraccarico di gestione delle chiavi. Sarà necessario creare una nuova chiave master della colonna e renderla disponibile per le applicazioni che eseguono query sulle colonne interessate.

Per informazioni su come ruotare una chiave master di una colonna e crittografare nuovamente una colonna sul posto per ruotare una chiave di crittografia di colonna, vedere [Ruotare le chiavi abilitate per l'enclave](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-3-re-encrypt-a-selected-column-with-an-enclave-enabled-column-encryption-key-on-the-client-side"></a>Metodo 3: Crittografare nuovamente una colonna selezionata con una chiave di crittografia di colonna abilitata per l'enclave sul lato client
Questo metodo implica la nuova crittografia di una colonna con una chiave di crittografia di colonna abilitata per l'enclave e abilita le query complesse con la crittografia casuale. Poiché la chiave di crittografia di colonna corrente non è abilitata per l'enclave, non è possibile crittografare nuovamente la colonna sul posto. Usare la procedura guidata Always Encrypted o il cmdlet Set-SqlColumnEncryption per crittografare di nuovo la colonna all'esterno del database.

- Vantaggi:
  - Consente di abilitare in modo selettivo la funzionalità dell'enclave (crittografia sul posto e query complesse, se si sta eseguendo di nuovo la crittografia della colonna con la crittografia casuale) per una colonna o un piccolo subset di colonne.
  - Può abilitare i calcoli avanzati per le colonne in un unico passaggio.
  - Non è richiesta la creazione di una nuova chiave master della colonna, quindi l'impatto sulle applicazioni è minore.
  
- Svantaggi:
  - Per crittografare di nuovo i dati, lo strumento li sposterà al di fuori del database, ma questa operazione può richiedere molto tempo ed è soggetta a errori di rete.

Per altre informazioni su come ruotare la crittografia di una colonna tramite uno strumento lato client, vedere [Ruotare le chiavi Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) e [Ruotare le chiavi Always Encrypted con PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md)
- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)
