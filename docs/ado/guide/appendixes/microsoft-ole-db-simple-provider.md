---
description: Panoramica di Microsoft OLE DB Simple Provider
title: Provider semplice Microsoft OLE DB | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- simple provider [ADO]
- providers [ADO], OLE DB simple provider
- OLE DB simple provider [ADO]
ms.assetid: 1e7dc6f0-482c-4103-8187-f890865e40fc
author: rothja
ms.author: jroth
ms.openlocfilehash: a07224d352f059b8aad3709c6d4f9605c16696b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029235"
---
# <a name="microsoft-ole-db-simple-provider-overview"></a>Panoramica di Microsoft OLE DB Simple Provider
Microsoft OLE DB Simple Provider (OSP) consente a ADO di accedere a tutti i dati per i quali un provider è stato scritto utilizzando [OLE DB Simple Provider (OSP) Toolkit](/previous-versions/windows/desktop/ms715822(v=vs.85)). I provider semplici sono destinati ad accedere alle origini dati che richiedono solo il supporto OLE DB fondamentale, ad esempio matrici in memoria o documenti XML.

## <a name="connection-string-parameters"></a>Parametri della stringa di connessione
 Per connettersi all'OLE DB DLL del provider semplice, impostare l'argomento del *provider* sulla proprietà [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) su:

```vb
MSDAOSP
```

 Questo valore può essere impostato o letto anche usando la proprietà del [provider](../../reference/ado-api/provider-property-ado.md) .

 È possibile connettersi a provider semplici che sono stati registrati come provider OLE DB completi usando il nome del provider registrato, determinato dal writer del provider.

## <a name="typical-connection-string"></a>Stringa di connessione tipica
 Una stringa di connessione tipica per questo provider è la seguente:

```vb
"Provider=MSDAOSP;Data Source=serverName"
```

 La stringa è costituita dalle parole chiave seguenti:

|Parola chiave|Descrizione|
|-------------|-----------------|
|**Provider**|Specifica il provider di OLE DB per SQL Server.|
|**Origine dati**|Specifica il nome di un server.|

## <a name="xml-document-example"></a>Esempio di documento XML
 Il provider di OLE DB Simple Provider (OSP) in MDAC 2,7 o versioni successive e Windows Data Access Components (Windows DAC) è stato migliorato per supportare l'apertura di **Recordset** ADO gerarchici su file XML arbitrari. Questi file XML possono contenere lo schema di persistenza XML ADO, ma non è obbligatorio. Questa operazione è stata implementata connettendo il OSP al **MSXML2.DLL**; è pertanto necessario **MSXML2.DLL** o versione successiva.

 Il file **portfolio.xml** usato nell'esempio seguente contiene l'albero seguente:

```console
Portfolio
   Stock
      Shares
      Symbol
      Price
      Info
         Company Name
         WebSite
```

 Il DSO XML utilizza l'euristica incorporata per convertire i nodi in un albero XML in capitoli in un **Recordset** gerarchico.

 Utilizzando queste euristiche predefinite, l'albero XML viene convertito in un **Recordset** gerarchico a due livelli nel formato seguente:

```console
Parent Recordset
Shares, Symbol, Price, $Text
   Child Recordset
      Company Name, WebSite, $Text
```

 Si noti che i tag portfolio e info non sono rappresentati nel **Recordset** gerarchico. Per una spiegazione del modo in cui il DSO XML converte gli alberi XML in **Recordset** gerarchici, vedere le regole seguenti. La colonna $Text viene illustrata nella sezione seguente.

## <a name="rules-for-assigning-xml-elements-and-attributes-to-columns-and-rows"></a>Regole per l'assegnazione di elementi e attributi XML a colonne e righe
 Il DSO XML segue una procedura per l'assegnazione di elementi e attributi a colonne e righe nelle applicazioni con associazione a dati. XML viene modellato come albero con un tag che contiene l'intera gerarchia. Una descrizione XML di un libro, ad esempio, può contenere tag capitolo, tag figure e tag section. Al livello più alto, si tratta del tag Book, contenente il capitolo, la figura e la sezione dei sottoelementi. Quando il DSO XML esegue il mapping degli elementi XML a righe e colonne, i sottoelementi, non l'elemento di primo livello, vengono convertiti.

 Il DSO XML utilizza questa procedura per la conversione dei sottoelementi:

-   Ogni sottoelemento e attributo corrisponde a una colonna in alcuni **Recordset** della gerarchia.

-   Il nome della colonna corrisponde al nome del sottoelemento o dell'attributo, a meno che l'elemento padre non abbia un attributo e un sottoelemento con lo stesso nome, nel qual caso un "!" viene anteposto al nome della colonna del sottoelemento.

-   Ogni colonna è una colonna *semplice* che contiene valori scalari (in genere stringhe) o una colonna **Recordset** contenente **Recordset** figlio.

-   Le colonne corrispondenti agli attributi sono sempre semplici.

-   Le colonne che corrispondono ai sottoelementi sono colonne del **Recordset** se il sottoelemento ha i propri sottoelementi o attributi (o entrambi) oppure il padre del sottoelemento ha più di un'istanza del sottoelemento come figlio. In caso contrario, la colonna è semplice.

