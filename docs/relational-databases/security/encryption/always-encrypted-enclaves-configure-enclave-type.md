---
title: Configurare l'enclave sicuro in SQL Server
description: Configurare l'enclave sicura per Always Encrypted con enclave sicure in SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: b2bd51dffb858f30e29b4a1b60e1c728afdbeb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067256"
---
# <a name="configure-the-secure-enclave-in-sql-server"></a>Configurare l'enclave sicuro in SQL Server

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Prima di poter usare [Always Encrypted con enclave sicure](always-encrypted-enclaves.md) in SQL Server, è necessario configurare l'istanza per inizializzare l'enclave sicura durante l'avvio. Per impostazione predefinita, SQL Server non inizializza l'enclave sicura. È possibile modificare questo comportamento impostando l'opzione di configurazione del server **column encryption enclave type** sul valore che rappresenta un tipo di enclave valido per l'ambiente in uso.

> [!NOTE]
> Il ruolo responsabile della configurazione dell'enclave sicura è l'amministratore di database. Vedere [Ruoli e responsabilità per la configurazione dell'attestazione con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Il tipo di enclave supportato per [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] è la sicurezza basata sulla virtualizzazione (VBS). Prima di configurare il tipo di enclave VBS, è consigliabile configurare l'attestazione con il servizio Sorveglianza host (HGS) per il computer che ospita l'istanza. Per iniziare a usare HGS, vedere [Pianificare l'attestazione del servizio Sorveglianza host](always-encrypted-enclaves-host-guardian-service-plan.md). La configurazione dell'attestazione abilita anche la sicurezza basata sulla virtualizzazione, necessaria per l'inizializzazione corretta di un'enclave VBS. Per altre informazioni, vedere [Verificare che sia in esecuzione la sicurezza basata sulla virtualizzazione](always-encrypted-enclaves-host-guardian-service-register.md#step-2-verify-virtualization-based-security-is-running).

Per istruzioni dettagliate su come configurare il tipo di enclave, vedere [Configurare il tipo di enclave per l'opzione di configurazione del server Always Encrypted](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md).

## <a name="next-steps"></a>Passaggi successivi

 [Gestire le chiavi per Always Encrypted con enclave sicuri](always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Vedere anche  
 
 [Opzioni di configurazione del server (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)
