---
description: '&#x40;&#x40;OPTIONS (Transact-SQL)'
title: '@@OPTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@OPTIONS'
- '@@OPTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, current SET options
- '@@OPTIONS function'
- current SET options
ms.assetid: 3d5c7f6e-157b-4231-bbb4-4645a11078b3
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 43da748085a31a9d9f25ef20bdb690f2b94e4cb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183940"
---
# <a name="x40x40options-transact-sql"></a>&#x40;&#x40;OPTIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sulle opzioni SET correnti.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
@@OPTIONS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **integer**  
  
## <a name="remarks"></a>Osservazioni  
 Le opzioni possono derivare dall'uso del comando **SET** o dal valore di **sp_configure user options**. I valori di sessione configurati con il comando **SET** sostituiscono le opzioni di **sp_configure**. Molti strumenti, come ad esempio [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], configurano automaticamente le opzioni impostate. A ogni utente è associata una funzione @@OPTIONS che rappresenta la configurazione.  
  
 Tramite l'istruzione SET è possibile modificare le opzioni di linguaggio e di esecuzione delle query per una sessione utente specifica. **\@\@OPTIONS** è in grado di rilevare solo le opzioni impostate su ON o OFF.  
  
 La funzione **\@\@OPTIONS** restituisce una mappa di bit delle opzioni, convertita in un numero intero in base 10 (decimale). Le impostazioni di bit vengono archiviate nei percorsi descritti in un tabella dell'argomento [Configurare l'opzione di configurazione del server user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md).  
  
 Per decodificare il valore di **\@\@OPTIONS**, convertire il numero intero restituito da **\@\@OPTIONS** in binario e quindi cercare i valori nella tabella riportata in [Configurare l'opzione di configurazione del server user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md). Se, ad esempio, `SELECT @@OPTIONS;` restituisce il valore `5496`, usare la calcolatrice di Windows (**calc.exe**) per convertire il valore decimale `5496` in binario. Il risultato è `1010101111000`. I caratteri più a destra (binario 1, 2 e 4) corrispondono a 0. Ciò indica che i primi tre elementi della tabella sono impostati su OFF. Consultando la tabella, si noterà che si tratta di **DISABLE_DEF_CNST_CHK**, **IMPLICIT_TRANSACTIONS** e **CURSOR_CLOSE_ON_COMMIT**. L'elemento successivo, ovvero **ANSI_WARNINGS** nella posizione `1000`, è impostato su ON. Continuare a lavorare verso sinistra nella mappa di bit e verso il basso nell'elenco di opzioni. Quando le opzioni più a sinistra corrispondono a 0, vengono troncate dalla conversione dei tipi. La mappa di bit `1010101111000` è in realtà `001010101111000` per rappresentare tutte e 15 le opzioni.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-demonstration-of-how-changes-affect-behavior"></a>R. Dimostrazione di come le modifiche influiscono sul comportamento  
 L'esempio seguente illustra la differenza nel comportamento di concatenazione con due diverse impostazioni dell'opzione **CONCAT_NULL_YIELDS_NULL**.  
  
```sql  
SELECT @@OPTIONS AS OriginalOptionsValue;  
SET CONCAT_NULL_YIELDS_NULL OFF;  
SELECT 'abc' + NULL AS ResultWhen_OFF, @@OPTIONS AS OptionsValueWhen_OFF;  
  
SET CONCAT_NULL_YIELDS_NULL ON;  
SELECT 'abc' + NULL AS ResultWhen_ON, @@OPTIONS AS OptionsValueWhen_ON;  
```  
  
### <a name="b-testing-a-client-nocount-setting"></a>B. Test di un'impostazione NOCOUNT del client  
 Nell'esempio seguente viene impostata l'opzione `NOCOUNT``ON` e viene quindi testato il valore di `@@OPTIONS`. L'opzione `NOCOUNT``ON` impedisce che, per ogni istruzione di una sessione, il messaggio relativo al numero di righe interessate venga inviato al client che esegue la richiesta. Il valore di `@@OPTIONS` viene impostato su `512` (0x0200), che rappresenta l'opzione NOCOUNT. In questo esempio viene verificato se l'opzione NOCOUNT è stata abilitata nel client. Ciò può risultare utile, ad esempio, per tenere traccia delle differenze di esecuzione in un client.  
  
```sql  
SET NOCOUNT ON  
IF @@OPTIONS & 512 > 0   
RAISERROR ('Current user has SET NOCOUNT turned on.', 1, 1)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di configurazione &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Configurare l'opzione di configurazione del server user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)  
  
  
