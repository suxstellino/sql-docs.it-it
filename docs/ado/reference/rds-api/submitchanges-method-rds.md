---
description: Metodo SubmitChanges (Servizi Desktop remoto)
title: Metodo SubmitChanges (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SubmitChanges method [ADO]
ms.assetid: 250062a4-13c4-4bed-807d-8b9ad81536d4
author: rothja
ms.author: jroth
ms.openlocfilehash: 052c131cb430d911d37f6782565c00a1d3cc53d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166109"
---
# <a name="submitchanges-method-rds"></a>Metodo SubmitChanges (Servizi Desktop remoto)
Invia le modifiche in sospeso del [Recordset](../ado-api/recordset-object-ado.md) memorizzato nella cache locale e aggiornabile all'origine dati specificata nella proprietà [Connect](./connect-property-rds.md) o nella proprietà [URL](./url-property-rds.md) .  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.SubmitChanges DataFactory.SubmitChanges Connection, Recordset  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *DataFactory*  
 Variabile oggetto che rappresenta un oggetto [RDSServer. DataFactory](./datafactory-object-rdsserver.md) .  
  
 *Connection*  
 Valore **stringa** che rappresenta la connessione creata con **RDS.** Proprietà [Connect](./connect-property-rds.md) dell'oggetto DataControl.  
  
 *Recordset*  
 Variabile oggetto che rappresenta un oggetto **Recordset** .  
  
## <a name="remarks"></a>Commenti  
 Per poter usare il metodo **SubmitChanges** con RDS, è necessario impostare le proprietà [Connect](./connect-property-rds.md), [Server](./server-property-rds.md)e [SQL](./sql-property.md) **. Oggetto DataControl** .  
  
 Se si chiama il metodo [CancelUpdate](./cancelupdate-method-rds.md) dopo aver chiamato **SubmitChanges** per lo stesso oggetto **Recordset** , la chiamata a **CancelUpdate** ha esito negativo perché è già stato eseguito il commit delle modifiche.  
  
 Solo i record modificati vengono inviati per la modifica e tutte le modifiche vengono riuscite o tutte le modifiche vengono interrotte insieme.  
  
 È possibile usare **SubmitChanges** solo con l'oggetto **RDSServer. DataFactory** predefinito. Gli oggetti business personalizzati non possono utilizzare questo metodo.  
  
 Se è stata impostata la proprietà **URL** , **SubmitChanges** invierà le modifiche al percorso specificato dall'URL.  
  
## <a name="applies-to"></a>Si applica a  

:::row:::
    :::column:::
        [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
    :::column-end:::
    :::column:::
        [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo SubmitChanges (VBScript)](./submitchanges-method-example-vbscript.md)   
 [Pulsanti di comando Rubrica](../../guide/remote-data-service/address-book-command-buttons.md)   
 [Metodo CancelUpdate (RDS)](./cancelupdate-method-rds.md)   
 [Metodo Refresh (Servizi Desktop remoto)](./refresh-method-rds.md)