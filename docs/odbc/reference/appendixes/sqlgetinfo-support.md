---
description: Supporto per SQLGetInfo
title: Supporto di SQLGetInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLGetInfo
- backward compatibility [ODBC], SQLGetInfo
- SQLGetInfo function [ODBC], support
ms.assetid: 57326f57-daba-46b6-b0be-6c97213b9ef1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dc92c6bd409829da13395b1286a5762e4504ced
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202750"
---
# <a name="sqlgetinfo-support"></a>Supporto per SQLGetInfo
Quando ODBC 2. *x* l'applicazione chiama **SQLGetInfo** a un driver ODBC 3 *. x* , gli argomenti *InfoType* nella tabella seguente devono essere supportati.  
  
|*InfoType*|Restituisce|  
|----------------|-------------|  
|SQL_ALTER_TABLE (ODBC 2,0) **Nota:**  questo tipo di informazioni non è deprecato; le maschere di maschera nella colonna a destra sono deprecate.|Maschera di tipo SQLINTEGER che enumera le clausole nell'istruzione **ALTER TABLE** supportata dall'origine dati.<br /><br /> Per determinare le clausole supportate sono utilizzate le maschere di comando seguenti:<br /><br /> SQL_AT_DROP_COLUMN = è supportata la possibilità di eliminare le colonne. Indica se questa operazione comporta un comportamento Cascade o Restrict definito dal driver. (ODBC 2,0)<br /><br /> SQL_AT_ADD_COLUMN = è supportata la possibilità di aggiungere più colonne in una singola istruzione ALTER TABLE. Questo bit non si combina con altri SQL_AT_ADD_COLUMN_XXX bit o SQL_AT_CONSTRAINT_XXX bit. (ODBC 2,0)|  
|SQL_FETCH_DIRECTION (ODBC 1,0)<br /><br /> Il tipo di informazioni è stato introdotto in ODBC 1,0; ogni maschera di maschera è contrassegnata con la versione in cui è stata introdotta.|Maschera di tipo SQLINTEGER che enumera le opzioni di direzione di recupero supportate.<br /><br /> Le seguenti maschere di maschera vengono utilizzate insieme al flag per determinare quali opzioni sono supportate:<br /><br /> SQL_FD_FETCH_NEXT (ODBC 1,0) SQL_FD_FETCH_FIRST (ODBC 1,0) SQL_FD_FETCH_LAST (ODBC 1,0) SQL_FD_FETCH_PRIOR (ODBC 1,0) SQL_FD_FETCH_ABSOLUTE (ODBC 1,0) SQL_FD_FETCH_RELATIVE (ODBC 1,0) SQL_FD_FETCH_BOOKMARK (ODBC 2,0)|  
|SQL_LOCK_TYPES (ODBC 2,0)|Maschera di errore SQLINTEGER che enumera i tipi di blocco supportati per l'argomento *Flock* in **SQLSetPos**.<br /><br /> Le seguenti maschere di maschera vengono utilizzate insieme al flag per determinare quali tipi di blocco sono supportati:<br /><br /> SQL_LCK_NO_CHANGE SQL_LCK_EXCLUSIVE SQL_LCK_UNLOCK|  
|SQL_ODBC_API_CONFORMANCE (ODBC 1,0)|Valore SQLSMALLINT che indica il livello di conformità ODBC.<br /><br /> SQL_OAC_NONE = None<br /><br /> SQL_OAC_LEVEL1 = livello 1 supportato<br /><br /> SQL_OAC_LEVEL2 = livello 2 supportato|  
|SQL_ODBC_SQL_CONFORMANCE (ODBC 1,0)|Valore SQLSMALLINT che indica la grammatica SQL supportata dal driver. Per una definizione dei livelli di conformità SQL, vedere [Appendice C: grammatica SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md) .<br /><br /> SQL_OSC_MINIMUM = grammatica minima supportata<br /><br /> SQL_OSC_CORE = grammatica di base supportata<br /><br /> SQL_OSC_EXTENDED = grammatica estesa supportata|  
|SQL_POS_OPERATIONS (ODBC 2,0)|Maschera di tipo SQLINTEGER che enumera le operazioni supportate in **SQLSetPos**.<br /><br /> Per determinare quali opzioni sono supportate, è necessario utilizzare le seguenti maschere di maschera per insieme al flag:<br /><br /> SQL_POS_POSITION (ODBC 2,0) SQL_POS_REFRESH (ODBC 2,0) SQL_POS_UPDATE (ODBC 2,0) SQL_POS_DELETE (ODBC 2,0) SQL_POS_ADD (ODBC 2,0)|  
|SQL_POSITIONED_STATEMENTS (ODBC 2,0)|Maschera di tipo SQLINTEGER che enumera le istruzioni SQL posizionate supportate.<br /><br /> Per determinare quali istruzioni sono supportate, vengono utilizzate le maschere di comando seguenti:<br /><br /> SQL_PS_POSITIONED_DELETE SQL_PS_POSITIONED_UPDATE SQL_PS_SELECT_FOR_UPDATE|  
|SQL_SCROLL_CONCURRENCY (ODBC 1,0)|Maschera di tipo SQLINTEGER che enumera le opzioni di controllo della concorrenza supportate per il cursore.<br /><br /> Per determinare quali opzioni sono supportate, vengono utilizzate le maschere di comando seguenti:<br /><br /> SQL_SCCO_READ_ONLY = il cursore è di sola lettura. Non sono consentiti aggiornamenti.<br /><br /> SQL_SCCO_LOCK = il cursore utilizza il livello di blocco più basso sufficiente per garantire che la riga possa essere aggiornata.<br /><br /> SQL_SCCO_OPT_ROWVER = il cursore usa il controllo della concorrenza ottimistica, confrontando le versioni di riga, ad esempio SQLBase ROWID o Sybase TIMESTAMP.<br /><br /> SQL_SCCO_OPT_VALUES = il cursore usa il controllo della concorrenza ottimistica, confrontando i valori.|  
|SQL_STATIC_SENSITIVITY (ODBC 2,0)|Maschera di bit SQLINTEGER che enumera se le modifiche apportate da un'applicazione a un cursore statico o gestito da keyset tramite **SQLSetPos** o istruzioni Update o DELETE posizionate possono essere rilevate da tale applicazione.<br /><br /> SQL_SS_ADDITIONS = le righe aggiunte sono visibili al cursore; il cursore può scorrere fino a queste righe. La posizione in cui queste righe vengono aggiunte al cursore dipende dal driver.<br /><br /> SQL_SS_DELETIONS = le righe eliminate non sono più disponibili per il cursore e non lasciano un "foro" nel set di risultati; Quando il cursore scorre da una riga eliminata, non può tornare alla riga.<br /><br /> SQL_SS_UPDATES = gli aggiornamenti alle righe sono visibili al cursore; Se il cursore scorre da e torna a una riga aggiornata, i dati restituiti dal cursore sono i dati aggiornati, non i dati originali. Questa opzione si applica solo ai cursori statici o agli aggiornamenti sui cursori gestiti da keyset che non aggiornano la chiave. Questa opzione non è valida per un cursore dinamico o nel caso in cui una chiave venga modificata in un cursore misto.<br /><br /> Se un'applicazione è in grado di rilevare le modifiche apportate al set di risultati da altri utenti, inclusi altri cursori nella stessa applicazione, dipende dal tipo di cursore.|  
  
 Un'applicazione ODBC 3.*x* che utilizza un driver ODBC 3 *. x* non deve chiamare **SQLGetInfo** con gli argomenti *InfoType* descritti nella tabella precedente, ma deve utilizzare gli argomenti ODBC 3 *. x* *InfoType* elencati nel paragrafo seguente. Non esiste una corrispondenza uno-a-uno tra gli argomenti *InfoType* utilizzati in ODBC 2. *x* e quelli utilizzati in ODBC 3 *. x*. Applicazione ODBC 3 *. x* che utilizza ODBC 2. il driver *x* , d'altra parte, deve usare gli argomenti *InfoType* descritti in precedenza.  
  
 Alcuni tipi di informazioni nella tabella precedente sono deprecati a favore dei tipi di informazioni sugli attributi del cursore. Questi tipi di informazioni deprecate sono SQL_FETCH_DIRECTION, SQL_LOCK_TYPES, SQL_POS_OPERATIONS, SQL_POSITIONED_STATEMENTS, SQL_SCROLL_CONCURRENCY e SQL_STATIC_SENSITIVITY. I nuovi tipi di attributi di cursore sono SQL_XXX_CURSOR_ATTRIBUTES1and SQL_XXX_CURSOR_ATTRIBUTES2, dove XXX corrisponde a DYNAMIC, FORWARD_ONLY, KEYSET_DRIVEN o STATIC. Ognuno dei nuovi tipi indica le funzionalità del driver per un singolo tipo di cursore. Per ulteriori informazioni su queste opzioni, vedere la descrizione della funzione [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) .
