---
description: Creazione di file di valori di variabile (AccessToSQL)
title: Creazione di file di valori di variabile (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 808595c3-8ef1-40bd-a93e-5cf237950e08
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7653d131ca9f345353de46b2579735a527bca5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044931"
---
# <a name="creating-variable-value-files-accesstosql"></a>Creazione di file di valori di variabile (AccessToSQL)
Un file di valori di variabile è un file XML che include i valori dei parametri dei comandi, ad esempio il nome del server di origine o di destinazione, che cambiano di frequente tra le migrazioni del server. Quando si verifica un numero elevato di migrazioni di database, vengono creati più file di variabili per l'archiviazione del valore di ogni server di origine a cui viene fatto riferimento in un file di script Master con l'opzione **-v** nella riga di comando. Questo comportamento consente di mantenere i valori statici in pochi file di script con i valori delle variabili in più file variabili.  
  
> [!NOTE]  
> -  I nomi delle variabili hanno il prefisso e il suffisso è un simbolo di $ (dollaro). Se a una variabile non viene assegnato un valore nel file di valori di variabile, si verificherà un errore durante l'analisi del file di script, con conseguente blocco del processo di esecuzione della console.  
> -  Il carattere di escape per **$** è **$$** . Se il valore di una variabile o di un valore statico di un parametro contiene un **$** simbolo (dollaro), **$$** è necessario specificare per considerarlo come un carattere anziché come variabile.  
> -  Ai fini della gestibilità, le variabili possono essere dichiarate all'interno di `'variable-group'` elementi per la separazione logica di variabili definite dall'utente.  L'utilizzo di questo elemento non è obbligatorio.  
  
**Esempi:**  
  
**Esempio 1:**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$type$" value="MyProject"/>  
  
    <variable name="$project_folder$" value=".\$project_name$"/>  
  
    <variable name="$project_name$" value="$type$ConsoleProject"/>  
  
    <variable name="$project_overwrite$" value="true"/>  
  
    <variable name="$project_type$" value="sql-server-2008"/>  
  
  </variable-group>  
  
</variables>  
```  
**Esempio 2:**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetServerName$" value="xxx"/>  
  
      <variable name="$TargetDB$" value="xxx"/>  
  
      <variable name="$TargetUserName$" value="xxx"/>  
  
      <variable name="$TargetPassword$" value="xxx"/>  
  
      <variable name="$TargetIsTrusted$" value="xxx"/>  
  
      <variable name="$TrustedConnection$" value="xxx"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="TestTable1"/>  
  
      <variable name="$ObjectName2$" value="TestProc1"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Convalida file di valori di variabile  
L'utente può convalidare facilmente il proprio file di valori di variabile rispetto al file di definizione dello schema **ConsoleScriptVariablesSchema. xsd** disponibile nella cartella "schemi".  
  
## <a name="next-step"></a>Passaggio successivo  
Il passaggio successivo per la gestione della console consiste nel [creare i file di connessione del Server &#40;AccessToSQL&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
## <a name="see-also"></a>Vedi anche  
[Creazione dei file di connessione del server (accesso)](./creating-the-server-connection-files-accesstosql.md)  
