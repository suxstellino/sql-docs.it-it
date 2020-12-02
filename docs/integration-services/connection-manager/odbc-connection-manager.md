---
description: gestione connessione ODBC
title: Gestione connessione ODBC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.odbcconnection.f1
helpviewer_keywords:
- connections [Integration Services], ODBC
- ODBC connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], ODBC
ms.assetid: e8c77aa7-6772-485e-918e-cef9eeb18c58
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7fe63ad503b49a760823635902b3744a1ae2d85a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "91719545"
---
# <a name="odbc-connection-manager"></a>gestione connessione ODBC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Una gestione connessione ODBC consente la connessione di un pacchetto a un'ampia gamma di sistemi di gestione di database in base alla specifica ODBC (Open Database Connectivity).  
  
 Quando si aggiunge una connessione ODBC a un pacchetto e si impostano le proprietà della gestione connessione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea una gestione connessione e la aggiunge alla raccolta **Connections** del pacchetto. In fase di esecuzione la gestione connessione viene risolta in una connessione ODBC fisica.  
  
 La proprietà **ConnectionManagerType** della gestione connessione viene impostata su **ODBC**.  
  
 Per configurare la gestione connessione ODBC, procedere nel modo seguente:  
  
-   Specificare una stringa di connessione che fa riferimento al nome di un'origine dei dati utente o di sistema.  
  
-   Specificare il server a cui connettersi.  
  
-   Indicare se la connessione viene mantenuta in fase di esecuzione.  
  
## <a name="configuration-of-the-odbc-connection-manager"></a>Configurazione della gestione connessione ODBC  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per altre informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic su uno degli argomenti seguenti:  
  
-   [Riferimento all'interfaccia utente di Gestione connessione ODBC]()  
  
 Per informazioni sulla configurazione di una gestione connessione a livello di programmazione, vedere l'articolo relativo a <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> e [Aggiunta di connessioni a livello di programmazione](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="odbc-connection-manager-ui-reference"></a>Riferimento all'interfaccia utente di Gestione connessione ODBC
  Utilizzare la finestra di dialogo **Configura gestione connessione ODBC** per aggiungere una connessione a un'origine dei dati ODBC.  
  
 Per ulteriori informazioni sulla gestione connessione ODBC, vedere [ODBC Connection Manager](../../integration-services/connection-manager/odbc-connection-manager.md).  
  
### <a name="options"></a>Opzioni  
 **Connessioni dati**  
 Consente di selezionare una gestione connessione ODBC esistente nell'elenco.  
  
 **Proprietà connessione dati**  
 Consente di visualizzare proprietà e valori per la gestione connessione ODBC selezionata.  
  
 **Nuove**  
 Consente di creare una gestione connessione ODBC tramite la finestra di dialogo **Gestione connessione** . In questa finestra di dialogo è inoltre possibile creare una nuova origine dei dati ODBC, se necessario.  
  
 **Elimina**  
 Selezionare una connessione e quindi eliminarla utilizzando il pulsante **Elimina** .  
## <a name="see-also"></a>Vedere anche  
 [Connessioni in Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
