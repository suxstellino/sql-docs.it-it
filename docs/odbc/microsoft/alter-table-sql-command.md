---
description: ALTER TABLE (comando SQL)
title: Comando ALTER TABLE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- alter table [ODBC]
ms.assetid: 3a01a291-f4d9-43bc-a725-5a95546ff364
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 186647c40517c5ecf77a55e6bbc08155c98ffed9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205756"
---
# <a name="alter-table---sql-command"></a>ALTER TABLE (comando SQL)
Modifica a livello di codice la struttura di una tabella.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
ALTER TABLE TableName1  
   ADD | ALTER [COLUMN] FieldName1  
      FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]  
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
 - Or -  
ALTER TABLE TableName1  
   ALTER [COLUMN] FieldName2  
      [NULL | NOT NULL]  
      [SET DEFAULT eExpression2]  
      [SET CHECK lExpression2 [ERROR cMessageText2]]  
      [DROP DEFAULT]  
      [DROP CHECK]  
 - Or -  
ALTER TABLE TableName1  
   [DROP [COLUMN] FieldName3]  
   [SET CHECK lExpression3 [ERROR cMessageText3]]  
   [DROP CHECK]  
   [ADD PRIMARY KEY eExpression3 TAG TagName2]  
   [DROP PRIMARY KEY]  
   [ADD UNIQUE eExpression4 [TAG TagName3]]  
   [DROP UNIQUE TAG TagName4]  
   [ADD FOREIGN KEY [eExpression5] TAG TagName4  
      REFERENCES TableName2 [TAG TagName5]]  
   [DROP FOREIGN KEY TAG TagName6 [SAVE]]  
   [RENAME COLUMN FieldName4 TO FieldName5]  
   [NOVALIDATE]  
