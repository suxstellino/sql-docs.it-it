---
description: sp_configure (Transact-SQL)
title: sp_configure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_configure
- sp_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_configure
ms.assetid: d18b251d-b37a-4f5f-b50c-502d689594c8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: af05732674dcf09c6bd828125df5fe4773b62018
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071362"
---
# <a name="sp_configure-transact-sql"></a>sp_configure (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)]

  Visualizza o modifica le impostazioni di configurazione globali per il server corrente.

> [!NOTE]  
> Per le opzioni di configurazione a livello di database, vedere [ALTER database scoped configuration &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Per configurare Soft-NUMA, vedere [&#41;Soft-numa &#40;SQL Server ](../../database-engine/configure-windows/soft-numa-sql-server.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server  
  
sp_configure [ [ @configname = ] 'option_name'   
    [ , [ @configvalue = ] 'value' ] ]  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
-- List all of the configuration options  
sp_configure  
[;]  
  
-- Configure Hadoop connectivity  
sp_configure [ @configname= ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @configname = ] 'option_name'` Nome di un'opzione di configurazione. *option_name* è **varchar(35)** e il valore predefinito è NULL. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] riconosce qualsiasi stringa univoca che faccia parte del nome della configurazione. Se non si specifica alcun nome di opzione, viene restituito l'elenco completo delle opzioni.  
  
 Per informazioni sulle opzioni di configurazione disponibili e sulle relative impostazioni, vedere [Opzioni di configurazione del Server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
`[ @configvalue = ] 'value'` Nuova impostazione di configurazione. *value* è **int** e il valore predefinito è NULL. Il valore massimo dipende dalla singola opzione.  
  
 Per visualizzare il valore massimo per ogni opzione, vedere la colonna **Maximum** della vista del catalogo **sys.configurations** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 Quando viene eseguito senza parametri, **sp_configure** restituisce un set di risultati con cinque colonne e ordina le opzioni alfabeticamente in ordine crescente, come illustrato nella tabella seguente.  
  
 I valori per **config_value** e **run_value** non sono automaticamente equivalenti. Dopo l'aggiornamento di un'impostazione di configurazione utilizzando **sp_configure**, l'amministratore di sistema deve aggiornare il valore di configurazione in esecuzione utilizzando riconfigure o RECONFIGURE with override. Per altre informazioni, vedere la sezione Osservazioni.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nvarchar(35)**|Nome dell'opzione di configurazione.|  
|**minimum**|**int**|Valore minimo dell'opzione di configurazione.|  
|**maximum**|**int**|Valore massimo dell'opzione di configurazione.|  
|**config_value**|**int**|Valore in cui è stata impostata l'opzione di configurazione utilizzando **sp_configure** (valore in **sys.configurations. value**). Per ulteriori informazioni su queste opzioni, vedere [Opzioni di configurazione del Server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md) e [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
|**run_value**|**int**|Valore corrente dell'opzione di configurazione (valore in **sys.configurations.value_in_use**).<br /><br /> Per ulteriori informazioni, vedere [sys.configurations &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare **sp_configure** per visualizzare o modificare le impostazioni a livello di server. Per modificare le impostazioni a livello di database, utilizzare ALTER DATABASE. Per modificare le impostazioni che interessano solo la sessione utente corrente, utilizzare l'istruzione SET.  
  
### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="updating-the-running-configuration-value"></a>Aggiornamento del valore di configurazione corrente  
 Quando si specifica un nuovo *valore* per un' *opzione*, il set di risultati Mostra questo valore nella colonna **config_value** . Questo valore inizialmente è diverso dal valore nella colonna **run_value** , che mostra il valore di configurazione attualmente in esecuzione. Per aggiornare il valore di configurazione in esecuzione nella colonna **run_value** , l'amministratore di sistema deve eseguire RECONFIGURE o RECONFIGURE with override.  
  
 Sia RECONFIGURE che RECONFIGURE WITH OVERRIDE funzionano con tutte le opzioni di configurazione. L'istruzione RECONFIGURE, tuttavia, non accetta i valori di opzione che non rientrano in un intervallo ragionevole o che possono causare conflitti tra le opzioni. Riconfigura, ad esempio, genera un errore se il valore dell' **intervallo di recupero** è maggiore di 60 minuti o se il valore della **maschera di affinità** si sovrappone al valore di **affinity i/O mask** . RECONFIGURE WITH OVERRIDE, invece, accetta qualsiasi valore di opzione con il tipo di dati corretto e impone la riconfigurazione utilizzando il valore specificato.  
  
> [!CAUTION]  
> Un valore non corretto può compromettere la configurazione dell'istanza del server. Utilizzare RECONFIGURE WITH OVERRIDE con cautela.  
  
 L'istruzione RECONFIGURE aggiorna alcune opzioni in modo dinamico. Per altre è necessario arrestare e riavviare il server. Ad esempio, le opzioni memoria **minima** del server e memoria server **max server memory** vengono aggiornate dinamicamente nella [!INCLUDE[ssDE](../../includes/ssde-md.md)] ; pertanto, è possibile modificarle senza riavviare il server. Al contrario, per riconfigurare il valore corrente dell'opzione **Fill Factor** è necessario riavviare [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Dopo l'esecuzione di RECONFIGURE in un'opzione di configurazione, è possibile verificare se l'opzione è stata aggiornata dinamicamente eseguendo **sp_configure '**_option_name_*_'_*. I valori nelle colonne **run_value** e **config_value** devono corrispondere per un'opzione aggiornata dinamicamente. È anche possibile verificare quali opzioni sono dinamiche osservando la colonna **is_dynamic** della vista del catalogo **sys.configurations** .  
 
 La modifica viene inoltre scritta nel log degli errori di SQL Server.
  
> [!NOTE]  
>  Se un *valore* specificato è troppo elevato per un'opzione, la colonna **run_value** riflette il fatto che la [!INCLUDE[ssDE](../../includes/ssde-md.md)] memoria dinamica è stata impostata per impostazione predefinita anziché utilizzare un'impostazione non valida.  
  
 Per ulteriori informazioni, vedere [riconfigurare &#40;&#41;Transact-SQL ](../../t-sql/language-elements/reconfigure-transact-sql.md).  
  
## <a name="advanced-options"></a>Opzioni avanzate  
 Alcune opzioni di configurazione, ad esempio **affinity mask** e **Recovery Interval**, sono designate come opzioni avanzate. Per impostazione predefinita non è possibile visualizzarle e modificarle. Per renderle disponibili, impostare l'opzione di configurazione **Mostra opzioni avanzate** su 1. 
 
> [!CAUTION]  
> Quando l'opzione **Mostra opzioni avanzate** è impostata su 1, questa impostazione si applica a tutti gli utenti. È consigliabile utilizzare solo questo stato temporaneamente e tornare a 0 quando viene eseguito con l'attività che ha richiesto la visualizzazione delle opzioni avanzate.  
  
 Per ulteriori informazioni sulle opzioni di configurazione e le relative impostazioni, vedere [Opzioni di configurazione del Server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per modificare un'opzione di configurazione o per eseguire l'istruzione RECONFIGURE, è necessario concedere l'autorizzazione a livello di server ALTER Settings. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-the-advanced-configuration-options"></a>R. Visualizzazione dell'elenco delle opzioni di configurazione avanzate  
 Nell'esempio seguente viene illustrato come impostare ed elencare tutte le opzioni di configurazione. Le opzioni di configurazione avanzate vengono visualizzate se innanzitutto si imposta `show advanced option` su `1`. In seguito alla modifica di questa opzione, se si esegue `sp_configure` senza parametri, verranno visualizzate tutte le opzioni di configurazione.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'show advanced option', '1';  
```  
  
 Viene restituito il messaggio seguente: "L'impostazione 0 dell'opzione di configurazione 'show advanced options' è stata sostituita con 1. Per eseguire l'installazione, utilizzare RECONFIGURE".  
  
 Eseguire `RECONFIGURE` e visualizzare tutte le opzioni di configurazione:  
  
```sql  
RECONFIGURE;  
EXEC sp_configure;  
```  
  
### <a name="b-changing-a-configuration-option"></a>B. Modifica di un'opzione di configurazione  
 Nell'esempio seguente viene impostato il valore di `recovery interval` del sistema su `3` minuti.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'recovery interval', '3';  
RECONFIGURE WITH OVERRIDE;  
```  
  
## <a name="examples-sspdw"></a>Esempi: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-list-all-available-configuration-settings"></a>C. Elencare tutte le impostazioni di configurazione disponibili  
 L'esempio seguente mostra come impostare ed elencare tutte le opzioni di configurazione.  
  
```sql  
EXEC sp_configure;  
```  
  
 Il risultato restituisce il nome dell'opzione seguito dai valori minimi e massimo per l'opzione. Il **config_value** è il valore che [!INCLUDE[ssDW](../../includes/ssdw-md.md)] verrà utilizzato al termine della riconfigurazione. **config_value** è il valore in uso. The **config_value** e **run_value** sono in genere uguali, a meno che il valore non sia in corso di modifica.  
  
### <a name="d-list-the-configuration-settings-for-one-configuration-name"></a>D. Elencare le impostazioni di configurazione per un nome di configurazione  
  
```sql  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="e-set-hadoop-connectivity"></a>E. Impostare la connettività Hadoop  
 Per impostare la connettività Hadoop sono necessari alcuni passaggi aggiuntivi, oltre a eseguire sp_configure. Per la procedura completa, vedere [creare un'origine dati esterna &#40;&#41;Transact-SQL ](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
  
