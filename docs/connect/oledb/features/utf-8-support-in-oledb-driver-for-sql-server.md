---
title: Supporto UTF-8 in OLE DB Driver for SQL Server
description: Informazioni sul driver OLE DB per il supporto SQL Server per la codifica del server UTF-8 e la codifica client UTF-8.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 52e66fe97869046d182f816a846b1b0121949385
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673909"
---
# <a name="utf-8-support-in-ole-db-driver-for-sql-server"></a>Supporto UTF-8 in OLE DB Driver for SQL Server

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Microsoft OLE DB Driver for SQL Server (versione 18.2.1) aggiunge il supporto per la codifica server UTF-8. Per informazioni sul supporto della codifica UTF-8 per SQL Server, vedere:

- [Regole di confronto e supporto Unicode](../../../relational-databases/collations/collation-and-unicode-support.md)
- [Supporto UTF-8](../../../relational-databases/collations/collation-and-unicode-support.md#utf8)

Nella versione 18.4.0 del driver è stato aggiunto il supporto per la codifica client UTF-8, abilitata con la casella di controllo "Use Unicode UTF-8 for worldwide language support" (Usa Unicode UTF-8 per il supporto linguistico internazionale) in Impostazioni area in Windows 10.

> [!NOTE]  
> Microsoft OLE DB Driver per SQL Server usa la funzione [GetACP](/windows/win32/api/winnls/nf-winnls-getacp) per determinare la codifica del buffer di input DBTYPE_STR.
>
> Gli scenari in cui GetACP restituisce una codifica UTF-8, abilitata con la casella di controllo "Use Unicode UTF-8 for worldwide language support" (Usa Unicode UTF-8 per il supporto linguistico internazionale) in Impostazioni area in Windows 10, sono supportati a partire dalla versione 18.4. Se nelle versioni precedenti il buffer deve archiviare dati Unicode, il tipo di dati del buffer deve essere impostato su *DBTYPE_WSTR* (con codifica UTF-16).

## <a name="data-insertion-into-a-utf-8-encoded-char-or-varchar-column"></a>Inserimento dei dati in una colonna CHAR or VARCHAR con codifica UTF-8

Quando si crea un buffer del parametro di input per l'inserimento, il buffer viene descritto tramite una matrice di [strutture DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Ogni struttura DBBINDING associa un singolo parametro al buffer del consumer e contiene informazioni come la lunghezza e il tipo del valore dati. Per un buffer del parametro di input di tipo CHAR, il campo *wType* della struttura DBBINDING deve essere impostato su DBTYPE_STR. Per un buffer del parametro di input di tipo WCHAR, il campo *wType* della struttura DBBINDING deve essere impostato su DBTYPE_WSTR.

Quando si esegue un comando con i parametri, il driver costruisce informazioni sul tipo di dati dei parametri. Se il tipo di buffer di input e il tipo di dati del parametro corrispondono, non viene eseguita alcuna conversione nel driver. In caso contrario, il driver converte il buffer del parametro di input nel tipo di dati del parametro. Il tipo di dati del parametro può essere impostato in modo esplicito dall'utente chiamando [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)). Se le informazioni non vengono specificate, il driver deriva le informazioni sul tipo di dati del parametro (a) recuperando i metadati della colonna dal server quando viene preparata l'istruzione o (b) tentando una conversione predefinita dal tipo di dati del parametro di input.

Il buffer del parametro di input può essere convertito nelle regole di confronto della colonna server dal driver o dal server a seconda del tipo di dati del buffer di input e del tipo di dati del parametro. Durante la conversione, può verificarsi la perdita dei dati se la tabella codici del client o la tabella codici delle regole di confronto del database non è in grado di rappresentare tutti i caratteri nel buffer di input. La tabella seguente descrive il processo di conversione durante l'inserimento dei dati nella colonna che supporta UTF-8:

|Tipo di dati del buffer|Tipo di dati del parametro|Conversione|Precauzione per l'utente|
|---             |---                |---       |---            |
|DBTYPE_STR|DBTYPE_STR|Conversione server dalla tabella codici del client alla tabella codici delle regole di confronto del database; conversione server dalla tabella codici delle regole di confronto del database alla tabella codici delle regole di confronto della colonna.|Verificare che la tabella codici del client e la tabella codici delle regole di confronto del database siano in grado di rappresentare tutti i caratteri nei dati di input. Per inserire un carattere polacco, la tabella codici del client può ad esempio essere impostata su 1250 (ANSI dell'Europa centrale) e le regole di confronto del database possono avvalersi del polacco come designazione delle regole di confronto (ad esempio Polish_100_CI_AS_SC) oppure supportare UTF-8.|
|DBTYPE_STR|DBTYPE_WSTR|Conversione driver dalla tabella codici del client alla codifica UTF-16; conversione server dalla codifica UTF-16 alla tabella codici delle regole di confronto della colonna.|Verificare che la tabella codici del client sia in grado di rappresentare tutti i caratteri nei dati di input. Per inserire un carattere polacco, la tabella codici del client potrebbe ad esempio essere impostata su 1250 (ANSI dell'Europa centrale).|
|DBTYPE_WSTR|DBTYPE_STR|Conversione driver dalla codifica UTF-16 alla tabella codici delle regole di confronto del database; conversione server dalla tabella codici delle regole di confronto del database alla tabella codici delle regole di confronto della colonna.|Verificare che la tabella codici delle regole di confronto del database sia in grado di rappresentare tutti i caratteri nei dati di input. Per inserire un carattere polacco, la tabella codici delle regole di confronto del database può ad esempio avvalersi del polacco come designazione delle regole di confronto (ad esempio Polish_100_CI_AS_SC) oppure supportare UTF-8.|
|DBTYPE_WSTR|DBTYPE_WSTR|Conversione server da UTF-16 alla tabella codici delle regole di confronto della colonna.|No.|

## <a name="data-retrieval-from-a-utf-8-encoded-char-or-varchar-column"></a>Recupero dati da una colonna CHAR or VARCHAR con codifica UTF-8

Quando si crea un buffer per i dati recuperati, il buffer viene descritto tramite una matrice di [strutture DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Ogni struttura DBBINDING associa una singola colonna nella riga recuperata. Per recuperare i dati della colonna di tipo CHAR, impostare il campo *wType* della struttura DBBINDING su DBTYPE_STR. Per recuperare i dati della colonna di tipo WCHAR, impostare il campo *wType* della struttura DBBINDING su DBTYPE_WSTR.

Per l'indicatore del tipo di buffer DBTYPE_STR, il driver converte i dati con codifica UTF-8 nella codifica del client. L'utente deve assicurarsi che la codifica client sia in grado di rappresentare i dati della colonna UTF-8. In caso contrario potrebbe verificarsi la perdita dei dati.

Per l'indicatore del tipo di buffer DBTYPE_WSTR, il driver converte i dati con codifica UTF-8 nella codifica UTF-16.

## <a name="communication-with-servers-that-dont-support-utf-8"></a>La comunicazione con i server non supporta UTF-8

Microsoft OLE DB Driver per SQL Server garantisce che i dati vengano esposti al server in un modo comprensibile al server stesso. Quando si inseriscono dati da client abilitati per UTF-8, il driver converte le stringhe con codifica UTF-8 nella tabella codici delle regole di confronto del database prima di inviarle al server.

> [!NOTE]  
> L'uso dell'interfaccia [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) per l'inserimento di dati con codifica UTF-8 in una colonna di testo legacy è limitato solo ai server che supportano UTF-8. Per informazioni, vedere [BLOB e oggetti OLE](../ole-db-blobs/blobs-and-ole-objects.md).

## <a name="see-also"></a>Vedere anche  

[Driver OLE DB per funzionalità di SQL Server](oledb-driver-for-sql-server-features.md)  
[Supporto UTF-16 nel driver OLE DB per SQL Server](utf-16-support-in-oledb-driver-for-sql-server.md)  
