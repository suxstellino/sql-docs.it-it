---
title: Documentazione sulla sicurezza per SQL Server e il database SQL di Azure
description: Riferimento ai contenuti relativi a sicurezza e protezione per SQL Server e il database SQL di Azure.
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799f00bfd7ddf0763212cefb00d8fea72a7dce27
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739921"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Centro di sicurezza per il motore di database di SQL Server e il database SQL di Azure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questa pagina vengono forniti i collegamenti che consentono di trovare informazioni su sicurezza e protezione nel [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 **Legenda**  
  
 ![Screenshot della legenda che spiega le icone per la disponibilità delle funzionalità.](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> Autenticazione: informazioni sull'utente  
  
|||  
|-|-|  
|**Scelta del tipo di autenticazione**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Autenticazione di Windows<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Active Directory|Scelta del tipo di autenticazione (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Scegliere una modalità di autenticazione](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Connettersi al Database SQL utilizzando l’autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)|  
|**Dove viene eseguita l'autenticazione**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Database master: account di accesso e utenti di database<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Database utente: utenti di database contenuti|Autenticazione a livello di database master (account di accesso e utenti di database)<br /><br /> [Creazione di un account di accesso di SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Gestione di database e account di accesso in database SQL di Azure](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [Creazione di un utente di database](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> Autenticazione a livello di database utente<br /><br /> [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Uso di altre identità**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Credenziali<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Esecuzione con un altro account di accesso<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Esecuzione con un altro utente di database|[Credenziali &#40;motore di database&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Esecuzione con un altro account di accesso](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Esecuzione con un altro utente di database](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> Autorizzazione: operazioni consentite  
  
|||  
|-|-|  
|**Concessione, revoca e negazione delle autorizzazioni**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Classi a protezione diretta<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Autorizzazioni server granulari<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Autorizzazioni di database granulari|[Gerarchia delle autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [Autorizzazioni](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Entità a protezione diretta](../../relational-databases/security/securables.md)<br /><br /> [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Sicurezza in base ai ruoli**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Ruoli a livello di server<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Ruoli a livello di database|[Ruoli a livello di server](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Limitazione dell'accesso ai dati agli elementi di dati selezionati**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Limitare l'accesso ai dati con viste/routine<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Sicurezza a livello di riga<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Dynamic Data Masking<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Oggetti firmati|Uso di [viste](../../relational-databases/views/views.md) e [routine](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)per limitare l'accesso ai dati<br /><br /> [Sicurezza a livello di riga (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [Sicurezza a livello di riga (Database SQL di Azure)](./row-level-security.md)<br /><br /> [Mascheramento dati dinamici (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Mascheramento dati dinamici (Database SQL di Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [Oggetti firmati](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> Crittografia: archiviazione di dati segreti  
  
|||  
|-|-|  
|**Crittografia dei file**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Crittografia BitLocker (a livello di unità)<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Crittografia NTFS (a livello di cartella)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Transparent Data Encryption (a livello di file)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Crittografia dei backup (a livello di file)|[BitLocker (a livello di unità)](https://support.microsoft.com/kb/2855131)<br /><br /> [Crittografia NTFS (a livello di cartella)](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [Transparent Data Encryption (a livello di file)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Crittografia dei backup (a livello di file)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Crittografia delle origini**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Modulo Extensible Key Management<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Chiavi archiviate in Azure Key Vault<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[Modulo Extensible Key Management](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Chiavi archiviate nell'insieme di credenziali di Azure](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Crittografia di colonne, dati e chiavi**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Crittografia mediante certificato<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Crittografia mediante chiave simmetrica<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Crittografia mediante chiave asimmetrica<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Crittografia mediante passphrase|[Crittografia mediante certificato](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Crittografia mediante chiave asimmetrica](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Crittografia mediante chiave simmetrica](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Crittografia mediante passphrase](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Crittografia di una colonna di dati](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> Sicurezza della connessione: limitazioni e protezione  
  
|||  
|-|-|  
|**Protezione con firewall**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Impostazioni di Windows Firewall<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Impostazioni del firewall dei servizi di Azure<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Impostazioni del firewall del database|[Configurare Windows Firewall per l'accesso al motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Impostazioni del firewall del database SQL di Azure](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Impostazioni del firewall dei servizi di Azure](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Crittografia dei dati in transito**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Connessioni SSL forzate<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Connessioni SSL facoltative|[Abilitare connessioni crittografate al motore di database](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [Abilitare connessioni crittografate al motore di database](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md), [Sicurezza di rete](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [Supporto di TLS 1.2 per Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> Controllo: registrazione dell'accesso  
  
|||  
|-|-|  
|**Controllo automatizzato**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit (livello server e database)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Audit (livello database)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Rilevare le minacce| <br /><br /> [SQL Server Audit &#40;Database Engine&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [Controllo del database SQL](/azure/azure-sql/database/auditing-overview)<br /><br /> [Introduzione ad Advanced Threat Protection per il database SQL](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [Valutazione delle vulnerabilità del database SQL](/azure/sql-database/sql-vulnerability-assessment) |  
|**Controllo personalizzato**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Trigger|Implementazione di controlli personalizzati: Creazione di [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) e di [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**Conformità**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Conformità|SQL Server:<br />                        [Criteri comuni](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> Database SQL:<br />                        [Centro protezione di Microsoft Azure: conformità per funzionalità](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> Attacco intrusivo nel codice SQL  
 In un attacco SQL injection il malware viene inserito in stringhe successivamente passate al [!INCLUDE[ssDE](../../includes/ssde-md.md)] per l'analisi e l'esecuzione. Per la prevenzione degli attacchi di questo tipo è necessario esaminare tutte le procedure che creano istruzioni SQL, poiché in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono eseguite tutte le query sintatticamente valide ricevute. Tutti i sistemi di database sono a rischio di attacchi SQL injection e molte delle vulnerabilità vengono introdotte nell'applicazione che sta eseguendo una query sul [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Per contrastare gli attacchi SQL injection è possibile usare stored procedure e comandi con parametri, evitando l'SQL dinamico e limitando le autorizzazioni per tutti gli utenti.  Per altre informazioni, vedere [Attacco intrusivo nel codice SQL](../../relational-databases/security/sql-injection.md).  
  
 Collegamenti aggiuntivi per i programmatori di applicazioni:  
  
-   [Scenari di sicurezza delle applicazioni in SQL Server](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [Scrittura di istruzioni SQL dinamiche protette in SQL Server](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [Procedura: Proteggere da attacchi SQL injection in ASP.NET](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>Vedere anche  
 [Introduzione alle autorizzazioni del motore di database](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Sicurezza di SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Certificati SQL Server e chiavi simmetriche](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [Crittografia di SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Configurazione superficie di attacco](../../relational-databases/security/surface-area-configuration.md)   
 [Password complesse](../../relational-databases/security/strong-passwords.md)   
 [Proprietà di database TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Caratteristiche e attività del motore di database](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [Protezione della proprietà intellettuale di SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]