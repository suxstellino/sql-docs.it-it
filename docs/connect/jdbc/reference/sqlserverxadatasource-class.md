---
description: Classe SQLServerXADataSource
title: Classe SQLServerXADataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 95fc7b07-2498-4a7e-8f7f-ee0d86b598b4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61d86781a9f730c78a6e2d78da0d5152d3f84e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180354"
---
# <a name="sqlserverxadatasource-class"></a>Classe SQLServerXADataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rappresenta una factory per oggetti [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) usata internamente.  
  
 **Pacchetto:** com.microsoft.sqlserver.jdbc  
  
 **Estende:** [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
 **Implementa:** javax.sql.XADataSource  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
public class SQLServerXADataSource  
```  
  
## <a name="remarks"></a>Osservazioni  
 Un oggetto che implementa l'interfaccia SQLServerXADataSource viene generalmente registrato con un servizio di denominazione che usa l'interfaccia JNDI (Java Naming and Directory Interface).  
  
 La classe SQLServerXADataSource specifica connessioni di database da usare nelle transazioni distribuite (XA). La classe SQLServerXADataSource supporta anche il pool di connessioni fisiche. Le interfacce SQLServerXADataSource e SQLServerXAConnection, definite nel pacchetto javax.sql, vengono implementate da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Un oggetto SQLServerXAConnection è una connessione in pool che può partecipare a una transazione distribuita. Più precisamente, SQLServerXAConnection estende l'interfaccia SQLServerPooledConnection aggiungendo il metodo [getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md). Questo metodo produce un oggetto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) che può essere usato da un gestore transazioni per coordinare le attività eseguite in questa connessione con gli altri partecipanti della transazione distribuita. Poiché estendono l'interfaccia SQLServerPooledConnection, gli oggetti SQLServerXAConnection supportano tutti i metodi degli oggetti SQLServerPooledConnection. Si tratta di connessioni fisiche riutilizzabili a un'origine dati sottostante che generano handle di connessioni logiche che possono essere passati nuovamente a un'applicazione JDBC.  
  
 Gli oggetti SQLServerXAConnection sono prodotti da un oggetto SQLServerXADataSource. Gli oggetti SQLServerConnectionPoolDataSource e gli oggetti SQLServerXADataSource sono simili perché sono entrambi implementati al di sotto di un livello di origine dati visibile all'applicazione JDBC. Questa architettura consente a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di supportare transazioni distribuite in modo trasparente all'applicazione. L'oggetto SQLServerXADataSource può essere configurato per integrarsi con [!INCLUDE[msCoName](../../../includes/msconame_md.md)] Distributed Transaction Coordinator (DTC) per offrire una reale elaborazione di transazioni distribuite.  
  
## <a name="see-also"></a>Vedere anche  
 [Membri di SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [Informazioni di riferimento sull'API del driver JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
