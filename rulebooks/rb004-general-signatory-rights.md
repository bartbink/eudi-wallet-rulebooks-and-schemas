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

> 2. Petronella registered her company with the clerk, Bernt Herman Ört, at the Chamber of Commerce. Brent followed local procedures when entering her in the registry making Petronella's Signatory Rights official.

<!--
```
@startuml
title Petronella Malteskog can individually sign on behalf of TB Accounting Firm AB


class Authorisation <<Full Rights>> {
 is_grounded_in
 is_provided_by :
 has_rule :
}

class AuthorisationRule <<sign individually>> {
 authorises :
 joint approval : [<S>Y</S>/N]
}

interface Post_ <<Clerk>> {
 name : Clerk
 held_by_person :
}

interface Post <<Director>> {
 name : Director
 held_by_person :
}

class Person_ <<Bernt>> {
 full name : Bernt Herman Ört
}

class Person <<Petronella>> {
 full name : Petronella Malteskog
}

class Organisation_ <<Chamber of Commerce>> {
 EUID : SEBOLREG.100000-0001
 statutairyName : Chamber of Commerce
 has_post
}

class Organisation <<TB Accounting Firm AB>> {
 EUID : SEBOLREG.556192-9323
 statutairyName : TB Accounting Firm AB
 has_authorisation
 has_post
}

class Ground <<Company Law>> {
 name: Company Law
}

Authorisation::is_provided_by -left-> Post_ #line:DarkRed;text:DarkRed
Authorisation::has_rule -right-> AuthorisationRule #line:DarkRed;text:DarkRed
AuthorisationRule::authorises -right-> Post #line:DarkRed;text:DarkRed
Authorisation::is_grounded_in -up-> Ground #line:DarkRed;text:DarkRed
Organisation::has_post -down-> Post #line:DarkRed;text:DarkRed
Organisation_::has_post -down-> Post_ #line:DarkRed;text:DarkRed
Post::held_by_person -down-> Person  #line:DarkRed;text:DarkRed
Post_::held_by_person -down-> Person_  #line:DarkRed;text:DarkRed

class Authorisation #LightYellow;line:DarkRed;text:DarkRed
interface Post #LightYellow;line:DarkRed;text:DarkRed
class Person #LightYellow;line:DarkRed;text:DarkRed
class AuthorisationRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed

class Ground #LightGreen;line:DarkGreen;text:DarkGreen
interface Post_ #LightGreen;line:DarkGreen;text:DarkGreen
class Person_ #LightGreen;line:DarkGreen;text:DarkGreen
class Organisation_ #LightGreen;line:DarkGreen;text:DarkGreen

hide circle

@enduml

```
-->

