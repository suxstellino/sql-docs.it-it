---
title: Pagina Autorizzazioni o Entità a sicurezza diretta | Microsoft Docs
description: Usare la pagina Autorizzazioni o la pagina Entità a protezione diretta per visualizzare o impostare le autorizzazioni per le entità a protezione diretta in SQL Server.
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.permissions.f1
- sql13.swb.SecurableAndEffectPermissions.f1
- sql13.swb.common.columnperm.f1
- sql13.swb.availabilitygroupproperties.permission.f1
- sql13.swb.SecurableAndEffectivePermission.f1
ms.assetid: b3bf077a-bec2-4161-ac0c-460586199906
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bc8f417dba29049a5845fe450eea82c8f753a5a1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748041"
---
# <a name="permissions-or-securables-page"></a>Pagina Autorizzazioni o Entità a sicurezza diretta
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Usare la pagina **Autorizzazioni** o la pagina **Entità a protezione diretta** per visualizzare o impostare le autorizzazioni per le entità a protezione diretta. È possibile accedere a questa pagina da diversi tipi di oggetti. Il contenuto della pagina può modificare leggermente, in base alle modalità in cui la pagina è aperta e al contenuto. Quando la pagina viene visualizzata, la griglia superiore della pagina potrebbe contenere alcuni elementi o potrebbe essere vuota. Per aggiungere elementi alla griglia superiore, fare clic su **Cerca**. Selezionare un elemento in tale griglia, quindi impostare le autorizzazioni appropriate nella scheda **Esplicita** . Per visualizzare autorizzazioni aggregate, usare la scheda **Valide** .  
  
 Per comprendere le possibili combinazioni di entità a protezione diretta ed entità, vedere i collegamenti relativi alla sintassi specifica delle entità a protezione diretta nell'argomento [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md). Per altre informazioni, vedere [Entità a protezione diretta](../../relational-databases/security/securables.md).  
  
## <a name="page-header"></a>Intestazione di pagina  
 L'intestazione della pagina **Autorizzazioni** o **Entità a protezione diretta** dipende dall'entità a protezione diretta o dall'entità. Vengono visualizzate informazioni significative sull'elemento, ad esempio il nome.  
  
## <a name="upper-grid"></a>Griglia superiore  
 La griglia superiore include uno o più elementi per i quali è possibile impostare le autorizzazioni. In questa finestra di dialogo è disponibile un pulsante **Cerca** che consente di selezionare oggetti o entità da aggiungere alla griglia superiore. Nel nome della griglia potrebbe essere visualizzato **Entità a protezione diretta** oppure uno o più tipi di entità o di entità a protezione diretta. Le colonne visualizzate nella griglia superiore variano a seconda del tipo di entità o di entità a sicurezza diretta.  
  
 **Nome**  
 Nome di ogni entità o entità a sicurezza diretta aggiunto alla griglia.  
  
 **Tipo**  
 Descrive il tipo di ogni elemento.  
  
## <a name="explicit-tab"></a>Scheda Esplicita  
 Nella scheda **Esplicita** sono elencate le possibili autorizzazioni per l'entità a protezione diretta selezionate nella griglia superiore. Per configurare le autorizzazioni, selezionare o deselezionare le caselle di controllo **Concedi** (o **Consenti**), **Con diritto di concessione** e **Nega** . Le opzioni effettivamente disponibili dipendono dall'autorizzazione esplicita in questione.  
  
 **Autorizzazioni**  
 Nome dell'autorizzazione.  
  
 **Utente che concede le autorizzazioni**  
 Entità che ha concesso l'autorizzazione.  
  
 **Concedi**  
 Selezionare questa opzione per concedere l'autorizzazione all'account di accesso, deselezionarla per revocare l'autorizzazione.  
  
 **Con diritto di concessione**  
 Riflette lo stato dell'opzione WITH GRANT relativo all'autorizzazione elencata. Il contenuto di questa casella è di sola lettura. Per applicare questa autorizzazione, utilizzare l'istruzione [GRANT](../../t-sql/statements/grant-transact-sql.md) .  
  
 **Nega**  
 Selezionare questa opzione per negare l'autorizzazione all'account di accesso, deselezionarla per revocare l'autorizzazione.  
  
 **Autorizzazioni colonna**  
 Per oggetti che contengono colonne, ad esempio tabelle, viste o funzioni con valori di tabella, il pulsante **Autorizzazioni colonna** consente di visualizzare la finestra di dialogo **Autorizzazioni colonna** . In questa finestra di dialogo è possibile impostare le autorizzazioni **Concedi**, **Consenti** o **Nega** in colonne singole di una tabella o di una vista. Questa opzione non è disponibile per tutti i tipi di oggetti o tutte le autorizzazioni.  
  
## <a name="effective-tab"></a>Scheda Valide  
 Le autorizzazioni di un'entità correlate a quelle di un'entità a sicurezza diretta possono derivare da autorizzazioni impostate per numerose entità diverse. A un account di accesso, ad esempio, potrebbero essere concesse autorizzazioni sia a livello individuale che come appartenente a un gruppo. Nella scheda **Valide** viene visualizzato il risultato della combinazione di autorizzazioni esplicite e di quelle ricevute in base all'appartenenza a un gruppo oppure a un ruolo. Le autorizzazioni concesse vengono aggregate. Un'autorizzazione negata ha la priorità su tutte le autorizzazioni concesse.  
  
 **Autorizzazioni**  
 Nome dell'autorizzazione.  
  
 **Colonna**  
 Nomi delle colonne interessate dall'autorizzazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Centro sicurezza per il motore di Database di SQL Server e il Database SQL di Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
