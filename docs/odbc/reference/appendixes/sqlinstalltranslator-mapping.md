---
description: Mapping di SQLInstallTranslator
title: Mapping di SQLInstallTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLInstallTranslator function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLInstallTranslator
ms.assetid: bcd9ba4f-7834-4bc4-876e-c7478998e524
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b063768fed57adffd4a6e5051e5383a2ad32a943
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202705"
---
# <a name="sqlinstalltranslator-mapping"></a>Mapping di SQLInstallTranslator
Quando un'applicazione ODBC *2. x* chiama **SQLInstallTranslator** tramite un driver ODBC *3. x* , gestione driver esegue il mapping della chiamata a **SQLInstallTranslatorEx**. Un'applicazione non deve chiamare **SQLInstallTranslator** in Gestione driver ODBC *3. x* con l'argomento *lpszInfFile* impostato su un valore diverso da null. ODBC. Il file INF utilizzato in ODBC *2. x* non è più supportato in ODBC *3. x*, anche per compatibilità con le versioni precedenti.