![image](https://github.com/user-attachments/assets/9d029166-03fc-445f-9d76-52f5414d7b16)

This second step provides us with the grounds of the right to sign. Since the company law states Petronella in her role as director/owner can sign, it is confirmed by the clerk of the chamber of commerce as a principal by entering the company and the accompanying details in the register.
Here we see **the minimal information needed to state signatory rights at a company confirmed by a principal** as is mandatory in France.  

> 3. Completing the details, and making the model robust.  

To complete the credential model, additional attributes are needed. Also the model is generalised as a class model. For diagram purposes some classes, like Organisation, Post and Person, are repeated in the diagram for readability purposes.  

The following things are added to Signatory Right an Signatory Rule:  
- start date and end date: to ensure the temporal nature of a signatory right.
- name: to add meaning to a right and a rule
- the light yellow entities are the minimum entities to define a signatory right as mentioned on a EUCC.
- cardinalities are added for a stricter model
- the light green entities are optional in most member states. They declare how the signatory rights are grounded and embedded in company law.  

> **NOTE:** An organisation can not sign a document or a contract. It is always a person who is signing for a company. This is a legal matter to be able to trace back the signing to a person acting on behalf of an organisation. 

<!--
```
@startuml
title Signatory Rights model

class SignatoryRight {
 name : string
 is_grounded_in
 is_provided_by :
 has_rule :
 start date : date time
 end date : date time
}

class SignatoryRule {
 name : string
 authorises :
 joint approval : [Y/N]
 jointType : ENUM {Alone, All, One of,
 Majority, At Least x of, explicitly}
 number of mandatees : Integer
 explicit_posts : [Y/N]
 start date : datetime
 end date : date time
}

interface Post_ {
 name : string
 held_by_person :
}

interface Post {
 name : string
 held_by_person :
}

class Person_ {
 full name : string
}

class Person {
 full name : string
}

class Organisation_ {
 EUID : string
 statutairyName : string
 has_post :
 has_signatory_right :
}

class Organisation {
 EUID : string
 statutairyName : string
 has_post :
 has_signatory_right :
}

class Ground {
 name: string
}

SignatoryRight::is_provided_by -left-> "1..n" Post_ #line.dashed:DarkGreen;text:DarkGreen
SignatoryRight::has_rule -right-> "1..n" SignatoryRule #line:DarkRed;text:DarkRed
SignatoryRule::authorises -right-> "1..n" Post #line:DarkRed;text:DarkRed
SignatoryRight::is_grounded_in -up-> "1..n" Ground #line.dashed:DarkGreen;text:DarkGreen
Organisation::has_post -down-> "1..n" Post #line:DarkRed;text:DarkRed
Organisation::has_signatory_right -down-> "1..n" SignatoryRight #line:DarkRed;text:DarkRed
Organisation_::has_post -down-> "1..n" Post_ #line:DarkGreen;text:DarkGreen
Post::held_by_person -down-> "1..n" Person  #line:DarkRed;text:DarkRed
Post_::held_by_person -down-> "1..n" Person_  #line:DarkGreen;text:DarkGreen

class SignatoryRight #LightYellow;line:DarkRed;text:DarkRed
interface Post #LightYellow;line:DarkRed;text:DarkRed
class Person #LightYellow;line:DarkRed;text:DarkRed
class SignatoryRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed

class Ground #LightGreen;line:DarkGreen;text:DarkGreen
interface Post_ #LightGreen;line:DarkGreen;text:DarkGreen
class Person_ #LightGreen;line:DarkGreen;text:DarkGreen
class Organisation_ #LightGreen;line:DarkGreen;text:DarkGreen

hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/f9ac428a-1f34-4084-bcb4-aedfe3363c96)


### 1.2.2 Joint Signatory Rights

Joint Signatory Rights are the case when a multiple of persons can jointly sign any contract or document for an organisation and it is registered at the Chamber of Commerce / registry. This can be the case for a small or large company with multiple owners, a board or management for instance.

**Example**  
Petronella Malteskog is the appointed CEO of TB Accounting Firm AB by the board and has the signatory rights of the company. The board consisting of three people, Agnetha Fältskog,
Björn Ulvaeus and Benny Andersson, also has signatory rights if at least two of the three members sign. This means, Petronella or at least two of the board, can sign any contract, agreement or document for the organisation.  

The board's right is grounded in company law that states that the board, as the management of the organisation registered at the Chamber of Commerce, together with the company by-laws, can do so.  
Petronella has this right because it is mentioned with the company registration at the Chamber of Commerce, together with the company by-laws.  
This is registered by the clerk, Bernt Herman Ört, who followed local procedures when entering her in the registry.  

We will translate this into a model in a few steps:
> 1. TB Accounting Firm AB has a Rule that gives the CEO, a Post held by Petronella Malteskog, and all the members of the board when signing jointly by at least two members, the Authorisation to, individually and jointly respectively, sign on behalf of her company.

<!--
```
@startuml
title Petronella Malteskog can individually sign on behalf of TB Accounting Firm AB


class Authorisation <<Mixed Rights>> {
 has_rule :
}

class AuthorisationRule <<sign explicitly>> {
 authorises :
 joint approval : [Y/<S>N</S>]
 jointType : ENUM {<S>Alone, All, One of,
 <S>Majority, At Least x of</S>, explicitly}
 number of mandatees : 4
 explicit_posts : [Y/<S>N</S>]
}

interface Post1 <<Director>> {
 name : CEO
 has_role
 held_by_person :
}

interface Post2 <<Board member 1>> {
 name : Chair of the board
 has_role
 held_by_person :
}

interface Post3 <<Board member 2>> {
 name : Treasurer of the board
 has_role
 held_by_person :
}

interface Post4 <<Board member 3>> {
 name : Technology Chair
 has_role
 held_by_person :
}
class Person1 <<Petronella>> {
 full name : Petronella Malteskog
}

class Person2 <<Agnetha>> {
 full name : Agnetha Fältskog
}

class Person3 <<Björn>> {
 full name : Björn Ulvaeus
}

class Person4 <<Benny>> {
 full name : Benny Andersson
}

class Organisation <<TB Accounting Firm AB>> {
 EUID : SEBOLREG.556192-9323
 statutairyName : TB Accounting Firm AB
 has_authorisation
 has_post
}

Authorisation::has_rule -right-> AuthorisationRule #line:DarkRed;text:DarkRed

AuthorisationRule::authorises -right-> "alone" Post1 #line:DarkRed;text:DarkRed

<> diamond

AuthorisationRule::authorises - "at least 2" diamond #line:DarkRed;text:DarkRed
diamond - Post2 #line:DarkRed;text:DarkRed
diamond - Post3 #line:DarkRed;text:DarkRed
diamond - Post4 #line:DarkRed;text:DarkRed

Post1 -down-> Post2 #line:White
Post2 -down-> Post3 #line:White
Post3 -down-> Post4 #line:White

Organisation::has_authorisation -down-> Authorisation #line:DarkRed;text:DarkRed
Organisation::has_post -down-> Post1 #line:DarkRed;text:DarkRed
Post1::held_by_person -right-> Person1  #line:DarkRed;text:DarkRed
Post2::held_by_person -right-> Person2  #line:DarkRed;text:DarkRed
Post3::held_by_person -right-> Person3  #line:DarkRed;text:DarkRed
Post4::held_by_person -right-> Person4  #line:DarkRed;text:DarkRed

class Authorisation #LightYellow;line:DarkRed;text:DarkRed
interface Post1 #LightYellow;line:DarkRed;text:DarkRed
interface Post2 #LightYellow;line:DarkRed;text:DarkRed
interface Post3 #LightYellow;line:DarkRed;text:DarkRed
interface Post4 #LightYellow;line:DarkRed;text:DarkRed
class Person1 #LightYellow;line:DarkRed;text:DarkRed
class Person2 #LightYellow;line:DarkRed;text:DarkRed
class Person3 #LightYellow;line:DarkRed;text:DarkRed
class Person4 #LightYellow;line:DarkRed;text:DarkRed
class AuthorisationRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed


hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/e7d41054-415b-438f-9a8e-9947f3bf33e3)



