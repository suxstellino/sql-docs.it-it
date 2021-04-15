---
title: Sintassi di base della clausola FOR XML | Microsoft Docs
description: Informazioni sulla sintassi di base della clausola FOR XML e sul modo in cui viene usata per determinare la forma del codice XML risultante da una query SQL.
ms.custom: fresh2019may
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- BINARY BASE64 directive
- ROOT directive
- FOR XML clause, BINARY BASE64 directive
- FOR XML clause, syntax
- FOR XML clause, ROOT directive
ms.assetid: df19ecbf-d28e-4e9c-aaa3-700f8bbd3be4
author: rothja
ms.author: jroth
ms.openlocfilehash: aaefcc5f69721feeb6fb0ea0d9429241c0f386ef
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489008"
---
# <a name="basic-syntax-of-the-for-xml-clause"></a>Sintassi di base della clausola FOR XML

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La modalità FOR XML può essere RAW, AUTO, EXPLICIT o PATH. Tale modalità determina la forma della struttura XML risultante.  
  
> [!IMPORTANT]  
> La direttiva **XMLDATA** per l'opzione FOR XML è **deprecata**. Utilizzare la generazione XSD in caso di modalità RAW e AUTO. Non sono disponibili sostituzioni per la direttiva XMLDATA in modalità EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]

## <a name="syntax"></a>Sintassi

Di seguito viene indicata la sintassi di base descritta in [Clausola FOR (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md).

```  
[ FOR { BROWSE | <XML> } ]  
<XML> ::=  
XML   
    {   
      { RAW [ ('ElementName') ] | AUTO }   
        [   
           <CommonDirectives>   
           [ , { XMLDATA | XMLSCHEMA [ ('TargetNameSpaceURI') ]} ]
           [ , ELEMENTS [ XSINIL | ABSENT ]   
        ]  
      | EXPLICIT   
        [   
           <CommonDirectives>   
           [ , XMLDATA ]   
        ]  
      | PATH [ ('ElementName') ]   
        [   
           <CommonDirectives>   
           [ , ELEMENTS [ XSINIL | ABSENT ] ]  
        ]  
     }   
  
 <CommonDirectives> ::=   
   [ , BINARY BASE64 ]  
   [ , TYPE ]  
   [ , ROOT [ ('RootName') ] ]  
```  

### <a name="syntax-for-azure-sql-database"></a>Sintassi per il database SQL di Azure

Per la documentazione della clausola SELECT...**FOR XML** applicabile anche al database SQL di Azure, vedere [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md).

## <a name="arguments"></a>Argomenti

**RAW** [('_ElementName_')]  
 Converte ogni riga del set dei risultati della query in un elemento XML con l'identificatore generico, \<row />, come tag dell'elemento. È possibile specificare facoltativamente il nome dell'elemento riga quando si utilizza questa direttiva. La struttura XML risultante utilizzerà il valore *ElementName* specificato come elemento riga generato per ogni riga. Per altre informazioni, vedere [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
**AUTO**  
 Restituisce i risultati della query in un semplice albero XML nidificato. Ogni tabella nella clausola FROM, per cui è specificata almeno una colonna nella clausola SELECT, viene rappresentata come elemento XML. Le colonne elencate nella clausola SELECT vengono mappate agli attributi di elemento appropriati. Per altre informazioni, vedere [Usare la modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
**EXPLICIT**  
 Specifica che la forma dell'albero XML risultante viene definita in modo esplicito. Con questa modalità è tuttavia necessario che le query siano scritte in modo che le informazioni aggiuntive sulla nidificazione desiderata vengano specificate in modo esplicito. Per altre informazioni, vedere [Usare la modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md).  
  
**PATH**  
 Consente di combinare facilmente elementi e attributi, nonché di introdurre una nidificazione aggiuntiva per rappresentare proprietà complesse. È possibile utilizzare le query in modalità FOR XML EXPLICIT per costruire questo tipo di struttura XML da un set di righe, ma la modalità PATH costituisce un'alternativa più semplice. La modalità PATH, insieme alla possibilità di scrivere query FOR XML nidificate e alla direttiva TYPE per restituire istanze di tipo **xml** , consente di formulare più facilmente le query. e rappresenta un'alternativa alla scrittura della maggior parte delle query in modalità EXPLICIT. Per impostazione predefinita, la modalità PATH genera un wrapper dell'elemento \<row> per ogni riga nel set dei risultati. È possibile specificare facoltativamente il nome di un elemento. In tal caso, il nome specificato viene utilizzato come nome dell'elemento wrapper. Se si specifica una stringa vuota (FOR XML PATH ('')), non viene generato alcun elemento wrapper. Per altre informazioni, vedere [Usare la modalità PATH con FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md).  
  
**MLDATA**  
 Specifica che deve essere restituito uno schema XDR (XML-Data Reduced) inline. Lo schema viene aggiunto all'inizio del documento come schema inline. Per un esempio funzionante, vedere [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
**XMLSCHEMA**  
 Restituisce XML Schema W3C (XSD) inline. È possibile specificare facoltativamente un URI dello spazio dei nomi di destinazione quando si specifica questa direttiva. In tal modo, viene restituito lo spazio dei nomi specificato nello schema. Per altre informazioni, vedere [Generare uno schema XSD inline](../../relational-databases/xml/generate-an-inline-xsd-schema.md). Per un esempio funzionante, vedere [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
**ELEMENTS**  
 Se si specifica l'opzione ELEMENTS, le colonne vengono restituite come sottoelementi. In caso contrario, vengono mappate ad attributi XML. Questa opzione è supportata solo con le modalità RAW, AUTO e PATH. È possibile specificare facoltativamente XSINIL o ABSENT quando si utilizza questa direttiva. XSINIL specifica la creazione di un elemento con attributo **xsi:nil** impostato su True per i valori di colonna NULL. Per impostazione predefinita o quando si specifica ABSENT insieme a ELEMENTS, non viene creato alcun elemento per i valori NULL. Per un esempio funzionante, vedere [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md) e [Usare la modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
**BINARY BASE64**  
 Se si specifica l'opzione BINARY Base64, gli eventuali dati binari restituiti dalla query vengono rappresentanti nel formato con codifica Base64. Per recuperare dati binari in modalità RAW ed EXPLICIT, è necessario specificare questa opzione. In modalità AUTO, i dati binari vengono restituiti come riferimento per impostazione predefinita. Per un esempio funzionante, vedere [Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
**TYPE**  
 Specifica che la query restituisce i risultati come dati di tipo **xml** . Per altre informazioni, vedere [Direttiva TYPE nelle query FOR XML](../../relational-databases/xml/type-directive-in-for-xml-queries.md).  
  
**ROOT** [('_RootName_')]  
 Specifica l'aggiunta di un singolo elemento principale alla struttura XML risultante. È possibile specificare facoltativamente il nome dell'elemento radice da generare. Il valore predefinito è `<root>`.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo della modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)   
 [Utilizzo della modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)   
 [Utilizzo della modalità EXPLICIT con FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)   
 [Utilizzare la modalità PATH con FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
