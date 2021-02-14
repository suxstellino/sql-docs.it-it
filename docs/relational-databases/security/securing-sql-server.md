---
title: Sicurezza di SQL Server | Microsoft Docs
description: Usare questi articoli per creare e implementare un piano di sicurezza efficace in SQL Server. Informazioni su piattaforma, autenticazione, oggetti e applicazioni.
ms.custom: ''
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- database objects [SQL Server], security
- SQL Server, security
- operating systems [SQL Server], security
- security [SQL Server], planning
- applications [SQL Server], security
ms.assetid: 4d93489e-e9bb-45b3-8354-21f58209965d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2f3082f83296efe396fa2fca447782e13588df38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340123"
---
# <a name="securing-sql-server"></a>Sicurezza di SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

E possibile considerare la sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come una serie di passaggi che interessano quattro aree: la piattaforma, l'autenticazione, gli oggetti (inclusi i dati) e le applicazioni che accedono al sistema. Gli argomenti seguenti guideranno l'utente nella creazione e implementazione di un piano di sicurezza efficace.  
  
 Altre informazioni sulla sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili sul sito Web di [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) . che include una guida alle procedure consigliate e un elenco di controllo per la sicurezza. Questo sito contiene inoltre informazioni aggiornate sul service pack e i download più recenti.  
  
## <a name="platform-and-network-security"></a>Sicurezza della piattaforma e della rete  
 La piattaforma per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprende l'hardware fisico e i sistemi di rete che collegano i client ai server di database, nonché i file binari utilizzati per elaborare le richieste di database.  
  
