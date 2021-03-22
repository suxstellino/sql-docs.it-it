---
description: Informazioni su come creare interfacce e wrapper proxy che consentono di accedere alle estensioni dell'API JDBC.
title: Wrapper e interfacce
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 27fc9b72-9f21-4728-abcb-5c015f28a6ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 653cb21216c123a342f26134b3288b9f54b68b67
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793002"
---
# <a name="wrappers-and-interfaces"></a>Wrapper e interfacce

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] supporta le interfacce che consentono la creazione di un proxy di una classe e i wrapper che consentono di accedere alle estensioni dell'API JDBC specifiche di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] tramite un'interfaccia proxy.

## <a name="wrappers"></a>Wrapper

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] supporta l'interfaccia java.sql.Wrapper. Tale interfaccia fornisce un meccanismo per accedere alle estensioni dell'API JDBC specifiche di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] tramite un'interfaccia proxy.

L'interfaccia java.sql.Wrapper definisce due metodi: **isWrapperFor** e **unwrap**. Il metodo **isWrapperFor** verifica se l'oggetto di input specificato implementa questa interfaccia. Il metodo **unwrap** restituisce un oggetto che implementa questa interfaccia per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

I metodi **isWrapperFor** e **unwrap** sono esposti come segue:

- [Metodo isWrapperFor &#40;SQLServerCallableStatement&#41;](reference/iswrapperfor-method-sqlservercallablestatement.md)
- [Metodo unwrap &#40;SQLServerCallableStatement&#41;](reference/unwrap-method-sqlservercallablestatement.md)
- [Metodo isWrapperFor &#40;SQLServerConnectionPoolDataSource&#41;](reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)
- [Metodo unwrap &#40;SQLServerConnectionPoolDataSource&#41;](reference/unwrap-method-sqlserverconnectionpooldatasource.md)
- [Metodo isWrapperFor &#40;SQLServerDataSource&#41;](reference/iswrapperfor-method-sqlserverdatasource.md)
- [Metodo unwrap &#40;SQLServerDataSource&#41;](reference/unwrap-method-sqlserverdatasource.md)
- [Metodo isWrapperFor &#40;SQLServerPreparedStatement&#41;](reference/iswrapperfor-method-sqlserverpreparedstatement.md)
- [Metodo unwrap &#40;SQLServerPreparedStatement&#41;](reference/unwrap-method-sqlserverpreparedstatement.md)
- [Metodo isWrapperFor &#40;SQLServerStatement&#41;](reference/iswrapperfor-method-sqlserverstatement.md)
- [Metodo unwrap &#40;SQLServerStatement&#41;](reference/unwrap-method-sqlserverstatement.md)
- [Metodo isWrapperFor &#40;SQLServerXADataSource&#41;](reference/iswrapperfor-method-sqlserverxadatasource.md)
- [Metodo unwrap &#40;SQLServerXADataSource&#41;](reference/unwrap-method-sqlserverxadatasource.md)

## <a name="interfaces"></a>Interfacce

A partire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dal driver JDBC 3,0, le interfacce sono disponibili per un server applicazioni per accedere a un metodo specifico del driver dalla classe associata. Il wrapping della classe può essere eseguito dal server applicazioni tramite la creazione di un proxy, esponendo la funzionalità specifica di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] da un'interfaccia. Il driver [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] supporta le interfacce che includono costanti e metodi specifici di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], per consentire a un server applicazioni di creare un proxy della classe.

Le interfacce derivano da interfacce Java standard, in modo che sia possibile utilizzare lo stesso oggetto una volta che è stato ancrittografato per accedere alla funzionalità specifica del driver o alla funzionalità generica [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] .

Sono aggiunte le interfacce seguenti:

- [ISQLServerCallableStatement](reference/isqlservercallablestatement-interface.md)
- [ISQLServerConnection](reference/isqlserverconnection-interface.md)
- [ISQLServerDataSource](reference/isqlserverdatasource-interface.md)
- [ISQLServerPreparedStatement](reference/isqlserverpreparedstatement-interface.md)
- [ISQLServerResultSet](reference/isqlserverresultset-interface.md)
- [ISQLServerStatement](reference/isqlserverstatement-interface.md)

## <a name="example"></a>Esempio

### <a name="description"></a>Descrizione

In questo esempio viene illustrato come accedere a una funzione specifica di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] da un oggetto DataSource. Il wrapping della classe DataSource potrebbe essere stato eseguito da un server applicazioni. Per accedere alla costante o alla funzione specifica del driver JDBC è possibile annullare il wrapping dall'origine dei dati a un'interfaccia ISQLServerDataSource e utilizzare le funzioni dichiarate in questa interfaccia.

### <a name="code"></a>Codice

```java
import javax.sql.*;
import java.sql.*;
import com.microsoft.sqlserver.jdbc.*;

public class UnWrapTest {
   public static void main(String[] args) {
      // This is a test.  This DataSource object could be something from an appserver
      // which has wrapped the real SQLServerDataSource with its own wrapper
      SQLServerDataSource ds = new SQLServerDataSource();
      checkSendStringParametersAsUnicode(ds);
   }

   // Unwrap to the ISQLServerDataSource interface to access the getSendStringParametersAsUnicode function
   static void checkSendStringParametersAsUnicode(DataSource ds) {
      try {
         final ISQLServerDataSource sqlServerDataSource = ds.unwrap(ISQLServerDataSource.class);
         boolean sendStringParametersAsUnicode = sqlServerDataSource.getSendStringParametersAsUnicode();

         System.out.println("Send string as parameter value is:-" + sendStringParametersAsUnicode);

      } catch (SQLException sqlE) {
         System.out.println("Exception:-" + sqlE);
      }
   }
}
```

## <a name="see-also"></a>Vedere anche

[Informazioni sui tipi di dati del driver JDBC](understanding-the-jdbc-driver-data-types.md)
