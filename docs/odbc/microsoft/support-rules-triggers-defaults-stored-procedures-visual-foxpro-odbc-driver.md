---
description: Supporto di regole, trigger, valori predefiniti e stored procedure (driver ODBC Visual FoxPro)
title: Supporto per regole, trigger, valori predefiniti e stored procedure | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Visual FoxPro ODBC driver [ODBC], stored procedures
- FoxPro ODBC driver [ODBC], commands and functions
- commands for FoxPro ODBC driver [ODBC]
- FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], triggers
- stored procedures [ODBC], Visual FoxPro ODBC driver
- Visual FoxPro ODBC driver [ODBC], rules
- FoxPro commands and functions [ODBC]
- rules [ODBC]
- Visual FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], rules
- functions [ODBC], Visual FoxPro
- default values [ODBC]
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro ODBC driver [ODBC], triggers
- FoxPro ODBC driver [ODBC], stored procedures
- Visual FoxPro commands and functions [ODBC]
ms.assetid: e449de20-d6ca-4902-9f8e-814eb6e86650
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea801de44e1fce175ac549402e17b9fcc2e9f020
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061126"
---
# <a name="support-for-rules-triggers-default-values-and-stored-procedures-visual-foxpro-odbc-driver"></a>Supporto di regole, trigger, valori predefiniti e stored procedure (driver ODBC Visual FoxPro)
Non è possibile creare regole, trigger, valori predefiniti o stored procedure Visual FoxPro utilizzando il driver ODBC Visual FoxPro. Tuttavia, l'applicazione potrebbe interagire con regole, trigger, valori predefiniti o stored procedure esistenti durante l'inserimento, l'aggiornamento o l'eliminazione dei dati Visual FoxPro archiviati in un database.  
  
 Nella tabella seguente sono elencati i comandi e le funzioni di Visual FoxPro supportati dal driver ODBC Visual FoxPro quando i comandi o le funzioni sono presenti in regole, trigger, valori predefiniti o stored procedure.  
  
 Se l'applicazione interagisce con i dati le cui regole, trigger, valori predefiniti o stored procedure chiamano qualsiasi altra funzione o comando Visual FoxPro, il driver genera un errore. Vedere [comandi e funzioni Visual FoxPro](../../odbc/microsoft/unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver.md) non supportati per un elenco di comandi e funzioni non supportati dal driver.  
  
> [!TIP]  
>  Se si desidera inserire codice condizionale nelle regole, nei trigger o nelle stored procedure che determinano i comandi da eseguire quando vengono chiamati dal driver, è possibile utilizzare la funzione **Version ()** . La funzione **Version ()** restituisce "Visual FoxPro ODBC driver *\<version>* " quando viene chiamato dal driver.  
  
## <a name="visual-foxpro-commands-and-functions-supported-in-rules-triggers-default-values-and-stored-procedures"></a>Comandi e funzioni di Visual FoxPro supportati in regole, trigger, valori predefiniti e stored procedure  

:::row:::
    :::column:::
        Operatore $  
        Operatore %  
    :::column-end:::
    :::column:::
        Comando &  
        Comando &&   
    :::column-end:::
    :::column:::
        \* Command  
        = (Comando)  
    :::column-end:::
:::row-end:::

## <a name="a"></a>A  

:::row:::
    :::column:::
        Funzione ABS ()  
        Funzione ACOPY ()  
        Funzione ARCCOS ()  
        Funzione ADATABASES ()  
        Funzione ADBOBJECTS ()  
        Comando Aggiungi tabella  
        Funzione ADEL ()  
        Funzione AELEMENT ()  
        Funzione AERROR ()  
        ()-Funzione  
        Funzione AINS ()  
        Funzione ALEN ()  
        Funzione ALIAS ()  
    :::column-end:::
    :::column:::
        Funzione ALLTRIM ()  
        ALTER TABLE (comando SQL)  
        Operatore AND  
        Comando APPEND  
        Comando ACCODA da  
        Comando ACCODA da matrice  
        ACCODA comando generale  
        Comando APPEND MEMO  
        Comando APPEND PROCEDURES  
        Funzione ASC ()  
        Funzione ASCAN ()  
        ASIN () (funzione)  
        Funzione ASORT ()  
    :::column-end:::
    :::column:::
        Funzione ASUBSCRIPT ()  
        AT () (funzione)  
        Funzione AT_C ()  
        Funzione ATAN ()  
        Funzione ATC ()  
        Funzione ATCC ()  
        Funzione ATCLINE ()  
        Funzione ATLINE ()  
        Funzione ATN2 ()  
        Funzione AUSED ()  
        MEDIA comando  
    :::column-end:::
