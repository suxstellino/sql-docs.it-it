---
description: sp_OACreate (Transact-SQL)
title: sp_OACreate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OACreate
- sp_OACreate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OACreate
ms.assetid: eb84c0f1-26dd-48f9-9368-13ee4a30a27c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6d44aedf1328234cf51366c3e81bcfb2d6dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191984"
---
# <a name="sp_oacreate-transact-sql"></a>sp_OACreate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un'istanza di un oggetto OLE.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_OACreate { progid | clsid } , objecttoken OUTPUT [ , context ]   
```  
  
## <a name="arguments"></a>Argomenti  
 *ProgID*  
 ProgID dell'oggetto OLE da creare. Questa stringa di caratteri descrive la classe dell'oggetto OLE e ha il formato: **'**_OleComponent_**.** _Oggetto_**'**  
  
 *OleComponent* è il nome del componente del server di automazione OLE, mentre *Object* è il nome dell'oggetto OLE. L'oggetto OLE specificato deve essere valido e deve supportare l'interfaccia **IDispatch** .  
  
 Ad esempio, SQLDMO. SQLServer è il ProgID dell'oggetto SQL-DMO **SqlServer** . Il nome del componente di SQL-DMO è SQLDMO, l'oggetto **SqlServer** è valido e, come tutti gli oggetti SQL-DMO, l'oggetto **SqlServer** supporta **IDispatch**.  
  
 *CLSID*  
 CLSID dell'oggetto OLE da creare. Questa stringa di caratteri descrive la classe dell'oggetto OLE e ha il formato seguente: **' {**_nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn_**}'**. L'oggetto OLE specificato deve essere valido e deve supportare l'interfaccia **IDispatch** .  
  
 Ad esempio, {00026BA1-0000-0000-C000-000000000046} è il CLSID dell'oggetto SQL-DMO **SqlServer** .  
  
  **output** objecttoken  
 Token dell'oggetto restituito e deve essere una variabile locale di tipo di dati **int**. Questo token dell'oggetto identifica l'oggetto OLE creato e viene utilizzato nelle chiamate alle altre stored procedure di automazione OLE.  
  
 *context*  
 Specifica il contesto di esecuzione in cui viene eseguito il nuovo oggetto OLE. I possibili valori sono i seguenti:  
  
 **1** = solo server OLE in-process (. dll).  
  
 **4** = solo server OLE locale (exe).  
  
 **5** = consentito sia nel server OLE in-process che nel server OLE locale  
  
 Se non è specificato, il valore predefinito è **5**. Questo valore viene passato come parametro *dwClsContext* della chiamata a **CoCreateInstance**.  
  
 Se è consentito un server OLE in-process (usando un valore di contesto **1** o **5** o non specificando un valore di contesto), ha accesso alla memoria e ad altre risorse di proprietà di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un server OLE in-process può danneggiare la memoria o le risorse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], con conseguenti risultati imprevisti, ad esempio un errore di violazione di accesso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Quando si specifica il valore di contesto **4**, un server OLE locale non ha accesso alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risorse e non può danneggiare la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] memoria o le risorse.  
  
> [!NOTE]  
>  I parametri di questa stored procedure vengono specificati in base alla posizione, non in base al nome.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo) corrispondente al valore intero del codice HRESULT restituito dall'oggetto di automazione OLE.  
  
 Per ulteriori informazioni sui codici restituiti HRESULT, vedere [codici restituiti e informazioni sugli errori di automazione OLE](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="remarks"></a>Commenti  
 Se sono abilitate le procedure di automazione OLE, una chiamata a **sp_OACreate** avvierà l'ambiente di esecuzione condiviso di automazione OLE. Per ulteriori informazioni sull'abilitazione dell'automazione OLE, vedere [opzione di configurazione del server OLE Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md).  
  
 L'oggetto OLE creato viene distrutto automaticamente al termine del batch di istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o l'autorizzazione Execute direttamente in questa stored procedure. `Ole Automation Procedures` la configurazione deve essere **abilitata** per l'utilizzo di qualsiasi procedura di sistema correlata all'automazione OLE.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-progid"></a>R. Utilizzo di un valore ProgID  
 Nell'esempio seguente viene creato un oggetto SQL-DMO **SqlServer** utilizzando il ProgID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate 'SQLDMO.SQLServer', @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
### <a name="b-using-clsid"></a>B. Utilizzo di un valore CLSID  
 Nell'esempio seguente viene creato un oggetto SQL-DMO **SqlServer** utilizzando il relativo CLSID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate '{00026BA1-0000-0000-C000-000000000046}',  
    @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di automazione OLE &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Opzione di configurazione del server OLE Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)   
 [Script di automazione OLE di esempio](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
