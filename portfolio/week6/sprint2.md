# Sprint 2
Voordat de tweede sprint begon ben ik 's ochtends extra vroeg opgestaan (3 uur voor de start van de sprint) omdat ik
in de avond na de eerste sprint een tutorial had gevonden die er veel belovend uit zag. Deze heb ik geprobeerd te
en met een half succes want ik kreeg slechts één foutmelding. Na relatief lang gezeten te hebben op deze ene bug heb
ik ontdekt dat er ook een ViewPager2 bestaat die onlangs uitgebract is door Google ergens in eind 2019. Hier ben ik
pas op dit moment achter gekomen vermoedelijk omdat de hoeveelheid aan informatie over de ViewPager2 nog relatief
beperkt was waardoor ik telkens resultaten kreeg van de ViewPager wanneer ik naar een oplossing zocht. Dus hier heb
ik geen ontwerpkeuze voor gemaakt en had dus besloten om gelijk de ViewPager2 te proberen en te implementeren.
ViewPager2 lijkt redelijk veel op ViewPager en dus ik was al vrij bent met de code die nodig was om het werkend te
krijgen. En na enige tijd had ik hem werkend en werden de afbeeldingen mooi gecentreerd.

---

### Indicatie dat je horizontaal kan schuiven.
Om de gebruiker informatie te geven over hoe hij met de app moet omgaan zonder expliciet ergens te beschrijven hoe
dit moet, wilde ik twee extra kleine kaarten aan de zijkant van de grote kaart laten zien. Dit was tevens het
originele ontwerp volgens de draaddiagram. Maar na enige tijd en gestoei met meerdere manieren om dit te proberen
te implementeren, heb ik mij erbij neergelegd dat het te lastig is om dit te maken. Daarom heb ik naar alternatieve
oplossingen gezocht.

##### Ontwerpkeuzes
- **Icoon:** Een icoon dat aangeeft dat je de kaarten van links naar rechts (en vice versa) kan bewegen.
- **Animaties:** Bij het openen van het scherm een animatie laten zien waarbij de kaarten van helemaal achteraan naar
vooraan wordt gescrolld om te laten zien dat de kaarten horizontaal te verschuiven zijn. Dit wordt dan laten zien
een seconden.
- **Indicatoren:** kleine stipjes onder of boven de kaarten die aangeven op welke positie kaart je momenteel zit.

##### Gekozen ontwerp
Ondanks dat een icoon de makkelijkste oplossing was om te implementeren, was het tevens de minst mooiste oplossing.
Hierdoor bleven de animaties en indicatoren over. De zou het erg leuk hebben gevonden als de animaties zouden worden
gemaakt om te laten zien dat de kaarten horizontaal te verschuiven waren. Maar vanuit het oogpunt van de gebruiker
zou dit misschien als irritant ervaren worden omdat de gebruiker dan telkens een seconde lange animatie te zien krijgt
wanneer hij het scherm opent. Vandaar dat ik heb gekozen voor de indicatoren. Als bijkomende voordeel van de
indicatoren kreeg de gebruiker ook te zien op welke kaart hij zat omdat de indicatoren ook geanimeerd moesten worden.
De horizontale reeks van balletjes worden vaak geassocieerd met een horizontale slider dus vandaar dat dit ook één van
de goede ontwerpkeuzes was. En op het moment dat de gebruiker horizontaal begint te slider, ziet hij de kaarten bewegen
en bij het loslaten zal de indicatoren reeks geüpdate worden met de huidige positie van de ViewPager2.

---

### Indicatoren
Een kleine ontwerpkeuze maar toch wil ik deze even delen. Er zijn namelijk meerdere manieren om de reeks balletjes die
als indicatoren functioneren op te bouwen.

##### Ontwerpkeuzes
- Waaruit bestaan de indicatoren?
  - Het kunnen kleine icoontjes zijn die zijn opgeslagen als afbeelding in de Resources.
  - Het kunnen Android vectoren zijn die standaard worden geleverd binnen het programma Android Studio.
  - Het kunnen zelfgemaakte vectoren zijn met behulp van Google's New Material Design library.
- Hoe wordt de reeks weergeven (opgebouwd)?
  - De indicatoren worden in een ScrollView geplaats in de XML.
  - De indicatoren worden in een (tweede) RecyclerView geplaatst en telkens opnieuw gevuld met behulp van Java functies.
  - De indicatoren worden in een LineairLayout geplaatst en telkens opnieuw gevuld met behulp van Java functies.

