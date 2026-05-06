# Best Practices in Python

In questa sezione approfondiremo le *best practices* da seguire nello sviluppo di progetti Python, sottolineando anche l'associazione con Data Analytics.

<!-- New section -->

## Sommario 

1. **Gestione delle dipendenze** (pip, uv)
2. **Strutturazione e creazione di package** (vantaggi, struttura, condivisione, pyproject.toml)
3. **Code Quality** (PEP8, linters, ruff, pre-commit)
4. **Tipizzazione** (sintassi, typing, mypy)
5. **Logging, monitoraggio e configurazione** (logging, tqdm, yaml)  


<!-- New section -->

## 1. Gestione delle dipendenze

<!-- New subsection -->

In Python, per gestire le dipendenze del progetto, si lavora solitamente in ambienti virtuali isolati, così da evitare conflitti e ingombro nel sistema globale.

- **pip**: gestore predefinito di pacchetti in Python, con installazione dall'indice [PyPI](https://pypi.org/).  
- **poetry**: tool più moderno per gestire dipendenze, versioni e creazione di lockfile.  
- **conda**: diffusissimo in ambienti Data Science per la sua facilità d'uso e poca conoscenza di programmazione 
- **uv**: strumento di gestione progetti Python più completa.

<!-- .element: class="fragment" -->

<!-- New subsection -->

Nel panorama dello sviluppo Python, l’uso di ambienti virtuali (virtual environment, spesso in breve _venv_) è **fondamentale**.

Consente di creare un ecosistema isolato per installare librerie e dipendenze, evitando conflitti a livello di sistema o con altri progetti che sfruttano versioni differenti delle stesse librerie.

<!-- .element: class="fragment" -->

<!-- New subsection -->

### Creazione e utilizzo di un venv (virtual environment)

La procedura standard prevede:

1. **Creazione** di un ambiente virtuale:
```bash
python -m venv <nome_venv>
```

questo comando genera una cartella denominata `<nome_venv>` che conterrà l’interprete Python e i pacchetti installati esclusivamente per questo progetto. Tipicamente si scelgono nomi come _venv_ o _.venv_ e useremo questo d'ora in poi per semplicità.

<!-- .element: class="fragment" -->

<!-- New subsection -->

2. **Attivazione** dell'ambiente virtuale:

_Windows_:
```bash
venv\Scripts\activate
```

 _macOS/Linux_:
```bash
source venv/bin/activate
```

Una volta attivato, tutti i comandi di installazione (es. `python` o `pip install`) opereranno su questo ambiente locale.

<!-- .element: class="fragment" -->

<!-- New subsection -->

3. **Disattivazione** dell'ambiente virtuale:
```bash
deactivate
```

Questo comando ha effetto solo all'interno dell'ambiente virtuale e riporta l’utente all’ambiente Python globale.

<!-- .element: class="fragment" -->

<!-- New subsection -->

**Perché è importante?**

- **Isolamento**: non si rischia di creare conflitti con altre installazioni di librerie nel sistema.

<!-- .element: class="fragment" data-fragment-index="1" -->

- **Sperimentazione**: è possibile provare diverse versioni di Python (es. 3.7, 3.8, 3.9) o differenti rilasci di librerie.

<!-- .element: class="fragment" data-fragment-index="2" -->

- **Riproducibilità**: più facile far sì che ogni collaboratore o server di produzione abbia le stesse dipendenze installate.

<!-- .element: class="fragment" data-fragment-index="3" -->

<!-- New subsection -->

### pip (Pip install Package)

