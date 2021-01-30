---
title: sp_repldone (Transact-SQL) | Microsoft Docs
description: Aggiorna il record che identifica l'ultima transazione distribuita del server. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repldone
- sp_repldone_TSQL
helpviewer_keywords:
- sp_repldone
ms.assetid: 045d3cd1-712b-44b7-a56a-c9438d4077b9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4889616070df4a42150272e0c9cd17aee941b063
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203145"
---
# <a name="sp_repldone-transact-sql"></a>sp_repldone (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Aggiorna il record che identifica l'ultima transazione distribuita del server. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!CAUTION]  
>  L'esecuzione manuale di **sp_repldone** potrebbe invalidare l'ordine e la coerenza delle transazioni recapitate. **sp_repldone** deve essere utilizzato solo per la risoluzione dei problemi di replica come indicato da un professionista del supporto di replica esperto.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```
sp_repldone [ @xactid= ] xactid   
        , [ @xact_seqno= ] xact_seqno   
    [ , [ @numtrans= ] numtrans ]   
    [ , [ @time= ] time   
    [ , [ @reset= ] reset ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @xactid = ] xactid` Numero di sequenza del file di log (LSN) del primo record per l'ultima transazione distribuita del server. *xactid* è **binario (10)** e non prevede alcun valore predefinito.  
  
`[ @xact_seqno = ] xact_seqno` LSN dell'ultimo record per l'ultima transazione distribuita del server. *xact_seqno* è **binario (10)** e non prevede alcun valore predefinito.  
  
`[ @numtrans = ] numtrans` Numero di transazioni distribuite. *numtrans* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @time = ] time` Numero di millisecondi, se specificato, necessario per distribuire l'ultimo batch di transazioni. *Time* è di **tipo int** e non prevede alcun valore predefinito.  
  
`[ @reset = ] reset` Stato di reimpostazione. *Reset* è di **tipo int** e non prevede alcun valore predefinito. Se è **1**, tutte le transazioni replicate nel log vengono contrassegnate come distribuite. Se è **0**, il log delle transazioni viene reimpostato sulla prima transazione replicata e nessuna transazione replicata viene contrassegnata come distribuita. *Reset* è valido solo quando *xactid* e *xact_seqno* sono null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_repldone** viene utilizzata nella replica transazionale.  
  
 **sp_repldone** viene utilizzato dal processo di lettura log per tenere traccia delle transazioni distribuite.  
  
 Con **sp_repldone**, è possibile indicare manualmente al server che una transazione è stata replicata (inviata al server di distribuzione). È inoltre possibile cambiare la transazione contrassegnata come transazione successiva in attesa di replica e scorrere l'elenco delle transazioni replicate. Tutte le transazioni che precedono la transazione specificata, inclusa tale transazione, vengono contrassegnate come distribuite.  
  
 I parametri obbligatori *xactid* e *xact_seqno* possono essere ottenuti utilizzando **sp_repltrans** o **sp_replcmds**.  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_repldone**.  
  
## <a name="examples"></a>Esempio  
 Quando *xactid* è null, *xact_seqno* è null e *Reset* è **1**, tutte le transazioni replicate nel log vengono contrassegnate come distribuite. Ciò risulta utile quando nel log delle transazioni sono presenti transazioni replicate non più valide e si desidera troncare il log, ad esempio:  
  
```sql
EXEC sp_repldone @xactid = NULL, @xact_seqno = NULL, @numtrans = 0, @time = 0, @reset = 1  
```  
  
> [!CAUTION]  
>  È possibile utilizzare questa procedura in situazioni di emergenza per consentire il troncamento del log delle transazioni quando sono presenti transazioni in sospeso in attesa di replica.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replflush &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
