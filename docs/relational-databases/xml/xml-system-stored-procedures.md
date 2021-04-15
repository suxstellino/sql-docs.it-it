---
title: Stored procedure XML di sistema | Microsoft Docs
description: Informazioni sulle stored procedure di sistema XML di SQL Server usate per scrivere query con OPENXML.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- system stored procedures [SQL Server], XML
- sp_xml_removedocument
- OPENXML statement, system stored procedures
- sp_xml_preparedocument
- XML [SQL Server], system stored procedures
ms.assetid: e60c7f85-6823-4d28-93d6-b053d08cc830
author: rothja
ms.author: jroth
ms.openlocfilehash: f92b5bb8ec27dd9b15caf8bce013dbd5c3a4e66f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486579"
---
# <a name="xml-system-stored-procedures"></a>Stored procedure XML di sistema
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In SQL Server sono disponibili le stored procedure di sistema seguenti, utilizzate insieme all'istruzione OPENXML:  
  
-   [sp_xml_preparedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)  
  
-   [sp_xml_removedocument &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)  
  
 Per scrivere le query tramite l'istruzione OPENXML, è prima necessario creare una rappresentazione interna del documento XML chiamando la stored procedure **sp_xml_preparedocument**. La stored procedure restituisce un handle per la rappresentazione interna del documento XML. Tale handle viene quindi passato a OPENXML, che restituisce viste di set di righe del documento basate su query XPath. In particolare, vengono restituiti un modello di riga e uno o più modelli di colonna.  
  
> [!NOTE]  
>  L'handle del documento restituito dalla stored procedure **sp_xml_preparedocument** è valido per l'intera durata della sessione.  
  
 È possibile rimuovere la rappresentazione interna del documento XML dalla memoria chiamando la stored procedure di sistema **sp_xml_removedocument** .  
  
## <a name="see-also"></a>Vedere anche  
 [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md)  
  
  
