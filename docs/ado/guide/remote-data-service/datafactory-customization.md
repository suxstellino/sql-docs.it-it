---
description: Personalizzazione di Data Factory
title: Personalizzazione di datafactory | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory customization in RDS [ADO]
ms.assetid: 86d77985-a0d0-405a-8587-c85a20540a0e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4bc3a017e74e8384bd97d9503663731e353f2242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031983"
---
# <a name="datafactory-customization"></a>Personalizzazione di Data Factory
Remote Data Service (RDS) fornisce un modo per eseguire facilmente l'accesso ai dati in un sistema client/server a tre livelli. Un controllo dati client specifica la connessione e i parametri della stringa di comando per eseguire una query su un'origine dati remota, oppure su una stringa di connessione e parametri dell'oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) per eseguire un aggiornamento.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 I parametri vengono passati a un programma server che esegue l'operazione di accesso ai dati nell'origine dati remota. RDS fornisce un programma server predefinito denominato oggetto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) . L'oggetto **RDSServer. DataFactory** restituisce al client qualsiasi oggetto **Recordset** prodotto da una query.  
  
 Tuttavia, **RDSServer. DataFactory** è limitato all'esecuzione di query e aggiornamenti. Non può eseguire alcuna convalida o elaborazione sulla connessione o sulle stringhe di comando.  
  
 Con ADO è possibile specificare che la **DataFactory** funzioni insieme a un altro tipo di programma server chiamato *gestore*. Il gestore può modificare la connessione client e le stringhe di comando prima che vengano utilizzate per accedere all'origine dati. Inoltre, il gestore può applicare i diritti di accesso, che regolano la capacità del client di leggere e scrivere i dati nell'origine dati.  
  
 I parametri utilizzati dal gestore per modificare i parametri client e i diritti di accesso vengono specificati nelle sezioni di un file di personalizzazione.  
  
 Negli argomenti seguenti vengono fornite ulteriori informazioni sulla personalizzazione dell'oggetto **DataFactory** .  
  
-   [Informazioni sul file di personalizzazione](./understanding-the-customization-file.md)  
  
-   [Sezione sulla connessione del file di personalizzazione](./customization-file-connect-section.md)  
  
-   [Sezione SQL del file di personalizzazione](./customization-file-sql-section.md)  
  
-   [Sezione UserList del file di personalizzazione](./customization-file-userlist-section.md)  
  
-   [Sezione Logs del file di personalizzazione](./customization-file-logs-section.md)  
  
-   [Impostazioni obbligatorie dei client](./required-client-settings.md)  
  
-   [Scrittura di un gestore personalizzato](./writing-your-own-customized-handler.md)