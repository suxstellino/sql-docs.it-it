---
description: Impostazioni del progetto (mapping dei tipi) (MySQLToSQL)
title: Impostazioni progetto (mapping dei tipi) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 136fdf6d-657f-447b-af41-49bbc6e0e93e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a2f3e077a93d90afccf05023e41f8e5f2ea6fabc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017321"
---
# <a name="project-settings-type-mapping-mysqltosql"></a>Impostazioni del progetto (mapping dei tipi) (MySQLToSQL)
Le impostazioni del progetto di mapping dei tipi consentono di impostare i mapping dei tipi predefiniti per il progetto SSMA.  

Il mapping dei tipi è disponibile nelle finestre di dialogo Impostazioni progetto e impostazioni progetto predefinite:  
  
-   Utilizzare la finestra di dialogo Impostazioni progetto per impostare le opzioni di configurazione per il progetto corrente. Per accedere alle impostazioni del mapping dei tipi, scegliere Impostazioni progetto dal menu strumenti e quindi fare clic su mapping dei tipi nel riquadro sinistro.  
  
-   Utilizzare la finestra di dialogo Impostazioni progetto predefinite per impostare le opzioni di configurazione per tutti i progetti. Per accedere alle impostazioni del mapping dei tipi, nel menu Strumenti selezionare Impostazioni progetto predefinite, selezionare il tipo di progetto di migrazione per cui è necessario visualizzare le impostazioni/changed dall'elenco a discesa **versione destinazione migrazione** e quindi fare clic su mapping dei tipi nel riquadro sinistro.  
  
## <a name="options"></a>Opzioni  
  
##### <a name="source-type"></a>Tipo di origine  
Si tratta del tipo di dati MySQL, di cui è necessario eseguire il mapping al tipo di dati del database di destinazione.  
  
##### <a name="target-type"></a>Tipo destinazione  
Tipo di dati del database di destinazione per il tipo di dati MySQL specificato.  
  
##### <a name="add"></a>Add  
Fare clic su questo pulsante per aggiungere un tipo di dati all'elenco di mapping.  
  
##### <a name="edit"></a>Modifica  
Fare clic per modificare il tipo di dati selezionato nell'elenco mapping.  
  
##### <a name="remove"></a>Rimuovi  
Fare clic per rimuovere il mapping dei tipi di dati selezionati dall'elenco mapping.  
  
##### <a name="reset-to-default"></a>Ripristina predefiniti  
Fare clic per reimpostare l'elenco dei mapping dei tipi sui valori predefiniti di SSMA.  
  
## <a name="type-mappings"></a>Mapping dei tipi  
La tabella seguente illustra il mapping predefinito tra i tipi di dati di origine e di destinazione  
  
