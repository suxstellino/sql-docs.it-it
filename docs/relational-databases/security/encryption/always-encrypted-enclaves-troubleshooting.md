---
title: Risolvere i problemi comuni per Always Encrypted con enclave sicure
description: Risolvere i problemi comuni per Always Encrypted con enclave sicure
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: dc6bcbecdb29cdf0cf1fca8c41e971463bb0d6b9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236363"
---
# <a name="troubleshoot-common-issues-for-always-encrypted-with-secure-enclaves"></a>Risolvere i problemi comuni per Always Encrypted con enclave sicure

Questo articolo descrive come identificare e risolvere i problemi comuni che è possibile riscontrare quando si eseguono istruzioni Transact-SQL (TSQL) con [Always Encrypted con enclave sicure](always-encrypted-enclaves.md).

Per informazioni su come eseguire le query usando le enclave sicure, vedere [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md).

## <a name="database-connection-errors"></a>Errori di connessione del database

Per eseguire le istruzioni usando un'enclave sicura, è necessario abilitare Always Encrypted e specificare un URL di attestazione per la connessione di database, come illustrato in [Prerequisiti per l'esecuzione di istruzioni con enclave sicure](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves). La connessione avrà tuttavia esito negativo se si specifica un URL di attestazione, ma il database nel [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] o l'istanza di [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] di destinazione non supporta le enclave sicure oppure non è configurata correttamente.

