---
description: Mapping di SQLSetConnectOption
title: Mapping di SQLSetConnectOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetConnectOption
ms.assetid: a1b325cf-0c42-41c1-b141-b5a4fee7e708
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 919220fed87878bf0bcf23359f0714f702f9a6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202658"
---
# <a name="sqlsetconnectoption-mapping"></a>Mapping di SQLSetConnectOption
Quando ODBC 2. *x* l'applicazione chiama **SQLSetConnectOption** tramite un driver ODBC 3 *. x* , la chiamata a  
  
```  
SQLSetConnectOption(hdbc, fOption, vParam)  
```  
  
 il risultato è il seguente:  
  
-   Se *fOption* indica un attributo di connessione definito da ODBC che richiede una stringa, gestione driver chiama  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, SQL_NTS)  
    ```  
  
-   Se *fOption* indica un attributo di connessione definito da ODBC che restituisce un valore integer a 32 bit, gestione driver chiama  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0)  
    ```  
  
-   Se *fOption* indica un attributo di connessione definito dal driver, gestione driver chiama  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength)  
    ```  
  
 Nei tre casi precedenti, l'argomento *connectionHandle* è impostato sul valore in *HDBC*, l'argomento *attribute* è impostato sul valore in *fOption* e l'argomento *ValuePtr* è impostato sullo stesso valore di *parametro vParam*.  
  
 Poiché Gestione driver non è in grado di stabilire se l'attributo di connessione definito dal driver necessita di un valore integer stringa o a 32 bit, deve passare un valore valido per l'argomento *bufferLength* di **SQLSetConnectAttr**. Se il driver ha definito una semantica speciale per gli attributi di connessione definiti dal driver e deve essere chiamata usando **SQLSetConnectOption**, deve supportare **SQLSetConnectOption**.  
  
 Se ODBC 2. *x* l'applicazione chiama **SQLSetConnectOption** per impostare un'opzione di istruzione specifica del driver in un driver ODBC 3 *. x* e l'opzione è stata definita in ODBC 2. versione *x* del driver, è necessario definire una nuova costante manifesto per l'opzione nel driver ODBC 3 *. x* . Se la costante del manifesto precedente viene utilizzata nella chiamata a **SQLSetConnectOption**, gestione driver chiamerà **SQLSetConnectAttr** con l'argomento **StringLength** impostato su 0.  
  
 Per un driver ODBC 3 *. x* , gestione driver non verifica più se *fOption* è compreso tra SQL_CONN_OPT_MIN e SQL_CONN_OPT_MAX oppure è maggiore di SQL_CONNECT_OPT_DRVR_START.  
  
## <a name="setting-statement-options-on-the-connection-level"></a>Impostazione delle opzioni dell'istruzione a livello di connessione  
 In ODBC 2. *x*, un'applicazione può chiamare **SQLSetConnectOption** per impostare un'opzione di istruzione. Al termine, il driver stabilisce l'opzione Statement come impostazione predefinita per le istruzioni allocate in un secondo momento per tale connessione. Viene definito dal driver se il driver imposta l'opzione dell'istruzione per tutte le istruzioni esistenti associate alla connessione specificata.  
  
 Questa funzionalità è stata deprecata in ODBC 3 *. x*. I driver ODBC 3 *. x* devono supportare solo l'impostazione ODBC 2. attributi di istruzione *x* a livello di connessione se desiderano utilizzare ODBC 2. applicazioni *x* che eseguono questa operazione. Le applicazioni ODBC 3 *. x* non devono mai impostare gli attributi di istruzione a livello di connessione. Non è possibile impostare gli attributi dell'istruzione ODBC 3 *. x* a livello di connessione, ad eccezione degli attributi SQL_ATTR_METADATA_ID e SQL_ATTR_ASYNC_ENABLE, che sono sia attributi di connessione che attributi di istruzione, che possono essere impostati a livello di connessione o di istruzione.
