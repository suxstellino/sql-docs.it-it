---
description: sp_dropdistributor (Transact-SQL)
title: sp_dropdistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistributor
- sp_dropdistributor_TSQL
helpviewer_keywords:
- sp_dropdistributor
ms.assetid: 0644032f-5ff0-4718-8dde-321bc9967a03
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7a08c3f8cd035db38533f9b7fd56aaa34c39399
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205145"
---
# <a name="sp_dropdistributor-transact-sql"></a>sp_dropdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Disinstalla il server di distribuzione. La stored procedure viene eseguita nel server di distribuzione su qualsiasi database, a eccezione del database di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropdistributor [ [ @no_checks= ] no_checks ]   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @no_checks = ] no_checks` Indica se verificare la presenza di oggetti dipendenti prima di eliminare il server di distribuzione. *no_checks* è di **bit** e il valore predefinito è 0.  
  
 Se è **0**, **sp_dropdistributor** verifica che tutti gli oggetti di pubblicazione e distribuzione oltre al server di distribuzione siano stati eliminati.  
  
 Se è **1**, **sp_dropdistributor** Elimina tutti gli oggetti di pubblicazione e distribuzione prima di disinstallare il server di distribuzione.  
  
`[ @ignore_distributor = ] ignore_distributor` Indica se questo stored procedure viene eseguito senza connettersi al server di distribuzione. *ignore_distributor* è di **bit** e il valore predefinito è **0**.  
  
 Se è **0**, **sp_dropdistributor** si connette al server di distribuzione e rimuove tutti gli oggetti di replica. Se **sp_dropdistributor** non è in grado di connettersi al server di distribuzione, il stored procedure ha esito negativo.  
  
 Se è **1**, non viene eseguita alcuna connessione al server di distribuzione e gli oggetti di replica non vengono rimossi. Questo valore viene utilizzato se è in corso la disinstallazione del server di distribuzione oppure se il server è offline in modo permanente. Gli oggetti per questo server di pubblicazione nel server di distribuzione vengono rimossi solo dopo la reinstallazione successiva del server di distribuzione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropdistributor** viene utilizzato in tutti i tipi di replica.  
  
 Se nel server sono presenti altri oggetti editore o di distribuzione, **sp_dropdistributor** ha esito negativo a meno che **\@ no_checks** non sia impostato su **1**.  
  
 Questa stored procedure deve essere eseguita dopo l'eliminazione del database di distribuzione tramite l'esecuzione di **sp_dropdistributiondb**.  
  
## <a name="example"></a>Esempio  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributor-trans_1.sql)]  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_dropdistributor**.  
  
## <a name="see-also"></a>Vedere anche  
 [Disabilitare la pubblicazione e la distribuzione](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)   
 [sp_changedistributor_property &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
