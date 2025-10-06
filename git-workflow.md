## Workflow
### Single player
Ann di Feynd är frontendutvecklare och ska bygga en app för lokalbokning. Hon ska arbeta tillsammans med kollegan Zeke. De använder git för att dela kod och arbeta tillsammans.

Först skapar Ann ett repository med en första, minimal version av de filer som ska finnas. Från början finns bara branchen *main*. Ann vill skapa en dev branch och flera feature branches. Ann ska jobba med funktionen att visa/söka lokaler och kollegan Zeke med funktionen att boka en lokal.

```bash
git init
# Lägg till första version av filer
# Kom ihåg att skapa en .gitignore fil
git add .gitignore index.html styles/main.css script/main.js
git commit -m "Initial commit"

# Dags att skapa branches
git branch dev
git branch show-rooms-feature
git branch book-room-feature
```

Innan Ann kan börja jobba i projektet måste hon ladda upp projektet till GitHub.

1. Skapa ett nytt repository på GitHub (helst med samma namn)
2. Kryssa *inte* i att skapa några filer atomatiskt. Ann har redan skapat allt som behövs på sin dator.
3. Nu har du ett nytt, tomt repository på GitHub. Detta ska kopplas ihop med ditt lokala repository. Skriv i terminalen:

```bash
# Detta står även på GitHub
git remote add origin https://github.com/your-user-name/your-repo-name.git
git push -u origin main
```

Nu är det dags att börja jobba med sökfunktionen. Ann byter branch från *main* till *show-rooms-feature* och ska börja skriva kod. Innan hon börjar kontrollerar hon att statusen är rätt.
 Det behövs mera JavaScript så Ann kommer att lägga till en ny fil.

```bash
# Byt till rätt branch
git checkout show-room-feature

git status
# Git säger:
#   On branch show-rooms-feature
#   nothing to commit, working tree clean
```

Nu jobbar Ann i projektet. Hon gör ändringar och lägger till nya filer.
```bash
git status
# Git säger:
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   index.html
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       script.js
```

Git har märkt att Ann har gjort ändringar i mappen. Ann lägger till filerna i *staging area* och gör en *commit*.
```bash
# Lägga till ändringarna
git add --all
# Eller: git add index.html script.js

git status
# Git säger:
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   script.js
#       modified:   index.html

# Ok, då kör vi
git commit -m "Uppdate index.html and added script.js"
git push
# Pusha commit så att Zeke kan hämta den från GitHub om han behöver
# Första gången skriver man: git push -u origin main
```
---
### Multi player
Samtidigt ska Zeke börja jobba med bokning av rum. Eftersom Ann har skapat repot, behöver inte Zeke göra det. Han ska bara klona repot.
```bash
# Ladda ner repot från GitHub
git clone https://github.com/anns-user-name/anns-repo-name.git

# Git skapar en mapp för repot, gå in i den och börja jobba
cd mappens-namn/

git status
# Vänta nu! Git rapporterar att branchen är "main"
# Zeke måste byta branch innan han börjar jobba
git checkout book-room-feature
```

Zeke ändrar i index.html och lägger till en ny CSS-fil. Sedan ska han committa det till GitHub, så att Ann kan se ändringarna.

```bash
# Committa och pusha
git add index.html book.css
# Eller: git add --all

git commit -m "Booking styles"
git push

git status
# Kontrollera så att allt gick vägen
```

Ann vill använda Zekes snygga CSS. Eftersom de jobbar i olika branches behöver hon *hämta Zekes ändringar* från branchen Zeke jobbade i. Det gör man med `git merge`. Men innan hon gör det måste hon hämta ner uppdateringar från GitHub, annars känner inte Git på hennes dator till Zekes ändringar.

```bash
git status
# Ann måste ha ett "cleant" working directory för att kunna hämta ändringar

git fetch   # fråga GitHub om det har skett några ändringar sen sist
git status

# Nu kör vi! Hämta ändringarna från Zekes branch
# och kombinera dem med ändringarna i Anns branch
git merge book-room-feature
```

---
## Konfliktlösning - merge

### Status just nu
*Tabell över ändringar som gjorts i två olika branches:*

|Anns branch    |Zekes branch   |Resultat  |
|---------------|---------------|----------|
|script.js      |               |script.js |
|               |book.css       |book.css  |
|index.html (1) |index.html (2) |**MERGE** |

*Git försöker lösa alla sammanslagningar automatiskt. Men det är inte alltid det lyckas.*

Filerna `script.js` och `book.css` löser git självt.

