---
title: Mapping di MySQL e set di caratteri SQL Server (MySQLToSQL) | Microsoft Docs
description: Informazioni su come specificare un set di caratteri per i tipi di dati carattere MySQL, le espressioni e i valori letterali da usare durante la conversione del tipo di dati character da SSMA per MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 20b3f22e-16a2-4a87-b4eb-c277be6bf5c8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 13b2accd6b4dc2ba2bbd8c9c1e6db312650d2d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077622"
---
# <a name="mapping-mysql-and-sql-server-character-set-mysqltosql"></a>Mapping dei set di caratteri MySQL e SQL Server (MySQLToSQL)
È possibile specificare il set di caratteri (charset) per i tipi di dati carattere MySQL, le espressioni e i valori letterali.  
  
## <a name="charset-mapping"></a>Mapping del set di caratteri  
Il mapping del set di caratteri viene definito per ogni set di caratteri MySQL e utilizzato durante la conversione del tipo di dati character Specifica la modalità di conversione dei tipi di dati stringa di caratteri di un determinato set di caratteri:  
  
-   Ai tipi di carattere SQL Server nazionali (NCHAR/NVARCHAR) o  
  
-   Ai tipi di carattere SQL Server regolari (CHAR/VARCHAR)  
  
1.  i tipi di dati dei caratteri di database di destinazione **nazionali** sono:  
  
    1.  **nchar**  
  
    2.  **nvarchar**  
  
2.  i tipi di dati dei caratteri di destinazione **regolari** del database sono:  
  
    1.  **char**  
  
    2.  **varchar**  
  
3.  Il mapping dei tipi consente solo il mapping ai tipi di dati di tipo carattere **nazionale** . Dopo che il tipo di dati carattere MySQL viene convertito in base al mapping dei tipi, viene applicato il mapping del set di caratteri.  
  
> [!NOTE]  
> È possibile definire il mapping del set di caratteri a ogni livello di nodo di Esplora oggetti di metadati e rappresentare tutti i set di caratteri letti da MySQL.  
  
## <a name="charset-mapping-on-different-node-levels"></a>Mapping del set di caratteri in diversi livelli di nodo  
Il mapping del set di caratteri varia a seconda dei livelli di nodo, ovvero:  
  
1.  A livello del nodo dei metadati radice  
  
2.  A livello di database, categoria e nodi oggetto  
  
> [!NOTE]  
> La scheda selezionata per modificare il mapping del set di caratteri contiene tre pulsanti, indipendentemente dal mapping dei diversi livelli di nodo.  
>   
> Ad esempio:  
>   
> 1.  **Applica:** Applica le modifiche apportate dall'utente, abilitato solo quando il mapping del set di caratteri è stato modificato e non è ancora stato salvato.  
> 2.  **Annulla:** Annulla le modifiche apportate dall'utente. Il pulsante viene abilitato quando il mapping del set di caratteri viene modificato ma non salvato.  
> 3.  **Ripristina impostazioni predefinite:** Reimposta tutti i mapping sui valori predefiniti.  
  
