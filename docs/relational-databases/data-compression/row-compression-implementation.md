---
title: Implementazione della compressione di riga
description: Informazioni su come il motore di database di SQL Server implementa la compressione delle righe per semplificare la pianificazione dello spazio di archiviazione necessario per i dati.
ms.custom: ''
ms.date: 03/27/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- compression [SQL Server], row
- row compression [Database Engine]
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f216ae5de400f377dd247ecbc8a671d9f3faf397
ms.sourcegitcommit: 2f971c85d87623c0aed1612406130d840e7bdb2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2021
ms.locfileid: "105744475"
---
# <a name="row-compression-implementation"></a>Implementazione della compressione di riga
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento vengono riepilogate le modalità di implementazione della compressione di riga nel [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Tale riepilogo fornisce informazioni di base che consentono di pianificare lo spazio di archiviazione necessario per i dati.  
  
 L'abilitazione della compressione modifica solo il formato di archiviazione fisica dei dati associato a un tipo di dati, ma non la relativa sintassi o semantica. Le modifiche alle applicazioni non sono obbligatorie quando una o più tabelle sono abilitate per la compressione. Al nuovo formato di archiviazione del record sono associate le seguenti modifiche principali:  
  
-   Riduzione dell'overhead di metadati associati al record. Tali metadati rappresentano informazioni su colonne e sui relativi offset e lunghezze. In alcuni casi, l'overhead di metadati potrebbe essere maggiore del formato di archiviazione obsoleto.  
  
-   Uso del formato di archiviazione a lunghezza variabile per i tipi numerici (come **integer**, **decimal** e **float**) e i tipi basati su quello numerico (come **datetime** e **money**).  
  
-   Archiviazione di stringhe di caratteri a lunghezza fissa utilizzando un formato a lunghezza variabile senza archiviare i caratteri vuoti.  
  
> [!NOTE]  
>  Ottimizzazione dei valori NULL e 0 per tutti i tipi di dati in modo che non occupino alcun byte.  
  
## <a name="how-row-compression-affects-storage"></a>Influenza della compressione di riga sull'archiviazione  
 Nella tabella seguente è descritto il modo in cui la compressione di riga influisce sui tipi esistenti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]. Nella tabella non sono inclusi i risparmi in termini di spazio che possono essere ottenuti usando la compressione di pagina.  
  
