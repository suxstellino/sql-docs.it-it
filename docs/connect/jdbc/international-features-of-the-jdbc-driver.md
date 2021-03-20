---
description: Informazioni sulle funzionalità di internazionalizzazione del driver JDBC e su come creare un'applicazione localizzata.
title: Caratteristiche internazionali del driver JDBC
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: bbb74a1d-9278-401f-9530-7b5f45aa79de
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 159c1e35761876d786ad0a36c78eaa8ce8f3e847
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673425"
---
# <a name="international-features-of-the-jdbc-driver"></a>Caratteristiche internazionali del driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Le funzionalità di internazionalizzazione di [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] includono gli elementi seguenti:
  
- Supporto per un'interfaccia completamente localizzata nelle stesse lingue di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
- Supporto per le conversioni di linguaggio Java di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dipendenti dalle impostazioni locali  
- Supporto per lingue internazionali, indipendentemente dal sistema operativo  
- Supporto per nomi IDN (a partire da Microsoft JDBC Driver 6.0 per SQL Server)  
  
## <a name="handling-of-character-data"></a>Gestione dei dati di tipo carattere

I dati di tipo carattere in Java vengono gestiti come Unicode per impostazione predefinita. L'oggetto **String** Java rappresenta dati di tipo carattere Unicode. Nel driver JDBC l'unica eccezione a questa regola è costituita dai metodi per il richiamo e l'impostazione dei flussi ASCII, che rappresentano casi speciali poiché utilizzano flussi di byte con il presupposto implicito di singole tabelle codici conosciute (ASCII).  
  
Il driver JDBC fornisce inoltre la proprietà della stringa di connessione **sendStringParametersAsUnicode**. che può essere utilizzata per specificare che i parametri preparati per i dati di tipo carattere verranno inviati in formato ASCII or MBCS (Multi-byte Character Set) anziché Unicode. Per altre informazioni sulla proprietà della stringa di connessione **sendStringParametersAsUnicode**, vedere [Impostazione delle proprietà di connessione](setting-the-connection-properties.md).  
  
### <a name="driver-incoming-conversions"></a>Conversioni in ingresso nel driver

I dati di tipo text Unicode provenienti dal server non devono essere convertiti. Vengono passati direttamente in formato Unicode. I dati non Unicode provenienti dal server vengono convertiti dalla tabella codici, a livello di database o di colonna, a Unicode. Per eseguire queste conversioni vengono utilizzate le routine di conversione JVM (Java Virtual Machine). Le conversioni vengono eseguite su tutti metodi per il richiamo di flussi String e Character tipizzati.  
  
Se JVM non dispone del supporto della tabella codici appropriato per i dati del database, il driver JDBC genera un'eccezione "XXX codepage non supportata dall'ambiente Java". Per risolvere il problema, è necessario installare il supporto completo per caratteri internazionali richiesto per tale JVM.
  
### <a name="driver-outgoing-conversions"></a>Conversioni in uscita dal driver

I dati di tipo carattere che passano dal driver al server possono essere ASCII o Unicode. Ad esempio, i nuovi metodi per caratteri nazionali di JDBC 4.0, come i metodi setNString, setNCharacterStream e setNClob delle classi [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) e [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md), inviano sempre i valori i parametri al server in formato Unicode.  
  
D'altro canto, i metodi dell'API per caratteri non nazionali, ad esempio i metodi setString, setCharacterStream e setClob delle classi [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md) e [SQLServerCallableStatement](reference/sqlservercallablestatement-class.md) inviano i valori al server in formato Unicode solo quando la proprietà **sendStringParametersAsUnicode** è impostata su "true", che corrisponde al valore predefinito.  
  
## <a name="non-unicode-parameters"></a>Parametri non Unicode

Per prestazioni ottimali con il tipo **CHAR**, **VARCHAR** o **LONGVARCHAR** di parametri non Unicode, impostare la proprietà della stringa di connessione **sendStringParametersAsUnicode** su "false" e usare i metodi per caratteri non nazionali.  
  
## <a name="formatting-issues"></a>Problemi di formattazione

Per la data, l'ora e le valute, tutte le operazioni di formattazione con i dati localizzati vengono eseguite a livello di linguaggio Java usando l'oggetto Impostazioni locali e i diversi metodi di formattazione per i tipi di dati **Data**, **Calendario** e **Numero**. Nei rari casi in cui il driver JDBC deve passare dati dipendenti dalle impostazioni locali in un formato localizzato, viene utilizzato il formattatore appropriato con le impostazioni locali JVM predefinite.  
  
## <a name="collation-support"></a>Supporto delle regole di confronto

JDBC Driver 3.0 supporta tutte le regole di confronto supportate in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] e [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], nonché le nuove regole di confronto o le nuove versioni dei nomi di regole di confronto Windows introdotte in [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)].  
  
Per ulteriori informazioni sulle regole di confronto, vedere [regole di confronto e supporto Unicode](/previous-versions/sql/sql-server-2008-r2/ms143503(v=sql.105)) e [nome delle regole di confronto di Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).  
  
## <a name="using-international-domain-names-idn"></a>Uso di International Domain Names (IDN)

JDBC Driver 6.0 per SQL Server supporta l'uso di IDN (Internationalized Domain Name) e può convertire un serverName Unicode in codifica compatibile con ASCII (Punycode) quando richiesto durante una connessione.  Se i nomi IDN vengono archiviati in Domain Name System (DNS) come stringhe ASCII in formato Punycode (specificato dal RFC 3490), abilitare la conversione del nome del server Unicode impostando la proprietà serverNameAsACE su true.  In caso contrario, se il servizio DNS è configurato per consentire l'utilizzo di caratteri Unicode, impostare la proprietà serverNameAsACE su false (impostazione predefinita).  Per le versioni precedenti del driver JDBC, è anche possibile convertire serverName in Punycode usando i metodi [IDN.toASCII di Java](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html) prima di impostare tale proprietà per una connessione.  
  
> [!NOTE]  
> La maggior parte dei software resolver scritti per piattaforme non Windows si basano sugli standard Internet DSN ed è pertanto più facile utilizzare il formato Punycode per nomi IDN, mentre un Server DNS basato su Windows in una rete privata può essere configurato per consentire l'utilizzo di caratteri UTF-8 in base a ciascun server.  Per ulteriori informazioni, vedere [supporto di caratteri Unicode](/previous-versions/windows/it-pro/windows-server-2003/cc738403(v=ws.10)).  
  
## <a name="see-also"></a>Vedere anche

[Panoramica del driver JDBC](overview-of-the-jdbc-driver.md)  
