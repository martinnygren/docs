---
title: ITU/UTT for distributører
description: ITU/UTT API for distributører
weight: 100
---

> **_INFO:_**  Denne siden inneholder en del tekst som vil bli flyttet inn i OpenAPI-spesifikasjon når denne er klar

## Innledning

Utlegg-api-ekstern er en standardisert maskin-til-maskin tjeneste (API) som kan benyttes av eksterne partnere for innsyn i utleggsdata fra Brønnøysundregistrene.

Dokumentasjonen kan benyttes som veiledning for hvordan eksterne systemer skal integrere seg mot API'et.

Den skal gi et innblikk i hvordan API'et er bygd opp, teknologivalg, hvordan man gjør søk og hvordan man navigerer i API-ets modell.

Implementering av tjenesten krever at integrasjon fra en annen programvare eller system er bygget mot API'et. 

## Syntetiske testdata

Vi har en [Excel-fil med syntetiske testdata](../Testdata ITU-UTT pr 04.10.2019-PPE.xlsx) for personer/virksomheter som det er registrert saker på. Alt er oppkonstruert, både personer, virksomheter og saker.

## API-referanse

Utlegg-api-ekstern tilbyr opplysninger fra Løsøreregisteret om:

* Intet til utlegg og utleggstrekk på fødselsnummer/d-nummer og organisasjonsnummer
* Intet til utlegg på organisasjonsnummer

## Sikkerhetsmekanismer

