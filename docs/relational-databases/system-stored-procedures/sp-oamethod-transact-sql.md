---
description: sp_OAMethod (Transact-SQL)
title: sp_OAMethod (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAMethod
- sp_OAMethod_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAMethod
ms.assetid: 1dfaebe2-c7cf-4041-a586-5d04faf2e25e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da84a59353baffa92ac0fa30e55bfa4b248379f7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182415"
---
# <a name="sp_oamethod-transact-sql"></a>sp_OAMethod (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Chiama un metodo di un oggetto OLE.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_OAMethod objecttoken , methodname  
    [ , returnvalue OUTPUT ]   
    [ , [ @parametername = ] parameter [ OUTPUT ] [ ...n ] ]   
```  
  
## <a name="arguments"></a>Argomenti  
 *objecttoken*  
 Token dell'oggetto di un oggetto OLE creato in precedenza tramite **sp_OACreate**.  
  
 *MethodName*  
 Nome del metodo dell'oggetto OLE da chiamare.  
  
 **output** di returnValue    
 Valore restituito del metodo dell'oggetto OLE. Se specificato, deve essere una variabile locale del tipo di dati appropriato.  
  
 Se il metodo restituisce un singolo valore, specificare una variabile locale per *returnValue*, che restituisce il valore restituito del metodo nella variabile locale, oppure non specificare *returnValue*, che restituisce il valore restituito del metodo al client come set di risultati a colonna singola.  
  
 Se il valore restituito del metodo è un oggetto OLE, *returnValue* deve essere una variabile locale di tipo di dati **int**. Un token di oggetto viene archiviato nella variabile locale e questo token dell'oggetto può essere utilizzato con altre stored procedure di automazione OLE.  
  
 Quando il valore restituito del metodo è una matrice, se *returnValue* viene specificato, viene impostato su null.  
  
 Viene generato un errore in uno dei casi seguenti:  
  
-   *returnValue* viene specificato, ma il metodo non restituisce alcun valore.  
  
-   Il metodo restituisce una matrice a più di due dimensioni.  
  
-   Il metodo restituisce una matrice come parametro di output.  
  
`[ _@parametername = ] parameter[ OUTPUT ]` È un parametro del metodo. Se specificato, il *parametro* deve essere un valore del tipo di dati appropriato.  
  
 Per ottenere il valore restituito di un parametro di output, il *parametro* deve essere una variabile locale del tipo di dati appropriato ed è necessario specificare l' **output** . Se viene specificato un parametro costante o se l' **output** non è specificato, qualsiasi valore restituito da un parametro di output viene ignorato.  
  
 Se specificato, *parameterName* deve corrispondere al nome del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] parametro denominato. Si noti che **@** non _parametername_is una [!INCLUDE[tsql](../../includes/tsql-md.md)] variabile locale. Il simbolo di chiocciola ( **@** ) viene rimosso e *parameterName* viene passato all'oggetto OLE come nome del parametro. Tutti i parametri denominati devono essere specificati dopo tutti i parametri posizionali.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare più parametri.  
  
> [!NOTE]
>  *\@ parameterName* può essere un parametro denominato perché fa parte del metodo specificato e viene passato all'oggetto. Gli altri parametri della stored procedure vengono specificati in base alla posizione e non in base al nome.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo) corrispondente al valore intero del codice HRESULT restituito dall'oggetto di automazione OLE.  
  
 Per ulteriori informazioni sui codici restituiti HRESULT, i [codici restituiti e le informazioni sull'errore di automazione OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Set di risultati  
 Se il valore restituito del metodo è una matrice a una o due dimensioni, la matrice viene restituita al client come set di risultati:  
  
-   Una matrice unidimensionale viene restituita al client come set di risultati a riga singola con lo stesso numero di colonne del numero di elementi nella matrice. In altri termini, la matrice viene restituita come (colonne).  
  
-   Una matrice bidimensionale viene restituita al client come set di risultati costituito da un numero di colonne pari al numero di elementi della prima dimensione della matrice e un numero di righe pari al numero di elementi della seconda dimensione della matrice. In altri termini, la matrice viene restituita come (colonne, righe).  
  
 Quando un valore restituito da una proprietà o un valore restituito dal metodo è una matrice, **sp_OAGetProperty** o **sp_OAMethod** restituisce un set di risultati al client. I parametri di output dei metodi non possono essere rappresentati da matrici. Queste procedure eseguono un'analisi di tutti i valori di dati della matrice per determinare quali sono i tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appropriati e la lunghezza di dati da utilizzare per ogni colonna del set di risultati. Per una colonna specifica queste procedure utilizzano il tipo di dati e la lunghezza necessari per rappresentare tutti i valori di dati della colonna.  
  
 Se a tutti i valori di dati di una colonna è associato lo stesso tipo di dati, tale tipo verrà applicato all'intera colonna. Se i valori di dati di una colonna sono tipi di dati diversi, il tipo di dati della colonna viene scelto in base allo schema seguente.  
  
||INT|float|money|Datetime|varchar|NVARCHAR|  
|------|---------|-----------|-----------|--------------|-------------|--------------|  
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="remarks"></a>Osservazioni  
 È anche possibile usare **sp_OAMethod** per ottenere un valore della proprietà.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o l'autorizzazione Execute direttamente in questa stored procedure. `Ole Automation Procedures` la configurazione deve essere **abilitata** per l'utilizzo di qualsiasi procedura di sistema correlata all'automazione OLE.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-calling-a-method"></a>R. Chiamata di un metodo  
 Nell'esempio seguente viene chiamato il `Connect` metodo dell'oggetto **SqlServer** creato in precedenza.  
  
```  
EXEC @hr = sp_OAMethod @object, 'Connect', NULL, 'my_server',  
    'my_login', 'my_password';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
### <a name="b-getting-a-property"></a>B. Recupero di una proprietà  
 Nell'esempio seguente viene ottenuta la `HostName` Proprietà (dell'oggetto **SqlServer** creato in precedenza) e viene archiviata in una variabile locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAMethod @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
PRINT @property;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di automazione OLE &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Script di automazione OLE di esempio](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
