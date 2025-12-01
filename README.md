# Robust Organ Scheduler

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/micheleguidaa/robust-organ-scheduler/blob/main/robust_organ_scheduler.ipynb)

## Descrizione del Progetto

Questo repository ospita un **Sistema di Supporto alle Decisioni (DSS)** progettato per risolvere il problema della **pianificazione temporale negli interventi di prelievo multiorgano** da donatore singolo.

Il sistema calcola l'orario ottimo di inizio intervento ($t_{start}$) minimizzando il **massimo ritardo ponderato** per 5 organi differenti (Cuore, Polmoni, Fegato, Rene Sx, Rene Dx). L'algoritmo utilizza un approccio di **Ottimizzazione Robusta (Min-Max)** per mitigare i rischi legati all'incertezza della durata dell'intervento chirurgico.

**Autore:** Michele Guida  
**Corso:** Decision Support Systems

## Caratteristiche Principali

* **Gestione dell'Incertezza**: Valuta scenari multipli di durata ($d_{min}, d_{med}, d_{max}$) per garantire una soluzione sicura anche nel caso peggiore (*Worst Case*).
* **Logistica Complessa**: Sincronizza la fine dell'intervento con diverse tabelle di trasporto specifiche per ogni organo e destinazione.
* **Priorità Clinica**: Utilizza pesi ($w_i$) basati sull'urgenza clinica (es. Cuore=100, Polmoni=90, Reni=20) per guidare la decisione di pianificazione.
* **Due Varianti Algoritmiche**:
  * **Linear Scan** (2.a): Soluzione generale per dati non ordinati
  * **Binary Search** (2.b): Soluzione ottimizzata per dati ordinati con speedup significativo

## Modello Matematico

### Funzione di Costo

Il **tempo di attesa** $\delta_i$ per l'organo $i$ è definito come:

$$\delta_{i}(t_{start},d) = \min_{j: S_{i}^{j} \ge t_{start}+d} (S_{i}^{j} - (t_{start}+d))$$

La **funzione di costo** (ritardo totale pesato) è:

$$F(t_{start},d) = \sum_{i \in \mathbf{I}} w_{i} \cdot \delta_{i}(t_{start},d)$$

### Obiettivo: Ottimizzazione Robusta (Minimax)

$$t_{start}^{*} = \arg \min_{t_{start} \in \mathbf{T}} \left( \max_{d \in \mathbf{D}} F(t_{start}, d) \right)$$

## Complessità Computazionale

| Variante | Approccio | Complessità |
|----------|-----------|-------------|
| **2.a Unsorted** | Linear Scan | O(T · D · Σ S_i) |
| **2.b Sorted** | Binary Search | O(T · D · Σ log S_i) |

## Struttura del Repository

```text
robust-organ-scheduler/
├── README.md                      # Questo file
├── requirements.txt               # Dipendenze Python
└── robust_organ_scheduler.ipynb   # Notebook principale con implementazione e analisi
```

## Installazione e Utilizzo

### Prerequisiti

* Python 3.8+
* Jupyter Notebook o JupyterLab

### Setup locale

```bash
# Clona il repository
git clone https://github.com/micheleguidaa/robust-organ-scheduler.git
cd robust-organ-scheduler

# Installa le dipendenze
pip install -r requirements.txt

# Avvia Jupyter
jupyter notebook robust_organ_scheduler.ipynb
```

### Google Colab

Puoi eseguire il notebook direttamente su Google Colab cliccando sul badge in cima a questo README.

## Contenuto del Notebook

1. **Definizione del Problema**: Formalizzazione matematica del problema di scheduling robusto
2. **Implementazione Algoritmi**: Due varianti (Linear Scan e Binary Search)
3. **Test su Piccola Istanza**: Validazione della correttezza
4. **Generazione Scenari**: Funzione per creare dataset sintetici di dimensioni arbitrarie
5. **Analisi Sperimentale**: Stress-test con 50 ripetizioni per configurazione
6. **Discussione Risultati**: Confronto prestazionale e conclusioni

## Risultati Principali

L'analisi sperimentale dimostra che:

* La variante **Binary Search** offre uno speedup significativo rispetto alla Linear Scan quando il numero di partenze è elevato
* Entrambe le varianti scalano linearmente rispetto al numero di orari di inizio ($|\mathbf{T}|$)
* L'algoritmo rimane performante anche con migliaia di opzioni da valutare