Siden dette er begrensede API-er så skal kallende parter autentiseres gjennom [Maskinporten](https://difi.github.io/idporten-oidc-dokumentasjon/oidc_guide_maskinporten.html).
Detaljer rundt dette vil publiseres på et senere tidspunkt.

[Regelverk](https://lovdata.no/dokument/SF/forskrift/2015-12-11-1668/%C2%A76): hjemler for tilgjengeliggjøring av data fra Brønnøysundregistrene.

## Grensesnittbeskrivelse

Tjeneren tilbyr følgende funksjonalitet for eksterne systemer/brukere:

Alle kall som brukes for å hente ut data fra API'et bruker GET-metoder i HTTP.

| HTTP-metode   | URL                                                                | Beskrivelse                                                                               |
|:------------- |:------------------------------------------------------------------ |:----------------------------------------------------------------------------------------- |
| GET           | https://\<domain\>/utlegg/v1/distributor/fodselsnummer             | Hent opplysninger om intet til utlegg og utleggstrekk på et fødselsnummer eller d-nummer. |
| GET           | https://\<domain\>/utlegg/v1/enhet/distributor/organisasjonsnummer | Hent opplysninger om intet til utlegg på et organisasjonsnummer.                          |

**Merknader**:

* \<domain\> er ennå ikke avklart.
* \<v1\>: versjonering av API'et

---

### Oppslag på fødselsnummer eller d-nummer

#### Beskrivelse

Tjenesten tar imot en forespørsel om oppslag på et fødselsnummer eller d-nummer, forespørselen valideres før utførelsen og returnerer opplysninger om kun aktive intet til utlegg og utleggstrekk på fødselsnummer eller d-nummer.

#### Request

Tar i mot et fødselsnummer eller d-nummer.

#### Validering

Forespørselen skal alltid inneholde fødselsnummer eller d-nummer på den det gjøres oppslag på.
Dersom forespørselen inneholder et fødselsnummer eller d-nummer som ikke er lovlig oppbygd, returneres det en feilmelding.
Forespørselen skal alltid inneholde opplysninger om organisasjonsnummer til sluttbruker/den som gjør oppslag.
Det sjekkes at sluttbrukers organisasjonsnummer er registrert og ikke slettet i Enhetsregisteret. Dersom det ikke er registrert eller slettet returneres det en feilmelding.

#### Response

Returnerer tilbake et JSON-objekt som inneholder opplysninger om intet til utlegg og utleggstrekk.

Dersom kallet lykkes får man HTTP-status 200 og data fra tjenesten på JSON-format.

Eksempelrespons

```json
{
    "antallITU": 1,
    "antallUTT": 2,
    "utlegg": [
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "UTT",
            "avholdtForretning": "2018-07-30",
            "innfortILosoreregisteret": "2018-08-01",
            "trekkbelop": 500.00,
            "trekkvaluta": "NOK",
            "periodeStart": "2018-08-01",
            "periodeSlutt": "2020-08-10"
        },
        {
            "utleggstype": "UTT",
            "avholdtForretning": "2018-06-10",
            "innfortILosoreregisteret": "2018-08-08",
            "trekkprosent": 17.0,
            "periodeStart": "2018-07-13",
            "periodeSlutt": "2020-07-13"
        }
    ],
    "meldinger": []
}
```

---

### Oppslag på organisasjonsnummer

#### Beskrivelse

Tjenesten tar imot en forespørsel om oppslag på et organisasjonsnummer, forespørselen valideres før utførelsen og returnerer opplysninger om kun aktive intet til utlegg på organisasjonsnummeret.

#### Request

Tar i mot et organisasjonsnummer.

#### Validering

Forespørselen skal alltid inneholde organisasjonsnummer på det foretaket det gjøres oppslag på. 
Dersom forespørselen inneholder et organisasjonsnummer som ikke er lovlig oppbygd, returneres det en feilmelding.
Forespørselen skal alltid inneholde opplysninger om organisasjonsnummeret til sluttbruker/den som gjør oppslag.
Det sjekkes at sluttbrukers organisasjonsnummer er registrert og ikke slettet i Enhetsregisteret. Dersom det ikke er registrert eller slettet returneres det en feilmelding. 

#### Response

Returnerer tilbake et JSON-objekt som inneholder opplysninger om intet til utlegg.

Dersom kallet lykkes får man HTTP-status 200 og data fra tjenesten på JSON-format.

Eksempelrespons

```json
{
    "antallITU": 6,
    "antallUTT": 0,
    "utlegg": [
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        },
        {
            "utleggstype": "ITU",
            "avholdtForretning": "2018-09-04",
            "innfortILosoreregisteret": "2018-09-04"
        }
    ],
    "meldinger": []
}
```

---

## Feilmeldinger

Dersom det ikke finnes noen utlegg, eller ved ugyldig input vil det gis melding om dette i JSON-responsen. Dette ligger i form av en array "meldinger". Eksempel nedenfor.

```json
{
 "antallITU": 0,
 "antallUTT": 0,
 "utlegg": [],
 "meldinger": [
 "Det er ikke registrert opplysninger om intet til utlegg på dette fødselsnummeret/d-nummeret",
 "Det er ikke registrert opplysninger om utleggstrekk på dette fødselsnummeret/d-nummeret"
 ]
}
```

Dersom man ikke får HTTP-status 200, så får man en melding fra tjenesten i JSON-format.

| HTTP-kode   | Feilmelding                                                                                 |
|:----------- |:------------------------------------------------------------------------------------------- |
| 404         | Det er ikke registrert opplysninger om intet til utlegg på dette fødselsnummeret/d-nummeret |
| 404         | Det er ikke registrert opplysninger om utleggstrekk på dette fødselsnummeret/d-nummeret     |
| 404         | Det er ikke registrert opplysninger om intet til utlegg på dette organisasjonsnummeret      |
| 400         | Søkers organisasjonsnummer mangler                                                          |
| 400         | Søkers organisasjonsnummer er ugyldig                                                       |
| 400         | Organisasjonsnummer mangler                                                                 |
| 400         | Ugyldig organisasjonsnummer                                                                 |
| 400         | Fødselsnummer/d-nummer mangler                                                              |
| 400         | Ugyldig fødselsnummer/d-nummer                                                              |

## HTTP-statuskoder

Oversikt over HTTP-statuskoder i API'et.

| HTTP-kode                 | Beskrivelse |
|:------------------------- |:----------- |
| 200 OK                    | Henting av data gikk bra |
| 400 Bad Request           | Feil i spørring. Applikasjonen vil gi en detaljert feilmelding for hva som er feil med spørring |
| 404 Not Found             | Applikasjonen vil gi en detaljert feilmelding for hva som ikke ble funnet. Kan også bety at man bruker feil adresse for tjenesten (i så fall vil man få en standard "404 NOT FOUND" og ikke et svar fra applikasjonen) |
| 500 Internal Server Error | Feil på server side, for eksempel at en underliggende datakilde ikke svarer |

## Ordliste

Definisjoner på begrep som er brukt i denne dokumentasjonen.

| Begrep | Definisjon |
|:------ |:---------- |
| API | Programmeringsgrensesnitt |
| HTTP | Datakommunikasjonsstandard |
| HTTP-statuskoder | Statuskoder for datakommunikasjonsstandard |
| REST | Datakommunikasjonmønster |
| JSON | Åpen standard for dataformat |
| D-nummer | Identifikasjonsnummer tildeles personer med midlertidig tilknytning til Norge, det vil si som ikke er bosatt i Norge. Består av en modifisert sekssifret fødselsdato og et femsifret personnummer. Fødselsdatoen modifiseres ved at det legges til 4 på det første sifferet. |
| Fødselsnummer	| Identifikasjonsnummer fra folkeregistret og består av 11 siffer |
| Personidentifikator | Fødselsnummer eller d-nummer |
| Organisasjonsnummer | Identifikasjonsnummer for organisasjon |
| Utlegg | Intet til utlegg og utleggstrekk |
| ITU | Intet til utlegg |
| UTT | Utleggstrekk |
| Aktive | Med aktive menes de utleggstrekkene eller intet til utlegg som har status GO (godkjent) |

## JSON-schema som brukes for validering av responsen.

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
 "additionalProperties": false,
 "type": "object",
 "required": [
    "antallITU",
 "antallUTT",
 "utlegg"
 ],
 "properties": {
    "antallITU": {
      "type": "integer"
 },
 "antallUTT": {
      "type": "integer"
 },
 "meldinger": {
      "type": "array",
 "items": {"type": ["string", "null"]}
    },
 "utlegg": {
      "type": "array",
 "items": {
        "additionalProperties": false,
 "type": "object",
 "required": [
          "utleggstype",
 "avholdtForretning",
 "innfortILosoreregisteret"
 ],
 "oneOf": [
          {
            "properties": {
              "utleggstype": {"enum": ["UTT"]}
            },
 "required": ["periodeStart", "periodeSlutt"],
 "oneOf": [
              {"required": ["trekkprosent"]},
 {"required": ["trekkbelop", "trekkvaluta"]}
            ]
          },
 {
            "properties": {
              "utleggstype": {"enum": ["ITU"]}
            },
 "not": {
              "anyOf": [
                {"required": ["periodeStart"]},
 {"required": ["periodeSlutt"]},
 {"required": ["trekkprosent"]},
 {"required": ["trekkbelop"]},
 {"required": ["trekkvaluta"]}
              ]
            }
          }
        ],
 "properties": {
          "utleggstype": {
            "type": "string",
 "enum": ["ITU","UTT"]
          },
 "avholdtForretning": {
            "type": "string",
 "format": "date",
 "pattern": "^[12]\\d{3}\\-(0[1-9]|1[012])\\-(0[1-9]|[12][0-9]|3[01])$",
 "examples": "2017-11-28"
 },
 "innfortILosoreregisteret": {
            "type": "string",
 "format": "date",
 "pattern": "^[12]\\d{3}\\-(0[1-9]|1[012])\\-(0[1-9]|[12][0-9]|3[01])$",
 "examples": "2017-06-11"
 },
 "trekkprosent": {
            "type": "number",
 "minimum": 0.01,
 "maximum": 100.00,
 "examples": 50.00,
 "multipleOf": 0.01
 },
 "trekkbelop": {
            "type": "number",
 "examples": 5000.0
 },
 "trekkvaluta": {
            "type": "string",
 "pattern": "^[A-Z]{3}$",
 "examples": "NOK"
 },
 "periodeStart": {
            "type": "string",
 "format": "date",
 "pattern": "^[12]\\d{3}\\-(0[1-9]|1[012])\\-(0[1-9]|[12][0-9]|3[01])$",
 "examples": "2018-01-16"
 },
 "periodeSlutt": {
            "type": "string",
 "format": "date",
 "pattern": "^[12]\\d{3}\\-(0[1-9]|1[012])\\-(0[1-9]|[12][0-9]|3[01])$",
 "examples": "2023-01-16"
 }
        }
      }
    }
  }
}
```
