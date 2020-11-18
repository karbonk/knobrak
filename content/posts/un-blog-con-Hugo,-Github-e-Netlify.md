---
title: "Un blog con Hugo, Github e Netlify"
date: 2020-10-05T18:12:37+01:00
featured_image: "/images/hugo_github_netlify.png"
draft: false
---

[Hugo](http://gohugo.io/) è uno dei più popolari generatori di *siti statici* open-source ed è scritto in GO. È semplice, efficiente, facile da scalare e veloce da implementare. Basta installarlo, clonare i temi che ti piacciono da Github o dal sito ufficiale HUGO, apportare alcune modifiche al file di configurazione e distribuire, poi la pagina sarà online

[Github](https://github.com/) fornisce hosting statico gratuito e veloce su SSL per pagine personali, organizzative o di progetto direttamente da un repository GitHub.

[Netlify](https://www.netlify.com/) fornisce servizi di distribuzione continua, con  un'interfaccia basata su browser e molte altre caratteristiche per la gestione del sito web costruito con Hugo.

Ma cos'è un sito statico?

Un sito statico utilizza JAMstack (JavaScript, API & Markup) in contrapposizione ad architetture come LAMPstack.
I siti statici utilizzano l'elaborazione lato Client invece di un lato Server (database e linguaggi di scripting come PHP) su cui si basa Wordpress. Che cosa significa questo? per uno, i siti statici sono generalmente più veloci da caricare, in quanto per progettazione sono leggeri.

Tuttavia, poiché il sito si basa su processi lato client, è possibile essere limitati da contenuti dinamici, come i moduli di login e di contatto, che richiedono una certa pianificazione e non sono così semplici da implementare come in Wordpress.

## Hugo

### Installare Hugo
Le principali distribuzioni linux hanno Hugo nei loro repository. Per installarlo:

~~~
:~$ sudo apt-get install hugo
~~~

Il problema è che nella mia distribuzione Debian 10 la versione è obsoleta 0.55.6+really0.54.0-1 e molti temi di Hugo prevedono versione più recenti.
Ho scelto di installare il binario per la mia distribuzione linux che quando scrivo è hugo_0.76.3_Linux-64bit.deb.

~~~
:~$ sudo dpkg -i hugo_0.76.3_Linux-64bit.deb
~~~

### Costruire il sito in locale
Il primo passo è quello di creare una directory in locale dove si desidera salvare il sito web, nel mio caso creo la directory `hugo_sites`, in seguito la seleziono:

~~~
:~$ mkdir hugo_sites
:~$ cd hugo_sites
~~~

Dovremo ora creare la struttura del sito con Hugo. L'unica cosa sa fare è definire come si chiamerà il nostro sito in locale. Scelgo `mysite_local`:

~~~
~/hugo_sites$ hugo new site mysite_local
~~~

Hugo costruisce in locale all'interno della directory `hugo_sites` la directory `mysite_local` e presenta questa schermata.

{{<figure src="/img_hugo/01_hugo.png" class="mw7">}}

La struttura delle directory all'interno di `mysite_local`:

{{<figure src="/img_hugo/02_hugo.png" class="mw6">}}

A cosa servono questi file / cartelle?

Quelli con cui lavoreremo:
* `config.toml`: il file di configurazione, da modificare in seguito;
* `content`: memorizza tutto il contenuto del sito web, compresi tutti i post del blog, i curriculum vitae, di solito tutti file .md, può includere anche cartelle;
* `static`: memorizza i file statici come le immagini di sfondo, i loghi, css, js, ecc. I file in questa directory vengono copiati direttamente su /pubblic, directory ancora non presente, che verrà creata in seguito;
* `themes`: ora è vuota, ma salverà in seguito il tema scelto.

Gli altri, non saranno usati:
* `archetypes`: file .mdtemplate memorizzati;
* `data`: memorizza i file di dati per le chiamate di template;
* `layouts`: archivia file .htmltemplate;

Inoltre, verrà creata una cartella `pubblic` al momento dell'implementazione del sito web:
* `public`: dopo aver eseguito il comando `hugo` all'interno della directory `mysite_local` memorizzerà il file statico generato per l'implementazione del sito web.

### Scegliere un tema
Scegliere un tema dal sito ufficiale [Hugo themes](https://themes.gohugo.io/).
Ho usato il tema [Ananke](https://themes.gohugo.io/gohugo-theme-ananke/) tema usato de Hugo nel suo [Quick Start](https://gohugo.io/getting-started/quick-start/).

Si esegue nella directory `mysite_local` i seguenti comandi nel terminale:

~~~
~/hugo_sites$ cd mmysie_local
~/hugo_sites/mysite_local$ git init
~/hugo_sites/mysite_local$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
~~~

Il metodo di clonazione usato da git, `git clone`, per installare i temi non è supportato da Netlify. Un modo migliore è quello di installare un tema come sottomodulo git (vedi l'ultimo comando sopra).

### Modificare config.toml
Nella cartella "mysite_local" il file "config.toml" ora ha solo tre righe:
~~~
baseURL = "http//example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
~~~

Conviene sostituire il file `~/hugo_sites/mysite_local/config.toml` con il file `~/hugo_sites/mysite_local/themes/ananke/exampleSite/config.toml`. Ora il file `config.toml` di `mysite_local` è questo:
~~~
title = "Notre-Dame de Paris"
baseURL = "https://example.com"
languageCode = "en-us"
theme = "gohugo-theme-ananke"
themesDir = "../.."

DefaultContentLanguage = "en"
SectionPagesMenu = "main"
Paginate = 3 # this is set low for demonstrating with dummy content. Set to a higher number
googleAnalytics = ""
enableRobotsTXT = true

[sitemap]
  changefreq = "monthly"
  priority = 0.5
  filename = "sitemap.xml"

[params]
  favicon = ""
  site_logo = ""
  description = "The last theme you'll ever need. Maybe."
  facebook = ""
  twitter = "https://twitter.com/GoHugoIO"
  instagram = ""
  youtube = ""
  github = ""
  gitlab = ""
  linkedin = ""
  mastodon = ""
  slack = ""
  stackoverflow = ""
  rss = ""
  # choose a background color from any on this page: http://tachyons.io/docs/themes/skins/ and preface it with "bg-"
  background_color_class = "bg-black"
  featured_image = "/images/gohugo-default-sample-hero-image.jpg"
  recent_posts_number = 2
  ~~~ 

Questo file verrà modificato con un editor di testo:

~~~
title = "Mysite blog" # sostituite con il nome da dare al sito
baseURL = "/" # importante per la distribuzione del sito in netlify
languageCode = "en-us"
theme = "ananke" # non theme = "gohugo-theme-ananke"
# themesDir = "../.."  questa voce va commentata o eliminata

DefaultContentLanguage = "it" # imposta il linguaggio del sito a italiano
SectionPagesMenu = "main"
Paginate = 3 # this is set low for demonstrating with dummy content. Set to a higher number
googleAnalytics = ""
enableRobotsTXT = true

[sitemap]
  changefreq = "monthly"
  priority = 0.5
  filename = "sitemap.xml"

[markup] # questa sezione va aggiunta se si vuole combinare codice html e Markdown
  [markup.goldmark]
    [markup.goldmark.renderer]
      hardWraps = true
      unsafe = true
      xHTML = true

[params]
  show_reading_time = true # inserisce il tempo di lettura e il numero di parole
  author = "Jhon Doe" # autore del sito
  favicon = ""
  site_logo = ""
  description = "una descrizione del sito"
  facebook = "" # inserire i riferimenti social
  twitter = ""
  instagram = ""
  youtube = ""
  github = ""
  gitlab = ""
  linkedin = ""
  mastodon = ""
  slack = ""
  stackoverflow = ""
  rss = ""
  custom_css = ["css/custom.css"] #  se si vuole inserire un css personalizzato
  # choose a background color from any on this page: http://tachyons.io/docs/themes/skins/ and preface it with "bg-"
  background_color_class = "bg-black"
  featured_image = "images/banner_hugo.jpg" # va inserita un'immagine che identifichi il sito
  recent_posts_number = 2 # numero di post da presentare nella home page
  ~~~
  
### Creare il primo post
Creiamo il primo post con il comando:
~~~
~/hugo_sites/mysite_local$ hugo new posts/my-first-post.md
~~~

Se si apre `my-first-post.md` con un editor di testo in `mysite_local/content/posts` otteniamo:

{{<figure src="/img_hugo/03_hugo.png" class="mw5">}}

Come si può osservare il titolo del post è quello che abbiamo indicato nel comando di creazione del post dove gli spazi sostituiscono il carattere "-", poi è indicata la data di creazione e infine se il post è in stato di bozza (draft).
Conviene cambiare lo stato di `draft` da `true` a `false` per rendere visibile il post quando si controllerà in locale il sito (con il comando `hugo sever`) e ho cominciato a scrivere qualcosa in markdown, così:

{{<figure src="/img_hugo/04_hugo.png" class="mw6">}}

### Creare la directory /public
Questa sarà la directory utilizzata per esportare in html tutto quello che è stato costruito e modificato con Hugo. Il comando per costruire la directory è semplice quanto fondamentale per visualizzare il sito nel web:
~~~
~/hugo_sites/mysite_local$ hugo
~~~

### Creare il file netlify.toml
Questo ci servirà in seguito quando utilizzeremo netlify.com per la distribuzione via web del sito. Il file occorre inserirlo nella directory `~/hugo_sites/mysite_local` allo stesso livello di `config.toml`.
~~~
# File netlify.toml ridotto al minimo

[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.76.3" # versione installata di Hugo
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"
~~~

### Controllare il sito localmente
Prima visualizzare il sito appena creato in locale con un browser all'URL [localhost:1313](localhost:1313) occorre il comando:
~~~
~/hugo_sites/mysite_local$ hugo server
~~~
Le modifiche eseguite nella struttura e nei file del sito saranno immediatamente visibili nel browser.

## Github
La creazione di un repository pubblico su Github è gratuita e semplice da fare. 

* Installare e configurare Git;
~~~
~/hugo_sites/mysite_local$ sudo apt-get install git
~~~
* registrare un account su [GitHub](https://github.com/);
{{<figure src="/img_hugo/05_hugo_github.png" class="mw7">}}
* creare un repository.
{{<figure src="/img_hugo/06_hugo_github.png" class="mw7">}}
Si dà come nome al repository `mysite` per didtinguerlo dal sito preparato in locale `mysite_local`.
{{<figure src="/img_hugo/07_hugo_github.png" class="mw7">}}
Dopo aver creato il repository appare la seguente schermata dove in `code` copieremo l'indirizzo proposto del nostro repository `https://github.com/knobrak/mysite.git` che ci servirà quando utilizzeremo `git` per clonare il repository in locale.
{{<figure src="/img_hugo/08_hugo_github.png" class="mw7">}}

A questo punto lavoreremo con il terminale:
~~~
~/hugo_sites/mysite_local$ hugo
~/hugo_sites/mysite_local$ cd ~/hugo_sites/
~/hugo_sites/$ git init //inizializza git
~/hugo_sites/$ git clone https://github.com/knobrak/mysite.git
~~~
Git costruirà in locale la directory `mysite` che conterrà solo il file `README.md` che si affiancherà alla directory `mysite_local`. Da qui la necessità di cambiare directory alla seconda riga.

Dobbiamo ora copiare tutto il contenuto di `mysite_local` in `mysite` e andare nella directory `mysyte`.
~~~
~/hugo_sites/$ cp -R mysite_local/* mysite/
~/hugo_sites/$ cd ~/hugo_sites/mysite
~~~

Infine bisogna copiare dal locale sul repository di Github le modifiche fatte, con questi comandi git:
~~~
~/hugo_sites/mysite/$ hugo
~/hugo_sites/mysite/$ git add *
~/hugo_sites/mysite/$ git commit -m “second commit”
~/hugo_sites/mysite/$ git push
~~~
Dopo questo comando Github ci chiederà le nostre credenziali di account e dopo averle date copierà tutto quello che c'è in locale nella directory `mysite`.

Abbiamo finito con Github 

## Netlify
* Creare un account Netlify.
Occorre andare su [app.netlify.com](app.netlify.com) e selezionare il metodo di registrazione preferito. Utilizziamo il provider Git, anche se c'è la possibilità di iscriverti con un indirizzo email.
{{<figure src="/img_hugo/09_hugo_netlify.png" class="mw7">}}
Dovremo dare le nostre credenziali di Github, per registrarsi in Netlify.
{{<figure src="/img_hugo/10_hugo_netlify.png" class="mw6">}}
Ci viene richiesta l'autorizzazione a entrare nel nostro account Github, bisogna concederlo. Selezionare "Authorizi netlify".
{{<figure src="/img_hugo/11_hugo_netlify.png" class="mw6">}}
* Creare un nuovo sito con distribuzione continua.
Ora come membro di Netlify si viene diretti pannello di controllo. Selezionare "New site from git".
{{<figure src="/img_hugo/12_hugo_netlify.png" class="mw7">}}
Netlify inizierà quindi a guidare attraverso le fasi necessarie per una distribuzione continua. Per prima cosa, dovrete selezionare nuovamente il vostro git provider, nel nostro caso github.
{{<figure src="/img_hugo/13_hugo_netlify.png" class="mw7">}}
Occorre dare a Netlify i permessi per i nostri repository su Github.
{{<figure src="/img_hugo/14_hugo_netlify.png" class="mw6">}}
Selezionare il repo che si desidera utilizzare per la distribuzione continua. Nel nostro caso `mysite`, poi "Install". 
{{<figure src="/img_hugo/15_hugo_netlify.png" class="mw6">}}
* Configurazione delle impostazioni
Occorre verificare le impostazioni della configurazione per Netlify. Dovrebbero andare bene in quanto le abbiamo date con il file `netlify.toml`, poi "Deploy site".
{{<figure src="/img_hugo/16_hugo_netlify.png" class="mw6">}}
* Il sito è distribuito con un URL non proprio comodo `https://youthful-keller-557ddf.netlify.app`. Clik su "Site settings".
{{<figure src="/img_hugo/17_hugo_netlify.png" class="mw7">}}
* Cambiare l'URL proposto con un dominio di secondo livello di netlify.app. Selezionare "Domain management", poi "Options" e infine "Edit site name".
{{<figure src="/img_hugo/18_hugo_netlify.png" class="mw7">}}
* Digitare il nome del sito e poi "Save". Il sito sarà visibile all'indirizzo [https://mysite101.netlify.app](https://mysite101.netlify.app).
{{<figure src="/img_hugo/19_hugo_netlify.png" class="mw6">}}

Abbiamo finito anche con Netlify.