### <a name="physical-security"></a>Sicurezza fisica  
 Le procedure consigliate per la sicurezza fisica limitano rigorosamente l'accesso al server fisico e ai componenti hardware. Le stanze in cui si trovano l'hardware del server di database e i dispositivi di rete, ad esempio, devono essere chiuse a chiave ed essere ad accesso limitato e i supporti di backup devono essere conservati in un luogo sicuro, fuori sede, per limitarvi l'accesso.  
  
 L'implementazione della sicurezza della rete fisica inizia dall'allontanamento degli utenti non autorizzati dalla rete. Nella tabella seguente è indicato come ottenere ulteriori informazioni sulla sicurezza della rete.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] e accesso di rete ad altre edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|"Configurazione e protezione dell'ambiente server" nella documentazione online di [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="operating-system-security"></a>Sicurezza del sistema operativo  
 I service pack e gli aggiornamenti del sistema operativo comprendono importanti miglioramenti della sicurezza. Applicare tutti gli aggiornamenti al sistema operativo dopo averne eseguito il test con le applicazioni di database.  
  
 Anche i firewall rappresentano un metodo efficace per implementare la sicurezza. In termini logici, un firewall è un separatore o limitatore del traffico di rete che può essere configurato per applicare i criteri di sicurezza dei dati stabiliti dall'organizzazione. L'utilizzo di un firewall consente di aumentare la sicurezza a livello del sistema operativo, garantendo un punto di limitazione in cui è possibile implementare le misure di sicurezza. La tabella seguente contiene ulteriori informazioni sull'utilizzo di un firewall con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Configurazione di un firewall per l'utilizzo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurare Windows Firewall per l'accesso al motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Configurazione di un firewall per l'utilizzo con [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[Servizio Integration Services &#40;servizio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md)|  
|Configurazione di un firewall per l'utilizzo con [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|[Configurare Windows Firewall per consentire l'accesso ad Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|  
|Apertura di porte specifiche su un firewall per consentire l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|  
|Configurazione del supporto per la protezione estesa per l'autenticazione tramite associazione di canale e associazione al servizio|[Connessione al motore di database mediante la protezione estesa](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)|  
| &nbsp; | &nbsp; |
  
 La riduzione della superficie di attacco è una misura di sicurezza che implica l'arresto o la disabilitazione dei componenti non utilizzati. Consente di migliorare la sicurezza offrendo un minor numero di percorsi per potenziali attacchi a un sistema. Per limitare la superficie di attacco di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è consigliabile eseguire i servizi necessari con privilegi minimi, garantendo a servizi e utenti solo i diritti appropriati. Nella tabella seguente è indicato come ottenere ulteriori informazioni su servizi e accesso al sistema.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Servizi richiesti per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)|  
| &nbsp; | &nbsp; |
  
 Se il sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa Internet Information Services (IIS), per proteggere la superficie della piattaforma sono necessari ulteriori passaggi. Nella tabella seguente è indicato come ottenere informazioni su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Internet Information Services.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Sicurezza di IIS con [!INCLUDE[ssEW](../../includes/ssew-md.md)]|"Sicurezza IIS" nella documentazione online di [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Autenticazione|[Autenticazione in Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)] e accesso a IIS|"Diagramma di flusso della sicurezza di Internet Information Services" nella documentazione online di [!INCLUDE[ssEW](../../includes/ssew-md.md)]|  
| &nbsp; | &nbsp; |
  
### <a name="sql-server-operating-system-files-security"></a>Sicurezza dei file del sistema operativo con SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa file del sistema operativo per il funzionamento e l'archiviazione dei dati. Le procedure consigliate per la sicurezza dei file richiedono la restrizione dell'accesso a tali file. Nella tabella riportata di seguito sono disponibili informazioni su questi file.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] file di programma|[Percorsi dei file per le istanze predefinite e denominate di SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)|  
| &nbsp; | &nbsp; |
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service pack e aggiornamenti garantiscono una sicurezza avanzata. Per determinare quali sono i service pack più recenti disponibili per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere il sito Web di [SQL Server](https://go.microsoft.com/fwlink/?LinkID=31629) .  
  
 È possibile utilizzare lo script seguente per determinare il service pack installato nel sistema:  
  
```sql
SELECT CONVERT(char(20), SERVERPROPERTY('productlevel'));  
```  
  
## <a name="principals-and-database-object-security"></a>Sicurezza di entità e oggetti di database  
 Per entità si intendono individui, gruppi e processi a cui viene concesso l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le "entità a protezione diretta" sono il server, il database e gli oggetti contenuti nel database. Ogni entità dispone di un set di autorizzazioni configurabili per ridurre la superficie di attacco di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nella tabella seguente sono disponibili informazioni su entità ed entità a protezione diretta.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Utenti del server e del database, ruoli e processi|[Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)|  
|Sicurezza del server e degli oggetti di database|[Entità a protezione diretta](../../relational-databases/security/securables.md)|  
|Gerarchia della sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Gerarchia delle autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)|  
| &nbsp; | &nbsp; |
  
### <a name="encryption-and-certificates"></a>Crittografia e certificati  
 La crittografia non risolve i problemi relativi al controllo di accesso, ma migliora la sicurezza limitando la perdita di dati anche nel raro caso in cui il controllo degli accessi venga eluso. Se, ad esempio, il computer host del database è configurato scorrettamente e un utente malintenzionato ottiene dati sensibili, ad esempio il numero di una carta di credito, le informazioni sottratte potrebbero essere inutilizzabili se crittografate. Nella tabella seguente sono disponibili ulteriori informazioni sulla crittografia in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Gerarchia della crittografia in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)|  
|Implementazione di connessioni protette|[Abilitazione di connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)|  
|Funzioni di crittografia|[Funzioni di crittografia &#40;Transact-SQL&#41;](../../t-sql/functions/cryptographic-functions-transact-sql.md)|  
  
 I certificati sono chiavi software condivise tra due server che consentono comunicazioni protette grazie all'autenticazione avanzata. È possibile creare e utilizzare certificati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per migliorare la sicurezza degli oggetti e della connessione. La tabella seguente contiene informazioni sull'utilizzo di certificati con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Creazione di un certificato per l'uso con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)|  
|Utilizzo di un certificato con mirroring del database|[Utilizzare certificati per un endpoint del mirroring del database &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="application-security"></a>Sicurezza dell'applicazione

### <a name="client-programs"></a>Programmi client

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le procedure consigliate per la sicurezza comprendono la scrittura di applicazioni client protette. Per altre informazioni sulla protezione delle applicazioni client a livello di rete, vedere [Configurazione di rete dei client](../../database-engine/configure-windows/client-network-configuration.md).

### <a name="windows-defender-application-control-wdac"></a>Controllo di applicazioni di Windows Defender (WDAC)

<!--
This next live paragraph, about Windows Defender Application Control (WDAC), was requested by Bella Brahm, 2019/06/20. (GeneMi)

WDAC can also prevent the kind of highly sophisticated 'Nansh0u' attacks described in 'https://www.guardicore.com/2019/05/nansh0u-campaign-hackers-arsenal-grows-stronger/'. That webpage recommends this present article.
-->

Controllo di applicazioni di Windows Defender (WDAC, Windows Defender Application Control) impedisce l'esecuzione di codice non autorizzato. WDAC è un metodo efficace per attenuare la minaccia di malware basato su file eseguibile. Per altre informazioni, vedere la documentazione di [Controllo di applicazioni di Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control).

## <a name="sql-server-security-tools-utilities-views-and-functions"></a>Strumenti di sicurezza, utilità, viste e funzioni di SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprende strumenti, utilità, viste e funzioni utilizzabili per configurare e amministrare la sicurezza.  
  
### <a name="sql-server-security-tools-and-utilities"></a>Strumenti di sicurezza e utilità di SQL Server  
 Nella tabella seguente è indicato come ottenere informazioni sugli strumenti e le utilità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzabili per configurare e amministrare la sicurezza.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Connessione, configurazione e controllo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Utilizzo di SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)|  
|Connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed esecuzione di query dal prompt dei comandi|[Utilità sqlcmd](../../tools/sqlcmd-utility.md)|  
|Configurazione e controllo della rete per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md)|  
|Funzionalità di abilitazione e disabilitazione utilizzando la gestione basata sui criteri|[Amministrazione di server tramite la gestione basata su criteri](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)|  
|Modifica di chiavi simmetriche per un server di report|[Utilità rskeymgmt &#40;SSRS&#41;](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)|  
| &nbsp; | &nbsp; |

### <a name="sql-server-security-catalog-views-and-functions"></a>Viste e funzioni del catalogo relativo alla sicurezza di SQL Server  
 Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] espone informazioni sulla sicurezza in diverse viste e funzioni ottimizzate in termini di prestazioni e usabilità. Nella tabella seguente è indicato come ottenere informazioni sulle viste e sulle funzioni di sicurezza.  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Viste del catalogo relativo alla sicurezza che restituiscono informazioni su autorizzazioni, entità, ruoli e così via a livello di database e di server. Sono presenti viste del catalogo che forniscono informazioni su chiavi di crittografia, certificati e credenziali.|[Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Funzioni di sicurezza che restituiscono informazioni su utente corrente, autorizzazioni e schemi.|[Funzioni di sicurezza &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Viste a gestione dinamica relative alla sicurezza.|[Funzioni e viste a gestione dinamica relative alla sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)|  
| &nbsp; | &nbsp; |

## <a name="related-content"></a>Contenuto correlato  
 [Considerazioni sulla sicurezza per un'installazione di SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 [Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
[Procedure consigliate per la sicurezza di SQL Server 2012 - Attività operative e amministrative](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)   
[Blog sulla sicurezza di SQL Server](/archive/blogs/sqlsecurity/)  
[Procedura consigliata per la sicurezza e white paper di Label Security Toolkit](/archive/blogs/sqlsecurity/security-best-practice-and-label-security-whitepapers)  
[Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md)   
[Protezione della proprietà intellettuale di SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
