---
description: Effetti delle opzioni ISO
title: Effetti delle opzioni ISO | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ISO options (ODBC)
- ODBC applications, ISO options
- ODBC applications, statements
- SQL Server Native Client ODBC driver, ISO options
- statements [ODBC], ISO options
ms.assetid: 813f1397-fa0b-45ec-a718-e13fe2fb88ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d98b0a8256008bee508b61a3ade7360da3d43af
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751321"
---
# <a name="effects-of-iso-options"></a>Effetti delle opzioni ISO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lo standard ODBC è molto simile allo standard ISO e le applicazioni ODBC presuppongono un comportamento standard da un driver ODBC. Per garantire la conformità del comportamento con quello definito nello standard ODBC, il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client utilizza sempre le opzioni ISO disponibili nella versione di SQL Server a cui si connette.  
  
 Quando il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client si connette a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , il server rileva che il client utilizza il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client e imposta diverse opzioni in.  
  
 Il driver stesso genera queste istruzioni, senza alcuna richiesta da parte dell'applicazione ODBC. L'impostazione di queste opzioni rende più portabili le applicazioni ODBC che utilizzano il driver in quanto il comportamento del server corrisponde allo standard ISO.  
  
 Nelle applicazioni DB-Library in genere queste opzioni non vengono attivate. I siti che osservano un comportamento diverso tra i client ODBC o DB-Library quando si esegue la stessa istruzione SQL non devono presupporre che questo indichi un problema con il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client. È necessario eseguire prima di tutto l'istruzione nell'ambiente DB-Library con le stesse opzioni SET utilizzate dal [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client.  
  
 Poiché le opzioni SET possono essere attivate e disattivate in qualsiasi momento da utenti e applicazioni, gli sviluppatori di stored procedure e trigger devono testare anche le stored procedure e i trigger con le opzioni SET sopra indicate attivate e disattivate per garantirne il corretto funzionamento indipendentemente dalle opzioni impostate da una connessione specifica quando il trigger o la stored procedure viene richiamata. Un trigger o una stored procedure in cui è necessario impostare una di queste opzioni in modo specifico deve generare un'istruzione SET all'inizio del trigger o della stored procedure stessa. Questa istruzione SET rimarrà attiva soltanto durante l'esecuzione del trigger o della stored procedure. Al completamento della stessa, verrà ripristinata l'impostazione originale.  
  
 Durante la connessione a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], viene attivata anche una quarta opzione SET, CONCAT_NULL_YIELDS_NULL. Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client non imposta queste opzioni su se AnsiNPW = NO è specificato nell'origine dati o in [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md) o [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md).  
  
 Analogamente alle opzioni ISO indicate in precedenza, il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client non attiva l'opzione QUOTED_IDENTIFIER in se QuotedID = No è specificato nell'origine dati o in **SQLDriverConnect** o **SQLBrowseConnect**.  
  
 Per consentire al driver di conoscere lo stato corrente delle opzioni SET, le applicazioni ODBC non devono utilizzare l'istruzione SET [!INCLUDE[tsql](../../../includes/tsql-md.md)] per impostarle. Per eseguire questa operazione, infatti, devono utilizzare solo le opzioni dell'origine dati o della connessione. Se l'applicazione genera istruzioni SET, il driver può generare istruzioni SQL errate.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di istruzioni &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)   
 [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)   
 [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
  
