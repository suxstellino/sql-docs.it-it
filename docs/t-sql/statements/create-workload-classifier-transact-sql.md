---
description: CREATE WORKLOAD CLASSIFIER (Transact-SQL)
title: CREATE WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: synapse-analytics
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 5040da21c46a1f835f3a5744c7cd0bd1ff76a546
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755401"
---
# <a name="create-workload-classifier-transact-sql"></a>CREATE WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Crea un oggetto classificatore da usare nella gestione del carico di lavoro.  Il classificatore assegna le richieste in ingresso a un gruppo di carico di lavoro in base ai parametri specificati nella definizione dell'istruzione del classificatore stesso.  I classificatori vengono valutati con ogni richiesta inviata.  Se una richiesta non viene abbinata a un classificatore, viene assegnata al gruppo del carico di lavoro predefinito.  Il gruppo di carico di lavoro predefinito è la classe di risorse smallrc.

> [!NOTE]
> Il classificatore del carico di lavoro prende il posto dell'assegnazione della classe di risorse sp_addrolemember.  Dopo la creazione dei classificatori del carico di lavoro, eseguire sp_droprolemember per rimuovere eventuali mapping della classe di risorse ridondanti.

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti

 *classifier_name*  
 Specifica il nome con cui viene identificato il classificatore del carico di lavoro.  classifier_name è un sysname.  Può essere composto da un massimo di 128 caratteri e deve essere univoco all'interno dell'istanza.

 *WORKLOAD_GROUP* =  *'name'*    
 Quando le regole del classificatore soddisfano le condizioni, name esegue il mapping della richiesta a un gruppo di carico di lavoro.  name è un sysname.  Può contenere fino a 128 caratteri e deve essere il nome di un gruppo di carico di lavoro valido al momento della creazione del classificatore.

 I gruppi del carico di lavoro disponibili sono reperibili nella vista del catalogo [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md).

 *MEMBERNAME* =  *'security_account'*     
 Account di sicurezza usato per la classificazione.  Security_account è un sysname, senza impostazioni predefinite. Security_account può essere un utente del database, un ruolo del database, un account di accesso di Azure Active Directory o un gruppo di Azure Active Directory.
 
 *WLM_LABEL*   
 Specifica il valore dell'etichetta rispetto al quale è possibile classificare una richiesta.  label è un parametro facoltativo di tipo nvarchar(255).  Usare [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) nella richiesta per associare la configurazione del classificatore.

Esempio:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
Specifica il valore del contesto della sessione rispetto al quale è possibile classificare una richiesta.  context è un parametro facoltativo di tipo nvarchar(255).  Usare [sys.sp_set_session_context](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) con nome della variabile uguale a `wlm_context` prima di inviare una richiesta di impostazione del contesto della sessione.

Esempio:

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* ed *END_TIME*  
Specifica start_time ed end_time rispetto a cui è possibile classificare una richiesta.  Sia start_time che end_time hanno il formato HH:MM nel fuso orario UTC.  Start_time ed end_time devono essere specificati insieme.

Esempio:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Specifica l'importanza relativa di una richiesta.  I possibili valori di importanza sono i seguenti:

- LOW
- BELOW_NORMAL
- NORMAL (valore predefinito)
- ABOVE_NORMAL
- HIGH  

Se l'importanza non è specificata, viene usata l'impostazione di importanza del gruppo di carico di lavoro.  L'importanza del gruppo di carico di lavoro predefinita corrisponde a normal.  L'importanza ha effetti sull'ordine in cui le richieste vengono pianificate, offrendo quindi un accesso prioritario a risorse e blocchi.

## <a name="classification-parameter-weighting"></a>Ponderazione dei parametri di classificazione

Una richiesta può essere confrontata con più classificatori.  Per i parametri di classificatore viene effettuata una ponderazione.  Per assegnare un gruppo del carico di lavoro e una priorità, viene usato il classificatore con il valore di corrispondenza ponderata più alto.  La ponderazione è basata sui criteri seguenti:

|Parametro di classificatore |Peso   |
|---------------------|---------|
|USER                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Considerare le configurazioni dei classificatori seguenti.

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

L'utente `userloginA` è configurato per entrambi i classificatori.  Se userloginA esegue una query con un'etichetta uguale a `salesreport` tra le 18.00 e le 07.00 UTC, la richiesta viene classificata nel gruppo del carico di lavoro wgDashboards con priorità HIGH (alta).  Ci si potrebbe aspettare di dover classificare la richiesta in wgUserQueries con priorità LOW (bassa) per la generazione di report in orari di minore attività, ma il valore di ponderazione di WLM_LABEL è maggiore di quello di START_TIME/END_TIME.  Il valore di ponderazione di classifierA è 80 (64 per utente, più 16 per WLM_LABEL).  Il valore di ponderazione di classifierB è 68 (64 per utente, 4 per START_TIME/END_TIME).  In questo caso, è possibile aggiungere WLM_LABEL a classifierB.

 Per altre informazioni, vedere [Classificazione del carico di lavoro](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting).

## <a name="permissions"></a>Autorizzazioni

 È richiesta l'autorizzazione CONTROL DATABASE.  
  
## <a name="examples"></a>Esempi

 Nell'esempio riportato di seguito viene illustrato come creare un classificatore del carico di lavoro denominato `wgcELTRole`. Viene usato il gruppo di carico di lavoro staticrc20, l'utente `ELTRole` e la priorità viene impostata su `above_normal`.

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>Vedere anche

[Classificazione di [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

