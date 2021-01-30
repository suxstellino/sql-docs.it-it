---
description: Tipi di dati dei campi Visual FoxPro
title: Tipi di dati dei campi di Visual FoxPro | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- field data types [ODBC]
- Visual FoxPro ODBC driver [ODBC], data types
ms.assetid: 50b733dc-679a-4b10-bc5d-98bb474dead2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2032ee25039364186f33486e4662b84c36c06947
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207474"
---
# <a name="visual-foxpro-field-data-types"></a>Tipi di dati dei campi Visual FoxPro
Nella tabella seguente sono elencati i valori per l'argomento *FieldType* in alter table e create table e viene indicato se sono necessari gli argomenti *nFieldWidth* e *nPrecision* .  
  
|*FieldType*|*NFieldWidth*|*nPrecision*|Descrizione|  
|-----------------|-------------------|------------------|-----------------|  
|B|-|d|Double|  
|C|N|-|Campo carattere della larghezza *n*|  
|D|-|-|Data|  
|F|N|d|Campo numerico a virgola mobile con larghezza *n* con *d* posizioni decimali|  
|G|-|-|Generale|  
|I|-|-|Integer|  
|L|-|-|Logico|  
|M|-|-|Memo|  
|N|N|d|Campo numerico della larghezza *n* con *d* posizioni decimali|  
|T|-|-|Datetime|  
|S|-|-|Valuta|