:::row-end:::

## <a name="b"></a>B  

:::row:::
    :::column:::
        Comando BEGIN TRANSACTION  
        Funzione BETWEEN ()  
        Funzione BITAND ()  
        Funzione BITCLEAR ()  
        Funzione BITLSHIFT ()  
    :::column-end:::
    :::column:::
        Funzione BITNOT ()  
        Funzione BITOR ()  
        Funzione BITRSHIFT ()  
        Funzione BITSt ()  
        Funzione BITTEST ()  
    :::column-end:::
    :::column:::
        Funzione BITXOR ()  
        Comando BLANK  
        Funzione BOF ()  
    :::column-end:::
:::row-end:::

## <a name="c"></a>C  

:::row:::
    :::column:::
        Comando CALCULATE  
        Funzione CANDIDAta ()  
        Funzione CDOW ()  
        Funzione CDX ()  
        Funzione CEILING ()  
        Funzione CHR ()  
        Funzione CHRTRAN ()  
        Funzione CHRTRANC ()  
        Chiudi comandi  
        Funzione CMONTH ()  
    :::column-end:::
    :::column:::
        Comando CONTINUE  
        Comando copia indici  
        Comando COPY PROCEDURES  
        Comando Copia struttura  
        COPY STRUCTURE-comando esteso  
        Comando COPY TAG  
        COPIA in comando  
        Comando COPY TO ARRAY  
        Funzione COS ()  
        Comando COUNT  
    :::column-end:::
    :::column:::
        Funzione CPCONVERT ()  
        Funzione CPCURRENT ()  
        Funzione CPDBF ()  
        Funzione CTOD ()  
        Funzione CTOT ()  
        Funzione CURSORGETPROP ()  
        Funzione CURSORSETPROP ()  
        Funzione curvy ()  
    :::column-end:::
:::row-end:::

## <a name="d"></a>D  

:::row:::
    :::column:::
        Funzione DATE ()  
        DATETIME () (funzione)  
        Funzione DAY ()  
        Funzione DBC ()  
        Funzione DBF ()  
        Funzione DBGETPROP ()  
        Funzione DBUSed ()  
        DELETE (comando SQL)  
    :::column-end:::
    :::column:::
        Comando DELETE  
        DELETE TAG (comando)  
        Funzione DELETED ()  
        Funzione Descending ()  
        Funzione DIFFERENCE ()  
        Comando DIMENSION  
        Funzione DISKSPACE ()  
        Funzione DMY ()  
    :::column-end:::
    :::column:::
        Comando DO  
        FAI CASO... Comando ENDCASE  
        ESEGUI... Comando ENDDO  
        Funzione DOW ()  
        Funzione DTOC ()  
        Funzione DTOR ()  
        Funzione DTO ()  
        Funzione DTOT ()  
    :::column-end:::
:::row-end:::

## <a name="e"></a>E  

:::row:::
    :::column:::
        Funzione EMPTY ()  
        Comando END TRANSACTION  
        Funzione EOF ()  
    :::column-end:::
    :::column:::
        Funzione ERROR ()  
        Funzione EVALUATE ()  
        Comando EXIT  
    :::column-end:::
    :::column:::
        Funzione EXP ()  
    :::column-end:::
:::row-end:::

## <a name="f"></a>F  

:::row:::
    :::column:::
        Funzione FCOUNT ()  
        Funzione FDATE ()  
        Funzione FIELD ()  
        Funzione FILE ()  
        Funzione FILTER ()  
        Funzione FLDLIST ()  
    :::column-end:::
    :::column:::
        Funzione FLOCK ()  
        Funzione FLOOR ()  
        Scarica comando  
        Funzione FOR ()  
        PER... Comando ENDFOR  
        Funzione FOUND ()  
    :::column-end:::
    :::column:::
        Comando tabella gratuita  
        Funzione FSIZE ()  
        Funzione FTIME ()  
        FULLPATH () (funzione)  
        Comando FUNCTION  
        Funzione FV ()  
    :::column-end:::
:::row-end:::

## <a name="g"></a>G  

:::row:::
    :::column:::
        Raccogli comando  
        Funzione GETCP ()  
        Funzione GETENV ()  
    :::column-end:::
    :::column:::
        Funzione GETFLDSTATE ()  
        Funzione GETNEXTMODIFIED ()  
        Comando GO/GOTO  
    :::column-end:::
    :::column:::
        Funzione GOMONTH ()  
    :::column-end:::
