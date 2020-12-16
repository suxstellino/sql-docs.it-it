---
description: Eseguire query delle colonne usando Always Encrypted con enclave sicuri
title: Eseguire query delle colonne usando Always Encrypted con enclave sicuri | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 517e2a0beb7364507d585d9cd729069048cf1c82
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477562"
---
# <a name="query-columns-using-always-encrypted-with-secure-enclaves"></a>Eseguire query delle colonne usando Always Encrypted con enclave sicuri
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Questo articolo illustra le considerazioni generali per l'esecuzione di query su colonne crittografate usando un enclave sicuro lato server per [Always Encrypted con enclave sicuri](always-encrypted-enclaves.md). 

I tipi di query seguenti comportano l'uso di un enclave sicuro:
- Query che attivano operazioni di crittografia sul posto usando chiavi abilitate per l'enclave. Vedere [Configurare la crittografia delle colonne sul posto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md).
- *Query avanzate*: confronti di intervalli o operazioni basate su criteri di ricerca su colonne crittografate con la crittografia casuale e le chiavi abilitate per l'enclave.
- Query che creano o aggiornano indici su colonne crittografate con la crittografia casuale e le chiavi abilitate per l'enclave. Per altre informazioni, vedere [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Le query avanzate e gli indici sono supportati solo nelle colonne abilitate per l'enclave (colonne crittografate con chiavi di crittografia di colonna abilitate per l'enclave) che usano la crittografia casuale. La crittografia deterministica non supporta le query avanzate o gli indici.

> [!NOTE]
> Le query avanzate su colonne crittografate di tipo stringa di caratteri richiedono che le colonne usino le regole di confronto con un ordinamento binary2 (regole di confronto BIN2). 


## <a name="next-steps"></a>Passaggi successivi
- [Eseguire query delle colonne usando Always Encrypted con enclave sicuri con SSMS](always-encrypted-enclaves-query-columns-ssms.md)

## <a name="see-also"></a>Vedere anche
- [Esercitazione: Introduzione ad Always Encrypted con enclave sicuri tramite SSMS](../tutorial-getting-started-with-always-encrypted-enclaves.md)

