# Let's Standardize the 1970 Epoch!

The 1970 epoch is one of those things everyone uses and nobody has ever actually written down. It survives through folklore and muscle memory. We all know what “seconds since 1970” means, but try to find a formal definition you can cite and the whole thing dissolves into hand‑waving. Even modern documents fall back on phrases like “seconds since the start of 1970 UTC, ignoring leap seconds,” which sounds authoritative until you remember that UTC didn’t exist in its modern form until 1972 and leap seconds don’t politely vanish just because a standard tells them to.

Add in the awkward fact that “Unix” is a trademark, and our most widely used timestamp turns out to be a cultural artefact rather than a specification. So, rather than continuing to rely on tradition and vibes, this document proposes a precise, historically honest definition of the thing we’ve all been using anyway.

This is draft version 0.1 of a possible RFC and I will update this version if I make any substantive edits post-publication. (I’ll reserve versions 1.0 and above for IETF submissions.)

## RFC: 1970 Epochal Time
### Status of This Memo
This document is an informational specification. It does not define an Internet Standard.

### Abstract
This document defines 1970 Epochal Time, a timestamping system that assigns integer values to non‑leap SI seconds beginning at 1972‑01‑01T00:00:00Z. It provides a precise, citable definition of the widely used “seconds since 1970” convention while avoiding the historical ambiguities associated with pre‑1972 UTC and the trademark restrictions associated with the term “Unix time”. The system preserves numerical compatibility with existing Unix timestamps for all values greater than or equal to 63 072 000.

### 1. Introduction
The timestamp convention commonly described as “seconds since 1970” is widely used in computing systems, but it has never been formally defined in a citable standards document. Existing practice typically describes this value as the number of elapsed seconds since 1970‑01‑01T00:00:00Z, ignoring leap seconds. This description is imprecise and ambiguous for several reasons.

First, Coordinated Universal Time (UTC) in its modern, leap‑second‑regulated form was not introduced until 1972. No authoritative mapping exists between modern UTC and the years 1970–1971, making it unclear which exact instant in 1970 such timestamps are anchored to. Second, the instruction to “ignore leap seconds” does not specify how timestamps should behave during positive or negative leap seconds, creating ambiguity for both historical and future dates. Third, the term “Unix” is a registered trademark and is therefore unsuitable as the name of a formal standard.

This document defines 1970 Epochal Time, a timestamping system that provides a precise, citable, and historically consistent definition of the widely used “seconds since 1970” convention. It establishes the base of this system at the beginning of 1972, the first moment at which UTC was defined in its current form, and specifies the behaviour of timestamps in the presence of leap seconds while preserving numerical compatibility with existing Unix timestamps for all values from that point forward.

This document is a formalization of widespread existing practice only. It does not attempt to repair or improve those practices.

### 2. Terminology
**UTC** — Coordinated Universal Time, including leap‑second adjustments.  
**SI second** — the base unit of time as defined by the International System of Units.  
**Non-leap second** — the first 86400 seconds of each UTC day, not including a positive leap-second.  
**Positive leap second** — a UTC day extended to 86 401 seconds by inserting 23:59:60 (described in ITU‑R TF.460 as the insertion of a leap second).  
**Negative leap second** — a UTC day shortened to 86 399 seconds by omitting a second (described in ITU‑R TF.460 as the deletion of a leap second).  

### 3. Epochal Timestamp Definition and Construction
An **Epochal Timestamp** is an integer assigned to each non‑leap SI second beginning at 1972‑01‑01T00:00:00Z. It is constructed from a Day‑Number and a Second‑Number.

A **Day Number** is the count of days where the UTC date 1972‑01‑01 is Day‑Number 730 and increasing by one every UTC day.

A **Second Number** is the count of non‑leap SI seconds within a UTC day, beginning at 0 at the start of the day and increasing by one each SI second.

The Epochal Timestamp value itself is computed as:
```
Epochal-Timestamp = (Day-Number × 86400) + Second-Number
```
All Epochal Timestamp values less than 63 072 000 are undefined by this document. Dates prior to 1972‑01‑01 have no defined Epochal Timestamp value. Other specifications may assign meaning to these values and dates, but such interpretations are outside the scope of this document.

### 4. Leap Seconds
On UTC days containing a positive leap second, the inserted second (23:59:60) has no defined Epochal Timestamp value. The sequence proceeds directly from the Epochal Timestamp corresponding to 23:59:59 to the Epochal Timestamp corresponding to 00:00:00 of the next day.

On UTC days containing a negative leap second, the UTC day is 86 399 seconds long. The final valid Second Number for that day is 86 398. The Epochal Timestamp value that would have corresponded to the omitted second is undefined and the sequence remains continuous.

### 5. Note on Sub‑Second Extensions (Non‑Normative)
This document defines only integer Epochal Timestamp values. Specifications referencing this document may extend the representation of time into sub‑second precision using fractional values, fixed‑precision multipliers (such as milliseconds or nanoseconds), or any other encoding appropriate to the referencing specification. Sub‑second representation is outside the scope of this document.

### 6. Note on Clock Accuracy (Non‑Normative)
This document defines only the mapping between non-leap UTC seconds and integer Epochal Timestamp values. It does not specify how systems obtain or maintain the current time. Implementations may use local oscillators, network time protocols, hardware clocks, or any other mechanism to determine the present UTC second. The accuracy of the resulting Epochal Timestamp value depends on the accuracy of the underlying time source. Incorrect, drifting, or unsynchronized clocks will produce incorrect Epochal Timestamp values. This specification does not guarantee that any particular system will identify UTC seconds correctly.

### 7. Compatibility
All Epochal Timestamp values greater than 63 072 000 are numerically identical to Unix time as commonly implemented. This document does not define the meaning of timestamps below this value.

### 8. Security Considerations
This document defines a method for assigning integer values to UTC seconds beginning at 1972‑01‑01T00:00:00Z. It introduces no new security mechanisms and does not address authentication, confidentiality, integrity, or access control.

Incorrect or maliciously altered time sources may cause implementations to generate incorrect Epochal Timestamp values. Systems that rely on timestamps for security‑relevant decisions—such as expiration checks, replay protection, or log ordering—must ensure that their underlying time sources are trustworthy and appropriately synchronized. This specification does not define any mechanism for validating or securing time information.

Ambiguities arising from timestamps outside the defined range (values less than 63 072 000) may lead to misinterpretation if implementations assign meaning to undefined values. Specifications that extend or reinterpret these values must consider the security implications of doing so.

### 9. IANA Considerations
This document requires no IANA actions.

### 10. Acknowledgements
The author acknowledges the widespread informal use of “seconds since 1970”.

“UNIX” and “Unix”, at time of writing, are trademarks of The Open Group.
