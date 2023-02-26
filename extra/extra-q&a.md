# Extra Q&A

### 1. Ce înseamnă livelock? Cum diferă de un deadlock?

   Livelock-ul este un caz aparte de starvation: două thread-uri doresc să acceseze o resursă comună, și se întreabă reciproc dacă o folosesc.
   Cum amândouă doresc să o folosească, își pasează în mod indefinit accesul la resursa respectivă, fiecare din ele lăsând thread-ul celălalt să o acceseze primul.
   O situație din lumea reală este cea în care două persoane se sună reciproc simultan, iar linia apare ca fiind ocupată pentru amândouă.

   Livelock-ul diferă de deadlock prin faptul că thread-urile nu rămân în starea BLOCKED, ci își răspund reciproc (schibându-și starea), în mod indefinit.

<details>

   [Further Reading](https://stackoverflow.com/a/35129823)
</details>

### 2. Cum se implementează pe un sistem single-core un spinlock? Cum se implementează pe un sistem multi-core un spinlock?

   Spinlock-urile nu pot fi implementate pe un sistem single-core, întrucât pentru ca un thread să fie eliberat din busy wainting-ul spinlock-ului, este nevoie ca un alt thread, aflat în starea RUNNING, să îl scoată din rularea în gol.
   Acest lucru este imposibil pe un sistem single-core, pentru că pe acesta rulează un singur thread la un moment de timp dat. 

<details>

   [Further Reading](https://codex.cs.yale.edu/avi/os-book/OS9/practice-exer-dir/5-web.pdf)
</details>

   Pe un sistem multi-core, spinlock-ul este implementat printr-un compare and exchange atomic:

   ```C
   eax = 1;
   while( xchg(eax, lock_address) != 0 );
   // now I have the lock
   ... code ...
   *lock_address = 0; // release the lock
   ```

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Spinlock)
</details>

### 3. Ce este un apel thread-safe? Ce este un apel reentrant?

   Un apel thread-safe este un apel de funcție ce poate fi chemat de mai multe thread-uri simultan, fără a rezulta în race condition.

   Un apel reentrant este un apel de funcție ce poate fi preemptat la orice moment de timp și rechemat de același thread, rezultând într-un singur comportament, bine definit.

<details>

   [Further Reading](https://github.com/open-education-hub/operating-systems/blob/master/content/chapters/compute/lecture/slides/thread-safety.md)
</details>

### 4. Ce este fragmentarea internă a memoriei?

   Fragmenatarea internă a memoriei se referă la spațiul rămas nefolosit dintre dimensiunea cerută la alocare și dimensiunea block-ului.
   Dacă zona de memorie alocată este mai mică decât block-ul din care face parte, zona rămasă "liberă" poartă numele de fragmentare internă.

   Această problemă este specifică cazului în care memoria este împărțită în block-uri de mărime fixă ("mounted-sized blocks").
   Partiționarea dinamică rezolvă această problemă.

<details>

   [Further Reading](https://www.geeksforgeeks.org/variable-or-dynamic-partitioning-in-operating-system/)
</details>

### 5. Ce este fragmentarea externă a memoriei?

   Fragmentarea externă a memoriei se referă la situația în care cererea pentru memorie a unui proces poate fi satisfăcută ca dimensiune, dar nu și ca poziție, deoarece spațiul cerut nu este contiguu.

   Această problemă ține de alocator și de algoritmul de alocare, spre exemplu first-fit sau best-fit conduc la asfel de situații.

<details>

   [Further Reading](https://www.geeksforgeeks.org/difference-between-internal-and-external-fragmentation/)
</details>

### 6. Ce înseamnă operația de striping a unui executabil?

   Stripping-ul este modalitatea prin care se scot din executabil informațiile de debug.
   Astfel, se ajunge la binare mai mici și performanță mai bună.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Strip_(Unix))
</details>

### 7. Ce utilitare cunoașteți pentru analiză dinamică și ce utilitare cunoașteți pentru analiză statică?

   Analiză dinamică: `gdb`, `strace`, `ltrace`, `pmap`, `lsof`

   Analiză statică: `nm`, `ldd`, `objdump`

### 8. Ce înseamnă analiză statică și ce înseamnă analiză dinamică?

   Analiza statică se face asupra unor fișiere (executabile, programe) care nu se află în execuție.

   Analiza dinamică se face asupra proceselor, asupra a ceva care se află în execuție (care a fost lansat).

### 9. Ce înseamnă Stack Guard / Stack Smashing Protection (SSP)?

   Stack Guard-ul funcționează prin introducerea unor canare (valori, nu neapărat arbitrare), între valorile reținute pe stivă și adresă de retur.
   Astfel, un atac de tipul buffer overflow poate fi interceptat, iar programul este oprit, evitând schimbarea flow-ului programului.

<details>

   [Further Reading](https://www.redhat.com/en/blog/security-technologies-stack-smashing-protection-stackguard)
</details>

### 10. Ce efect are ASLR (Address Space Layout Randomization)?

   ASLR este modalitatea prin care spațiul de adrese al procesului este randomizat.
   Secțiunile critice, precum baza executabilului, stiva, heap-ul și bibliotecile primesc la fiacare rulare adrese diferite, pentru a se evita atacuri precum cel de code reuse.
   Spre exemplu, dacă atacatorul dorește să sară la adresa unei funcții malițioase (suprascriind adresa de retur), acest lucru nu mai este posibil, pentru că adresa target-ului se modifică constant (la fiecare rulare).

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Address_space_layout_randomization)
</details>

### 11. Ce efect are PIE (Position Independent Executable)?

   PIE este un tip de executabil care este încărcat la adrese diferite pentru fiecare rulare.
   Astfel, scade probabilitatea aplicării unui atac de tip ROP.

<details>

   [Further Reading](https://stackoverflow.com/a/38211729)
</details>

### 12. Ce înseamnă că o secvență de cod este PIC (Position Independent Code)?

    PIC are același rol ca PIE, singura diferență fiind faptul că simbolurile pot fi inversate.

<details>

   [Further Reading](https://stackoverflow.com/a/28132120)
</details>

### 13. Ce este un code pointer? De ce este interesant din perspectiva securității memoriei?

   Code pointer-ul este un tip de pointer ce stochează o adresă din zona de text, fie aceasta o adresă de retur, sau adresa unei funcții.
   Este important ca acesta să fie bine protejat (prin Code Pointer Integrity - CPI), deoarece, pot conduce foarte ușor la schimbarea fluxului de execuție și atacuri ROP.

<details>

   [Further Reading](https://dslab.epfl.ch/pubs/cpi.pdf)
</details>

### 14. Ce este un shellcode?

   Shellcode se referă la secvența de instrucțiuni folosită în cadrul unui atac de tip code injection.
   În general, scopul principal al unui astfel de atac este pornirea unui nou shell (`exec("/bin/sh")`), însă codul mașină al shellcode-ului poate conține orice, păstrând o dimensiune redusă și folosind apeluri de sistem pentru a prelua controlul sistemului pe care îl target-ează.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Shellcode)
</details>

### 15. Ce înseamnă code reuse din perspectiva securității memoriei?

   Code reuse este un tip de atac ce se bazează pe sărirea la adrese din spațiul procesului, schimbând, în același timp, flow-ul normal al programului.
   Se numește "code reuse" deoarece se folosesc adrese deja existente.

### 16. Ce înseamnă shell injection din perspectiva securității memoriei?

   Un atac de tipul shell injection începe prin a exploata o vulnerabilitate pentru a câștiga controlul asupra IP.
   În acest moment, IP-ul va executa shell code-ul injectat.
   Injectarea se realizează prin trimiterea shell code-ului în datele primite prin rețea și scrierea acestuia într-un fișier ce va urma a fi citit de proces.
   O altă modalitate de salvare a shell code-ului este în parametri din linia de comandă sau variabile de mediu.

### 17. Ce este un hard link?

   Un hard link este o structură (denumită și dentry) ce conține un inode number și numele fișierului referit prin inode.

### 18. Ce este un link simbolic/symlink?

   Un symlink este un inode al cărui conținut este o cale absolută.

### 19. Care este diferența dintre un link simbolic și un hard link?

   * nu pot exista hard link-uri către directoare, însă symlink-uri da

   * symlink-urile pot referi fișiere de pe altă partiție, hard link-urile nu

   * symlink-urile pot funcționa ca un rename pentru fișiere

### 20. De ce numele unui fișier nu se găsește în inode?

   Pentru că pot exista situații în care dorim redenumirea fișierelor, iar acest lucru devine mult mai simplu prin modificarea dentry-ului, decât prin înlocuirea numelui în FCB.
   De asemenea, dentry-ului sunt utile și în cazul în care se dorește duplicarea fișierului în locuri diferite (mecanismul de copiere).

### 21. Ce se întâmplă în cazul formatării unei partiții?

   Operația de formatare rezultă într-un sistem de fișiere, ocupându-se de crearea superblock-ului, a celorlalte zone și a inode-ului rădăcină.'
   O formatare raw rezultă în zeroizarea partiției.

<details>

   [Further Reading](https://docs.google.com/document/d/1bEXn4-6WCUXPvswp-bDXb_4cAePqxjJFgZszRKzYMDY/edit#heading=h.x92ryvox1zux)
</details>

### 22. Ce se întâmplă cu sistemul de fișiere în cazul folosirii cu succes a comenzii rm?

   * unlink

   * se șterge dentry-ul din data block-urile directorului

   * se decrementează numărul de hard link-uri către fișier, dacă se ajunge la 0 se eliberează FCB-ul

<details>

   [Further Reading](https://linux.die.net/man/2/unlink)
</details>

### 23. Care este un avantaj al folosirii hard link-urilor și un avantaj al folosirii link-urilor simbolice?

   Hard link: ușurință în mutarea, copierea și ștergerea fișierelor

   Symlink: putem referi fișiere de pe alte partiții

### 24. Ce efect are comanda mv  /path/to/a.dat /new/path/to/b.dat în sistemul de fișiere?

   Se creează un nou hard link pentru noua cale și se șterge vechiul hard link.

### 25. Care tipuri de fișiere nu au blocuri de date?

   Block-urile, char-urile, socket-urile și FIFO-urile nu au blocuri de date

### 26. Ce conțin blocurile de date ale unui director?

   dentry-urile fișierelor "copil".

### 27. Ce este o întrerupere? Când este livrată o întrerupere?

   Întreruperile sunt semnale trimise către procesor, cerându-se ca acesta să se întrerupă și să trateze cererea care a fost livrată.
   Dacă această cerere este acceptată, procesorul își suspendă activitatea curentă, își salvează starea și execută interrupt handler-ul.
   Dacă întreruperea nu a cerut terminarea procesului, după rularea handler-ului, procesorul continuă intrucțiunile pe care le pusese pe pauză.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Interrupt)
</details>

### 28. Cu ce diferă port-mapped I/O de memory-mapped I/O?

   PMIO deține un spațiu de adrese separat și este accesat folosind instrucțiuni specifice (și port-uri digitale).

   MMIO folosește același spațiu de adrese ca procesul și aceleași opcodes.

<details>

   [Further Reading](https://stackoverflow.com/a/15372318)
</details>

### 29. Care este rolul DMA-ului (Direct Memory Access)?

   DMA are rolul de a transfera date fără implicarea procesorului.
   Este folosită pentru a transfera date de la și către dispozitive I/O, fiind necesar, totuși un DMA controller care să se ocupe de schimbul de informație.

   DMA-ul a apărut în contextul în care procesorul era total ocupat cu operațiile de read/write pe dispozitive I/O.
   Direct Memory Access permite ca CPU-ul să inițieze transferul, să facă alte operații în timpul acestuia și să fie notificat printr-o întrerupere trimisă de DMAC că acesta s-a încheiat.

<details>

   [Further Reading](https://en.wikipedia.org/wiki/Direct_memory_access)
</details>

### 30. Când are sens să folosim polling în loc de întreruperi?

   Este de preferat să folosim polling atunci când evenimentele pe care le așteptăm sunt sincrone, nu foarte urgente și se întâmplă des.

<details>

   [Further Reading](https://stackoverflow.com/a/3072959)
</details>

### 31. Ce rol are mecanismul de TCP offload engine?

   TCP offload engine se ocupă cu mutarea unor task-uri (specifice stivei TCP/IP) precum connection establishment, checksum, sliding window sau connection termination, din sarcina CPU-ului în atribuțiile altor device-uri hardware (precum network controller-ul).

<details>

   [Further Reading](https://en.wikipedia.org/wiki/TCP_offload_engine)
</details>

### 32. Care este sursa primară pentru care un apel send() pe un socket TCP se blochează?

   `send()` se blochează pe un socket TCP dacă receive buffer-ul e plin.
   Spre deosebire de UDP, un socket de tip TCP nu așteaptă până la golirea totală a buffer-ului, ci poate trimite chiar și un singur byte, dacă s-a făcut loc pentru acesta.

### 33. Care este sursa primară pentru care un apel send() pe un socket UDP se blochează?

   `send()` se blochează pe un socket UDP dacă receive buffer-ul e plin.
   Totuși, un socket UDP va aștepta să se elibereze tot buffer-ul, pentru a putea trimite toată informația o dată.

<details>

   [Further Reading](https://stackoverflow.com/a/5509956)
</details>

### 34. Ce garanții ni se oferă în momentul în care apelul send() se întoarce în user space?

   Că informația a fost copiată într-un buffer de send.
