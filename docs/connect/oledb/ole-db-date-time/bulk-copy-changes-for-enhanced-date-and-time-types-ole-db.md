---
title: Modifiche apportate alla copia bulk per i tipi di data e ora migliorati (OLE DB) | Microsoft Docs
description: Informazioni sui miglioramenti apportati ai tipi di data/ora per supportare la funzionalità di copia bulk in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4ceeea79de149408cb57a92c340de97ca22125f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748801"
---
# <a name="bulk-copy-changes-for-enhanced-date-and-time-types-ole-db"></a>Modifiche della copia bulk per i tipi di data e ora migliorati (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In questo articolo vengono descritti i miglioramenti apportati ai tipi di data/ora per supportare la funzionalità di copia bulk in OLE DB Driver per SQL Server.  
  
## <a name="format-files"></a>File di formato  
 Nella tabella seguente viene descritto l'input utilizzato per specificare i tipi di data/ora e i nomi dei tipi di dati del file host corrispondenti quando si compilano file di formato in modo interattivo.  
  
|tipo di archiviazione di file|Tipo di dati del file host|Risposta alla richiesta: "Specificare il tipo di archiviazione di file del campo <nome_campo> [\<default>]:"|  
|-----------------------|-------------------------|-----------------------------------------------------------------------------------------------------|  
|Datetime|SQLDATETIME|d|  
|Smalldatetime|SQLDATETIM4|D|  
|Data|SQLDATE|de|  
|Tempo|SQLTIME|te|  
|Datetime2|SQLDATETIME2|d2|  
|Datetimeoffset|SQLDATETIMEOFFSET|do|  
  
 Il file XSD per i file di formato XML includerà le aggiunte seguenti:  
  
```  
<xs:complexType name="SQLDATETIME2">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATETIMEOFFSET">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATE">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLTIME">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
```  
  
## <a name="character-data-files"></a>File di dati di tipo carattere  
 Nei file di dati di tipo carattere i valori di data e ora vengono rappresentati come descritto nella sezione "Formati di dati: stringhe e valori letterali" di [Supporto dei tipi di dati per i miglioramenti relativi a data e ora OLE DB](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md) per OLE DB.  
  
 Nei file di dati nativi, i valori di data e ora per i quattro nuovi tipi vengono rappresentati come le rispettive rappresentazioni TDS (Tabular Data Stream) con scala 7, in quanto si tratta del valore massimo supportato da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e nei file di dati con estensione bcp non viene archiviata la scala di tali colonne. Non è stata apportata alcuna modifica all'archiviazione dei tipi **datetime** e **smalldatetime** esistenti o delle rispettive rappresentazioni TDS.  
  
 Di seguito vengono indicate le dimensioni dello spazio di archiviazione per i diversi tipi di archiviazione per OLE DB:  
  
|tipo di archiviazione di file|Dimensioni dello spazio di archiviazione in byte|  
|-----------------------|---------------------------|  
|Datetime|8|  
|smalldatetime|4|  
|Data|3|  
|time|6|  
|datetime2|9|  
|datetimeoffset|11|  
 
  
## <a name="bcp-types-in-msoledbsqlh"></a>Tipi BCP in msoledbsql.h  
 In msoledbsql.h sono definiti i tipi seguenti. Questi tipi vengono passati con il parametro *eUserDataType* di IBCPSession::BCPColFmt in OLE DB.  
  
|tipo di archiviazione di file|Tipo di dati del file host|Tipo in msoledbsql.h per l'uso con IBCPSession::BCPColFmt|valore|  
|-----------------------|-------------------------|-----------------------------------------------------------|-----------|  
|Datetime|SQLDATETIME|BCP_TYPE_SQLDATETIME|0x3d|  
|Smalldatetime|SQLDATETIM4|BCP_TYPE_SQLDATETIM4|0x3a|  
|Data|SQLDATE|BCP_TYPE_SQLDATE|0x28|  
|Tempo|SQLTIME|BCP_TYPE_SQLTIME|0x29|  
|Datetime2|SQLDATETIME2|BCP_TYPE_SQLDATETIME2|0x2a|  
|Datetimeoffset|SQLDATETIMEOFFSET|BCP_TYPE_SQLDATETIMEOFFSET|0x2b|  
  
## <a name="bcp-data-type-conversions"></a>Conversioni dei tipi di dati BCP  
 Nelle tabelle seguenti vengono fornite informazioni sulla conversione.  
  
 **Nota per OLE DB** Le conversioni seguenti vengono eseguite da IBCPSession. IRowsetFastLoad usa le conversioni OLE DB come definito in [Conversioni eseguite da client a server](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md). Si noti che i valori datetime vengono arrotondati a 1/300 di secondo, mentre per i valori smalldatetime i secondi vengono impostati su zero in seguito all'esecuzione delle conversioni client descritte di seguito. L'arrotondamento dei valori datetime viene applicato a ore e minuti, ma non alla data.  
  
