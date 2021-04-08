---
title: Logica di ripetizione dei tentativi configurabile in SqlClient
description: Informazioni su come usare la funzionalità di logica di ripetizione dei tentativi configurabile di Microsoft. Data. SqlClient quando si stabilisce una connessione o si esegue un comando.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: aa0b6a5d984b2891f2fcf868b15a495b4984b8ba
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004120"
---
# <a name="configurable-retry-logic-in-sqlclient"></a>Logica di ripetizione dei tentativi configurabile in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Un'applicazione che comunica con gli elementi in esecuzione nel cloud deve essere in grado di rilevare gli errori temporanei che possono verificarsi in questo ambiente. Questi errori sono in genere corretti in modo automatico. Se l'azione che ha generato un errore viene ripetuta dopo un intervallo di tempo appropriato, è probabile che abbia esito positivo.

> [!NOTE]
> Questa funzionalità è disponibile a partire da Microsoft. Data. SqlClient versione 3.0.0 Preview 1.

## <a name="retry-pattern"></a>Modello di ripetizione dei tentativi

Il tentativo di completare un'operazione nonostante errori temporanei, anziché generare un'eccezione e consentire a un utente di decidere l'azione successiva, è una decisione intelligente denominata modello di ripetizione dei tentativi. Per altre informazioni, vedere [modello di ripetizione dei tentativi](/azure/architecture/patterns/retry).

## <a name="transient-faults"></a>Errori temporanei

È possibile disporre di un'infrastruttura affidabile e utilizzare applicazioni note implementate con le tecnologie più recenti per ridurre i tempi di inattività del servizio. Tuttavia, non è possibile ridurre gli errori a zero. Gli errori temporanei sono quelli che talvolta si verificano per motivi noti e scompariranno dopo un breve periodo di tempo. Ad esempio, quando è in corso una modifica del bilanciamento del carico sul lato server, è possibile che si verifichi un errore o il timeout dei servizi richiesti. Per ulteriori informazioni, vedere [errori temporanei](/azure/azure-sql/database/troubleshoot-common-connectivity-issues#transient-errors-transient-faults).

## <a name="do-and-dont"></a>Do and not

Anche se l'uso di un modello di ripetizione dei tentativi migliora significativamente la resilienza di un'applicazione, può influire negativamente su un'applicazione se utilizzata in circostanze errate. Prima di aggiungere un'eccezione all'elenco degli errori temporanei, sospendere per un istante e chiedersi "si risolverà a breve?". Non correre. Se non si ha una risposta adeguata per la domanda, studiare i motivi. Per altre informazioni, vedere [risoluzione dei problemi di connettività e altri errori con il database SQL di Azure e Azure sql istanza gestita](/azure/azure-sql/database/troubleshoot-common-errors-issues).

## <a name="in-this-section"></a>Contenuto della sezione

[Logica di ripetizione dei tentativi configurabile in SqlClient introduttiva](configurable-retry-logic-sqlclient-introduction.md)  
Introduce una sezione diversa della logica di ripetizione dei tentativi configurabile.

[Provider di logica di ripetizione dei tentativi interni in SqlClient](internal-retry-logic-providers-sqlclient.md)  
Viene illustrato come utilizzare i provider di tentativi predefiniti per applicare la logica di ripetizione dei tentativi nel database.

[API di base della logica di ripetizione dei tentativi configurabili in SqlClient](configurable-retry-logic-core-apis-sqlclient.md)  
Viene illustrato come utilizzare le API di base per implementare la logica di ripetizione dei tentativi personalizzata.

[File di configurazione della logica di ripetizione dei tentativi configurabile con SqlClient](configurable-retry-logic-config-file-sqlclient.md)  
Viene illustrato come specificare i provider di logica di ripetizione dei tentativi predefiniti tramite un file di configurazione.

## <a name="see-also"></a>Vedi anche

- [Modello di ripetizione dei tentativi](/azure/architecture/patterns/retry)
- [Errori temporanei](/azure/azure-sql/database/troubleshoot-common-connectivity-issues#transient-errors-transient-faults)
- [Risoluzione dei problemi di connettività e di altri errori con il database SQL di Azure e Azure SQL Istanza gestita](/azure/azure-sql/database/troubleshoot-common-errors-issues)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