Men eftersom Ann och Zeke har gjort ändringar i samma fil, `index.html`, så behöver Git vår hjälp! Så här ser gamla index.html ut:

```html
 1	<!DOCTYPE html>
 2	<html lang="en" dir="ltr">
 3	<head>
 4		<meta charset="utf-8">
 5		<title>Boka lokaler </title>
 6	</head>
 7	<body>
 8		<section>
 9			<h1> Boka konferensrum </h1>
10			<ul>
11				<li> <button>Skatan</button> </li>
12				<li> <button>Bofinken</button> </li>
13				<li> <button>Strutsen</button> </li>
14			</ul>
15		</section>
16	</body>
17	</html>
```

Ann, som jobbar med sökfunktionen; har lagt till kod för att söka baserat på rummets namn, efter rad 9:

```html
10  <p> Sök efter rum: <input type="text" placeholder="Namn" /> </p>
```

Men Zeke, som jobbar med bokningen, har skrivit:

```html
10  <p> <input type="checkbox" /> Behöver projektor </p>
11	<ul>
12		<li> <button>Skatan (8 platser)</button> </li>
13		<li> <button>Bofinken (5 platser)</button> </li>
14		<li> <button>Strutsen (12 platser)</button> </li>
15	</ul>
```

Git tittar igenom ändringarna.

### Ändring 1

Zekes rad 11-15 är ändringar som bara finns hos Zeke. Git löser konflikten automatiskt genom att ta med raderna.

### Ändring 2

Ann och Zekes rad 10 är olika. Det blir därför en konflikt, som kan lösas på flera sätt:
1. behålla Anns version
1. behålla Zekes version
1. behålla båda raderna - det blir två rader i stället för en
1. kombinera raderna - det blir en rad med det bästa från båda
1. ibland behöver vi freestyla och skriva om koden för att det ska fungera rätt

I detta fallet väljer vi alternativ 4. Från rad 10:

```html
1	<!DOCTYPE html>
2	<html lang="en" dir="ltr">
3	<head>
4		<meta charset="utf-8">
5		<title>Boka lokaler </title>
6	</head>
7	<body>
8		<section>
9			<h1> Boka konferensrum </h1>
10			<p> Sök efter rum: <input type="text" placeholder="Namn" /> </p>
11 			<p> <input type="checkbox" /> Behöver projektor </p>
12			<ul>
13				<li> <button>Skatan (8 platser)</button> </li>
14				<li> <button>Bofinken (5 platser)</button> </li>
15				<li> <button>Strutsen (12 platser)</button> </li>
16			</ul>
17		</section>
18	</body>
19	</html>
```

Exempel på lösning med strategi 5 - vi skriver om rad 10. Genom att använda koden från båda ändringarna.
```html
10a	<p>
10b		Sök efter rum: <input type="text" placeholder="Namn" />
10c		<input type="checkbox" /> Behöver projektor
10d	</p>
```

### Dags att göra ändringarna

```bash
git merge book-room-feature
# Hoppsan! Git rapporterar en merge-konflikt! Så här ser filen ut:
```

Ann står i sin branch (terminalen är i Anns branch) och hon har gjort en merge med Zekes ändringar. Git markerar konflikten i index.html. Det kommer att dyka upp konfliktmarkörer i filen:

```html
 8		<section>
 9			<h1> Boka konferensrum </h1>
10	<<<<<<< HEAD  (vi står i Anns branch)
11			<p> Sök efter rum: <input type="text" placeholder="Namn" /> </p>
12	=======
13			<p> <input type="checkbox" /> Behöver projektor </p>
14			<ul>
15				<li> <button>Skatan (8 platser)</button> </li>
16				<li> <button>Bofinken (5 platser)</button> </li>
17				<li> <button>Strutsen (12 platser)</button> </li>
18			</ul>
19	>>>>>>> book-room-feature  (hämtat från Zekes branch)
20		</section>
```

### Lösningen

Ann tar bort konfliktmarkörerna från index.html och gör ändringarna. Sedan committar hon och pushar till GitHub.

```html
 8		<section>
 9			<h1> Boka konferensrum </h1>
10			<p> Sök efter rum: <input type="text" placeholder="Namn" /> </p>
11			<p> <input type="checkbox" /> Behöver projektor </p>
12			<ul>
13				<li> <button>Skatan (8 platser)</button> </li>
14				<li> <button>Bofinken (5 platser)</button> </li>
15				<li> <button>Strutsen (12 platser)</button> </li>
16			</ul>
17		</section>
```

```bash
git add index.html
git commit -m "Merge commit (search and book rooms combined)"
git push
```

Nu är konflikten löst!
