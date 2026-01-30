# SQL25

Proof Of Concept demo
- SQL Server 2025
- Create DemoDB with table:
    - Offence - criminal offences
    - Person - information of persons
    - Relationship Offence-Person m:n, as Suspect, Victim, Witness
    - MOT MeansOfTransportration info, e.g. cars. motorcycle, yacht, with Images
    - relationship MoT-Person m:n, as OwnedBy, StolenBy, DrivenBy
    - Generate Vector embeddings of MoT images
    - Create vector embedding of a natural language query, e.g.: list Persons that own a car that can drive faster than 200km/h
    - perform Vector search to return correct results
