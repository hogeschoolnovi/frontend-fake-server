# Nep database server

## Beschrijving
Om te oefenen met authenticatie kun je deze nepserver gebruiken. De nepserver draait apart van jouw frontend project, zodat we de "database" via een API kunnen benaderen. Zo kun je door gebruik te maken van specifieke eindpoints, data opvragen en toevoegen!

## Eindpoints

Wanneer deze server draait is hij benaderbaar op [http://localhost:3000](http://localhost:3000). Dis is de **basis url**, welke aan te vullen is middels de onderstaande endpoints.

### Registreren
`POST /register`

De keys `email` en `password` zijn vereist in de request body. Andere parameters mogen ook meeverstuurd worden, maar worden verder niet gevalideerd:

```javascript
{
  email: "klaas@novi.nl",
  password: "123456",
  username: "klaasie",
}
```

Het wachtwoord wordt vervolgens versleuteld opgeslagen (met _bcryptjs_). De response bevat de JWT token die **na één uur expireert**.

### Inloggen
`POST /login`

De keys `email` en `password` zijn vereist om mee in te kunnen loggen (dus **niet** `username`!). In de request body vinden we dus alleen het emailadres en wachtwoord van de gebruiker, er mogen geen andere gegevens meegestuurd worden:

```javascript
{
  email: "klaas@novi.nl",
  password: "123456",
}
```

De response bevat de JWT token die **na één uur expireert**.

### Gebruikersdetails opvragen
`GET /600/users/:id`

Alleen een ingelogde gebruiker kan zijn **eigen** gebruikersinformatie opvragen. Geef in het endpoint de `id` van gebruiker mee waarvan je de gegevens wil opvragen (dus bijvoorbeeld `/600/users/1` of `/600/users/2`). In de request body moet de JWT token worden meegestuurd om te checken of de ingelogde gebruiker wel toegang heeft tot deze resource. Gebruikers mogen immers alleen hun **eigen** gegevens opvragen. 

```javascript
{
  "Content-Type": "application/json",
  Authorization: "Bearer xxx.xxx.xxx",
}
```

### Afgeschermde data opvragen
`GET /660/private-content`

Alleen ingelogde gebruikers kunnen deze algemene afgeschermde content opvragen. Hierbij is de JWT token vereist in de request body:

```javascript
{
  "Content-Type": "application/json",
  Authorization: "Bearer xxx.xxx.xxx",
}
```

## Gebruik
Voor je de server kunt gebruiken zul je de de dependencies moeten installeren met het commando:

`npm install`

Er is een speciaal script aangemaakt om deze server te runnen. Het letterlijke script - mocht je nieuwsgierig zijn - kun je terugvinden in de `package.json`. Om de server te starten hoef je slechts het volgende commando in jouw terminal in te voeren:

`npm run json:server`

Deze server draait op [http://localhost:3000](http://localhost:3000), wanneer je dit in de browser opent zul je alle beschikbare endpoints zien verschijnen. 

_Let op_: omdat deze server op `localhost:3000` draait is het belangrijk deze server te starten voor je een React-project start. React zal dan automatisch vragen om dat project op een andere port te draaien.