---
title: Configurazione della connettività di PolyBase (Transact-SQL) | Microsoft Docs
description: Informazioni su come usare sp_configure per visualizzare o modificare le impostazioni di configurazione globali per la connettività tra Hadoop PolyBase e Archiviazione BLOB di Azure.
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: polybase
ms.topic: reference
helpviewer_keywords:
- PolyBase
ms.assetid: 82252e4f-b1d0-49e5-aa0b-3624aade2add
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: ce94e2cc4e2b35b18e24dd652d2e93c9104a63e0
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584046"
---
# <a name="polybase-connectivity-configuration-transact-sql"></a>Configurazione della connettività di PolyBase (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  Visualizza o modifica le impostazioni di configurazione globali per la connettività tra PolyBase, Hadoop e l'archivio BLOB di Azure.
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
--List all of the configuration options  
sp_configure  
[;]  
  
--Configure Hadoop connectivity  
sp_configure [ @configname = ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ **@configname=** ] **'** _option\_name_ **'**  
 Nome di un'opzione di configurazione. *option_name* è **varchar(35)** e il valore predefinito è NULL. Se non si specifica alcun nome di opzione, viene restituito l'elenco completo delle opzioni.  
  
 [ **@configvalue=** ] **'** _value_ **'**  
 Nuova impostazione di configurazione. *value* è **int** e il valore predefinito è NULL. Il valore massimo dipende dalla singola opzione.  
  
 **'hadoop connectivity'**  
 Specifica il tipo di origine dati Hadoop per tutte le connessioni da PolyBase a cluster Hadoop o archivi BLOB di Azure (WASB). Questa impostazione è necessaria per creare un'origine dati esterna per una tabella esterna. Per altre informazioni, vedere [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
 Queste sono le impostazioni di connettività di Hadoop e le origini dati Hadoop supportate corrispondenti. Può essere attiva una sola impostazione alla volta. Le opzioni 1, 4 e 7 consentono la creazione e l'uso di più tipi di origini dati esterne in tutte le sessioni nel server.  
  
-   Opzione 0: disabilitazione della connettività Hadoop  
  
-   Opzione 1: Hortonworks HDP 1.3 in Windows Server  
  
-   Opzione 1: archivio BLOB di Azure (WASB[S])  
  
-   Opzione 2: Hortonworks HDP 1.3 in Linux  
  
-   Opzione 3: Cloudera CDH 4.3 su Linux  
  
-   Opzione 4: Hortonworks HDP 2.0 in Windows Server  
  
-   Opzione 4: archivio BLOB di Azure (WASB[S])  
  
-   Opzione 5: Hortonworks HDP 2.0 in Linux  
  
-   Opzione 6: Cloudera 5.1, 5.2, 5.3, 5.4, 5.5, 5.9, 5.10, 5.11, 5.12 e 5.13 in Linux  
  
-   Opzione 7: Hortonworks 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 3.0 in Linux  
  
-   Opzione 7: Hortonworks 2.1, 2.2, 2.3 e 3.1 <sup>*</sup> in Windows Server  
  
-   Opzione 7: archivio BLOB di Azure (WASB[S])  

   <sup>*</sup> Hortonworks 3.1 richiede SQL Server 2019 CU9 (15.0.4102) o versione successiva.

 **RECONFIGURE**  
 Aggiorna il valore di esecuzione (run_value) in modo che corrisponda al valore di configurazione (config_value). Vedere [Set di risultati](#ResultSets) per le definizioni di run_value e config_value. Il nuovo valore di configurazione impostato da sp_configure diventa effettivo solo dopo l'impostazione del valore di esecuzione con l'istruzione RECONFIGURE.  
  
 Dopo l'esecuzione di RECONFIGURE è necessario arrestare e riavviare il servizio SQL Server. Si noti che con l'arresto del servizio SQL Server verranno arrestati automaticamente i due servizi aggiuntivi Motore PolyBase e Polybase Data Movement. Dopo il riavvio del servizio SQL Server, riavviare questi due servizi, che non vengono riavviati automaticamente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
##  <a name="result-sets"></a><a name="ResultSets"></a> Set di risultati  
 Se eseguita senza parametri, **sp_configure** restituisce un set di risultati con cinque colonne.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nvarchar(35)**|Nome dell'opzione di configurazione.|  
|**minimum**|**int**|Valore minimo dell'opzione di configurazione.|  
|**maximum**|**int**|Valore massimo dell'opzione di configurazione.|  
|**config_value**|**int**|Valore impostato con **sp_configure**.|  
|**run_value**|**int**|Valore corrente usato da PolyBase. Questo valore viene impostato tramite l'esecuzione di RECONFIGURE.<br /><br /> The **config_value** e **run_value** sono in genere uguali, a meno che il valore non sia in corso di modifica.<br /><br /> Se la riconfigurazione è in corso, potrebbe essere necessario un riavvio per ottenere un valore di esecuzione accurato.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dopo aver eseguito RECONFIGURE, per rendere effettivo il valore di esecuzione di 'hadoop connectivity', è necessario riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
In [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], dopo aver eseguito RECONFIGURE, per rendere effettivo il valore di esecuzione di 'hadoop connectivity', è necessario riavviare l'area [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 L'istruzione RECONFIGURE non è consentita in una transazione esplicita o implicita.  
  
## <a name="permissions"></a>Autorizzazioni  
 Tutti gli utenti possono eseguire **sp_configure** senza parametri o con il parametro @configname .  
  
 Per modificare un valore di configurazione o per eseguire RECONFIGURE, è necessaria l'autorizzazione a livello di server **ALTER SETTINGS** o l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-list-all-available-configuration-settings"></a>R. Elencare tutte le impostazioni di configurazione disponibili  
 L'esempio seguente mostra come impostare ed elencare tutte le opzioni di configurazione.  
  
```  
EXEC sp_configure;  
```  
  
 Il risultato restituisce il nome dell'opzione seguito dai valori minimi e massimo per l'opzione. **config_value** è il valore che verrà usato da SQL o PolyBase dopo il completamento della riconfigurazione. **config_value** è il valore in uso. The **config_value** e **run_value** sono in genere uguali, a meno che il valore non sia in corso di modifica.  
  
### <a name="b-list-the-configuration-settings-for-one-configuration-name"></a>B. Elencare le impostazioni di configurazione per un nome di configurazione  
  
```  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="c-set-hadoop-connectivity"></a>C. Impostare la connettività Hadoop  
 Questo esempio imposta PolyBase sull'opzione 7. Questa opzione consente a PolyBase per creare e usare tabelle esterne su Hortonworks 2.1, 2.2 e 2.3 su Linux e Windows Server e nell'archivio BLOB di Azure. SQL potrebbe includere, ad esempio, 30 tabelle esterne di cui 7 fanno riferimento a dati in Hortonworks 2.1 su Linux, 4 in Hortonworks 2.2 su Linux, 7 in Hortonworks 2.3 su Linux e le altre 12 all'archivio BLOB di Azure.  
  
```  
--Configure external tables to reference data on Hortonworks 2.1, 2.2, and 2.3 on Linux, and Azure blob storage  
  
sp_configure @configname = 'hadoop connectivity', @configvalue = 7;  
GO  
  
RECONFIGURE  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
