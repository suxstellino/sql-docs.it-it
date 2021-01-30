---
description: ALTER FULLTEXT STOPLIST (Transact-SQL)
title: ALTER FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT STOPLIST
- ALTER_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- ALTER FULLTEXT STOPLIST statement
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: f6ad87d5-6a34-435a-8456-8244947c5c83
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 969de54c394e5ec68b2f8acd6a092f8c3b5ab1f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204212"
---
# <a name="alter-fulltext-stoplist-transact-sql"></a>ALTER FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Inserisce o elimina una parola non significativa nell'elenco di parole non significative full-text predefinito del database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER FULLTEXT STOPLIST stoplist_name  
{   
        ADD [N] 'stopword' LANGUAGE language_term    
  | DROP   
    {  
        'stopword' LANGUAGE language_term   
      | ALL LANGUAGE language_term   
      | ALL  
     }  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *stoplist_name*  
 Nome dell'elenco di parole non significative da modificare. *stoplist_name* può essere composto da un massimo di 128 caratteri.  
  
 **'** *stopword* **'**  
 Stringa che può essere una parola con un significato linguistico nella lingua specificata o token senza significato linguistico. *stopword* può essere composto da un massimo di 64 caratteri, pari alla lunghezza massima dei token. È possibile specificare una parola non significativa come stringa Unicode.  
  
 LANGUAGE *language_term*  
 Specifica la lingua da associare all'elemento *stopword* aggiunto o eliminato.  
  
 È possibile specificare *language_term* come valore stringa, intero o esadecimale corrispondente all'identificatore delle impostazioni locali (LCID) della lingua, come indicato di seguito:  
  
|Formato|Descrizione|  
|------------|-----------------|  
|string|*language_term* corrisponde al valore della colonna **alias** nella vista di compatibilità [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La stringa deve essere racchiusa tra virgolette singole, come in **'** _language_term_*_'_*.|  
|Integer|*language_term* è l'identificatore LCID della lingua.|  
|Valore esadecimale|*language_term* corrisponde a 0x seguito dal valore esadecimale dell'identificatore LCID. Il valore esadecimale non deve superare le otto cifre, inclusi gli zeri iniziali. Se il valore è in formato DBCS (Double-Byte Character Set), verrà convertito in Unicode da SQL Server.|  
  
 ADD **'** _stopword_*_'_* LANGUAGE *language_term*  
 Aggiunge una parola non significativa all'elenco di parole non significative per la lingua specificata da LANGUAGE *language_term*.  
  
 Se la combinazione specificata di parola chiave e valore LCID della lingua non è univoca in STOPLIST, viene restituito un errore.  Se il valore LCID non corrisponde a una lingua registrata, viene generato un errore.  
  
 DROP { **'** _stopword_*_'_* LANGUAGE *language_term* | ALL LANGUAGE *language_term* | ALL }  
 Elimina una parola non significativa dall'elenco di parole non significative.  
  
 **'** *stopword* **'** LANGUAGE *language_term*  
 Elimina la parola non significativa specificata per la lingua specificata da *language_term*.  
  
 ALL LANGUAGE *language_term*  
 Elimina tutte le parole non significative per la lingua specificata da *language_term*.  
  
 ALL  
 Elimina tutte le parole non significative dall'elenco di parole non significative.  
  
## <a name="remarks"></a>Commenti  
 CREATE FULLTEXT STOPLIST è supportato solo per il livello di compatibilità 100 e superiore. Per i livelli di compatibilità 80 e 90, l'elenco di parole non significative di sistema viene sempre assegnato al database.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per designare un elenco di parole non significative come elenco predefinito per il database è necessaria l'autorizzazione ALTER DATABASE. Per modificare in altro modo un elenco di parole non significative è necessario essere il proprietario di tale elenco o appartenere al ruolo predefinito del database **db_owner** o **db_ddladmin**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificato un elenco di parole non significative denominato `CombinedFunctionWordList`, aggiungendo la parola 'en', prima per lo spagnolo, quindi per il francese.  
  
```sql  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'French';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurare e gestire parole non significative ed elenchi di parole non significative per la ricerca full-text](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
