---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a3859eea7237eebb83b994fef037e377b433940
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212106"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questa vista del catalogo contiene una riga per l'endpoint di Service Broker. Per ogni riga in questa visualizzazione è presente una riga corrispondente con lo stesso **endpoint_id** nella visualizzazione **sys.tcp_endpoints** che contiene i metadati di configurazione TCP. TCP è l'unico protocollo consentito in Service Broker.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Eredita le colonne da [sys. endpoints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_message_forwarding_enabled**|**bit**|Specifica se l'endpoint supporta l'inoltro di messaggi. Inizialmente è impostato su **0** (disabilitato). Non ammette i valori Null.|  
|**message_forwarding_size**|**int**|Numero massimo di megabyte di spazio di **tempdb** consentito per l'invio dei messaggi. Inizialmente è impostato su **10**. Non ammette i valori Null.|  
|**connection_auth**|**tinyint**|Tipo di autenticazione della connessione necessario per le connessioni all'endpoint. I possibili valori sono i seguenti:<br /><br /> **1** -NTLM<br /><br /> **2** -Kerberos<br /><br /> **3** -negoziazione<br /><br /> **4** -certificato<br /><br /> **5** -NTLM, certificato<br /><br /> **6** -Kerberos, certificato<br /><br /> **7** -negoziazione, certificato<br /><br /> **8** : certificato, NTLM<br /><br /> **9** -certificato, Kerberos<br /><br /> **10** : certificato, negoziazione<br /><br /> Non ammette i valori Null.|  
|**connection_auth_desc**|**nvarchar(60)**|Descrizione del tipo di autenticazione della connessione necessario per le connessioni all'endpoint. I possibili valori sono i seguenti:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE<br /><br /> Ammette valori Null.|  
|**certificate_id**|**int**|ID del certificato utilizzato per l'autenticazione, se disponibile.<br /><br /> 0 = viene utilizzata l'autenticazione di Windows.|  
|**encryption_algorithm**|**tinyint**|Algoritmo di crittografia. Di seguito sono riportati i valori possibili con le relative descrizioni e le opzioni DDL corrispondenti.<br /><br /> **0** : nessuna. Opzione DDL corrispondente: disabilitata.<br /><br /> **1** : RC4. Opzione DDL corrispondente: {required &#124; algoritmo obbligatorio RC4}.<br /><br /> **2** : AES. Opzione DDL corrispondente: algoritmo obbligatorio AES.<br /><br /> **3** : nessuna, RC4. Opzione DDL corrispondente: {supported &#124; algoritmo supportato RC4}.<br /><br /> **4** : nessuno, AES. Opzione DDL corrispondente: AES algoritmo supportato.<br /><br /> **5** : RC4, AES. Opzione DDL corrispondente: algoritmo obbligatorio RC4 AES.<br /><br /> **6** : AES, RC4. Opzione DDL corrispondente: algoritmo obbligatorio AES RC4.<br /><br /> **7** : None, RC4, AES. Opzione DDL corrispondente: algoritmo supportato RC4 AES.<br /><br /> **8** : nessuna, AES, RC4. Opzione DDL corrispondente: algoritmo supportato AES RC4.<br /><br /> Non ammette i valori Null.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Descrizione dell'algoritmo di crittografia. Di seguito sono elencati i valori possibili e le opzioni DDL corrispondenti:<br /><br /> Nessuno: disabilitato<br /><br /> RC4: {required &#124; algoritmo obbligatorio RC4}<br /><br /> AES: algoritmo obbligatorio AES<br /><br /> NONE, RC4: {supported &#124; algoritmo supportato RC4}<br /><br /> NESSUNO, AES: algoritmo supportato AES<br /><br /> RC4, AES: algoritmo obbligatorio RC4 AES<br /><br /> AES, RC4: algoritmo obbligatorio AES RC4<br /><br /> NONE, RC4, AES: algoritmo supportato RC4 AES<br /><br /> NONE, AES, RC4: algoritmo supportato AES RC4<br /><br /> Ammette valori Null.|  
  
## <a name="remarks"></a>Commenti  
  
> [!NOTE]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
