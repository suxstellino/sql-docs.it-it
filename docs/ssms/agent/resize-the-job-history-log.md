---
description: Modificare le dimensioni del log di cronologia processi
title: Modificare le dimensioni del log di cronologia processi
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- resizing job history log
- size [SQL Server], job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: ddee1ce8-9d1b-4017-9894-bf7256aed95d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 61b6bef074842a297a46df9ba548602527924873
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339821"
---
# <a name="resize-the-job-history-log"></a>Modificare le dimensioni del log di cronologia processi

[!INCLUDE[applies-to-version/_ssnoversion.md](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Questo argomento descrive come impostare le dimensioni massime per i log di cronologia processo di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

- **Prima di iniziare:**  

    [Sicurezza](#Security)  

- **Per impostare le dimensioni massime dei log della cronologia dei processi utilizzando:**  

    [SQL Server Management Studio](#SSMS)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  

### <a name="security"></a><a name="Security"></a>Sicurezza

Per informazioni dettagliate, vedere [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  

## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilizzo di SQL Server Management Studio

*Per ridimensionare il log cronologia processi in base alle dimensioni dei dati non elaborati*

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]ed espandere tale istanza.

2. Fare clic con il pulsante destro del mouse su **SQL Server Agent** e quindi scegliere **Proprietà**.

3. Selezionare la pagina **Cronologia**, quindi verificare che l'opzione **Limita dimensioni log cronologia processi** sia selezionata.

4. Nella casella **Dimensioni massime log cronologia processi** immettere il numero massimo di righe consentito per il log cronologia processi.

5. Nella casella **Numero massimo di righe di cronologia per processo** immettere il numero massimo di righe di cronologia consentito per un processo.

**Per ridimensionare il log cronologia processi in base al tempo:**

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)], quindi espandere questa istanza.  

2. Fare clic con il pulsante destro del mouse su **SQL Server Agent** e quindi scegliere **Proprietà**.

3. Selezionare la pagina **Cronologia** e quindi selezionare **Rimuovi cronologia dell'agente**.

4. Selezionare il numero appropriato di **Giorni**, **Settimane** o **Mesi**.