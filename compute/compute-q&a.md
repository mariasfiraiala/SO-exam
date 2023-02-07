# Compute Q&A

### 1. De ce este nevoie de procese?

   Procesul reprezintă modalitatea prin care o acțiune primește un identificator și posibilitatea de a putea fi executată.
   El este important pentru că propune o interfață comună pentru realizarea de sarcini, iar CPU-ul (prin management-ul sistemului de operare), se ocupă de punerea în practică a acestor sarcini, împlinindu-și la rândul său scopul fundamental.

   Nevoia de procese mai e dată de faptul că pe un sistem se dorește executarea mai multor acțiuni, consolidarea mai multor aplicații.

<details>
   
   [Further Reading](https://docs.google.com/document/d/1ZneG6oNqbbzEWLA6QwODLyYvHzdFj2OJ79Xka7QkkSs/edit#heading=h.v2p2vab8hzh2)
</details>

### 2. De ce este nevoie de thread-uri?

   Thread-ul reprezintă unitatea fundamentală planificabilă, mai multe thread-uri putând face parte din același proces.
   El apare ca urmare a necesității de a paraleliza, în general, fiind utilizat omogen, pentru acțiuni identice, realizate simultan.

<details>

   [Further Reading](https://www.ibm.com/docs/sv/aix/7.1?topic=programming-benefits-threads)
</details>

### 3. Ce este un proces?

   Procesul este entitatea activă folosită pentru a executa acțiuni (nu neapărat omogene), utilizând în mod echitabil resursele sistemului.

### 4. Ce este un thread?

   Thread-ul este cea mai mică unitate de procesare programabilă de sistemul de operare pentru execuție.
   El poate fi privit ca o combinație unică de (IP, SP).

### 5. Cu ce diferă un thread de un proces?

   1. Relație de incluziune: Procesele pot avea thread-uri, thread-urile nu pot avea procese.

   2. Memoria partajată: Procesele nu partajează resurse, pe când thread-urile da (întreg VAS-ul procesului care le naște, mai puțin TLS-ul, stiva și regiștrii).

   3. Erori: Dacă un proces își încheie execuția necontrolat, din cauza unei erori, programul poate continua să ruleze normal, pe când dacă un thread crapă, tot programul crapă.

   4. Overhead: Thread-urile sunt mai lightweight și pot fi planificate mai ușor, de asemenea, ele sunt mai rapide prin timpul de creare și cel de context switch.

<details>

   [Further Reading](https://www.geeksforgeeks.org/difference-between-process-and-thread/)
</details>

### 6. Cum este afectat spațiul virtual de adrese al unui proces în momentul creării unui thread?

   Spațiul virtual de adrese al procesului primește o nouă zonă de memorie, pentru stiva noului thread.

<details>

   [Further Reading](https://stackoverflow.com/questions/38555287/how-memory-management-happens-for-process-threads-in-one-virtual-address-space)
</details>

### 7. Ce zone de memorie au comune thread-urile unui proces și ce zone au specifice?

   Au la comun .text, .data, .rodata, .bss, heap-ul, bibliotecile dinamice și kernel space-ul.
   Au specifice doar TLS-ul și stiva.

### 8. Ce conține PCB (Process Control Block)?

   SP-ul, starea procesului, PID-ul, IP-ul, regiștrii, PT, FDT. 

<details>

   [Further Reading](https://www.cs.auckland.ac.nz/courses/compsci340s2c/lectures/lecture06.pdf)
</details>

### 9. Care sunt stările în care se poate găsi un thread?

   * NEW

   * READY

   * RUNNING

   * BLOCKED

   * TERMINATED

<details>

   [Further Reading](https://docs.google.com/document/d/1Um_iOiKEeKAPP0xjr8fXFbpPx3_wBdJZtjqWinqmbqs/edit#heading=h.7sfnwfm7vsop)
</details>

### 10. Ce efect are apelul fork()?

   `fork()` creează un proces copil, se întoarce de două ori (o dată în părinte cu PID-ul copilului, și o dată în copil cu valoarea 0) și este asociat cu mecanismul de copy-on-write.

<details>

   [Further Reading](https://www.geeksforgeeks.org/fork-system-call/)
</details>

### 11. Ce resurse partajează/nu partajează procesul părinte și procesul copil în cazul apelului fork()?

   După apelul funcției `fork()`, VAS-ul părintelui este copiat și atribuit copilului.
   Inițial, cele două VAS-uri vor conține aceleași corespondențe adrese virtuale - adrese fizice; de abia la write acestea vor fi modificate, atât pentru părinte, cât și pentru copil.
   Procesul copil mai primește, de asemenea, și o copie a FDT părintelui, cu descriptori de fișiere ce point-ează către aceleași open file structures ca file descriptor-ii părintelui.

### 12. Cum modifica fork() si exec() spațiul virtual de adrese?

   Atât `fork()` cât și `exec()` creează un spațiu nou de adrese, diferența dintre cele fiind aceea că `fork()` creează un nou spațiu de adrese pentru procesul copil, pe când `exec()` golește spațiul virtual de adrese al procesului curent (mai puțin partea de kernel space) și regiștrii și pregătește pentru a rula noul program.

<details>

   [Further Reading](https://stackoverflow.com/a/1653415)
</details>

### 13. Ce efect are apelul exec()?

   De cele mai multe ori, în sistemele Unix moderne apelul `exec()` este folosit împreună cu apelul `fork()` (modelul fork-and-exec) pentru a încărca un nou program în memorie, după crearea unui nou VAS.
   Un exemplu de suprascriere utilizând `exec()` este porinirea oricărui executabil din shell: shell-ul realizează un copil al său, a cărui imagine va fi înlocuită cu imaginea executabilului, care apoi va fi lăsat să ruleze.
   Shell-ul așteaptă copilul să-și încheie execuția, și apoi, îi dezalocă resursele.

### 14. Câte threaduri se pot găsi în starea RUNNING, READY și WAITING?

   În RUNNING se găsesc atâtea thread-uri câte procesoare avem.

   În READY și în WAITING se pot găsi oricâte thread-uri.

### 15. Ce este o schimbare de context? Ce se întâmplă la o schimbare de context?

   O schimbare de context reprezintă schimbarea stării unui thread: marchează trecerea către și de la starea de RUNNING.
   O schimbare de context implică:

   * salvarea stării thread-ului: punerea regiștrilor săi pe stivă, salvare IP-ului și SP-ului în TCB

   * introducerea handle-ului pentru TCB-ul thread-ului care pleacă de pe procesor în coada de READY sau BLOCKED, de la caz la caz

   * pregătirea următorului thread care va rula: retragerea IP-ului său din TCB-ul aflat în coada de priorități, restaurarea stării regiștrilor săi

<details>

   [Further Reading](https://stackoverflow.com/questions/7439608/steps-in-context-switching)
</details>

### 16. Ce cauzează schimbări de context?

   Schimbările de context apar la plecare voluntară/nevoluntară a thread-urilor de pe procesor:

   * `yield()`: thread-ul care rulează îl lasă voluntar pe următorul din coadă să îi ia locul

   * expirarea quantei de timp: thread-ul a rulat atât cât îi fusese indicat de scheduler

   * apel blocant: thread-ul așteaptă după I/O

   * apare un thread cu o prioritate mai mare

   * exit: thread-ul și-a încheiat treaba înainte de expirarea quantei

### 17. Ce este o schimbare de context voluntară și o schimbare de context nevoluntară?

   O schimbare de context voluntară este una în care thread-ul face o acțiune despre care știe sigur că va duce la înlocuirea sa de pe procesor (o acțiune, care, programabil, îl dă afară): apelurile blocante sau cele de `yield()`.

   O schimbare de context nevoluntară nu este dată de o acțiune directă, ci de ceea ce dictează scheduler-ul, pentru a menține echitatea timpului petrecut rulând: expirarea quantei, thread prioritar 

### 18. Ce sunt threadurile cu implementare user-level și thread-urile cu implementare kernel-level?

   Thread-urile user-level (sau green threads) sunt thread-uri ce rulează exclusiv în user space, fiind controlate de biblioteci; pentru sistemul de operare, ele sunt privite ca un singur thread, putând folosi la un moment dat un singur procesor.

   Thread-urile de sistem sunt thread-uri ce rulează în kernel space (produc context switch-uri) și sunt privite ca mai multe acțiuni ce pot fi paralelizate (ocupă mai mult de un core).

   Green thread-urile sunt utile atunci când context switch-urie dese produc mult mai mult overhead față de acțiunea propriu-zisă pe care o pun în aplicare acestea.

   Cu toate astea, kernel level threads sunt ușor paralelizabile și au implementări de bibliotecă în limbaje precum C (pthread).

<details>

   [Further Reading](https://www.geeksforgeeks.org/difference-between-user-level-thread-and-kernel-level-thread/)4
</details>

### 19. În ce situație este utilă zona TLS (thread local storage)?

   TLS este utilă atunci când dorim să avem variabile statice/globale vizibile pentru toate funcțiile thread-ului, dar locale lui.
   Cu alte cuvinte, aceste variabile sunt accesabile de thread-ul de care aparțin și doar de el, pentru thread având scope global/static.

### 20. De ce este necesară sincronizarea proceselor/thread-urilor?

   Sincronizarea este folosită pentru a asigura două comportamente din partea thread-urilor: serializare și acces exclusiv.
   Prin acestea, este menținut caracterul determinist al acțiunilor care pot fi deturnate de la cursul lor natural din cauza thread-urilor sau proceselor modificând zone de memorie partajată.

### 21. Ce înseamnă race condition?

   Race-condition se referă la rezultatul nedeterminist pe care o are acțiunea făcută două sau mai multe thread-uri simultan, dorind să modifice o resursă comună.

### 22. Ce înseamnă deadlock?

   Deadlock-ul este rezultatul acțiunii prin care două thread-uri așteaptă ceva din partea unui altuia.
   Cu alte cuvinte, thread-urile depind unul de altul și nu își pot continua execuția deoarece niciunul dintre ele nu poate ieși din loop.

<details>

   [Further Reading](https://www.baeldung.com/cs/deadlock-livelock-starvation)
</details>

### 23. Care sunt dezavantajele sincronizării?

   * greu de implementat dacă programatorul nu este experimentat

   * implementare ineficientă: regiuni critice mari, deadlock-uri, race conditions

   * overhead temporal mare:

      * la instrucțiuni atomice: lock pe magistrală, un singur procesor accesează memoria

      * la spinlock: busy waiting ocupând loc pe procesor

      * la mutex/semafor: apel de sistem, context switch

### 24. Ce înseamnă TOCTOU (time of check to time of use)?

   Se referă la un race condition care apare imediat după un compare, înainte de a se face instrucțiunea următoare (de folosire), când un alt thread apare și modifică valoarea considerată "safe".

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use)
</details>

### 25. Când se blochează un producător în problema producator-consumator? Dar un consumator?

   Un producător se blochează dacă consumatorul nu a citit informația pe care el a produs-o (când buffer-ul comun e plin).

   Un consumator se blochează dacă producătorul nu i-a oferit nicio informație pe care el să o poată folosi (când buffer-ul comun e gol).

### 26. De ce este necesară prezența unei instrucțiuni de tipul atomic_compare_and_swap în fiecare ISA?

   Pentru că acest tip de instrucțiune stă la baza tuturor mecanismelor de sincronizare.
   Dacă compare and swap nu ar fi garantat atomică, nici mutex-urile, nici semafoarele sau spinlock-urile nu ar putea exista.

### 27. Cu ce diferă un spinlock de un mutex? Când folosim spinlock-uri? Când folosim mutex-uri?

   Spinlock-urile sunt o formă de busy waiting în starea de RUNNING.
   Ele nu determină ieșirea de pe procesor a thread-ului care așteaptă.
   Sunt folosite atunci când timpul de așteptare nu este mare (zone critice mici, fără apeluri blocante), iar un context switch este mai costisitor în comparație.

   Mutex-urile sunt o formă de sincronizare care funcționează prin context switch, și ocazional apel de sistem (chemarea scheduler-ului la `lock()` și `unlock`).
   Sunt folosite în toate celelalte cazuri în care spinlock-ul nu este viabil: apeluri blocante, zone critice mari

<details>

   [Further Reading](https://www.geeksforgeeks.org/mutex-vs-semaphore/)
</details>

### 28. Ce efect are folosirea operatorului & din shell în crearea unui proces?

   Shell-ul nu mai face waiting blocant pentru procesul ce primește &.
   Acesta este trecut în background, își continuă execuția, iar la final, când dă exit, părintele (shell-ul) primește semnalul `SIGCHLD` și face `wait()`.

### 29. Ce este un proces zombie? Cum apare un proces zombie? Care este problema proceselor zombie?

   Un proces zombie este un proces care și-a încheiat execuția și căruia nu i s-a făcut wait de către părintele său.
   Problema cu procesele zombie este că ocupă resurse utile (PID, intrare în tabela de procese) degeaba, deoarece acestea nu mai au nimic de executat, dar nu sunt eliberate.

### 30. Ce este un proces orfan? De ce un proces este orfan foarte puțin timp?

   Un proces orfan este un proces al cărui părinte a murit înainte lui.
   Procesele orfane sunt adoptate rapid de către `init`.

### 31. Poate fi un proces zombie orfan? Ce se întâmplă cu un proces zombie orfan?

   Da.
   Un proces zombie orfan este tratat mai întâi ca unul orfan, fiind adoptat de `init`.
   Acesta îl va aștepta și îi va elibera resursele pe care părintele inițial nu a apucat să le trateze.

### 32. Ce se întâmplă dacă un thread realizează un acces invalid la o zonă de memorie?

   Thread-ul, împreună cu întreg procesul de care ține, va primi SEGFAULT, iar programul se va încheia cu o eroare.

### 33. Poate un thread să acceseze stiva altui thread? Cum?

   Da. Există mai multe variante:

   * se reține într-o variabilă globală o adresă de stivă a altui thread (pentru thread-uri ale aceluiași proces)

   * se iau adrese random, dacă adresa este validă totul merge bine, dacă nu, `SEGFAULT` (se merge la ghicit)

<details>

   [Further Reading](https://leetcode.com/discuss/interview-question/operating-system/124628/Can-a-posix-thread-modify-another-thread's-stack-variable-within-the-same-program/236079)
</details>

### 34. De ce schimbarea de context între două thread-uri ale aceluiași proces este mai rapidă decât schimbarea de context între două thread-uri din procese diferite?

   Pentru că thread-urile aceluiași proces share-uiesc majoritatea VAS-ului procesului, iar flush-ul TLB-ului nu este necesar.

### 35. Ce limitează numărul maxim de threaduri care pot fi create în cadrul unui proces?

   Dimensiunea memoriei virtuale, deoarece pentru fiecare nou thread trebuie creată o nouă stivă în cadrul VAS-ului procesului.

### 36. Ce limitează numărul maxim de procese care pot fi create în cadrul unui sistem de calcul?

   Numărul maxim de intrări în tabela de procese.
   De obicei un număr fix setat de sistemul de operare.

<details>

   [Further Reading](https://unix.stackexchange.com/questions/124040/how-to-determine-the-max-user-process-value)
</details>

### 37. Ce se întâmplă când toate procesele sistemului sunt blocate?

   Apare un proces IDLE care rulează în mod nedefinit (prin busy waiting) până un alt proces este pregătit să intre pe procesor.

<details>

   [Further Reading](https://stackoverflow.com/questions/14315923/why-does-linux-kernel-need-idle-thread)
</details>

### 38. Ce înseamnă waiting time (timp de așteptare) în planificarea proceselor?

   Waiting time este impul pe care îl petrec procesele în starea READY.
   Ideal, acesta este cât mai mic.

### 39. Putem avea un sistem multi-core cu un singur proces aflat în starea RUNNING și mai multe procese în READY?

   Nu, dacă prin proces se înțelege un singur thread.

   Da, dacă prin proces se înțelege un proces cu mai multe thread-uri; în acest caz, toate thread-urile procesului pot rula în același timp, ocupând toate procesoarele și nepermițând altor task-uri să ruleze.

### 40. Ce înseamnă starea READY/RUNNING/TERMINATED/WAITING(BLOCKED)?

   READY &rarr; thread-ul este gata pentru a intra pe procesor, așteaptă să i se facă loc, să fie planificat

   RUNNING &rarr; thread-ul rulează, are o quantă stabilită și poate executa oricâte acțiuni pe parcursul ei

   TERMINATED &rarr; thread-ul și-a încheiat execuția și face `exit()`

   BLOCKED &rarr; thread-ul a făcut un apel blocant și așteaptă după I/O

### 41. Ce este un proces I/O intensive?

   Un proces care face multe apeluri blocante, comunică des cu diskul sau placa de rețea; trece des din RUNNING în BLOCKED.

### 42. Ce este un proces CPU intensive?

   Un proces care face multe calcule aritmetice; adeseori ajunge CPU hog și este trimis nevoluntar (la expirarea quantei) în READY.

### 43. Cum tratează planificatorul procesele I/O intensive și procesele CPU intensive?

   Procesele I/O intensive sunt prioritare: ele, în mod garantat, fac multe apeluri blocante și ajung voluntar în BLOCKED, nu au tendința de a acapara CPU-ul.

   Pe de altă parte, procesele CPU intensive nu sunt favoritele scheduler-ului: au tendința de a ajunge CPU hogs, de aceea, ele primesc o quantă mai mică, pentru a menține wainting time-ul mic și eficiența împărțirii resurselor.

<details>

   [Further Reading](https://www.baeldung.com/cs/cpu-io-bound)
</details>

### 44. Două thread-uri ale unui proces execută aceeași funcție. Care sunt diferențele între cele două thread-uri?

   Poate diferi atât SP-ul, cât și IP-ul.
   Totul depinde de rapiditatea cu care thread-urile sunt planificate; nimic nu poate garanta sau ghici ordinea lor, tocmai de aceia, sincronizarea este importantă.
   Pentru că oferă o garanție a ordinii și serializării, atunci când scheduler-ul nu poate fi ghicit.

### 45. Cu ce diferă procesul copil (creat prin fork) de procesul părinte?

   Prin PID, prin VAS-uri (după write-uri multiple, inițial sunt identice, dar cu permisiunea de write scoasă).

### 46. Cu ce diferă un proces zombie de un proces orfan?

   Procesul zombie poate avea un părinte în viață.

### 47. Ce parametri ai planificatorului trebuie să modificăm pentru a avea un sistem cu productivitate mai mare?

   Procesele CPU intensive primesc o cuantă de timp mai mare.

### 48. Ce parametri ai planificatorului trebuie să modificăm pentru a avea un sistem cât mai interactiv (responsive)?

   Procesele interactive, de I/O primesc prioritate mare.

### 49. Am avea nevoie de folosirea unui apel de sistem pentru crearea unui thread în cazul unei implementări de tip user-level threads? Dar în cazul deschiderii unui fișier în același scenariu?

   Nu este necesar un syscall pentru crearea unui thread în user space, deoarece există conceptul de green threads (fire de execuție ce rulează la nivel de bibliotecă).

   Este necesar un syscall pentru deschiderea unui fișier, deoarece, în acest caz, interacționăm cu hardware-ul (disk-ul) și avem nevoie de permisunea kernel-ului pentru a face asta.

### 50. Ce se întâmplă dacă folosim apeluri de sistem blocante în interiorul unui spinlock?

   Deadlock.

### 51. De ce este necesară folosirea prefixului LOCK pentru realizarea operațiilor atomice?

   Prefixul LOCK anunță sistemul de operare să facă un lock hardware pe magistrală.
   Un singur core mai are acces, astfel, la memorie, iar operația atomică poate avea loc.

<details>

   [Further Reading](https://stackoverflow.com/a/8891781)
</details>

### 52. Care este avantajul folosirii mutexurilor în defavoarea spinlockurilor și invers?

   Mutex-urile sunt folosibile pentru critical regions mari, cu apeluri blocante, pe când spinlock-urile sunt ieftine când vine vorba de `lock()` și `unlock()`, pentru că nu fac context switch și nici nu invocă scheduler-ul.

### 53. Care este echivalentul apelului wait() pentru threaduri?

   `join()`

### 54. De ce este nevoie de apelul wait() / pthread_join()?

   Apelurile `wait()` / `pthread_join()` realizează eliberarea resurselor folosite de proces / thread după terminarea acestora.

### 55. Ce tranziție între stările unui thread nu este posibilă?

   READY &rarr; BLOCKED: thread-ul trebuie mai întâi să fie capabil să facă o acțiune pentru a putea face un apel blocant, iar asta se întâmplă numai dacă este running.