:::row-end:::

## <a name="h"></a>H  

:::row:::
    :::column:::
        Funzione HEADER ()
    :::column-end:::
    :::column:::
        Funzione HOUR ()
    :::column-end:::
:::row-end:::

## <a name="i"></a>I  

:::row:::
    :::column:::
        Funzione IDXCOLLATE ()  
        SE... ENDIF (comando)  
        Funzione IIF ()  
        Funzione INDBC ()  
        INDEX (comando)  
        Funzione INLIST ()  
    :::column-end:::
    :::column:::
        Comando INSERT-SQL  
        INT () (funzione)  
        Funzione α ()  
        Funzione BLANK ()  
        Funzione undigit ()  
        Funzione unexclusive ()  
    :::column-end:::
    :::column:::
        Funzione ISLEADBYTE ()  
        Funzione LOWER ()  
        ISNULL () (funzione)  
        Funzione ISREADONLY ()  
        Funzione ToUpper ()  
    :::column-end:::
:::row-end:::

## <a name="k"></a>K  

:::row:::
    :::column:::
        Funzione KEY ()
    :::column-end:::
    :::column:::
        Funzione di combinazione di maiuscole ()
    :::column-end:::
:::row-end:::

## <a name="l"></a>L  

:::row:::
    :::column:::
        LEFT () (funzione)  
        Funzione LEFTC ()  
        Funzione LEN ()  
        Funzione LENC ()  
        LIKE () (funzione)  
        Funzione LIKEC ()  
    :::column-end:::
    :::column:::
        Comando locale  
        INDIVIDUA comando  
        Funzione LOCK ()  
        Funzione LOG ()  
        Funzione LOG10 ()  
        Funzione LOOKUP ()  
    :::column-end:::
    :::column:::
        LOWER ()-funzione  
        Comando LPARAMETERS  
        Funzione LTRIM ()  
        Funzione LUPDATE ()  
    :::column-end:::
:::row-end:::

## <a name="m"></a>M  

:::row:::
    :::column:::
        Funzione MAX ()  
        MDX () (funzione)  
        Funzione MDY ()  
        Funzione MEMLINES ()  
    :::column-end:::
    :::column:::
        Funzione MESSAGE ()  
        Funzione MIN ()  
        Funzione MINUTE ()  
        Variabile di memoria di sistema _MLINE  
    :::column-end:::
    :::column:::
        Funzione lineam ()  
        Funzione MOD ()  
        Funzione MONTH ()  
        Funzione MTON ()  
    :::column-end:::
:::row-end:::

## <a name="n"></a>N  

:::row:::
    :::column:::
        Funzione NDX ()  
        Funzione Normalize ()  
    :::column-end:::
    :::column:::
        Operatore NOT  
        Comando Nota  
    :::column-end:::
    :::column:::
        Funzione NTOM ()  
        Funzione NVL ()  
    :::column-end:::
:::row-end:::

## <a name="o"></a>O  

:::row:::
    :::column:::
        Funzione OCCURs ()  
        Funzione OLDVAL ()  
        Funzione ON ()  
    :::column-end:::
    :::column:::
        Comando ON ERROR  
        Comando tasto ON  
        Comando Apri DATABASE  
    :::column-end:::
    :::column:::
        Operatore OR  
        Funzione ORDER ()  
        Funzione OS ()  
    :::column-end:::
:::row-end:::

## <a name="p"></a>P  

:::row:::
    :::column:::
        Comando PACK  
        Funzioni PADL () &#124; PADR () &#124; PADC ()  
        Comando PARAMETERS  
        Funzione PARAMETERS ()  
        Funzione PAYMENT ()  
    :::column-end:::
    :::column:::
        Funzione PI ()  
        Funzione PRIMARY ()  
        Comando privato  
        Comando PROCEDURE  
        Funzione PROGRAM ()  
    :::column-end:::
    :::column:::
        Funzione PROPER ()  
        PUBLIC (comando)  
        Funzione PV ()  
    :::column-end:::
:::row-end:::

## <a name="r"></a>R  