|Tipo di dati|Influenza sull'archiviazione|Descrizione|  
|---------------|--------------------------|-----------------|  
|**tinyint**|No|Lo spazio di archiviazione minimo necessario è 1 byte.|  
|**smallint**|Sì|Se il valore può essere archiviato in 1 byte, verrà utilizzato solo 1 byte.|  
|**int**|Sì|Utilizza solo i byte necessari. Ad esempio, se un valore può essere archiviato in 1 byte, verrà utilizzato solo 1 byte.|  
|**bigint**|Sì|Utilizza solo i byte necessari. Ad esempio, se un valore può essere archiviato in 1 byte, verrà utilizzato solo 1 byte.|  
|**decimal**|Sì|USA solo i byte necessari, indipendentemente dalla precisione specificata. Se, ad esempio, un valore può essere archiviato in 3 byte, l'archiviazione avrà solo 3 byte. Il footprint di archiviazione è esattamente uguale al formato di archiviazione vardecimal.|  
|**numeric**|Sì|USA solo i byte necessari, indipendentemente dalla precisione specificata. Se, ad esempio, un valore può essere archiviato in 3 byte, l'archiviazione avrà solo 3 byte. Il footprint di archiviazione è esattamente uguale al formato di archiviazione vardecimal.|  
|**bit**|Sì|L'overhead di metadati aumenta questo valore a 4 bit.|  
|**smallmoney**|Sì|Rappresenta i dati Integer utilizzando un numero intero di 4 byte. Il valore della valuta viene moltiplicato per 10.000 e il valore intero risultante viene archiviato rimuovendo tutte le cifre dopo il separatore decimale. e un'ottimizzazione dell'archiviazione analoga a quella associata ai tipi Integer.|  
|**money**|Sì|Rappresenta i dati Integer utilizzando un Integer a 8 byte. Il valore della valuta viene moltiplicato per 10.000 e il valore intero risultante viene archiviato rimuovendo tutte le cifre dopo il separatore decimale. A questo tipo sono associati un intervallo maggiore rispetto a **smallmoney**. e un'ottimizzazione dell'archiviazione analoga a quella associata ai tipi Integer.|  
|**float**|Sì|Il byte meno significativi con zeri non sono archiviati. La compressione **float** si applica soprattutto per valori non frazionari in mantissa.|  
|**real**|Sì|Il byte meno significativi con zeri non sono archiviati. La compressione **real** si applica soprattutto per valori non frazionari in mantissa.|  
|**smalldatetime**|No|Rappresenta i dati Integer usando due numeri interi a 2 byte. Per una data, sono necessari due 2 byte. La data rappresenta il numero di giorni dall'1/1/1901. Poiché a partire dal 1902 è necessario utilizzare 2 byte, dopo tale data non viene ottenuto alcun risparmio in termini di spazio.<br /><br /> L'ora rappresenta il numero di minuti a partire dalla mezzanotte. Per i valori di ora appena successivi alle 04.00, viene utilizzato il secondo byte.<br /><br /> Se un tipo di dati **smalldatetime** viene usato solo per rappresentare una data (caso comune), l'ora è 0.0. La compressione consente di risparmiare 2 byte archiviando l'ora nel formato con byte più significativo per la compressione di riga.|  
|**datetime**|Sì|Rappresenta i dati Integer utilizzando due numeri interi di 4 byte. Il numero intero rappresenta il numero di giorni con data di base 1/1/1900. I primi 2 byte possono rappresentare gli anni fino al 2079. In questo caso la compressione consente di risparmiare sempre 2 byte fino a quella data. Ogni valore intero rappresenta 3,33 millisecondi. Poiché la compressione esaurisce i primi 2 byte nei i primi cinque minuti e deve utilizzare il quarto byte dopo le 16.00, a partire da tale ora è possibile risparmiare solo 1 byte in termini di spazio. Quando **datetime** viene compresso come qualsiasi altro numero intero, la compressione consente di risparmiare 2 byte nell'archiviazione della data.|  
|**date**|No|Rappresenta i dati Integer usando 3 byte. In questo modo è possibile rappresentare la data a partire dall'1/1/0001. Poiché per le date contemporanee la compressione di riga utilizza tutti i 3 byte, non viene ottenuto alcun risparmio in termini di spazio.|  
|**time**|No|Rappresenta i dati Integer usando da 3 a 6 byte. Sono disponibili diversi valori di precisione, da 0 a 9, rappresentabili con un numero di byte compreso tra 3 e 6. Lo spazio compresso viene utilizzato nel modo seguente:<br /><br /> **Precisione = 0. Byte = 3**. Ogni valore intero rappresenta un secondo. La compressione può rappresentare le ore fino alle 18.00 utilizzando 2 byte, con un risparmio potenziale di 1 byte.<br /><br /> **Precisione = 1. Byte = 3**. Ogni valore intero rappresenta 1/10 secondi. Poiché la compressione utilizza il terzo byte prima delle 02.00, il risparmio in termini di spazio è ridotto.<br /><br /> **Precisione = 2. Byte = 3**. Caso analogo al precedente. È improbabile ottenere un risparmio.<br /><br /> **Precisione = 3. Byte = 4**. Poiché i primi 3 byte vengono utilizzati prima delle 05.00, il risparmio ottenuto è ridotto.<br /><br /> **Precisione = 4. Byte = 4**. Poiché i primi 3 byte vengono utilizzati nei primi 27 secondi, non viene ottenuto alcun risparmio in termini di spazio.<br /><br /> **Precisione = 5, Byte = 5**. Il quinto byte verrà utilizzato dopo le 12.00.<br /><br /> **Precisione = 6 e 7, Byte = 5**. Non viene ottenuto alcun risparmio in termini di spazio.<br /><br /> **Precisione = 8, Byte = 6**. Il sesto byte verrà utilizzato dopo le 03.00.<br /><br /> <br /><br /> La compressione di riga non comporta alcuna modifica nell'archiviazione. In generale, se si comprime il tipo di dati **time** , non è possibile prevedere un risparmio significativo in termini di spazio.|  
|**datetime2**|Sì|Rappresenta i dati Integer utilizzando un numero di byte compreso tra 6 e 9. I primi 4 byte rappresentano la data. I byte utilizzati per rappresentare l'ora dipenderanno dalla precisione dell'ora specificata.<br /><br /> Il valore intero rappresenta il numero di giorni a partire dall'1/1/0001. Il limite superiore è la data del 31/12/9999. Per rappresentare una data nell'anno 2005, la compressione utilizza 3 byte.<br /><br /> Non viene ottenuto alcun risparmio nella rappresentazione dell'ora, poiché è consentito l'utilizzo di un numero di byte compreso tra 2 e 4 per valori di precisione dell'ora diversi. Di conseguenza, per rappresentare l'ora con valore di precisione di un secondo, la compressione utilizza 2 byte e il secondo byte viene utilizzato dopo 255 secondi.|  
|**datetimeoffset**|Sì|Tipo simile a **datetime2**, a parte il fatto che in questo caso sono disponibili 2 byte per il fuso orario in formato (HH.MM).<br /><br /> Analogamente a **datetime2**, la compressione consente di risparmiare 2 byte.<br /><br /> Per i valori del fuso orario, il valore MM potrebbe essere uguale a 0 per la maggior parte dei casi. Di conseguenza, la compressione consente di risparmiare 1 byte.<br /><br /> La compressione di riga non apporta alcuna modifica all'archiviazione.|  
|**char**|Sì|I caratteri di riempimento finali vengono rimossi. Si noti che in [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene inserito lo stesso carattere di riempimento indipendentemente dalle regole di confronto utilizzate.|  
|**varchar**|No|Nessun effetto.|  
|**text**|No|Nessun effetto.|  
|**nchar**|Sì|I caratteri di riempimento finali vengono rimossi. Si noti che in [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene inserito lo stesso carattere di riempimento indipendentemente dalle regole di confronto utilizzate.|  
|**nvarchar**|No|Nessun effetto.|  
|**ntext**|No|Nessun effetto.|  
|**binary**|Sì|Gli zero finali vengono rimossi.|  
|**varbinary**|No|Nessun effetto.|  
|**image**|No|Nessun effetto.|  
|**cursor**|No|Nessun effetto.|  
|**timestamp** / **rowversion**|Sì|Rappresenta i dati Integer utilizzando 8 byte. È disponibile un contatore timestamp gestito per ogni database, il cui valore iniziale è pari a 0. È possibile comprimere questo tipo in modo analogo a qualsiasi altro valore intero.|  
|**sql_variant**|No|Nessun effetto.|  
|**uniqueidentifier**|No|Nessun effetto.|  
|**tabella**|No|Nessun effetto.|  
|**xml**|No|Nessun effetto.|  
|Tipi definiti dall'utente|No|Rappresentati internamente come **varbinary**.|  
|FILESTREAM|No|Rappresentati internamente come **varbinary**.|  
  
## <a name="see-also"></a>Vedere anche  
 [Compressione dei dati](../../relational-databases/data-compression/data-compression.md)   
 [Implementazione della compressione di pagina](../../relational-databases/data-compression/page-compression-implementation.md)  
  
  
