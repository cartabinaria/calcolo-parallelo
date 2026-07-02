> **Nota**: Esercizi generati via LLM basandosi sul codice della [repository](https://github.com/mmarzolla/parallel-etudes) del Prof. Marzolla per avere esempi aggiuntivi in stile esame. Fare sempre riferimento al materiale ufficiale per la massima accuratezza.

---

# Esercizio 1 — Binary search generalizzata

La funzione seguente implementa una ricerca binaria generalizzata su un array ordinato `x[]` di lunghezza `n`.

```c
int omp_bsearch(const int *x, int n, int key)
{
    const int P = omp_get_max_threads();
    int cmp[P];
    int m[P];
    int start = 0, end = n-1;

#pragma omp parallel default(none) shared(start, end, cmp, m, x, key, P)
    {
        const int my_id = omp_get_thread_num();

        while (end-start > P) {
            m[my_id] = start + ((end-start)*my_id + P)/(P+1);
            cmp[my_id] = (x[m[my_id]] < key ? 1 : -1);

#pragma omp barrier

            if (my_id == 0 && cmp[my_id] < 0)
                end = m[my_id];
            else if (my_id == P-1 && cmp[my_id] > 0)
                start = m[my_id-1]+1;
            else if (my_id > 0 && cmp[my_id-1] > 0 && cmp[my_id] < 0) {
                start = m[my_id-1]+1;
                end = m[my_id];
            }
#pragma omp barrier
        }
    }
    ...
}
```

La funzione viene invocata su un array di lunghezza `n`, con `P = n` thread.

**Determinare `work`, `span` e `speedup` della funzione.**

---

# Esercizio 2 — Visita di un albero binario

La funzione seguente visita ricorsivamente un albero binario.

```c
void bt_visit(const bintree_node_t *root)
{
    if (root == NULL)
        return;
    else {
#pragma omp task firstprivate(root)
        bt_visit(root->left);
#pragma omp task firstprivate(root)
        bt_visit(root->right);

        printf("Thread %d visits %d\n", omp_get_thread_num(), root->val);
        sleep(1);
    }
}
```

La funzione viene chiamata inizialmente su un albero con `n` nodi.

Si assuma che:
- ogni nodo richieda tempo costante per essere visitato;
- i due sottoalberi di ogni nodo possano essere elaborati in parallelo;
- la radice della visita sia avviata da un solo thread in una regione parallela.

**Determinare `work`, `span` e `speedup` nei seguenti casi:**
1. albero perfettamente bilanciato;
2. albero degenere (a lista);
3. albero binario completo di altezza `h`.

---

# Esercizio 3 — Somma di matrici

La funzione seguente somma due matrici quadrate di lato `n`.

```c
void matsum(const float *p, const float *q, float *r, int n)
{
#pragma omp parallel for default(none) shared(p,q,r,n) schedule(static)
    for (int i=0; i<n; i++) {
        for (int j=0; j<n; j++) {
            r[i*n + j] = p[i*n + j] + q[i*n + j];
        }
    }
}
```

**Determinare `work`, `span` e `speedup` assumendo `P = n` processori.**

---

# Esercizio 4 — Prodotto matrice-vettore triangolare

La funzione seguente calcola il prodotto tra una matrice triangolare superiore `A` e un vettore `b`.

```c
void tri_gemv(const float *A, const float *b, float *c, int n)
{
    for (int i=0; i<n; i++) {
        c[i] = 0;
        for (int j=i; j<n; j++) {
            c[i] += A[i*n + j] * b[j];
        }
    }
}
```

La funzione viene parallelizzata idealmente assegnando in parallelo le righe ai processori.

**Determinare `work`, `span` e `speedup` con `P = n` processori.**

---

# Esercizio 5 — Odd-even sort

L’algoritmo seguente ordina un array `v[]` di lunghezza `n`.

```c
void odd_even_sort( int* v, int n )
{
#pragma omp parallel default(none) shared(n,v)
    for (int phase = 0; phase < n; phase++) {
        if ( phase % 2 == 0 ) {
#pragma omp for
            for (int i=0; i<n-1; i += 2 ) {
                cmp_and_swap( &v[i], &v[i+1] );
            }
        } else {
#pragma omp for
            for (int i=1; i<n-1; i += 2 ) {
                cmp_and_swap( &v[i], &v[i+1] );
            }
        }
    }
}
```

**Determinare `work`, `span` e `speedup` al variare di `n`, assumendo che ogni confronto-swap costi tempo costante.**

---

# Esercizio 6 — Traversal di un albero binario bilanciato

Considera una versione idealizzata in cui ogni nodo dell’albero genera due task figli, uno per il sottoalbero sinistro e uno per il destro.

```c
void visit(const node_t *x)
{
    if (x == NULL)
        return;

#pragma omp task
    visit(x->left);

#pragma omp task
    visit(x->right);

    process(x);
}
```

Supponi che:
- `process(x)` costi `O(1)`;
- l’albero sia perfettamente bilanciato e abbia `n` nodi.

**Determinare `work`, `span` e `speedup`.**

---

# Esercizio 7 — Ricerca lineare parallela

La funzione seguente cerca un elemento `key` in un array non ordinato.

```c
int linear_search(const int *x, int n, int key)
{
    int found = -1;
#pragma omp parallel for reduction(max:found)
    for (int i=0; i<n; i++) {
        if (x[i] == key)
            found = i;
    }
    return found;
}
```

**Determinare `work`, `span` e `speedup` assumendo `P = n` processori.**

---

# Esercizio 8 — Dot product

La funzione seguente calcola il prodotto scalare di due vettori.

```c
float dot(const float *x, const float *y, int n)
{
    float sum = 0;
#pragma omp parallel for reduction(+:sum)
    for (int i=0; i<n; i++) {
        sum += x[i] * y[i];
    }
    return sum;
}
```

**Determinare `work`, `span` e `speedup` con `P = n`.**

---

# Esercizio 9 — Sieve of Eratosthenes

La funzione seguente implementa il crivello di Eratostene.

```c
void sieve(int *is_prime, int n)
{
    for (int p = 2; p*p <= n; p++) {
        if (is_prime[p]) {
#pragma omp parallel for
            for (int k = p*p; k <= n; k += p) {
                is_prime[k] = 0;
            }
        }
    }
}
```

**Determinare `work`, `span` e `speedup` in funzione di `n`, assumendo parallelismo ideale su ogni iterazione interna.**

---

# Esercizio 10 — Merge sort con task

La funzione seguente ordina ricorsivamente un array.

```c
void mergesort_rec(int* v, int i, int j, int* tmp)
{
    if (i >= j)
        return;
    else {
        int m = (i+j)/2;

#pragma omp task shared(v,i,m,tmp)
        mergesort_rec(v, i, m, tmp);

#pragma omp task shared(v,j,m,tmp)
        mergesort_rec(v, m+1, j, tmp);

#pragma omp taskwait
        merge(v, i, m, j, tmp);
    }
}
```

La funzione viene avviata da un singolo thread su un array di lunghezza `n`.

**Determinare `work`, `span` e `speedup` assumendo `P = n`.**

---

# Esercizio 11 — Monete Monte Carlo π

La funzione seguente stima il valore di π tramite simulazione Monte Carlo.

```c
double estimate_pi(long n)
{
    long inside = 0;
#pragma omp parallel for reduction(+:inside)
    for (long i=0; i<n; i++) {
        double x = rand01();
        double y = rand01();
        if (x*x + y*y <= 1.0)
            inside++;
    }
    return 4.0 * inside / n;
}
```

**Determinare `work`, `span` e `speedup` assumendo `P = n`.**

---

# Esercizio 12 — Inclusive scan

La funzione seguente calcola la scan inclusiva di un vettore.

```c
void inclusive_scan(int *x, int n)
{
    for (int offset = 1; offset < n; offset *= 2) {
#pragma omp parallel for
        for (int i = offset; i < n; i++) {
            x[i] += x[i-offset];
        }
    }
}
```

**Determinare `work`, `span` e `speedup` in funzione di `n`.**

---

# Esercizio 13 — Matrix-vector multiply triangolare

La funzione seguente calcola `c = A b`, dove `A` è triangolare superiore.

```c
void tri_gemv2(const float *A, const float *b, float *c, int n)
{
    for (int i=0; i<n; i++) {
        c[i] = 0;
#pragma omp parallel for reduction(+:c[i])
        for (int j=i; j<n; j++) {
            c[i] += A[i*n + j] * b[j];
        }
    }
}
```

**Determinare `work`, `span` e `speedup` assumendo parallelismo perfetto sul ciclo interno e `P = n`.**

---

# Esercizio 14 — Visita di un albero binario con sleep

La funzione seguente visita ogni nodo di un albero binario e introduce un ritardo fisso.

```c
void bt_visit(const bintree_node_t *root)
{
    if (root == NULL)
        return;

#pragma omp task firstprivate(root)
    bt_visit(root->left);

#pragma omp task firstprivate(root)
    bt_visit(root->right);

    sleep(1);
}
```

Se l’albero ha `n` nodi, **determinare `work`, `span` e `speedup`** nei casi:
- albero bilanciato;
- albero degenerato.

---

# Esercizio 15 — Array rank / generalized search

La funzione seguente suddivide un intervallo in `P+1` sottointervalli e valuta in parallelo una condizione su punti campione.

```c
int generalized_search(const int *x, int n, int key)
{
    int start = 0, end = n-1;
    const int P = omp_get_max_threads();
    int cmp[P];
    int m[P];

#pragma omp parallel default(none) shared(start,end,cmp,m,x,key,P)
    {
        int id = omp_get_thread_num();
        while (end-start > P) {
            m[id] = start + ((end-start)*id + P)/(P+1);
            cmp[id] = (x[m[id]] < key);
#pragma omp barrier
#pragma omp barrier
        }
    }
    return -1;
}
```

**Determinare `work`, `span` e `speedup` se ogni iterazione del ciclo `while` dimezza circa la dimensione del sottoproblema.**

---

Se vuoi, nel prossimo messaggio posso anche fartene **altri 15 più tosti**, oppure posso farteli **ordinati per difficoltà**:
- facili
- medi
- difficili

Così li puoi risolvere uno alla volta.