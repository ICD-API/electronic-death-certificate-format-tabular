# Simple Tabular Death Certificate exchange format
This is a specific format used by the [Doris Tool](https://icd.who.int/doris) as a simple tabular alternative to the [standard JSON format](https://github.com/ICD-API/electronic-death-certificate-format).

This format can be used as an Excel (.xlsx) file or as a comma separated text file (.csv). 

You may find small sample files here:
- [Excel sample (.xlsx)](sample.xlsx)
- [Comma separated text file sample (.csv)](sample.csv)

Detailed descriptions of the fields are explained below:

## Data type

The data type used are:

| Type | Description |
| --- | --- |
| `string` | in CSV files, alphanumeric values need to be in quotation marks `"`. |
| `integer` | Numeric field, whole numbers allowed |
| `boolean` | The values allowed are `true` and `false` |
| `date` | The date field used in the certificate is using the format defined in the [W3C](https://www.w3.org/TR/NOTE-datetime). In CSV files, the date value need to be between quotation marks `"`. |
| `durations` | Durations define the amount of intervening time in a time interval used in the certificate for the interval field. The format is defined in the [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601#Durations). In CSV files, the duration value need to be insert between quotation marks `"`.|

Example of date:

```
    Year:
        YYYY (eg 1997)  
    Year and month:  
        YYYY-MM (eg 1997-07)  
    Complete date:  
        YYYY-MM-DD (eg 1997-07-16)  
    Complete date plus hours and minutes:  
        YYYY-MM-DDThh:mmTZD (eg 1997-07-16T19:20+01:00)  
    Complete date plus hours, minutes and seconds:  
        YYYY-MM-DDThh:mm:ssTZD (eg 1997-07-16T19:20:30+01:00)  
    Complete date plus hours, minutes, seconds and a decimal fraction of a second  
        YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)
where:
     YYYY = four-digit year
     MM   = two-digit month (01=January, etc.)
     DD   = two-digit day of month (01 through 31)
     hh   = two digits of hour (00 through 23) (am/pm NOT allowed)
     mm   = two digits of minute (00 through 59)
     ss   = two digits of second (00 through 59)
     s    = one or more digits representing a decimal fraction of a second
     TZD  = time zone designator (Z or +hh:mm or -hh:mm)
```

Example of duration:

```
The duration is represented by the format P[n]Y[n]M[n]DT[n]H[n]M[n]S, P[n]W or P<date>T<time>
In these representations, the [n] is replaced by the value for each of the date and time elements that follow the [n]. Leading zeros are not required, but the maximum number of digits for each element should be agreed to by the communicating parties. The capital letters P, Y, M, W, D, T, H, M, and S are designators for each of the date and time elements and are not replaced.
where:
    P is the duration designator (for period) placed at the start of the duration representation.
    Y is the year designator that follows the value for the number of calendar years.
    M is the month designator that follows the value for the number of calendar months.
    W is the week designator that follows the value for the number of weeks.
    D is the day designator that follows the value for the number of calendar days.
    T is the time designator that precedes the time components of the representation.
    H is the hour designator that follows the value for the number of hours.
    M is the minute designator that follows the value for the number of minutes.
    S is the second designator that follows the value for the number of seconds.

To resolve ambiguity, "P1M" is a one-month duration and "PT1M" is a one-minute duration.

Practical examples:
    "PT10S" is a ten seconds duration
    "PT10M" is a ten minutes duration
    "PT10H" is a ten hours duration
    "P5D" is a five days duration
    "P2W" is a two weeks duration
    "P10M" is a ten months duration
    "P10Y" is a ten years duration
    "", "P" or "PT" is used for unknown interval.
```

```
Note: CauseOfDeath fields can be provided either as code, URI or text. we have individual columns for them. Having just one of them is necessary. (Currently Doris does not support text)
```

## Description

| Attribute | Input/Output | Type | Description |
| --- | --- | --- | --- |
| `CertificateKey` | _input_ | `string` | Can be used to identify the certificate. |
| `ICDVersion` | _input_ | `string` | Specify the ICD revision used for the coding of the certificate. DORIS currently supports `ICD11`  |
| `ICDMinorVersion` | _input_ | `string` | Specify the ICD minor version used for the coding of the certificate associated to the ICD version. |
| `Sex` | _input_ | `string` | 0:Male, - 1:Female, - 9:Unknown |
| `DateBirth` | _input_ | `date` | _see date format above_ |
| `DateDeath` | _input_ | `date` | _see date format above_ |
| `EstimatedAge` | _input_ | `durations` | _see durations format above_ |
| `CauseOfDeathTextA` | _input_ | `string` | Cause field A. Textual conditions.  |
| `CauseOfDeathCodeA` | _input_ | `string` | Cause field A. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”.  |
| `CauseOfDeathURIA` | _input_ | `string` | Cause field A. Classification URI comma separated (Used only for ICD-11). Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `IntervalA` | _input_ | `durations` | Time interval from onset to death for Field A.|
| `CauseOfDeathTextB` | _input_ | `string` | Cause field B. Textual conditions. |
| `CauseOfDeathCodeB` | _input_ | `string` | Cause field B. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `CauseOfDeathURIB` | _input_ | `string` | Cause field B. Classification URI comma separated (Used only for ICD-11). Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `IntervalB` | _input_ | `durations` | Time interval from onset to death for Field B. |
| `CauseOfDeathTextC` | _input_ | `string` | Cause field C. Textual conditions. |
| `CauseOfDeathCodeC` | _input_ | `string` | Cause field C. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `CauseOfDeathURIC` | _input_ | `string` | Cause field C. Classification URI comma separated (Used only for ICD-11). Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `IntervalC` | _input_ | `durations` | Time interval from onset to death for Field C. |
| `CauseOfDeathTextD` | _input_ | `string` | Cause field D. Textual conditions. |
| `CauseOfDeathCodeD` | _input_ | `string` | Cause field D. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `CauseOfDeathURID` | _input_ | `string` | Cause field D. Classification URI comma separated (Used only for ICD-11). Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `IntervalD` | _input_ | `durations`  |Time interval from onset to death for Field D. |
| `CauseOfDeathTextE` | _input_ | `string` | Cause field E. Textual conditions. |
| `CauseOfDeathCodeE` | _input_ | `string` | Cause field E. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `CauseOfDeathURIE` | _input_ | `string` | Cause field E. Classification URI comma separated (Used only for ICD-11). Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `IntervalE` | _input_ | `durations` | Time interval from onset to death for Field E. |
| `CauseOfDeathTextPart2` | _input_ | `string` | Cause field Part2. Textual conditions. |
| `CauseOfDeathCodePart2` | _input_ | `string` | Cause field Part2. Classification codes comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `CauseOfDeathURIPart2` | _input_ | `string` | Cause field Part2. Classification URI comma separated. Its allowed to use post coordination, i.e. “Stem A & Ext 1 / Stem B”. |
| `MaternalDeathWasPregnant` | _input_ | `integer` | 0: No, - 1: Yes, -  9: Unknown |
| `MaternalDeathPregnancyContribute` | _input_ | `integer` |  0: No, - 1: Yes,  - 9: Unknown  |
| `UnderlyingCauseOfDeath` | _input_ | `string` | Manually assigned underlying cause of death provided as code (optional)|
| `UnderlyingCauseOfDeathURI` | _input_ | `string` | Manually assigned underlying cause of death provided as a linearization URI (optional) | 
| `Reject` | _output_ | `boolean` | `false` if the computation was able to select the underlying cause of death, `true` otherwise. The computation can fail for multiple reasons (codes not found in the specific linearization, implausibility of the coding, errors of the system.). The reason can be identified in the file logger or rule logger fields. |
| `Report` | _output_ | `string` | Overview of the steps used by the rule engine to select the Underlaying cause of death. |
| `Errors` | _output_ | `string` | Report field used in case errors occur during computation or rule engine has failed the computation. |
| `Warnings` | _output_ | `string` | Report field used in case the rule engine triggered warnings during the computation. |
| `UnderlyingCauseOfDeathComputed` | _output_ | `string` | Underlying cause of death computed by the system as code. (stem code only) |
| `UnderlyingCauseOfDeathComputedURI` | _output_ | `string` | Underlying cause of death computed by the system as URI (stem code only)|
| `UnderlyingCauseOfDeathComputedComplete` | _output_ | `string` | Underlying cause of death computed by the system (may include postcoordination combination)|
| `UnderlyingCauseOfDeathComputedCompleteURI` | _output_ | `string` | Underlying cause of death computed by the system as linearization URI (may include postcoordination combination) |

