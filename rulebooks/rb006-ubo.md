

![image](https://github.com/user-attachments/assets/b083bc65-d838-4520-afbf-bb4619d26793)


The provided diagram outlines **draft** version of the EWC-UBO Credential model for the sharing of Ultimate Beneficial Owners (UBOs) within an entity. The model covers the key elements required according to BORIS. The visual layout is easy to understand (for me at least).  
The goal here was to first get the Credential Dfined. In a next iteration namespaces will be added and a serialisation to JSONLD.
  
* The credential layout is based on the Company Certificate credential provided as a first draft.
* The Credential entities and relations are inspired by BORIS. The attribute notation is/should be conform the EU Core Vocabularies.

## Concerns:
* This needs to be extended with namespaces and translated into JsonLD.
* The **credential layout** is copied from the first draft of Company Credential. The layout needs to be verified to be according the ARF.
* **All attributes**, also the metadata attributes need to follow <mark>EU Core Vocabulary definitions</mark> where possible.
* **A credential can be composed of other credentials**, like the LPID in this case.
* **Should the NPID be used in the UBO?**. The Natural person referred to in the UBO credential should not be directly linked to the NPID of that person, because causes dependency where you don't want it.
* The BORIS model **doesn't allow multiple companies** to be connected to the Natural Person. To overcome this, the CredentialSubject should be a split into CredentialSubject and LPID a separate entities.
* BORIS uses the entity Physical Person where EWC uses Natural Person.
* `legallntermediaries` and `ownershipStructure` both reference a string. This results in informal descriptions.
* The model does not accommodate the non disclosure of minors.
* What is the naming convention of credentials?


The primary elements and their relationships are described below:

## Key Classes and Attributes

1. **UBOCredentialsOfAPerson**
   - Represents the credentials of a person related to UBO registration.
   - **Attributes:**
     - `issuer_name`: Name of the credential issuer.
     - `issuer_id`: Identifier of the issuer.
     - `issuer_country`: Country of the issuer.
     - `issuance_date`: Date and time of issuance.
     - `expiry_date`: Date and time of expiry.
     - `authentic_source_id`: ID of the authentic source.
     - `authentic_source_name`: Name of the authentic source.
   - **Associations:**
     - Connected to `CredentialStatus` and `CredentialSchema`.
     - Contains `CredentialSubject`.

2. **CredentialSubject**
   - Represents the subject of the credential.
   - **Attributes:**
     - `BeneficialOwner`: Represents the beneficial owner.
     - `ofLPID`: The Company of the beneficial owner.
     - `registrationDate`: Date of registration.
     - `inactiveDate`: Date of inactivity.
     - `lastChangeDate`: Date of last change.
     - `natureAndExtentOfInterest`: Array describing the nature and extent of interest.
     - `ownershipType`: Enumeration to choose the type of ownership (direct, indirect, both).
     - `legallntermediaries`: Legal intermediaries involved.
     - `ownershipStructure`: Structure of ownership.
     - `PEPStatus`: Politically Exposed Person status.
     - `specificMention`: Boolean for specific mention.
     - `registrationComments`: Comments on registration.
     - `registrationDocuments`: Documents related to registration.
   - **Associations:**
     - Connected to `NaturalPerson`, `LPID`, and `NatureAndExtentOfInterest`.

3. **NatureAndExtentOfInterest**
   - Describes the nature and extent of the interest held by the beneficial owner.
   - **Attributes:**
     - `natureOfIterestHeld`: Enum indicating the nature of interest (control through ownership, voting rights, senior management position, other means).
     - `extent`: Float indicating the extent of the interest. This differs from some registers that register the extent in bracketed values.The advantage of a float is it is precise and accommodates all value limits. Some member states use 10% where others use 25% brackets.
     - `extentDescription`: Description of the extent.

4. **LPID**
   - Represents a linked party identifier.
   - **Attributes:**
     - `EUID`: European Unique Identifier.
     - `StatutoryName`: Statutory name of the entity.

5. **NaturalPerson**
   - Represents a natural person who is the beneficial owner.
   - **Attributes:**
     - `dateOfBirth`: Date of birth.
     - `placeOfBirth`: Location of birth.
     - `citizenship`: Jurisdiction of citizenship.
     - `residency`: Jurisdiction of residency.
     - `domicile`: Address of domicile.
     - `tin`: Tax Identification Number.
     - `naturalPersonNumber`: Natural person number.
     - `identificationDocumentNumber`: ID document number.
     - `isAlive`: Boolean indicating if the person is alive.
     - `firstNames`: First names.
     - `surname`: Surname.
   - **Associations:**
     - Connected to `IdentificationDocument`.

6. **IdentificationDocument**
   - Represents identification documents.
   - **Attributes:**
     - `IDDocumentType`: Enum indicating the type of document (ID, passport, driver's license).

7. **CredentialStatus**
   - Represents the status of the credential.
   - **Attributes:**
     - `id`: URI of the status.
     - `type`: Type of status.

8. **CredentialSchema**
   - Represents the schema of the credential.
   - **Attributes:**
     - `id`: ID of the schema.
     - `type`: Type of schema.

## Associations and Relationships

- **UBOCredentialsOfAPerson** is the main class that connects to `CredentialSubject`, `CredentialStatus`, and `CredentialSchema`.
- **CredentialSubject** links to `NaturalPerson`, `LPID`, and `NatureAndExtentOfInterest`.
- **NaturalPerson** is associated with `IdentificationDocument`.

## The draft in J-son

The J-son draft is not yet in J-son LD, but in J-son for now.

```json
{
  "$schema": "http://json-schema.org/draft-7/schema#",
  "title": "EU - UBO Credentials of person",
  "description": "Schema for EU credentials of a person -  EWC WP3",
  "type": "object",
  "properties": {
    "issuer_name": {
      "description": "Name of issuer from the MS that issued the attestation",
      "type": "string"
    },
    "issuer_id": {
      "description": "LPID Id of the issuing authority. (Business register identifier for BRIS)",
      "type": "string"
    },
    "issuer_country": {
      "description": "Alpha-2 country code, as defined in ISO 3166-1, of the issuing country or territory.",
      "type": "string",
      "minLength": 2,
      "maxLength": 2
    },
    "issuance_date": {
      "description": "Date and possibly time of issuance",
      "type": "string",
      "format": "date-time"
    },
    "expiry_date": {
      "description": "Date and possibly time of LPID expiration",
      "type": "string",
      "format": "date-time"
    },
    "authentic_source_id": {
      "description": "LPID Id of the issuing authority. (Business register identifier for BRIS)",
      "type": "string"
    },
    "authentic_source_name": {
      "description": "Name of issuer from the MS that issued the LPID instance",
      "type": "string"
    },
    "credentialSubject": {
      "description": "Attributes Beneficial Owner Details",
      "type": "object",
      "properties": {
       "beneficialOwner": {
          "description": "will follow",
          "$ref": "#/$defs/NaturalPerson"
        },
        "ofLPID": {
          "description": "will follow",
          "$ref": "#/$defs/LPID"
        },
        "registrationDate": {
          "description": "Date of registration",
          "type": "string",
          "format": "date"
        },
        "inactiveDate": {
          "description": "Date of inactivation",
          "type": "string",
          "format": "date"
        },
        "lastChangeDate": {
          "description": "Date of last change",
          "type": "string",
          "format": "date"
        },
        "natureAndExtentOfInterest": {
          "description": "Nature and extent of interest held",
          "type": "array",
          "items": {
            "$ref": "#/$defs/NatureAndExtentOfInterest"
          }
        },
        "ownershipType": {
          "description": "Type of ownership",
          "type": "string",
          "enum": ["direct", "indirect", "both"]
        },
        "legalIntermediaries": {
          "description": "Legal intermediaries",
          "type": "string"
        },
        "ownershipStructure": {
          "description": "Ownership structure",
          "type": "string"
        },
        "PEPStatus": {
          "description": "PEP status",
          "type": "string"
        },
        "specificMention": {
          "description": "Specific mention",
          "type": "boolean"
        },
        "registrationComments": {
          "description": "Registration comments",
          "type": "string"
        },
        "registrationDocuments": {
          "description": "Registration documents",
          "type": "string"
        }
      },
      "required": [
        "beneficialOwner",
        "ofLPID",
        "registrationDate",
        "natureAndExtentOfInterest",
        "ownershipType"
      ]
    },
    "credentialStatus": {
      "description": "Defines suspension and/or revocation details for the issued credential. Further redefined by the type extension",
      "type": "object",
      "properties": {
        "id": {
          "description": "Exact identity for the credential status",
          "type": "string",
          "format": "uri"
        },
        "type": {
          "description": "Defines the revocation type extension",
          "type": "string"
        }
      },
      "required": [
        "id",
        "type"
      ]
    },
    "credentialSchema": {
      "description": "One or more schemas that validate the Verifiable Credential.",
      "anyOf": [
        {
          "$ref": "#/$defs/credentialSchema"
        },
        {
          "type": "array",
          "items": {
            "$ref": "#/$defs/credentialSchema"
          }
        }
      ]
    }
  },
  "required": [
    "credentialSubject",
    "credentialStatus",
    "credentialSchema",
    "issuer_name",
    "issuer_id",
    "issuer_country",
    "issuance_date",
    "expiry_date",
    "authentic_source_id",
    "authentic_source_name"
  ],
  "$defs": {
    "credentialSchema": {
      "description": "Contains information about the credential schema on which the issued credential is based",
      "type": "object",
      "properties": {
        "id": {
          "description": "References the credential schema stored on the Trusted Schemas Registry (TSR) on which the Verifiable Authorisation is based on",
          "type": "string",
          "format": "uri"
        },
        "type": {
          "description": "Defines credential schema type",
          "type": "string",
          "enum": [
            "FullJsonSchemaValidator2021"
          ]
        }
      },
      "required": [
        "id",
        "type"
      ]
    },
    "NaturalPerson": {
      "type": "object",
      "properties": {
        "dateOfBirth": {
          "type": "string",
          "format": "date"
        },
        "placeOfBirth": {
          "type": "string"
        },
        "citizenship": {
          "type": "string"
        },
        "residency": {
          "type": "string"
        },
        "domicile": {
          "type": "string"
        },
        "tin": {
          "type": "string"
        },
        "naturalPersonNumber": {
          "type": "string"
        },
        "identificationDocumentNumber": {
          "$ref": "#/$defs/IdentificationDocument"
        },
        "isAlive": {
          "type": "boolean"
        },
        "firstNames": {
          "type": "string"
        },
        "surname": {
          "type": "string"
        }
      },
      "required": ["dateOfBirth", "firstNames", "surname"]
    },
    "LPID": {
      "type": "object",
      "properties": {
        "EUID": {
          "type": "string"
        },
        "StatutoryName": {
          "type": "string"
        }
      },
      "required": ["EUID", "StatutoryName"]
    },
    "NatureAndExtentOfInterest": {
      "type": "object",
      "properties": {
        "natureOfInterestHeld": {
          "type": "string",
          "enum": ["control through ownership", "voting rights", "senior management position", "other means"]
        },
        "extent": {
          "type": "number"
        },
        "extentDescription": {
          "type": "string"
        }
      },
      "required": ["natureOfInterestHeld", "extent"]
    },
    "IdentificationDocument": {
      "type": "object",
      "properties": {
        "IDDocumentType": {
          "type": "string",
          "enum": ["ID", "passport", "driverslicence"]
        }
      },
      "required": ["IDDocumentType"]
    }
  }
}
````

