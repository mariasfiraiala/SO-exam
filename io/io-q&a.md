# I/O Q&A

### 1. Ce conține un FCB (File Control Block)?

   Un FCB conține: un identificator (ino), permisiuni, ownership, size, timestamps, pointeri la DB-uri și numărul de link-uri.

### 2. Ce reprezintă un descriptor de fișier?

   Descriptorul de fișier este un indice în tabela de file descriptors.
   Ca intrare pentru un descriptor de fișiere este reținut un pointer către un open channel structure.

### 3. Ce reprezintă tabele de descriptori de fișiere?

   Tabelele de descriptori de fișiere rețin toate fișierele pe care un proces le folosește la un moment dat.
   Acestea sunt vectori de pointeri către open file structures și sunt populate prin apeluri precum `open()`, `pipe()`, `dup()`, `create()`, `socket()` sau `accept()`. 

### 4. Câte tabele de descriptori de fișiere se găsesc într-un sistem de operare?

   Câte procese.

### 5. Ce efect are apelul dup()?

   Duplică un file descriptor deja existent și ocupă o poziție nouă în FDT.

### 6. Ce efect are apelul close()?

   Eliberează file descriptor-ul asociat.
   Scade numărul de link-uri către un open file structure.
   Dacă numărul ajunge să fie 0, atunci open file structure-ul este eliberat la rândul său.

### 7. Ce apeluri modifică pointer-ul/cursorul de fișier (file pointer)?

   `open()`, `read()`, `write()`, `lseek()`

### 8. Ce apeluri modifică dimensiunea fișierului?

   `open()`, `write()`, `ftruncate()`

### 9. Ce efect are apelul/comanda truncate?

   Setează dimensiunea fișierului la size-ul care este dat ca argument.

### 10. Care sunt tipurile de fișiere pe un sistem de fișiere uzual Unix?

   regular, directory, symlink, FIFO, block, char, socket.

### 11. Ce este un sistem de fișiere virtual?

   Un sistem de fișiere virtual este o interfață abstractă pe care sistemul de operare o expune către sistemele de fișiere tradiționale (ext3, VFAT, FreeBSD).
   Astfel, diferențele de implementare interne fiecărui astfel de sistem de fișiere nu devin o problemă, iar kernelul dobândește o modalitate de comunicare facilă cu multiple sisteme de fișiere simultan.

### 12. Ce este un dispozitiv virtual?

   Un dispozitiv virtual mimează existența unui dispozitiv hardware fizic, acesta fiind, însă, o simplă intrare în sistemul de fișiere și neavând niciun backup fizic în spate.
   Un astfel de dispozitiv virtual este creat cu apelul `mknod`.

### 13. Ce tipuri de dispozitive cunoașteți? Clasificați-le din orice punct de vedere cunoașteți.

   În funcție de câți bytes gestionează:

   1. char devices (se ocupă cu un singur caracter la un moment dat - dispozitive seriale): tastatură, mouse, terminal.

   2. block devices (se ocupă cu blocuri de date cu dimensiunea multiplu de 512 - random acces devices): disk, CD-ROM, flash drives  

### 14. Cu ce diferă un dispozitiv de tip bloc de un dispozitiv de tip caracter? Dați câte un exemplu de fiecare.

   1. char devices: pentru char devices nu se face buffering; read și write există ca funcții și sunt blocante; au un driver mai simplu, pentru ca sunt de tip stream și nu au de reținut decât o singură poziție (cea curentă).

   2. block devices: block devices sunt accesate prin cache; read și write sunt asincrone; au un driver complicat, pentru că acesta trebuie să treacă prin buffer cache pentru a scrie și citi.

### 15. De ce nu are sens operația de seek pe un dispozitiv de tip caracter?

   Pentru că dispozitivele de tip caracter sunt de tip stream și rețin un singur byte la un moment dat.

### 16. Ce adresă IP locală și ce port local are un socket întors de apelul accept()?

   Apelul `accept()` întoarce un fd pentru un socket cu adresa IP a rețelei locale și portul aplicației care dorește să se conecteze.

### 17. Ce valoare poate întoarce un apel read() sau un apel write()?

   `read()` întoarce numărul de bytes citiți, 0 pentru EOF sau -1 în caz de eroare.

   `write()` întoarce numărul de bytes scriși sau -1 în caz de eroare.

### 18. Ce operații se pot face pe fișiere?

   `create()`, `open()`, `read()`, `write()`, `lseek()`, `truncate`, `close`, `delete`

