1. De ce este nevoie de procese?

1. De ce este nevoie de thread-uri?

1. Ce este un proces?

1. Ce este un thread?

1. Cu ce diferă un thread de un proces?

1. Cum este afectat spațiul virtual de adrese al unui proces în momentul creării unui thread?

1. Ce zone de memorie au comune thread-urile unui proces și ce zone au specifice?

1. Ce conține PCB (Process Control Block)?

1. Care sunt stările în care se poate găsi un thread?

1. Ce efect are apelul fork()?

1. Ce resurse partajează/nu partajează procesul părinte și procesul copil în cazul apelului fork()?

1. Cum modifica fork() si exec() spațiul virtual de adrese?

1. Ce efect are apelul exec()?	

1. Câte threaduri se pot găsi în starea RUNNING, READY și WAITING?

1. Ce este o schimbare de context? Ce se întâmplă la o schimbare de context?

1. Ce cauzează schimbări de context?

1. Ce este o schimbare de context voluntară și o schimbare de context nevoluntară?

1. Ce sunt threadurile cu implementare user-level și thread-urile cu implementare kernel-level?

1. În ce situație este utilă zona TLS (thread local storage)?

1. De ce este necesară sincronizarea proceselor/thread-urilor?

1. Ce înseamnă race condition?

1. Ce înseamnă deadlock?

1. Care sunt dezavantajele sincronizării?

1. Ce înseamnă TOCTOU (time of check to time of use)?

1. Când se blochează un producător în problema producator-consumator? Dar un consumator?

1. De ce este necesară prezența unei instrucțiuni de tipul atomic_compare_and_swap în fiecare ISA?

1. Cu ce diferă un spinlock de un mutex? Când folosim spinlock-uri? Când folosim mutex-uri?

1. Ce efect are folosirea operatorului & din shell în crearea unui proces?

1. Ce este un proces zombie? Cum apare un proces zombie? Care este problema proceselor zombie?

1. Ce este un proces orfan? De ce un proces este orfan foarte puțin timp?

1. Poate fi un proces zombie orfan? Ce se întâmplă cu un proces zombie orfan?

1. Ce se întâmplă dacă un thread realizează un acces invalid la o zonă de memorie?

1. Poate un thread să acceseze stiva altui thread? Cum?

1. De ce schimbarea de context între două thread-uri ale aceluiași proces este mai rapidă decât schimbarea de context între două thread-uri din procese diferite?

1. Ce limitează numărul maxim de threaduri care pot fi create în cadrul unui proces?

1. Ce limitează numărul maxim de procese care pot fi create în cadrul unui sistem de calcul?

1. Ce se întâmplă când toate procesele sistemului sunt blocate?

1. Ce înseamnă waiting time (timp de așteptare) în planificarea proceselor?

1. Putem avea un sistem multi-core cu un singur proces aflat în starea RUNNING și mai multe procese în READY?

1. Ce înseamnă starea READY/RUNNING/TERMINATED/WAITING(BLOCKED)?

1. Ce este un proces I/O intensive?

1. Ce este un proces CPU intensive?

1. Cum tratează planificatorul procesele I/O intensive și procesele CPU intensive?

1. Două thread-uri ale unui proces execută aceeași funcție. Care sunt diferențele între cele două thread-uri?

1. Cu ce diferă procesul copil (creat prin fork) de procesul părinte?

1. Cu ce diferă un proces zombie de un proces orfan?

1. Ce parametri ai planificatorului trebuie să modificăm pentru a avea un sistem cu productivitate mai mare?

1. Ce parametri ai planificatorului trebuie să modificăm pentru a avea un sistem cât mai interactiv (responsive)?

1. Am avea nevoie de folosirea unui apel de sistem pentru crearea unui thread în cazul unei implementări de tip user-level threads? Dar în cazul deschiderii unui fișier în același scenariu?

1. Ce se întâmplă dacă folosim apeluri de sistem blocante în interiorul unui spinlock?

1. De ce este necesară folosirea prefixului LOCK pentru realizarea operațiilor atomice?

1. Care este avantajul folosirii mutexurilor în defavoarea spinlockurilor și invers?

1. Care este echivalentul apelului wait() pentru threaduri?

1. De ce este nevoie de apelul pthread_wait() / pthread_join()?

1. Ce tranziție între stările unui thread nu este posibilă?
