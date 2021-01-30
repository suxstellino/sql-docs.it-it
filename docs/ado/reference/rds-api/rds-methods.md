---
description: Metodi di Servizi Desktop remoto
title: Metodi RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- RDS methods [ADO]
- methods [ADO], RDS
ms.assetid: c2c6af1a-3c44-4c9d-ad33-b381552c71af
author: rothja
ms.author: jroth
ms.openlocfilehash: c5eb9926646d247bc252232d3fab155337f03890
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168840"
---
# <a name="rds-methods"></a>Metodi di Servizi Desktop remoto
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
|Metodo|Descrizione|  
|-|-|  
|[Annulla (RDS)](./cancel-method-rds.md)|Annulla l'esecuzione di una chiamata al metodo asincrona in sospeso.|  
|[CancelUpdate (Servizi Desktop remoto)](./cancelupdate-method-rds.md)|Annulla tutte le modifiche apportate alla riga corrente o nuova di un oggetto **Recordset** .|  
|[ConvertToString (Servizi Desktop remoto)](./converttostring-method-rds.md)|Converte un **Recordset** in una stringa MIME che rappresenta i dati del recordset.|  
|[CreateObject (Servizi Desktop remoto)](./createobject-method-rds.md)|Crea il proxy per l'oggetto business di destinazione e restituisce un puntatore a tale oggetto.|  
|[CreateRecordset (Servizi Desktop remoto)](./createrecordset-method-rds.md)|Crea un **Recordset** vuoto e disconnesso.|  
|[Metodo Execute (Servizi Desktop remoto)](./execute-method-rds.md)|Eseguire la richiesta e creare un set di righe di dati avanzato (da usare con ADO 2,5 e versioni successive).|  
|[Metodo Execute21 (Servizi Desktop remoto)](./execute21-method-rds.md)|Eseguire la richiesta e creare un set di righe di dati avanzato (per l'utilizzo con ADO 2,1).|  
|[InvokeService (Servizi Desktop remoto)](./invokeservice-rds.md)|Restituisce un puntatore all'interfaccia richiesta su una versione più idonea dell'oggetto.|  
|[MoveFirst, MoveLast, MoveNext, MovePrevious (RDS)](./movefirst-movelast-movenext-and-moveprevious-methods-rds.md)|Passa al record primo, ultimo, successivo o precedente in un oggetto **Recordset** specificato.|  
|[Query (Servizi Desktop remoto)](./query-method-rds.md)|Usa una stringa di query SQL valida per restituire un **Recordset**.|  
|[Aggiornamento (Servizi Desktop remoto)](./refresh-method-rds.md)|Esegue una query sull'origine dati specificata nella proprietà **Connect** e aggiorna i risultati della query.|  
|[Reimposta (RDS)](./reset-method-rds.md)|Esegue l'ordinamento o il filtro su un **Recordset** sul lato client, in base alle proprietà di ordinamento e filtro specificate.|  
|[SubmitChanges (Servizi Desktop remoto)](./submitchanges-method-rds.md)|Invia le modifiche in sospeso del **Recordset** memorizzato nella cache locale e aggiornabile all'origine dati specificata nella proprietà **Connect** .|  
|[Metodo Synchronize (Servizi Desktop remoto)](./synchronize-method-rds.md)|Sincronizzare il recordset specificato con il database specificato dalla stringa di connessione (per l'utilizzo con ADO 2,5 e versioni successive).|  
|[Metodo Synchronize21 (Servizi Desktop remoto)](./synchronize21-method-rds.md)|Sincronizzare il recordset specificato con il database specificato dalla stringa di connessione (per l'utilizzo con ADO 2,1).|