### 19. Ce operații asupra fișierelor modifică/nu modifică valoarea cursorului unui fișier?

   Modifică cursorul: `open()`, `read()`, `write()`, `lseek()`.

   Nu modifică cursorul: `ftruncate()`

### 20. Ce operații asupra fișierelor modifică/nu modifică dimensiunea fișierului?

   Modifică dimensiunea: `open()`, `write()`, `truncate()`.

   Nu modifică dimensiunea: `read()`, `lseek()`

### 21. Unde este reținută valoarea cursorului de fișiere (file pointer) și unde este reținută dimensiunea fișierului?

   Cursorul de fișiere este reținut în open file structure, pe când dimensiunea fișierului este reținută în FCB.

### 22. De ce avem două buffere asociate fiecărui socket, ce rol are fiecare?

   Socket-ul, spre deosebire de pipe, permite comunicarea bidirecțională.
   Pentru a asigura faptul că sender-ul și receiver-ul nu trimit informație dintr-o parte în alta în același timp, pe același canal, există două buffere: unul de send și altul de receive.

### 23. Ce este o operație asincronă?

   O operație asincronă este caracterizată de faptul că nu poate fi determinat momentul în care aceasta apare și de faptul că rulează în mod independent.

### 24. Ce este o operație neblocantă?

   O operație neblocantă este caracterizată de faptul că se întoarce imediat, indiferent dacă a finalizat acțiunea sau nu.
   Spre exemplu, un `read()` neblocant ar citi direct (dacă ar avea ce) sau s-ar întoarce cu un mesaj de eroare special, care anunță faptul că nu și-a dus la bun sfârșit task-ul, pentru că s-ar fi blocat. 

### 25. Ce întoarce o operație asincronă?

   Operațiile asincrone nu întorc date, ci un cod de eroare, 0 sau 1.
   Datele pot fi accesate printr-un `eventfd()`.

### 26. Ce întoarce o operație blocantă?

   Operația blocantă întoarce întotdeauna rezultatul acțiunii; tocmai de aceea este blocantă, pentru că așteaptă mereu până la încheierea task-ului.

### 27. Cu ce diferă un socket de rețea de un socket UNIX?

   Socket-ul de rețea este folosit pentru comunicarea proceselor de pe sisteme diferite.
   Socket-ul UNIX este o formă de comunicare între procese aflate pe același sistem.

### 28. Care este diferența între un pipe anonim și un pipe cu nume (named pipe)?

   Pipe-ul cu nume permite comunicarea între două procese neînrudite, pe când prin pipe-ul anonim se poate transmite informație doar între procese înrudite.
   Mai mult, pipe-ul cu nume apare ca o intrare în sistemul de fișiere și reprezintă chiar un tip de fișier (FIFO).

### 29. Ce este buffer cache-ul? Care este rolul său?

   Buffer cache-ul este un buffer de blocuri aflat în kernel space.
   Acesta reține blocurile de date folosite recent, pentru a scădea numărul de interacțiuni cu disk-ul, o componentă extrem de lentă față de memorie.

### 30. De ce operația write pe fișiere este foarte rar blocantă?

   Operația de `write()` ar fi blocantă dacă buffer-ul din kernel space ar fi plin.
   Acesta este extrem de rar plin, pentru că dacă se umple, este fie eliberat (prin scrierea pe disk a informațiilor pe care le reține), fie mărit.

### 31. În ce situație operația read() pe fișier se blochează?

   Operația de `read()` este blocantă în cazul în care buffer-ul din kernel space este gol.
   Acțiunea va fi blocată până când cel puțin un octet ajunge în buffer.

### 32. Care este rolul unui device driver?

   Device driver-ul este o bucată de software folosită pentru a controla diferite componente hardware.
   Are rolul de a permite sistemului de operare să se folosească de periferice.

### 33. Ce rol are controller-ul hardware?

   Controlează componentele hardware (are rol de administrator).
   Este la rândul său o componentă hardware care permite transmiterea, flow-ul de date, între mai multe entități fizice.

### 34. Ce înseamnă zero-copy? Ce mecanism/apel folosește zero-copy?

   Zero-copy se referă la transferul de date între mai multe buffer-e din kernel space fără a se trece prin user space.
   Interacțiunea cu aplicația devine minimă, iar costul temporal adus de domain switch este evitat.

   Apeluri folosite pentru zero-copy sunt `sendfile()`, `splice()` (UNIX), `TransmitFile` (Windows).

