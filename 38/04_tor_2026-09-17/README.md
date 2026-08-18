# Kode review med pull requests


## Underviser: Benjamin



## Beskrivelse

Vi skal arbejde med at kvalitetssikre koden ved hjælp af pull requests.
Når en udvikler pusher sin kode til Github, skal resten af teamet (eller udvalgte teammedlemmer) i en pull request
kunne reviewe og diskutere de nye ændringer, før de integreres ind i main branch.



## Læringsmål

- At kunne anvende GitHub flow workflow.
- At kunne anvende pull requests i Github til kodereviews.



## Forberedelse

Inden undervisningen skal i skrive en prompt til et AI-værktøj der forberede jer på læringsmålene ovenfor. Når vi mødes i klassen vælger jeg en der skal vise deres prompt og svaret. Tænk over om i føler at i har lært noget af at forberede jer med AI. 



## Peer instruction

<!--

### Spørgsmål 1

Hvad er en pull request egentlig?

A. En pull request er en kopi af hele repositoryet
B. En pull request er et forslag om at merge forskellen mellem to branches
C. En pull request er det samme som et commit
D. En pull request sender automatisk kode til produktion



### Spørgsmål 2

Hvad sker der når man accepterer en pull request

A. Ændringerne kopieres automatisk til alle udvikleres lokale branches.
B. Ændringerne bliver flettet ind i den branch, pull requesten peger mod.
C. Den oprindelige branch slettes altid automatisk.
D. Hele projektets Git-historik bliver erstattet af historikken fra pull requesten.



### Spørgsmål 3

En udvikler skal implementere login-funktionalitet. Hvilken passer bedst?

A. Arbejd direkte på `main`.
 B. Lav en `feature/login` fra `develop`, merge tilbage til `develop` via Pull Request, og senere bliver `develop` releaset til `main`.
 C. Lav en `hotfix/login` fra `main`.
 D. Lav alle commits direkte på `develop`.

-->



## Indhold 



# Opgave 1: Hobbyer gennem pull requests



## Formål

I skal gennemføre hele pull-request-flowet i en lille opgave.

Arbejd i grupper på mellem 2 og 3.



## Repository

Én person opretter et repository og inviterer de øvrige som collaborators.

Repositoryet starter med:

```
hobbies.md
```

`hobbies.md` indeholder links til de andres hobby filer

```
# Gruppens hobbyer

- [Martin](MartinHobbies.md)
- [Benjamin](benjaminHobbies.md)
```



## Individuel ændring

Hvert gruppemedlem skal:

1. Klone repositoryet.
2. Oprette sin egen branch.
3. Oprette en personlig Markdown-fil med ders hobbyer. Fx `benjaminHobbies.md`
4. Tilføje et link til filen i `hobbies.md`.
5. Committe ændringen.
6. Pushe branchen.
7. Oprette en pull request.



Filen skal mindst indeholde:

```
# Martins hobbyer

## Badminton

Jeg spiller badminton, fordi ...

## Madlavning

Jeg kan godt lide at ...

## Udstyr

- Ketcher
- Træningssko
- Drikkedunk
```



Krav:

- én `h1`;
- mindst to `h2`;
- mindst én liste;

Tilføj linket i `hobbies.md`:

```
- [Martin](MartinHobbies.md)
```



## Pull request-beskrivelse

```
## Hvad har jeg ændret?

- Tilføjet min personlige hobbyfil
- Tilføjet et link i hobbies.md

## Sådan kontrolleres ændringen

- Åbn hobbies.md
- Klik på linket med mit navn
- Kontrollér at Markdown-filen vises korrekt
```



## Review

Et andet gruppemedlem kontrollerer:

- virker linket?
- er filnavnet forståeligt?
- er Markdown-strukturen korrekt?
- er alle krav opfyldt?
- indeholder pull requesten kun relevante ændringer?

Revieweren skriver:

- mindst én konkret positiv kommentar;
- mindst ét spørgsmål eller forbedringsforslag.



## Efter review

Forfatteren skal:

1. læse feedbacken;
2. foretage mindst én relevant forbedring;
3. committe og pushe på samme branch;
4. kontrollere at pull requesten opdateres;
5. bede om et nyt review.

