---
title: Pagina opzioni SQL Server-servizi di Azure
description: Opzioni (servizi di Azure)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Azure_Services.Azure_Cloud
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/15/2021
ms.openlocfilehash: 0983317d1d5c59e486764532708c566bb740000a
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2021
ms.locfileid: "98245704"
---
# <a name="options-azure-services"></a>Opzioni (servizi di Azure)

Usare questa pagina per specificare le opzioni correlate ai servizi cloud di Azure. Per accedere a questa finestra di dialogo, passare a **strumenti > opzioni > servizi di Azure** dalla barra dei menu superiore.

## <a name="miscellaneous"></a>Varie

| Opzione | Informazioni | Descrizione |
|--------|-------------|-------------|
| ADAL Finestra di output livello traccia | **Informazioni** <br> **Nessuno** <br> **Verbose** <br> **Avviso** | Livello di traccia dell'account di accesso Azure Active Directory (AAD) da inviare alla finestra di output. |
| URL del portale di Azure Data Factory | `https://adf.azure.com` | Specifica l'URL per il portale di Azure Data Factory. |
| Abilitare l'accesso condizionale al database SQL di Azure (SPERIMENTAle) | **True** <br> **False** | SPERIMENTAle: Se true, richiede un token di accesso per il database SQL di Azure che include un'attestazione per l'accesso al server selezionato. Per rendere effettive le impostazioni, potrebbe essere necessario riavviare SSMS. |
| Endpoint della raccolta | `https://gallery.azure.com` | Specifica l'endpoint per la raccolta Gestione risorse di modelli di distribuzione. |
| Destinatari del grafo | `https://graph.windows.net` | ID risorsa dell'endpoint Graph. |
| Endpoint grafo | `https://graph.windows.net` | Specifica l'URL per le richieste di Azure Active Directory Graph. |
| URL portale di gestione | `https://portal.azure.com` | Specifica l'URL per il portale di gestione. |
| URL del file di impostazioni di pubblicazione | `https://go.microsoft.com/fwlink/?LinkID=335839` | Specifica l'URL da cui `.publishsettings` è possibile scaricare il file. |
| Nome dell'entità servizio del database SQL | `https://database.windows.net/` | SPN del database SQL di Azure per ottenere un token quando si usa l'autenticazione AAD. Inoltre, i destinatari del token JSON Web (JWT) per l'analisi o la convalida del token Web JSON sul lato server (JWT). |

## <a name="resource-management"></a>Gestione delle risorse

| Opzione | Informazioni | Descrizione |
|--------|-------------|-------------|
| Autorità di Active Directory | `https://login.microsoftonline.com` | Specifica l'autorità di base per l'autenticazione Azure Active Directory (AAD). |
| ID risorsa endpoint servizio Active Directory | `https://management.core.windows.net` | Specifica i destinatari per i token che autenticano le richieste agli endpoint di Azure Resource Manager (ARM) o di gestione dei servizi (RDFE). |
| Endpoint Gestione risorse | `https://management.azure.com` | Specifica l'URL per le richieste di gestione delle risorse. |
| Endpoint servizio | `https://management.core.windows.net` | Specifica l'endpoint per le richieste di gestione dei servizi. |

## <a name="select-an-azure-cloud"></a>Selezionare un cloud di Azure

| Opzione | Informazioni | Descrizione |
|--------|-------------|-------------|
| Nome | **Cloud di Azure Cina** <br><br> **Cloud di Azure** <br><br> **Cloud di Azure per la Germania** <br><br> **Azure US Gov** <br><br> **Impostazione personalizzata** | Selezionare il cloud di Azure a cui Management Studio si connette per individuare e gestire le risorse. Selezionare **personalizzata** per specificare URL e suffissi DNS personalizzati. |

## <a name="service-addresses"></a>Indirizzi del servizio

| Opzione | Informazioni | Descrizione |
|--------|-------------|-------------|
| Endpoint dei servizi Azure Data Lake | `azuredatalakeanalytics.net` | Specifica il suffisso dell'endpoint del processo e del catalogo Azure Data Lake Analytics. |
| Azure Data Lake Store endpoint del file System | `azuredatalakestore.net` | Specifica il suffisso dell'endpoint di sistema dei file Azure Data Lake Store. | 
| Destinatari Azure Key Vault | `https://vault.azure.net` | Specifica i destinatari per i token di accesso che autorizzano le richieste di Key Vault servizi. |
| Suffisso DNS Azure Key Vault | `vault.azure.net` | Specifica il suffisso del nome di dominio per i server Azure Key Vault. |
| Suffisso DNS del database SQL | `database.windows.net` | Specifica il suffisso del nome di dominio per i server di database SQL di Azure. |
| Endpoint di archiviazione | `core.windows.net` | Specifica l'endpoint per l'accesso all'archiviazione. L'opzione include i BLOB, le tabelle, le code e le archiviazioni di file. |
| Suffisso DNS di gestione traffico | `trafficmanager.net` | Specifica il suffisso del nome di dominio per i servizi di gestione traffico di Azure. |