|A -><br /><br /> From|Data|time|smalldatetime|Datetime|datetime2|datetimeoffset|char|wchar|  
|------------------------|----------|----------|-------------------|--------------|---------------|--------------------|----------|-----------|  
|Data|1|-|1, 6|1, 6|1, 6|1, 5, 6|1, 3|1, 3|  
|Tempo|N/D|1, 10|1, 7, 10|1, 7, 10|1, 7, 10|1, 5, 7, 10|1, 3|1, 3|  
|Smalldatetime|1, 2|1, 4, 10|1|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime|1, 2|1, 4, 10|1, 12|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime2|1, 2|1, 4, 10|1, 12|1, 10|1, 10|1, 5, 10|1, 3|1, 3|  
|Datetimeoffset|1, 2, 8|1, 4, 8, 10|1, 8, 10|1, 8, 10|1, 8, 10|1, 10|1, 3|1, 3|  
|char/wchar (date)|9|-|9, 6, 12|9, 6, 12|9, 6|9, 5, 6|N/D|N/D|  
|char/wchar (time)|-|9, 10|9, 7, 10, 12|9, 7, 10, 12|9, 7, 10|9, 5, 7, 10|N/D|N/D|  
|char/wchar (datetime)|9, 2|9, 4, 10|9, 10, 12|9, 10, 12|9, 10|9, 5, 10|N/D|N/D|  
|char/wchar (datetimeoffset)|9, 2, 8|9, 4, 8, 10|9, 8, 10, 12|9, 8, 10, 12|9, 8, 10|9, 10|N/D|N/D|  
  
#### <a name="key-to-symbols"></a>Descrizione dei simboli  
  
|Simbolo|Significato|  
|------------|-------------|  
|-|Non viene supportata alcuna conversione.<br />|  
|1|Se i dati specificati non sono validi, viene inserito un errore. Per i valori datetimeoffset, la parte relativa all'ora deve essere compresa nell'intervallo supportato in seguito alla conversione in UTC, anche se non è necessaria alcuna conversione in UTC. Ciò è dovuto al fatto che TDS e il server in genere normalizzano l'ora nei valori datetimeoffset per UTC. Di conseguenza, il client deve verificare che i componenti relativi all'ora siano compresi nell'intervallo supportato in seguito alla conversione in UTC.|  
|2|Il componente relativo all'ora viene ignorato.|  
|3|Se si verifica un troncamento con perdita di dati, viene inserito un errore. Per datetime2 il numero di cifre per i secondi frazionari, ovvero la scala, è determinato dalle dimensioni della colonna di destinazione in base alla tabella seguente. Per dimensioni di colonna maggiori dell'intervallo specificato nella tabella, si presuppone una scala 9. Questa conversione deve consentire fino a nove cifre per i secondi frazionari, il massimo consentito da OLE DB.<br /><br /> **Tipo:** DBTIME2<br /><br /> **Scala prevista 0** 8<br /><br /> **Scala prevista 1..9** 1..9<br /><br /> <br /><br /> **Tipo:** DBTIMESTAMP<br /><br /> **Scala implicita 0:** 19<br /><br /> **Scala implicita 1..9:** 21..29<br /><br /> <br /><br /> **Tipo:** DBTIMESTAMPOFFSET<br /><br /> **Scala implicita 0:** 26<br /><br /> **Scala implicita 1..9:** 28..36|  
|4|Il componente relativo alla data viene ignorato.|  
|5|Il fuso orario è impostato su UTC, ad esempio 00:00.|  
|6|L'ora è impostata su zero.|  
|7|La data è impostata su 1900-01-01.|  
|8|La differenza di fuso orario viene ignorata.|  
|9|La stringa viene analizzata e convertita in un valore date, datetime, datetimeoffset o time a seconda del primo carattere di punteggiatura rilevato e della presenza degli altri componenti. La stringa viene quindi convertita nel tipo di destinazione, in base alle regole indicate nella tabella alla fine di questo articolo per il tipo di origine individuato dal processo. Se i dati specificati non possono essere analizzati senza errore o se qualsiasi componente è al di fuori dell'intervallo consentito o non vi è conversione dal tipo del valore letterale al tipo di destinazione, viene inserito un errore. Per i parametri datetime e smalldatetime, se l'anno non è compreso nell'intervallo supportato da questi tipi viene inserito un errore.<br /><br /> Per datetimeoffset, il valore deve essere compreso nell'intervallo supportato in seguito alla conversione in UTC, anche se non è necessaria alcuna conversione in UTC. Ciò è dovuto al fatto che TDS e il server normalizzano sempre l'ora in valori datetimeoffset per UTC. Il client deve pertanto verificare che i componenti relativi all'ora siano compresi nell'intervallo supportato in seguito alla conversione in UTC. Se il valore non è compreso nell'intervallo UTC supportato, viene inserito un errore.|  
|10|Per le conversioni da client a server, se si verifica un troncamento con perdita di dati viene inserito un errore. Questo errore si verifica anche se il valore non è incluso nell'intervallo che può essere rappresentato dall'intervallo UTC utilizzato dal server. Se si verifica un troncamento dei secondi o dei secondi frazionari in una conversione da server a client, viene generato solo un avviso.|  
|11|Per le conversioni da client a server, se si verifica un troncamento con perdita di dati viene inserito un errore.|
|12|I secondi vengono impostati su zero e i secondi frazionari vengono ignorati. Non è possibile alcun errore di troncamento.|  
|N/D|Viene mantenuto il comportamento di [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] e versioni precedenti.|  
  
## <a name="see-also"></a>Vedere anche     
 [Miglioramenti relativi a data e ora &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