-   Quando sono presenti più istanze di un sottoelemento (in padri diversi), la relativa colonna è  una colonna recordset *se una delle istanze* implica una colonna **Recordset** ; la colonna è semplice solo se *tutte le* istanze implicano una colonna semplice.

-   Tutti i **Recordset** hanno una colonna aggiuntiva denominata $text.

 Il codice necessario per costruire un **Recordset** è il seguente:

```vb
Dim adoConn as ADODB.Connection
Dim adoRS as ADODB.Recordset

Set adoRS = New ADODB.Connection
Set adoRS = New ADODB.Recordset

adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
adoRS.Open "https://WebServer/VRoot/portfolio.xml, adoConn
```

> [!NOTE]
>  È possibile specificare il percorso del file di dati utilizzando quattro diverse convenzioni di denominazione.

```vb
'HTTP://
adoRS.Open "https://WebServer/VRoot/portfolio.xml", adoConn
'FILE://
adoRS.Open "file:/// C:\\Directory\\portfolio.xml", adoConn
'UNC Path
adoRS.Open "\\ComputerName\ShareName\portfolio.xml", adoConn
'Full DOS Path
adoRS.Open "C:\Directory\portfolio.xml", adoConn
```

 Non appena il **Recordset** è stato aperto, è possibile utilizzare i normali comandi di spostamento del **Recordset** ADO.

 I **Recordset** generati da OSP presentano alcune limitazioni:

-   I cursori client (**adUseClient**) non sono supportati.

-   I **Recordset** gerarchici creati sul codice XML arbitrario non possono essere salvati in modo permanente utilizzando **Recordset. Save**.

-   I **Recordset** creati con OSP sono di sola lettura.

-   XMLDSO aggiunge una colonna aggiuntiva di dati ($Text) a ogni **Recordset** nella gerarchia.

 Per ulteriori informazioni sull'OLE DB provider semplice, vedere [compilazione di un provider semplice](/previous-versions/windows/desktop/ms721067(v=vs.85)).

## <a name="code-example"></a>Esempio di codice
 Il codice di Visual Basic seguente illustra l'apertura di un file XML arbitrario, la costruzione di un **Recordset** gerarchico e la scrittura ricorsiva di ogni record di ogni **Recordset** nella finestra di debug.

 Di seguito è riportato un semplice file XML che contiene le quotazioni azionarie. Il codice seguente usa questo file per costruire un **Recordset** gerarchico a due livelli.

```xml
<portfolio>
   <stock>
      <shares>100</shares>
      <symbol>MSFT</symbol>
      <price>$70.00</price>
      <info>
         <companyname>Microsoft Corporation</companyname>
         <website>https://www.microsoft.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>AAPL</symbol>
      <price>$107.00</price>
      <info>
         <companyname>Apple Computer, Inc.</companyname>
         <website>https://www.apple.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>DELL</symbol>
      <price>$50.00</price>
      <info>
         <companyname>Dell Corporation</companyname>
         <website>https://www.dell.com</website>
      </info>
    </stock>
    <stock>
       <shares>100</shares>
       <symbol>INTC</symbol>
       <price>$115.00</price>
       <info>
          <companyname>Intel Corporation</companyname>
          <website>https://www.intel.com</website>
       </info>
   </stock>
</portfolio>
```

 Di seguito sono riportate due Visual Basic sottoprocedure. Il primo crea il **Recordset** e lo passa alla procedura secondaria *WalkHier* , che esamina in modo ricorsivo la gerarchia, scrivendo ogni **campo** di ogni record in ogni **Recordset** nella finestra di debug.

```vb
Private Sub BrowseHierRecordset()
' Add ADO 2.7 or later to Project/References
' No need to add MSXML2, ADO just passes the ProgID through to the OSP.

    Dim adoConn As ADODB.Connection
    Dim adoRS As ADODB.Recordset
    Dim adoChildRS As ADODB.Recordset

    Set adoConn = New ADODB.Connection
    Set adoRS = New ADODB.Recordset
    Set adoChildRS = ADODB.Recordset

    adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
    adoRS.Open "https://bwillett3/Kowalski/portfolio.xml", adoConn

    Dim iLevel As Integer
    iLevel = 0
    WalkHier iLevel, adoRS

End Sub

Sub WalkHier(ByVal iLevel As Integer, ByVal adoRS As ADODB.Recordset)
    iLevel = iLevel + 1
    PriorLevel = iLevel
    While Not adoRS.EOF
        For ndx = 0 To adoRS.Fields.Count - 1
            If adoRS.Fields(ndx).Name <> "$Text" Then
                If adoRS.Fields(ndx).Type = adChapter Then
                    Set adoChildRS = adoRS.Fields(ndx).Value
                    WalkHier iLevel, adoChildRS
                Else
                    Debug.Print iLevel & ": adoRS.Fields(" & ndx & _
                       ") = " & adoRS.Fields(ndx).Name & " = " & _
                       adoRS.Fields(ndx).Value
                End If
            End If
        Next ndx
        adoRS.MoveNext
    Wend
    iLevel = PriorLevel
End Sub
```