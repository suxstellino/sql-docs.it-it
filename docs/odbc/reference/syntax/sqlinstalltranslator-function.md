---
description: Funzione SQLInstallTranslator
title: Funzione SQLInstallTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslator
helpviewer_keywords:
- SQLInstallTranslator function [ODBC]
ms.assetid: 453b21ff-3c2b-4069-8ff7-5c727f062d89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40f4c4a5df497c64d081a8d03d1765a4fe5e060a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189662"
---
# <a name="sqlinstalltranslator-function"></a>Funzione SQLInstallTranslator
**Conformità**  
 Versione introdotta: ODBC 2,5, deprecato  
  
 **Summary**  
 In ODBC 3,0, **SQLInstallTranslator** è stato sostituito da [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md). Le chiamate a **SQLInstallTranslator** verranno mappate a **SQLInstallTranslatorEx**. Per ulteriori informazioni, vedere **SQLInstallTranslatorEx**.  
  
 **SQLInstallTranslator** restituirà false se un'applicazione la chiama in Gestione driver ODBC *3. x* con l'argomento *lpszInfFile* impostato su un valore diverso da null. Il file ODBC. inf utilizzato in ODBC *2. x* non è più supportato in ODBC *3. x*, anche per compatibilità con le versioni precedenti.