### 35. Ce garanții ni se oferă în momentul în care apelul send() se întoarce în user space?

   Că datele în întregime sau doar o parte  (de aceea `send()` trebuie făcut într-un loop pentru a asigura că s-au trimis toate datele) au fost copiate într-un buffer de send.

### 36. Cu ce diferă afișarea folosind printf() față de folosirea write()?

   `printf()` spre deosebire de apelul de sistem `write()`, este (line) buffered, în sensul în care menține un buffer în user space care va fi flush-uit și trimis spre kernel mai rar (la endline, la apel explicit de flush, la închiderea descriptorului sau când bufferul din user space s-a umplut).

### 37. De ce subsistemul de networking nu folosește buffer cache-ul?

   Pentru că buffer cache-ul este un cache exclusiv pentru lucrul cu disk-ul.
   Blocurile de date cele mai frecvent folosite sunt aduse în kernel space (în acest buffer) și accesate direct de aici, deoarece interacțiunea cu disk-ul este scumpă din punct de vedere al timpului (memoria persistentă e lentă).
   Astfel, buffer cache-ul nu ar avea sens pentru subsistemul de networking care funcționează pe bază de pachet, ca unitate fundamentală de comunicare.

### 38. Ce rol are apelul / comanda sync?

   `sync()` realizează sincronizarea dintre buffer-ele din kernel space cu ceea ce rezidă pe disk.

### 39. Care este rolul apelului ioctl / DeviceIoControl?

   `ioctl()` este un apel de sistem ce se ocupă cu comunicarea dintre aplicații și device driver-e.
   El poate fi folosit pentru mai multe tipuri de fișiere, precum char/block devices, sockets, în general tot ceea ce are un file descriptor.

### 40. De ce în general doar utilizatorul root are permisiuni de scriere (uneori doar root are permisiuni de citire) pe intrările din /dev?

   Pentru că interacționarea cu device-urile hardware se face privilegiat.
   Astfel, user-ul nu poate aduce riscuri de securitate, iar comunicarea este lăsată în mâna kernel-ului, aceasta rămânând funcțională și sigură.

### 41. De ce este apelul fwrite() mai rapid decât write atunci când facem multe scrieri?

   `fwrite()` fiind buffered, devine mai rapid la mai multe scrieri: acesta le va combina pe toate într-un singur mare apel de sistem, în loc să facă pentru fiecare acțiune de scriere un syscall separat.

### 42. Ce se întâmplă dacă facem open de mai multe ori consecutiv pe același fișier?

   Se va adăuga un nou file descriptor în FDT și se va crea un alt open file structure pentru fd-ul creat.

### 43. Cum se modifica tabelul de file descriptori după open() si dup()? Este echivalent cu doua apeluri open()?

   `open()` creează un nou file descriptor și un nou open file structure în FDT, pe când `dup()` creează un nou file descriptor și point-ează către același open file structure de la vechiul fd, pe care îl primește ca argument, incrementând numărul de referințe (link-uri) din structură.
   `dup()` nu este neapărat echivalent cu două apeluri `open()` pentru că **nu** creează un open file structure; el doar copiază referința dintr-o parte a vectorului de fd-uri în alta.

### 44. Ce fișiere sunt deschise, în general, la crearea unui proces nou?

   `stdin`, `stdout`, `stderr` cu file descriptorii 0, 1, 2.

### 45. De ce este utilă prezența unor dispozitive pur virtuale în ierarhia /dev (ex. /dev/vboxnetctl, /dev/urandom)?

   Pentru a putea emula un tip de dispozitiv hardware fără a avea la îndemână piesa respectivă; pentru a putea avea funcționalități precum cele date de urandom și random, pentru care nu există un echivalent fizic.

### 46. De ce, în general, apelul write() pe un fișier nu blochează threadul curent?

   Pentru că realizează un simplu `memcpy()` între buffere-ele din user space și cele din kernel space.

### 47. În ce situație un apel read() pe un fișier blochează threadul curent?

   Atunci când buffer-ul din kernel space e gol.

### 48. De ce spunem că operațiile read() / write() seamănă cu operația memcpy()?

   Operațiile de `read()` și `write()` seamănă cu `memcpy()` deoarece doar trimit informația dintr-o parte în alta (buffer-e US - KS) prin copiere.
