---
description: sp_pdw_add_network_credentials (analisi delle sinapsi di Azure)
title: sp_pdw_add_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 68a4abf4e532719d67032c2eeb64a0027c1c88a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271798"
---
# <a name="sp_pdw_add_network_credentials-azure-synapse-analytics"></a>sp_pdw_add_network_credentials (analisi delle sinapsi di Azure)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  In questo modo vengono archiviate le credenziali di rete in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] e associate a un server. Usare, ad esempio, questo stored procedure per concedere [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] le autorizzazioni di lettura/scrittura appropriate per eseguire operazioni di backup e ripristino del database in un server di destinazione o per creare una copia di backup di un certificato usato per Transparent Data Encryption.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', 'password'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti  
 '*target_server_name*'  
 Specifica il nome host o l'indirizzo IP del server di destinazione. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] consente di accedere a questo server utilizzando le credenziali di nome utente e password passate a questo stored procedure.  
  
 Per connettersi tramite la rete InfiniBand, utilizzare l'indirizzo IP InfiniBand del server di destinazione.  
  
 *target_server_name* è definito come nvarchar (337).  
  
 '*user_name*'  
 Specifica la user_name che dispone delle autorizzazioni per accedere al server di destinazione. Se per il server di destinazione sono già presenti credenziali, queste verranno aggiornate con le nuove credenziali.  
  
 *user_name* è definito come nvarchar (513).  
  
 '*password* ꞌ  
 Specifica la password per *user_name*.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione **ALTER server state** .  
  
## <a name="error-handling"></a>Gestione degli errori  
 Si verifica un errore se l'aggiunta di credenziali non riesce nel nodo di controllo e in tutti i nodi di calcolo.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Questa stored procedure aggiunge le credenziali di rete per l'account NetworkService [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . L'account NetworkService esegue ogni istanza di SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sul nodo di controllo e sui nodi di calcolo. Quando si esegue un'operazione di backup, ad esempio, il nodo di controllo e ogni nodo di calcolo utilizzeranno le credenziali dell'account NetworkService per ottenere le autorizzazioni di lettura e scrittura per il server di destinazione.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>R. Aggiungere le credenziali per l'esecuzione di un backup del database  
 Nell'esempio seguente vengono associate le credenziali nome utente e password per l'utente di dominio seattle\david con un server di destinazione con un indirizzo IP 10.172.63.255. L'utente seattle\david dispone delle autorizzazioni di lettura/scrittura per il server di destinazione. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] le credenziali vengono archiviate e utilizzate per la lettura e la scrittura da e verso il server di destinazione, in base alle esigenze per le operazioni di backup e ripristino.  
  
```sql  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 Per il comando backup è necessario immettere il nome del server come indirizzo IP.  
  
> [!NOTE]  
>  Per eseguire il backup del database su InfiniBand, assicurarsi di usare l'indirizzo IP InfiniBand del server di backup.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_pdw_remove_network_credentials &#40;Azure sinapsi Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

