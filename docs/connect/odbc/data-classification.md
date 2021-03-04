---
description: Uso della classificazione dei dati con Microsoft ODBC Driver for SQL Server
title: Uso della classificazione dei dati con Microsoft ODBC Driver for SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837415"
---
# <a name="data-classification"></a>Classificazione dei dati
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Panoramica
Ai fini della gestione dei dati sensibili, SQL Server e il server SQL di Azure hanno introdotto la possibilità di specificare colonne di database con metadati di riservatezza che consentono all'applicazione client di gestire diversi tipi di dati sensibili, ad esempio dati sanitari, finanziari e così via, in base ai criteri di protezione dati.

Per altre informazioni su come assegnare la classificazione alle colonne, vedere [Individuazione dati e classificazione SQL](../../relational-databases/security/sql-data-discovery-and-classification.md).

Microsoft ODBC Driver 17.2 consente il recupero di questi metadati tramite SQLGetDescField usando l'identificatore di campo SQL_CA_SS_DATA_CLASSIFICATION.

## <a name="format"></a>Format
SQLGetDescField ha la sintassi seguente:

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [Input] Handle IRD (descrittore riga di implementazione). Può essere recuperato da una chiamata a SQLGetStmtAttr con l'attributo di istruzione SQL_ATTR_IMP_ROW_DESC
  
 *RecNumber*  
 [Input] 0
  
 *FieldIdentifier*  
 [Input] SQL_CA_SS_DATA_CLASSIFICATION
  
 *ValuePtr*  
 [Output] Buffer di output
  
 *BufferLength*  
 [Input] Lunghezza del buffer di output in byte

 *StringLengthPtr* [Output] Puntatore al buffer in cui restituire il numero totale di byte disponibili da restituire in *ValuePtr*.
 
> [!NOTE]
> Se la dimensione del buffer è sconosciuta, può essere determinata chiamando SQLGetDescField con *ValuePtr* come NULL ed esaminando il valore di *StringLengthPtr*.
 
Se le informazioni di classificazione dei dati non sono disponibili, verrà restituito l'errore *Identificatore del campo del descrittore non valido*.

Al completamento di una chiamata a SQLGetDescField, il buffer a cui punta *ValuePtr* conterrà i dati seguenti:

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> `nn nn`, `tt tt` e `cc cc` sono Integer multibyte, archiviati con il byte meno significativo nell'indirizzo inferiore.

*`sensitivitylabel`* e *`informationtype`* sono entrambi del formato

 `nn [n bytes name] ii [i bytes id]`

*`columnsensitivity`* è del formato

 `nn nn [n sensitivityprops]`

Per ogni colonna *(c)* , sono presenti *n* *`sensitivityprops`* a 4 byte:

 `ss ss tt tt`

s - indice nella matrice *`sensitivitylabels`* , `FF FF` se non etichettati

t - indice nella matrice *`informationtypes`* , `FF FF` se non etichettati


<br><br>
Il formato dei dati può essere espresso come le pseudo-strutture seguenti:

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>Esempio di codice
Applicazione di prova che illustra come leggere i metadati di classificazione dei dati. In Windows può essere compilata usando `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` ed eseguita con una stringa di connessione e una query SQL (che restituisce colonne classificate) come parametri:

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>Versione supportata
Microsoft ODBC Driver 17.2 consente il recupero di informazioni di classificazione dei dati tramite `SQLGetDescField` se `FieldIdentifier` è impostato su `SQL_CA_SS_DATA_CLASSIFICATION` (1237). 

A partire da Microsoft ODBC Driver 17.4.1.1 è possibile recuperare la versione della classificazione dei dati supportata da un server tramite `SQLGetDescField` usando l'identificatore di campo `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238). Nella versione 17.4.1.1 la versione della classificazione dei dati supportata è impostata su "2".

 

La versione 17.4.2.1 ha introdotto la versione predefinita della classificazione dei dati che è impostata su "1" ed è la versione che il driver segnala a SQL Server come supportata. Il nuovo attributo di connessione `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) può consentire all'applicazione di modificare la versione supportata della classificazione dei dati da "1" fino al massimo supportato.  

Esempio: 

Per impostare la versione, è necessario effettuare questa chiamata immediatamente prima della chiamata a SQLConnect o SQLDriverConnect:
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

Il valore della versione attualmente supportata di Classificazione dati può essere recuperato tramite la chiamata di SQLGetConnectAttr: 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```