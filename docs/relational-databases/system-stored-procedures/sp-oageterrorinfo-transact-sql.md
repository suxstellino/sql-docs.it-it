---
description: sp_OAGetErrorInfo (Transact-SQL)
title: sp_OAGetErrorInfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetErrorInfo_TSQL
- sp_OAGetErrorInfo
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetErrorInfo
ms.assetid: ceecea08-456f-4819-85d9-ecc9647d7187
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d55126c7feb362b910e83034a66e4d691f1be84c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191981"
---
# <a name="sp_oageterrorinfo-transact-sql"></a>sp_OAGetErrorInfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ottiene informazioni sull'errore di automazione OLE.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_OAGetErrorInfo [ objecttoken ]  
    [ , source OUTPUT ]   
    [ , description OUTPUT ]   
    [ , helpfile OUTPUT ]   
    [ , helpid OUTPUT ]   
```  
  
## <a name="arguments"></a>Argomenti  
 *objecttoken*  
 Token di oggetto di un oggetto OLE creato in precedenza tramite **sp_OACreate** o è null. Se viene specificato *objecttoken* , vengono restituite le informazioni sull'errore relative a tale oggetto. Se viene specificato NULL, vengono restituite le informazioni sull'errore relative all'intero batch.  
  
  **output** origine  
 Origine delle informazioni sull'errore. Se specificato, deve essere una variabile locale **char**, **nchar**, **varchar** o **nvarchar** . Se necessario, il valore restituito viene troncato in base alla dimensione della variabile locale.  
  
  **output** Descrizione  
 Descrizione dell'errore. Se specificato, deve essere una variabile locale **char**, **nchar**, **varchar** o **nvarchar** . Se necessario, il valore restituito viene troncato in base alla dimensione della variabile locale.  
  
  **output** di fileguida  
 File della Guida relativo all'oggetto OLE. Se specificato, deve essere una variabile locale **char**, **nchar**, **varchar** o **nvarchar** . Se necessario, il valore restituito viene troncato in base alla dimensione della variabile locale.  
  
  **output** HelpID  
 ID di contesto del file della Guida. Se specificato, deve essere una variabile **int** locale.  
  
> [!NOTE]  
>  I parametri di questa stored procedure vengono specificati in base alla posizione, non in base al nome.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo) corrispondente al valore intero del codice HRESULT restituito dall'oggetto di automazione OLE.  
  
 Per ulteriori informazioni sui codici restituiti HRESULT, vedere [codici restituiti e informazioni sugli errori di automazione OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Set di risultati  
 Se non viene specificato alcun parametro di output, le informazioni sull'errore vengono restituite al client come set di risultati.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|------------------|---------------|-----------------|  
|**Error (Errore) (Error (Errore)e)**|**binario (4)**|Rappresentazione binaria del numero di errore.|  
|**Origine**|**nvarchar (nn)**|Origine dell'errore.|  
|**Descrizione**|**nvarchar (nn)**|Descrizione dell'errore.|  
|**HelpFile**|**nvarchar (nn)**|File della Guida relativo all'origine.|  
|**HelpID**|**int**|ID di contesto della Guida nel file di origine della Guida.|  
  
## <a name="remarks"></a>Commenti  
 Ogni chiamata a un stored procedure di automazione OLE (eccetto **sp_OAGetErrorInfo**) Reimposta le informazioni sull'errore. Pertanto, **sp_OAGetErrorInfo** ottiene informazioni sugli errori solo per la chiamata stored procedure di automazione OLE più recente. Si noti che poiché **sp_OAGetErrorInfo** non reimposta le informazioni sull'errore, può essere chiamato più volte per ottenere le stesse informazioni sull'errore.  
  
 Nella tabella seguente vengono elencati gli errori di automazione OLE e le cause più comuni.  
  
|Errore e codice HRESULT|Causa più comune|  
|-----------------------|------------------|  
|**Tipo di variabile non valido (0x80020008)**|Il tipo di dati di un [!INCLUDE[tsql](../../includes/tsql-md.md)] valore passato come parametro del metodo non corrisponde al [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] tipo di dati del parametro del metodo oppure è stato passato un valore null come parametro del metodo.|  
|**Nome sconosciuto (0x8002006)**|Il nome di proprietà o metodo specificato non è stato trovato per l'oggetto specificato.|  
|**Stringa della classe non valida (0x800401f3)**|Il valore ProgID o CLSID specificato non è registrato come oggetto OLE in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È necessario registrare i server di automazione OLE personalizzati prima di potervi creare un'istanza utilizzando **sp_OACreate**. A tale scopo, è possibile utilizzare l'utilità Regsvr32.exe per i server in-process (dll) o l'opzione della riga di comando **/regserver** per i server locali (exe).|  
|**Esecuzione del server non completata (0x80080005)**|L'oggetto OLE specificato è registrato come server OLE locale (file exe), ma non è stato possibile trovare o avviare il file.|  
|**Impossibile trovare il modulo specificato (0x8007007e)**|L'oggetto OLE specificato è registrato come server OLE in-process (file dll), ma non è stato possibile trovare o caricare il file.|  
|**Tipo non corrispondente (0x80020005)**|Il tipo di dati di una variabile locale [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizzata per l'archiviazione del valore restituito di una proprietà o un metodo non corrisponde al tipo di dati di [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] del valore restituito della proprietà o del metodo oppure è stato richiesto il valore restituito di una proprietà o un metodo ma non viene restituito alcun valore.|  
|**Il tipo di dati o il valore del parametro ' context ' di sp_OACreate non è valido. (0x8004275B)**|Il valore del parametro di contesto deve essere 1, 4 o 5.|  
  
 Per ulteriori informazioni sull'elaborazione dei codici restituiti HRESULT, vedere [codici restituiti e informazioni sugli errori di automazione OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o l'autorizzazione Execute direttamente in questa stored procedure. `Ole Automation Procedures` la configurazione deve essere **abilitata** per l'utilizzo di qualsiasi procedura di sistema correlata all'automazione OLE.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono visualizzate le informazioni sull'errore di automazione OLE.  
  
```  
DECLARE @output varchar(255);  
DECLARE @hr int;  
DECLARE @source varchar(255);  
DECLARE @description varchar(255);  
PRINT 'OLE Automation Error Information';  
EXEC @hr = sp_OAGetErrorInfo @object, @source OUT, @description OUT;  
IF @hr = 0  
BEGIN  
    SELECT @output = '  Source: ' + @source  
    PRINT @output  
    SELECT @output = '  Description: ' + @description  
    PRINT @output  
END  
ELSE  
BEGIN  
    PRINT '  sp_OAGetErrorInfo failed.'  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di automazione OLE &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Script di automazione OLE di esempio](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
