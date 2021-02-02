---
title: Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted | Microsoft Docs
description: Informazioni su come impostare la configurazione di Always Encrypted per le colonne del database usando la procedura guidata Always Encrypted in SQL Server.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.alwaysencryptedwizard.encryption.f1
- sql13.swb.alwaysencryptedwizard.f1
- sql13.swb.alwaysencryptedwizard.masterkey.f1
helpviewer_keywords:
- Wizard, Always Encrypted
ms.assetid: 68daddc9-ce48-49aa-917f-6dec86ad5af5
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf502949e206b45bf7e1e332962c8c093d1bcb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236703"
---
# <a name="configure-column-encryption-using-always-encrypted-wizard"></a>Configurare la crittografia delle colonne usando la procedura guidata Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

La procedura guidata Always Encrypted è un potente strumento che consente di impostare la configurazione [Always Encrypted](always-encrypted-database-engine.md) desiderata per le colonne di database selezionate. In base alla configurazione corrente e alla configurazione di destinazione desiderata, la procedura guidata è in grado di crittografare una colonna, rimuovere la crittografia o riapplicarla, ad esempio usando una nuova chiave di crittografia della colonna o un tipo di crittografia diverso da quello in uso, configurato per la colonna. È possibile configurare più colonne in una singola sessione della procedura guidata.

La procedura guidata consente di crittografare le colonne con chiavi di crittografia della colonna esistenti. In alternativa, si può scegliere di generare una nuova chiave di crittografia della colonna oppure sia una nuova chiave di crittografia della colonna che una nuova chiave master di colonna. 

La procedura guidata funziona spostando i dati all'esterno del database ed eseguendo operazioni di crittografia all'interno del processo di SSMS. La procedura guidata crea una o più nuove tabelle con la configurazione di crittografia desiderata nel database, carica tutti i dati dalle tabelle originali, esegue le operazioni di crittografia richieste, carica i dati nelle nuove tabelle e quindi scambia le tabelle originali con le nuove tabelle.

> [!NOTE]
> L'esecuzione di operazioni di crittografia può richiedere molto tempo. Durante questo periodo, il database non è disponibile per la scrittura di transazioni. Per le operazioni di crittografia in tabelle di grandi dimensioni, lo strumento consigliato è PowerShell. Vedere [Configurare la crittografia delle colonne usando Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md).

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Se si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] e l'istanza di SQL Server è configurata con un'enclave sicura, è possibile eseguire le operazioni di crittografia sul posto, senza trasferire i dati all'esterno del database. Vedere [Configurare la crittografia delle colonne sul posto usando Always Encrypted con enclave sicure](always-encrypted-enclaves-configure-encryption.md). Si noti che la procedura guidata non supporta la crittografia sul posto.

::: moniker-end

