---
description: catalog.configure_catalog (database SSISDB)
title: catalog.configure_catalog (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 72690c61-f462-4c25-9fce-08a687b0bd41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5d52f2ea964433863f735fff505b1d80bb2e3fc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346851"
---
# <a name="catalogconfigure_catalog-ssisdb-database"></a>catalog.configure_catalog (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene configurato il catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] impostando una proprietà del catalogo su un valore specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```sql
catalog.configure_catalog [ @property_name = ] property_name , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @property_name = ] *property_name*  
 Nome della proprietà del catalogo. *property_name* è di tipo **nvarchar(255)**. Per altre informazioni sulle proprietà disponibili, vedere [catalog.catalog_properties &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
 [ @property_value = ] *property_value*  
 Valore della proprietà del catalogo. *property_value* è di tipo **nvarchar(255)**. Per altre informazioni sui valori delle proprietà, vedere [catalog.catalog_properties &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Questa stored procedure determina se *property_value* è valido per ogni *property_name*.  
  
 La stored procedure può essere eseguita solo se non esiste alcuna esecuzione attiva, ad esempio in sospeso, in coda, in esecuzione e sospesa.  
  
 Mentre viene configurato il catalogo, tutte le relative altre stored procedure non vengono completate e generano il messaggio di errore "Server attualmente in fase di esecuzione".
  
 Quando il catalogo viene configurato, nel log delle operazioni viene scritta una voce.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Proprietà inesistente  
  
-   Valore della proprietà non valido.  
  
  