Pull requesten må merges, når den er godkendt





# Opgave 2



## Formål

I skal anvende et forenklet Git Flow i en realistisk programmeringsopgave. Arbejd i grupper af 2 eller 3

I arbejder med tre typer branches:

```
main
develop
feature/...
```

- `main` indeholder den stabile version.
- `develop` indeholder den nyeste samlede udviklingsversion.
- `feature/...` bruges til én afgrænset feature.

I skal ikke udvikle direkte på `main` eller `develop`! 



## Forbered repositoryet

Opret `main` og `develop` branches inden i går igang med at kode! Sørg for alle gruppemedlemmer kan har de branches lokalt



## Vælg en lille feature

Brug Turistguide-projektet eller et andet fælles Spring Boot-projekt.

Hvert gruppe medlem vælger én lille feature, eksempelvis: Featuren skal være lille nok til at kunne implementeres, testes og reviewes i undervisningstiden.



## Opret en feature branch

Feature-branchen skal oprettes fra `develop`. Branchens navn skal beskrive ændringen.

Gode eksempler:

```
feature/show-category
feature/delete-attraction
feature/validate-attraction-name
```



## Implementér featuren

Udvikleren skal:

1. Implementere en lille del ad gangen.
2. Køre projektet og relevante tests.
3. Lave forståelige commits.
4. Pushe feature-branchen.



## Opret pull request til develop

Pull requesten skal gå:

```
feature/show-category → develop
```

Den skal **ikke** gå direkte til `main`!

Pull requesten skal indeholde:

```
## Problem

Hvilket behov eller hvilken fejl løser ændringen?

## Løsning

Hvordan er problemet løst?

## Sådan testes ændringen

1. ...
2. ...
3. ...

## Afgrænsning

Hvad er ikke en del af denne pull request?
```



## Code review

Revieweren undersøger:

- løser ændringen den aftalte feature?
- er pull requesten tilpas lille?
- kommer branchen fra `develop`?
- peger pull requesten tilbage på `develop`?
- er navne og struktur forståelige?
- er der uvedkommende ændringer?
- mangler der tests?
- er relevante fejltilfælde håndteret?
- kan eksisterende funktionalitet være blevet ødelagt?

Revieweren skal skrive:

- mindst én konkret positiv kommentar;
- mindst ét spørgsmål eller forbedringsforslag;
- en tydelig godkendelse eller anmodning om ændringer.



## Efter review

Udvikleren skal:

1. Svare på kommentarerne.
2. Foretage relevante ændringer.
3. Pushe nye commits til den samme feature-branch.
4. Kontrollere, at pull requesten automatisk opdateres.
5. Anmode om et nyt review.

Pull requesten merges til `develop`, når:

- Definition of Done er opfyldt;
- ændringen er testet;
- revieweren har godkendt;
- der ikke er uløste konflikter.

Efter merge kan feature-branchen slettes.



## Simuler en release

Når alle gruppens features er merged til `develop`, skal gruppen simulere en release.

Opret en pull request:

```
develop → main
```

Denne pull request skal samle den færdige udviklingsversion og gøre den til den nye stabile version.

Inden merge skal gruppen kontrollere:

- at alle tests passerer;
- at projektet kan starte;
- at de nye features fungerer sammen;
- at der ikke er uløste konflikter;
- at pull requesten kun indeholder de forventede features.

Når release-pull-requesten er godkendt, merges den til `main`.



## Git Flow i opgaven

Det samlede flow ser sådan ud:

```
main
  │
  └── develop
        │
        ├── feature/show-category
        │         │
        │         └── pull request til develop
        │
        └── feature/delete-attraction
                  │
                  └── pull request til develop

develop
  │
  └── pull request til main
            │
            └── release
```



## Refleksion

Diskutér i gruppen:

1. Hvorfor oprettes feature branches fra `develop` og ikke fra `main`?
2. Hvorfor merges features tilbage til `develop`?
3. Hvad er forskellen på en feature-pull-request og en release-pull-request?
4. Hvilken risiko opstår, hvis en feature merges direkte til `main`?
5. Hvornår er Git Flow nyttigt?
6. Hvornår kan Git Flow være unødigt komplekst?
7. Hvorfor bør feature branches leve kort tid?


