---
title: MagneticStripeReaderErrorOccured
description: The MagneticStripeReaderErrorOccured event occurs when there is a magnetic stripe reader (MSR) error, such as a scanning error.
ms.assetid: 'c2402411-1bbf-44c1-bf7f-813f6d967822'
ms.date: 9/7/2018
ms.localizationpriority: medium
---

# MagneticStripeReaderErrorOccured

This event occurs when there is a magnetic stripe reader (MSR) error, such as a scanning error.

## Syntax

```cpp
typedef struct _MSR_ERROR_EVENT
{
    PosEventDataHeader Header;
    MsrTrackErrorType Track1Status;
    MsrTrackErrorType Track2Status;
    MsrTrackErrorType Track3Status;
    MsrTrackErrorType Track4Status;
    UnifiedPosErrorSeverity Severity;
    UnifiedPosErrorReason Reason;
    UINT32 ExtendedReason;
    MSR_DATA_RECEIVED CardData;
    wchar_t Message[MSR_ERROR_MAX_MESSAGE_LENGTH];
} MSR_ERROR_EVENT, *PMSR_ERROR_EVENT;
```

The following table shows the memory layout of the data buffer for this event.

| Memory value                                                                   | Description                                                                                                                               |
|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 0x00000009                                                          | **EventType = PosEventType:: MagneticStripeReaderErrorOccurred**                                                               |
| UINT32                                                              | **DataLength** = sizeof(**PosEventDataHeader**) + sizeof(**MSR\_ERROR\_EVENT**)                                                |
| 32-bit [MsrTrackErrorType](https://msdn.microsoft.com/library/windows/hardware/dn772173)                   | **Track1Status**                                                                                                               |
| 32-bit [MsrTrackErrorType](https://msdn.microsoft.com/library/windows/hardware/dn772173)                   | **Track2Status**                                                                                                               |
| 32-bit [MsrTrackErrorType](https://msdn.microsoft.com/library/windows/hardware/dn772173)                   | **Track3Status**                                                                                                               |
| 32-bit [MsrTrackErrorType](https://msdn.microsoft.com/library/windows/hardware/dn772173)                   | **Track4Status**                                                                                                               |
| 32-bit [UnifiedPosErrorSeverity](https://msdn.microsoft.com/library/windows/hardware/dn790053)       | **Severity**                                                                                                                   |
| 32-bit [UnifiedPosErrorReason](https://msdn.microsoft.com/library/windows/hardware/dn790050)           | **Reason**                                                                                                                     |
| UINT32                                                              | **Extended Reason**                                                                                                            |
| 32-bit [MsrCardType](https://msdn.microsoft.com/library/windows/hardware/dn772167)                               | **CardType**                                                                                                                   |
| unsigned char                                                       | **Track1EncryptedDataLength**                                                                                                  |
| unsigned char                                                       | **Track2EncryptedDataLength**                                                                                                  |
| unsigned char                                                       | **Track3EncryptedDataLength**                                                                                                  |
| unsigned char                                                       | **Track4EncryptedDataLength**                                                                                                  |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track1EncryptedDataLength** bytes of encrypted track 1 data                                                                  |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track2EncryptedDataLength** bytes of encrypted track 2 data                                                                  |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track3EncryptedDataLength** bytes of encrypted track 3 data                                                                  |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track4EncryptedDataLength** bytes of encrypted track 4 data                                                                  |
| unsigned char                                                       | **Track1MaskedDataLength**                                                                                                     |
| unsigned char                                                       | **Track2MaskedDataLength**                                                                                                     |
| unsigned char                                                       | **Track3MaskedDataLength**                                                                                                     |
| unsigned char                                                       | **Track4MaskedDataLength**                                                                                                     |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track1MaskedDataLength** bytes of masked track 1 data                                                                        |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track2MaskedDataLength** bytes of masked track 2 data                                                                        |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track3MaskedDataLength** bytes of masked track 3 data                                                                        |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track4MaskedDataLength** bytes of masked track 4 data                                                                        |
| unsigned char                                                       | **Track1DiscretionaryDataLength**                                                                                              |
| unsigned char                                                       | **Track2DiscretionaryDataLength**                                                                                              |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track1DiscretionaryDataLength** bytes of discretionary track 1 data                                                          |
| unsigned char \[MSR\_TRACK\_SIZE\]                                  | **Track2DiscretionaryDataLength** bytes of discretionary track 2 data                                                          |
| unsigned char                                                       | **CardAuthenicationDataLength** - length of the data after encryption, including padding                                       |
| unsigned char                                                       | **CardAuthenticationDataAbsoluteLength** - length of data before encryption (may be needed to strip padding during decryption) |
| unsigned char\[MSR\_ADDITIONAL\_SECURITY\_INFORMATION\_DATA\_SIZE\] | **CardAuthenticationDataAbsoluteLength** bytes of card authentication data                                                     |
| unsigned char                                                       | **AdditionalSecurityInformationLength**                                                                                        |
| unsigned char\[MSR\_ADDITIONAL\_SECURITY\_INFORMATION\_SIZE\]       | **AdditionalSecurityInformationLength** bytes of additional security information                                               |
| wchar\_T \[MSR\_ERROR\_MAX\_MESSAGE\_LENGTH\]                       | Up to **MSR\_ERROR\_MAX\_MESSAGE\_LENGTH** wchar\_t of error **Null**-terminated message text                                  |


## Remarks

If a scanning error occurs, and some scan data was obtained, the event data contains the partial scan data.

## Requirements

**Header:** pointofservicedriverinterface.h