:::row:::
    :::column:::
        Funzione RAND ()  
        Funzione RAT ()  
        Funzione RATC ()  
        Funzione RATLINE ()  
        RICHIAMA comando  
        Funzione RECCOUNT ()  
        Funzione RECNO ()  
        Funzione RECSIZE ()  
    :::column-end:::
    :::column:::
        Comando REGIONAL  
        Funzione RELATION ()  
        Comando Rimuovi tabella  
        SOSTITUISCi comando  
        Comando SOSTITUISCi da matrice  
        Funzione REPLICAte ()  
        Comando RETRY  
        RETURN (comando)  
    :::column-end:::
    :::column:::
        RIGHT () (funzione)  
        Funzione RIGHTC ()  
        Funzione RLOCK ()  
        ROLLBACK (comando)  
        Funzione ROUND ()  
        Funzione RTOD ()  
        Funzione RTRIM ()  
    :::column-end:::
:::row-end:::

## <a name="s"></a>S  

:::row:::
    :::column:::
        ANALIZZA... Comando ENDSCAN  
        SCATTER (comando)  
        Funzione SEC ()  
        Funzione SECONDS ()  
        Comando SEEK  
        Funzione SEEK ()  
        Comando SELECT  
        Funzione SELECT ()  
        Comando SELECT-SQL  
        Funzione SET ()  
        SET BLOCKSIZE (comando)  
        IMPOSTA comando CARRY  
        SET CENTURY (comando)  
        SET COLLATE (comando)  
        Comando SET DATABASE  
        Comando imposta data  
        IMPOSTA comando predefinito  
        SET DELETED (comando)  
        SET EXACT (comando)  
        SET EXCLUSIVE (comando)  
        Comando SET FDOW  
    :::column-end:::
    :::column:::
        Comando imposta campi  
        Comando Imposta filtro  
        IMPOSTA comando fisso  
        IMPOSTA comando FULLPATH  
        Comando SET FWEEK  
        Comando SET HOURs  
        SET INDEX (comando)  
        SET LOCK (comando)  
        IMPOSTA comando per più blocchi  
        IMPOSTA il comando NEAR  
        Comando SET NOCPTRANS  
        Imposta notifica comando  
        SET NULL (comando)  
        SET OPTIMIZe (comando)  
        Comando Imposta ordine  
        SET PATH (comando)  
        Comando SET PROCEDURE  
        SET RELATION (comando)  
        SET RELATION OFF-comando  
        SET REPROCESS (comando)  
        SET SKIP (comando)  
    :::column-end:::
    :::column:::
        Comando SET UDFPARMS  
        SET UNIQUE (comando)  
        Comando imposta VOLUME  
        Funzione SETFLDSTATE ()  
        Funzione SIGN ()  
        Funzione SIN ()  
        Comando SKIP  
        Ordina comando  
        Funzione SPACE ()  
        Funzione SQRT ()  
        Comando STORE  
        Funzione STR ()  
        Funzione STRCONV ()  
        Funzione STRTRAN ()  
        Funzione STUFF ()  
        Funzione STUFFC ()  
        Funzione substr ()  
        Funzione SUBSTRC ()  
        Comando SUM  
        Funzione SYS (2011)  
    :::column-end:::
:::row-end:::

## <a name="t"></a>T  

:::row:::
    :::column:::
        Funzione TABLEREVERT ()  
        Funzione TABLEUPDATE ()  
        Funzione TAG ()  
        Funzione TAGCOUNT ()  
        Funzione TAGNO ()  
        Variabile di memoria di sistema _TALLY  
    :::column-end:::
    :::column:::
        Funzione TAN ()  
        Funzione TARGET ()  
        Funzione TIME ()  
        Comando TOTAL  
        Variabile di memoria di sistema _TRIGGERLEVEL  
        Funzione TRIM ()  
    :::column-end:::
    :::column:::
        Funzione TTOC ()  
        Funzione TTOD ()  
        Funzione TXNLEVEL ()  
        Funzione TYPE ()  
    :::column-end:::
:::row-end:::

## <a name="u"></a>U  

:::row:::
    :::column:::
        UNIQUE () (funzione)  
        Comando di sblocco  
        Aggiorna comando  
    :::column-end:::
    :::column:::
        UPDATE (comando SQL)  
        Funzione UPPER ()  
        Comando USE  
    :::column-end:::
    :::column:::
        Funzione USED ()  
    :::column-end:::
:::row-end:::

## <a name="v"></a>V  

:::row:::
    :::column:::
        Funzione VAL ()
    :::column-end:::
    :::column:::
        Funzione VERSION ()
    :::column-end:::
:::row-end:::

## <a name="w"></a>W  

Funzione WEEK ()

## <a name="y"></a>S  

Funzione YEAR ()

## <a name="z"></a>Z  

Comando ZAP
