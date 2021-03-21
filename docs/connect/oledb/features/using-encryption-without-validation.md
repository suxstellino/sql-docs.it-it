---
title: Uso della crittografia senza convalida | Microsoft Docs
description: Informazioni sulla crittografia senza convalida per le connessioni SQL Server. OLE DB Driver per SQL Server supporta la crittografia senza convalida.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], encryption
- cryptography [OLE DB Driver for SQL Server]
- MSOLEDBSQL, encryption
- encryption [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, encryption
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 805350b09c624709fa1ea6746c2e97eb2bdbed59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751121"
---
# <a name="using-encryption-without-validation"></a>Utilizzo della crittografia senza convalida
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crittografa sempre pacchetti di rete associati all'accesso. Se non è stato eseguito il provisioning di nessun certificato nel server quando viene avviato, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] genera un certificato autofirmato utilizzato per crittografare pacchetti di accesso.  

I certificati autofirmati non garantiscono la sicurezza. Lo handshake crittografato è basato su NT LAN Manager (NTLM). È consigliabile eseguire il provisioning di un certificato verificabile in SQL Server per garantire una connettività sicura. Il protocollo TLS (Transport Security Layer) può essere reso sicuro solo con la convalida del certificato.

Le applicazioni possono inoltre richiedere l'attivazione della crittografia per tutto il traffico di rete mediante le parole chiave della stringa di connessione o le proprietà di connessione. Le parole chiave sono "Encrypt" per OLE DB se si usa una stringa del provider con **IDbInitialize::Initialize** o "Use Encryption for Data" per ADO e OLE DB se si usa una stringa di inizializzazione con **IDataInitialize**. Questa configurazione può essere eseguita anche da Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando l'opzione **Forza crittografia protocollo** e configurando il client in modo che richieda connessioni crittografate. Per impostazione predefinita, la crittografia di tutto il traffico di rete per una connessione richiede che nel server sia stato eseguito il provisioning di un certificato. Se si imposta il client in modo che consideri attendibile il certificato nel server, l'ambiente può risultare vulnerabile agli attacchi man-in-the-middle. Se si distribuisce un certificato verificabile nel server, assicurarsi di impostare le opzioni client relative all'attendibilità del certificato su FALSE.

Per informazioni sulle parole chiave della stringa di connessione, vedere [Uso delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md ).  
  
 Per abilitare l'uso della crittografia quando nel server non è stato eseguito il provisioning di un certificato, è possibile usare Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per impostare le opzioni **Forza crittografia protocollo** e **Certificato server attendibile**. In questo caso, la crittografia utilizzerà un certificato server autofirmato senza convalida se nel server non è stato eseguito il provisioning di alcun certificato verificabile.  
  
 Le applicazioni possono inoltre utilizzare la parola chiave "TrustServerCertificate" o il relativo attributo di connessione associato per garantire l'utilizzo della crittografia. Le impostazioni dell'applicazione non riducono mai il livello di sicurezza impostato dal client di Gestione configurazione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], bensì possono potenziarlo. Un'applicazione può ad esempio richiedere la crittografia, se non è stata impostata l'opzione **Forza crittografia protocollo** per il client. Per garantire che la crittografia venga applicata anche quando non è stato eseguito il provisioning di un certificato server, un'applicazione può richiedere la crittografia e "TrustServerCertificate". Tuttavia, se TrustServerCertificate non è attivata nella configurazione client, è comunque necessario il provisioning di un certificato server. Nella tabella seguente vengono descritti tutti i casi:  
  
|Impostazione client Forza crittografia protocollo|Impostazione client Considera attendibile certificato server|Attributo/stringa di connessione Encrypt/Use Encryption for Data|Attributo/stringa di connessione Trust Server Certificate|Risultato|  
|----------------------------------------------|---------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|------------|  
|No|N/D|No (impostazione predefinita)|Ignorato|Nessuna crittografia.|  
|No|N/D|Sì|No (impostazione predefinita)|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|No|N/D|Sì|Sì|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  
|Sì|No|Ignorato|Ignorato|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|Sì|Sì|No (impostazione predefinita)|Ignorato|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  
|Sì|Sì|Sì|No (impostazione predefinita)|La crittografia viene applicata solo se è disponibile un certificato server verificabile; in caso contrario, il tentativo di connessione non riesce.|  
|Sì|Sì|Sì|Sì|La crittografia viene sempre applicata, ma può essere utilizzato un certificato server auto-firmato.|  
||||||

> [!CAUTION]
> La tabella precedente è solo una guida al comportamento del sistema con configurazioni diverse. Per una connettività sicura, verificare che sia il client sia il server richiedano la crittografia. Assicurarsi anche che il server abbia un certificato verificabile e che l'impostazione **TrustServerCertificate** nel client sia FALSE.

## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server 
 Il provider OLE DB per SQL Server supporta la crittografia senza convalida tramite l'aggiunta della proprietà di inizializzazione dell'origine dati SSPROP_INIT_TRUST_SERVER_CERTIFICATE, implementata nel set di proprietà DBPROPSET_SQLSERVERDBINIT. È stata inoltre aggiunta una nuova parola chiave, "TrustServerCertificate", per la stringa di connessione. Accetta i valori yes o no. Il valore predefinito è no. Quando si utilizzano i componenti del servizio, accetta i valori true o false; false è l'impostazione predefinita.  
  
 Per altre informazioni sui miglioramenti apportati al set di proprietà DBPROPSET_SQLSERVERDBINIT, vedere [Proprietà di inizializzazione e di autorizzazione](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md).  

  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
