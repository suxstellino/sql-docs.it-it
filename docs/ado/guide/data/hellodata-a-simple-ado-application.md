---
description: 'HelloData: applicazione ADO semplice'
title: 'HelloData: semplice applicazione ADO | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- HelloData sample application [ADO]
- ADO, samples
ms.assetid: de4bcd56-dac2-45e6-95ab-9fd7f25878fc
author: rothja
ms.author: jroth
ms.openlocfilehash: ec46ed0f94ffe9bd50d0576ab7b3855011bcf775
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033161"
---
# <a name="hellodata-a-simple-ado-application"></a>HelloData: applicazione ADO semplice
Questa semplice applicazione illustra le quattro operazioni ADO principali: recupero, analisi, modifica e aggiornamento dei dati. Queste operazioni vengono eseguite sul database di esempio Northwind incluso in Microsoft® SQL Server. Per concentrarsi sulle nozioni di base di ADO e per impedire confusione del codice, la gestione degli errori nell'esempio è minima.  
  
### <a name="to-run-hellodata"></a>Per eseguire HelloData  
  
1.  Creare un nuovo progetto di Visual Basic EXE standard che fa riferimento alla libreria ADO. Per ulteriori informazioni, vedere [riferimento alle librerie ADO](../referencing-the-ado-libraries.md).  
  
2.  Creare quattro pulsanti di comando nella parte superiore del form, impostando le proprietà **nome** e **didascalia** sui valori indicati nella tabella alla fine di questo argomento.  
  
3.  Sotto i pulsanti aggiungere un **controllo DataGrid Microsoft** (msdatgrd. ocx). Il file msdatgrd. ocx è incluso in Visual Basic e si trova nella directory \Windows\System32. o \Winnt\System32. Per aggiungere il controllo DataGrid al riquadro Casella degli strumenti Visual Basic, scegliere **componenti** dal menu **progetto** . Quindi selezionare la casella accanto a "Microsoft DataGrid Control 6,0 (SP3) (OLEDB)" e quindi fare clic su **OK**. Per aggiungere il controllo al progetto, trascinare il controllo DataGrid dalla casella degli strumenti nel form Visual Basic.  
  
4.  Creare una **casella di testo** nel form sotto la griglia e impostarne le proprietà come illustrato nella tabella. Al termine, il form dovrebbe essere simile alla figura seguente.  
  
5.  Infine, copiare il codice elencato nel [codice HelloData](./hellodata-code.md)e incollarlo nella finestra dell'editor di codice del modulo. Premere **F5** per eseguire il codice.  
  
> [!NOTE]
>  Nell'esempio seguente, e nell'intera guida, viene usato l'ID utente "MyId" con la password "123aBc" per l'autenticazione nel server. È necessario sostituire questi valori con credenziali di accesso valide per il server. Sostituire anche il valore "SQLServer" con il nome del server.  
  
 Per una descrizione dettagliata del codice, vedere i [commenti in HelloData](./comments-on-hellodata.md).  
  
 ![Form1 per l'applicazione VB HelloData](../../../ado/guide/data/media/hellodata.gif "HelloData")  
  
|Tipo di controllo|Proprietà|Valore|  
|------------------|--------------|-----------|  
|Form|Nome|Form1|  
||Altezza|6500|  
||Larghezza|6500|  
|DataGrid di MS|Nome|grdDisplay1|  
|TextBox|Nome|txtDisplay1|  
||Multiline|true|  
|Pulsante di comando|Nome|cmdGetData|  
||Didascalia|Get Data|  
|Pulsante di comando|Nome|cmdExamineData|  
||Didascalia|Esaminare i dati|  
|Pulsante di comando|Nome|cmdEditData|  
||Didascalia| Modifica dei dati|  
|Pulsante di comando|Nome|cmdUpdateData|  
||Didascalia|Dati di aggiornamento|