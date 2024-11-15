# DRAFT Full Power of Attorney or Mandate Rulebook DRAFT

6th of November 2024

updated 15th of November 2024

# Table of Contents


- [1 Introduction](#1-introduction)

   - [1.1 Scope ](#11-scope)

   - [1.2 Background](#12-background)

   - [1.3 Goal of the LPID attestation](#13-goal-of-the-lpid-attestation)

   - [1.4 Key words](#14-key-words)

   - [1.5 Terminology](#15-terminology)

- [2 LPID Issuance process](#2-lpid-issuance-process)

- [3 LPID attributes](#3-lpid-attributes)

- [4 Trust infrastructure details](#4-trust-infrastructure-details)

   - [4.1 Trust requirements on the LPID attestation from the perspective of
company registration offices as authentic sources for the LPID](#41-trust-requirements-on-the-lpid-attestation-from-the-perspective-of-company-registration-offices-as-authentic-sources-for-the-lpid)

   - [4.2 Trust a signature or seal over a LPID](#42-trust-a-signature-or-seal-over-a-lpid)

   - [4.3 LPID Provider Trusted List](#43-lpid-provider-trusted-list)

   - [4.4 SD-JWT-compliant PIDs](#44-sd-jwt-compliant-pids)

- [5 References](#5-references)

# 1 Introduction

## 1.1 Scope

This document is the Full Power of Attorney (or Mandate) Rulebook.
It contains requirements specific to the Full Power of Attorney and its issuance process.
This Rulebook contains the following topics: elaboration on Full Power of Attorney, a reference to the attributes, a reference to the Full Power of Attorney issuance process, and Trust Infrastructure details.

## 1.2 Elaboration on Full Power of Attorney

The meaning of Power of Attorney or Mandate in our context is stated as follows: **the authority to solely act for an organisation (or another person comes later) in all matters**.

The Power of Attorney, Mandate or Authorisation extends Signatory Rights from a model point of view. The key point is that one person, who has an authorisation, authorises an other person to act on his or her behalf. If this person is in a role of representing an organisation, the next person can be authorised to represent the organisation in a specific matter.

From here we will call it Mandate.

The designation of the two persons can be the following:
> * a mandator mandates a mandatee.
> * a principal appoints an attorney (not to be confused with a solicitor).
> * an authority authorises an agent.

We will not use this part in the model. This model is the minimum a relaying party needs to trust a person has power of attorney.

### 1.2.1 A mandate

As mentioned before a mandate builds upon the model of Signatory Rights.

**Example**  
To All To Whom These Present Shall Come, M/s A & B Co. Ltd., a company registered under the Companies Act, 1956, and having its registered office at Leiden . (hereinafter referred to as the 'Company)

Whereas the Company is carrying on business of manufacturing and selling pharmaceutical products of various types.

And Whereas the Company has several branches in India including a Branch at Mumbai having Mr. Anil Mehta as Manager of the said Branch at present.

And Whereas in order to facilitate the business carried on at the said branch the Company proposes to appoint the said Mr. Anil Mehta as a Constituted attorney of the Company with following specific powers and authority.

NOW, LET IT BE KNOWN AND WITNESSED BY Mr. Rajesh Sharma, Notary Public, that the Company hereby appoints and constitutes the said Mr. Anil Mehta as its true and lawful attorney or agent, granting full powers and authority to perform and execute all acts, deeds, and matters as specified herein on behalf of, in the name of, and for the Company.

> To open one or more accounts of the Company in the name of the Company with one or more Banks and to operate the same for and on behalf of the Company by drawing, accepting, endorsing negotiating, releasing, paying and or satisfying any promissory notes, bills of exchange, cheques, drafts, hundies or orders for payment of moneys and delivery of securities, goods, or effects or other negotiable instruments and mercantile documents which may be deemed necessary or proper in respect of the business of the Company or its offices at the said Branch.

IN WITNESS WHEREOF the Company has put its seal this first day of July, 2024

The common seal of the said M/s A & B Co. Ltd., is hereto affixed pursuant to the resolution of the Board of Directors dated in the presence of Mr. Steven Pickard Brouwer a Director duly authorised in that behalf, in the presence of Mr. Anil Mehta and Mr. Rajesh Sharma.

So to summerise:

A power of attorney to open and manage a bank account for the Indian Branch of M/s A & B Co. Ltd. is appointed to Mr. Anil Mehta by Mr. Steven Pickard Brouwer. 


We will translate this into a model in a few steps:
> 1. setup the skeleton mandate (remember we will call it a mandate).

<!--
```
@startuml
title A power of attorney to open and manage a bank account


class Mandate <<Mixed Rights>> {
 is_mandated_by :
 has_rule :
}

class MandateRule <<sign explicitly>> {
 mandates :
 joint approval : [<S>Y</S>/N]
 jointType : ENUM {Alone, <S>All, One of,
 <S>Majority, At Least x of, explicitly</S>}
 number of authorised : 1
 explicit_posts : [<S>Y</S>/N]
}

interface Mandatee <<Anil>> {
 name : Anil Mehta
}

class Organisation <<M/s A & B Co. Ltd.>> {
 EUID : NLHR.55431
 statutairyName : M/s A & B Co. Ltd.
 has_Mandate :
}

Mandate::has_rule -right-> MandateRule #line:DarkRed;text:DarkRed

MandateRule::mandates -right-> Mandatee #line:DarkRed;text:DarkRed

Organisation::has_Mandate -down-> Mandate #line:DarkRed;text:DarkRed


class Mandate #LightYellow;line:DarkRed;text:DarkRed
interface Mandatee #LightYellow;line:DarkRed;text:DarkRed
class MandateRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed


hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/4963847c-c928-40ac-a31d-86ce0df53b63)

In this first step it clearly states that Petronella has the right to sign individually for her company. If we would add attributes like start and end date, some other administrative attributes, **this would be the minimal information to state** Petronella's **signatory rights at het company**. Together with the EUCC, where she is mentioned as a person who can individually represent her company, a relying party can trust the credentials (once the person holding the signatory rights attestation is matched with the person mentioned on the EUCC).

> 2. make it robust.

If we would add attributes like start and end date, some other administrative attributes, **this would be the minimal information to state** Petronella's and the board's **signatory rights at the company**. Together with the EUCC, where Petronella and the board members are mentioned as persons who can individually or jointly represent the company, a relying party can trust the credentials (once the persons holding the signatory rights attestation are matched with the persons mentioned on the EUCC).


<!--
```
@startuml
title Full Mandate model (provision issued by organisation)

class Mandate {
 name : string
 is_grounded_in
 is_provided_by :
 has_rule :
 start_date : date time
 end_date : date time
}

class MandateRule {
 name : string
 authorises :
 joint approval : [Y/<s>N</s>]
 jointType : ENUM {Alone, <s>All, One of,
 <s>Majority, At Least x of, explicitly</s>}
 number of mandatees : Integer
 explicit_posts : [<s>Y/</s>N]
 start_date : datetime
 end_date : date time
}

interface Post {
 code : <string>
 name : <string>
 held_by_organisation :
 has_role :
 held_by_person :
}

class Role {
 name :
 in_organisation :
}

class Person {
 full name : string
}

class Organisation {
 EUID : string
 statutairyName : string
 has_mandate :
}

Mandate::has_rule -right-> "1..n" MandateRule #line:DarkRed;text:DarkRed :"       "
MandateRule::authorises -right-> "1..n" Post #line:DarkRed;text:DarkRed :"       "
Organisation::has_mandate -down-> "1..n" Mandate #line:DarkRed;text:DarkRed
Post::held_by_person -down-> "1..n" Person  #line:DarkRed;text:DarkRed
Post::has_role -right-> "0..n" Role #line:DarkRed;text:DarkRed :"       "
Post::held_by_organisation -left-> "0..1" Organisation #line:DarkRed;text:DarkRed
Role::in_organisation "1" -down-> "1" Organisation #line:DarkRed;text:DarkRed

class Mandate #LightYellow;line:DarkRed;text:DarkRed
interface Post #LightYellow;line:DarkRed;text:DarkRed
class Person #LightYellow;line:DarkRed;text:DarkRed
class Role #LightYellow;line:DarkRed;text:DarkRed
class MandateRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed

hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/ca13ab67-8b04-4553-b20a-2aaadff570f3)


## 1.3 Goal of the Mandate attestation

The goal of the Mandate attestation is for relying parties to ensure trust in a person representing an organisation and who is the Holder of the **Signatory Rights by** 
   1\) verifying the integrity of the Signatory Rights. This could for example be done by verifying trusted lists or technical controls against schema.

   2\) validating the Signatory Rights. Validating includes checking that the Signatory Rights is not altered or revoked

The Signatory Rights ensures those two factors and therefore that a Person Representing a Company is trustworthy. 

to be added, PoA chaining all the way up to the Full Signatory Rights.

## 1.4 [I'm here] Key words

This document uses the capitalized key words 'SHALL', 'SHOULD' and 'MAY' as specified in \[RFC 2119\], i.e., to indicate requirements,
recommendations and options specified in this document.

In addition, 'must' (non-capitalized) is used to indicate an external constraint, i.e., a requirement that is not mandated by this document, but, for instance, by an external document. The word 'can' indicates a capability, whereas other words, such as 'will', and 'is' or 'are' are intended as statements of fact.

## 1.5 Terminology

This document uses terminology specified in Regulation (EU) 2024/1183.

# 2 LPID Issuance process

A generic LPID Issuance process has been described by business registries in the EWC pilot. While different business registries have
national processes, there is an agreement on the controls that need to be done before issuing an LPID. Those controls and generic steps are described in RFC-005 chapter 3.0 LPID Issuance process: [RFC-005](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc005-issue-legal-person-identification-data.md)

# 3 LPID attributes

LPID attributes have been decided together by business registries in the EWC pilot.

The LPID SHALL only have two mandatory LPID attributes, the legal person identifier in form of the EUID, and the legal person name(s).

The reason is that the LPID would have to be revoked more often if the LPID attestation would include more attributes which could change over time, and since the LPID is the basis for trust in an organization and is being exchanged in all attestation transactions, this would disturb business processes too often. Other legal person attributes, such as other types of IDs or signatories, can instead be issued in other separate attestations.

The LPID attributes are described in a table in chapter 5.10.3 in RFC-005:
[RFC-005](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc005-issue-legal-person-identification-data.md)

# 4 Trust infrastructure details

In this chapter, trust requirements and general considerations regarding the LPID attestation itself are described.

## 4.1 Trust requirements on the LPID attestation from the perspective of company registration offices as authentic sources for the LPID

In the ARF 1.2. the following information for PID Providers is given.

PID Providers are trusted entities responsible to:

  > ● verify the identity of the EUDI Wallet User in compliance with LoA
high requirements,

  > ● issue PID to the EUDI Wallet in a harmonised common format and

  > ● make available information for Relying Parties to verify the validity of the PID.

The LPID SHALL contain the qualified electronic signature or qualified electronic seal of the issuing body and adhere to the legal requirements defined in Annex VII of the Regulation (EU) 2024/1183.

The LPID SHALL follow the SD-JWT format.

It SHALL not be possible to log into company registers solely with the LPID, since procedures legally require an individual person to act.

LPID Issuers SHALL follow the LPID requirements and trust mechanisms defined by Authentic Sources on national level.

Authentic Sources that are company registration offices need to accept each other's PUB-EAA attestations according to the regulation.
Therefore, common legal trust mechanisms need to be stablished in order for the trust ecosystem to be trustworthy:

  > ●   The LPID unique identifier SHALL be unique and agreed upon on EU and EES level.

  > ●   There SHALL be one common schema for the LPID which is accepted by all company registries offices.

  > ●   Only mandatory metadata and attributes SHALL be present in the LPID attestations.

  > ●   The LPID attestation SHALL be an atomic attestation, i.e. it cannot be enriched with other data, and selective disclosure is not
    possible.

  > ●   The LPID SHALL be in a machine-readable format defined in the ARF during its whole lifecycle.

  > ●   The LPID SHALL be in a format that can scale to additional/new legal forms.

  > ●   The LPID SHALL apply for all legal persons.

  > ●   The issuer of the LPID SHALL be responsible for its revocation.

## 4.2 Trust a signature or seal over a LPID

To trust a signature or seal over an LPID, the Relying Party needs a mechanism to validate that the public key it uses to verify that
signature or seal is trusted. OpenID4VP provides such mechanisms.
However, additional details need to be analysed to fully specify these mechanisms for LPIDs within the EUDI Wallet ecosystem. It is assumed that this will be part of a detailed specification from a standard organization.

## 4.3 LPID Provider Trusted List

For authenticating LPIDs, trust anchors will be used that are present in a LPID Provider Trusted List.

## 4.4 SD-JWT-compliant PIDs

LPID is fully compliant with \[OpenID4VP\] and \[SD-JWT VC\].

# 5 References

RFC-005:
[eudi-wallet-rfcs/ewc-rfc005-issue-legal-person-identification-data.md
at main · EWC-consortium/eudi-wallet-rfcs ·
GitHub](https://github.com/EWC-consortium/eudi-wallet-rfcs/blob/main/ewc-rfc005-issue-legal-person-identification-data.md)

SD-JWT VC:
<https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/>

OpenID4VP:
<https://openid.net/specs/openid-4-verifiable-presentations-1_0.html>
