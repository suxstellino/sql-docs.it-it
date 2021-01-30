---
description: INDEX (comando)
title: Comando INDEX | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- index command [ODBC]
ms.assetid: 694e8cf5-2f69-4001-9c1e-b735a4da3aff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ac225132e2d89f1edd4baab012b2383cd508588
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183390"
---
# <a name="index-command"></a>INDEX (comando)
Consente di creare un file di indice per visualizzare e accedere ai record di tabella in ordine logico.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
INDEX ON eExpression TO IDXFileName | TAG TagName [OF CDXFileName]  
   [FOR lExpression]  
   [COMPACT]  
   [ASCENDING | DESCENDING]  
   [UNIQUE | CANDIDATE]  
   [ADDITIVE]  
```  
  
## <a name="arguments"></a>Argomenti  
 *eExpression*  
 Specifica un'espressione di indice che può includere il nome di un campo o campi della tabella corrente. Una chiave di indice basata sull'espressione di indice viene creata nel file di indice per ogni record della tabella. Visual FoxPro usa queste chiavi per visualizzare e accedere ai record nella tabella.  
  
> [!NOTE]  
>  Sebbene non sia consigliabile, *eExpression* può anche essere una variabile di memoria, un elemento di matrice o un'espressione di campo o campo da una tabella in un'altra area di lavoro. Non è possibile utilizzare i campi Memo solo nelle espressioni di file di indice; devono essere combinate con altre espressioni di caratteri. Se si accede a un indice che contiene una variabile o un campo che non esiste più o non è possibile individuarlo, Visual FoxPro genera un messaggio di errore.  
  
 Se si tenta di compilare un indice con una chiave che varia di lunghezza, la chiave verrà riempita con spazi. Le chiavi di indice a lunghezza variabile non sono supportate in Visual FoxPro.  
  
 È possibile creare una chiave di indice con lunghezza zero. Se, ad esempio, l'espressione index è una sottostringa di un campo Memo vuoto, viene creata una chiave di indice di lunghezza zero. Una chiave di indice di lunghezza zero genera un messaggio di errore. Quando Visual FoxPro crea un indice, valuta i campi nel primo record della tabella. Se un campo è vuoto, potrebbe essere necessario immettere alcuni dati temporanei nel campo nel primo record per evitare una chiave di indice di lunghezza 0.  
  
 A *IDXFileName*  
 Crea un file di indice. idx. Al file di indice viene assegnata l'estensione predefinita. idx.  
  
 TAG *tagName*[di *CDXFileName*]  
 Crea un file di indice composto. Un file di indice composto è un singolo file di indice costituito da un numero qualsiasi di tag separati (voci di indice). Ogni tag è identificato dal nome univoco del tag. I nomi di tag devono iniziare con una lettera o un carattere di sottolineatura e possono essere costituiti da qualsiasi combinazione di un massimo di 10 lettere, cifre o caratteri di sottolineatura. Il numero di tag in un file di indice composto è limitato solo dalla memoria e dallo spazio su disco disponibili.  
  
 I file di indice composto da più voci sono sempre Compact. Non è necessario includere COMPACT durante la creazione di un file di indice composto. Ai nomi dei file di indice composti viene assegnata un'estensione CDX.  
  
 È possibile creare due tipi di file di indice composti: strutturale e non strutturale.  
  
 **File di indice composti strutturali** È possibile creare un file di indice composto strutturale con TAG *tagName* escludendo la clausola facoltativa di *CDXFileName* . Un file di indice composto strutturale ha sempre lo stesso nome di base della tabella e viene aperto automaticamente al momento dell'apertura della tabella.  
  
 **File di indice composti non strutturali** È possibile creare un file di indice composto non strutturale includendo *CDXFileName* dopo il tag *tagName*. A differenza di un file di indice composto strutturale, è necessario aprire esplicitamente un file di indice composto non strutturale con la clausola INDEX in uso.  
  
 Se è già stato creato e aperto un file di indice composto, l'emissione di un indice con TAG *tagName* aggiunge un tag al file di indice composto.  
  
 PER *lExpression*  
 Specifica una condizione in base alla quale sono disponibili solo i record che soddisfano l'espressione di filtro *lExpression* per la visualizzazione e l'accesso. le chiavi di indice vengono create nel file di indice solo per i record corrispondenti all'espressione di filtro.  
  
 La tecnologia Visual FoxPro Rushmore ottimizza un indice... PER il comando *lExpression* se *lExpression* è un'espressione ottimizzabile. Per ottenere prestazioni ottimali, utilizzare un'espressione ottimizzabile nella clausola FOR.  
  
 COMPACT  
 Crea un file Compact. idx.  
  
 ASCENDENTE  
 Specifica un ordine crescente per il file con estensione CDX. Per impostazione predefinita, i tag. CDX vengono creati in ordine crescente. È possibile includere l'ordine crescente come promemoria dell'ordine del file di indice. Una tabella può essere indicizzata in ordine inverso includendo la decrescente.  
  
 DECRESCENTE  
 Specifica un ordine decrescente per il file con estensione CDX. Non è possibile includere decrescente durante la creazione di file di indice. idx.  
  
 UNIQUE  
 Specifica che solo il primo record rilevato con un valore di chiave di indice specifico è incluso in un file con estensione idx o un tag. CDX. UNIQUE può essere utilizzato per impedire la visualizzazione o l'accesso ai record duplicati. Tutti i record aggiunti con chiavi di indice duplicate vengono esclusi dal file di indice. L'utilizzo dell'opzione UNIQUE di INDEX è identico all'esecuzione di SET UNIQUE ON prima di emettere un indice o un REINDEX.  
  
 Quando un indice univoco o un tag di indice è attivo e un record duplicato viene modificato in un modo che modifica la chiave di indice, viene aggiornato il tag index o index. Tuttavia, non è possibile accedere o visualizzare il record duplicato successivo con la chiave di indice originale fino a quando il file non viene reindicizzato tramite REINDEX.  
  
 CANDIDATO  
 Crea un tag di indice strutturale candidato. La parola chiave CANDIDAta può essere inclusa solo quando si crea un tag di indice strutturale; in caso contrario, Visual FoxPro genera un messaggio di errore.  
  
 Un tag di indice candidato impedisce i valori duplicati nel campo o nella combinazione di campi specificati nell'espressione dell'indice *eExpression*. Il termine *candidato* si riferisce al tipo di indice; Poiché gli indici candidati impediscono valori duplicati, vengono qualificati come un "candidato" come indice primario.  
  
 Visual FoxPro genera un errore se si crea un tag di indice candidato per un campo o una combinazione di campi che contiene già valori duplicati.  
  
 ADDITIVE  
 Mantiene aperto tutti i file di indice aperti in precedenza. Se si omette la clausola ADDITIVe quando si crea un file di indice o un file per una tabella con indice, i file di indice aperti in precedenza (ad eccezione dell'indice composto strutturale) verranno chiusi.  
  
## <a name="remarks"></a>Commenti  
 I record in una tabella con un file di indice vengono visualizzati e accessibili nell'ordine specificato dall'espressione dell'indice. L'ordine fisico dei record della tabella non viene modificato da un file di indice.  
  
## <a name="index-types"></a>Tipi di indice  
 Visual FoxPro consente di creare due tipi di file di indice:  
  
-   File di indice composti. CDX contenenti più voci di indice denominate tag  
  
-   file di indice. idx contenenti una voce di indice  
  
 È anche possibile creare un file di indice composto strutturale, che viene aperto automaticamente con la tabella.  
  
> [!NOTE]  
>  Poiché i file di indice composti strutturali vengono aperti automaticamente al momento dell'apertura della tabella, sono il tipo di indice preferito.  
  
 Includere COMPACT per creare file di indice Compact. idx. I file di indice composti sono sempre Compact.  
  
## <a name="index-order-and-updating"></a>Ordine e aggiornamento degli indici  
 Solo un file di indice (il file di indice master) o il tag (il tag Master) controlla l'ordine in cui la tabella viene visualizzata o a cui si accede. Alcuni comandi (ad esempio, SEEK) utilizzano il tag o il file di indice master per la ricerca di record. Tuttavia, tutti i file di indice Open. idx e. CDX vengono aggiornati in base alle modifiche apportate alla tabella.  
  
## <a name="user-defined-functions"></a>Funzioni definite dall'utente  
 Sebbene un'espressione di indice possa contenere una funzione definita dall'utente, non usare funzioni definite dall'utente in un'espressione di indice. Le funzioni definite dall'utente in un'espressione di indice aumentano il tempo necessario per creare o aggiornare l'indice. Inoltre, gli aggiornamenti dell'indice potrebbero non verificarsi quando viene utilizzata una funzione definita dall'utente per un'espressione di indice.  
  
 Se si usa una funzione definita dall'utente in un'espressione di indice, Visual FoxPro deve essere in grado di individuare la funzione definita dall'utente. Quando Visual FoxPro crea un indice, l'espressione di indice viene salvata nel file di indice, ma solo un riferimento alla funzione definita dall'utente viene incluso nell'espressione dell'indice.  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER TABLE-comando SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Comando DELETE TAG](../../odbc/microsoft/delete-tag-command.md)   
 [IMPOSTA comando COLLATE](../../odbc/microsoft/set-collate-command.md)   
 [SET UNIQUE (comando)](../../odbc/microsoft/set-unique-command.md)
