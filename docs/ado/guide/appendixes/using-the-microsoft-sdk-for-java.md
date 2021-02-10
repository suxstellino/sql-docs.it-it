---
description: Uso di Microsoft SDK per Java
title: Uso di Microsoft SDK per Java | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Java (Microsoft SDK for)
- Microsoft SDK for Java [ADO]
ms.assetid: 2d7cb5b5-8307-49dd-b07e-c07069bb1626
author: rothja
ms.author: jroth
ms.openlocfilehash: 13a518a3cbf7c4d9f83da5810e340406147f86f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028831"
---
# <a name="using-the-microsoft-sdk-for-java"></a>Uso di Microsoft SDK per Java

> [!IMPORTANT]
> Microsoft ha interrotto il supporto di Visual J++ nel gennaio 2004.

Microsoft SDK per Java è il kit per sviluppatori per l'ambiente Microsoft Internet Explorer. Sono disponibili strumenti, informazioni ed esempi che consentono di sviluppare programmi Java e applet basati su JDK 1,1 e la macchina virtuale Microsoft Win32 (VM Microsoft). Microsoft SDK per Java non è associato a Microsoft Visual J++. Per scaricare l'SDK, fare clic qui.  
  
 L'utilità Jactivex.exe genera classi da una libreria dei tipi, ma può essere richiamata solo nella riga di comando. Questa funzionalità non è integrata con l'ambiente di sviluppo di Visual J++. Diversamente dalle classi generate dalla creazione guidata libreria dei tipi Java, è possibile eseguire l'istruzione nei wrapper della classe creati dall'SDK. Questa operazione è utile per il debug del modo in cui il codice utilizza le classi wrapper ADO.  
  
 Questo meccanismo legge la libreria dei tipi ADO e genera le classi di cui è possibile creare un'istanza all'interno dell'applicazione. Genera le classi nel percorso seguente: \\<directory Windows \> \Java\trustlib\msado15.  
  
 La creazione di un'applicazione ADO in Java con Microsoft SDK per Java è fondamentalmente identica, dal punto di vista del codice sorgente, all'uso della procedura guidata per la libreria dei tipi Java. Per il codice di esempio, vedere [wrapper della classe ADO Java](./ado-java-class-wrappers.md). L'unica differenza reale consiste nel modo in cui vengono generate le classi wrapper in primo luogo, come illustrato nei passaggi seguenti.  
  
### <a name="to-create-an-ado-project-with-the-microsoft-sdk-for-java"></a>Per creare un progetto ADO con Microsoft SDK per Java  
  
1.  Eseguire quanto segue al prompt dei comandi. È necessario impostare il percorso in modo da includere la directory Microsoft SDK per Java bin oppure eseguire il comando da tale percorso. In genere, Microsoft SDK per Java viene installato nello stesso percorso di Visual Studio. Si tratta di una singola istruzione di comando.  
  
    ```java
    \<path to DevStudio>\<path to Java SDK>\bin\JactiveX.exe /javatlb "C:\program files\common files\system\ado\msado15.dll"  
    ```  
  
2.  Eseguire il comando seguente per compilare le classi generate. L'opzione/g: t attiva la generazione di simboli di debug in modo che sia possibile tracciare in. Simboli Java. Rimuoverlo per le compilazioni di rilascio.  
  
    ```java
    jvc /g:t c:\<windows>\Java\trustlib\msado15\*.Java  
    ```  
  
3.  Per usare questi file, aprire il progetto in Visual J++. Dal menu **progetto** scegliere **Aggiungi al progetto**. Selezionare **file** e aggiungere tutti i. File JAVA generati nella directory trustlib\msado15 del progetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Wrapper di classe Java ADO](./ado-java-class-wrappers.md)