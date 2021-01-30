---
description: sp_cursorprepexec (Transact-SQL)
title: sp_cursorprepexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorprepexec_TSQL
- sp_cursorprepexec
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorprepexec
ms.assetid: 8094fa90-35b5-4cf4-8012-0570cb2ba1e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b59597048ba7edc1102c3151d720e9bc6ff36942
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209696"
---
# <a name="sp_cursorprepexec-transact-sql"></a>sp_cursorprepexec (Transact-SQL)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Compila un piano per il batch o l'istruzione di cursore inviata, quindi crea e popola il cursore. sp_cursorprepexec combina le funzioni di sp_cursorprepare e sp_cursorexecute. Questa procedura viene richiamata specificando ID = 5 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento](../../database-engine/configure-windows/media/topic-link.gif "icona di collegamento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursorprepexec prepared handle OUTPUT, cursor OUTPUT, params , statement , options  
    [ , scrollopt [ , ccopt [ , rowcount ] ] ]  
    [, '@parameter_name[,...n ]']
```  
  
## <a name="arguments"></a>Argomenti  
 *handle preparato*  
 Identificatore dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *handle* preparato generato. *handle preparato* obbligatorio e restituisce **int**.  
  
 *cursor*  
 Identificatore del cursore generato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Cursor* è un parametro obbligatorio che deve essere fornito in tutte le procedure successive che agiscono su questo cursore, ad esempio sp_cursorfetch.  
  
 *params*  
 Identifica le istruzioni con parametri. La definizione *params* delle variabili viene sostituita per i marcatori di parametro nell'istruzione. *params* è un parametro obbligatorio che richiede un valore di input **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Usare una stringa **ntext** come valore di input quando *stmt* è con parametri e il valore di *scrollopt* PARAMETERIZED_STMT è on.  
  
 *istruzione*  
 Definisce il set di risultati del cursore. Il parametro *Statement* è obbligatorio e richiede un valore di input **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Le regole per la specifica del valore stmt sono le stesse di quelle per sp_cursoropen, con l'eccezione che il tipo di dati stringa *stmt* deve essere **ntext**.  
  
 *options*  
 Parametro facoltativo tramite cui viene restituita una descrizione delle colonne dei set di risultati del cursore. * le opzioni richiedono il valore di input **int** seguente.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 Opzione di scorrimento. *scrollopt* è un parametro facoltativo che richiede uno dei valori di input **int** seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 A causa della possibilità che l'opzione richiesta non sia appropriata per il cursore definito da *\<stmt>* , questo parametro funge sia da input sia da output. In casi di questo tipo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna un tipo appropriato e modifica questo valore.  
  
 *ccopt*  
 Opzioni del controllo della concorrenza. *ccopt* è un parametro facoltativo che richiede uno dei valori di input **int** seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (precedentemente noto come LOCKCC)|  
|0x0004|**Ottimistica** (precedentemente nota come OPTCC)|  
|0x0008|OPTIMISTIC (precedentemente noto come OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Come per *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può assegnare un valore diverso da quello richiesto.  
  
 *RowCount*  
 Parametro facoltativo che indica il numero di righe del buffer di recupero da utilizzare con AUTO_FETCH. Il valore predefinito è 20 righe. il *conteggio delle righe* si comporta in modo diverso quando viene assegnato come valore di input rispetto a un valore restituito.  
  
|Come valore di input|Come valore restituito|  
|--------------------|---------------------|  
|Quando AUTO_FETCH viene specificato con FAST_FORWARD cursori *RowCount* rappresenta il numero di righe da inserire nel buffer di recupero.|Rappresenta il numero di righe nel set di risultati. Quando viene specificato il valore *scrollopt* AUTO_FETCH, *RowCount* restituisce il numero di righe recuperate nel buffer di recupero.|  

*parameter_name* Designare uno o più nomi di parametro come definito nell'argomento params.  È necessario specificare un parametro per ogni parametro incluso nei parametri. Questo argomento non è obbligatorio quando l'istruzione Transact-SQL o il batch nei params non contiene parametri definiti.
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Se params restituisce un valore NULL, l'istruzione non è parametrizzata.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorexecute &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursorprepare &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
