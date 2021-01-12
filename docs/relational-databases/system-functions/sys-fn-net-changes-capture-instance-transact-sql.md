---
description: sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL)
title: sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_net_changes_TSQL
- fn_net_changes_TSQL
- fn_net_changes
- sys.fn_net_changes
dev_langs:
- TSQL
helpviewer_keywords:
- fn_net_changes_<capture_instance>
- sys.fn_net_changes_<capture_instance>
ms.assetid: 342fa030-9fd9-4b74-ae4d-49f6038a5073
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eb0c58b3544afd5fa529db0c95af7c2f6ba3e6d3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096348"
---
# <a name="sysfn_net_changes_ltcapture_instancegt-transact-sql"></a>sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Wrapper per le funzioni di query di **net changes** . Gli script necessari per creare queste funzioni vengono generati dalla stored procedure sys.sp_cdc_generate_wrapper_function.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_net_changes_<capture_instance> ('start_time', 'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all with mask  
  | all with merge  
}  
```  
  
## <a name="arguments"></a>Argomenti  
 *start_time*  
 Valore **DateTime** che rappresenta l'endpoint inferiore dell'intervallo delle voci della tabella delle modifiche da includere nel set di risultati.  
  
 Nel set di risultati sono incluse solo le righe della tabella delle modifiche CDC. <capture_instance>_CT tabella delle modifiche con un'ora di commit associata rigorosamente maggiore di *start_time* .  
  
 Se viene fornito un valore NULL per questo argomento, l'endpoint inferiore dell'intervallo della query corrisponderà all'endpoint inferiore dell'intervallo valido per l'istanza di acquisizione.  
  
 *end_time*  
 Valore **DateTime** che rappresenta l'endpoint superiore dell'intervallo delle voci della tabella delle modifiche da includere nel set di risultati.  
  
 Questo parametro può assumere uno dei due significati, a seconda del valore scelto per @closed_high_end_point quando viene chiamato sys.sp_cdc_generate_wrapper_function per generare lo script per la creazione della funzione wrapper:  
  
-   @closed_high_end_point = 1  
  
     Solo le righe del CDC. <capture_instance>_CT tabella delle modifiche con un valore in \_ \_ $Start _lsn e un'ora di commit corrispondente minore o uguale a **start_time** sono incluse nel set di risultati.  
  
-   @closed_high_end_point = 0  
  
     Solo le righe del CDC. <capture_instance>_CT tabella delle modifiche con un valore in \_ \_ $Start _lsn e un'ora di commit corrispondente rigorosamente inferiore a **start_time** sono incluse nel set di risultati.  
  
 Se viene fornito un valore NULL per questo argomento, l'endpoint superiore dell'intervallo della query corrisponderà all'endpoint superiore dell'intervallo valido per l'istanza di acquisizione.  
  
 *<row_filter_option>* :: = {All | All con mask | all con merge}  
 Opzione applicata al contenuto delle colonne dei metadati e alle righe restituite nel set di risultati. Le opzioni possibili sono le seguenti:  
  
 all  
 Restituisce il contenuto finale di una riga modificata nelle colonne di contenuto e l'operazione necessaria per applicare la riga nella colonna di metadati __CDC_OPERATION.  
  
 all with mask  
 Restituisce il contenuto finale di tutte le colonne modificate nelle colonne di contenuto e l'operazione necessaria per applicare ciascuna riga nella colonna di metadati __CDC_OPERATION. Se è stato specificato un elenco di flag di aggiornamento al momento della generazione dello script di creazione della funzione wrapper, questa opzione è necessaria per popolare la maschera di aggiornamento.  
  
 all with merge  
 Restituisce il contenuto finale di tutte le righe modificate nelle colonne di contenuto.  
  
 I due possibili valori per la colonna __CDC_OPERATION sono i seguenti:  
  
-   D, se la riga deve essere eliminata.  
  
-   M, se la riga deve essere inserita o aggiornata.  
  
 La logica per determinare se è necessaria un'operazione di inserimento o di aggiornamento per applicare una modifica alla destinazione aggiunge maggiore complessità alla query. Utilizzare questa opzione per ottenere prestazioni ottimali quando non è necessario distinguere tra le operazioni di inserimento e aggiornamento. Questo approccio funziona meglio negli ambienti di destinazione dove è direttamente disponibile un'operazione di unione, ad esempio un ambiente [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di colonna|Descrizione|  
|-----------------|-----------------|-----------------|  
|\<columns from @column_list>|**varia**|Le colonne identificate nell'argomento **column_list** per la sp_cdc_generate_wrapper_function quando viene chiamata per generare lo script per creare il wrapper. Se *column_list* è null, tutte le colonne di origine rilevate verranno visualizzate nel set di risultati.|  
|__CDC_OPERATION|**nvarchar (2)**|Codice operativo che indica che l'operazione è necessaria per applicare la riga all'ambiente di destinazione. L'operazione può variare in base al valore dell'argomento *row_filter_option* fornito nella chiamata seguente:<br /><br /> *row_filter_option* =' all',' all with mask '<br /><br /> 'D' - operazione di eliminazione<br /><br /> 'I' - operazione di inserimento<br /><br /> 'UN' - operazione di aggiornamento<br /><br /> *row_filter_option* =' all with merge '<br /><br /> 'D' - operazione di eliminazione<br /><br /> 'M' - operazione di inserimento oppure di aggiornamento|  
|\<columns from @update_flag_list>|**bit**|Flag di bit denominato aggiungendo _uflag al nome della colonna. Il flag accetta un valore non null solo quando *row_filter_option* **=' all with mask '** e \_ _CDC_OPERATION **=' un'**. È impostato su 1 se la colonna corrispondente è stata modificata all'interno della finestra di query. Altrimenti, è impostato su 0.|  
  
## <a name="remarks"></a>Osservazioni  
 Il fn_net_changes_<capture_instance funzione> funge da wrapper per la funzione di query CDC.fn_cdc_get_net_changes_<capture_instance>. La stored procedure sys.sp_cdc_generate_wrapper viene utilizzata per creare lo script per il wrapper.  
  
 Le funzioni wrapper non vengono create automaticamente. Per creare le funzioni wrapper, è necessario eseguire due operazioni:  
  
1.  Eseguire la stored procedure per generare lo script di creazione del wrapper.  
  
2.  Eseguire lo script per creare effettivamente la funzione wrapper.  
  
 Le funzioni wrapper consentono agli utenti di eseguire sistematicamente query per le modifiche apportate all'interno di un intervallo delimitato da valori **DateTime** anziché da valori LSN. Le funzioni wrapper eseguono tutte le conversioni necessarie tra i valori **DateTime** forniti e i valori LSN necessari internamente come argomenti per le funzioni di query. Quando le funzioni wrapper vengono utilizzate in modo seriale per elaborare un flusso di dati delle modifiche, assicurano che nessun dato venga perso o ripetuto purché venga seguita la convenzione seguente: il @end_time valore dell'intervallo associato a una chiamata viene fornito come @start_time valore per l'intervallo associato alla chiamata successiva.  
  
 Utilizzando il parametro @closed_high_end_point durante la creazione dello script, è possibile generare wrapper per supportare un limite superiore chiuso o un limite superiore aperto nella finestra della query specificata, ovvero è possibile decidere se le voci che dispongono di un'ora di commit uguale al limite superiore dell'intervallo di estrazione devono essere incluse nell'intervallo. Per impostazione predefinita, il limite superiore è incluso.  
  
 Il set di risultati restituito dalla funzione wrapper **net changes** restituisce solo le colonne rilevate presenti nell'oggetto al @column_list momento della generazione del wrapper. Se @column_list è NULL, vengono restituite tutte le colonne di origine rilevate. Le colonne di origine sono seguite dalla colonna __CDC_OPERATION, una colonna di uno o due caratteri che identifica l'operazione.  
  
 I flag di bit vengono quindi aggiunti al set di risultati per ogni colonna identificata nel parametro @update_flag_list . Per il wrapper **net changes** , i flag di bit saranno sempre null se l'oggetto @row_filter_option utilizzato nella chiamata alla funzione wrapper è' all'o ' all with merge '. Se @row_filter_option è impostato su' all with mask ' e __CDC_OPERATION è' d'o ' i ', anche il valore del flag sarà null. Se \_ _CDC_OPERATION è' un', il flag verrà impostato su 1 o 0, a seconda che l'operazione di **net** Update abbia causato o meno una modifica alla colonna.  
  
 Il modello di configurazione Change Data Capture ' Instantiate CDC wrapper funzioni con valori for schema ' Mostra come utilizzare il stored procedure sp_cdc_generate_wrapper_function per ottenere gli script di creazione per tutte le funzioni wrapper per le funzioni di query definite di uno schema. Il modello crea quindi tali script. Per ulteriori informazioni sui modelli, vedere [Esplora modelli](../../ssms/template/template-explorer.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_generate_wrapper_function &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  
  
