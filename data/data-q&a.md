# Data Q&A

### 1. Cum asigură sistemul de operare separația între procese?

   Sistemul de operare separă procesele prin mecanismul de memorie virtuală (de paginare, acces granulat la RAM).

### 2. Ce înseamnă mecanismul de memorie virtuală?

   Memoria virtuală este modalitatea prin care kernel-ul oferă proceselor impresia că dețin întreaga memorie, când lucrurile nu stau chiar așa.
   Adresele de memorie virtuală ajung să fie corelate cu unele fizice de abia la acces, ceea ce permite împărțirea corectă a RAM-ului.

### 3. Ce reprezintă spațiul virtual de adrese al unui proces?

   Spatiul virtual de adrese este o înlănțuire de zone (segmente) folosită pentru a reține ce memorie procesul dorește să rezerve sau să elibereze.
   Conține tabela de pagini a procesului, care reține corespondența dintre adresele virtuale și cele fizice.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Virtual_address_space)
</details>

### 4. Cu ce diferă zona de date de zona de cod în spațiul virtual de adrese al unui proces?

   Cele două zone diferă, practic, prin permisiunile lor: zona de date e RW-, pe când zona de cod e R-X, iar teoretic prin funcțiile lor: zona de date reține, în general, variabilele folosite de program, pe când zona de cod reține instrucțiunile care se execută.

### 5. Ce permisiuni are zona .text/.data/.rodata/.bss/de stivă/de heap?

   .text: R-X

   .data: RW-

   .rodata: R--

   .bss: RW-

   stiva: RW-

   heap: RW-

### 6. Ce este paginarea memoriei?

   Paginarea memoriei este mecanismul prin care RAM-ul este împărțit în unități de o anumită dimensiune (ex: 4096 pentru Linux).
   Aceste unități sunt apoi folosite ca metrică pentru alocarea și rezervarea memoriei, menținând accesul granulat la aceasta.

### 7. Ce rol are tabela de pagini?

   Tabela de pagini reține corespondența (map-area) dintre adresa virtuală (indexul din vectorul de pagini) și adresa fizică (valoarea reținută în vector).

### 8. Ce este și ce rol are MMU (Memory Management Unit)?

   MMU-ul (sau MTU pe `ARM`) are rolul de a translata adresa de memorie virtuală în fizică.
   El consultă PFN-ul, permisiunile și bitul de validitate pentru a determina ce face mai departe: generează un page fault major sau minor.

