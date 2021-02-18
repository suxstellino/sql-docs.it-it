---
title: Eseguire l'aggiornamento a un cluster di Big Data abilitato per la gestione della configurazione
titleSuffix: SQL Server big data clusters
description: Eseguire l'aggiornamento a un cluster di Big Data abilitato per la gestione della configurazione
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0a9f6addcf73e0c50d67f75f19fedd51b2f3dbc9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344017"
---
# <a name="upgrading-to-a-configuration-management-enabled-cluster-cu8-or-lower-to-cu9"></a>Aggiornamento a un cluster abilitato per la gestione della configurazione (CU8 o inferiore a CU9 +)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
A partire da CU9, i cluster di Big Data includono una funzionalità di gestione della configurazione che consente agli amministratori di modificare o ottimizzare varie parti del cluster di Big data post-distribuzione e ottenere informazioni più approfondite sulle configurazioni in esecuzione nel catalogo di integrazione applicativa dei dati. Prima di CU9, le configurazioni di cluster di Big data erano in genere modificabili solo in fase di distribuzione, con una soluzione alternativa per configurare alcune configurazioni SQL tramite un `mssql-custom.conf` file personalizzato. Questa soluzione alternativa è stata risolta e queste impostazioni sono configurabili tramite la funzionalità Gestione configurazione.

## <a name="migrating-sql-configurations-in-mssql-customconf-to-the-configuration-management-system"></a>Migrazione delle configurazioni SQL in MSSQL-Custom. conf al sistema di gestione della configurazione
Se è stato creato un personalizzato `mssql-custom.conf` per le istanze di SQL Server Master, seguire le istruzioni monouso seguenti per gestire le impostazioni tramite il sistema di configurazione e non con il file. Se non si esegue questa procedura, la funzionalità di gestione della configurazione non gestirà tali configurazioni SQL e le `mssql-custom.conf` Impostazioni eseguiranno l'override di tutte le modifiche apportate a tali impostazioni tramite la funzionalità di gestione della configurazione.

Passaggi:
1. Aggiornare il cluster Big data a CU9
> [!NOTE]
> Le impostazioni definite tramite `mssql-custom.conf` non verranno modificate o rimosse. Non verranno semplicemente riflesse e gestite dal framework di configurazione.

2. Impostare e applicare le impostazioni definite in precedenza in `mssql-custom.conf` utilizzando la nuova funzionalità di configurazione. Per una guida dettagliata per la modifica delle impostazioni, [SQL Server vedere Panoramica della configurazione post-distribuzione per i cluster di Big Data](configure-bdc-postdeployment.md) . Per un elenco completo delle impostazioni disponibili per ogni ambito, vedere [SQL Server proprietà di configurazione di Big Data Clusters](reference-config-bdc-overview.md) . Si noti che alcune impostazioni come customerFeedback possono avere un ambito modificato ma sono ancora disponibili.
3. Rinominare il `mssql-custom.conf` file `deprecated-mssql-custom.conf` in nel `mssql-server` contenitore in ogni pod master. Se è presente un solo master, `master-0` . Nel caso in cui sia necessario un downgrade o un rollback a un cluster non abilitato per la configurazione (CU8 o inferiore), questo file può essere riutilizzato per applicare queste configurazioni SQL personalizzate.

## <a name="downgrading-from-a-configuration-management-enabled-cluster-to-a-non-configuration-management-enabled-cluster-cu9-to-cu8-or-lower"></a>Downgrade da un cluster abilitato per la gestione della configurazione a un cluster non abilitato per la gestione della configurazione (CU9 + a CU8 o inferiore)
Il downgrade da un cluster abilitato per la gestione della configurazione (CU9 +) a un cluster non abilitato per la gestione della configurazione (CU8 o inferiore) eliminerà la possibilità di ottimizzare la post-distribuzione del cluster di Big Data. Sarà inoltre necessario utilizzare il `mssql-custom.conf` file facoltativo per impostare le configurazioni SQL. Se il file è stato rinominato in `deprecated-mssql-custom.conf` durante l'aggiornamento a CU9 +, rinominarlo in `mssql-custom.conf` . Se il file è stato eliminato o non è stato creato in precedenza e ora è necessario definire queste configurazioni speciali di SQL, crearlo seguendo le istruzioni riportate qui: [SQL Server proprietà di configurazione dell'istanza master-versione precedente di CU9](reference-config-master-instance.md). Tutte le impostazioni definite e modificate tramite l'esperienza di gestione della configurazione verranno ripristinate nelle configurazioni precedenti o nei valori predefiniti del sistema. 

Una volta effettuato il downgrade del cluster, verranno ripristinate le impostazioni predefinite o i valori specificati nel bdc.jsdi distribuzione. Non sono necessari altri passaggi dopo il downgrade.