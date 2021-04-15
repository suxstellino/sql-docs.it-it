---
title: Gestione NULL (SQLXML)
description: Informazioni su come è possibile specificare attributi o elementi NULL in un updategram SQLXML 4.0 usando l'attributo updg:nullvalue.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- updg:nullvalue attribute
- updategrams [SQLXML], null values
- nullvalue attribute
- null values [SQLXML]
ms.assetid: 5e11eebb-d94e-4ce6-a6d0-870225706bc1
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7a02455ffcc8bea5c2b668eefc62f9268bc651f6
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491489"
---
# <a name="null-handling-sqlxml-40"></a>Gestione dei valori Null (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Nella sintassi XML il valore Null indica un'assenza. Ad esempio, se un attributo o un valore di elemento è NULL, tale attributo o elemento è assente dal documento XML. In [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML **l'attributo updg:nullvalue** consente di specificare NULL per un elemento o un valore di attributo.  
  
 Ad esempio, l'updategram seguente garantisce che il valore **Title** per un contatto con **ContactID** 64 sia NULL e quindi aggiorna il valore **Title** su "Mr". per questo contatto.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync updg:nullvalue="IsNULL"  >  
    <updg:before>  
       <Person.Contact ContactID="64" Title="IsNULL" />  
    </updg:before>  
    <updg:after>  
       <Person.Contact ContactID="64" Title="Mr." />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Quando i parametri vengono passati a un updategram, è possibile passare Null come valore del parametro. Questa operazione viene eseguita specificando **l'attributo nullvalue** nel **\<updg:header>** blocco . Per un esempio, vedere Passaggio di parametri agli [updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/passing-parameters-to-updategrams-sqlxml-4-0.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Considerazioni sulla sicurezza di Updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