1.  **A livello del nodo dei metadati radice:**  La griglia di mapping charset contiene la griglia charset con una colonna separata per ogni set di caratteri. Le colonne della griglia sono:  
  
    1.  La prima colonna della griglia denominata **CharSet nome** contiene il nome del set di caratteri.  
  
    2.  Il secondo nome **CharSet Description** contiene la descrizione del set di caratteri.  
  
    3.  La terza colonna denominata, il **tipo di set di caratteri di destinazione** contiene impostazioni di mapping per un set di caratteri specifico. I valori per questa colonna sono:  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
    > [!IMPORTANT]  
    > I valori predefiniti per un set di caratteri specifico hanno il prefisso ' (default)' dopo CHAR/VARCHAR o NCHAR/NVARCHAR.  
  
    Di seguito è riportato il mapping del set di caratteri tra il database MySQL e il database di destinazione sul livello del nodo dei metadati radice:  
  
    |Nome del set di caratteri|Descrizione charset|Tipo di set di caratteri di destinazione (impostazione predefinita)|  
    |-|-|-|  
    |Big5|Big5 cinese tradizionale|NCHAR/NVARCHAR (impostazione predefinita)|  
    |dec8|DICEMBRE Europa occidentale|CHAR/VARCHAR (impostazione predefinita)|  
    |cp850|DOS Europa occidentale|CHAR/VARCHAR (impostazione predefinita)|  
    |hp8|HP ovest Europa|CHAR/VARCHAR (impostazione predefinita)|  
    |koi8r|KOI8-R relcom russo|CHAR/VARCHAR (impostazione predefinita)|  
    |Latino 1|CP1252 Europa occidentale|CHAR/VARCHAR (impostazione predefinita)|  
    |Latin2|ISO 8859-2 Europa centrale|CHAR/VARCHAR (impostazione predefinita)|  
    |swe7|7bit svedese|CHAR/VARCHAR (impostazione predefinita)|  
    |ascii|US-ASCII|CHAR/VARCHAR (impostazione predefinita)|  
    |ujis|EUC-JP giapponese|NCHAR/NVARCHAR (impostazione predefinita)|  
    |SJIS|MAIUSC-JIS giapponese|NCHAR/NVARCHAR (impostazione predefinita)|  
    |Ebraico|ISO 8859-8 Ebraico|CHAR/VARCHAR (impostazione predefinita)|  
    |tis620|TIS620 Thai|CHAR/VARCHAR (impostazione predefinita)|  
    |euckr|EUC-KR coreano|NCHAR/NVARCHAR (impostazione predefinita)|  
    |koi8u|KOI8-U ucraino|CHAR/VARCHAR (impostazione predefinita)|  
    |GB2312|GB2312 cinese semplificato|NCHAR/NVARCHAR (impostazione predefinita)|  
    |greek|ISO 8859-7 Greco|CHAR/VARCHAR (impostazione predefinita)|  
    |CP 1250|Europa centrale di Windows|CHAR/VARCHAR (impostazione predefinita)|  
    |GBK|GBK cinese semplificato|NCHAR/NVARCHAR (impostazione predefinita)|  
    |latin5|ISO 8859-9 Turco|CHAR/VARCHAR (impostazione predefinita)|  
    |armscii8|ARMSCII-8 armeno|CHAR/VARCHAR (impostazione predefinita)|  
    |utf8|UTF-8 Unicode|NCHAR/NVARCHAR (impostazione predefinita)|  
    |ucs2|UCS-2 Unicode|NCHAR/NVARCHAR (impostazione predefinita)|  
    |CP866|DOS russo|CHAR/VARCHAR (impostazione predefinita)|  
    |keybcs2|Czech-Slovak DOS Kamenicky|CHAR/VARCHAR (impostazione predefinita)|  
    |macce|Mac centrale Europa|CHAR/VARCHAR (impostazione predefinita)|  
    |macro|Mac occidentale Europa|CHAR/VARCHAR (impostazione predefinita)|  
    |CP852|Europa centrale (DOS)|CHAR/VARCHAR (impostazione predefinita)|  
    |latin7|ISO 8859-13 Baltico|CHAR/VARCHAR (impostazione predefinita)|  
    |CP 1251|Windows Cirillico|CHAR/VARCHAR (impostazione predefinita)|  
    |CP 1256|Windows arabo|CHAR/VARCHAR (impostazione predefinita)|  
    |CP 1257|Baltico di Windows|CHAR/VARCHAR (impostazione predefinita)|  
    |BINARY|Pseudo charset binario|CHAR/VARCHAR (impostazione predefinita)|  
    |geostd8|Georgiano GEOSTD8|CHAR/VARCHAR (impostazione predefinita)|  
    |cp932|SJIS per Windows Japanese|NCHAR/NVARCHAR (impostazione predefinita)|  
    |eucjpms|UJIS per Windows Japanese|NCHAR/NVARCHAR (impostazione predefinita)|  
  
2.  **A livello di database, categoria o nodo oggetto:** Nel livello di nodo database, categoria o oggetto, la griglia di mapping charset contiene le stesse righe del livello del nodo dei metadati radice, vale a dire:  
  
    1.  La prima colonna della griglia denominata, il **nome del set di caratteri** contiene il nome del set di caratteri.  
  
    2.  La seconda colonna denominata set di **caratteri Descrizione** contiene la descrizione del set di caratteri.  
  
    3.  L'unica differenza è rappresentata dai valori della terza colonna della griglia. La terza colonna denominata, il **tipo di dati di destinazione** contiene le impostazioni di mapping per un set di caratteri specifico. I valori per la colonna sono i seguenti:  
  
        -   Ereditato (CHAR/VARCHAR o NCHAR/NVARCHAR)  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
> [!IMPORTANT]  
> -   Nel mapping dei set di caratteri tra il database MySQL e il database di destinazione sui livelli di database, categoria e nodo oggetto, i valori predefiniti per un set di caratteri specifico in ogni livello diverso dalla radice per il **tipo di dati di destinazione** della colonna devono essere "ereditato".  
> -   Nella griglia il valore **ereditato** è suffisso con ' (char/varchar)' o ' (nchar/nvarchar)', a seconda del valore ereditato dall'elemento padre da questo particolare set di caratteri.  
  
