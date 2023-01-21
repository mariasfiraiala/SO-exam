1. Cum asigură sistemul de operare separația între procese?

1. Ce înseamnă mecanismul de memorie virtuală?

1. Ce reprezintă spațiul virtual de adrese al unui proces?

1. Cu ce diferă zona de date de zona de cod în spațiul virtual de adrese al unui proces?

1. Ce permisiuni are zona .text/.data/.rodata/.bss/de stivă/de heap?

1. Ce este paginarea memoriei?

1. Ce rol are tabela de pagini?

1. Ce este și ce rol are MMU (Memory Management Unit)?

1. Ce rol are TLB?

1. Care este ordinul de mărime al numărului de intrări ale TLB?

1. Ce conține o intrare în tabela de pagini?

1. Ce înseamnă tabelă de pagini multi-nivel (ierarhică)? De ce este utilă?

1. Când are loc un TLB miss?

1. De ce se golește TLB-ul (TLB flush) la schimbare de context?

1. De ce nu este nevoie de TLB flush la schimbarea de context între două thread-uri ale aceluiași proces?

1. Ce înseamnă mecanismul de copy-on-write?

1. Dați exemple de situații în care are loc mecanismul de copy-on-write.

1. Cu ce apel de sistem asociem copy-on-write?

1. Când se duplică o pagină marcată copy-on-write?

1. Cine detectează un acces de scriere într-o pagină marcată copy-on-write?

1. Ce înseamnă demand paging?

1. În ce situație apare page fault fără a cauza segmentation fault?

1. Ce rol are spațiul de swap?

1. Când are loc swap in și swap out?

1. Care este rolul unui page fault? În ce condiții apare?

1. Care sunt secțiunile/zonele din spațiul de adrese al unui proces?

1. Ce secțiuni ale unui executabil se pot inspecta doar în timpul rulării?

1. Care sunt zonele writable din spațiul de adrese al unui proces?

1. De ce sunt avantajoase bibliotecile dinamice pentru spațiul de adrese al unui proces?

1. Două procese sunt pornite din același executabil, ce zone din spațiul de adrese vor partaja?

1. Se alocă un buffer a[100]. De ce a[105] NU va rezulta, în general, în Segmentation fault?

1. În ce situație a[300] rezultă în Segmentation fault?

1. Câte pagini ocupa char a[100] pentru sistem standard cu pagini de 4096 de octeți?

1. Câte pagini fizice alocă un apel mmap() care alocă 1MB? O pagină ocupă 4KB.

1. Câte pagini fizice alocă un apel calloc() care alocă 1MB? O pagină ocupă 4KB.

1. Ce înseamnă maparea unui fișier în memorie? De ce este avantajos să mapăm fișiere față de folosirea read/write?

1. Câte page fault-uri se pot obține în cazul operației *a = b?

1. Care este numărul maxim de page fault-uri pe care îl poate genera expresia a = b + c?

1. Ce informații sunt reținute în stivă? Ce variabile C?

1. Ce se întâmplă la faza de loading (încărcarea unui executabil în memorie și crearea unui proces)?

1. Ce înseamnă deturnarea fluxului de execuție a unui program (control flow hijack)? De ce este acest lucru relevant pentru un atacator?

1. Ce înseamnă memory leak / memory disclosure? De ce este acest lucru relevant pentru un atacator?

1. De ce în general, preferăm o împărțire a spațiului virtual de adrese între kernel space și user space? Și nu un spațiu dedicat pentru kernel space?

1. Ce secvență de cod C va duce la o excepție de acces la memorie (de tip Segmentation fault)? De ce?

1. Cu ce diferă o funcție de o variabilă într-un executabil și/sau în cadrul spațiului de adrese al unui proces?

1. Două procese partajează o zonă de memorie. Cum se manifestă acest lucru în tabelele de pagini ale celor două procese?

1. Putem avea mai multă memorie fizică decât dimensiunea maximă a spațiului virtual de adrese al unui proces? Dar invers?

1. Ce zone de memorie se aloca static? Dar dinamic?

1. Ce se întâmplă cu o variabilă modificată într-un proces copil din perspectiva procesului părinte?

1. Ce reprezintă un loader? Ce rol are acesta?

1. La ce folosim apelul mprotect()? La ce mecanism de securitate putem face bypass folosind acest apel?

1. În ce zonă de memorie se află o variabilă globală, inițializată cu valoarea 0?
