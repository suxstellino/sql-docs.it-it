---
description: Proprietà Handler (Servizi Desktop remoto)
title: Proprietà Handler (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Handler property [ADO]
ms.assetid: fdc34362-6d47-4727-b171-8d033159408e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4829eadfc91be74ae3f487d9f315162788b7ca0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053122"
---
# <a name="handler-property-rds"></a>Proprietà Handler (Servizi Desktop remoto)
Indica il nome di un programma di personalizzazione sul lato server (gestore) che estende la funzionalità di [RDSServer. DataFactory](./datafactory-object-rdsserver.md)ed eventuali parametri usati dal *gestore*.  
  
 **Si applica a:** [oggetto DataControl (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataControl.Handler = String  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataControl*  
 Variabile oggetto che rappresenta un Servizi Desktop remoto [. Oggetto DataControl](./datacontrol-object-rds.md) .  
  
 *Stringa*  
 Valore **stringa** che contiene il nome del gestore e qualsiasi parametro, tutti separati da virgole (ad esempio, `"handlerName,parm1,parm2,...,parm` *N* `"` ).  
  
## <a name="remarks"></a>Commenti  
 Questa proprietà supporta la [personalizzazione](../../guide/remote-data-service/datafactory-customization.md), una funzionalità che richiede l'impostazione della proprietà [CursorLocation](../ado-api/cursorlocation-property-ado.md) su **adUseClient**.  
  
 Il nome del gestore e i relativi parametri, se presenti, sono separati da virgole (","). Si otterrà un comportamento imprevedibile se un punto e virgola (";") viene visualizzato in qualsiasi punto all'interno della *stringa*. È possibile scrivere un gestore personalizzato, purché supporti l'interfaccia **IDataFactoryHandler** .  
  
 Il nome del gestore predefinito è **MSDFMAP. Il gestore** e il relativo parametro predefinito sono un file di personalizzazione denominato **MSDFMAP.INI**. Utilizzare questa proprietà per richiamare file di personalizzazione alternativi creati dall'amministratore del server.  
  
 L'alternativa all'impostazione della proprietà **handler** consiste nel specificare un gestore e i parametri nella proprietà [ConnectionString](../ado-api/connectionstring-property-ado.md) . ovvero "**handler =**_handlerName, parametro1, parametro2,...;_".  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataControl (Servizi Desktop remoto)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Handler (VB)](./handler-property-example-vb.md)   
 [Personalizzazione di datafactory](../../guide/remote-data-service/datafactory-customization.md)   
 [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)