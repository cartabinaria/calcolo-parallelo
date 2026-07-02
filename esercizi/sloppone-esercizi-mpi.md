> **Nota**: Esercizi generati via LLM basandosi sul codice della [repository](https://github.com/mmarzolla/parallel-etudes) del Prof. Marzolla per avere esempi aggiuntivi in stile esame. Fare sempre riferimento al materiale ufficiale per la massima accuratezza.

---

# Esercizio 1 — Istogramma

È dato un array `v[N]` di lunghezza `N` contenente valori interi appartenenti all’insieme `{0, ..., 99}`; l’array non è ordinato. Si vuole calcolare l’istogramma `h[100]` del numero di occorrenze di ciascun valore in `v[]`. In altre parole, per ogni `k = 0, ..., 99`, `h[k]` deve essere il numero di volte in cui il valore `k` compare in `v[]`. Descrivere un algoritmo in grado di risolvere il problema su architettura a memoria distribuita usando MPI.

Si assuma che:

- la lunghezza `N` di `v[]` sia multipla del numero `P` di processi MPI, e sia molto maggiore di `P`;
- il valore di `N` sia inizialmente noto a tutti i processi;
- il contenuto di `v[]` sia inizialmente noto solo al processo 0;
- al termine dell’esecuzione il contenuto dell’istogramma `h[]` debba essere noto al processo 0.

Si descriva nel modo più preciso possibile il comportamento di ciascun processo MPI. **Non è richiesto scrivere codice**: è possibile usare pseudocodice e/o linguaggio naturale, purché la spiegazione sia precisa. In particolare, si indichino quali funzioni MPI si userebbero nei vari passi, e quali buffer di memoria è necessario usare/allocare.

---

# Esercizio 2 — Prodotto matrice-vettore

È data una matrice quadrata `A[N][N]` e un vettore `b[N]`, entrambi di tipo `float`. Si vuole calcolare il prodotto matrice-vettore `c[N] = A * b`, cioè per ogni `i = 0, ..., N-1` vale `c[i] = sum_j A[i][j] * b[j]`. Descrivere un programma MPI in grado di eseguire tale calcolo su un sistema a memoria distribuita.

Si assuma che:

- la dimensione `N` della matrice `A` e del vettore `b` sia inizialmente nota a tutti i processi;
- `N` sia multiplo del numero di processi MPI `P`;
- il contenuto di `A[]` e `b[]` sia inizialmente noto solo al processo 0;
- al termine della computazione, il risultato `c[]` debba essere noto al processo 0.

Si descriva in modo preciso come distribuire il lavoro tra i processi, quale parte della matrice viene assegnata a ciascun processo, quali comunicazioni sono necessarie e come si ricompone il risultato finale.

---

# Esercizio 3 — Somma di matrici

Sono date due matrici quadrate `A[N][N]` e `B[N][N]`, entrambe di tipo `double`. Si vuole calcolare una matrice `C[N][N]` tale che `C[i][j] = A[i][j] + B[i][j]` per ogni `i, j`. Si chiede di descrivere un algoritmo MPI per eseguire tale operazione su un’architettura a memoria distribuita.

Si assuma che:

- `N` sia multiplo di `P` e molto maggiore di `P`;
- il valore di `N` sia inizialmente noto a tutti i processi;
- `A[]` e `B[]` siano inizialmente noti solo al processo 0;
- `C[]` debba essere noto al processo 0 al termine dell’esecuzione.

Si indichi con precisione come vengono suddivise le righe della matrice, quali buffer locali servono a ciascun processo e quali primitive MPI sono più adatte per distribuire i dati e raccogliere il risultato.

---

# Esercizio 4 — Somma di vettori

Sono dati due vettori `x[N]` e `y[N]` di tipo `int`. Si vuole calcolare un vettore `z[N]` tale che `z[i] = x[i] + y[i]` per ogni `i`. Si vuole descrivere un algoritmo MPI in grado di risolvere il problema in modo efficiente.

Si assuma che:

- `N` sia molto maggiore del numero di processi `P`;
- `N` sia multiplo di `P`;
- `x[]` e `y[]` siano inizialmente noti solo al processo 0;
- `z[]` debba essere noto al processo 0 al termine della computazione.

Si descriva il comportamento dei processi MPI nei tre passi classici: distribuzione iniziale dei dati, calcolo locale e raccolta finale del risultato.

---

# Esercizio 5 — Media globale

È dato un array `v[N]` di numeri reali. Si vuole calcolare la media aritmetica dei valori contenuti in `v[]`. Descrivere un algoritmo MPI per risolvere il problema su una macchina a memoria distribuita.

Si assuma che:

- `v[]` sia inizialmente noto solo al processo 0;
- il numero `N` di elementi sia noto a tutti i processi;
- il risultato finale debba essere noto al processo 0.

Si descriva con precisione come ciascun processo contribuisce al calcolo, e quali primitive MPI sono più appropriate per combinare i risultati locali.

---

# Esercizio 6 — Prodotto scalare

Sono dati due vettori `x[N]` e `y[N]` di tipo `double`. Si vuole calcolare il prodotto scalare `s = sum_i x[i] * y[i]`. Si descriva un programma MPI per eseguire il calcolo in parallelo.

Si assuma che:

- `N` sia noto a tutti i processi ed eventualmente multiplo di `P`;
- `x[]` e `y[]` siano inizialmente noti solo al processo 0;
- il valore finale di `s` debba essere noto al processo 0.

Si spieghi quali dati devono essere distribuiti, come si effettua il calcolo locale e quale operazione di comunicazione finale si usa per ottenere il risultato globale.

---

# Esercizio 7 — Ricerca del massimo

È dato un array `v[N]` di interi. Si vuole trovare il valore massimo contenuto in `v[]` e l’indice della sua prima occorrenza. Descrivere un algoritmo MPI che risolva il problema.

Si assuma che:

- `v[]` sia inizialmente noto solo al processo 0;
- `N` sia molto maggiore del numero di processi `P`;
- il risultato finale debba essere noto al processo 0.

Si indichi come ciascun processo calcola il proprio massimo locale, come si combinano i massimi parziali e come si gestisce il caso di pari valori presenti in più processi.

---

# Esercizio 8 — Verifica di ordinamento

È dato un array `v[N]` di interi. Si vuole verificare se l’array è ordinato in modo non decrescente. Descrivere un algoritmo MPI che risolva il problema su un sistema a memoria distribuita.

Si assuma che:

- `v[]` sia inizialmente noto solo al processo 0;
- `N` sia multiplo di `P`;
- il risultato finale debba essere un valore booleano noto al processo 0.

Si spieghi come distribuire l’array, quali controlli effettuare localmente e quale informazione di bordo è necessaria per decidere correttamente il risultato globale.

---

# Esercizio 9 — Trasposizione di matrice

È data una matrice quadrata `A[N][N]` di tipo `float`. Si vuole calcolare la trasposta `B`, cioè `B[j][i] = A[i][j]`. Descrivere un algoritmo MPI per eseguire la trasposizione.

Si assuma che:

- `A[]` sia inizialmente noto solo al processo 0;
- `N` sia multiplo di `P`;
- la matrice trasposta `B[]` debba essere disponibile al processo 0 al termine.

Si indichi una strategia per distribuire i blocchi di matrice, minimizzare le comunicazioni e ricostruire il risultato finale.

---

# Esercizio 10 — Somma di blocchi

È data una matrice `A[N][N]`. Si vuole costruire una matrice più piccola `S[M][M]`, con `M = N/B`, in cui ogni elemento `S[i][j]` contiene la somma degli elementi del blocco `B x B` della matrice `A` corrispondente alla posizione `(i, j)`. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- `N` sia multiplo di `B` e di `P`;
- `A[]` sia inizialmente noto solo al processo 0;
- `S[]` debba essere disponibile al processo 0 al termine.

Si spieghi come partizionare il lavoro sui processi e come raccogliere i risultati parziali.

---

# Esercizio 11 — Filtro 1D con halo exchange

È dato un array `x[N]` di numeri reali. Si vuole calcolare un array `y[N]` tale che, per ogni indice interno `i`, `y[i] = (x[i-1] + x[i] + x[i+1]) / 3`. Gli elementi di bordo possono essere gestiti separatamente. Si chiede di descrivere un algoritmo MPI per eseguire il calcolo.

Si assuma che:

- `x[]` sia inizialmente noto solo al processo 0;
- `N` sia multiplo di `P`;
- il risultato `y[]` debba essere noto al processo 0.

Si descriva con precisione quali elementi di confine devono essere scambiati tra processi adiacenti, quali buffer servono e quando avviene lo scambio.

---

# Esercizio 12 — Calcolo dell’istogramma su immagine

È data un’immagine in scala di grigi rappresentata da una matrice `I[H][W]` con valori interi compresi tra `0` e `255`. Si vuole calcolare l’istogramma dei livelli di grigio, cioè un vettore `h[256]` in cui `h[k]` contiene il numero di pixel di intensità `k`. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- l’immagine sia inizialmente nota solo al processo 0;
- `H * W` sia molto maggiore di `P`;
- il risultato finale debba essere noto al processo 0.

Si spieghi come distribuire l’immagine, come costruire istogrammi locali e come combinarli correttamente.

---

# Esercizio 13 — Selezione con soglia

È dato un array `v[N]` di numeri reali e una soglia `T`. Si vuole costruire un array `w[]` contenente tutti e soli gli elementi di `v[]` maggiori di `T`, mantenendo l’ordine relativo di apparizione. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- `v[]` sia inizialmente noto solo al processo 0;
- il numero di elementi selezionati non sia noto a priori;
- il risultato finale debba essere noto al processo 0.

Si descriva come i processi possano produrre porzioni locali dell’output e come si possa ottenere il concatenamento corretto dei risultati.

---

# Esercizio 14 — Somma di tutti gli elementi maggiori di soglia

È dato un array `v[N]` di interi e una soglia `T`. Si vuole calcolare la somma di tutti gli elementi di `v[]` maggiori di `T`. Descrivere un algoritmo MPI per eseguire il calcolo.

Si assuma che:

- `v[]` sia noto solo al processo 0;
- `N` sia molto grande;
- il risultato finale sia uno scalare noto al processo 0.

Si spieghi come effettuare il calcolo locale su ogni processo e quale operazione MPI usare per combinare i risultati parziali.

---

# Esercizio 15 — Ricerca di un elemento

È dato un array `v[N]` di interi non ordinati e un valore `key`. Si vuole verificare se `key` è presente nell’array e, in caso affermativo, determinare la prima posizione in cui appare. Descrivere un algoritmo MPI che risolva il problema.

Si assuma che:

- `v[]` sia inizialmente noto solo al processo 0;
- `N` sia molto maggiore di `P`;
- il risultato finale debba essere noto al processo 0.

Si descriva come distribuire l’array, come combinare eventuali risposte locali positive e come gestire il fatto che la chiave possa comparire in più blocchi.

---

# Esercizio 16 — Somma ripetuta di vettori

Sono dati `K` vettori `v_0, v_1, ..., v_{K-1}`, ciascuno di lunghezza `N`. Si vuole calcolare il vettore somma `s[N]` definito da `s[i] = sum_{t=0}^{K-1} v_t[i]`. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- tutti i vettori siano inizialmente noti solo al processo 0;
- `K` e `N` siano noti a tutti i processi;
- il risultato finale debba essere noto al processo 0.

Si spieghi come partizionare il lavoro sia rispetto ai vettori sia rispetto agli elementi, e come evitare squilibri tra processi.

---

# Esercizio 17 — Conteggio di parole distinte

È dato un testo molto lungo, già separato in parole, e si vuole costruire una tabella che associ a ogni parola distinta il numero di occorrenze. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- il testo sia inizialmente noto solo al processo 0;
- il numero di parole distinte non sia noto a priori;
- il risultato finale debba essere noto al processo 0.

Si descriva come rappresentare localmente le frequenze, come gestire la fusione di risultati parziali e quali difficoltà introduce l’uso di strutture dati dinamiche.

---

# Esercizio 18 — Somma della diagonale principale

È data una matrice quadrata `A[N][N]`. Si vuole calcolare la somma degli elementi della diagonale principale. Descrivere un algoritmo MPI in grado di risolvere il problema.

Si assuma che:

- `A[]` sia inizialmente noto solo al processo 0;
- `N` sia molto grande;
- il risultato finale debba essere noto al processo 0.

Si spieghi se conviene distribuire l’intera matrice oppure solo le informazioni necessarie, e motivare la scelta.

---

# Esercizio 19 — Controllo di soglia su matrice

È data una matrice `A[N][N]` di interi e una soglia `T`. Si vuole verificare se esiste almeno un elemento della matrice maggiore di `T`. Descrivere un algoritmo MPI per eseguire il controllo.

Si assuma che:

- `A[]` sia inizialmente noto solo al processo 0;
- `N` sia multiplo di `P`;
- il risultato finale debba essere un booleano noto al processo 0.

Si indichi come i processi effettuano il controllo locale e come si combina l’esito globale.

---

# Esercizio 20 — Traversal distribuito di un albero

È dato un albero binario molto grande, i cui nodi contengono valori interi. Si vuole contare quanti nodi contengono un valore maggiore di una soglia `T`. Descrivere un algoritmo MPI per risolvere il problema.

Si assuma che:

- l’albero sia inizialmente noto solo al processo 0;
- la struttura sia abbastanza grande da giustificare una distribuzione del lavoro;
- il risultato finale debba essere noto al processo 0.

Si spieghi quali difficoltà introduce una struttura non lineare come un albero e come si potrebbero assegnare sottoalberi o porzioni del lavoro ai processi.



