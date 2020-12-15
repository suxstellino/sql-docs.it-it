---
description: Come creare un progetto SMO Visual C# in Visual Studio .NET
title: Creare un progetto SMO per Visual C# in Visual Studio .NET
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- Visual C# [SMO]
ms.assetid: 1e7abb16-23a0-4a18-91ad-253261e6bf84
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 943cfd787cccff42973862b12f0f52feae24410c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467192"
---
# <a name="how-to-create-a-visual-c-smo-project-in-visual-studio-net"></a>Come creare un progetto SMO Visual C# in Visual Studio .NET
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  In questa sezione viene descritto come compilare un'applicazione console SMO semplice.  
  
 In questo esempio vengono importati spazi dei nomi affinché il programma faccia riferimento ai tipi SMO. L'importazione dello spazio dei nomi dell' **agente** è facoltativa. Utilizzarla quando si scrive un programma che utilizza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Lo spazio dei nomi **comune** è necessario per stabilire una connessione sicura all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lo spazio dei nomi **SqlClient** viene utilizzato per elaborare gli errori di eccezione SQL.  
  
### <a name="creating-a-visual-c-smo-project-in-visual-studionet"></a>Creazione di un progetto Visual C# SMO in Visual Studio.NET  
  
1. Avviare Visual Studio
  
2. Scegliere **nuovo** dal menu **file** , quindi **progetto**.  Verrà visualizzata la finestra di dialogo **Nuovo progetto** .   
  
3. Nel riquadro [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] **installato** passare a **modelli** \\ **Visual C#** \\ **Windows** e selezionare **applicazione console**.  
  
4. Opzionale Nella casella di testo **nome** Digitare il nome della nuova applicazione.  

5. Fare clic su **OK** per caricare il modello applicazione console.  

6. Seguire le istruzioni in [installazione di SMO](installing-smo.md) per installare il pacchetto per il progetto a cui fare riferimento.
  
7. Scegliere **Codice** dal menu **Visualizza**.
    
8. Nel codice, prima dell'istruzione Namespace, digitare le istruzioni **using** seguenti per qualificare i tipi nello spazio dei nomi SMO:
  
    ```  
    using Microsoft.SqlServer.Management.Smo;  
    using Microsoft.SqlServer.Management.Common;  
    ```  
  
15. SMO dispone di vari spazi dei nomi in Microsoft.SqlServer.Management.Smo, ad esempio Microsoft.SqlServer.Management.Smo.Agent. Aggiungere questi spazi dei nomi poiché sono necessari.  
  
16. A questo punto è possibile aggiungere il codice SMO.  

