# Bella

A knowledge compiler for:

- (s)d-DNNF circuits,
- wDNNF, pwDNNF, and nwDNNF circuits.

**Supported OS**: Linux, macOS (Apple Silicon), and Windows

> [!IMPORTANT]
> The source code is available in the [Hydra repository](https://github.com/Illner/Hydra).

> [!NOTE]
> **Cara**, an isomorphism-aware #SAT solver using the same core, is available in the [CaraSolver repository](https://github.com/Illner/CaraSolver).

## Running Bella

To print the help:

```console
./Bella -h
```

To print the version:

```console
./Bella -v
```

To run the knowledge compiler:

```console
./Bella < -w | -pw | -nw | -d | -sd > < -ph | -ka | -cd > -i input_file
        [-c] [-e] [-r] [ -s statistics_file ] [ -o output_file ] [ -t positive_integer (default: 86400) ]
        [ -r_dh | -dlcs_dh | -dlis_dh | -dlcs_dlis_dh | -vsids_dh | -vsads_dh | -jwos_dh | -jwts_dh | -eupc_dh | -aupc_dh ]
        [ -n_ccs | -s_ccs | -h_ccs | -b_ccs | -i_ccs | -c_ccs [integer] (min: 0, max: 10, default: 2) ] [ -n_cccs | -s_cccs | -c_cccs ]
        [ -n_hccs | -s_hccs | -h_hccs | -b_hccs | -c_hccs [integer] (min: 0, max: 10, default: 2) ] [ -n_hcccs | -s_hcccs | -c_hcccs ]
        [ -n_hnw | -s_hnw | -cl_hnw ] [ -a_rhc | -iup_rhc | -fs_rhc | -ehc_rhc | -iup_fs_rhc ]
```

### Recommended Usage

On Linux and macOS:

```console
./Bella -w -ph -e -i input_file
```

On Windows:

```console
./Bella -w -ka -e -i input_file
```

> [!TIP]
> On Windows, hMETIS is significantly slower because it communicates via files.
> Therefore, we suggest using KaHyPar instead.

> [!NOTE]
> Replace `-w` with the desired circuit type (see [Configurations](#configurations)).

### Configurations

Circuit types:
* **-w** — wDNNF circuit
* **-pw** — pwDNNF circuit
* **-nw** — nwDNNF circuit

* **-d** — d-DNNF circuit
* **-sd** — sd-DNNF circuit

Hypergraph partitioning:
* **-ph** — PaToH (Linux and macOS), hMETIS (Windows) *(**recommended** on Linux and macOS)*
* **-ka** — KaHyPar (Linux, macOS, and Windows) *(**recommended** on Windows)*
* **-cd** — Cara (Linux and macOS)

Files:
* **-i** — specify the CNF file name
* **-s** — specify the file name where the statistics will be saved
* **-o** — specify the file name where the compiled circuit will be saved

Decision heuristics:
* **-r_dh** — random
* **-dlcs_dh** — dynamic largest combined sum (DLCS)
* **-dlis_dh** — dynamic largest individual sum (DLIS)
* **-dlcs_dlis_dh** — DLCS + DLIS as a tie-breaker (DLCS-DLIS)
* **-vsids_dh** — variable state independent decaying sum (VSIDS)
* **-vsads_dh** — variable state aware decaying sum (VSADS) *(default)*
* **-jwos_dh** — Jeroslow–Wang (one-sided)
* **-jwts_dh** — Jeroslow–Wang (two-sided)
* **-eupc_dh** — exact unit propagation count (EUPC)
* **-aupc_dh** — approximate unit propagation count (AUPC)

Component caching schemes:
* **-n_ccs** — none
* **-s_ccs** — standard
* **-h_ccs** — hybrid
* **-b_ccs** — basic
* **-i_ccs** — i *(default)*
* **-c_ccs** — Cara: optionally sets the number of sample moments *(min: 0, max: 10, default: 2)*

> [!NOTE]
> All the component caching schemes except Cara's are described in
> J.-M. Lagniez and P. Marquis, _Enhanced Caching for #SAT Solving_, 2020 (preprint), <https://hal.science/hal-02963599>.

Component cache cleaning strategies:
* **-n_cccs** — none
* **-s_cccs** — sharpSAT
* **-c_cccs** — Cara *(default)*

Hypergraph cut caching schemes:
* **-n_hccs** — none *(default)*
* **-s_hccs** — standard
* **-h_hccs** — hybrid
* **-b_hccs** — basic
* **-c_hccs** — Cara: optionally sets the number of sample moments *(min: 0, max: 10, default: 2)*

Hypergraph cut cache cleaning strategies:
* **-n_hcccs** — none *(default)*
* **-s_hcccs** — sharpSAT
* **-c_hcccs** — Cara

Hypergraph node weight types:
* **-n_hnw** — none
* **-s_hnw** — standard
* **-cl_hnw** — clause length *(default)*

Hypergraph cut recomputation strategies:
* **-a_rhc** — hypergraph cuts are computed at each node
* **-iup_rhc** — a new hypergraph cut is computed when immense unit propagation is performed *(default)*
* **-fs_rhc** — a new hypergraph cut is computed when the current formula is split
* **-ehc_rhc** — a new hypergraph cut is computed when the current hypergraph cut is empty
* **-iup_fs_rhc** — a new hypergraph cut is computed when immense unit propagation is performed, or the current formula is split

General options:
* **-c** — count the models
* **-h** — print the help message
* **-v** — print version information
* **-e** — use the equivalence simplification method *(**highly recommended**)*
* **-t** — set the compilation timeout *(default: 86400 s)*
* **-r** — write the statistics file in a human-readable form

> [!NOTE]
> For wDNNF, pwDNNF, and nwDNNF circuits, model counting is also supported, but the models are counted using enumeration, so counting runs with polynomial delay rather than in polynomial time.

### Syntax of Circuit Files

The file format extends the one defined in the user manual (Section C) of [the c2d compiler](http://reasoning.cs.ucla.edu/c2d/).

* A weak AND node is specified as follows: ***B*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* A positive weak AND node is specified as follows: ***P*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* A negative weak AND node is specified as follows: ***N*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* A (classical) decomposable AND node is specified as follows: ***A*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*

## Tests

Bella ships with two test binaries. Run both to verify a build.

### HydraTest

```console
./HydraTest
```

> [!WARNING]
> Some tests for caching assume that the type `unsigned long long int` has precisely 64 bits.

> [!NOTE]
> The test takes around 10 seconds.

### BellaTest

```console
./BellaTest
```

> [!NOTE]
> The test takes around 1 hour.

## Third-Party Software

### SAT Solvers

* [MiniSat 2.2.0 (d4v2 version)](https://github.com/crillab/d4v2)

* [Glucose 3.0 (d4v2 version)](https://github.com/crillab/d4v2) — _work in progress_

* [MiniSat 2.2.0](https://github.com/niklasso/minisat) — _implemented, not used_

* [CaDiCaL 3.0.0](https://github.com/arminbiere/cadical) — _work in progress_

### Hash Maps

* [unordered_dense v4.5.0](https://github.com/martinus/unordered_dense)

* [robin-hood-hashing 3.11.5](https://github.com/martinus/robin-hood-hashing)

* [flat_hash_map](https://github.com/skarupke/flat_hash_map) — _implemented, not used_

### Hypergraph Partitioning

* [PaToH v3.3](https://faculty.cc.gatech.edu/~umit/software.html) — _used on Linux and macOS_

* [hMETIS 1.5.3](https://papers.karypis.org/glaros/software/metis/overview.html) — _used only on Windows_

* [KaHyPar v.1.3.3](https://kahypar.org/) — _used on Linux, macOS, and Windows_

## Licence

Bella is released under the [MIT License](LICENSE). The bundled third-party software components (see above) are subject to their own licences. Some of them are restricted to academic and research use. For the licences, see `Hydra/external/` in
the [Hydra repository](https://github.com/Illner/Hydra).

## Papers

If you use **Bella for (s)d-DNNF/wDNNF circuits** in an academic setting, please cite the following paper describing the knowledge compiler:

```bibtex
@article{Illner_Kucera_2024, 
    author  = {Illner, Petr and Ku\v{c}era, Petr}, 
    title   = {A Compiler for Weak Decomposable Negation Normal Form}, 
    volume  = {38}, 
    url     = {https://ojs.aaai.org/index.php/AAAI/article/view/28926}, 
    DOI     = {10.1609/aaai.v38i9.28926}, 
    number  = {9}, 
    journal = {Proceedings of the AAAI Conference on Artificial Intelligence},
    year    = {2024}, 
    month   = {Mar.}, 
    pages   = {10562-10570} 
}
```

If you use **Bella for pwDNNF/nwDNNF circuits** or **Cara** in an academic setting, please cite the following paper describing the knowledge compiler and caching scheme:

```bibtex
@article{Illner_2025, 
    author  = {Illner, Petr}, 
    title   = {New Compilation Languages Based on Restricted Weak Decomposability}, 
    volume  = {39}, 
    url     = {https://ojs.aaai.org/index.php/AAAI/article/view/33643}, 
    DOI     = {10.1609/aaai.v39i14.33643}, 
    number  = {14}, 
    journal = {Proceedings of the AAAI Conference on Artificial Intelligence}, 
    year    = {2025}, 
    month   = {Apr.}, 
    pages   = {14987-14996} 
}
```
