# Robust organ scheduler

## Descrizione del Progetto
Questo repository ospita un **Sistema di Supporto alle Decisioni (DSS)** progettato per risolvere il problema della pianificazione temporale negli interventi di prelievo multiorgano da donatore singolo.

Il sistema calcola l'orario ottimo di inizio intervento ($t_{start}$) minimizzando il **massimo ritardo ponderato** (ischemia time) per 5 organi differenti (Cuore, Polmoni, Fegato, Reni). L'algoritmo utilizza un approccio di **Ottimizzazione Robusta (Min-Max)** per mitigare i rischi legati all'incertezza della durata dell'intervento chirurgico.

## Caratteristiche Principali
* **Gestione dell'Incertezza**: Valuta scenari multipli di durata ($d_{min}, d_{med}, d_{max}$) per garantire una soluzione sicura anche nel caso peggiore (*Worst Case*).
* **Logistica Complessa**: Sincronizza il tempo di prontezza unico ($T_{ready}$) con diverse tabelle di trasporto (aerei, treni) specifiche per ogni organo e destinazione.
* **Priorità Clinica**: Utilizza pesi ($w_k$) basati sull'urgenza clinica (es. il cuore ha peso maggiore dei reni) per guidare la decisione di pianificazione.

## Modello Matematico
Il problema è modellato come una minimizzazione del massimo costo (Min-Max):

$$t_{start}^{*} = \arg \min_{t \in \mathcal{T}} \left( \max_{d \in D} \sum_{k=1}^{5} w_{k} \cdot \delta_{k}(t, d) \right)$$

Dove $\delta_k$ rappresenta il tempo di attesa per il trasporto dell'organo $k$.
