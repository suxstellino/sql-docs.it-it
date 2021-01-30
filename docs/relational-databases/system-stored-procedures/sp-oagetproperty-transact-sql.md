---
description: sp_OAGetProperty (Transact-SQL)
title: sp_OAGetProperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetProperty_TSQL
- sp_OAGetProperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetProperty
ms.assetid: 240eeeb9-6d8b-4930-b912-1d273ca0ab38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0542ae7cf67fb5b05c63a4c8cdb9ef9fb31c62e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174412"
---
# <a name="sp_oagetproperty-transact-sql"></a>sp_OAGetProperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ottiene il valore di una proprietà di un oggetto OLE.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_OAGetProperty objecttoken , propertyname   
    [ , propertyvalue OUTPUT ]  
    [ , index...]   
```  
  
## <a name="arguments"></a>Argomenti  
 *objecttoken*  
 Token dell'oggetto di un oggetto OLE creato in precedenza tramite **sp_OACreate**.  
  
 *propertyname*  
 Nome di proprietà dell'oggetto OLE da restituire.  
  
  **output** di PropertyValue  
 Valore di proprietà restituito. Se specificato, deve essere una variabile locale del tipo di dati appropriato.  
  
 Se la proprietà restituisce un oggetto OLE, *PropertyValue* deve essere una variabile locale di tipo di dati **int**. Un token di oggetto viene archiviato nella variabile locale e questo token dell'oggetto può essere utilizzato con altre stored procedure di automazione OLE.  
  
 Se la proprietà restituisce un singolo valore, specificare una variabile locale per *PropertyValue*, che restituisce il valore della proprietà nella variabile locale. in alternativa, non specificare *PropertyValue*, che restituisce il valore della proprietà al client come set di risultati a colonna singola e a riga singola.  
  
 Quando la proprietà restituisce una matrice, se viene specificato *PropertyValue* , viene impostato su null.  
  
 Se *PropertyValue* viene specificato, ma la proprietà non restituisce un valore, si verifica un errore. Viene inoltre generato un errore se la proprietà restituisce una matrice a più di due dimensioni.  
  
 *index*  
 Parametro di indice. Se specificato, *index* deve essere un valore del tipo di dati appropriato.  
  
 Ad alcune proprietà sono associati parametri. Tali proprietà sono denominate proprietà indicizzate e i parametri corrispondenti sono denominati parametri di indice. A una proprietà possono essere associati più parametri di indice.  
  
> [!NOTE]  
>  I parametri di questa stored procedure vengono specificati in base alla posizione, non in base al nome.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo) corrispondente al valore intero del codice HRESULT restituito dall'oggetto di automazione OLE.  
  
 Per ulteriori informazioni sui codici restituiti HRESULT, vedere [codici restituiti e informazioni sugli errori di automazione OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Set di risultati  
 Se la proprietà restituisce una matrice a una o due dimensioni, la matrice viene restituita al client come set di risultati:  
  
-   Una matrice unidimensionale viene restituita al client come set di risultati a riga singola con lo stesso numero di colonne del numero di elementi nella matrice. In altri termini, la matrice viene restituita in forma di colonne.  
  
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
  
### <a name="a-using-a-local-variable"></a>R. Utilizzo di una variabile locale  
 Nell'esempio seguente viene ottenuta la `HostName` Proprietà (dell'oggetto **SqlServer** creato in precedenza) e viene archiviata in una variabile locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAGetProperty @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END  
PRINT @property;  
```  
  
### <a name="b-using-a-result-set"></a>B. Utilizzo di un set di risultati  
 Nell'esempio seguente viene ottenuta la `HostName` proprietà dell'oggetto **SqlServer** creato in precedenza e viene restituita al client come set di risultati.  
  
```  
EXEC @hr = sp_OAGetProperty @object, 'HostName';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di automazione OLE &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Script di automazione OLE di esempio](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
