# Bella

A knowledge compiler for:

- (s)d-DNNF circuits,
- wDNNF, pwDNNF and nwDNNF circuits.

**Supported OS**: Linux, macOS (Intel & Apple Silicon), Windows

> [!IMPORTANT]
> The source code is available in the <a href="https://github.com/Illner/Hydra" target="_blank">Hydra repository</a>.

> [!NOTE]
> **Cara**, a #SAT solver using the same core, is available in the <a href="https://github.com/Illner/CaraSolver" target="_blank">CaraSolver repository</a>.

## Running Bella

To run the knowledge compiler:

```console
./Bella -h
```

```console
./Bella -v
```

```console
./Bella < -w | -pw | -nw | -d | -sd > < -ph | -ka | -cd > -i input_file
        [-c] [-e] [-r] [ -s statistics_file ] [ -o output_file ] [ -t positive_integer (default: 86400) ]
        [ -r_dh | -dlcs_dh | -dlis_dh | -dlcs_dlis_dh | -vsids_dh | -vsads_dh | -jwos_dh | -jwts_dh | -eupc_dh | -aupc_dh ]
        [ -n_ccs | -s_ccs | -h_ccs | -b_ccs | -i_ccs | -c_ccs [integer] (min: 0, max: 10, default: 2) ] [ -n_cccs | -s_cccs | -c_cccs ]
        [ -n_hccs | -s_hccs | -h_hccs | -b_hccs | -c_hccs [integer] (min: 0, max: 10, default: 2) ] [ -n_hcccs | -s_hcccs | -c_hcccs ]
        [ -n_hnw | -s_hnw | -cl_hnw ] [ -a_rhc | -iup_rhc | -fs_rhc | -ehc_rhc | -iup_fs_rhc ]
```

> [!TIP]
> On Windows, hMETIS is significantly slower because it communicates via files.
> Therefore, we suggest using KaHyPar instead.

### Configurations

Circuit types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-w** - wDNNF circuit <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-pw** - pwDNNF circuit <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-nw** - nwDNNF circuit <br>

&nbsp;&nbsp;&nbsp;&nbsp; **-d** - d-DNNF circuit <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-sd** - sd-DNNF circuit

Partitioning hypergraph types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-ph** - PaToH (Linux, macOS), hMETIS (Windows) *(**recommended**)*<br>
&nbsp;&nbsp;&nbsp;&nbsp; **-ka** - KaHyPar (Linux, macOS, Windows) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-cd** - Cara (Linux, macOS)

Files: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-i** - specifies the CNF file name <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s** - specifies the file name where the statistics will be saved <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-o** - specifies the file name where the compiled circuit will be saved

Decision heuristics: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-r_dh** - random <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-dlcs_dh** - dynamic largest combined sum (DLCS) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-dlis_dh** - dynamic largest individual sum (DLIS) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-dlcs_dlis_dh** - DLCS + DLIS as a tie-breaker (DLCS-DLIS) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-vsids_dh** - variable state independent decaying sum (VSIDS) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-vsads_dh** - variable state aware decaying sum (VSADS) *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-jwos_dh** - Jeroslow-Wang (one-sided) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-jwts_dh** - Jeroslow-Wang (two-sided) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-eupc_dh** - exact unit propagation count (EUPC) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-aupc_dh** - approximate unit propagation count (AUPC)

Component caching schemes: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_ccs** - none <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_ccs** - standard <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-h_ccs** - hybrid <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-b_ccs** - basic <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-i_ccs** - i *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_ccs** - Cara: optionally sets the number of sample moments *(min: 0, max: 10, default: 2)*

Component cache cleaning strategies: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_cccs** - none <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_cccs** - sharpSAT <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_cccs** - Cara *(default)*

Hypergraph cut caching schemes: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_hccs** - none *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_hccs** - standard <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-h_hccs** - hybrid <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-b_hccs** - basic <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_hccs** - Cara: optionally sets the number of sample moments *(min: 0, max: 10, default: 2)*

Hypergraph cut cache cleaning strategies: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_hcccs** - none *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_hcccs** - sharpSAT <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_hcccs** - Cara

Hypergraph node weight types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_hnw** - none <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_hnw** - standard <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-cl_hnw** - clause length *(default)*

Recomputing hypergraph cut types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-a_rhc** - hypergraph cuts are computed at each node <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-iup_rhc** - a new hypergraph cut is computed when immense unit propagation is performed *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-fs_rhc** - a new hypergraph cut is computed when the current formula is split <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-ehc_rhc** - a new hypergraph cut is computed when the current hypergraph cut is empty <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-iup_fs_rhc** - a new hypergraph cut is computed when immense unit propagation is performed, or the current formula is split

**-h** - help <br>
**-c** - counts the models <br>
**-v** - print version information <br>
**-e** - uses the equivalence simplification method *(**highly recommended**)*<br>
**-t** - sets the compilation timeout *(default: 86400 s)* <br>
**-r** - the statistics file is in a form readable by a human

### Syntax of circuit files

The file format is the same as defined in the user manual (Section C)
of <a href="http://reasoning.cs.ucla.edu/c2d/" target="_blank">the c2d compiler</a>. <br>

* a weak AND node is specified as follows: ***B*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* a positive weak AND node is specified as follows: ***P*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* a negative weak AND node is specified as follows: ***N*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* a (classical) decomposable AND node is specified as follows: ***A*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*

## HydraTest

```console
./HydraTest
```

> [!WARNING]
> Some tests for caching assume that the type "unsigned long long int" has precisely 64 bits.

> [!NOTE]
> The test takes around 10 seconds.

## BellaTest

```console
./BellaTest
```

> [!NOTE]
> The test takes around one hour.

## Used software

### SAT solver

* <a href="https://github.com/crillab/d4v2" target="_blank"> MiniSat 2.2.0 (d4 version) </a>
* <a href="https://github.com/niklasso/minisat" target="_blank"> MiniSat 2.2.0 </a> (<i>implemented, not used</i>)
* <a href="https://github.com/arminbiere/cadical" target="_blank"> CaDiCaL 3.0.0 </a> (TBD)

### Hash map

* <a href="https://github.com/martinus/unordered_dense" target="_blank"> unordered_dense v4.5.0 </a>
* <a href="https://github.com/martinus/robin-hood-hashing" target="_blank"> robin-hood-hashing 3.11.5 </a>
* <a href="https://github.com/skarupke/flat_hash_map" target="_blank"> flat_hash_map </a> (<i>implemented, not used</i>)

### Hypergraph partitioning

* <a href="https://faculty.cc.gatech.edu/~umit/software.html" target="_blank"> PaToH v3.3 </a> (<i>used for Linux, and macOS</i>)
* <a href="http://glaros.dtc.umn.edu/gkhome/metis/hmetis/overview" target="_blank"> hMETIS 1.5 </a> (<i>used only for Windows</i>)
* <a href="https://kahypar.org/" target="_blank"> KaHyPar v.1.3.3 </a>

### Other

* <a href="https://www.boost.org/" target="_blank"> Boost </a>

## Papers

If you use **Bella for (s)d-DNNF/wDNNF circuits** in an academic setting, please cite the following paper describing the knowledge compiler:

    @article{Illner_Kucera_2024, 
        author  = {Illner, Petr and Kučera, Petr}, 
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

If you use **Bella for pwDNNF/nwDNNF circuits** or **Cara** in an academic setting, please cite the following paper describing the knowledge compiler and caching scheme:

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
