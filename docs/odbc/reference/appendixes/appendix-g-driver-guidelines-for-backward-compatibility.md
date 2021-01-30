---
description: 'Appendice G: Indicazioni per la compatibilità con le versioni precedenti dei driver'
title: 'Appendice G: linee guida sui driver per la compatibilità con le versioni precedenti | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC drivers [ODBC], backward compatibility
- backward compatibility [ODBC], drivers
- compatibility [ODBC], drivers
ms.assetid: 911cd335-f2c0-4d03-9739-1078308a678a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 337640ec943833dc333483fe2256fb8204c92e9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212517"
---
# <a name="appendix-g-driver-guidelines-for-backward-compatibility"></a>Appendice G: Indicazioni per la compatibilità con le versioni precedenti dei driver
In questa appendice vengono fornite informazioni per i writer di driver che lavorano su ODBC 3. driver *x* che devono supportare ODBC 2. applicazioni *x* . Per ulteriori informazioni sulla compatibilità con le versioni precedenti, vedere [compatibilità con le versioni precedenti e conformità agli standard](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Cursori a blocchi, cursori scorrevoli e compatibilità con le versioni precedenti per i driver ODBC 3. x](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) : le nuove funzionalità sono disponibili in ODBC 3. *x* e non in ODBC 2. *x*. ODBC 3. i driver *x* in genere non devono preoccuparsi della compatibilità con le nuove funzionalità, perché ODBC 2. le applicazioni *x* non le usano mai. Le uniche eccezioni sono le funzionalità correlate a **SQLFetch**, **SQLFetchScroll**, **SQLSetPos** e **SQLExtendedFetch**; Per ulteriori informazioni, vedere, più avanti in questa appendice.  
  
-   [Mapping di funzioni deprecate](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) : le funzionalità duplicate sono funzionalità implementate in modo diverso in ODBC 3. *x* e ODBC 2. *x*. ODBC 3. i driver *x* non devono preoccuparsi della compatibilità con le versioni precedenti con le funzionalità duplicate, perché Gestione driver esegue sempre il mapping di ODBC 2. funzionalità *x* per ODBC 3. funzionalità *x* quando si chiama ODBC 3. driver *x* . Quindi, ODBC 3. il driver *x* vede solo ODBC 3. funzionalità *x* . Per ulteriori informazioni su questi mapping, vedere, più avanti in questa appendice.  
  
-   [Modifiche comportamentali e driver ODBC 3. x](../../../odbc/reference/appendixes/behavioral-changes-and-odbc-3-x-drivers.md) : le modifiche del comportamento sono funzionalità gestite in modo diverso in ODBC 3. *x* e ODBC 2. *x*. ODBC 3. i driver *x* devono preoccuparsi delle modifiche del comportamento e agiscono in risposta all'attributo dell'ambiente SQL_ATTR_ODBC_VERSION impostato dall'applicazione.
