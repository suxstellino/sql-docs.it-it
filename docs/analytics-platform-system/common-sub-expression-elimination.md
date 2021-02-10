---
title: Sottoespressione comune
description: Consente di visualizzare un miglioramento delle query di esempio introdotto nella piattaforma analitica System CU 7.3
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: d1d60b85bec6ec810cc506e1002dbc8ecc60ed50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052792"
---
# <a name="common-subexpression-elimination-explained"></a>Spiegazione dell'eliminazione della sottoespressione comune

APS CU 7.3 migliora le prestazioni delle query con l'eliminazione della sottoespressione comune in SQL Query Optimizer. Il miglioramento migliora le query in due modi. Il primo vantaggio è la possibilità di identificare ed eliminare tali espressioni per ridurre il tempo di compilazione SQL. Il secondo e più importante vantaggio è che le operazioni di spostamento dei dati per queste sottoespressioni ridondanti vengono eliminate, quindi il tempo di esecuzione per le query diventa più veloce.

```sql
select top 100 asceding.rnk, i1.i_product_name best_performing, i2.i_product_name worst_performing
  from(select *
       from (select item_sk,rank() over (order by rank_col asc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V1)V11
       where rnk  < 11) asceding,
      (select *
       from (select item_sk,rank() over (order by rank_col desc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V2)V21
       where rnk  < 11) descending,
  item i1,
  item i2
  where asceding.rnk = descending.rnk
    and i1.i_item_sk=asceding.item_sk
    and i2.i_item_sk=descending.item_sk
  order by asceding.rnk
  ;
```
Si consideri la query precedente da strumenti di benchmark TPC-DS.  Nella query precedente la sottoquery è la stessa, ma la clausola ORDER BY con la funzione Rank () over viene ordinata in due modi diversi. Prima di CU 7.3, la sottoquery viene valutata ed eseguita due volte, una per l'ordine crescente e una per l'ordine decrescente, che incorre in due operazioni di spostamento dei dati. Dopo l'installazione di APS CU 7.3, la parte della sottoquery viene valutata una volta riducendo lo spostamento dei dati e terminando la query più velocemente.

È stata introdotta un' [opzione di funzionalità](appliance-feature-switch.md) denominata "OptimizeCommonSubExpressions" che consentirà di testare la funzionalità anche dopo l'aggiornamento ad APS cu 7.3. La funzionalità è attivata per impostazione predefinita, ma può essere disattivata. 

> [!NOTE] 
> Per le modifiche ai valori delle opzioni di funzionalità è necessario riavviare il servizio.

È possibile provare la query di esempio creando le tabelle seguenti nell'ambiente di test e valutando il piano di spiegazione per la query indicata in precedenza. 

```sql
CREATE TABLE [dbo].[store_sales] (
    [ss_sold_date_sk] int NULL, 
    [ss_sold_time_sk] int NULL, 
    [ss_item_sk] int NOT NULL, 
    [ss_customer_sk] int NULL, 
    [ss_cdemo_sk] int NULL, 
    [ss_hdemo_sk] int NULL, 
    [ss_addr_sk] int NULL, 
    [ss_store_sk] int NULL, 
    [ss_promo_sk] int NULL, 
    [ss_ticket_number] int NOT NULL, 
    [ss_quantity] int NULL, 
    [ss_wholesale_cost] decimal(7, 2) NULL, 
    [ss_list_price] decimal(7, 2) NULL, 
    [ss_sales_price] decimal(7, 2) NULL, 
    [ss_ext_discount_amt] decimal(7, 2) NULL, 
    [ss_ext_sales_price] decimal(7, 2) NULL, 
    [ss_ext_wholesale_cost] decimal(7, 2) NULL, 
    [ss_ext_list_price] decimal(7, 2) NULL, 
    [ss_ext_tax] decimal(7, 2) NULL, 
    [ss_coupon_amt] decimal(7, 2) NULL, 
    [ss_net_paid] decimal(7, 2) NULL, 
    [ss_net_paid_inc_tax] decimal(7, 2) NULL, 
    [ss_net_profit] decimal(7, 2) NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([ss_item_sk]),  PARTITION ([ss_sold_date_sk] RANGE RIGHT FOR VALUES (2450815, 2451180, 2451545, 2451911, 2452276, 2452641, 2453006)));

CREATE TABLE [dbo].[item] (
    [i_item_sk] int NOT NULL, 
    [i_item_id] char(16) COLLATE Latin1_General_100_CI_AS_KS_WS NOT NULL, 
    [i_rec_start_date] date NULL, 
    [i_rec_end_date] date NULL, 
    [i_item_desc] varchar(200) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_current_price] decimal(7, 2) NULL, 
    [i_wholesale_cost] decimal(7, 2) NULL, 
    [i_brand_id] int NULL, 
    [i_brand] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_class_id] int NULL, 
    [i_class] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_category_id] int NULL, 
    [i_category] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manufact_id] int NULL, 
    [i_manufact] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_size] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_formulation] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_color] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_units] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_container] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manager_id] int NULL, 
    [i_product_name] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL
)
WITH (CLUSTERED INDEX ( [i_item_sk] ASC ), DISTRIBUTION = REPLICATE);
```
Se si esamina il piano di spiegazione della query, si noterà che prima di CU 7.3 (o quando l'opzione della funzionalità è disattivata) la query ha 17 numero totale di operazioni e dopo CU 7.3 (o con l'opzione di funzionalità attivata) la stessa query Mostra 9 numero totale di operazioni. Se si contano solo le operazioni di spostamento dei dati, si noterà che il piano precedente prevede quattro operazioni di spostamento rispetto a due operazioni di spostamento nel nuovo piano. Il nuovo Query Optimizer è stato in grado di ridurre due operazioni di spostamento dei dati riutilizzando la tabella temporanea già creata con il nuovo piano riducendo in tal modo il runtime di query. 


