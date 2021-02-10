---
description: Utilizzo di ADO con Microsoft Visual Basic e Visual Basic, Applications Edition
title: Utilizzo di ADO con Microsoft Visual Basic | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, Visual Basic
- Visual Basic [ADO]
ms.assetid: 9dfb6784-037d-4f9d-bb7f-b506b4498573
author: rothja
ms.author: jroth
ms.openlocfilehash: 229662bd5db68039202f1f39d7309c12ff6354a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028861"
---
# <a name="using-ado-with-microsoft-visual-basic-and-visual-basic-for-applications"></a>Utilizzo di ADO con Microsoft Visual Basic e Visual Basic, Applications Edition
La configurazione di un progetto ADO e la scrittura di codice ADO sono simili se si utilizza Visual Basic o Visual Basic, Applications Edition. Questo argomento illustra l'uso di ADO con Visual Basic e Visual Basic, Applications Edition e rileva eventuali differenze.

## <a name="referencing-the-ado-library"></a>Riferimento alla libreria ADO
 Il progetto deve fare riferimento alla libreria ADO.

#### <a name="to-reference-ado-from-microsoft-visual-basic"></a>Per fare riferimento a ADO da Microsoft Visual Basic

1.  In Visual Basic scegliere **riferimenti** dal menu **progetto** .

2.  Selezionare **Microsoft ActiveX Data Objects la libreria x. x** dall'elenco. Verificare che siano selezionate anche le librerie seguenti:

    -   Visual Basic, Applications Edition

    -   Oggetti e procedure di Visual Basic Runtime

    -   Oggetti e procedure Visual Basic

    -   automazione OLE

3.  Fare clic su **OK**.

 È possibile utilizzare ADO con la stessa facilità con Visual Basic, Applications Edition, ad esempio utilizzando Microsoft Access.

#### <a name="to-reference-ado-from-microsoft-access"></a>Per fare riferimento a ADO da Microsoft Access

1.  In Microsoft Access selezionare o creare un modulo dalla scheda **moduli** della finestra **database** .

2.  Scegliere **riferimenti** dal menu **strumenti** .

3.  Selezionare **Microsoft ActiveX Data Objects la libreria x. x** dall'elenco. Verificare che siano selezionate anche le librerie seguenti:

    -   Visual Basic, Applications Edition

    -   Libreria oggetti di Microsoft Access 8,0 (o versione successiva)

    -   Libreria oggetti di Microsoft DAO 3,5 (o versione successiva)

4.  Fare clic su **OK**.

## <a name="creating-ado-objects-in-visual-basic"></a>Creazione di oggetti ADO in Visual Basic
 Per creare una variabile di automazione e un'istanza di un oggetto per tale variabile, è possibile usare due metodi: **Dim** o **CreateObject**.

### <a name="dim"></a>Dim
 È possibile utilizzare la parola chiave **New** con **Dim** per dichiarare e creare istanze di oggetti ADO in un unico passaggio:

```
Dim conn As New ADODB.Connection
```

 In alternativa, la dichiarazione di istruzione **Dim** e la creazione di istanze di oggetti possono essere anche due passaggi:

```
Dim conn As ADODB.Connection
Set conn = New ADODB.Connection
```

> [!NOTE]
>  Non è necessario usare in modo esplicito `ADODB` ProgID con l'istruzione **Dim** , supponendo che sia stato fatto riferimento alla libreria ADO nel progetto. Tuttavia, l'uso di questo garantisce che non si verificheranno conflitti di denominazione con altre librerie.

> [!NOTE]
>  Se, ad esempio, si includono riferimenti a ADO e DAO nello stesso progetto, è necessario includere un qualificatore per specificare il modello a oggetti da utilizzare durante la creazione di istanze di oggetti **Recordset** , come nel codice seguente:

```
Dim adoRS As ADODB.Recordset
Dim daoRS As DAO.Recordset
```

### <a name="createobject"></a>CreateObject
 Con il metodo **CreateObject** , la dichiarazione e la creazione di istanze dell'oggetto devono essere due passaggi discreti:

```
Dim conn1
Set conn1 = CreateObject("ADODB.Connection") As Object
```

 Gli oggetti di cui è stata creata un'istanza con **CreateObject** sono ad associazione tardiva, il che significa che non sono fortemente tipizzati e il completamento da riga di comando è disabilitato. Tuttavia, consente di ignorare il riferimento alla libreria ADO dal progetto e consente di creare istanze di versioni specifiche di oggetti. Ad esempio:

```
Set conn1 = CreateObject("ADODB.Connection.2.0") As Object
```

 Questa operazione può essere eseguita anche creando in modo specifico un riferimento alla libreria dei tipi ADO versione 2,0 e creando l'oggetto.

 La creazione di istanze di oggetti tramite il metodo **CreateObject** è in genere più lenta rispetto all'utilizzo dell'istruzione **Dim** .

## <a name="handling-events"></a>Gestione degli eventi
 Per gestire gli eventi ADO in Microsoft Visual Basic, è necessario dichiarare una variabile a livello di modulo usando la parola chiave **WithEvents** . La variabile può essere dichiarata solo come parte di un modulo di classe e deve essere dichiarata a livello di modulo. Per una discussione più approfondita sulla gestione degli eventi ADO, vedere [gestione degli eventi ADO](../data/handling-ado-events.md).

## <a name="visual-basic-examples"></a>Esempi di Visual Basic
 Molti esempi di Visual Basic sono inclusi nella documentazione di ADO. Per ulteriori informazioni, vedere [esempi di codice ADO in Microsoft Visual Basic](../../reference/ado-api/ado-code-examples-in-visual-basic.md).

## <a name="see-also"></a>Vedere anche
 [Microsoft ActiveX Data Objects (ADO)](../../microsoft-activex-data-objects-ado.md) [utilizzo di ADO con Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md) [utilizzo di ADO con i linguaggi di scripting](./using-ado-with-scripting-languages.md)