In this first step it clearly states that:  
- The board has the right to sign jointly for the organisation if at least two board members sign.  
- And Petronella has the right to sign individually for the organisation.  

The attributes of Signatory Rule needs some clarification.  
> **authorises:** points to all the involved Posts that are (partly) authorised by this rule.
> **joint approval:** is a boolean that states No if all involved Posts have full signatory rights by signing alone. It states Yes if there is at least one joint signage stated. So in our example it is Yes.
> **jointType:** This is an Enumeration value
> * alone is chosen when all Posts can sign individually
> * all is chosen when all Posts must jointly sign
> * One of is chosen when one of the Posts is needed to sign
> * Majority is chosen when a majority is needed to sign
> * At least x of n is chosen when there is a minimum threshold of Posts to sign needed
> * Explicitly is chosen if none of the above suits the Authorisation Rule and the signatures per Post can be stated explicitly. This is the case in our example.  

> **number of authorised:** states the total number of Posts involved in this rule.  
> **explicit posts:** is a boolean that states if the Authorisation Rule require explicit combination of Posts. This is the case in our example. (This attribute may be obsolete because the Enumeration already states this fact).  

If we would add attributes like start and end date, some other administrative attributes, **this would be the minimal information to state** Petronella's and the board's **signatory rights at the company**. Together with the EUCC, where Petronella and the board members are mentioned as persons who can individually or jointly represent the company, a relying party can trust the credentials (once the persons holding the signatory rights attestation are matched with the persons mentioned on the EUCC). More on this later.


> 2. The board and the CEO are registered as Signatory Rights holders of te company with the clerk, Bernt Herman Ört, at the Chamber of Commerce who followed local procedures when entering her in the registry.