##### Gekozen ontwerp
Hier moest ik twee keuzes maken. In eerste instatie wilde ik gebruik maken van kleine icoontjes opgeslagen als
afbeeldingen maar ik heb besloten om dit toch niet te doen omdat het programma dan telkens de afbeeldingen moet
laden, ondanks dat het simpele afbeeldingen kunnen zijn. En in de Android vectoren bibliotheek heb ik geen bruikbare
indicatoren gevonden dus heb ik gekozen om zelf icoontjes te maken met behulp van Google's New Material Design
library. En omdat ik het simpel wilde houden, heb ik een LinearLayout gebruikt en niet een RecyclerView. En met behulp
van de Java methoden hieronder heb ik de indicatoren in de LinearLayout gezet.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Call function on page change.
    onboardingViewPager.registerOnPageChangeCallback(new ViewPager2.OnPageChangeCallback() {
        @Override
        public void onPageSelected(int position) {
            super.onPageSelected(position);
            setCurrentOnboardingIndicator(position);
            changeDeck(position, false);
            setCardInformation(position);
    
            dirtyPos = position;
        }
    });
    ...
}

public void setupOnboardingIndicators() {
    ImageView[] indicators = new ImageView[onboardingAdapterViewPager.getItemCount()];

    // Create LinearLayout parameters.
    LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(
            ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT
    );

    layoutParams.setMargins(8, 0, 8, 0);

    // Add all indicators to the LinearLayout.
    for (int i = 0; i < indicators.length; i++) {
        // Create new ImageView without XML elements.
        indicators[i] = new ImageView(getApplicationContext());

        // Set the image.
        indicators[i].setImageDrawable(ContextCompat.getDrawable(
                getApplicationContext(),
                R.drawable.onboarding_indicator_inactive
        ));

        // Apply LayoutParameters and add to the LinearLayout
        indicators[i].setLayoutParams(layoutParams);
        layoutOnboardingIndicators.addView(indicators[i]);
    }
}

private void setCurrentOnboardingIndicator(int index) {
    int childCount = layoutOnboardingIndicators.getChildCount();

    // Update all images regardless of whether it's the same.
    for (int i = 0; i < childCount; i++) {
        ImageView imageView = (ImageView) layoutOnboardingIndicators.getChildAt(i);

        if (i == index) {
            imageView.setImageDrawable(
                    ContextCompat.getDrawable(getApplicationContext(), R.drawable.onboarding_indicator_active)
            );
        } else {
            imageView.setImageDrawable(
                    ContextCompat.getDrawable(getApplicationContext(), R.drawable.onboarding_indicator_inactive)
            );
        }
    }
}
```

Zoals in de code blok te zien is, heb ik de RecyclerView gevuld met een array van ImageViews. Deze array wordt in
de layoutOnboardingIndicators (: LinearLayout) gezet met enkele layout eigenschappen. Vervolgens wordt de
setCurrentOnboardingIndicator(int index) methode aangeroepen op het moment dat de ViewPager2 merkt dat de gebruiker
van pagina is veranderd.

---

### Reflectie
Tegen het einde van de dag heb de ViewPager2 en de indicatoren werkend gekregen. Het was wederom weer een
moeizame dag maar wel een dag met redelijk wat gemaakte ontwerpkeuzes. Uiteraard kan ik niet al mijn ontwerpkeuzes
beschrijven en onderbouwen want dan zal dit document tientallen kleine ontwerpkeuzes bevatten waardoor je de minder
belangrijke ontwerpkeuzes niet meer van de belangrijke ontwerpkeuzes kan onderscheiden.

En ook vind ik het goed dat ik eerst een draaddiagram heb gemaakt zodat ik altijd kan refereren naar het originele
ontwerp zodat mijn gemaakte onderdelen goed aansluiten op andermans gemaakte onderdelen.

#### Navigatie
[Index](../readme.md) / [Sprint 1](sprint1.md) / **Sprint 2** / [Sprint 3](../week7/sprint3.md)
/ [Sprint 4](../week7/sprint4.md) / [Stelling reflectie](../overig/stelling-reflectie.md) / [JSON applicaties](../overig/json-applicaties.md)