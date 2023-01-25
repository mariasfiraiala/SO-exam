## Software Stack Q&A

1. Ce este un apel de sistem?

   Apelul de sistem este un API expus de kernel pentru domain switch: trecerea din user space în kernel space.
   Asigură o metodă programatică prin care se cere un serviciu de la nucleu, precum interacțiunea cu I/O-ul, crearea de noi procese sau alocarea de memorie.

1. De ce sunt necesare apeluri de sistem?

   Apelurie de sistem sunt necesare din două puncte de vedere:

   * pentru a extinde funcționalitățile kernel-ului

   * pentru separarea, protejarea, încapsularea kernel-ului de restul aplicațiilor, prin partiționarea pe ring-uri.

1. Ce avantaj / dezavantaje au apelurile de sistem?

   Avantaj: Mențin kernel-ul izolat de restul aplicațiilor, rulând în mod privilegiat, permințând în același timp accesul optim la resursele hardware (pentru care există cerere mare din partea aplicațiilor).

   Dezavantaj: Overhead temporal.

1. Ce înseamnă user/application mode/space (mod neprivilegiat)? Ce înseamnă kernel/supervisor mode/space (mod privilegiat)?

   Users space-ul este nivelul de privilegiu 3 (`CPL`/`CPSR` = 3), în care aplicațiile, procesele rulează.

   Kernel space-ul este nivelul de privilegiu 0 (`CPL`/`CPSR` = 0), în care nucleul rulează, asigurându-se de împărțirea corectă a resurselor.

1. Cum se realizează tranziția din mod neprivilegiat în mod privilegiat?

   Tranziția se realizează prin API-ul pus la dispoziție de sistemul de operare (syscall API), chemat de aplicații prin wrapper-e de bibliotecă sau direct, prin inline asm.

1. Ce se întâmplă în momentul tranziției în mod privilegiat? Cum se/Cine asigură (enforcement) existența modului privilegiat?

   * Se invocă un wrapper de bibliotecă;

   * Funcția pregătește argumentele și mută în `eax` numărul syscall-ului;

   * Funcția cheamă un trap;

   * Procesorul este mutat în kernel mode;

   * Kernel-ul invocă o rutină de syscall;

   * Registrele sunt salvate pe stiva din kernel;

   * Argumentele sunt verificate pentru a fi valide;

   * Acțiunea dorită este lansată.

   Astfel, după cum se observă, aplicația din user space trigger-uiește o acțiune care conduce la implicarea kernel-ului.
   Enforcement-ul distincției dintre kernel space și user space este asigurat de modurile de privilegiu ale procesorului, marcate printr-un bit.
   Acesta este salvat în registrul `CPL` pe `x86` sau `CPSR` pe `ARM`.

1. Ce este o bibliotecă?

   O bibliotecă este o colecție de obiecte.

1. Care este diferența dintre o aplicație și o bibliotecă?

   O aplicație are un entry-point, pe când biblioteca nu.
   Cu alte cuvinte, biblioteca nu poate fi executată singură.
   O aplicație, da.

1. Care este asocierea apel de bibliotecă / apel de sistem?

   Asocierea dintre un apel de bibliotecă și un apel de sistem este aceea că un apel de bibliotecă poate rezulta într-un apel de sistem.

1. Ce este entry point-ul într-un executabil?

   Entry-point-ul este prima instrucțiune care este pusă în IP la momentul lansării executabilului.
   Stre exemplu, pentru sintaxa NASM, entry-point-ul unui program este by default (pentru `ld` ca linker) simbolul `_start`.

1. Care este rolul bibliotecii standard C (libc)?

   `libc` oferă o suită de funcții, atât pentru folosirea integrală în user space (precum `strlen()`), cât și pentru interacțiunea cu kernel-ul (wrapper-e precum `printf()` sau `scanf()`).

1. Ce acțiuni se pot executa doar în mod privilegiat?

   * Întreruperi

   * Alocarea memoriei

   * Operații cu cache-ul

   * Interacțiunea cu I/O-ul

1. Ce operații / instrucțiuni low-level (ISA) se pot executa doar în mod privilegiat?

   Un exemplu este instrucțiunea `INT` (cheamă o întrerupere software).
   Un alt exemplu este `HLT` (oprește procesorul până la apariția unei întreruperi sau a unui semnal).

1. Ce este un sistem de operare monolitic?

   Un sistem de operare monolitic este un sistem de operare în care management-ul sistemelor de fișiere, a memoriei, a device-urilor și a proceselor cade în atribuțiile kernel-ului.