**pip** è il gestore di pacchetti di default in Python, permette di installare librerie dal [Python Package Index (PyPI)](https://pypi.org/).

Una volta attivato il venv, possiamo sfruttare i seguenti comandi:

<!-- .element: class="fragment" data-fragment-index="1" -->

- `pip install <nome_pacchetto>`: installa una libreria/pacchetto
- `pip uninstall <nome_pacchetto>`: disinstalla una libreria/pacchetto
- `pip list`: mostra i pacchetti installati nell'ambiente
- `pip freeze > requirements.txt`: per salvare la lista dei pacchetti installati come "lockfile"
- `pip install -r requirements.txt`: per installare i pacchetti da un file di requirements

<!-- .element: class="fragment" data-fragment-index="1" -->

<!-- New subsection -->

### uv: Gestione avanzata dei progetti Python

[uv](https://docs.astral.sh/uv/) è uno strumento creato da _astral-sh_ che fornisce un’esperienza di gestione progetti Python più completa rispetto a pip. 

Con un singolo framework è possibile:

<!-- .element: class="fragment" data-fragment-index="1" -->

- installare versioni diverse di Python e creare l’ambiente virtuale
- gestire ed aggiornare le dipendenze
- eseguire script di build o azioni di packaging

<!-- .element: class="fragment" data-fragment-index="1" -->

In pratica, uv si propone come soluzione “tutto in uno” per la gestione dei progetti. I suoi pregi sono estrema velocità e ottima robustezza agli errori grazie all'implementazione in Rust.

<!-- .element: class="fragment" data-fragment-index="2" -->

<!-- New subsection -->

Le sue *feature* più importanti includono:

1. **Installazione di Python**: provare varie versioni di Python (es. 3.7, 3.8, 3.9) risulta molto più rapido con `uv python install <python-version>`

2. **Gestione progetti Python**: permette di gestire i progetti a 360° gradi, dalla creazione (`uv init`), l'aggiunta e rimozione di dipendenze (`uv add`, `uv remove`), installazione o aggiornamento di tutte le dipendenze (`uv sync`).

3. **Creazione ottimizzata venv**: il comando `uv venv` (o `uv venv --python <python-version>`) creano l'ambiente virtuale che `uv` gestirà. I comandi al punto precedente creano il venv in automatico.

<!-- New subsection -->

4. **Task integrati**: `uv run` è un “task runner” integrato che consente di eseguire script nell'ambiente, sostituendo altri comandi:  
   ```bash
   uv run main.py
   uv run tests
   ```

5. **Supporto a build e packaging**: uv semplifica i passaggi di redistribuzione del codice come la creazione di pacchetti o wheel (.whl) con comandi specifici (`uv build`, `uv publish`), integrando le procedure di packaging definite in `pyproject.toml` o negli strumenti di build moderni.

6. **Compatibilità con la gestione legacy (pip)**: uv mantiene un'interfaccia per l'utilizzo di pip per un controllo a un livello più basso, con `uv pip install`, `uv pip uninstall` o `uv pip list`.

<!-- New section -->

## 2. **Strutturazione e creazione di package**

<!-- New subsection -->

### Perché creare un package

Organizzare il codice come *package* invece che come raccolta di script offre vantaggi sostanziali:
- **Manutenibilità**: il codice è suddiviso in moduli con responsabilità chiare.
- **Riutilizzo**: classi e funzioni ben incapsulate possono essere riusate in più progetti.
- **Condivisione**: si può pubblicare il package su [PyPi](https://pypi.org/) e installarlo con `pip install <nome>`.

<!-- New subsection -->

<div class="cols">

<div style="max-height: 400px; min-height: 400px; min-width: 300px" class="pre-no-block pre-no-max-height">

```text
nome_progetto/
├── .venv/
├── src/
│   └── nome_pacchetto/
│       ├── __init__.py
│       ├── modulo1.py
│       ├── modulo2.py
│       └── ...
├── tests/
│   ├── test_modulo1.py
│   └── test_modulo2.py
├── docs/
├── setup.py / pyproject.toml
├── README.md
├── LICENSE
└── .gitignore
```

</div>

<div>

Un layout tipico potrebbe essere:  
- `src/nome_pacchetto/`: contiene effettivamente il package, con `__init__.py` a segnalare a Python che si tratta di un package.
- `tests/`: raccoglie i file di test (unitari, integrazione, ecc.) per garantire una buona copertura del codice.
- `setup.py`/`pyproject.toml`: file di configurazione e metadati del progetto (nome, versioni, dipendenze).

</div>

</div>

<!-- New subsection -->

Python interpreta un package se al suo interno è presente il file `__init__.py`, che può anche essere spesso vuoto.

Quando eseguiamo del codice all'interno di un package, gli script prendono il nome di modulo e bisogna specificare a Python che il file da eseguire fa parte di un package nel modo seguente:

```bash
python -m mio_package.sottocartella_package.script_modulo
```

In questo modo Python risolve correttamente gli import relativi all'interno del package.

> Per non partire da zero esistono tool come [Cookiecutter](https://github.com/cookiecutter/cookiecutter) che generano lo scheletro del progetto da template predefiniti.

<!-- New subsection -->

### Metodi di packaging

Quando si sviluppa un pacchetto Python destinato a essere condiviso o installato, è fondamentale definire correttamente i metadati e le dipendenze. Si può impostare il packaging del progetto con **pyproject.toml**

<!-- New subsection -->

### pyproject.toml

Lo standard moderno per definire metadati e dipendenze di un progetto Python è il file **pyproject.toml** ([PEP 518](https://peps.python.org/pep-0518/)). Sostituisce i vecchi `setup.py` e `setup.cfg`, che oggi sono considerati legacy. Esempio:


```toml
[project]
name = "mio_pacchetto"
version = "0.1.0"
description = "Un pacchetto base per esempio"
authors = [
  { name="Mario Rossi", email="mario.rossi@example.com" }
]
dependencies = [
  "requests",
  "numpy"
]

[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"
```

<!-- New subsection -->

### Lockfile e riproducibilità

Quando si esegue `uv sync`, uv legge `pyproject.toml`, installa le dipendenze e genera un **lockfile** che fissa le versioni esatte di tutti i pacchetti (incluse le sotto-dipendenze).

Rispetto al classico `requirements.txt`, il lockfile garantisce che tutti i collaboratori (e i server di produzione) usino esattamente le stesse versioni, evitando il classico *"funziona sulla mia macchina"*.


<!-- New subsection -->

### Build e publish con uv

Una volta configurato `pyproject.toml`, distribuire il package è immediato:

```bash
uv build     # genera la wheel nella cartella dist/
uv publish   # pubblica direttamente su PyPi
```

Questi comandi sostituiscono il flusso tradizionale `python -m build` + `twine upload`, integrando tutto nei metadati di `pyproject.toml`.

<!-- New section -->

## 3. **Code Quality**

<!-- New subsection -->

Garantire una buona *code quality* significa adottare convenzioni di stile uniformi, automatizzare i controlli e prevenire bug ricorrenti. Il punto di partenza è la [PEP8](https://peps.python.org/pep-0008/), che definisce lo stile raccomandato per il codice Python.

<!-- New subsection -->

### PEP8 in breve

PEP8 (*Python Enhancement Proposal* n. 8) è la guida stilistica di riferimento. Le convenzioni principali:

- **Indentazione**: 4 spazi (mai tab)
- **Lunghezza righe**: 79-88 caratteri
- **Naming**: `CamelCase` per classi, `snake_case` per funzioni e variabili, `UPPER_CASE` per costanti
- **Import**: uno per riga, raggruppati (stdlib, third-party, locali)
- **Spazi bianchi**: regole precise attorno a operatori e dopo virgole

> [PEP257](https://peps.python.org/pep-0257/) si focalizza invece sulle docstring.

<!-- New subsection -->

### Linter e formatter

Per non dover ricordare a memoria tutte le regole, esistono tool che automatizzano il controllo:

- **Linter**: analizza il codice e segnala problemi, sia *stilistici* (formattazione) che *logici* (variabili non usate, import inutili, possibili bug).
- **Formatter**: riscrive automaticamente il codice nello stile corretto.

Storicamente si usavano tool diversi per ogni compito (Flake8, Black, isort, pylint...). Oggi lo standard di fatto è **Ruff**, che li sostituisce tutti.

<!-- New subsection -->

### Ruff

[Ruff](https://docs.astral.sh/ruff/) è un linter e formatter scritto in Rust, estremamente veloce e configurabile via `pyproject.toml`. È sviluppato dagli stessi autori di uv.

Comandi principali:

```bash
ruff check                  # analizza i file della directory
ruff check --fix            # corregge automaticamente quando possibile
ruff format                 # formatta i file (sostituisce Black)
```

> Demo live: [Ruff Playground](https://play.ruff.rs/)

Esistono altri linter (Pylint, Flake8, PyFlakes, pycodestyle), ma per progetti nuovi Ruff è la scelta consigliata.

<!-- New subsection -->

### pre-commit

Anche con Ruff installato, ci si può dimenticare di eseguirlo prima di un `git commit`. Per evitarlo si usa [pre-commit](https://pre-commit.com/): un framework che esegue automaticamente dei controlli (chiamati *hook*) **prima** che il commit venga registrato da Git.

<!-- New subsection -->

### Come funziona

Il flusso è il seguente:

1. **Installazione del tool** nell'ambiente:
```bash
pip install pre-commit
```

2. **Configurazione**: si crea un file `.pre-commit-config.yaml` nella root del repository, che elenca gli hook da eseguire (qui useremo Ruff):


```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.6.0
    hooks:
      - id: ruff
        args: ["--fix"]
      - id: ruff-format
```


<!-- New subsection -->

3. **Attivazione nel repository**: si esegue una sola volta per progetto.
```bash
pre-commit install
```

Questo comando aggancia pre-commit al meccanismo di hook di Git (più precisamente, crea uno script in `.git/hooks/pre-commit`). Da questo momento in poi non serve più chiamare nessun comando manualmente.

4. **Uso quotidiano**: ogni volta che si esegue `git commit`, pre-commit si attiva da solo sui file modificati:
   - se tutti gli hook passano → il commit avviene normalmente
   - se un hook fallisce o modifica i file (es. Ruff li riformatta) → il commit viene **bloccato**. L'utente deve fare `git add` dei file corretti e ripetere `git commit`.


<!-- New section -->

## 4. **Tipizzazione**

<!-- New subsection -->

Python è un linguaggio a tipizzazione **dinamica**: il tipo di una variabile è determinato a runtime in base al valore che contiene. Questo dà flessibilità ma rende facile introdurre bug che si manifestano solo durante l'esecuzione.


Le **type hint** (introdotte in [PEP 484](https://peps.python.org/pep-0484/)) permettono di annotare il codice con i tipi attesi, senza modificarne il comportamento. Sono un suggerimento per l'IDE, per i tool di analisi statica e soprattutto per chi legge il codice.
<!-- .element: class="fragment" data-fragment-index="1" -->

<!-- New subsection -->

### Sintassi base

Le annotazioni si applicano a variabili, parametri e valori di ritorno:

```python
nome: str = "Mario"
età: int = 30

def saluta(nome: str) -> str:
    return f"Ciao, {nome}"
```

> **Importante**: le type hint **non vengono verificate a runtime**. Python non solleva errori se passi un `int` a una funzione che si aspetta una `str`. Servono per analisi statica e leggibilità.

<!-- New subsection -->

### Tipi composti con `typing`

Per tipi più complessi si usa il modulo `typing` (in versioni recenti di Python molti tipi sono usabili direttamente):

```python
# Collezioni
prezzi: list[float] = [1.99, 2.50, 3.75]
utenti: dict[str, int] = {"Mario": 30, "Anna": 25}

# Tipi opzionali e unioni
from typing import Optional, Union

def trova_utente(id: int) -> Optional[str]:
    # restituisce una stringa oppure None
    ...

def parse(valore: Union[str, int]) -> str:
    # accetta sia str che int
    ...
```

`Optional[X]` è equivalente a `Union[X, None]` ed è il caso più comune.

<!-- New subsection -->

### Vantaggi pratici

- **Leggibilità**: chi legge il codice sa subito cosa aspettarsi senza dover dedurre dal contesto.
- **Autocompletamento IDE**: VS Code e PyCharm usano le annotazioni per suggerimenti più precisi.
- **Bug catturati prima dell'esecuzione**: con un type checker, molti errori emergono in fase di sviluppo invece che in produzione.
- **Documentazione viva**: a differenza dei commenti, le annotazioni non possono "diventare obsolete" silenziosamente perché sono parte del codice.

<!-- New subsection -->

### Type checking con mypy

[mypy](http://mypy-lang.org/) è il tool più diffuso per la verifica statica dei tipi. Analizza il codice **senza eseguirlo** e segnala ogni incoerenza tra tipi dichiarati e tipi effettivamente usati.

```bash
pip install mypy
mypy nomefile.py
```

Esempio:

```python
def somma(a: int, b: int) -> int:
    return a + b

risultato = somma("10", 20)   # mypy segnala l'errore qui
```

```bash
error: Argument 1 to "somma" has incompatible type "str"; expected "int"
```

Senza mypy questo errore si scoprirebbe solo a runtime, quando Python tenta di concatenare `"10" + 20` e solleva un `TypeError`.

<!-- New subsection -->

### Configurazione e altri tool

mypy si configura tramite la sezione `[tool.mypy]` di `pyproject.toml` (o un file `mypy.ini` separato). L'opzione `--strict` abilita controlli più severi, utili per progetti maturi.

Esistono alternative:
- **Pyright** (Microsoft): più veloce, integrato nell'estensione *Pylance* di VS Code.
- **pyre** (Meta), **pytype** (Google): meno diffusi, usati principalmente nei rispettivi ecosistemi aziendali.

Per progetti nuovi, mypy o Pyright sono le scelte standard.

<!-- New section -->

## 5. **Logging, monitoraggio e configurazione**

<!-- New subsection -->

Quando un programma cresce, due esigenze diventano centrali:
- **tracciare cosa succede** durante l'esecuzione (per debug, audit, monitoraggio)
- **separare i parametri dal codice**, in modo da poter cambiare configurazione senza toccare i sorgenti

Vediamo gli strumenti standard di Python per entrambe.

<!-- New subsection -->

### Perché non usare `print`?

`print` è veloce da scrivere ma ha limiti seri:
- non distingue tra messaggi informativi, di debug e di errore
- non si può "spegnere" facilmente in produzione
- non include automaticamente timestamp, modulo di origine o contesto
- scrive solo su stdout

Il modulo `logging` della libreria standard risolve tutti questi problemi.

<!-- New subsection -->

### Logging: uso base

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.debug("Dettaglio per sviluppatori")    # non viene mostrato (sotto INFO)
logging.info("Caricamento dataset completato")
logging.warning("File di config non trovato, uso default")
logging.error("Connessione al DB fallita")
```

<!-- New subsection -->

I cinque livelli standard, in ordine crescente di gravità:

| Livello    | Quando usarlo                                  |
|------------|------------------------------------------------|
| `DEBUG`    | Dettagli interni utili durante lo sviluppo     |
| `INFO`     | Eventi normali del flusso (avvio, completamento) |
| `WARNING`  | Qualcosa di anomalo ma non bloccante           |
| `ERROR`    | Un'operazione è fallita                         |
| `CRITICAL` | Errore grave che compromette l'applicazione    |

Impostando `level=logging.INFO`, i messaggi di livello inferiore (`DEBUG`) vengono ignorati. È sufficiente cambiare una riga per attivare/disattivare la verbosità in produzione.

<!-- New subsection -->

### Configurazioni più ricche

Per progetti reali si configurano *handler* (dove inviare i log: console, file, syslog...) e *formatter* (formato del messaggio):

```python
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

formatter = logging.Formatter(
    "[%(asctime)s] [%(levelname)s] %(name)s: %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S"
)

# Handler per console
console = logging.StreamHandler()
console.setFormatter(formatter)
logger.addHandler(console)

# Handler per file
file_handler = logging.FileHandler("app.log")
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
```

<!-- New subsection -->

 Esistono alternative come [Loguru](https://github.com/Delgan/loguru), che semplificano la configurazione iniziale.
 
 Per progetti ML, [Weights & Biases](https://wandb.ai/) (wandb) è lo standard per il *tracking di esperimenti*: lo vedremo nella parte sull'ottimizzazione degli iperparametri.

<!-- New subsection -->

### tqdm: barre di progresso

In contesti ML/DL si lavora spesso con loop lunghi (training di modelli, preprocessing di dataset). [tqdm](https://github.com/tqdm/tqdm) trasforma qualsiasi iterabile in una barra di progresso con stima del tempo residuo:

```python
from tqdm import tqdm
import time

for i in tqdm(range(100), desc="Training"):
    time.sleep(0.05)
```

Output:
```bash
Training:  47%|████▋     | 47/100 [00:02<00:02, 19.84it/s]
```
Funziona anche con `range`, liste, file, generatori e si integra nativamente con Pandas (`tqdm.pandas()`) e con i loop di training di PyTorch.

<!-- New subsection -->

### Configurazione: file YAML

Mettere parametri direttamente nel codice (*hardcoded*) è una cattiva pratica:
- per cambiare un valore bisogna modificare il sorgente
- è difficile tracciare quale configurazione ha prodotto un certo risultato
- non si possono confrontare run diverse

La soluzione è esternalizzare i parametri in un **file di configurazione**. Il formato più diffuso è **YAML**, particolarmente leggibile e adatto a strutture annidate.

<!-- New subsection -->

### Esempio YAML

`config.yaml`:

```yaml
model:
  type: random_forest
  n_estimators: 100
  max_depth: 10
data:
  train_path: "data/train.csv"
  val_path: "data/val.csv"
  batch_size: 64
training:
  epochs: 30
  learning_rate: 0.001
  device: cuda
```

<!-- New subsection -->

Caricamento in Python con la libreria `pyyaml`:

```python
import yaml

with open("config.yaml") as f:
    config = yaml.safe_load(f)

print(config["model"]["n_estimators"])   # 100
print(config["training"]["learning_rate"]) # 0.001
```

`yaml.safe_load` restituisce un normale dizionario Python annidato.

<!-- New subsection -->

### Vantaggi della configurazione esterna

- **Riproducibilità**: il file di config va in Git insieme al codice, quindi ogni esperimento è documentato.
- **Sperimentazione rapida**: cambiare iperparametri richiede solo modifica del file, niente ricompilazione né rilancio dell'IDE.
- **Separazione delle preoccupazioni**: il codice descrive *come* fare le cose, il config descrive *con quali parametri*.

> Per esigenze più avanzate (override da CLI, composizione di più file di config, gestione di esperimenti su cluster) esistono framework come [Hydra](https://hydra.cc/) e [OmegaConf](https://omegaconf.readthedocs.io/), molto usati in ambito ML.