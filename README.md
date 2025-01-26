### Výsledek EDA (ADF statistika a p-hodnota)

**Augmented Dickey-Fuller (ADF) test** je statistický test používaný k ověření, zda je časová řada stacionární.

- **ADF statistika**: −1,6112401818572721.
  - Obecně platí, že čím je ADF statistika více záporná, tím silnější důkazy máme proti nulové hypotéze nestacionarity.

- **P-hodnota**: 0,4773246176606757.
  - Tato hodnota je vyšší než typické prahové hodnoty (např. 0,05 nebo 0,01), což znamená, že nemůžeme zamítnout nulovou hypotézu o nestacionaritě.

Z toho vyplývá, že časová řada pravděpodobně obsahuje jednotkový kořen a je nestacionární. Nestacionarita se obvykle projevuje přítomností trendů, sezónních vzorců nebo jiných struktur, které se nemění v čase.

**Další úvahy**:

1. **Transformace dat**:
    - V případě nestacionarity je často nutné provést diferencování pro dosažení stacionarity.
    - Silné sezónní složky mohou maskovat stacionaritu, což může částečně vysvětlit vysokou p-hodnotu.

2. **Velikost vzorku**:
    - ADF test může být neprůkazný, pokud je velikost vzorku malá nebo pokud jsou přítomny složité sezónní složky.

3. **Vizualizace dat**:
    - Základní vizualizace časové řady odhalila zřetelné trendy a opakující se sezónní vrcholy, což potvrzuje nestacionaritu.
    - Tyto grafy mohou také ukázat přítomnost náhlých změn nebo odlehlých hodnot.

4. **Modelování nestacionárních dat**:
    - Např. modely SARIMAX mohou být vhodné, pokud správně zohlední nestacionaritu a sezónní vlivy.

---

### ACF vypadá sinusoidálně, PACF je také sinusoidální, ale tlumený

- **ACF (autokorelační funkce)**:
  - Měří, jak současné hodnoty časové řady souvisejí s jejími minulými hodnotami při různých zpožděních.
  - Sinusoidální vzor v ACF často naznačuje přítomnost silné sezónnosti nebo cyklického chování v čase.
  - Pokud ACF nekonverguje rychle k nule, naznačuje to přetrvávající korelace napříč více zpožděními běžné u sezónních dat.

- **PACF (parciální autokorelační funkce)**:
  - Ukazuje korelaci s daným zpožděním po zohlednění vlivu mezilehlých zpoždění.
  - Sinusoidální vzor v PACF naznačuje komplexní interakce, kde každé zpoždění má částečnou korelaci, která se časem snižuje.

- **Význam sinusoidálních vzorů**:
  - Sinusoidální ACF může znamenat dobře definovaný sezónní cyklus, což znamená, že časová řada silně koreluje se sebou samou v sezónních intervalech.
  - Tlumené sinusoidální PACF naznačuje, že vliv zpožděných hodnot existuje, ale postupně klesá.

---

**Praktické důsledky**:

1. **Modelování sezónních složek**:
    - Cyklické vzory v ACF/PACF naznačují potřebu více než jedné sezónní diferenciace.

2. **Sezónní vlivy**:
    - V reálných datech z oblasti cestovního ruchu jsou cyklické vzory spojeny s obdobími dovolených, změnami počasí a ekonomickými cykly.

3. **Vliv na rezidua**:
    - Špatně zohledněná sezónnost často zanechává vysokou autokorelaci v reziduích, což naznačuje nedostatečný model.
    - Sinusoidální vzory v ACF/PACF u měsíčních dat vedou k vrcholům přibližně na 12 zpožděních, což odpovídá ročnímu cyklu.

4. **Modelová doporučení**:
    - Tyto vzory potvrzují, že konečný model by měl zahrnovat odpovídající sezónní diferenciaci nebo sezónní AR/MA parametry.

---

### Nejlepší nalezený model: SARIMAX(2, 1, 2)x(0, 2, 2, 12)

SARIMAX znamená **Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors**, přestože v tomto případě nejsou zahrnuty exogenní proměnné.

**Parametry modelu:**

- **(p, d, q)**: nestacionární část
  - **p = 2**: model zahrnuje dva autoregresní členy.
  - **d = 1**: jedna úroveň diferenciace.
  - **q = 2**: dva členy klouzavého průměru.

- **(P, D, Q, m)**: sezónní část
  - **P = 0**: žádné dodatečné sezónní autoregresní členy.
  - **D = 2**: dvakrát sezónně diferencovaná řada.
  - **Q = 2**: dva sezónní členy klouzavého průměru.
  - **m = 12**: měsíční cyklus.

---

**Význam parametrů**:

1. **Nestacionarita**:
    - Dvě sezónní diferenciace (D=2) ukazují, že data vyžadovala silný přístup ke snížení sezónní nestacionarity, což je konzistentní s výsledky EDA.

2. **Autoregresní a MA členy**:
    - Přítomnost autoregresních členů (p=2) naznačuje, že minulé hodnoty jsou významně prediktivní.
    - Dva sezónní členy klouzavého průměru (Q=2) naznačují přetrvávající sezónní šoky.

3. **Optimalizace modelu**:
    - Tento model byl nalezen na základě minimalizace AIC, což je metrika vyvažující kvalitu přizpůsobení modelu a jeho složitost.

---

### Distribuce reziduí a fluktuace

- **Distribuce reziduí**:
  - Rezidua vykazují přibližně gaussovské rozdělení (ve tvaru zvonu) centrované kolem nuly.
  - Největší fluktuace byly zaznamenány kolem roku 2012 (nevim) a 2021 (covid).

- **Heteroskedasticita**:
  - Stabilita rozptylu reziduí v čase, mimo speciální šoky, naznačuje homoskedasticitu (stejnorodost rozptylu), což je důležité pro modely ARIMA.

- **Normalita reziduí**:
  - Testy jako Shapiro-Wilk mohou potvrdit, že rezidua jsou přibližně normální, přestože Jarque-Bera naznačuje odchylky.

---

### Interpretace souhrnných statistik (Ljung-Box, Jarque-Bera, heteroskedasticita, skew, kurtosis)

1. **Ljung-Box**:
    - Q-statistika = 0,01, p-hodnota = 0,94.
    - Nemůžeme zamítnout nulovou hypotézu, že rezidua nevykazují autokorelaci na zpoždění 1. To znamená, že rezidua se chovají jako bílý šum.

2. **Jarque-Bera**:
    - Statistika = 251,64, p-hodnota = 0,00.
    - Zamítáme nulovou hypotézu normality reziduí, což naznačuje odchylky (pravděpodobně v oblastech s extrémními hodnotami).

3. **Heteroskedasticita**:
    - H = 5,37, p-hodnota = 0,00.
    - Variance reziduí není konstantní (heteroskedasticita).
    - pro heteroskedacitiu je vhodné využít např. model GARCH.

4. **Skew a kurtosis**:
    - **Skew = 0,49**: rezidua jsou mírně vychýlená doprava.
    - **Kurtosis = 10,71**: vysoká hodnota naznačuje extrémní odlehlé hodnoty.

---

Celkově výsledky ukazují, že:

- Rezidua mají malou autokorelaci (to je dobře).
- Objevují se problémy s nestandardní distribucí (důležité pro odhady intervalů).
- Model je dobře nastaven pro bodové predikce, ale může vyžadovat dodatečné úpravy pro lepší intervalové odhady.