<!--
```
@startuml
title Petronella Malteskog and the board heve signatory rights registered and therefor grounded in Company Law and Bylaws


class SignatoryRight <<Mixed Rights>> {
 is_grounded_in
 is_provided_by :
 has_rule :
}

class SignatoryRule <<sign explicitly>> {
 authorises :
 joint approval : [Y/<S>N</S>]
 jointType : ENUM {<S>Alone, All, One of,
 <S>Majority, At Least x of</S>, explicitly}
 number of authorised : 4
 explicit_posts : [Y/<S>N</S>]
}

interface Post1 <<Director>> {
 name : CEO
 has_role
 held_by_person :
}

interface Post2 <<Board member 1>> {
 name : Chair of the board
 has_role
 held_by_person :
}

interface Post3 <<Board member 2>> {
 name : Treasurer of the board
 has_role
 held_by_person :
}

interface Post4 <<Board member 3>> {
 name : Technology Chair
 has_role
 held_by_person :
}

interface Post5 <<Clerk>> {
 name : Clerk
 held_by_person :
}



class Person1 <<Petronella>> {
 full name : Petronella Malteskog
}

class Person2 <<Agnetha>> {
 full name : Agnetha Fältskog
}

class Person3 <<Björn>> {
 full name : Björn Ulvaeus
}

class Person4 <<Benny>> {
 full name : Benny Andersson
}

class Person5 <<Bernt>> {
 full name : Bernt Herman Ört
}

class Organisation_ <<Chamber of Commerce>> {
 EUID : SEBOLREG.100000-0001
 statutairyName : Chamber of Commerce
 has_post
}

class Organisation <<TB Accounting Firm AB>> {
 EUID : SEBOLREG.556192-9323
 statutairyName : TB Accounting Firm AB
 has_SignatoryRight
 has_post
}

class Ground <<Company Law>> {
 name: Company Law
}

class Ground_ <<Organisation By-Laws>> {
 name: By-Laws
}

SignatoryRight::is_provided_by -left-> Post5 #line:DarkRed;text:DarkRed
SignatoryRight::has_rule -right-> SignatoryRule #line:DarkRed;text:DarkRed
SignatoryRight::is_grounded_in -up-> Ground #line:DarkRed;text:DarkRed
SignatoryRight::is_grounded_in -up-> Ground_ #line:DarkRed;text:DarkRed
SignatoryRule::authorises -right-> "alone" Post1 #line:DarkRed;text:DarkRed

<> diamond

SignatoryRule::authorises - "at least 2" diamond #line:DarkRed;text:DarkRed
diamond - Post2 #line:DarkRed;text:DarkRed
diamond - Post3 #line:DarkRed;text:DarkRed
diamond - Post4 #line:DarkRed;text:DarkRed

Post1 -down-> Post2 #line:White
Post2 -down-> Post3 #line:White
Post3 -down-> Post4 #line:White

Organisation::has_SignatoryRight -down-> SignatoryRight #line:DarkRed;text:DarkRed
Organisation_::has_post -down-> Post5 #line:DarkRed;text:DarkRed
Organisation::has_post -down-> Post1 #line:DarkRed;text:DarkRed

Post1::held_by_person -right-> Person1  #line:DarkRed;text:DarkRed
Post2::held_by_person -right-> Person2  #line:DarkRed;text:DarkRed
Post3::held_by_person -right-> Person3  #line:DarkRed;text:DarkRed
Post4::held_by_person -right-> Person4  #line:DarkRed;text:DarkRed
Post5::held_by_person -down-> Person5  #line:DarkRed;text:DarkRed

class SignatoryRight #LightYellow;line:DarkRed;text:DarkRed
interface Post1 #LightYellow;line:DarkRed;text:DarkRed
interface Post2 #LightYellow;line:DarkRed;text:DarkRed
interface Post3 #LightYellow;line:DarkRed;text:DarkRed
interface Post4 #LightYellow;line:DarkRed;text:DarkRed
class Person1 #LightYellow;line:DarkRed;text:DarkRed
class Person2 #LightYellow;line:DarkRed;text:DarkRed
class Person3 #LightYellow;line:DarkRed;text:DarkRed
class Person4 #LightYellow;line:DarkRed;text:DarkRed
class SignatoryRule #LightYellow;line:DarkRed;text:DarkRed
class Organisation #LightYellow;line:DarkRed;text:DarkRed

interface Post5 #LightGreen;line:DarkGreen;text:DarkGreen
class Person5 #LightGreen;line:DarkGreen;text:DarkGreen
class Organisation_ #LightGreen;line:DarkGreen;text:DarkGreen
class Ground #LightGreen;line:DarkGreen;text:DarkGreen
class Ground_ #LightGreen;line:DarkGreen;text:DarkGreen

hide circle

@enduml
```
-->

![image](https://github.com/user-attachments/assets/87b39fa2-8dbf-438c-8b5d-f238a57e9c1e)


This second step provides us with the grounds of the right to sign. Since the company law and bylaws state that the board can sign any document or agreement if at least two members sign, and Petronella in her role as CEO can sign, it is confirmed by the clerk of the chamber of commerce as a principal by entering the organisation and the accompanying details in the register. Here we see **the minimal information needed to state signatory rights at a company confirmed by a principal as is mandatory in France**.

> 3. Completing the details, and making the model robust.

To complete the credential model, additional attributes are needed. Also the model is generalised as a class model. For diagram purposes some classes, like Organisation, Post and Person, are repeated in the diagram for readability purposes.

The following things are added to Signatory Right an Signatory Rule:

- start date and end date: to ensure the temporal nature of a signatory right.
- name: to add meaning to a right and a rule
- the light yellow entities are the minimum entities to define a signatory right as mentioned on a EUCC.
- cardinalities are added for a stricter model
- the light green entities are optional in most member states. They declare how the signatory rights are grounded and embedded in company law.

> **NOTE:** An organisation can not sign a document or a contract. It is always a person who is signing for a company. This is a legal matter to be able to trace back the signing to a person acting on behalf of an organisation.  

This leads to the same Signatory Rights model:

![image](https://github.com/user-attachments/assets/f9ac428a-1f34-4084-bcb4-aedfe3363c96)

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
