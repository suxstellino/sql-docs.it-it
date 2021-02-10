---
description: Comandi e funzioni Visual FoxPro non supportati (driver ODBC Visual FoxPro)
title: Funzioni e comandi Visual FoxPro non supportati | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- FoxPro ODBC driver [ODBC], commands and functions
- functions [ODBC], Visual FoxPro
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro commands and functions
- FoxPro ODBC driver
ms.assetid: afdb6b7e-738d-42ca-8053-67ae50873ca6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4ac7f25709cf91e03f9ad8b15f9fb0026ee5e98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015391"
---
# <a name="unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver"></a>Comandi e funzioni Visual FoxPro non supportati (driver ODBC Visual FoxPro)
Nella tabella seguente sono elencati i comandi e le funzioni di FoxPro che non sono supportati dal driver ODBC Visual FoxPro, ma sono supportati da Microsoft® Visual FoxPro®.  
  
 Se l'applicazione interagisce con i dati le cui regole, trigger, valori predefiniti o stored procedure chiamano questi comandi o funzioni Visual FoxPro, il driver può generare un errore.  
  
## <a name="unsupported-visual-foxpro-commands-and-functions"></a>Funzioni e comandi Visual FoxPro non supportati  

:::row:::
    :::column:::
        ! Comando (vedere eseguire &#124;. Comando  
        #<a name="define--undef"></a>DEFINISCI... #UNDEF  
        #<a name="if--endif-preprocessor-directive"></a>IF... #ENDIF direttiva per il preprocessore  
        #<a name="ifdef-124-ifndef"></a>IFDEF &#124; #IFNDEF  
        #<a name="include-preprocessor-directive"></a>INCLUDi direttiva per il preprocessore  
        Operatore di risoluzione dell'ambito::  
        ? &#124;? Comando  
    :::column-end:::
    :::column:::
        ??? Comando  
        @ ... BOX (comando)  
        @ ... Comando CLASS  
        @ ... Cancella comando  
        @ ... Comando EDIT-Edit boxs  
        @ ... Comando FILL  
        @ ... Ottieni  
    :::column-end:::
    :::column:::
        @ ... Comando di MENU  
        @ ... PROMPT dei comandi  
        @ ... Comando SAY  
        @ ... SCROLL (comando)  
        @ ... A Command  
        \ &#124; \\ \ comando  
    :::column-end:::
:::row-end:::

## <a name="a"></a>A  

:::row:::
    :::column:::
        Comando ACCEPT  
        Funzione ACLASS ()  
        Comando di MENU attiva  
        ATTIVA comando POPUP  
        Comando attiva schermata  
        Comando attiva finestra  
    :::column-end:::
    :::column:::
        Metodo ActivateCell  
        Aggiungi classe (comando)  
        Funzione ADIR ()  
        Funzione AFONT ()  
        Funzione AINSTANCE ()  
        Variabile di memoria di sistema _ALIGNMENT  
    :::column-end:::
    :::column:::
        Funzione AMEMBERS ()  
        Funzione ANSITOOEM ()  
        Funzione APRINTERS ()  
        Funzione ASELOBJ ()  
        Comando ASSIST  
    :::column-end:::
:::row-end:::

## <a name="b"></a>B  

:::row:::
    :::column:::
        Funzione barra ()  
        Funzione BARCOUNT ()  
        Funzione BARPROMPT ()  
        Variabile di memoria di sistema _BEAUTIFY  
    :::column-end:::
    :::column:::
        Variabile di memoria di sistema _BOX  
        Comando Sfoglia  
        Variabile di memoria di sistema _BROWSER  
        Comando Compila APP  
    :::column-end:::
    :::column:::
        Comando BUILD EXE  
        Comando Compila progetto  
        Variabile di memoria di sistema _BUILDER  
    :::column-end:::
:::row-end:::

## <a name="c"></a>C  

:::row:::
    :::column:::
        Variabile di memoria di sistema _CALCVALUE  
        CHIAMA comando  
        Annulla comando  
        Funzione CAPSLOCK ()  
        Comando CD  
        Comando CHANGE  
        Comando CHDIR  
        Funzione CHRSAW ()  
        Variabile di memoria di sistema _CLIPTEXT  
        Comando Chiudi MEMO  
        Funzione CNTBAR ()  
        Funzione CNTPAD ()  
        Funzione COL ()  
        Compila comando  
    :::column-end:::
    :::column:::
        Comando Compila DATABASE  
        Comando Compila modulo  
        Funzione COMPOBJ ()  
        Oggetto contenitore  
        Oggetto Control  
        Variabile di memoria di sistema _CONVERTER  
        Comando copia FILE  
        COPY MEMO (comando)  
        Crea comando  
        Comando CREATE CLASS  
        Comando CREATE CLASSLIB  
        Comando crea SET di colori  
        Comando Crea connessione  
        Comando CREATE DATABASE  
    :::column-end:::
    :::column:::
        Comando Crea modulo  
        Crea da comando  
        Comando Crea etichetta  
        Comando di MENU crea  
        Comando Crea progetto  
        Comando crea QUERY  
        Comando crea REPORT  
        Comando Crea schermata  
        Comando Crea vista SQL  
        Comando CREATE TRIGGER  
        Comando Crea visualizzazione  
        Funzione CREATEOBJECT ()  
        Funzione CURDIR ()  
        Variabile di memoria di sistema _CUROBJ  
    :::column-end:::
:::row-end:::

## <a name="d"></a>D  

:::row:::
    :::column:::
        Variabile di memoria di sistema _DBLCLICK  
        Funzione DBSETPROP ()  
        Funzioni DDE  
        DISATTIVA comando di MENU  
        DISATTIVA comando POPUP  
        DISATTIVA comando finestra  
        DECLARE (comando)  
        Comando DECLARE-DLL  
        Comando Definisci barra  
        Comando Definisci casella  
        Comando Definisci classe  
        Comando di MENU Definisci  
    :::column-end:::
    :::column:::
        Definisci comando di riempimento  
        Definisci comando POPUP  
        Comando Definisci finestra  
        Comando Elimina connessione  
        Comando DELETE DATABASE  
        Comando DELETE FILE  
        Comando DELETE TRIGGER  
        Comando Elimina visualizzazione  
        Variabile di memoria di sistema _DIARYDATE  
        DIR (comando)  
        Comando DIRECTORY  
        Visualizza comando  
    :::column-end:::
    :::column:::
        Comando Visualizza connessioni  
        Comando Visualizza DATABASE  
        Comando Visualizza dll  
        Comando Visualizza file  
        Comando Visualizza memoria  
        Comando Visualizza oggetti  
        Comando Visualizza routine  
        Comando Visualizza stato  
        Comando Visualizza struttura  
        Comando Visualizza tabelle  
        Comando Visualizza visualizzazioni  
        Comando DO FORM  
    :::column-end:::
:::row-end:::

## <a name="e"></a>E  

:::row:::
    :::column:::
        Modifica comando  
        Comando EJECT  
        Comando Rimuovi pagina  
    :::column-end:::
    :::column:::
        Comando ERASE  
        Comando ERROR  
        Esporta comando  
    :::column-end:::
    :::column:::
        Comando esterno  
    :::column-end:::
:::row-end:::

## <a name="f"></a>F  

:::row:::
    :::column:::
        Funzione FCHSIZE ()  
        Funzione FCLOSE ()  
        Funzione FCREATE ()  
        Funzione FEOF ()  
        Funzione FERRItor ()  
        Funzione FFLUSH ()  
        Funzione FGETS ()  
    :::column-end:::
    :::column:::
        FILEr (comando)  
        Comando trova  
        Funzione FKLABEL ()  
        Funzione FKMAX ()  
        Funzione FONTMETRIC ()  
        Funzione FOPEn ()  
        Variabile di memoria di sistema _FOXDOC  
    :::column-end:::
    :::column:::
        Variabile di memoria di sistema _FOXGRAPH  
        Funzione FPUTS ()  
        FREAD () (funzione)  
        Funzione FSEEK ()  
        Funzione FWRITE ()  
    :::column-end:::
:::row-end:::

## <a name="g"></a>G  

:::row:::
    :::column:::
        Variabile di memoria di sistema _GENGRAPH  
        Variabile di memoria di sistema _GENMENU  
        Variabile di memoria di sistema _GENPD  
        Variabile di memoria di sistema _GENSCRN  
        Variabile di memoria di sistema _GENXTAB  
    :::column-end:::
    :::column:::
        Funzione GETBAR ()  
        Funzione GetColor ()  
        Funzione GETDIR ()  
        Comando getexpr  
        Funzione GETFILE ()  
    :::column-end:::
    :::column:::
        Funzione GETFONT ()  
        Funzione GetObject ()  
        Funzione GETPAD ()  
        Funzione GETPICT ()  
        Funzione GetPrinter ()  
    :::column-end:::
:::row-end:::

## <a name="h"></a>H  

:::row:::
    :::column:::
        Comando della Guida  
        Comando Nascondi MENU  
    :::column-end:::
    :::column:::
        Nascondi comando POPUP  
        Comando Nascondi finestra  
    :::column-end:::
    :::column:::
        HOME () (funzione)  
    :::column-end:::
:::row-end:::

## <a name="i"></a>I  

:::row:::
    :::column:::
        Funzione IMESTATUS ()  
        Comando IMPORT  
        Variabile di memoria di sistema _INDENT  
        Indice sul comando  
    :::column-end:::
    :::column:::
        Funzione INKEY ()  
        INPUT (comando)  
        Inserisci comando  
        Funzione INSMODE ()  
    :::column-end:::
    :::column:::
        Funzione COLOR ()  
        Funzione tomouse ()  
    :::column-end:::
:::row-end:::

## <a name="j"></a>J  

Comando JOIN

## <a name="k"></a>K  

Comando tastiera

## <a name="l"></a>L  

:::row:::
    :::column:::
        LABEL (comando)  
        Funzione LASTKEY ()  
        Funzione lino ()  
    :::column-end:::
    :::column:::
        Elenca comandi  
        Comando LIST CONNECTIONs  
        Variabile di memoria di sistema _LMARGIN  
    :::column-end:::
    :::column:::
        Comando LOAD  
        Funzione LOCFILE ()  
    :::column-end:::
:::row-end:::

## <a name="m"></a>M  

:::row:::
    :::column:::
        Funzione MCOL ()  
        Comando MD  
        Funzione MDOWN ()  
        Funzione MEMORY ()  
        Comando di MENU  
        Funzione MENU ()  
        Comando da MENU a  
        MESSAGEBOX () (funzione)  
        Comando MKDIR  
        Comando modifica classe  
        Comando Modifica comando  
        Comando modifica connessione  
    :::column-end:::
    :::column:::
        Comando modifica DATABASE  
        Comando modifica FILE  
        Comando modifica modulo  
        Modifica comando generale  
        Comando modifica etichetta  
        Comando modifica MEMO  
        Comando di MENU modifica  
        Comando MODIFY PROCEDURE  
        Comando modifica progetto  
        Comando modifica QUERY  
        Comando modifica REPORT  
        Comando modifica schermata  
    :::column-end:::
    :::column:::
        Comando modifica struttura  
        Comando modifica visualizzazione  
        Comando modifica finestra  
        Comando del MOUSE  
        Sposta comando POPUP  
        Comando Sposta finestra  
        Funzione MRKBAR ()  
        Funzione MRKPAD ()  
        Funzione MROW ()  
        Funzione MWINDOW ()  
    :::column-end:::
:::row-end:::

## <a name="n"></a>N  

Funzione NUMLOCK ()

## <a name="o"></a>O  

:::row:::
    :::column:::
        Funzione OBJNUM ()  
        Funzione OBJTOCLIENT ()  
        Funzione OBJVAR ()  
        Funzione OEMTOANSI ()  
        Comando ON APLABOUT  
        Comando su barra  
        Comando di ESCAPE  
        Comando barra di uscita  
    :::column-end:::
    :::column:::
        Comando di MENU Esci  
        Comando di riempimento in uscita  
        Comando POPUP di uscita  
        ON KEY = comando  
        Comando per l'etichetta chiave  
        Comando ON MACHELP  
        Comando ON PAD  
        Comando della pagina  
    :::column-end:::
    :::column:::
        Comando ON READERROR  
        Comando barra di selezione  
        Comando di MENU selezione  
        Comando per il riquadro di selezione  
        Comando POPUP di selezione  
        AL comando SHUTDOWN  
    :::column-end:::
:::row-end:::

## <a name="p"></a>P  

:::row:::
    :::column:::
        Comando PACK DATABASE  
        Funzione PAD ()  
        Variabile di memoria di sistema _PADVANCE  
        Variabile di memoria di sistema _PAGENO  
        Variabile di memoria di sistema _PBPAGE  
        Funzione PCOL ()  
        Variabile di memoria di sistema _PCOLNO  
        Variabile di memoria di sistema _PCOPIES  
        Variabile di memoria di sistema _PDRIVER  
        Variabile di memoria di sistema _PDSETUP  
        Variabile di memoria di sistema _PECODE  
        Variabile di memoria di sistema _PEJECT  
        Funzione PEMSTATUS ()  
    :::column-end:::
    :::column:::
        Variabile di memoria di sistema _PEPAGE  
        Comando Riproduci MACRO  
        Variabile di memoria di sistema _PLENGTH  
        Variabile di memoria di sistema _PLINENO  
        Variabile di memoria di sistema _PLOFFSET  
        Comando POP KEY  
        Comando MENU POP  
        Comando POP POPUP  
        Funzione POPUP ()  
        Variabile di memoria di sistema _PPITCH  
        Variabile di memoria di sistema _PQUALITY  
        Variabile di memoria di sistema _PRETEXT  
        PRINTJOB... Comando ENDPRINTJOB  
    :::column-end:::
    :::column:::
        Funzione PRINTSTATUS ()  
        Funzione PRMBAR ()  
        Funzione PRMPAD ()  
        Funzione PROMPT ()  
        Funzione prora ()  
        Funzione PRTINFO ()  
        Variabile di memoria di sistema _PSCODE  
        Variabile di memoria di sistema _PSPACING  
        Comando PUSH KEY  
        Comando di MENU PUSH  
        Comando POPUP PUSH  
        Funzione PUTFILE ()  
        Variabile di memoria di sistema _PWAIT  
    :::column-end:::
:::row-end:::

## <a name="q"></a>Q  

QUIT-comando

## <a name="r"></a>R  

:::row:::
    :::column:::
        Comando RD  
        Funzione RDLEVEL ()  
        Comando READ  
        Comando di MENU READ  
        Funzione READKEY ()  
        Funzione REFRESH ()  
        Comando REINDEX  
        Comando RELEASE  
        Comando della barra della versione  
        Comando RELEASE CLASSLIB  
        RELEASE LIBRARY (comando)  
        Comando menu versione  
        Comando RELEASE MODULE  
    :::column-end:::
    :::column:::
        Comando RELEASE PAD  
        Comando rilascia POPUP  
        Comando RELEASE PROCEDURE  
        Comando rilascia WINDOWS  
        Comando Rimuovi classe  
        Rinomina comando  
        Comando Rinomina classe  
        Comando Rinomina connessione  
        Comando Rinomina tabella  
        Comando Rinomina visualizzazione  
        Comando REPORT  
        Funzione Requery ()  
        Comando RESTOre FROM  
    :::column-end:::
    :::column:::
        Comando RESTOre MACROs  
        Comando Ripristina schermata  
        Comando Ripristina finestra  
        RESUME (comando)  
        Funzione RGB ()  
        Funzione RGBSCHEME ()  
        Variabile di memoria di sistema _RMARGIN  
        Comando RMDIR  
        ROW () (funzione)  
        ESEGUIRE &#124;. Comando  
        Comando RUNSCRIPT  
    :::column-end:::
:::row-end:::

## <a name="s"></a>S  

:::row:::
    :::column:::
        Comando SAVE MACROs  
        Comando Salva schermata  
        Salva in comando  
        Salva comando WINDOWS  
        Funzione SCHEME ()  
        Funzione SCOLS ()  
        Variabile di memoria di sistema _SCREEN  
        SCROLL (comando)  
        IMPOSTA comando  
        IMPOSTA comando alternativo  
        SET ANSI (comando)  
        Comando SET APLABOUT  
        IMPOSTA comando salvataggio automatico  
        SET BELL (comando)  
        IMPOSTA comando BLINK  
        SET BORDER (comando)  
        Comando SET BRSTATUS  
        Comando SET CLASSLIB  
        SET CLEAR (comando)  
        Comando SET CLOCK  
        IMPOSTA il colore del comando  
        Imposta colore del comando Schema  
        IMPOSTA il comando set colori  
        Imposta colore su comando  
        IMPOSTA il comando compatibile  
        IMPOSTA comando conferma  
        IMPOSTA comando CONSOLE  
        IMPOSTA CPCOMPILE  
        IMPOSTA CPDIALOG  
        Comando Imposta valuta  
        Comando imposta cursore  
        IMPOSTA comando DataSession  
        SET DEBUG-comando  
        IMPOSTA comando decimali  
        IMPOSTA comando delimitatori  
        IMPOSTA comando di sviluppo  
        Comando Imposta dispositivo  
    :::column-end:::
    :::column:::
        IMPOSTA comando Visualizza  
        Comando SET DOHISTORY  
        IMPOSTA comando ECHO  
        IMPOSTA comando ESCAPE  
        Comando SET FORMAT  
        Comando SET FUNCTION  
        Comando imposta intestazioni  
        IMPOSTA comando della Guida  
        Comando SET HELPFILTER  
        Comando Imposta intensità  
        Comando Imposta chiave  
        Imposta comando per la configurazione  
        Comando SET LOGERRORS  
        Comando SET MACDESKTOP  
        Comando SET MACHELP  
        IMPOSTA comando MACKEY  
        Comando SET MARGIN  
        SET MARK OF Command  
        Imposta contrassegno su comando  
        Comando SET MEMOWIDTH  
        Comando imposta messaggio  
        IMPOSTA comando MOUSE  
        SET odometro (comando)  
        IMPOSTA il comando OLEOBJECT  
        Comando imposta TAVOLOzza  
        Comando SET PDSETUP  
        Comando SET POINT  
        Comando Imposta stampante  
        Comando SET READBORDER  
        SET REFRESH (comando)  
        Comando SET RESOURCE  
        Comando Imposta sicurezza  
        IMPOSTA comando SEGNAPUNTi  
        Comando imposta secondi  
        IMPOSTA SEPARATOre (comando)  
        Comando SET SHADOWs  
        SET SKIP OF Command  
    :::column-end:::
    :::column:::
        Comando imposta spazio  
        Comando imposta stato  
        Comando imposta barra di stato  
        Comando SET STEP  
        IMPOSTA comando STICKY  
        Comando SET SYSFORMATS  
        Comando SET SYSMENU  
        IMPOSTA comando TALK  
        Comando SET TEXTMERGE  
        IMPOSTA comando delimitatori TEXTMERGE  
        Comando imposta argomento  
        Comando imposta ID argomento  
        Comando SET TRBETWEEN  
        Comando SET TYPEAHEAD  
        Comando imposta visualizzazione  
        IMPOSTARE la finestra del comando MEMO  
        Comando SET XCMDFILE  
        Variabile di memoria di sistema _SHELL  
        Mostra comando GET  
        Mostra comando ottiene  
        Mostra comando di MENU  
        Mostra comando oggetto  
        Mostra comando POPUP  
        Mostra comando finestra  
        Comando POPUP dimensioni  
        Comando finestra dimensioni  
        Funzione SKPBAR ()  
        Funzione SKPPAD ()  
        Funzione SOUNDEX ()  
        Variabile di memoria di sistema _SPELLCHK  
        Funzioni SQL  
        Funzione SROWS ()  
        Variabile di memoria di sistema _STARTUP  
        SOSPENDi comando  
        Funzioni SYS () ad eccezione di SYS (2011)  
        Funzione SYSMETRIC ()  
    :::column-end:::
:::row-end:::

## <a name="t"></a>T  

:::row:::
    :::column:::
        Variabile di memoria di sistema _TABS  
        TESTO... Comando ENDTEXT  
        Variabile di memoria di sistema _THROTTLE  
    :::column-end:::
    :::column:::
        Funzione TRANSFORM ()  
        Variabile di memoria di sistema _TRANSPORT  
        Funzione TXTWIDTH ()  
    :::column-end:::
    :::column:::
        Comando di tipo  
    :::column-end:::
:::row-end:::

## <a name="u"></a>U  

:::row:::
    :::column:::
        Funzione UPDATED ()  
    :::column-end:::
    :::column:::
        Comando USE  
    :::column-end:::
:::row-end:::

## <a name="v"></a>V  

:::row:::
    :::column:::
        Comando VALIDATE DATABASE  
    :::column-end:::
    :::column:::
        Funzione VARREAD ()  
    :::column-end:::
    :::column:::
        Funzione VERSION ()  
    :::column-end:::
:::row-end:::

## <a name="w"></a>W  

:::row:::
    :::column:::
        Comando WAIT  
        Funzione WBORDER ()  
        Funzione WCHILD ()  
        Funzione WCOLS ()  
        Funzione WEXIST ()  
        Funzione WFONT ()  
        Variabile di memoria di sistema _WINDOWS  
        CON... Comando ENDWITH  
    :::column-end:::
    :::column:::
        Variabile di memoria di sistema _WIZARD  
        Funzione WLAST ()  
        Funzione WLCOL ()  
        Funzione WLROW ()  
        Funzione WMAXIMUM ()  
        Funzione WMINIMUM ()  
        Funzione WONTOP ()  
        Funzione WOUTPUT ()  
    :::column-end:::
    :::column:::
        Funzione WPARENT ()  
        Variabile di memoria di sistema _WRAP  
        Funzione WREAD ()  
        Funzione WROWS ()  
        Funzione WTITLE ()  
        Funzione WVISIBLE ()  
    :::column-end:::
:::row-end:::

## <a name="z"></a>Z  

Comando della finestra ZOOM
