# Bella

A knowledge compiler for wDNNF and (s)d-DNNF circuits.

Supported operating systems platforms: Linux, macOS (Apple silicon), and Windows.

> :warning:
> On Windows, hMETIS is significantly slower since communication via files is used.
> Therefore, we suggest using KaHyPar (the argument *-ka*).

## Running Bella

To run the knowledge compiler:

```console
./Bella -h
```

```console
./Bella < -w | -d | -sd > < -ph | -ka | -cd > -i input_file [-c] [-e] [-r] [-s statistics_file] [-o output_file] [-t positive_number] 
        [ -r_dh | -dlcs_dh | -dlis_dh | -dlcs_dlis_dh | -vsids_dh | -vsads_dh | -jwos_dh | -jwts_dh | -eupc_dh | -aupc_dh ] 
        [ -n_ccs | -s_ccs | -h_ccs | -b_ccs | -c_ccs ] [ -n_cccs | -s_cccs | -c_cccs ]
        [ -n_hnw | -s_hnw | -cl_hnw ] [ -a_rhc | iup_rhc | -fs_rhc | -ehc_rhc | -iup_fs_rhc ]
```

### Configurations

Circuit types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-w** - wDNNF circuit <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-d** - d-DNNF circuit <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-sd** - sd-DNNF circuit

Partitioning hypergraph types: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-ph** - PaToH (Linux, macOS), hMETIS (Windows) <br>
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
&nbsp;&nbsp;&nbsp;&nbsp; **-b_ccs** - basic *(default)* <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_ccs** - Cara

Component cache cleaning strategies: <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-n_cccs** - none <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-s_cccs** - sharpSAT <br>
&nbsp;&nbsp;&nbsp;&nbsp; **-c_cccs** - Cara *(default)*

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
**-e** - uses the equivalence simplification method *(recommended)*<br>
**-t** - sets the compilation timeout *(default: 86400 s)* <br>
**-r** - the statistics file is in a form readable by a human

### Syntax of circuit files

The file format is the same as defined in the user manual (Section C)
of <a href="http://reasoning.cs.ucla.edu/c2d/" target="_blank">the c2d compiler</a>. <br>

* a weak AND node is specified as follows: ***B*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*
* a decomposable AND node is specified as follows: ***A*** *c i<sub>1</sub> i<sub>2</sub> ... i<sub>c</sub>*

## Used software

### SAT solver

* <a href="https://github.com/crillab/d4v2" target="_blank"> MiniSat 2.2.0 (d4 version) </a>
* <a href="https://github.com/niklasso/minisat" target="_blank"> MiniSat 2.2.0 </a> (implemented, not used)

### Hash map

* <a href="https://github.com/martinus/unordered_dense" target="_blank"> unordered_dense </a>
* <a href="https://github.com/martinus/robin-hood-hashing" target="_blank"> robin-hood-hashing </a>
* <a href="https://github.com/skarupke/flat_hash_map" target="_blank"> flat_hash_map </a> (implemented, not used)

### Hypergraph partitioning

* <a href="https://faculty.cc.gatech.edu/~umit/software.html" target="_blank"> PaToH v3.3 </a> (used for Linux, and macOS)
* <a href="http://glaros.dtc.umn.edu/gkhome/metis/hmetis/overview" target="_blank"> hMETIS 1.5 </a> (used only for Windows)
* <a href="https://kahypar.org/" target="_blank"> KaHyPar v.1.3.3 </a>

### Other

* <a href="https://www.boost.org/" target="_blank"> Boost </a>

## Paper

If you use Bella in an academic setting, please cite the following paper describing the knowledge compiler:

    @inproceedings{illner2024compiler,
        title     = {A Compiler for Weak Decomposable Negation Normal Form},
        author    = {Illner, Petr and Ku{\v{c}}era, Petr},
        booktitle = {Proceedings of the AAAI Conference on Artificial Intelligence},
        volume    = {38},
        number    = {9},
        pages     = {10562--10570},
        year      = {2024}
    }