<details>

   [Further Reading](https://developer.arm.com/documentation/101811/0102/The-Memory-Management-Unit--MMU-)
</details>

### 9. Ce rol are TLB?

   TLB-ul este un cache pentru intrările din tabela de pagini a procesului curent.
   Acesta reține PTE-uri folosite recent, și pentru fiecare adresă virtuală, verifică dacă are adresa fizică memorată.
   Dacă da, atunci are loc un TLB hit, altfel rezultă un TLB miss.

<details>

   [Further Reading](https://developer.arm.com/documentation/101811/0102/Translation-Lookaside-Buffer-maintenance)
</details>

### 10. Care este ordinul de mărime al numărului de intrări ale TLB?

   16-512 intrări

### 11. Ce conține o intrare în tabela de pagini?

   PFN (page frame number), permisiuni, bit de validitate și un dirty bit.

### 12. Ce înseamnă tabelă de pagini multi-nivel (ierarhică)? De ce este utilă?

   Este o tabelă de pagini care ca și informație reține adrese ale altor tabele de pagini.
   Intrările ultimului nivel de tabele de pagini rețin informația legată de frame-uri.
   Primul nivel conține o singură tabelă de pagini și adresa acesteia este reținută în PTBR.

<details>

   [Further Reading](https://www.geeksforgeeks.org/multilevel-paging-in-operating-system/)
</details>

### 13. Când are loc un TLB miss?

   Un TLB miss are loc atunci când pagina pe care se află adresa accesată nu a mai fost folosită, a fost folosită cu mult timp și a fost eliminată la update-ul TLB-ului sau când efectiv nu există o adresă fizică alocată pentru cea virtuală. 

### 14. De ce se golește TLB-ul (TLB flush) la schimbare de context?

   Pentru ca TLB-ul să rețină informația procesului curent, nu pe cea a procesului precendent.
   Nu este permis, astfel, accesul proceselor la informație ce nu ar trebui să fie partajată. 

### 15. De ce nu este nevoie de TLB flush la schimbarea de context între două thread-uri ale aceluiași proces?

   Pentru că thread-urile aceluiași proces share-uiesc tabela sa de pagini, deci folosesc și au voie să acceseze aceleași adrese fizice.

### 16. Ce înseamnă mecanismul de copy-on-write?

   Mecanismul de copy-on-write este modalitatea prin care procesele copil primesc corespondența pagini - frames.
   Inițial, spațiul virtual de adrese al procesului copil point-ează către același spațiu fizic de adrese.
   Permisiunile sunt schimbate pe read only, și de abia la scriere, se duplică frame-urile, iar permisiunile sunt updatate.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Copy-on-write)
</details>

### 17. Dați exemple de situații în care are loc mecanismul de copy-on-write.

   În management-ul memoriei virtuale: la `fork()`.

   În biblioteci: clasa de string-uri din C++ (pentru standardul C++98)

### 18. Cu ce apel de sistem asociem copy-on-write?

   `fork()`

### 19. Când se duplică o pagină marcată copy-on-write?

   La scriere.

### 20. Cine detectează un acces de scriere într-o pagină marcată copy-on-write?

   Kernel-ul interceptează încercarea de a scrie pe o pagină marcată COW.
   Acesta alocă un nou frame (inițializat cu datele de pe pagina COW), face update la tabela de pagini cu noua corespondență și decrementează numărul de referințe către pagina COW care a cauzat intervenția sa.

<details>

   [Further Reading](https://lwn.net/Articles/849638/)
</details>

### 21. Ce înseamnă demand paging?

   Demand paging este mecanismul prin care o pagină virtuală primește un corespondent fizic (un frame) de abia la momentul folosirii ei (la acces).

### 22. În ce situație apare page fault fără a cauza segmentation fault?

   La primul acces al unei pagini ce nu a fost încă map-ată.
   Aceasta este rezervată (apare în VAS), dar nu are încă asociată un frame.

<details>

   [Further Reading](https://www.kernel.org/doc/gorman/html/understand/understand007.html)
</details>

### 23. Ce rol are spațiul de swap?

   Spațiul de swap are rolul de a reține paginile inactive din RAM, pentru a se putea face loc pentru altele noi, când memoria fizică este plină.

### 24. Când are loc swap in și swap out?

   Swap out are loc atunci când spațiul fizic devine insuficient.
   
   Swap in are loc la un swap out.

<details>

   [Further Reading](https://www.geeksforgeeks.org/swapping-in-operating-system/)
</details>

### 25. Care este rolul unui page fault? În ce condiții apare?

   Page fault-ul este o excepție ridicată de MMU, care apare atunci când un proces încearcă să acceseze o pagină fără a împlini anumite condiții preliminarii.
   Are rolul de a semnala shared memory, demand paging-ul sau un posibil acces invalid, împărțind-se în minor, major sau invalid.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Page_fault)
</details>

### 26. Care sunt secțiunile/zonele din spațiul de adrese al unui proces?

   .text

   .data

   .rodata

   .bss

   stiva

   heap

   biblioteci dinamice

   kernel

### 27. Ce secțiuni ale unui executabil se pot inspecta doar în timpul rulării?

   Stiva, heap-ul și bibliotecile dinamice.

### 28. Care sunt zonele writable din spațiul de adrese al unui proces?

   .data, .bss, stiva șî heap-ul

### 29. De ce sunt avantajoase bibliotecile dinamice pentru spațiul de adrese al unui proces?

   Pentru că sunt încărcate frame cu frame o singură dată, fiind folosite de mai multe procese în același timp.

### 30. Două procese sunt pornite din același executabil, ce zone din spațiul de adrese vor partaja?

   Bibliotecile dinamice, kernel space, .text și .rodata.

### 31. Se alocă un buffer a[100]. De ce a[105] NU va rezulta, în general, în Segmentation fault?

   Pentru că, în general, se alocă o pagină întreagă, iar elementul 105 se află în ea.

### 32. În ce situație a[300] rezultă în Segmentation fault?

   Atunci când lui a[100] îi este alocat spațiu la finalul unei pagini, iar a[300] ar intra în zona unei pagini nealocate încă.

### 33. Câte pagini ocupa char a[100] pentru sistem standard cu pagini de 4096 de octeți?

   Cel puțin una, cel mult două.

### 34. Câte pagini fizice alocă un apel mmap() care alocă 1MB? O pagină ocupă 4KB.

   0, `mmap()` nu alocă, ci rezervă spațiu virtual.

### 35. Câte pagini fizice alocă un apel calloc() care alocă 1MB? O pagină ocupă 4KB.

   Cel puțin 255, cel mult 256.

### 36. Ce înseamnă maparea unui fișier în memorie? De ce este avantajos să mapăm fișiere față de folosirea read/write?

   Fișierele map-ate în memorie prezintă un mecanism prin care procesele acesează fișiere prin incorporarea lor directă în VAS.
   Astfel, se reduce în mod semnificativ I/O data movement (și overhead-ul temporal), întrucât datele fișierului nu mai trebuie copiate în buffer-ele procesului așa cum s-ar întâmpla în cazul funcțiilor `read()` și `write()`.

<details>

   [Further Reading](https://www.ibm.com/docs/en/aix/7.2?topic=memory-understanding-mapping)
</details>

### 37. Câte page fault-uri se pot obține în cazul operației *a = b?

   4: unul pentru dereferențiere, unul pentru a, unul pentru operația în sine, dacă nu a fost map-ată pagina de .text aferentă și unul pentru b.

### 38. Care este numărul maxim de page fault-uri pe care îl poate genera expresia a = b + c?

   5: unul pentru a, unul pentru pentru operația de atribuire în sine, dacă nu a fost map-ată pagina de .text aferentă, unul pentru b, unul pentru c, unul pentru operația de adunare, dacă nu a fost map-ată pagina de .text aferentă.

### 39. Ce informații sunt reținute în stivă? Ce variabile C?

   variabilele locale funcției, adresa de return, stack-frame-ul, parametrii funcției.

### 40. Ce se întâmplă la faza de loading (încărcarea unui executabil în memorie și crearea unui proces)?

   În timpul fazei de loading, sistemul de operare citește executabilul de pe disk și map-ează părți din el în RAM.
   Tot acum se translatează adresele logice în adrese fizice.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Loader_(computing))
</details>

### 41. Ce înseamnă deturnarea fluxului de execuție a unui program (control flow hijack)? De ce este acest lucru relevant pentru un atacator?

   Cotrol-flow hijack este un tip de atac care urmărește schimbarea flow-ului normal al aplicației (injectarea IP/PC), și executarea de cod fraudulos prin vulnerabilități precum buffer overflow, index out of bounds, integer overflow sau string formatting.
   Este relevant pentru că scopul final al unui atac de tip control-flow hijack este preluarea controlului (pornirea unui nou shell, preluarea controlului unui server web, etc).

### 42. Ce înseamnă memory leak / memory disclosure? De ce este acest lucru relevant pentru un atacator?

   Memory disclosure este un tip de atac care urmărește descoperirea unor informații sensibile, neautorizate.
   Mecanisme hardware precum branch prediction sau speculative execution și vulnerabilități ca memory dump, memory reuse sau cross-use au condus la atacuri celebre (Spectre, Meltdown) care au afectat miliarde de procesoare.

### 43. De ce în general, preferăm o împărțire a spațiului virtual de adrese între kernel space și user space? Și nu un spațiu dedicat pentru kernel space?

   Pentru că kernel space-ul este la rândul lui specific fiecărui proces, iar separarea celor două spații asigură protecția memoriei și a hardware-ului împotriva software-ului malițios sau eronat.

<details>

   [Further Reading](https://unix.stackexchange.com/questions/472223/whats-the-use-of-having-a-kernel-part-in-the-virtual-memory-space-of-linux-proc)
</details>

### 44. Ce secvență de cod C va duce la o excepție de acces la memorie (de tip Segmentation fault)? De ce?

   Dereferențierea unui pointer null.
   Acesta este reprezentat ca o referință către adresa 0 din adress space, iar, prin urmare, MMU-ul indică ca nefăcând parte din memorie pagina care o conține.
   Pagina nu este, astfel, inclusă în VAS, iar o posibilă scriere sau citire pe aceasta rezultă într-un acces invalid și un SEGFAULT.

### 45. Cu ce diferă o funcție de o variabilă într-un executabil și/sau în cadrul spațiului de adrese al unui proces?

   O variabilă nu este executabilă, ea se află în zona .data, .rodata, .bss sau stivă.

   O funcție este un simbol ce poate fi executat.
   Ea se află în zona .text și are permisiuni de citire și execuție.

### 46. Două procese partajează o zonă de memorie. Cum se manifestă acest lucru în tabelele de pagini ale celor două procese?

   Cele două tabele de pagini vor avea anumite intrări comune, reprezentând adresele fzice pe care cele două procese le folosesc concomitent.

### 47. Putem avea mai multă memorie fizică decât dimensiunea maximă a spațiului virtual de adrese al unui proces? Dar invers?

   În general, memoria virtuală este mai mare sau egală cu cea fizică.
   Totuși, în anumite scenarii, memoria fizică poate fi mai mare decât cea virtuală (Intel 32-bit cu PAE).

<details>

   [Further Reading](https://stackoverflow.com/a/26424819)
</details>

### 48. Ce zone de memorie se aloca static? Dar dinamic?

   Stiva și heap-ul sunt alocate dinamic; text, data, rodata și bss sunt alocate static, înainte de începerea execuției.

### 49. Ce se întâmplă cu o variabilă modificată într-un proces copil din perspectiva procesului părinte?

   Procesele nu share-uiesc variabile, deci nu s-ar întâmpla nimic cu "variabila modificată într-un proces copil".

### 50. Ce reprezintă un loader? Ce rol are acesta?

   Loader-ul este o componentă a sistemului de operare care se ocupă cu încărcarea programelor și a bibliotecilor.
   Are rolul de a valida permisiuni, de map-are a executabilului de pe disk în memorie, de a copia argumentele în memoria virtuală, inițializează regiștrii (spre exemplu, `SP`) și face jump la entry-point (`_start`).
   Pe sistemele Unix, loader-ul este handler-ul apelului de sistem `execve()`.

### 51. La ce folosim apelul mprotect()? La ce mecanism de securitate putem face bypass folosind acest apel?

   Apelul `mprotect()` este folosit la schimbarea permisiunilor unuei zone de memorie.
   Cu ajutorul său se poate face bypass la `W^X`.

### 52. În ce zonă de memorie se află o variabilă globală, inițializată cu valoarea 0?

   .bss