```  
  
## <a name="arguments"></a>Argomenti  
 *TableName1*  
 Specifica il nome della tabella la cui struttura è stata modificata.  
  
 Aggiungi [COLUMN] *FieldName1*  
 Specifica il nome del campo da aggiungere.  
  
 ALTER [COLUMN] *FieldName1*  
 Specifica il nome di un campo esistente da modificare.  
  
 *FieldType* [( *NFieldWidth* [, *nPrecision*]])  
 Specifica il tipo di campo, la larghezza del campo e la precisione del campo (numero di posizioni decimali) per un campo nuovo o modificato.  
  
 *FieldType* è una singola lettera che indica il tipo di [dati](../../odbc/microsoft/visual-foxpro-field-data-types.md)del campo. Per alcuni tipi di dati del campo è necessario specificare *nFieldWidth* o *nPrecision* o entrambi.  
  
 *nFieldWidth* e *nPrecision* vengono ignorati per i tipi D, G, I, L, M, P, T e Y. Per impostazione predefinita, *nPrecision* è zero (senza posizioni decimali) se *nPrecision* non è incluso per i tipi B, F o N.  
  
 NULL &#124; NOT NULL  
 Consente o impedisce i valori null nel campo.  
  
 Se si omette NULL e NOT NULL, l'impostazione corrente di SET NULL determina se nel campo sono consentiti valori null. Tuttavia, se si omette NULL e NOT NULL e si include la clausola PRIMARY KEY o UNIQUE, l'impostazione corrente di SET NULL viene ignorata e il campo non è NULL per impostazione predefinita.  
  
 CONTROLLARE *lExpression1*  
 Specifica una regola di convalida per il campo. *lExpression1* deve restituire un'espressione logica e può essere una funzione definita dall'utente o un stored procedure. Ogni volta che viene aggiunto un record vuoto, viene controllata la regola di convalida. Se la regola di convalida non consente un valore di campo vuoto in un record accodato, viene generato un errore.  
  
 ERRORE *cMessageText1*  
 Consente di specificare il messaggio di errore visualizzato quando la regola di convalida dei campi genera un errore.  
  
 *EEXPRESSION1* predefinito  
 Specifica un valore predefinito per il campo. Il tipo di dati di *eExpression1* deve corrispondere al tipo di dati per il campo.  
  
 PRIMARY KEY  
 Crea un tag di indice primario. Il tag index ha lo stesso nome del campo.  
  
 UNIQUE  
 Crea un tag di indice candidato con lo stesso nome del campo.  
  
> [!NOTE]  
>  Gli indici candidati (creati includendo l'opzione UNIQUE, fornita per la compatibilità ANSI in ALTER TABLE o CREATE TABLE), differiscono dagli indici creati utilizzando l'opzione UNIQUE del comando INDEX. Un indice creato con UNIQUE nel comando INDEX consente le chiavi di indice duplicate; gli indici candidati non consentono chiavi di indice duplicate.  
  
 I valori null e i record duplicati non sono consentiti in un campo usato per un indice primario o candidato.  
  
 Se si sta creando un nuovo campo usando Aggiungi colonna, Visual FoxPro non genererà un errore se si crea un indice primario o candidato per un campo che supporta valori null. Tuttavia, Visual FoxPro genererà un errore se si tenta di immettere un valore null o duplicato in un campo che viene usato per un indice primario o candidato.  
  
 Se si modifica un campo esistente e l'espressione di indice primaria o candidata è costituita da campi della tabella, Visual FoxPro controlla i campi per verificare se contengono valori null o record duplicati. In caso affermativo, Visual FoxPro genera un errore e la tabella non viene modificata.  
  
 FA riferimento al tag *TableName2* *TagName1*  
 Specifica la tabella padre a cui viene stabilita una relazione permanente. TAG *TagName1* specifica il tag di indice della tabella padre su cui si basa la relazione. I nomi dei tag di indice possono contenere un massimo di 10 caratteri.  
  
 NOCPTRANS  
 Impedisce la conversione in una tabella codici diversa per i campi di tipo carattere e memo. Se la tabella viene convertita in un'altra tabella codici, i campi per cui è stato specificato NOCPTRANS non vengono convertiti. NOCPTRANS può essere specificato solo per i campi di tipo carattere e memo.  
  
 Nell'esempio seguente viene creata una tabella denominata MyTable che contiene due campi di tipo carattere e due campi di tipo Memo. Il secondo campo carattere, CHAR2, e il secondo campo Memo, Memo2, includono NOCPTRANS per impedire la conversione.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 ALTER [COLUMN] *FieldName2*  
 Specifica il nome di un campo esistente da modificare.  
  
 IMPOSTA *EEXPRESSION2* predefinito  
 Specifica un nuovo valore predefinito per un campo esistente. Il tipo di dati di *eExpression2* deve corrispondere al tipo di dati per il campo.  
  
 Imposta controllo *lExpression2*  
 Specifica una nuova regola di convalida per un campo esistente. *lExpression2* deve restituire un'espressione logica e può essere una funzione definita dall'utente o un stored procedure.  
  
 ERRORE *cMessageText2*  
 Consente di specificare il messaggio di errore visualizzato quando la regola di convalida dei campi genera un errore. Il messaggio viene visualizzato solo quando i dati vengono modificati in una finestra Sfoglia o modifica.  
  
 DROP DEFAULT  
 Rimuove il valore predefinito per un campo esistente.  
  
 ELIMINA CONTROLLO  
 Rimuove la regola di convalida per un campo esistente.  
  
 DROP [COLUMN] *FieldName3*  
 Specifica un campo da rimuovere dalla tabella. La rimozione di un campo dalla tabella consente inoltre di rimuovere l'impostazione del valore predefinito del campo e la regola di convalida dei campi.  
  
 Se la chiave di indice o le espressioni di trigger fanno riferimento al campo, le espressioni diventano non valide quando il campo viene rimosso. In questo caso, non viene generato un errore quando il campo viene rimosso, ma le espressioni di trigger o la chiave di indice non valide genereranno errori in fase di esecuzione.  
  
 Imposta controllo *lExpression3*  
 Specifica la regola di convalida della tabella. *lExpression3* deve restituire un'espressione logica e può essere una funzione definita dall'utente o un stored procedure.  
  
 ERRORE *cMessageText3*  
 Consente di specificare il messaggio di errore visualizzato quando la regola di convalida della tabella genera un errore. Il messaggio viene visualizzato solo quando i dati vengono modificati in una finestra Sfoglia o modifica.  
  
 ELIMINA CONTROLLO  
 Rimuove la regola di convalida della tabella.  
  
 AGGIUNGERE il tag *eExpression3* della chiave primaria *TagName2*  
 Aggiunge un indice primario alla tabella. *eExpression3* specifica l'espressione della chiave di indice primaria e *TagName2* specifica il nome del tag di indice primario. I nomi dei tag di indice possono contenere un massimo di 10 caratteri. Se TAG *TagName2* viene omesso e *eExpression3* è un singolo campo, il tag di indice primario avrà lo stesso nome del campo specificato in *eExpression3*.  
  
 ELIMINA CHIAVE PRIMARIA  
 Rimuove l'indice primario e il relativo tag di indice. Poiché una tabella può avere una sola chiave primaria, non è necessario specificare il nome della chiave primaria. La rimozione dell'indice primario comporta anche l'eliminazione di eventuali relazioni permanenti in base alla chiave primaria.  
  
 Aggiungi *EEXPRESSION4* univoco [tag *TagName3*]  
 Aggiunge un indice candidato alla tabella. *eExpression4* specifica l'espressione della chiave di indice candidata e *TagName3* specifica il nome del tag di indice candidato. I nomi dei tag di indice possono contenere un massimo di 10 caratteri. Se si omette TAG *TagName3* e se *eExpression4* è un singolo campo, il tag index candidato avrà lo stesso nome del campo specificato in *eExpression4*.  
  
 DROP UNIQUE TAG *TagName4*  
 Rimuove l'indice candidato e il relativo tag di indice. Poiché una tabella può avere più chiavi candidate, è necessario specificare il nome del tag di indice candidato.  
  
 Aggiungi chiave esterna [ *eExpression5*] tag *TagName4*  
 Aggiunge un indice esterno (non primario) alla tabella. *eExpression5* specifica l'espressione di chiave dell'indice esterno e *TagName4* specifica il nome del tag di indice esterno. I nomi dei tag di indice possono contenere un massimo di 10 caratteri.  
  
 REFERENCEs *TableName2*[tag *TagName5*]  
 Specifica la tabella padre a cui viene stabilita una relazione permanente. Includere il TAG *TagName5* per stabilire una relazione basata su un tag di indice esistente per la tabella padre. I nomi dei tag di indice possono contenere un massimo di 10 caratteri. Se si omette il TAG *TagName5*, la relazione viene stabilita usando il tag di indice primario della tabella padre.  
  
 DROP FOREIGN KEY TAG *TagName6*[Save]  
 Elimina una chiave esterna il cui tag di indice è *TagName6*. Se si omette SAVE, il tag index viene eliminato dall'indice strutturale. Includere SAVE per impedire l'eliminazione del tag index dall'indice strutturale.  
  
 Rinominare la colonna *FieldName4* in *FieldName5*  
 Consente di modificare il nome di un campo nella tabella. *FieldName4* specifica il nome del campo che viene rinominato. *FieldName5* specifica il nuovo nome del campo.  
  
> [!CAUTION]  
>  Prestare attenzione quando si rinominano i campi della tabella perché le espressioni di indice, le regole di convalida di campi e tabelle, i comandi e le funzioni possono fare riferimento ai nomi dei campi originali.  
  
 NOVALIDATE  
 Specifica che Visual FoxPro consente di apportare modifiche alla struttura della tabella. Queste modifiche potrebbero violare l'integrità dei dati nella tabella. Per impostazione predefinita, Visual FoxPro impedisce ad ALTER TABLE di apportare modifiche che violano l'integrità dei dati nella tabella. Includere novalidate per eseguire l'override di questo comportamento predefinito.  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare ALTER TABLE per modificare la struttura di una tabella che non è stata aggiunta a un database. Tuttavia, Visual FoxPro genera un errore se si includono le clausole DEFAULT, FOREIGN KEY, PRIMARY KEY, REFERENCEs o SET quando si modifica una tabella gratuita.  
  
 ALTER TABLE può ricompilare la tabella creando una nuova intestazione di tabella e aggiungendo i record all'intestazione della tabella. Se ad esempio si modifica il tipo o la larghezza di un campo, è possibile che la tabella venga ricompilata.  
  
 Dopo la ricompilazione di una tabella, vengono eseguite le regole di convalida dei campi per tutti i campi il cui tipo o larghezza è stato modificato. Se si modifica il tipo o la larghezza di qualsiasi campo nella tabella, viene eseguita la regola della tabella.  
  
 Se si modificano le regole di convalida del campo o della tabella per una tabella con record, Visual FoxPro testa le nuove regole di convalida del campo o della tabella in base ai dati esistenti e genera un avviso sulla prima occorrenza di una regola di convalida di un campo o di una tabella o di una violazione del trigger.  
  
 Se la tabella modificata si trova in un database, ALTER TABLE-SQL richiede l'utilizzo esclusivo del database. Per aprire un database per l'utilizzo esclusivo, includere esclusivo nel DATABASE OPEN.  
  
## <a name="see-also"></a>Vedere anche  
 [Comando CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INDEX (comando)](../../odbc/microsoft/index-command.md)
