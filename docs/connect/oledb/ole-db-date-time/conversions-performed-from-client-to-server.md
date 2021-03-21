---
title: Conversioni eseguite da client a server | Microsoft Docs
description: Informazioni sulle conversioni di data/ora eseguite tra un'applicazione client scritta con OLE DB Driver per SQL Server e SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB], client to server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ff13ec5b2df249d31aeb10a32761d119ed418d3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748521"
---
# <a name="conversions-performed-from-client-to-server"></a>Conversioni eseguite da client a server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Questo articolo descrive le conversioni di data/ora eseguite tra un'applicazione client scritta con il driver OLE DB per SQL Server e [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (o versione successiva).  
  
## <a name="conversions"></a>Conversioni  
 In questo articolo vengono descritte le conversioni eseguite sul client. Se il client specifica una precisione frazionaria dei secondi per un parametro che differisce da quella definita nel server, la conversione client potrebbe comportare un errore nei casi in cui il server consentirebbe l'esecuzione dell'operazione. In particolare, il client tratta come errore qualsiasi troncamento dei secondi frazionari, mentre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] arrotonda valori al secondo intero più vicino.  
  
 Se ICommandWithParameters::SetParameterInfo non viene chiamato, le associazioni DBTYPE_DBTIMESTAMP vengono convertite come se fossero **datetime2**.  
  
|A -><br /><br /> From|DBDATE (date)|DBTIME (time)|DBTIME2 (time)|DBTIMESTAMP (smalldatetime)|DBTIMESTAMP (datetime)|DBTIMESTAMP (datetime2)|DBTIMESTAMPOFFSET (datetimeoffset)|STR|WSTR|SQLVARIANT<br /><br /> (sql_variant)|  
|----------------------|---------------------|---------------------|----------------------|-----------------------------------|------------------------------|-------------------------------|------------------------------------------|---------|----------|-------------------------------------|  
|DATE|1, 2|1, 3, 4|4, 12|1, 12|1, 12|1, 12|1, 5, 12|1, 12|1, 12|1, 12<br /><br /> datetime2(0)|  
|DBDATE|1|-|-|1, 6|1, 6|1, 6|1, 5, 6|1, 10|1, 10|1<br /><br /> Data|  
|DBTIME|-|1|1|1, 7|1, 7|1, 7|1, 5, 7|1, 10|1, 10|1<br /><br /> Time(0)|  
|DBTIME2|-|1, 3|1|1, 7, 10, 14|1, 7, 10, 15|1, 7, 10|1, 5, 7, 10|1, 10, 11|1, 10, 11|1<br /><br /> Time(7)|  
|DBTIMESTAMP|1, 2|1, 3, 4|1, 4, 10|1, 10, 14|1, 10, 15|1, 10|1, 5, 10|1, 10, 11|1, 10, 11|1, 10<br /><br /> datetime2(7)|  
|DBTIMESTAMPOFFSET|1, 2, 8|1, 3, 4, 8|1, 4, 8, 10|1, 8, 10, 14|1, 8, 10, 15|1, 8, 10|1, 10|1, 10, 11|1, 10, 11|1, 10<br /><br /> datetimeoffset(7)|  
|FILETIME|1, 2|1, 3, 4|1, 4, 13|1, 13|1, 13|1, 13|1, 5, 13|1, 13|1, 10|1, 13<br /><br /> datetime2(3)|  
|BYTES|-|-|-|-|-|-|-|N/D|N/D|N/D|  
|VARIANT|1|1|1|1, 10|1, 10|1, 10|1, 10|N/D|N/D|1, 10|  
|SSVARIANT|1, 16|1, 16|1, 16|1, 10, 16|1, 10, 16|1, 10, 16|1, 10, 16|N/D|N/D|1, 16|  
|BSTR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|N/D|N/D|N/D|  
|STR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|N/D|N/D|N/D|  
|WSTR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|N/D|N/D|N/D|  
  
## <a name="key-to-symbols"></a>Descrizione dei simboli  
  
