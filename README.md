Toto repo obsahuje slovenský slovník bez diakritiky pre prehliadače postavené na Chromiume (Google Chrome, Brave, Vivaldi, Edge...). Nižšie sa nachádza návod, ako tento slovník vložiť do prehliadača. Pokiaľ chceš niečo doplniť do tohto návodu, alebo si našiel chybu, vytvor pull request, alebo mi napíš na jan@jankelemen.dev

Pokiaľ používaš Firefox, môžeš využiť [túto](https://addons.mozilla.org/en-US/firefox/addon/sk-sk-ascii_spellchecking/) extenziu od Zdenka Podobného

Pred vložením slovníka bez diakritiky             |  Po vložení slovníka
:-------------------------:|:-------------------------:
![Before](https://github.com/jankelemen/sk-ascii-dictionary-for-chromium/blob/master/images/before.png)  |  ![After](https://github.com/jankelemen/sk-ascii-dictionary-for-chromium/blob/master/images/after.png)


# Návod na vloženie slovníka:

1. 
- Pokiaľ používaš **Chrome**: Choď do nastavení. Naľavo klikni na `rozšírené` a z menu, ktoré sa vyroluje vyber `jazyky`. Následne klikni na `jazyk` a potom `pridať jazyky`. V okne, ktoré sa otvorí si vyber hocijaký jazyk, ktorý nepotrebuješ na kontrolu gramatiky (ďalej v návode tento jazyk budem nazývať "nepotrebný jazyk"). Slovník tohto jazyka potom nahradíš tým našim. Ja v tomto návode ako "nepotrebný jazyk" použijem Češtinu. Nakoniec nižšie v sekcií `používať kontrolu pravopisu pre jazyky` zaškrtni novo vybraný jazyk
- Pri **ostatných chromium based** prehliadačoch postupuj tak isto ako pri Chrome, jedine niektoré nastavenia sa budú pravdepodobne kúsok inak volať
- Tie jazykové nastavenia by mali vyzerať nejak takto:
<img src="https://github.com/jankelemen/sk-ascii-dictionary-for-chromium/blob/master/images/settings.png" alt="Settings" height="20%">

2. Počkaj zopár sekúnd pokým prehliadač na pozadí stiahne novo vybraný slovník a potom prehliadač zatvor

3. 
- Ak si na **Windowse**
  - a používaš Chrome, choď do priečinka `C:\Users\<meno požívateľa>\AppData\Local\Google\Chrome\User Data`
  - a používaš Brave, choď do priečinka `C:\Users\<meno požívateľa>\AppData\Local\BraveSoftware\Brave-Browser\User Data`
  - a používaš Vivladi, choď do priečinka `C:\Users\<meno požívateľa>\AppData\Local\Vivaldi\Application\Dictionaries`
  - a používaš Operu, choď do priečinka `C:\Users\<meno požívateľa>\AppData\Roaming\Opera Software\Opera Stable\dictionaries`
  - a používaš Edge, choď do priečinka `C:\Users\<meno požívateľa>\AppData\Local\Microsoft\Edge\User Data`
> **Poznámka:** Na Windowse treba mať zapnuté zobrazovanie skrytých položiek, inak priečinok `AppData` neuvidíš. (v  prieskumníku klikni hore na "zobraziť" a potom zaklikneš checkbox "skryté položky" )

- Ak si na **Linuxe**
	- a používaš Chrome, choď do priečinka `~/.config/google-chrome/Dictionaries`
	- a používaš Chromium, choď do priečinka `~/.config/chromium/Dictionaries`
	- a používaš Brave, choď do priečinka `~/.config/BraveSoftware/Brave-Browser/Dictionaries`
	- a používaš Vivaldi, choď do priečinka `~/.config/vivaldi/Dictionaries`

- Ak si na **Macku**
  - a používaš Chrome, choď do priečinka `/Users/<meno požívateľa>/Library/Application Support/Google/Chrome`
> **Poznámka:** Cesta pre Mac pravdepodobne nie je úplná, keďže nemám Mac a nemám to kde otestovať. Pokiaľ ale vieš, aká má byť správna cesta, neváhaj urobiť pull request, alebo ma kontaktuj na predtým uvedenom maile

4. V otvorenom priečinku nájdi súbor, ktorý je zodpovedný za kontrolovanie gramatiky pre "nepotrebný jazyk". Tento súbor musí mať príponu `.bdic`. V mojom prípade to je `cs-CZ-3-0.bdic`, keďže som si vybral Češtinu. Tento súbor teraz nahraď súborom `output.bdic`, ktorý stiahneš [tu](https://raw.githubusercontent.com/jankelemen/sk-ascii-dictionary-for-chromium/master/output.bdic)
> **Poznámka:** Nahradiť znamená, že `output.bdic` bude v priečinku, kde bol slovník "nepotrebného jazyka" a bude mať rovnaké meno

5. Hotovo. Môžeš ísť vyskúšať, či nový slovník funguje napríklad [sem](https://editpad.org)


# Návod ako si sám vytvoriť slovník bez diakritiky

Slovník, ktorý sa nachádza v tomto repe je upravený slovenský slovník verzie 3.0 z Chromiumu. Celkovo som našiel 3 spôsoby ako tento slovník upraviť. Info ohľadom súborov `.dic` `.aff` `.dic_delta` a kompilácie do `.bidc` si môžeš prečítať [tu](https://github.com/jankelemen/convert-dict-tool-from-chromium). Slovník, ktorý je v tomto repe som vytvoril 3. spôsobom

## 1. spôsob:
Súbory `.aff` a `.dic` nechaj na pokoji a do `.dic_delta` "napchaj" celý slovník bez diakritiky, napríklad [tento](https://github.com/sk-spell/hunspell-sk_ascii). Takže výsledný slovník bude obsahovať slová s aj bez diakritiky. Nevýhoda je ale tá, že tento slovník je veľký, keďže `.dic_delta` nemôže obsahovať hunspell pravidlá a kontrola pravopisu je potom pomalá (keď klikneš pravým tlačidlom na červeným podčiarknuté slovo, tak niekdy trvá aj niekoľko sekúnd, pokým prehliadač ponúkne sugescie)

## 2. spôsob:
Ide tiež o kombinovanie slovníka s a bez diakritiky. Súbory `.aff`, `.dic` a `.dic_delta` uprav scriptom, ktorý napríklad takýto slovník:
```
Abakus
abdikovať/NW
```
Upraví na takýto:
```
Abakus
abdikovať/NW
abdikovat/NW
```
Dá sa to docieliť napríklad týmto Python scriptom: 
```python
import unidecode

def makeUnicode(inputFile, outputFile, encodingCode):
    def isAscii(s):
        return all(ord(c) < 128 for c in s)

    original = open(inputFile, 'r', encoding=encodingCode)
    output = open(outputFile, 'w', encoding=encodingCode)

    for i in original:
        output.write(i)
        if not isAscii(i):
            output.write(unidecode.unidecode(i))

    original.close()
    output.close()

makeUnicode('sk_SK.dic', 'output.dic', 'ISO8859-2')
makeUnicode('sk_SK.dic_delta', 'output.dic_delta', 'UTF-8')
```
Tu je ale zas tá nevýhoda, že musíš manuálne doplniť do `.aff` súboru pravidlá pre slová bez diakritiky. Alebo možno sa to dá urobiť nejakým šikovným scriptom, ale mne sa s tým nechcelo piplať

## 3. spôsob:
Vytvor nový slovník, ktorý bude identický s tým originálnym až na to, že nebude obsahovať žiadnu diakritiku.
Takže keď slovník vyzerá takto:
```
Abakus
abdikovať/NW
```
Nový slovník bude vyzerať takto:
```
Abakus
abdikovat/NW
```
Toto je Python script, ktorý som použil na vytvorenie takéhoto slovníka:
```python
import unidecode

def makeUnicode(inputFile, outputFile, encodingCode):
    original = open(inputFile, 'r', encoding=encodingCode)
    output = open(outputFile, 'w', encoding='UTF-8')

    for i in original:
        output.write(unidecode.unidecode(i))

    original.close()
    output.close()

makeUnicode('sk_SK.dic', 'output.dic', 'ISO8859-2')
makeUnicode('sk_SK.aff', 'output.aff', 'ISO8859-2')
makeUnicode('sk_SK.dic_delta', 'output.dic_delta', 'UTF-8')
```
Následne som ešte manuálne prepísal začiatočné riadky v súboroch `.dic` a `.aff`.
V `.aff` súbore som toto:
```
# verzia 0.5.5
SET ISO8859-2
#Male pismena sa zotriedene podla pravdepodobnosti vyskytu v slovencine
TRY eonairvtlmpukszdjhácšíbyúčýžťéľgfňóäŕôďxĺwqAÁEÉĚIÍOÓUÚŮYÝBCČDĎLMNŇFJKPRŘSŠTŤVGHQWXZŽ
```
Zmenil na toto:
```
# verzia 0.5.5
SET UTF-8 
#Male pismena sa zotriedene podla pravdepodobnosti vyskytu v slovencine
TRY eonairvtlmpukszdjhcbygfxwqAEIOUYBCDLMNFJKPRSTVGHQWXZ
```
V `.dic` súbore som toto:
```
175211
A
a
Á
á
ä
```
Zmenil na toto:
```
175208
A
a
```
Tento slovník má zas tú nevýhodu, že musíš "obetovať" nejakú reč, ktorá bude nahradená týmto slovníkom
