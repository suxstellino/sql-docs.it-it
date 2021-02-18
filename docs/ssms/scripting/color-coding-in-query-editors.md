---
title: Codifica con colori negli editor di query
description: Informazioni su come vengono codificate a colori le categorie di testo per facilitare la ricerca di testo specifico e su come configurare una combinazione di colori personalizzata.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], color coding
- color coding [Query Editor]
ms.assetid: 802882dc-c997-4e3f-8a01-994bb43169ae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96fe150cd54cfd37074949a278ed40570dd18e83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354755"
---
# <a name="color-coding-in-query-editors"></a>Codifica con colori negli editor di query

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Al testo immesso negli editor del codice viene assegnata una categoria. Ogni categoria viene identificata con un colore. I colore consentono di individuare rapidamente il testo nel codice. I commenti ad esempio vengono evidenziati con il colore verde scuro. Nella tabella seguente sono riportati i colori più comuni. È possibile visualizzare l'intero elenco dei colori e le relative categorie e configurare uno schema di colori personalizzato dal menu **Strumenti**, **Opzioni** . Per altre informazioni su come modificare i colori predefiniti, vedere [Modificare lo stile, le dimensioni e il colore del carattere](./change-font-color-size-and-style.md).  
  
## <a name="default-code-colors"></a>Colori del codice predefiniti  
  
|Colore|Category|  
|-----------|--------------|  
|Rosso|Stringa SQL|  
|Verde scuro|Comment|  
|Nero su sfondo argento|Comando SQLCMD|  
|Fucsia|Funzioni di sistema|  
|Green|Tabella di sistema o funzione con valori di tabella, nonché gli schemi di sistema sys e INFORMATION_SCHEMA.|  
|Blu|Parola chiave|  
|Verde acqua|Numeri di riga o parametro modello|  
|Bordeaux|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stored procedure|  
|Grigio scuro|Operatori|  
  
## <a name="status-bar"></a>Barra di stato  
 È possibile configurare server registrati o server [!INCLUDE[ssDE](../../includes/ssde-md.md)] in Esplora oggetti per avere colori diversi nella barra di stato dell'Editor di query [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Ciò consente di identificare a quale server è connessa ogni finestra dell'editor quando si dispone di molte finestre aperte contemporaneamente. Per altre informazioni sull'impostazione dei colori della barra di stato, vedere [Barra di stato &#40;editor di query del motore di database&#41;](./status-bar-database-engine-query-editor.md).  
  
 In alcuni tipi di editor la barra di stato non viene visualizzata oppure non supporta più colori.  
  
