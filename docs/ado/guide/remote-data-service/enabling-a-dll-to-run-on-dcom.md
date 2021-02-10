---
description: Abilitazione di un file DLL per l'esecuzione su DCOM
title: Abilitazione di una DLL per l'esecuzione su DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DLL on DCOM in RDS [ADO]
- DCOM in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: 5f1c2205-191c-4fb4-9bd9-84c878ea46ed
author: rothja
ms.author: jroth
ms.openlocfilehash: 31aa1fde6ddce21bc314bf14aea3ea563567d4a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031973"
---
# <a name="enabling-a-dll-to-run-on-dcom"></a>Abilitazione di un file DLL per l'esecuzione su DCOM
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Nei passaggi seguenti viene descritto come consentire a un oggetto business. dll di utilizzare sia DCOM che Microsoft Internet Information Services (HTTP) tramite Servizi componenti.  
  
1.  Creare un nuovo pacchetto vuoto nello snap-in MMC Servizi componenti.  
  
     Lo snap-in MMC Servizi componenti verrà utilizzato per creare un pacchetto e aggiungere la DLL al pacchetto. In questo modo il file con estensione dll viene reso accessibile tramite DCOM, ma rimuove l'accessibilità tramite IIS. Se si archivia il registro di sistema per il file con estensione dll, la chiave **InProc** ora è vuota. l'impostazione dell'attributo Activation, descritta più avanti in questo argomento, aggiunge un valore nella chiave **InProc** .  
  
2.  Installare un oggetto business nel pacchetto.  
  
     -oppure-  
  
     Importare l'oggetto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) nel pacchetto.  
  
3.  Impostare l'attributo Activation per il pacchetto su **nel processo del creatore** (applicazione libreria).  
  
     Per rendere accessibile il file con estensione dll tramite DCOM e IIS nello stesso computer, è necessario impostare l'attributo Activation del componente nello snap-in MMC Servizi componenti. Dopo aver impostato l'attributo su **nel processo del creatore**, si noterà che è stata aggiunta una chiave del server **InProc** nel registro di sistema che punta a una dll surrogata di Servizi componenti.  
  
 Per ulteriori informazioni su Servizi componenti (o Microsoft Transaction Service, se si utilizza Windows NT) e su come eseguire questi passaggi, visitare il sito Web di Microsoft Transaction Server.