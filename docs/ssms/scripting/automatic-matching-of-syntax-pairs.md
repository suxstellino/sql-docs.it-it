---
title: Corrispondenza automatica di coppie della sintassi
description: Informazioni sulla corrispondenza automatica di coppie della sintassi nell'editor di query (corrispondenza tra delimitatori), nell'editor di query XMLA (corrispondenza tra parentesi graffe) e nell'editor MDX e DMX (corrispondenza tra parentesi).
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], delimiter highlighting
- IntelliSense [SQL Server], syntax pair matching
ms.assetid: bfc54cda-bfd6-4545-a5b9-f9db2ae13769
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbc0daf1948c72f66f0c4a5b12d4d21af535ca0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354778"
---
# <a name="automatic-matching-of-syntax-pairs"></a>Corrispondenza automatica di coppie della sintassi
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La corrispondenza automatica di coppie della sintassi consente di verificare immediatamente se gli elementi della sintassi che devono essere codificati in coppie sono abbinati correttamente. Questa corrispondenza è nota come corrispondenza tra delimitatori nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , corrispondenza tra parentesi graffe nell'editor di query XMLA di Analysis Services e corrispondenza tra parentesi negli editor MDX e DMX.  
  
## <a name="database-engine-query-editor-delimiter-matching"></a>Corrispondenza tra delimitatori nell'editor di query del Motore di database  
 Nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene individuata la corrispondenza tra i delimitatori che identificano i limiti dei blocchi di codice. Ciò avviene in due modi:  
  
-   L'editor evidenzia entrambi delimitatori di una coppia quando si finisce di digitare il secondo delimitatore della coppia.  
  
-   Ogni volta che il cursore si trova in uno dei delimitatori di una coppia, è possibile utilizzare il tasto di scelta rapida CTRL+] per passare al delimitatore corrispondente.  
  
### <a name="delimiter-pairs"></a>Coppie di delimitatori  
 La corrispondenza tra delimitatori automatica riconosce i seguenti set di delimitatori:  
  
|Delimitatore iniziale|Delimitatore di chiusura|  
|--------------------|-----------------------|  
|**(**|**)**|  
|**BEGIN**|**END**|  
|**BEGIN TRY**|**END TRY**|  
|**BEGIN CATCH**|**END CATCH**|  
  
 La corrispondenza tra delimitatori automatica non riconosce i delimitatori per gli identificatori inseriti tra parentesi ([NomeOggetto]) o gli identificatori tra virgolette ("NomeOggetto"). La corrispondenza di coppie non abbina le virgolette singole per i valori letterali stringa ('stringa') perché la codifica dei colori indica già se la stringa è stata delimitata o meno.  
  
### <a name="delimiter-highlighting"></a>Evidenziazione dei delimitatori  
 La corrispondenza evidenzia sia l'elemento iniziale che l'elemento di chiusura di una coppia di delimitatori. In questo modo è possibile identificare visivamente blocchi di codice e verificare eventuali coppie di delimitatori non corrispondenti.  
  
 I delimitatori vengono evidenziati quando si digita la lettera finale che completa la coppia. Per una coppia BEGIN END in cui si digita prima BEGIN e successivamente END, ad esempio, l'evidenziazione viene attivata quando si digita la lettera finale in END. Non è necessario digitare il delimitatore iniziale seguito dal delimitatore di chiusura per attivare l'evidenziazione. Se si digita END come primo delimitatore e successivamente si scorre all'indietro lo script per digitare BEGIN, l'evidenziazione viene attivata quando si digita la lettera finale in BEGIN. Non è necessario che la lettera finale digitata sia la lettera finale del delimitatore. Se ad esempio il delimitatore BEGIN viene scritto in modo errato come BEIN, la coppia BEGIN END viene evidenziata quando si inserisce la lettera G finale.  
  
 La coppia di delimitatori rimane evidenziata fino a quando non si sposta il cursore. L'evidenziazione viene disattivata quando il cursore viene spostato, anche se la nuova posizione del cursore non è diversa rispetto al delimitatore. È possibile attivare di nuovo l'evidenziazione eliminando e ridigitando qualsiasi lettera in un membro della coppia.  
  
## <a name="analysis-services-xmla-query-editor-brace-matching"></a>Corrispondenza tra parentesi graffe nell'editor di query XMLA di Analysis Services  
 La corrispondenza tra parentesi graffe nell'editor di query XMLA indica se sono stati chiusi elementi evidenziando parentesi graffe di apertura e di chiusura. È inoltre possibile utilizzare il tasto di scelta rapida CTRL+] per passare da una parentesi a quella corrispondente.  
  
 Nell'editor di query XMLA la corrispondenza tra parentesi graffe viene eseguita per i termini seguenti:  
  
-   Tag di inizio e di fine corrispondenti.  
  
-   Qualsiasi coppia di parentesi angolari "\<" and ">".  
  
-   Inizio e fine di commenti.  
  
-   Inizio e fine di istruzioni di elaborazione.  
  
-   Inizio e fine di blocchi CDATA.  
  
-   Inizio e fine di dichiarazioni DTD.  
  
-   Virgolette di apertura e di chiusura sugli attributi.  
  
## <a name="mdx-and-dmx-editor-parenthesis-matching"></a>Corrispondenza tra parentesi negli editor DMX e MDX  
 Negli editor MDX (Multidimensional Expressions) e DMX (Data Mining Expressions) vengono messe automaticamente in corrispondenza le coppie di parentesi nelle funzioni.