|Tipo di dati MySQL|Tipo di dati di SQL Server|  
|-|-|  
|bigint|bigint|  
|bigint [*.. 255]|bigint|  
|BINARY|binario [1]|  
|binario [0.. 1]|binario [1]|  
|binario [2.. 255]|binario [*]|  
|bit|binario [1]|  
|bit [0.. 8]|binario [1]|  
|bit [17.. 24]|binario [3]|  
|bit [25.. 32]|binario [4]|  
|bit [33.. 40]|binario [5]|  
|bit [41.. 48]|binario [6]|  
|bit [49.. 56]|binario [7]|  
|bit [57.. 64]|binario [8]|  
|bit [9.. 16]|binario [2]|  
|blob|varbinary(max)|  
|BLOB [0.. 1]|varbinary [1]|  
|BLOB [2.. 8000]|varbinary [*]|  
|BLOB [8001.. *]|varbinary(max)|  
|bool|bit|  
|boolean|bit|  
|char|nchar [1]|  
|byte char|binario [1]|  
|byte Char [0.. 1]|binario [1]|  
|byte Char [2.. 255]|binario [*]|  
|Char [0.. 1]|nchar [1]|  
|Char [2... 255]|nchar [*]|  
|character|nchar [1]|  
|carattere variabile [0.. 1]|nvarchar [1]|  
|carattere variabile [2.. 255]|NVARCHAR|  
|carattere [0.. 1]|nchar [1]|  
|carattere [2.. 255]|nchar [*]|  
|Data|Data|  
|Datetime|datetime2 [0]|  
|dec|decimal|  
|Dec [*... 65]|Decimal [*] [0]|  
|Dec [*... 65] [ \* .. 30|Decimal [*] [ \* ]|  
|decimal|decimal|  
|decimale [*.. 65]|Decimal [*] [0]|  
|decimale [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|double|float [53]|  
|double precision|float [53]|  
|precisione doppia [*.. 255] [ \* .. 30|numeric [*] [ \* ]|  
|doppio [*... 255] [ \* .. 30|numeric [*] [ \* ]|  
|fixed|NUMERIC|  
|correzione di [*... 65] [ \* .. 30|numeric [*] [ \* ]|  
|float|float [24]|  
|float [*.. 255] [ \* .. 30|numeric [*] [ \* ]|  
|float [*.. 53]|float [53]|  
|INT|INT|  
|int [*... 255]|INT|  
|numero intero|INT|  
|intero [*... 255]|INT|  
|longblob|varbinary(max)|  
|longtext|nvarchar(max)|  
|mediumblob|varbinary(max)|  
|mediumint|INT|  
|MEDIUMINT [*.. 255]|INT|  
|mediumtext|nvarchar(max)|  
|carattere nazionale|nchar [1]|  
|char nazionale [0.. 1]|nchar [1]|  
|char nazionale [2.. 255]|nchar [*]|  
|carattere nazionale|nchar [1]|  
|carattere nazionale variabile|nvarchar [1]|  
|carattere nazionale variabile [0.. 1]|nvarchar [1]|  
|carattere nazionale variabile [2.. 4000]|nvarchar [*]|  
|carattere nazionale variabile [4001.. *]|nvarchar(max)|  
|carattere nazionale [0.. 1]|nchar [1]|  
|carattere nazionale [2.. 255]|nchar [*]|  
|varchar nazionale|nvarchar [1]|  
|varchar nazionale [0.. 1]|nvarchar [1]|  
|varchar nazionale [2.. 4000]|nvarchar [*]|  
|varchar nazionale [4001.. *]|nvarchar(max)|  
|NCHAR|nchar [1]|  
|nchar varchar|nvarchar [1]|  
|nchar varchar [0.. 1]|nvarchar [1]|  
|nchar varchar [2.. 4000]|nvarchar [*]|  
|nchar varchar [4001.. *]|nvarchar(max)|  
|nchar [0.. 1]|nchar [1]|  
|nchar [2.. 255]|nchar [*]|  
|NUMERIC|NUMERIC|  
|numerico [*.. 65]|numeric [*] [0]|  
|numerico [*.. 65] [ \* .. 30|numeric [*] [ \* ]|  
|NVARCHAR|nvarchar [1]|  
|nvarchar [0.. 1]|nvarchar [1]|  
|nvarchar [2.. 4000]|nvarchar [*]|  
|nvarchar [4001.. *]|nvarchar(max)|  
|real|float [53]|  
|Real [*... 255] [ \* .. 30|numeric [*] [ \* ]|  
|serial|bigint|  
|SMALLINT|SMALLINT|  
|smallint [*.. 255]|SMALLINT|  
|text|nvarchar(max)|  
|testo [0.. 1]|nvarchar [1]|  
|testo [2.. 4000]|nvarchar [*]|  
|testo [4001.. *]|nvarchar(max)|  
|time|time|  
|timestamp|Datetime|  
|tinyblob|varbinary [255]|  
|TINYINT|smallint|  
|tinyint [*... 255]|SMALLINT|  
|tinytext|nvarchar [255]|  
|bigint senza segno|bigint|  
|bigint senza segno [*.. 255]|bigint|  
|Dec senza segno|decimal|  
|unsigned Dec [*.. 65]|Decimal [*] [0]|  
|unsigned Dec [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|decimale senza segno|decimal|  
|unsigned Decimal [*.. 65]|Decimal [*] [0]|  
|unsigned Decimal [*.. 65] [ \* .. 30|Decimal [*] [ \* ]|  
|Double senza segno|float [53]|  
|precisione doppia senza segno|float [53]|  
|precisione doppia senza segno [*.. 255] [ \* .. 30|numeric [*] [ \* ]|  
|Double senza segno [*.. 255] [ \* .. 30|numeric [*] [ \* ]|  
|senza segno fisso|NUMERIC|  
|senza segno (fixed) [*.. 65] [ \* .. 30|numeric [*] [ \* ]|  
|float senza segno|float [24]|  
|float senza segno [*.. 255] [ \* .. 30|numeric [*] [ \* ]|  
|float senza segno [*.. 53]|float [53]|  
|int senza segno|bigint|  
|unsigned int [*.. 255]|bigint|  
|Unsigned Integer|bigint|  
|Unsigned Integer [*.. 255]|bigint|  
|unsigned MEDIUMINT|INT|  
|unsigned MEDIUMINT [*.. 255]|INT|  
|numerico senza segno|NUMERIC|  
|numerico senza segno [*.. 65]|numeric [*] [0]|  
|numerico senza segno [*.. 65] [ \* .. 30|numeric [*] [ \* ]|  
|Real senza segno|float [53]|  
|Real senza segno [*.. 255 [[ \* .. 30|numeric [*] [ \* ]|  
|smallint senza segno|INT|  
|smallint senza segno [*.. 255]|INT|  
|tinyint senza segno|TINYINT|  
|tinyint senza segno [*.. 255]|TINYINT|  
|varbinary [0.. 1]|varbinary [1]|  
|varbinary [2.. 8000]|varbinary [*]|  
|varbinary [8001.. *]|varbinary(max)|  
|varchar [0.. 1]|nvarchar [1]|  
|varchar [2.. 4000]|nvarchar [*]|  
|varchar [4001.. *]|nvarchar(max)|  
|anno|SMALLINT|  
|anno [2.. 2]|SMALLINT|  
|anno [4.. 4]|SMALLINT|  
  
##### <a name="add"></a>Add  
Fare clic su questo pulsante per aggiungere un tipo di dati all'elenco di mapping.  
  
##### <a name="edit"></a>Modifica  
Fare clic per modificare un tipo di dati nell'elenco mapping.  
  
##### <a name="remove"></a>Rimuovi  
Fare clic per rimuovere il mapping dei tipi di dati selezionati dall'elenco mapping.  
  
##### <a name="reset-to-default"></a>Ripristina predefiniti  
Fare clic per reimpostare tutti i mapping dei tipi di dati sulle impostazioni predefinite SSMA.  
  
