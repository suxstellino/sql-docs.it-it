---
description: Creazione dei file di connessione del server (MySQLToSQL)
title: Creazione dei file di connessione del server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Server connection file validation
- Server connection files
ms.assetid: df0e970c-da0b-4118-b359-c9dcbbad16d6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 439d70f428fff6e5445fca54e8bd669096a1c6c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016627"
---
# <a name="creating-the-server-connection-files-mysqltosql"></a>Creazione dei file di connessione del server (MySQLToSQL)
È possibile specificare le informazioni sul server nella sezione server del file script o in un file di connessione server separato. Il parametro della riga di comando per il file di connessione del server è, `-c <serverconnectionfile>` . Se lo stesso ID server è presente nel file di script e nel file di connessione al server, viene considerata la definizione del server nel file di script.  
  
**Esempio:**  
  
```xml  
<!--Sample of server connection file commands -->  
  
<mysql name ="<source-server-unique-name>">  
  
   <standard-mode>  
  
      <server value ="<server-name>"/>  
  
      <user-id value ="<user-name>"/>  
  
      <password value ="<password>"/>  
  
      <port value ="<port>"/>  
  
   </standard-mode>  
  
</mysql>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
   <sql-server-authentication>  
  
      <server value="<server-name>"/>  
  
      <database value="<database-name>"/>  
  
      <user-id value="<user-name>"/>  
  
      <password value="<password>"/>  
  
      <encrypt value="<true/false>"/>  
  
      <trust-server-certificate value="<true/false>"/>  
  
   </sql-server-authentication>  
  
</sql-server>  
```  
  
```xml  
<!--Connection to SQL Azure-->  
  
<sql-azure name="<target-server-unique-name>">  
  
   <server value="<server-name>"/>  
  
   <database value="<database-name>"/>  
  
   <user-id value="<user-name>"/>  
  
   <password value="<password>"/>  
  
</sql-azure>  
```  
  
## <a name="server-connection-file-validation"></a>Convalida file di connessione server  
L'utente può facilmente convalidare il proprio file di connessione del server in base al file di definizione dello schema **' 2ssconsolescriptserversschema. xsd '** disponibile nella cartella ' schemi '.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo per la gestione della console è [l'esecuzione della console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Esecuzione della console SSMA (MySQL)](./executing-the-ssma-console-mysqltosql.md)  
