---
title: Note sulla versione di SQL Server 2019 | Microsoft Docs
description: Informazioni sulle limitazioni di SQL Server 2019 (15.x), sui problemi noti, sulle risorse della Guida e su altre note sulla versione.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-ver15
ms.openlocfilehash: e8a9f06dc33acab6862027cd2fdb55bb4aca4dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080432"
---
# <a name="sql-server-2019-release-notes"></a>Note sulla versione di [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]
[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo descrive le limitazioni e i problemi noti relativi a [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]. Per informazioni correlate, vedere:

> [Novità di [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]](../sql-server/what-s-new-in-sql-server-ver15.md)

## [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] è la versione pubblica più recente di [!INCLUDE[SQL Server 2019](../includes/ssnoversion-md.md)].

Informazioni dettagliate sulle licenze sono disponibili nella cartella `License Terms` del supporto di installazione.

## <a name="documentation"></a>Documentazione

- **Problema e impatto per i clienti**: la documentazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] può essere filtrata in base alla versione. Usare il controllo nell'angolo superiore sinistro di ogni pagina della documentazione per filtrare in base ai requisiti.

## <a name="build-number"></a>Numero di build

Il numero della build RTM per SQL Server 2019 è `15.0.2000.5`.

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="sql-server-installation-may-fail-if-ssms-18x-is-installed"></a>L'installazione di SQL Server potrebbe non riuscire se è installato SSMS 18.x

- **Problema e impatto per i clienti**: l'installazione di [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] non viene eseguita quando le installazioni seguenti vengono eseguite in questo ordine:
  1. Nel server è installato SQL Server Management Studio (SSMS) versione 18.0, 18.1, 18.2 o 18.3.
  1. Si è tentato di installare [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] da un supporto rimovibile. Ad esempio, il supporto di installazione è un DVD.

- **Soluzione alternativa**:
  1. Disinstallare tutte le versioni di SSMS precedenti a SSMS 18.3.1.
  1. Installare una versione più recente di SSMS (18.3.1 o versione successiva). Per la versione più recente, vedere [Scaricare SSMS](../ssms/download-sql-server-management-studio-ssms.md).
  1. Installare [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] normalmente.

- **Si applica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]

## <a name="utf-8-collations"></a>Regole di confronto UTF-8

- **Problema e impatto per i clienti**: le regole di confronto che supportano UTF-8 non possono essere usate con le funzionalità seguenti.
  - [Tabelle OLTP in memoria](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)
  - [Always Encrypted con enclave sicuri](../relational-databases/security/encryption/always-encrypted-enclaves.md) (se non si usano enclave sicuri, [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) può usare UTF-8)

  > [!WARNING]
  > La creazione di un file [bacpac](../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) di un database contenente colonne di tabella definite come [CHAR o VARCHAR](../t-sql/data-types/char-and-varchar-transact-sql.md) con più di 4000 byte avrà esito negativo.
  
  > [!NOTE]
  > Attualmente non è disponibile il supporto dell'interfaccia utente per la scelta di regole di confronto con supporto UTF-8 in Azure Data Studio o SQL Server Data Tools (SSDT). [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) versione 18 supporta la scelta di regole di confronto abilitate per UTF-8 nell'interfaccia utente.

- **Si applica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="master-data-service-notification-email-contains-broken-link"></a>Il messaggio di posta elettronica di notifica di Master Data Services contiene un collegamento interrotto

- **Problema e impatto per i clienti**: Il messaggio di posta elettronica di notifica da Master Data Services (MDS) contiene un collegamento interrotto. Il collegamento passa a una pagina che restituisce un errore simile al messaggio seguente:

   `The view 'Index' or its master was not found or no view engine supports the searched locations.`

- **Soluzione alternativa**: Aprire il portale di MDS e passare alla risorsa manualmente.

- **Si applica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="see-also"></a>Vedere anche

- [Requisiti hardware e software per l'installazione di SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

## <a name="machine-learning-services"></a>Machine Learning Services

Per problemi in SQL Server Machine Learning Services, vedere [Problemi noti in SQL Server Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md).

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]
