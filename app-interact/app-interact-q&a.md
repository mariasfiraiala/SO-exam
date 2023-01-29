# App Interaction Q&A

1. Ce forme de comunicare inter-proces cunoști?

   * comunicare: pipe-uri, fișiere, sockets, shared memory

   * sincronizare: prin semafoare cu nume

   * semnale

1. Ce este un semnal? Când se trimite un semnal către un proces?

   Semnalul este un mesaj trimis asincron proceselor pentru a declanșa un anumit comportament.
   La primirea semnalului este rulat un handler specific care tratează eroarea, întrerupe, suspendă sau omoară procesul.

1. Cine trimite un semnal unui proces?

   Sistemul de operare, alt proces sau chiar el însuși.

1. Cum este implementat operatorul | din shell?

   `|` este implementat similar cu `pipe()` din C: este folosit un buffer comun în care primul proces trimite informație, iar cel de-al doilea o primește.
   Cele două capete ale buffer-ului corespund celor două procese care comunică.

1. Care sunt avantajele și dezavantajele folosirii memoriei partajate pentru comunicarea inter-proces?

    Avantaje: mai rapidă decât orice alt tip de comunicare, ieftină ca memorie, este evitată folosirea și copierea între multiple buffer-e.

    Dezavantaje: sincronizarea trebuie făcută "de mână" de către programator.

1. Care sunt avantajele și dezavantajele pipe-urilor pentru comunicarea inter-proces?

   Avantaje: comunicare facilă și relativ rapidă între procese înrudite (pentru pipe-uri anonime) sau neînrudite (pipe-uri cu nume).

   Dezavantaje: comunicarea nu e bidirecțională (există doar sender și receiver) și dimensiunea limitată a buffer-ului

1. Care este limitarea folosirii pipe-urilor anonime?

   Pot întreține comunicarea doar între procese înrudite.

1. Care este diferența principală între FIFO și socket UNIX?

   FIFO-urile au un singur canal de comunicare (unidirecțional), pe când socket-urile UNIX prezintă două buffer-e (comunicare bidirecțională), unul pentru send și unul pentru receive.

1. Ce primitive de interacțiune pot fi folosite între procese de pe sisteme diferite?

   Doar socket-uri de rețea.

1. Ce primitive de comunicare pot fi folosite doar între threaduri?

   Doar shared memory.

1. Ce primitive de comunicare pot fi folosite doar între procese înrudite?

   Doar pipe-uri anonime.

1. Cum pot două procese să folosească semafoare anonime?

   Două procese înrudite pot folosi același semafor anonim dacă la crearea acestuia este pus 1 pentru argumentul de `pshared`.

1. Cum pot două procese neînrudite să folosească semafoare anonime?

   Două procese neînrudite pot folosi același semafor anonim dacă acesta este plasat într-o zonă de memorie pe care procesele o share-uiesc.

1. Ce rol are apelul mmap() în cazul partajării memoriei între două procese?

   `mmap()` (prin folosirea flag-ului `MAP_ANONYMOUS|MAP_SHARED`) creează zone de memorie anonime ce pot fi folosite de procese înrudite.
   Mai mult, map-ează fișiere ce folosesc atât mecanismele de demand paging cât și de lazy loading pentru o folosire cât mai eficientă a memoriei.

1. Ce mecanisme putem folosi pentru a asigura accesul exclusiv la o zonă de memorie partajată între două procese?

   Sincronizarea proceselor este mecanismul prin care asigurăm acces exclusiv: vom folosi mutex-uri (reținute în zone partajate de memorie) sau semafoare (mult mai ușor cu semafoare, pentru că putem folosi semafoare cu nume).

1. De ce este nevoie de sincronizare la folosirea memoriei partajate dar nu la folosirea unui socket pentru comunicarea între două procese?

   Pentru că sincronizarea zonelor de memorie partajată este imposibil de asigurat de kernel.
   Acesta, însă, garantează sincronizarea socket-urilor și a celorlalte tipuri de comunicare prin stream-uri.

1. Ce înseamnă aplicație omogenă? Dați un exemplu.

   O aplicație omogenă folosește thread-uri/procese pentru a realiza același tip de acțiune concomitent ("Multiple threads/processes doing the same work").
   De obicei, astfel de aplicații dau dovadă de interacțiune minimală între thread-uri/procese, acestea doar dau join/wait la finalul execuției job-ului.

   ex: multi-threaded web servers, `libx264` din `ffmpeg` (thread-uri), paginile de browser din firefox (thread-uri), paginile de browser din chrome (procese).

1. Ce înseamnă aplicație eterogenă? Dați un exemplu.

   O aplicație eterogenă folosește thread-uri/procese pentru a realiza tipuri diferite de acțiuni concomitent ("Multiple threads/processes, each doing different work").
   De obicei, astfel de aplicații dau dovadă de interacțiune bogată între thread-uri/procese, sunt folosite mecanisme de IPC.

   ex: postfix, gitlab.

1. Care este avantajul și dezavantajul distribuirii unei aplicații pe mai multe sisteme?

   Avantaje: se evită încărcarea sistemului propriu, putere computațională mai mare (sisteme distribuite).

   Dezavantaje: comunicare mai greoaie între componente (se pot folosi doar sockets de rețea, mai scumpi ca timp)

1. Care este avantajul implementării unei aplicații în format multi-process față de format multi-threaded?

   Securitate: procesele nu pot accesa facil memoria unui altuia.

   Robustețe: dacă un proces crapă, nu crapă tot programul (așa cum s-ar întâmpla la thread-uri).

   Scalabilitate: poți avea mai multe procese ale aceleiași aplicații rulând concomitent pe mai multe sisteme.

1. Cum pot două procese partaja memorie anonimă? Adică folosind flagul MAP_ANONYMOUS la mmap().

   Folosirea flag-ului `MAP_ANONYMOUS` (împreună cu `MAP_SHARED`) are următoarele efecte:

   * map-are și zeroizarea unei zone distincte de memorie

   * copiii moștenesc zonele map-ate ale părinților

   * nu există copy on write când unul dintre procese încearcă să scrie

   * zonele map-ate anonim sunt folosite ca mecanism de IPC, ceea ce numim shared memory

   Mai putem map-a anonim memorie și prin deschiderea lui `/dev/zero` pentru citire, după cum urmează:

   ```C
    fd = open("/dev/zero", O_RDWR);   
    addr = mmap(NULL, length, PROT_READ | PROT_WRITE, MAP_PRIVATE, fd, 0);
   ```

1. Ce mecanism de interacțiune între aplicații au un buffer și ce mecanisme de interacțiune nu au un buffer?

   * cu buffer: pipes, sockets

   * fără buffer: fișiere, shared memory, mutex-uri, semafoare, semnale
