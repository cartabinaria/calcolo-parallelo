> **Nota**: Esercizi generati via LLM basandosi sul codice della [repository](https://github.com/mmarzolla/parallel-etudes) del Prof. Marzolla per avere esempi aggiuntivi in stile esame. Fare sempre riferimento al materiale ufficiale per la massima accuratezza.

---

# 1) Embarrassingly parallel
```c
#define N ...
double x[N], y[N], z[N];

/* ... */

for (int i=0; i<N; i++) {
    z[i] = x[i] + y[i];
}
```

**Consegna:**  
Discutere se il frammento è parallelizzabile. In caso positivo, proporre una strategia OpenMP semplice ed evidenziare eventuali problemi di memoria o scalabilità.

---

# 2) Reduzione
```c
#define N ...
double v[N];
double sum = 0.0;

/* ... */

for (int i=0; i<N; i++) {
    sum += v[i];
}
```

**Consegna:**  
Discutere se il frammento è parallelizzabile. In caso positivo, indicare se è necessario usare una `reduction` e motivare la scelta.

---

# 3) Dipendenza tra iterazioni
```c
#define N ...
double a[N];

/* ... */

for (int i=1; i<N; i++) {
    a[i] = a[i] + a[i-1];
}
```

**Consegna:**  
Discutere se il ciclo può essere parallelizzato così com’è. Se no, spiegare quale dipendenza impedisce il parallelismo e se esiste una riformulazione possibile.

---

# 4) Loop annidato rettangolare
```c
#define N ...
#define M ...
double A[N][M];

/* ... */

for (int i=0; i<N; i++) {
    for (int j=0; j<M; j++) {
        A[i][j] = 2.0 * A[i][j];
    }
}
```

**Consegna:**  
Discutere come parallelizzare il ciclo. Valutare se conviene parallelizzare il ciclo esterno, quello interno, o usare `collapse`.

---

# 5) Loop annidato non rettangolare
```c
#define N ...
double T[N][N];

/* ... */

for (int i=0; i<N; i++) {
    for (int j=i; j<N; j++) {
        T[i][j] = T[i][j] + 1.0;
    }
}
```

**Consegna:**  
Discutere se il frammento è parallelizzabile e quale difficoltà introduce il fatto che il dominio iterativo non sia rettangolare.

---

# 6) Accesso con dipendenza diagonale
```c
#define N ...
double A[N][N];

/* ... */

for (int i=1; i<N; i++) {
    for (int j=1; j<N; j++) {
        A[i][j] = A[i-1][j-1] + A[i][j];
    }
}
```

**Consegna:**  
Discutere se il codice è parallelizzabile. In caso negativo, identificare la dipendenza e spiegare se il problema può essere risolto con una diversa organizzazione del calcolo.

---

# 7) Ricerca del massimo
```c
#define N ...
int v[N];
int max = v[0];

/* ... */

for (int i=1; i<N; i++) {
    if (v[i] > max)
        max = v[i];
}
```

**Consegna:**  
Discutere se il ciclo è parallelizzabile. Se sì, dire quale costrutto OpenMP è più adatto e quali sono i possibili effetti di un’implementazione ingenua.

---

# 8) Matrix-vector multiply
```c
#define N ...
double A[N][N], x[N], y[N];

/* ... */

for (int i=0; i<N; i++) {
    y[i] = 0;
    for (int j=0; j<N; j++) {
        y[i] += A[i][j] * x[j];
    }
}
```

**Consegna:**  
Discutere il parallelismo disponibile nel codice. Indicare quali cicli sono indipendenti e quale forma di parallelizzazione OpenMP è più naturale.

---

# 9) Triangular GEMV
```c
#define N ...
double A[N][N], x[N], y[N];

/* ... */

for (int i=0; i<N; i++) {
    y[i] = 0;
    for (int j=i; j<N; j++) {
        y[i] += A[i][j] * x[j];
    }
}
```

**Consegna:**  
Discutere se il frammento sia parallelizzabile e se compaiano problemi di sbilanciamento del carico. Proporre una strategia OpenMP e commentarne i limiti.

---

# 10) Odd-even sort
```c
#define N ...
int v[N];

/* ... */

for (int phase=0; phase<N; phase++) {
    if (phase % 2 == 0) {
        for (int i=0; i<N-1; i+=2) {
            cmp_and_swap(&v[i], &v[i+1]);
        }
    } else {
        for (int i=1; i<N-1; i+=2) {
            cmp_and_swap(&v[i], &v[i+1]);
        }
    }
}
```

**Consegna:**  
Discutere se le singole fasi possono essere parallelizzate. Specificare quali iterazioni possono essere eseguite concorrente e quale sincronizzazione serve tra una fase e la successiva.

---

# 11) Binary search
```c
#define N ...
int x[N];
int key;

/* ... */

int low = 0, high = N-1;
while (low <= high) {
    int mid = (low + high) / 2;
    if (x[mid] == key)
        return mid;
    else if (x[mid] < key)
        low = mid + 1;
    else
        high = mid - 1;
}
```

**Consegna:**  
Discutere se il frammento possa essere parallelizzato in modo utile. Se sì, spiegare a quale livello si può introdurre parallelismo; se no, spiegare perché il controllo è sequenziale.

---

# 12) Binary tree traversal
```c
typedef struct node {
    int value;
    struct node *left;
    struct node *right;
} node_t;

/* ... */

void visit(node_t *r)
{
    if (r == NULL)
        return;

    visit(r->left);
    visit(r->right);
    process(r->value);
}
```

**Consegna:**  
Discutere come parallelizzare la visita usando OpenMP tasks. Evidenziare vantaggi, possibili overhead e casi in cui la soluzione potrebbe non essere efficiente.

---

# 13) Matrix sum con bilanciamento
```c
#define N ...
double A[N][N], B[N][N], C[N][N];

/* ... */

for (int i=0; i<N; i++) {
    for (int j=0; j<N; j++) {
        C[i][j] = A[i][j] + B[i][j];
    }
}
```

**Consegna:**  
Discutere se conviene parallelizzare il ciclo esterno, il ciclo interno oppure usare `collapse(2)`. Motivare la scelta in termini di bilanciamento e località di memoria.

---

# 14) Filtro 2D
```c
#define N ...
double X[N][N], Y[N][N];

/* ... */

for (int i=1; i<N-1; i++) {
    for (int j=1; j<N-1; j++) {
        Y[i][j] = (X[i-1][j] + X[i+1][j] + X[i][j-1] + X[i][j+1]) / 4.0;
    }
}
```

**Consegna:**  
Discutere se il calcolo può essere parallelizzato e se esistono problemi di dipendenza o di accesso concorrente. Indicare anche eventuali effetti positivi dell’uso di `collapse(2)`.

---

# 15) Loop con accumulo per riga
```c
#define N ...
double A[N][N], row_sum[N];

/* ... */

for (int i=0; i<N; i++) {
    row_sum[i] = 0.0;
    for (int j=0; j<N; j++) {
        row_sum[i] += A[i][j];
    }
}
```

**Consegna:**  
Discutere quali livelli del ciclo possono essere parallelizzati. Evidenziare il motivo per cui non è necessario alcun `reduction` globale.

---

# 16) Prefix sum
```c
#define N ...
int v[N];

/* ... */

for (int i=1; i<N; i++) {
    v[i] = v[i] + v[i-1];
}
```

**Consegna:**  
Discutere se il frammento è parallelizzabile direttamente. Se no, spiegare perché e indicare che tipo di algoritmo parallelo servirebbe per risolvere il problema.

---

# 17) Crivello di Eratostene
```c
#define N ...
int is_prime[N];

/* ... */

for (int p=2; p*p<N; p++) {
    if (is_prime[p]) {
        for (int k=p*p; k<N; k+=p) {
            is_prime[k] = 0;
        }
    }
}
```

**Consegna:**  
Discutere se e come si possa introdurre parallelismo. Evidenziare le parti indipendenti e quelle che invece richiedono coordinamento tra iterazioni.

---

# 18) Blocco iterativo con dipendenza tra step
```c
#define N ...
double a[N], b[N];

/* ... */

for (int t=0; t<1000; t++) {
    for (int i=1; i<N-1; i++) {
        b[i] = (a[i-1] + a[i] + a[i+1]) / 3.0;
    }
    for (int i=1; i<N-1; i++) {
        a[i] = b[i];
    }
}
```

**Consegna:**  
Discutere se il frammento è parallelizzabile e a quale livello. Distinguere tra parallelismo intra-iterazione e inter-iterazione.

---

## Se vuoi, posso fare ancora meglio
Posso trasformare questi esercizi in uno di questi tre formati:

1. **versione “compito vero”**  
   con layout simile alla foto, titolo e testo più pulito;

2. **versione “solo codice + consegna”**  
   così li puoi stampare e risolvere;

3. **versione graduata per difficoltà**  
   facile / media / difficile, come una simulazione d’esame.

Se vuoi, nel prossimo messaggio ti preparo direttamente la **versione finale pronta da studiare**.