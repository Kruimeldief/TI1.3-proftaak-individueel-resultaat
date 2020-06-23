# JSON applicaties
Veel applicaties maken gebruik van JSON omdat het een welbekend gestandardiseerde representatie is van
gestructureerde data op basis van de JavaScript object syntax. Enkele voorbeelden van programma's die gebruik
maken van JSON zijn:
- Spellen (bijvoorbeeld Teeworlds).
- Web services zoals RESTful API.
- Web applicaties

### Teeworlds
Net zoals veel andere spellen maakt ook Teeworlds gebruik van JSON om zowel spel- als serverinstellingen op te
slaan. Zo kunnen bijvoorbeeld de gebruikers interface instellingen hieronder gemakkelijk gelezen en geschreven
worden.

```json
{
  "settings": {
    "sidebar_active": 1,
    "sidebar_tab": 0
  },
  "filter": [
    {
      "Favorites": {
        "type": 2,
        "settings": {
          "filter_hash": 3084,
          "filter_ping": 999,
          "filter_address": ""
        }
      }
    },
    {
      "All": {
        "type": 1,
        "settings": {
          "filter_hash": 2060,
          "filter_ping": 999,
          "filter_address": ""
        }
      }
    }
  ]
}
```

### RESTful API
Ook web services zoals de RESTful API maakt gebruik van JSON. Op het moment dat je data wilt opvragen, ontvang je
dit in de vorm van een JSON data object.

```json
{
  "data": [
    {
      "type": "articles",
      "id": "1",
      "attributes": {
        "title": "JSON:API grows a plant!",
        "body": "The shortest article. Ever.",
        "created": "2020-06-23T14:56:29.000Z",
        "updated": "2020-06-23T14:56:28.000Z"
      },
      "relationships": {
        "author": {
          "data": {
            "id": "42",
            "type": "people"
          }
        }
      }
    }
  ],
  "included": [
    {
      "type": "people",
      "id": "42",
      "attributes": {
        "name": "Jack",
        "age": 30
      }
    }
  ]
}
```

### Web applicaties
Niet alleen web applicaties maar ook web widgets maken gebruik van JSON om data op te halen. Denk hierbij bijvoorbeeld
aan de weergave van weerberichten, betaalservicen of social media berichten. Bijvoorbeeld het verzenden en ontvangen
van de temperaturen per uur op een dag wordt gedaan met behulp van JSON data overdrachten. Of het versturen van je
login gegevens op een website.

#### Navigatie
[Index](../readme.md) / [Sprint 1](../week6/sprint1.md) / [Sprint 2](../week6/sprint2.md) / [Sprint 3](../week7/sprint3.md)
/ [Sprint 4](../week7/sprint4.md) / [Stelling reflectie](stelling-reflectie.md) / **JSON applicaties**