|Simbolo|Significato|  
|------------|-------------|  
|-|Non viene supportata alcuna conversione. Se l'associazione viene convalidata quando viene chiamato IAccessor::CreateAccessor, viene restituito DBBINDSTATUS_UPSUPPORTEDCONVERSION in *rgStatus*. Quando la convalida della funzione di accesso viene rinviata, viene impostato DBSTATUS_E_BADACCESSOR.|  
|N/D|Non applicabile.|  
|1|Se i dati forniti non sono validi, viene impostato DBSTATUS_E_CANTCONVERTVALUE. I dati di input vengono convalidati prima che vengano applicate le conversioni, pertanto quando un componente verrà ignorato da una conversione successiva, dovrà ancora essere valido per consentire la conversione.|  
|2|I campi relativi all'ora vengono ignorati.|  
|3|I secondi frazionari devono essere zero. In caso contrario, viene impostato DBSTATUS_E_DATAOVERFLOW.|  
|4|Il componente relativo alla data viene ignorato.|  
|5|Il fuso orario viene impostato sul fuso orario del client.|  
|6|L'ora è impostata su zero.|  
|7|La data viene impostata sulla data corrente.|  
|8|L'ora viene convertita in formato UTC. Se si verifica un errore durante questa conversione, viene impostato DBSTATUS_E_CANTCONVERTVALUE.|  
|9|La stringa viene analizzata come valore letterale ISO e convertita nel tipo di destinazione. Se l'operazione non riesce, la stringa viene analizzata come valore letterale data OLE (che presenta anche componenti di ora) e convertita da data OLE (DBTYPE_DATE) nel tipo di destinazione.<br /><br /> Se il tipo di destinazione è DBTIMESTAMP, **smalldatetime**, **datetime** o **datetime2**, la stringa deve essere conforme alla sintassi per i valori letterali date, time o **datetime2** oppure alla sintassi riconosciuta da OLE. Se la stringa è un valore letterale data, tutti i componenti di ora vengono impostati su zero. Se la stringa è un valore letterale ora, la data viene impostata sul valore corrente.<br /><br /> Per tutti gli altri tipi di destinazione, la stringa deve essere conforme alla sintassi per i valori letterali del tipo di destinazione.|  
|10|In caso di troncamento dei secondi frazionari con perdita di dati, viene impostato DBSTATUS_E_DATAOVERFLOW. Per le conversioni di stringhe, il controllo dell'overflow è possibile solo quando la stringa è conforme alla sintassi ISO. Se la stringa è un valore letterale data OLE, i secondi frazionari vengono arrotondati.<br /><br /> Per le conversioni di dati di tipo DBTIMESTAMP (datetime) in dati di tipo smalldatetime, nel driver OLE DB per SQL Server viene troncato automaticamente il valore dei secondi anziché generare l'errore DBSTATUS_E_DATAOVERFLOW.|  
|11|Il numero di cifre per i secondi frazionari (scala) è determinato dalla dimensione della colonna di destinazione in base alla tabella riportata di seguito. Per dimensioni di colonna maggiori dell'intervallo specificato nella tabella, si presuppone una scala 9. Questa conversione deve consentire fino a nove cifre per i secondi frazionari, il massimo consentito da OLE DB.<br /><br /> Se tuttavia il tipo di origine è DBTIMESTAMP e i secondi frazionari corrispondono a zero, non vengono generati alcuna cifra per i secondi frazionari né il separatore decimale. Questo comportamento assicura la compatibilità con le versioni precedenti per le applicazioni sviluppate utilizzando provider OLE DB meno recenti.<br /><br /> Una dimensione di colonna pari a ~ 0 implica una dimensione illimitata in OLE DB (9 cifre, a meno che non si applichi la regola delle 3 cifre per DBTIMESTAMP).|  
|12|Viene mantenuta la semantica di conversione precedente a [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] per DBTYPE_DATE. I secondi frazionari vengono troncati in corrispondenza di zero.|  
|13|Viene mantenuta la semantica di conversione precedente a [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] per DBTYPE_FILETIME. Se si usa l'API FileTimeToSystemTime di Windows, la precisione frazionaria dei secondi è limitata a 1 millisecondo.|  
|14|Viene mantenuta la semantica di conversione precedente a [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] per **smalldatetime**. I secondi vengono impostati su 0.|  
|15|Viene mantenuta la semantica di conversione precedente a [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] per **datetime**. I secondi vengono arrotondati al 300° di secondo più prossimo.|  
|16|Il comportamento di conversione di un valore (di un tipo specificato) incorporato in una struttura client SSVARIANT corrisponde al comportamento dello stesso valore e tipo quando non è incorporato in una struttura client SSVARIANT.|  
  
| Type | Lunghezza (in caratteri) | Scalabilità |  
| ---- | ----------------- | ----- |  
|DBTIME2|8, 10..18|0,1..9|  
|DBTIMESTAMP|19, 21..29|0,1..9|  
|DBTIMESTAMPOFFSET|26, 28..36|0,1..9|  
  
## <a name="see-also"></a>Vedere anche  
 [Associazioni e conversioni &#40;OLE DB&#41;](../../oledb/ole-db-date-time/conversions-ole-db.md)  
  
  
