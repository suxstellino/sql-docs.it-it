---
description: sys.external_language_files (Transact-SQL)-SQL Server
title: sys.external_language_files (Transact-SQL)-SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- external_languages
- external_languages_TSQL
- sys.external_languages
- sys.external_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_languages catalog view
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 8fe1a38d24306d697dd0ebfbd9dbf79502ab94c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061612"
---
# <a name="sysexternal_language_files-transact-sql"></a>sys.external_language_files (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Questa vista del catalogo fornisce un elenco dei file di estensione del linguaggio esterno nel database. **R** e **Python** sono nomi riservati e nessun linguaggio esterno può essere creato con tali nomi specifici.

Quando viene creata una lingua esterna da un file_spec, in questa visualizzazione viene elencata l'estensione stessa e le relative proprietà. Questa vista conterrà una voce per ogni lingua, per sistema operativo.

## <a name="sysexternal_languages"></a>sys.external_languages

Nella vista del catalogo sys.external_language_files viene elencata una riga per ogni estensione del linguaggio esterno nel database. Parametri

|Nome colonna |Tipo di dati | Descrizione|
|------|------|------|
|external_language_id |INT | ID della lingua esterna|
|contenuto|varbinary(max) |Contenuto del file di estensione della lingua esterna|
|file_name|nvarchar (266)|Nome del file di estensione del linguaggio|
|Piattaforma|TINYINT|ID della piattaforma host in cui è installato SQL Server|
|platform_desc |nvarchar(60)|Nome della piattaforma host. I valori validi sono ' WINDOWS ',' LINUX '.|
|parametri|nvarchar(4000)|Prameters lingua esterna|
|environment_variables |nvarchar(4000)|Variabili di ambiente della lingua esterna|

## <a name="see-also"></a>Vedi anche  

+ [sys.external_languages](sys-external-languages-transact-sql.md)  
+ [CREA LINGUA ESTERNA](../../t-sql/statements/create-external-language-transact-sql.md)  
