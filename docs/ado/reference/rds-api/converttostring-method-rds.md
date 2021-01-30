---
description: Metodo ConvertToString (Servizi Desktop remoto)
title: Metodo ConvertToString (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ConvertToString method [ADO]
ms.assetid: b3f36bc8-6f69-49b0-83cd-2ccd3afebfbe
author: rothja
ms.author: jroth
ms.openlocfilehash: 4edcebbfdee8a9a1411189786593c129ee32771c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169035"
---
# <a name="converttostring-method-rds"></a>Metodo ConvertToString (Servizi Desktop remoto)
Converte un [Recordset](../ado-api/recordset-object-ado.md) in una stringa MIME che rappresenta i dati del recordset.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
DataFactory.ConvertToString(Recordset)  
```  
  
#### <a name="parameters"></a>Parametri  
 *DataFactory*  
 Variabile oggetto che rappresenta un oggetto [RDSServer. DataFactory](./datafactory-object-rdsserver.md) .  
  
 *Recordset*  
 Variabile oggetto che rappresenta un oggetto **Recordset** .  
  
## <a name="remarks"></a>Commenti  
 Con i file ASP, usare **ConvertToString** per incorporare il **Recordset** in una pagina HTML generata sul server per il trasporto in un computer client.  
  
 **ConvertToString** carica innanzitutto il **Recordset** nelle tabelle dei servizi del cursore e quindi genera un flusso in formato MIME.  
  
 Nel client, Remote Data Service può convertire di nuovo la stringa MIME in un **Recordset** completamente funzionante. Funziona bene per gestire meno di 400 righe di dati con una larghezza non superiore a 1024 byte per riga. Non è consigliabile usarlo per il flusso di dati BLOB e set di risultati di grandi dimensioni tramite HTTP. Sulla stringa non viene eseguita alcuna compressione di rete, quindi i set di dati di dimensioni molto grandi impiegheranno molto tempo per il trasporto su HTTP rispetto al formato Tablegram ottimizzato per la rete definito e distribuito da Remote Data Service come formato del protocollo di trasporto nativo.  
  
> [!NOTE]
>  Se si usano Active Server pagine per incorporare la stringa MIME risultante in una pagina HTML del client, tenere presente che le versioni di VBScript precedenti alla versione 2,0 limitano le dimensioni della stringa a 32K. Se questo limite viene superato, viene restituito un errore. Quando si usa l'incorporamento MIME tramite i file ASP, l'ambito della query è relativamente piccolo. Per risolvere il problema, scaricare la versione più recente di VBScript dal sito Web Microsoft Windows Script Technologies.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di metodo ConvertToString (VB)](../ado-api/converttostring-method-example-vb.md)   
 [Esempio del metodo ConvertToString (VBScript)](./converttostring-method-example-vbscript.md)