1. Ce este un sistem de operare de tip microkernel?

   Un sistem de operare microkernel este unul ale cărui componente (scheduler-e, sisteme de fișiere, stiva de rețea) rulează ca aplicații în user space.

1. Care sunt avantajele unui sistem de operare monolitic?

   Performanța: toate componentele esențiale se află în același loc, iar comunicare cu hardware-ul este mediată prin syscall API.

1. Care sunt avantajele unui sistem de operare de tip microkernel?

   Securitatea: kernel-ul fiind mult mai mic, suprafața de atac scade; un serviciu de sistem poate fi repornit fără un reboot al întregului sistem de operare.

1. Care tip de sistem de operare are mai multe apeluri de sistem?

   Un sistem de operare de tip monolitic, pentru că invocarea componentelor sale esențiale se face prin syscall; pe când în cazul unui sistem de tip microkernel, comunicarea se realizează în user space, fără a fi nevoie de syscall-urile specifice.

1. Care este avantajul folosirii mașinilor virtuale din perspectiva securității?

   Un attack de tip hyperjacking este foarte greu de aplicat.

1. Ce este o bibliotecă statică? Ce este o bibliotecă dinamică?

   O bibliotecă statică este una ale cărei simboluri sunt copiate direct în executabilul programului, la link time.

   O bibliotecă dinamică este una ale cărei simboluri sunt aduse în executabil la load time, printr-un segment partajat.

1. Când preferăm folosirea static linking, respectiv dynamic linking? Cu ce diferă un executabil dinamic de un executabil static?

   Se preferă folosirea static linking pentru portabilitate, mai exact în cazul în care biblioteca nu există pe sistem, sau a fost modificată.

   Se preferă folosirea dynamic linking atunci când se dorește folosirea optimă a memoriei și a disk-ului, datorită mecanismului de shared library code și a executabilelor de mărime mică.

1. Dați exemplu de apel de sistem blocant.

   `read()`

1. Dați exemplu de apel de sistem neblocant.

   `getpid()`

1. De ce, în general, o aplicație trebuie să execute un apel de sistem pentru a accesa un dispozitiv hardware? De ce NU poate accesa direct dispozitivul hardware?

   Pentru că în sistemele de operare care nu sunt unikernele, rulează în același timp mai multe aplicații.
   Majoritatea acestora doresc să interacționeze cu hardware-ul, apărând astfel probleme cu accesul concurent la resurse.
   Sistemul de operare, prin syscall (poate fi privit drept o cerere la un ghișeu), mediază accesul, forțând aplicațiile să nu se calce în picioare.

1. Ce înțelegem prin overhead spațial și overhead temporal?

   Creșterea spațiului folosit, respectiv a timpului de execuție, în general, apărută o dată cu apariția anumitor enforecement-uri.

1. Dați exemplu de mecanism / funcție care reduce overhead-ul spațial și unul care reduce overhead-ul temporal.

   Mecanismul de shared memory reduce overhead-ul spațial, prin accesarea directă a buffer-ului din kernel space.

   Mecanismul de zero copy reduce overhead-ul temporal, prin evitarea domain switch-urilor.

1. Ce înseamnă double buffering? În ce situație concretă (mecanism/funcție) apare?

   Double buffering se referă la prezența a două buffere: unul în user space și altul în kernel space.
   Apare adesea la funcții precum cele de read și write.

1. Ce se întâmplă când există o eroare critică (de tip Segmentation fault) la nivelul sistemului de operare?

   Dacă în nucleu se ajunge la Segmentation Fault, atunci sistemul de operare intră în kernel panic. 

1. Dați exemplu de un apel de bibliotecă standard C care nu cauzează apel de sistem.

   `strlen()`

1. Dați exemplu de un apel de bibliotecă standard C care cauzează apel de sistem.

   `scanf()`

1. Care este avantajul și dezavantajul existenței unei stive software la nivelul unui sistem de calcul?

   Avantaj: izolare a aplicațiilor bună, portabilitate, modularitate

   Dezavantaj: overhead temporal

1. Care este avantajul și dezavantajul folosirii unui limbaj de programare high-level (precum Java, Python, Lua)?

   Avantaj: management automat al memoriei (scapi de dureri de cap)

   Dezavantaj: overhead temporal, pierderea controlului

1. Care este avantajul și dezavantajul folosirii unui limbaj de programare low-level (precum C, C++, Rust, D)?

   Avantaj: control (faci ce vrea mușchiul tău), rapiditate

   Dezavantaj: prone to mistakes, mai greu de portat
