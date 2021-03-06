# Electron

[Electron](https://electronjs.org) jest otwartoźródłową biblioteką, stworzoną przez GitHub, aby umożliwić tworzenie multiplatformowych aplikacji z użyciem języków HTML, CSS oraz JavaScript. Electron osiąga to łącząc [Chromium](https://www.chromium.org/Home) oraz [Node.js](https://nodejs.org) w jednolite środowisko uruchomieniowe, którego aplikacje mogą być wydawane na MacOS, Windows oraz Linux.

Electron powstał w 2013 roku jako framework, z użyciem którego [Atom](https://atom.io), edytor tekstu GitHub'a, został zbudowany. Źródła zarówno edytora jak i frameworku zostały upublicznione wiosną 2014 roku.

Od tamtego momentu, Electron stał się popularnym narzędziem, używanym przez developerów open source, startupy oraz korporacje. [Zobacz, kto używa Electrona](https://electronjs.org/apps).

Kontynuuj czytanie, aby dowiedzieć się więcej o darczyńcach oraz wydaniach Electrona, albo zacznij tworzyć aplikacje z [Szybkim Startem](quick-start.md) Electrona.

## Zespół oraz współtwórcy

Electron jest rozwijany przez zespół Github i grupę [aktywnych kontrybutorów](https://github.com/electron/electron/graphs/contributors) ze społeczności. Niektórzy z twórców są to niezależne osoby pozostali pracują w większych firmach rozwijających produkt Electron. Jesteśmy zadowoleni, że możemy dodawać współtwórców jako osóby rozwijające projekt. Dowiedz się więcej o [współtworzeniu Electron](https://github.com/electron/electron/blob/master/CONTRIBUTING.md).

## Wydania

[Nowe wydania Electron ](https://github.com/electron/electron/releases) są częste. Wydajemy je, kiedy są znaczące poprawki błędów, nowe API lub kiedy ukazują się zaktualizowane wersje Chromium lub Node.js.

### Aktualizowanie Bibliotek

Electron Chromium jest zazwyczaj aktualizowana w ciągu jednego lub dwóch tygodni po wydaniu nowej stabilnej wersji, w zależności od aktualizacji.

Po wydaniu nowej wersji Node.js, Electron zazwyczaj czeka około miesiąca za aktualizacją w celu wydania bardziej stabilnej wersji.

Electron, Node.js oraz Chromium używają pojedyńczej instancji V8 - zazwyczaj wersji, którą używa Chromium. Większość czasu *działa*, ale czasami oznacza łatanie Node.js.

### Numer Wersji

Od wersji Electron 2.0 [śledzi`semver`](http://semver.org). Dla większości aplikacji użycie dowolnej najnowszej wersji npm, uruchomienie `$ npm install electron` wystraczy, aby działał poprawnie.

Proces aktualizacji jest dokładnie opisany w naszej [Specyfikacji wersji](electron-versioning.md).

### LTS

Nie istnieje aktualnie długoterminowe wsparcie do starszych wersji Electrona. Jeśli aktualna wersja Electron u Ciebie działa, możesz pozostać przy niej tak długo, jak będziesz chciał. Jeśli chciałbyś skorzystać z nowych funkcjonalności, w miarę ich pojawiania się, powinieneś zaktualizować swoją wersję programu do najnowszej.

Główne zmiany wchodzą z wersją `v1.0.0`. Jeśli nie używasz tej wersji programu, powinieneś [poczytać o zmianach w `v1.0.0`](https://electronjs.org/blog/electron-1-0).

## Główne założenia

Aby zachować niewielki (rozmiar pliku) oraz zrównoważony ( ilość zależności oraz APIs) projekt, ogranicza się on do głównych funkcjonalności.

Na przykład, Electron wykorzystuje tylko biblioteki do renderowania z Chromium zamiast całego Chromium. To powoduje, że łatwiej jest zaktualizować Chromium, ale oznacza również, że niektóre funkcjonalności z Google Chrome nie istnieją w Electron.

Nowe funkcjonalności dodawane w Electron powinny przede wszystkim być natywnymi API. Jeśli funkcja należy do modułu Node.js, prawdopodobnie powinna tam być. Spójrz na [narzędzia zbudowane przez społeczeństwo](https://electronjs.org/community).

## Historia

Poniżej przedstawiona jest historia Electron.

| :calendar:        | :tada:                                                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Kwiecień 2013** | [Atom Shell](https://github.com/electron/electron/commit/6ef8875b1e93787fa9759f602e7880f28e8e6b45).                 |
| **Maj 2014**      | [Atom Shell jako projekt open source](http://blog.atom.io/2014/05/06/atom-is-now-open-source.html).                 |
| **Kwiecień 2015** | [Zmiana nazwy z Atom Shell na Electron](https://github.com/electron/electron/pull/1389).                            |
| **Maj 2016**      | [Wypuszczenie wersji `v1.00`](https://electronjs.org/blog/electron-1-0).                                            |
| **Maj 2016**      | [Kompatybilność Electron app z Mac App Store](https://electronjs.org/docs/tutorial/mac-app-store-submission-guide). |
| **Sierpień 2016** | [Wsparcie Electron app przez Windows Store](https://electronjs.org/docs/tutorial/windows-store-guide).              |