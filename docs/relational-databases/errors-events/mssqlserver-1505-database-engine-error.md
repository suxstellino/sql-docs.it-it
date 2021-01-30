---
description: MSSQLSERVER_1505
title: MSSQLSERVER_1505 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1505 (Database Engine error)
ms.assetid: ef4df75d-0f36-4c8b-b36c-e427f65f91ca
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: eb5140faa2aab243639d2e67232da53a7de1d4e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196940"
---
# <a name="mssqlserver_1505"></a>MSSQLSERVER_1505
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1505|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DUP_KEY|  
|Testo del messaggio|Istruzione CREATE UNIQUE INDEX interrotta. Trovata chiave duplicata per il nome di oggetto '%.\*ls' and index name '%.\*ls'.  Valore della chiave duplicata: %ls.|  
  
## <a name="explanation"></a>Spiegazione  
Questo errore si verifica quando si tenta di creare un indice univoco e il valore duplicato specificato è presente in più di una riga della tabella. Un indice univoco viene creato quando si crea un indice e si specifica la parola chiave UNIQUE o quando si crea un vincolo UNIQUE. Nella tabella non può essere contenuta alcuna riga con valori duplicati nelle colonne definite nell'indice o nel vincolo.  
  
Si considerino i dati nella tabella **Employee** seguente:  
  
|LastName|FirstName|JobTitle|HireDate|  
|------------|-------------|------------|------------|  
|Walters|Rob|Progettista senior di strumenti|2004-11-19|  
|Brown|Kevin|Assistente marketing|NULL|  
|Brown|Jo|Progettista|NULL|  
|Walters|Rob|Progettista di strumenti|2001-08-09|  
  
Non è possibile creare un indice univoco sulle combinazioni delle colonne **LastName** o **LastName**, **FirstName** poiché nelle righe sono presenti valori duplicati.  
  
La potenziale violazione di univocità nella colonna **HireDate** è meno ovvia. Ai fini dell'indicizzazione, i valori NULL vengono considerati uguali. Non è pertanto possibile creare un indice univoco o un vincolo UNIQUE se i valori di chiave sono NULL in più di una riga. In base ai dati precedenti, non è possibile creare un indice univoco nelle combinazioni delle colonne **HireDate** o **LastName**, **HireDate**.  
  
Il messaggio di errore 1505 restituisce la prima riga che viola il vincolo di univocità. Nella tabella possono essere presenti altre righe duplicate. Per individuare tutte le righe duplicate, eseguire una query sulla tabella specificata e utilizzare le clausole GROUP BY e HAVING per segnalarle. Nella query seguente, ad esempio, vengono restituite le righe della tabella **Employee** in cui sono presenti nomi e cognomi duplicati.  
  
SELECT LastName, FirstName, count(*) FROM dbo.Employee GROUP BY LastName, FirstName HAVING count(\*) > 1;  
  
## <a name="user-action"></a>Azione dell'utente  
Prendere in considerazione le seguenti soluzioni:  
  
-   Aggiungere o rimuovere colonne nella definizione dell'indice o del vincolo per creare un indice composto univoco. Nell'esempio precedente, l'aggiunta di una colonna **MiddleName** alla definizione dell'indice o del vincolo potrebbe consentire di risolvere il problema di duplicazione.  
  
-   Selezionare le colonne definite come NOT NULL quando si scelgono le colonne per un indice o un vincolo univoco. In questo modo viene eliminata la possibilità di una violazione di univocità causata più di una riga contiene NULL nei valori della chiave.  
  
-   Se i valori duplicati sono il risultato di errori di immissione di dati, correggere manualmente i dati e quindi creare l'indice o il vincolo. Per informazioni sulla rimozione di righe duplicate in una tabella, vedere l'articolo 139444 della Knowledge Base: [How to remove duplicate rows from a table in SQL Server](https://support.microsoft.com/kb/139444) (Come rimuovere le righe duplicate da una tabella in SQL Server)  
  
## <a name="see-also"></a>Vedere anche  
[CREATE INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/create-index-transact-sql.md)  
[Creare indici univoci](~/relational-databases/indexes/create-unique-indexes.md)  
[Creare vincoli UNIQUE](~/relational-databases/tables/create-unique-constraints.md)  
  
