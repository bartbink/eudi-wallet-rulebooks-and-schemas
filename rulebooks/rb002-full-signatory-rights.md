# DRAFT Full Signatory Rights Rulebook DRAFT

4th of November 2024

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

This document is the Full Signatory Rights Rulebook with the minimum of attributes.
It contains a description of Full Signatory Rights, the requirements for the individual Full Signatory Rights, and its issuance process.
This Rulebook contains the following topics: elaboration on the individual Full Signatory Rights, a reference to the attributes, a reference to the generic Full Signatory Rights issuance process, and Trust Infrastructure details.

## 1.2 Elaboration on Full Signatory Rights (SR)

The meaning of Full Signatory Rights (SR) reaches further than what we use here. In our context Full Signatory Rights are stated as follows: **the signing authority or attorney under Corporate Signatory Rights is authorised to sign individually on behalf of the company, and is registered at the Chamber of Commerce or other national registry under Company Law**.

Joint Signatory Rights and Signatory Rights with the complete attributes will be stated separately.  
Any restriction or delegation that can be stated in a Signatory Rights document will be captured in an extension of the Signatory Rights model, being Power of Attorney, Mandate or Authorisation.

### 1.2.1 Full Signatory Rights

**Full Signatory Rights** apply when a single individual has the authority to independently sign any contract or document on behalf of a company, as recorded in the Chamber of Commerce or relevant registry. This setup is common for sole proprietors or small companies with a single owner, for example.

**Example**  

Petronella Malteskog is the director and sole owner of TB Accounting Firm AB, holding full signatory rights for the company. This grants her the authority to sign any contract, agreement, or document on behalf of the business. Her right to sign independently is grounded in company law, which allows sole owners or otherwise of registered companies to act in this capacity. Her registration was processed by clerk Bernt Herman Ört, who followed standard procedures to enter the company in the registry.  

We will translate this into a model in a few steps:
> 1. TB Accounting Firm AB has a Rule that gives the Director, a Post held by Petronella Malteskog, the Authorisation to individually sign on behalf of her company.

<!--
```
@startuml
title Petronella Malteskog can individually sign on behalf of TB Accounting Firm AB


class SignatoryRight <<Full Rights>> {
 has_rule :
}

class SignatoryRule <<sign individually>> {
 authorises :
 joint approval : [<S>Y</S>/N]
}

interface Post <<Director>> {
 name : Director
 has_role
 held_by_person :
}

class Person <<Petronella>> {
 full name : Petronella Malteskog
}

class Organisation <<TB Accounting Firm AB>> {
 EUID : SEBOLREG.556192-9323
 statutairyName : TB Accounting Firm AB
 has_authorisation
 has_post
}

SignatoryRight::has_rule -right-> SignatoryRule #line:DarkRed;text:DarkRed
SignatoryRule::authorises -right-> Post #line:DarkRed;text:DarkRed
Organisation::has_authorisation -down-> SignatoryRight #line:DarkRed;text:DarkRed
Organisation::has_post -down-> Post #line:DarkRed;text:DarkRed
Post::held_by_person -down-> Person  #line:DarkRed;text:DarkRed

class SignatoryRight #LightYellow;line:DarkRed;text:DarkRed
interface Post #LightYellow;line:DarkRed;text:DarkRed
class Person #LightYellow;line:DarkRed;text:DarkRed
class SignatoryRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed


hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/6db4c818-0774-4f22-a7f4-30e725b893d0)

In this first step it clearly states that Petronella has the right to sign individually for her company.  

If we would add attributes like start and end date, some other administrative attributes, **this would be the minimal information to state** Petronella's **signatory rights at het company**. Together with the EUCC, where she is mentioned as a person who can individually represent her company, a relying party can trust the credentials (once the person holding the signatory rights attestation is matched with the person mentioned on the EUCC).  
More on that later.

> 2. Completing the details, and making the model robust.  

To complete the credential model, additional attributes are needed. Also the model is generalised as a class model. For diagram purposes some classes, like Organisation, Post and Person, are repeated in the diagram for readability purposes.  

The following things are added to Signatory Right an Signatory Rule:  
- start date and end date: to ensure the temporal nature of a signatory right.
- name: to add meaning to a right and a rule
- the light yellow entities are the minimum entities to define a signatory right as mentioned on a EUCC.
- cardinalities are added for a stricter model

> **NOTE:** An organisation can not sign a document or a contract. It is always a person who is signing for a company. This is a legal matter to be able to trace back the signing to a person acting on behalf of an organisation. 

<!--
```
@startuml
title Full Signatory Rights model (provision signed by registry)

class SignatoryRight {
 name : string
 is_grounded_in
 is_provided_by :
 has_rule :
 start_date : date time
 end_date : date time
}

class SignatoryRule {
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
 has_post :
 has_signatory_right :
}

SignatoryRight::has_rule -right-> "1..n" SignatoryRule #line:DarkRed;text:DarkRed :"       "
SignatoryRule::authorises -right-> "1..n" Post #line:DarkRed;text:DarkRed :"       "
Organisation::has_signatory_right -down-> "1..n" SignatoryRight #line:DarkRed;text:DarkRed
Post::held_by_person -down-> "1..n" Person  #line:DarkRed;text:DarkRed
Post::has_role -right-> "0..n" Role #line:DarkRed;text:DarkRed :"       "
Post::held_by_organisation -left-> "0..1" Organisation #line:DarkRed;text:DarkRed
Role::in_organisation "1" -down-> "1" Organisation #line:DarkRed;text:DarkRed

class SignatoryRight #LightYellow;line:DarkRed;text:DarkRed
interface Post #LightYellow;line:DarkRed;text:DarkRed
class Person #LightYellow;line:DarkRed;text:DarkRed
class Role #LightYellow;line:DarkRed;text:DarkRed
class SignatoryRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed

hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/1cf00438-9b7a-40a2-8426-e100ac97a8fc)


## 1.3 Goal of the Signatory Rights attestation

The goal of the Signatory Rights attestation is for relying parties to ensure trust in a person representing an organisation and who is the Holder of the Signatory Rights by 
   1\) verifying the integrity of the Signatory Rights. This could for example be done by verifying trusted lists or technical controls against schema.

   2\) validating the Signatory Rights. Validating includes checking that the Signatory Rights is not altered or revoked

The Signatory Rights ensures those two factors and therefore that a Person Representing a Company is trustworthy. 

## 1.4 Key words

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
