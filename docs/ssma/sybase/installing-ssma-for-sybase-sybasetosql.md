---
title: Installazione di SSMA per SAP ASE (SybaseToSQL) | Microsoft Docs
description: Usare questi articoli per installare, aggiornare e disinstallare SQL Server Migration Assistant per SAP ASE, che include un'applicazione client e un pacchetto di estensione.
ms.custom: ''
ms.date: 11/29/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8d5a4ce6-b747-46e3-9184-645d56e8b35c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8fca8219dfe89c8229b8d296fe355b97417d9daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073365"
---
# <a name="installing-ssma-for-sap-ase-sybasetosql"></a>Installazione di SSMA per SAP ASE (SybaseToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per SAP Adaptive Server Enterprise (ASE) è costituito da un'applicazione client usata per eseguire una migrazione da SAP ASE a o da un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure. Contiene anche un pacchetto di estensione che supporta la migrazione dei dati e l'uso delle funzioni di sistema dell'ambiente del servizio app nei database migrati.  
  
Installare l'applicazione client nel computer da cui si prevede di eseguire i passaggi di migrazione. Installare i file del pacchetto di estensione nel computer in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui devono essere ospitati i database migrati.  
  
## <a name="upgrading-ssma-for-sap-ase"></a>Aggiornamento di SSMA per SAP ASE  
Se si vuole eseguire l'aggiornamento a una versione successiva di SSMA per SAP ASE, è necessario prima disinstallare il client e il pacchetto di estensione server. Installare quindi la versione più recente.  
  
Se si apre un progetto da una versione precedente di SSMA per SAP ASE, SSMA chiede se si vuole convertire il progetto nella versione più recente. Fare clic su **Sì** per usare il progetto nella versione più recente di SSMA.  
  
## <a name="contents"></a>Contenuto  
  
|Articolo|Descrizione|  
|---------|---------------|  
|[Installazione di SSMA per il client SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)|Fornisce informazioni e istruzioni per l'installazione di SSMA per il client SAP ASE.|  
|[Installazione dei componenti di SSMA in SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)|Vengono fornite informazioni e istruzioni per l'installazione del pacchetto di estensione nelle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Rimozione di SSMA per i componenti di SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/removing-ssma-for-sybase-components-sybasetosql.md)|Vengono fornite istruzioni per la disinstallazione del programma client e del pacchetto di estensione.|  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database SAP ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