- Se si usa il [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], controllare che il database usi la configurazione hardware della [serie DC](https://docs.microsoft.com/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series). Per altre informazioni, vedere [Abilitare Intel SGX per il database SQL di Azure](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).
- Se si usa [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], controllare che l'enclave sicura sia configurata correttamente per l'istanza. Per altre informazioni, vedere [Configurare l'enclave sicura in SQL Server](always-encrypted-enclaves-configure-enclave-type.md).

## <a name="attestation-errors-when-using-microsoft-azure-attestation"></a>Errori di attestazione quando si usa l'attestazione Microsoft Azure

> [!NOTE]
> Questa sezione si applica solo a [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Prima che un driver client invii un'istruzione T-SQL al server logico SQL di Azure per l'esecuzione, il driver attiva il flusso di lavoro di attestazione dell'enclave seguente usando l'attestazione di Microsoft Azure.

1. Il driver client passa l'URL di attestazione, specificato nella connessione di database, al server logico SQL di Azure.
2. Il server logico SQL di Azure raccoglie l'evidenza relativa all'enclave, l'ambiente di hosting e il codice in esecuzione all'interno dell'enclave. Il server invia quindi una richiesta di attestazione al provider di attestazioni, a cui si fa riferimento nell'URL di attestazione.
3. Il provider di attestazioni convalida l'evidenza in base ai criteri configurati e rilascia un token di attestazione al server logico SQL di Azure. Il provider di attestazioni firma il token di attestazione con la chiave privata.
4. Il server logico SQL di Azure invia il token di attestazione al driver client.
5. Il client contatta il provider di attestazioni all'URL di attestazione specificato per recuperare la chiave pubblica e verifica la firma nel token di attestazione.

Possono verificarsi errori in vari passaggi del flusso di lavoro descritto in precedenza a causa di problemi di configurazione. Di seguito sono riportati gli errori di attestazione comuni, le cause radice e i passaggi consigliati per la risoluzione dei problemi:

- Il server logico SQL di Azure non riesce a connettersi al provider di attestazioni nell'attestazione di Azure (passaggio 2 del flusso di lavoro precedente), specificato nell'URL di attestazione. Il problema potrebbe essere dovuto alle cause seguenti:
  - L'URL di attestazione non è corretto o non è completo. Per altre informazioni, vedere [Determinare l'URL di attestazione per i criteri di attestazione](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).
  - Il provider di attestazioni è stato eliminato accidentalmente.
  - Il firewall è stato configurato per il provider di attestazioni, ma non consente l'accesso ai servizi Microsoft.
  - Un errore di rete intermittente rende non disponibile il provider di attestazioni.
- Il server logico SQL di Azure non è autorizzato a inviare richieste di attestazione al provider di attestazioni. Assicurarsi che l'amministratore del provider di attestazioni abbia aggiunto il server di database al ruolo con autorizzazioni di lettura per le attestazioni. Per altre informazioni, vedere [Concedere al server di database SQL di Azure l'accesso al provider di attestazioni](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#grant-your-azure-sql-database-server-access-to-your-attestation-provider).
- La convalida dei criteri di attestazione ha esito negativo (nel passaggio 3 del flusso di lavoro precedente).
  - Un criterio di attestazione non corretto è la probabile causa radice. Assicurarsi di usare i criteri consigliati da Microsoft. Per altre informazioni, vedere [Creare e configurare un provider di attestazioni](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#create-and-configure-an-attestation-provider).
  - La convalida dei criteri può avere esito negativo anche a causa di una violazione della sicurezza che compromette l'enclave lato server.
- L'applicazione client non riesce a connettersi al provider di attestazioni e a recuperare la chiave di firma pubblica (nel passaggio 5). Il problema potrebbe essere dovuto alle cause seguenti:
  - La configurazione dei firewall tra l'applicazione e il provider di attestazioni può bloccare le connessioni. Per risolvere i problemi relativi alla connessione bloccata, verificare che sia possibile connettersi all'endpoint OpenId del provider di attestazioni. Usare, ad esempio, un Web browser nel computer che ospita l'applicazione per verificare se è possibile connettersi all'endpoint OpenID. Per altre informazioni, vedere [Configurazione dei metadati - Get](https://docs.microsoft.com/rest/api/attestation/metadataconfiguration/get).

## <a name="attestation-errors-when-using-host-guardian-service"></a>Errori di attestazione quando si usa il servizio Sorveglianza host

> [!NOTE]
> Questa sezione si applica solo a [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Prima che un driver client invii un'istruzione T-SQL a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per l'esecuzione, il driver attiva il flusso di lavoro di attestazione dell'enclave seguente usando il servizio Sorveglianza host.

1. Il driver client chiama [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] per avviare l'attestazione.
2. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] raccoglie l'evidenza relativa all'enclave, l'ambiente di hosting e il codice in esecuzione all'interno dell'enclave. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] richiede un certificato di integrità all'istanza del servizio Sorveglianza host, per cui è stato registrato il computer che ospita [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Registrare il computer per il servizio Sorveglianza host](always-encrypted-enclaves-host-guardian-service-register.md).
3. Il servizio Sorveglianza host convalida l'evidenza e rilascia il certificato di integrità a [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Il servizio Sorveglianza host firma il certificato di integrità con la chiave privata.
4. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] invia il certificato di integrità al driver client.
5. Il driver client contatta il servizio Sorveglianza host all'URL di attestazione, specificato per la connessione di database, per recuperare la chiave pubblica del servizio Sorveglianza host. Il driver client verifica la firma nel certificato di integrità.

Possono verificarsi errori in vari passaggi del flusso di lavoro descritto in precedenza a causa di problemi di configurazione. Di seguito sono riportati alcuni errori di attestazione comuni, le cause radice e i passaggi consigliati per la risoluzione dei problemi:

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] non riesce a connettersi al servizio Sorveglianza host (passaggio 2 del flusso di lavoro precedente) a causa di un errore di rete intermittente. Per risolvere il problema di connessione, l'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deve verificare che il computer riesca a connettersi al computer del servizio Sorveglianza host.
- La convalida nel passaggio 3 ha esito negativo. Per risolvere il problema di convalida:
  - L'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deve collaborare con l'amministratore dell'applicazione client per verificare che il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sia registrato per la stessa istanza del servizio Sorveglianza host dell'istanza a cui si fa riferimento nell'URL di attestazione sul lato client.
  - L'amministratore del computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deve verificare che il computer [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] possa eseguire l'attestazione correttamente, seguendo le istruzioni riportate in [Passaggio 5: Verificare che l'host possa essere eseguire l'attestazione correttamente](always-encrypted-enclaves-host-guardian-service-register.md#step-5-confirm-the-host-can-attest-successfully).
- L'applicazione client non riesce a connettersi al servizio Sorveglianza host e a recuperare la chiave di firma pubblica (nel passaggio 5). La probabile causa è:
  - La configurazione di uno dei firewall tra l'applicazione e il provider di attestazioni può bloccare le connessioni. Verificare che il computer che ospita l'app possa connettersi al computer del servizio Sorveglianza host.

## <a name="in-place-encryption-errors"></a>Errori di crittografia sul posto

Questa sezione elenca gli errori comuni che possono verificarsi quando si usa `ALTER TABLE`/`ALTER COLUMN` per la crittografia sul posto (oltre agli errori di attestazione descritti nelle sezioni precedenti). Per altre informazioni, vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md).

- La chiave di crittografia della colonna che si sta provando a usare per crittografare, decrittografare o crittografare nuovamente i dati non è una chiave abilitata per l'enclave. Per altre informazioni sui prerequisiti per la crittografia sul posto, vedere [Prerequisiti](always-encrypted-enclaves-configure-encryption.md#prerequisites). Per informazioni su come effettuare il provisioning delle chiavi abilitate per l'enclave, vedere [Effettuare il provisioning delle chiavi abilitate per l'enclave](always-encrypted-enclaves-provision-keys.md).
- Always Encrypted e i calcoli dell'enclave non sono stati abilitati per la connessione di database. Vedere [Prerequisiti per l'esecuzione di istruzioni con enclave sicure](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- L'istruzione `ALTER TABLE`/`ALTER COLUMN` attiva un'operazione di crittografia e modifica il tipo di dati della colonna o imposta le regole di confronto con una tabella codici diversa dalla tabella codici delle regole di confronto corrente. Non è consentito combinare le operazioni di crittografia con le modifiche al tipo di dati o alla pagina delle regole di confronto. Per risolvere il problema, usare istruzioni separate: un'istruzione per modificare il tipo di dati o la tabella codici delle regole di confronto e un'altra istruzione per la crittografia sul posto.

## <a name="errors-when-running-confidential-dml-queries-using-secure-enclaves"></a>Errori durante l'esecuzione di query DML riservate con enclave sicure

Questa sezione elenca gli errori comuni che possono verificarsi quando si eseguono query DML riservate con enclave sicure (oltre agli errori di attestazione descritti nelle sezioni precedenti).

- La chiave di crittografia configurata per la colonna su cui si esegue la query non è una chiave abilitata per l'enclave.
- Always Encrypted e i calcoli dell'enclave non sono stati abilitati per la connessione di database. Per altre informazioni, vedere [Prerequisiti per l'esecuzione di istruzioni con enclave sicure](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- La colonna su cui si esegue la query usa la crittografia deterministica. Le query DML riservate che usano enclave sicure non sono supportate con la crittografia deterministica. Per altre informazioni su come modificare il tipo di crittografia in casuale, vedere [Abilitare Always Encrypted con enclave sicure per le colonne crittografate esistenti](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- La colonna di stringhe su cui si esegue la query usa regole di confronto diverse da BIN2 o UTF-8. Cambiare le regole di confronto in BIN2 o UTF-8. Per altre informazioni, vedere [Istruzioni DML con enclave sicure](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
- La query attiva un'operazione non supportata. Per l'elenco di operazioni supportate all'interno delle enclave, vedere [Istruzioni DML con enclave sicure](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
## <a name="next-steps"></a>Passaggi successivi

- [Sviluppare applicazioni usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Vedere anche

- [Eseguire istruzioni Transact-SQL con enclave sicure](always-encrypted-enclaves-query-columns.md).
- [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-configure-encryption.md)
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md)