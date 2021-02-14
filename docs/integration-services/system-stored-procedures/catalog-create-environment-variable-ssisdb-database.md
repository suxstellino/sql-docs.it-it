---
description: catalog.create_environment_variable (database SSISDB)
title: catalog.create_environment_variable (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346812"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una variabile di ambiente nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>Argomenti  
 [@folder_name =] *folder_name*  
 Nome della cartella in cui è contenuto l'ambiente. *folder_name* è di tipo **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 Il nome dell'ambiente. *environment_name* è di tipo **nvarchar(128)** .  
  
 [@variable_name =] *variable_name*  
 Nome della variabile di ambiente. *variable_name* è di tipo **nvarchar(128)**.  
  
 [@data_type =] *data_type*  
 Tipo di dati della variabile. I tipi di dati di variabile di ambiente supportati includono **Boolean**, **Byte**, **DateTime**, **Double**, **Int16**, **Int32**, **Int64**, **Single**, **String**, **UInt32** e **UInt64**. Tra i tipi di dati di variabile di ambiente non supportati sono inclusi **Char**, **DBNull**, **Object** e **Sbyte**. Il tipo di dati del parametro *data_type* è **nvarchar(128)**.  
  
 [@sensitive =] *sensitive*  
 Viene indicato se nella variabile è contenuto o meno un valore importante. Utilizzare un valore pari a `1`, per indicare che il valore della variabile di ambiente è importante o, in caso contrario, un valore pari a `0`. Un valore, se importante, viene crittografato quando viene archiviato; altrimenti, viene archiviato non crittografato. *Sensitive* è di tipo **bit**.  
  
 [@value =] *value*  
 Valore della variabile di ambiente. *value* è di tipo **sql_variant**.  
  
 [@description =] *description*  
 Descrizione della variabile di ambiente. *value* è di tipo **nvarchar(1024)**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sull'ambiente  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Nome della cartella, nome dell'ambiente o nome della variabile di ambiente non valido  
  
-   Nome della variabile già esistente nell'ambiente  
  
-   Utente senza autorizzazioni appropriate.  
  
## <a name="remarks"></a>Osservazioni  
 Una variabile di ambiente può essere utilizzata per assegnare in modo efficace un valore a un parametro del progetto o a un parametro del pacchetto da utilizzare nell'esecuzione di un pacchetto. Le variabili di ambiente consentono l'organizzazione dei valori del parametro. I nomi della variabile devono essere univoci all'interno di un ambiente.  
  
 La stored procedure consente di convalidare il tipo di dati della variabile per assicurarsi che sia supportato dal catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
> [!TIP]  
>  In [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] si consideri di usare il tipo di dati **Int16** al posto del tipo di dati **Sbyte** non supportato.  
  
 Il valore passato a questa stored procedure con il parametro *value* viene convertito da un tipo di dati di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in un tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in base alla tabella seguente:  
  
|Tipo di dati di Integration Services|Tipo di dati di SQL Server|  
|------------------------------------|--------------------------|  
|**Boolean**|**bit**|  
|**Byte**|**binary**, **varbinary**|  
|**DateTime**|**datetime**, **datetime2**, **datetimeoffset**, **smalldatetime**|  
|**Double**|Numerico esatto: **decimale**, **numerico**; numerico approssimato: **float**, **reale**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**Singolo**|Numerico esatto: **decimale**, **numerico**; numerico approssimato: **float**, **reale**|  
|**Stringa**|**varchar**, **nvarchar**, **char**|  
|**UInt32**|**int** (**int** è il mapping disponibile più vicino a **Uint32**).|  
|**UInt64**|**bigint** (**int** è il mapping disponibile più vicino a **Uint64**).|  
  
  
