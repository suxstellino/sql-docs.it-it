---
description: Recordset (sintassi ADO per Visual C++)
title: Recordset (sintassi ADO per Visual C++) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- Recordset collection [ADO], ADO for Visual C++ syntax
ms.assetid: affc847c-a533-4c8a-bdff-3682fdb5df5f
author: rothja
ms.author: jroth
ms.openlocfilehash: d675e8349b652c3e0d9e248a34260cc06d5612c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166722"
---
# <a name="recordset-ado-for-visual-c-syntax"></a>Recordset (sintassi ADO per Visual C++)
## <a name="methods"></a>Metodi  
  
```  
AddNew(VARIANT FieldList, VARIANT Values)  
Cancel(void)  
CancelBatch(AffectEnum AffectRecords)  
CancelUpdate(void)  
Clone(LockTypeEnum LockType, _ADORecordset **ppvObject)  
Close(void)  
CompareBookmarks(VARIANT Bookmark1, VARIANT Bookmark2, CompareEnum *pCompare)  
Delete(AffectEnum AffectRecords)  
Find(BSTR Criteria, LONG SkipRecords, SearchDirectionEnum SearchDirection, VARIANT Start)  
GetRows(long Rows, VARIANT Start, VARIANT Fields, VARIANT *pvar)  
GetString(StringFormatEnum StringFormat, long NumRows, BSTR ColumnDelimeter, BSTR RowDelimeter, BSTR NullExpr, BSTR *pRetString)  
Move(long NumRecords, VARIANT Start)  
MoveFirst(void)  
MoveLast(void)  
MoveNext(void)  
MovePrevious(void)  
NextRecordset(VARIANT *RecordsAffected, _ADORecordset **ppiRs)  
Open(VARIANT Source, VARIANT ActiveConnection, CursorTypeEnum CursorType, LockTypeEnum LockType, LONG Options)  
Requery(LONG Options)  
Resync(AffectEnum AffectRecords, ResyncEnum ResyncValues)  
Save(BSTR FileName, PersistFormatEnum PersistFormat)  
Supports(CursorOptionEnum CursorOptions, VARIANT_BOOL *pb)  
Update(VARIANT Fields, VARIANT Values)  
UpdateBatch(AffectEnum AffectRecords)  
```  
  
## <a name="properties"></a>Proprietà  
  
```  
get_AbsolutePage(PositionEnum *pl)  
put_AbsolutePage(PositionEnum Page)  
get_AbsolutePosition(PositionEnum *pl)  
put_AbsolutePosition(PositionEnum Position)  
get_ActiveCommand(IDispatch **ppCmd)  
get_ActiveConnection(VARIANT *pvar)  
put_ActiveConnection(VARIANT vConn)  
putref_ActiveConnection(IDispatch *pconn)  
get_BOF(VARIANT_BOOL *pb)  
get_Bookmark(VARIANT *pvBookmark)  
put_Bookmark(VARIANT vBookmark)  
get_CacheSize(long *pl)  
put_CacheSize(long CacheSize)  
get_CursorLocation(CursorLocationEnum *plCursorLoc)  
put_CursorLocation(CursorLocationEnum lCursorLoc)  
get_CursorType(CursorTypeEnum *plCursorType)  
put_CursorType(CursorTypeEnum lCursorType)  
get_DataMember(BSTR *pbstrDataMember)  
put_DataMember(BSTR bstrDataMember)  
get_DataSource(IUnknown **ppunkDataSource)  
putref_DataSource(IUnknown *punkDataSource)  
get_EditMode(EditModeEnum *pl)  
get_EOF(VARIANT_BOOL *pb)  
get_Filter(VARIANT *Criteria)  
put_Filter(VARIANT Criteria)  
get_LockType(LockTypeEnum *plLockType)  
put_LockType(LockTypeEnum lLockType)  
get_MarshalOptions(MarshalOptionsEnum *peMarshal)  
put_MarshalOptions(MarshalOptionsEnum eMarshal)  
get_MaxRecords(long *plMaxRecords)  
put_MaxRecords(long lMaxRecords)  
get_PageCount(long *pl)  
get_PageSize(long *pl)  
put_PageSize(long PageSize)  
get_RecordCount(long *pl)  
get_Sort(BSTR *Criteria)  
put_Sort(BSTR Criteria)  
get_Source(VARIANT *pvSource)  
put_Source(BSTR bstrConn)  
putref_Source(IDispatch *pcmd)  
get_State(LONG *plObjState)  
get_Status(long *pl)  
get_StayInSync(VARIANT_BOOL *pbStayInSync)  
put_StayInSync(VARIANT_BOOL bStayInSync)  
get_Fields(ADOFields **ppvObject)  
```  
  
## <a name="events"></a>Eventi  
  
```  
EndOfRecordset(VARIANT_BOOL *fMoreData, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
FetchComplete(ADOError *pError, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
FetchProgress(long Progress, long MaxProgress, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
FieldChangeComplete(LONG cFields, VARIANT Fields, ADOError *pError, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
MoveComplete(EventReasonEnum adReason, ADOError *pError, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
RecordChangeComplete(EventReasonEnum adReason, LONG cRecords, ADOError *pError, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
RecordsetChangeComplete(EventReasonEnum adReason, ADOError *pError, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
WillChangeField(LONG cFields, VARIANT Fields, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
WillChangeRecord(EventReasonEnum adReason, LONG cRecords, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
WillChangeRecordset(EventReasonEnum adReason, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
WillMove(EventReasonEnum adReason, EventStatusEnum *adStatus, _ADORecordset *pRecordset)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Recordset (ADO)](./recordset-object-ado.md)