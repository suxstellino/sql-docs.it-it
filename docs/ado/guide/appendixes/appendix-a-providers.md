---
description: 'Appendice A: provider di dati e servizi'
title: 'Appendice A: Providers | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- providers [ADO]
- ADO, providers
- service providers [ADO]
- service components [ADO]
ms.assetid: e2581b47-b11e-4e1e-b96c-d39c77c5b48a
author: rothja
ms.author: jroth
ms.openlocfilehash: a49518c1fcc1618884751d503def46a29dd9c048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029485"
---
# <a name="appendix-a-data-and-service-providers"></a>Appendice A: provider di dati e servizi
Questa sezione illustra tre tipi di provider: provider di dati, provider di servizi e componenti del servizio. I provider rientrano in due categorie, ovvero quelle che forniscono i dati e i servizi. Un *provider di dati* possiede i propri dati e li espone in forma tabulare all'applicazione. Un *provider di servizi* incapsula un servizio tramite la produzione e l'utilizzo di dati, aumentando le funzionalità nelle applicazioni ADO. Un provider di servizi può anche essere definito ulteriormente come *componente del servizio*, che deve interagire con altri provider di servizi o componenti.

## <a name="data-providers"></a>Provider di dati
 ADO è potente e flessibile perché può connettersi a uno dei diversi provider di dati e comunque esporre lo stesso modello di programmazione, indipendentemente dalle funzionalità specifiche di un determinato provider.

 Tuttavia, poiché ogni provider di dati è univoco, il modo in cui l'applicazione interagisce con ADO può variare leggermente in base al provider di dati. Le differenze in genere rientrano in una delle tre categorie seguenti:

-   Parametri di connessione nella proprietà [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) .

-   Utilizzo oggetto [comando](../../reference/ado-api/command-object-ado.md) .

-   Comportamento del [Recordset](../../reference/ado-api/recordset-object-ado.md) specifico del provider.

 I dettagli per ogni provider di dati attualmente disponibile in Microsoft sono elencati di seguito.

|Area|Argomento|
|----------|-----------|
|Database ODBC|[Provider Microsoft OLE DB per ODBC](./microsoft-ole-db-provider-for-odbc.md)|
|Servizio Microsoft Indexing|[Provider OLE DB per il Servizio di indicizzazione Microsoft](./microsoft-ole-db-provider-for-microsoft-indexing-service.md)|
|Servizio Active Directory|[Provider Microsoft OLE DB per Microsoft Active Directory Service](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md)|
|Database Microsoft Jet|[Provider di OLE DB per Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md)|
|Microsoft SQL Server|[Provider Microsoft OLE DB per SQL Server](./microsoft-ole-db-provider-for-sql-server.md)|
|database Oracle|[Provider OLE DB per Oracle](./microsoft-ole-db-provider-for-oracle.md)|
|Pubblicazione su Internet|[Provider Microsoft OLE DB per Internet Publishing](./microsoft-ole-db-provider-for-internet-publishing.md)|
|Origini dati semplici|[Provider semplice Microsoft OLE DB](./microsoft-ole-db-simple-provider.md)|

## <a name="provider-specific-dynamic-properties"></a>Proprietà dinamiche Provider-Specific
 Le raccolte [Properties](../../reference/ado-api/properties-collection-ado.md) degli oggetti [Connection](../../reference/ado-api/connection-object-ado.md), [Command](../../reference/ado-api/command-object-ado.md)e [Recordset](../../reference/ado-api/recordset-object-ado.md) includono proprietà dinamiche specifiche del provider. Queste proprietà forniscono informazioni sulle funzionalità specifiche del provider oltre alle proprietà predefinite supportate da ADO.

 Dopo aver stabilito la connessione e aver creato questi oggetti, utilizzare il metodo [Refresh](../../reference/ado-api/refresh-method-ado.md) sulla raccolta **Properties** dell'oggetto per ottenere le proprietà specifiche del provider. Per informazioni dettagliate su queste proprietà dinamiche, vedere la documentazione del provider e la [Guida per programmatori OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) .

## <a name="service-providers"></a>Provider di servizi
 Per usare un provider di servizi, è necessario specificare una parola chiave. È inoltre necessario conoscere le proprietà dinamiche specifiche del provider associate a ogni provider di servizi. I dettagli specifici del provider sono elencati per ogni provider di servizi attualmente disponibile da Microsoft:

-   [Servizio Microsoft di modifica della forma dei dati per OLE DB](./microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)

-   [Provider di persistenza Microsoft OLE DB](./microsoft-ole-db-persistence-provider-ado-service-provider.md)

-   [Provider Microsoft OLE DB Remoting](./microsoft-ole-db-remoting-provider-ado-service-provider.md)

## <a name="service-components"></a>Componenti del servizio
 Il [servizio cursore per OLE DB](./microsoft-cursor-service-for-ole-db-ado-service-component.md) componente del servizio integra le funzioni di supporto del cursore dei provider di dati. Richiede anche una parola chiave e dispone di proprietà dinamiche.

 Per ulteriori informazioni sui provider di OLE DB, vedere [Microsoft OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85)).

## <a name="provider-commands"></a>Comandi del provider
 Per ogni provider elencato qui, se le applicazioni consentono agli utenti di immettere istruzioni SQL come comandi del provider, è sempre necessario convalidare l'input dell'utente e vigilare su possibili attacchi di hacker usando istruzioni SQL potenzialmente pericolose, ad esempio `DROP TABLE t1` , come parte dell'input utente.

## <a name="see-also"></a>Vedere anche
 Oggetto [comando (ADO)](../../reference/ado-api/command-object-ado.md) [Connection Object (ADO)](../../reference/ado-api/connection-object-ado.md) [provider Microsoft OLE DB per Internet pubblicazione](./microsoft-ole-db-provider-for-internet-publishing.md) [del provider Microsoft OLE DB per Microsoft Active Directory Service](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md) provider [microsoft OLE DB per Microsoft indexing Service](./microsoft-ole-db-provider-for-microsoft-indexing-service.md) provider [Microsoft OLE DB provider per ODBC](./microsoft-ole-db-provider-for-odbc.md) [provider Microsoft OLE DB per Oracle](./microsoft-ole-db-provider-for-oracle.md) [provider Microsoft OLE DB per SQL Server](./microsoft-ole-db-provider-for-sql-server.md) provider [Microsoft OLE DB per Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md) [Properties Collection (ADO)](../../reference/ado-api/properties-collection-ado.md) [Recordset Object (ADO](../../reference/ado-api/recordset-object-ado.md) ) metodo di [aggiornamento (RDS](../../reference/rds-api/refresh-method-rds.md) )