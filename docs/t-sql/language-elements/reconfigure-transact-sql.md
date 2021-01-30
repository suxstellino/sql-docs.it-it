---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: cawrites
ms.author: chadam
ms.openlocfilehash: b920a123f104e02b2906d6f55d181bdedbdd1f37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182257"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna il valore attualmente configurato (la colonna **config_value** del set di risultati di **sp_configure**) di un'opzione di configurazione modificata tramite la stored procedure di sistema **sp_configure**. Poiché con alcune opzioni di configurazione è necessario arrestare e riavviare il server per aggiornare il valore corrente, RECONFIGURE non aggiorna sempre il valore corrente (la colonna **run_value** del set di risultati di **sp_configure**) per un valore di configurazione modificato.    
    
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Sintassi    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 RECONFIGURE    
 Specifica che, se le impostazioni di configurazione non richiedono l'arresto e il riavvio del server, viene aggiornato il valore corrente. RECONFIGURE controlla anche che i nuovi valori di configurazione non contengano valori non validi (ad esempio un valore di tipo di ordinamento inesistente in **syscharsets**) o sconsigliati. Con le opzioni di configurazione che non richiedono l'arresto e il riavvio del server, dopo avere specificato RECONFIGURE il valore corrente e il valore attualmente configurato per l'opzione di configurazione dovrebbero coincidere.    
    
 WITH OVERRIDE    
 Consente di disabilitare la verifica dei valori di configurazione non validi o non consigliati per l'opzione di configurazione avanzata **recovery interval**.    
    
 Praticamente qualsiasi opzione di configurazione può essere riconfigurata tramite l'opzione WITH OVERRIDE, salvo alcuni casi che possono generare errori irreversibili. Ad esempio l'opzione **min server memory** non può essere configurata con un valore superiore a quello specificato nell'opzione di configurazione **max server memory**.
      
## <a name="remarks"></a>Osservazioni    
 **sp_configure** non accetta nuovi valori di opzioni di configurazione non compresi nell'intervallo valido previsto per ogni opzione di configurazione.    
    
 L'istruzione RECONFIGURE non è consentita in una transazione esplicita o implicita. Quando si riconfigurano diverse opzioni contemporaneamente, in caso di esito negativo di una o più delle operazioni di riconfigurazione nessuna delle operazioni di riconfigurazione avrà effetto.    
    
 Durante la riconfigurazione di Resource Governor, vedere l'opzione RECONFIGURE di [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md).    
    
## <a name="permissions"></a>Autorizzazioni    
 Le autorizzazioni per RECONFIGURE vengono assegnate per impostazione predefinita agli utenti che dispongono dell'autorizzazione per ALTER SETTINGS. I ruoli predefiniti del server **sysadmin** e **serveradmin** dispongono di questa autorizzazione in modo implicito.    
    
## <a name="examples"></a>Esempi    
 Nell'esempio seguente viene impostato il limite massimo per l'opzione di configurazione `recovery interval` su `75` minuti e si utilizza `RECONFIGURE WITH OVERRIDE` per la relativa installazione. Gli intervalli di recupero superiori a 60 minuti non sono consigliati e per impostazione predefinita non sono consentiti. Poiché è specificata l'opzione `WITH OVERRIDE`, tuttavia, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non controlla se il valore specificato (`75`) è un valore valido per l'opzione di configurazione `recovery interval`.    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>Vedere anche    
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  