Usare PowerShell è una procedura consigliata 

 - Per una procedura dettagliata completa che spiega come configurare Always Encrypted con la procedura guidata e come usarlo in un'applicazione client, vedere le esercitazioni seguenti sul database SQL di Azure:
    - [Proteggere i dati sensibili nel database SQL di Azure con Always Encrypted e le chiavi master di colonna nell'archivio certificati di Windows](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
    - [Proteggere i dati sensibili nel database SQL di Azure con Always Encrypted e le chiavi master di colonna in Azure Key Vault](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

 - Per informazioni sull'uso della procedura guidata, vedere il video relativo alla [protezione dei dati sensibili con Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted). Vedere anche il blog del team di sicurezza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sulla [procedura guidata per la crittografia di SSMS e su come abilitare Always Encrypted in pochi semplici passaggi](https://techcommunity.microsoft.com/t5/SQL-Server/SSMS-Encryption-Wizard-Enabling-Always-Encrypted-in-a-Few-Easy/ba-p/384545).  
 - Per altre informazioni sulle chiavi Always Encrypted, vedere [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md).
 - Per informazioni sui tipi di crittografia supportati in Always Encrypted, vedere [Selezione della crittografia deterministica o casuale](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).
 
 ## <a name="permissions"></a>Autorizzazioni
Per eseguire operazioni di crittografia tramite la procedura guidata sono necessarie le autorizzazioni **VIEW ANY COLUMN MASTER KEY DEFINITION** e **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION**. È inoltre necessario avere le autorizzazioni per accedere alle chiavi master di colonna in uso negli archivi che contengono le chiavi:
- **Archivio certificati - Computer locale**: è necessario avere accesso in lettura al certificato usato come chiave master della colonna o essere l'amministratore del computer.
- **Azure Key Vault**: sono necessarie le autorizzazioni get, unwrapKey e verify per l'insieme di credenziali contenente la chiave master della colonna.
- **Provider dell'archivio chiavi (CNG)** : l'autorizzazione e le credenziali obbligatorie potrebbero essere richieste quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione KSP.
- **Provider del servizio di crittografia (CAPI)** : potrebbero essere richieste l'autorizzazione e le credenziali obbligatorie quando si usa un archivio chiavi o una chiave, in base all'archivio e alla configurazione CSP.

Inoltre, se si stanno creando nuove chiavi tramite la procedura guidata, è necessario avere le autorizzazioni aggiuntive elencate in [Effettuare il provisioning delle chiavi master di colonna con la finestra di dialogo Nuova chiave master della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) e [Effettuare il provisioning delle chiavi di crittografia di colonna con la finestra di dialogo Nuova chiave di crittografia della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).

## <a name="open-the-always-encrypted-wizard"></a>Aprire la procedura guidata Always Encrypted
È possibile avviare la procedura guidata a tre livelli diversi: 
- A livello di database, se si vogliono crittografare più colonne situate in tabelle diverse.
- A livello di tabella, se si vogliono crittografare più colonne situate nella stessa tabella.
- A livello di colonna, se si vuole crittografare una specifica colonna.
 
 1. Connettersi a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con il componente Esplora oggetti di [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
   
 2. Per crittografare:
     1. Più colonne situate in una tabella diversa in un database, fare clic con il pulsante destro del mouse sul database, scegliere **Attività** e quindi selezionare **Crittografa colonne**.
     1. Più colonne situate nella stessa tabella, passare alla tabella, fare clic con il pulsante destro del mouse su di essa, quindi selezionare **Crittografa colonne**.
     1. Una singola colonna, passare alla colonna, fare clic con il pulsante destro del mouse su di essa, quindi selezionare **Crittografa colonne**.


   
 ## <a name="column-selection-page"></a>Pagina Selezione colonna
In questa pagina è possibile selezionare le colonne da crittografare, crittografare nuovamente o decrittografare, oltre che definire la configurazione di crittografia di destinazione per le colonne selezionate.

Per crittografare una colonna in testo non crittografato, selezionare un tipo di crittografia (**Deterministica** o **Casuale**) e una chiave di crittografia per la colonna. 

Per modificare un tipo di crittografia o per ruotare (modificare) una chiave di crittografia della colonna per una colonna già crittografata, selezionare il tipo di crittografia desiderato e la chiave. 

Se si vuole che la procedura guidata crittografi o ricrittografi una o più colonne usando una nuova chiave di crittografia della colonna, selezionare una chiave il cui nome contiene **(Nuova)** . La procedura guidata genererà la chiave.

Per decrittografare una colonna attualmente crittografata, selezionare il tipo di crittografia **Testo non crittografato**.


> [!NOTE]
> La procedura guidata non supporta le operazioni di crittografia in tabelle temporali e in memoria. È possibile creare tabelle temporali o in memoria vuote usando Transact-SQL e inserire dati usando l'applicazione.

## <a name="master-key-configuration-page"></a>Pagina Configurazione della chiave master
Se per qualsiasi colonna nella pagina precedente è stata selezionata una chiave di crittografia della colonna generata automaticamente, in questa pagina occorre selezionare una chiave master di colonna esistente oppure configurare una nuova chiave master di colonna che crittograferà la chiave di crittografia della colonna. 

Quando si configura una nuova chiave master di colonna, è possibile selezionare una chiave esistente nell'archivio certificati di Windows o in Azure Key Vault e fare in modo che la procedura guidata crei nel database solo un oggetto metadati per la chiave. In alternativa, si può scegliere di generare nel database sia la chiave che l'oggetto metadati che descrive la chiave. 

Per altre informazioni sulla creazione e l'archiviazione delle chiavi master di colonna nell'archivio certificati di Windows, in Azure Key Vault o in altri archivi di chiavi, vedere [Creare e archiviare chiavi master della colonna per Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!TIP]
> La procedura guidata consente di esplorare e creare chiavi solo nell'archivio certificati Windows e in Azure Key Vault. Inoltre, genera automaticamente i nomi delle nuove chiavi e degli oggetti metadati del database che descrivono le chiavi. Per avere un maggiore controllo sulla modalità di provisioning delle chiavi e più possibilità di scelta per un archivio chiavi contenente una chiave master della colonna, è possibile usare le finestre di dialogo **Nuova chiave master della colonna** e **Nuova chiave di crittografia della colonna** per creare le chiavi, quindi eseguire la procedura guidata e selezionare le chiavi create. Vedere [Effettuare il provisioning delle chiavi master di colonna con la finestra di dialogo Nuova chiave master della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) e [Effettuare il provisioning delle chiavi di crittografia di colonna con la finestra di dialogo Nuova chiave di crittografia della colonna](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). 

## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query sulle colonne usando Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Sviluppare applicazioni usando Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Vedere anche  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Panoramica della gestione delle chiavi per Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configurare Always Encrypted usando SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)
 - [Configurare la crittografia delle colonne usando Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md)
 - [Configurare la crittografia delle colonne usando Always Encrypted con un pacchetto di applicazione livello dati](configure-always-encrypted-using-dacpac.md)