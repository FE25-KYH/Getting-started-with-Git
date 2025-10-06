# Git
## Kommandoreferens

### `git init`
Skapa ett nytt git-repository. Det har inga filer ursprungligen. Börja med att skapa en `.gitignore` fil med innehållet:
```
node_modules/
DS_Store
```

### `git status`
Ta reda på alla filers status, i ditt lokala repo. Berättar för dig om det finns konflikter.

### `git add`
Lägg till utvalda filer och mappar i *staging area*. Du kan se vad som finns i staging area med kommandot `git status`.

Finns i två versioner:
```bash
# Lägga till filer och mappar individuellt
git add file1 folder1

# Lägga till allt i repots mapp som inte är committat.
git add --all
```

### `git commit -m "Meddelande"`
Skapar en ny *snapshot* (ögonblicksbild, commit) av koden i ditt lokala repo. Glöm inte lägga till ett commit-meddelande som beskriver vad committen innehåller.

*Varning! Om du glömmer meddelandet så startas terminal-editorn Vim. [Läs här hur man avslutar den](https://stackoverflow.com/questions/11828270/how-do-i-exit-the-vim-editor)*.

När du har gjort en commit så säger vi att din *working directory* är *clean*, för det finns (för tillfället) inga ändringar som inte är committade. Flera kommandon kräver att du är *clean* för att använda dem.

### `git log`
Visar alla commits som gjorts i repot. Har många inställningar. Till exempel kan du visa de tre senaste commits som gjorts på en rad var geom att skriva:
`git log -3 --oneline`

### `git remote`
Visar om repot är anslutet till några "remote repository", till exempel GitHub. När du skapar ett nytt projekt på din dator ska du använda git remote för att lägga till det på din GitHub:
```bash
git remote add origin https://github.com/your-user-name/your-repo-name.git
```

### `git push`
Skickar alla commits du har gjort till GitHub. Innan du har pushat finns dina commits bara *lokalt*, alltså på din dator. 
Första gången behöver du använda `git push -u origin main` för att pusha till repot i GitHub. För att pusha upp en ny bransch behöver man först skriva `git push --set-upstream origin branch-name`

### `git fetch`
Ta reda på *om* det finns nya commits på GitHub. Om du skriver `git status` efteråt så får du veta om du är "up to date" eller om det finns nya uppdateringar som du behöver hämta.
 
### `git pull`
Hämta nya uppdateringar från GitHub. Hämtar alla commits som du inte har på din dator.

### `git branch`
Visar vilka branches som finns på din lokala dator. Du kan skapa en ny branch genom att skriva `git branch new-branch-name`.

### `git checkout branch-name`
Säger åt git att byta branch. Detta går bara om du är *clean*.

När man byter branch så uppdateras alla filer i mappen, så att de matchar den valda branchen.

### `git merge from-branch`
Hämtar ändringar som gjorts i "from-branch" och kombinerar dem med din aktuella branch. Vi använder det för att kombinera kod. Git försöker lösa det automatiskt, men om två personer gjort ändringar i samma fil får vi en *merge-konflikt*.

### `git reset`
Används till många olika saker, som har att göra med tidigare commits:
+ ångra `git add` (*unstage*)
+ backa till en tidigare commit utan att ändra filerna
+ se hur en eller flera filer såg ut i en tidigare commit
+ "ångra" en commit

---

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

# Ångra och återställa

Olika sorters ångra är olika svårt! Så länge vi bara jobbar på vår egen dator är det relativt lätt att backa bandet. Men så snart något är uppladdat på GitHub, så kan mina teammedlemmar använda koden. Därför är det svårare att ångra

1. Ångra `git add` - jag har lagt till filer i staging area av misstag
1. Ångra `git commit` - jag har committat filer av misstag, men inte pushat
1. Ångra `git push` - jag har pushat en eller flera commits
1. Specialfall - jag pushade lösenordet till databasen till GitHub

## 1 Ångra git add
Använd om du har lagt till filer med `git add`, men inte committat än.
```bash
# Backa ändringar i en fil
git reset HEAD fil1.js fil2.js

# Backa alla ändringar
git reset HEAD
```

## 2 Ångra git commit
Använd om du har committat en eller flera gånger, men inte pushat än. Börja med att ta reda på vilken revision du vill backa till.

```bash
# Se de senaste 5 committsen
git log --oneline -5

#ce9a71b (HEAD -> main) En commit som jag vill ångra
#afc4fb6 En commit som jag vill tillbaka till
```

Varje revision har en unik kod på 7 tecken. Koden för vår mål-revision är `afc4fb6`. Byt ut den mot din eget revisionskod.

```bash
git reset afc4fb6 --hard
```

Obs! Flaggan `--hard` talar om att filerna i *working directory* ska slängas. Man kan provköra utan den, om man är osäker.

## 3 Ångra git push
Hoppsan! Om du har pushat till GitHub, så finns det en risk att någon annan i ditt team har laddat ner dina ändringar och gjort merge med sin kod. Bäst är att göra en ny commit, som återställer koden till det tillstånd du vill att den ska vara i.

1. Prata med alla teammedlemmar, så att de vet att de ska undvika din branch tills du har löst problemet
1. Skapa en "motsatscommit"
1. Pusha den

> Exempel: om jag har gjort en commit som lägger till en extra rad, så är motsatscommiten till den, en commit som tar bort raden.

#### `git revert`
Revert skapar en motsatscommit för en specifik revision. Man måste ange HEAD eller SHA-1 koden.
```bash
# ångra bara den senaste, pushade committen
git revert HEAD

# ångra en specifik commit: afc4fb6
git revert afc4fb6

# Glöm inte att kontrollera hur det gick!
git status
```

#### Ångra flera committs
Flera revert efter varandra kan klumpas ihop till en commit.
```bash
git revert ce9a71b --no-commit
git revert afc4fb6 --no-commit
git commit -m "Revert: berätta vad som ingår i committsen du ångrar"

# Kolla så att allt ser bra ut innan du pushar!
git status

git push
```

## 4 Pushade känsliga uppgifter
You're screwed. Det går inte att 100% garantera att ingen kan plocka ut lösenordet ur koden på GitHub. Byt alla lösenord som har läckt.
