---
description: Programmazione ADO VBScript
title: Programmazione ADO VBScript | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, VBScript
- VBScript [ADO]
ms.assetid: 6aaaf6d0-1376-4473-bea6-b81f2645a9ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 31e6c98d12c6c6f472053634d8b3aea314ccbab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037871"
---
# <a name="vbscript-ado-programming"></a>Programmazione ADO VBScript
## <a name="creating-an-ado-project"></a>Creazione di un progetto ADO  
 Microsoft Visual Basic, Scripting Edition non supporta le librerie dei tipi, pertanto non è necessario fare riferimento a ADO nel progetto. Di conseguenza, non sono supportate funzionalità associate come il completamento della riga di comando. Inoltre, per impostazione predefinita, le costanti enumerate ADO non sono definite in VBScript.  
  
 ADO fornisce tuttavia due file di inclusione contenenti le seguenti definizioni da usare con VBScript:  
  
-   Per gli script sul lato server, usare Adovbs. Inc, installato nella cartella c:\Programmi\File Files\System\ado\ per impostazione predefinita.  
  
-   Per l'esecuzione di script sul lato client, usare Adcvbs. Inc, installato nella cartella c:\Programmi\File Files\System\msdac\ per impostazione predefinita.  
  
 È possibile copiare e incollare le definizioni di costanti da questi file nelle pagine ASP oppure, se si esegue lo script sul lato server, copiare il file Adovbs. Inc in una cartella nel sito Web e fare riferimento a tale file dalla pagina ASP come indicato di seguito:  
  
```vb
<!--#include File="adovbs.inc"-->  
```  
  
## <a name="creating-ado-objects-in-vbscript"></a>Creazione di oggetti ADO in VBScript  
 Non è possibile usare l'istruzione **Dim** per assegnare oggetti a un tipo specifico in VBScript. Inoltre, VBScript non supporta la **nuova** sintassi utilizzata con l'istruzione **Dim** in Visual Basic, Applications Edition. È invece necessario utilizzare la chiamata di funzione **CreateObject** :  
  
```vb
Dim Rs1  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
```  
  
## <a name="vbscript-examples"></a>Esempi di VBScript  
 Il codice seguente è un esempio generico di programmazione sul lato server di VBScript in un file di Active Server pagina (ASP):  
  
```vb
<%  @LANGUAGE="VBSCRIPT" %>  
<%  Option Explicit %>  
<!--#include File="adovbs.inc"-->  
<HTML>  
    <BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
  
    <!-- Your ASP Code goes here -->  
<%  
Dim Source  
Dim Connect  
Dim Rs1  
  
Source = "SELECT * FROM Authors"  
Connect = "Provider=sqloledb;Data Source=srv;" & _  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
Rs1.Open Source, Connect, adOpenForwardOnly  
Response.Write("Success!")  
%>  
    </BODY>  
</HTML>  
```  
  
 Con la documentazione ADO sono inclusi esempi VBScript più specifici. Per ulteriori informazioni, vedere [esempi di codice ADO in Microsoft Visual Basic Scripting Edition](../../reference/ado-api/ado-code-examples-vbscript.md).  
  
## <a name="differences-between-vbscript-and-visual-basic"></a>Differenze tra VBScript e Visual Basic  
 L'utilizzo di ADO con VBScript è analogo all'utilizzo di ADO con Visual Basic in molti modi, inclusa la sintassi utilizzata. Esistono tuttavia alcune differenze significative:  
  
-   VBScript supporta solo il tipo di dati Variant, che può ospitare tipi diversi di dati. È possibile archiviare i dati necessari in un tipo di dati Variant e i dati funzioneranno in modo appropriato a causa del cast eseguito da VBScript. Riconosce il tipo richiesto da ADO e converte il valore nella variabile di conseguenza.  
  
-   Non è possibile usare **in errore \<label> goto** in VBScript.  
  
-   VBScript supporta alcune delle funzioni Visual Basic predefinite, ad esempio **MsgBox**, **date** e **numeric**. Poiché VBScript è un subset di Visual Basic, tuttavia, non tutte le funzioni predefinite sono supportate. VBScript, ad esempio, non supporta la funzione **Format** e le funzioni di i/O del file.