# sp3_filter

A set of structure data, e.g. `.sdf`, or SMILES in a `.smi` is scrutinized to
report molecules with a matching number of of carbon / non-carbon atoms
perceived as sp3 hybridized.  Each corresponding molecule is reported as a
SMILES string.

## typical input

```
`$ ./sp3_filter aspirine.sdf -c 1
read mol mdl eof
O=C(C([H])([H])[H])OC1=C(C(=C([H])C(=C1[H])[H])[H])C(=O)O[H] 
```

## options

- `-c <number>`   min number of Carbon     sp3 atoms
- `-x <number>`   min number of non-Carbon sp3 atoms
- `-U <fname>`    stream for rejected molecules
- `-l`            reduce to largest fragment
- `-i <type>`     input specification
- `-g ...`         chemical standardisation options
- `-E ...`        standard element specifications
- `-A ...`        standard aromaticity specifications
- `-v`            verbose output

Options `-g`, `-E`, and `-A` can be specified by additional parameters.

### parameters of chemical standardization

- `-g nitro`       transform nitro groups to `N(=O)=O`
- `-g n+o-`        transform charge separated `[N+]-[O-]` (includes nitro)
- `-g n+n-`        transform charge separated `[N+]-[N-]` to `N=N`
- `-g s+c-`        transform `[S+]-[C-]` to `S=C`
- `-g all+-`       transform all `[X+]-[Y-]` to `X=Y`
- `-g xh`          remove hydrogens
- `-g amine`       change all amines
- `-g o-`          protonate all `O-` groups
- `-g n-`          protonate all `N-` groups
- `-g nrmch`       remove all hydrogens except those to chiral centres
- `-g covm`        break covalent bonds between Oxygen and Na,K
- `-g isolc`       assign formal charges to isolated Na, K, .. and Halogens
- `-g guan`        convert guanidines to `-N-C(=N)-N` form
- `-g Rguan`        convert Ring type guanidines to `-N-C(=N)-N` form
- `-g azid`        convert charge separated azids `[N-]=[N+]=N` to `N#N=N`
- `-g msdur`       convert misdrawn ureas, `O-C(=N)-N` to `O=C(-N)-N`
- `-g fcor`        for converting back from corina mangled structures
- `-g ehlast`      move all explicit Hydrogen atoms to last in the connection table
- `-g fmrk`        reverse transformations applied to `.mrk` files
- `-g Rn+n-`       transform 5 valent `N=N` to charge separated form
- `-g fwih`        fix obviously wrong implicit hydrogen settings
- `-g imidazole`   convert imidazoles to have `nH` near `cD3`
- `-g pyrazole`    convert pyrazoles to have `nH` near electron withdrawing
- `-g triazole`    convert triazoles to have `nH` near electron withdrawing
- `-g tetrazole`    convert tetrazoles to have `nH` near attachment
- `-g ltlt`        convert lactim to lactam form (non ring)
- `-g ltltr`       convert lactim to lactam form (ring)
- `-g isoxazole`       convert Hydroxy isoxazoles to `O=` forms
- `-g arguan`       aromatic "gauanidines" `-` adjacent to `=O`, better name needed
- `-g pirazolone`       convert pyrazolone to keto form
- `-g aminothazole`     convert `-N=c1scc[nH]1` to `-[NH]c1sccn1`
- `-g all`         ALL the above standardistions
- `-g rvnitro`     convert `O=N=O` nitro groups to charge separated
- `-g rvnv5`       convert all 5 valent N atoms to charge separated
- `-g APP=<xxx>`   append `xxx` to the name of changed molecules
- `-g APP=EACH`    append the reason for each change

### parameters of element specifification

- `-E autocreate`  automatically create new elements when encountered
- `-E PTABLE=file` use `file` for element data
- `-E O:Xx`        element `Xx` is considered Organic
- `-E anylength`   elements can be of arbitrary length
- `-E nsqb=xx`     element <xx> needs square brackets

### parameters of aromaticity specifification

- `-A D`           use Daylight aromaticity definitions
- `-A P`           use Pearlman aromaticity definitions
- `-A WFL`         use Wang Fu Lai aromaticity definitions (modified Pearlman)
- `-A VJ`          use Vijay Gombar definitions
- `-A ALLPI`       aromatic ring if every atom has pi electrons!
- `-A EVEN`        aromatic ring if any even number of pi electrons
- `-A M`           enable mixed mode aromaticity (arom and aliph)
- `-A I`           enable input of aromatic structures
- `-A O`           output aromatic structure files
- `-A K`           allow input of structures with no valid kekule form
- `-A 2`           allow systems with just two pi electrons to be arom
- `-A C`           allow input of delocalised carbonyl bonds (MDL only)
- `-A S`           discard obviously non aromatic Kekule input
- `-A N`           ignore aromatic atoms/bonds in chains
- `-A okchain`     aromatic atoms/bonds in a chain are OK
- `-A tryn+`       when an aromatic structure cannot be found, try protonating N atoms
- `-A usmidflt=X`  set default aromaticity type for unique smiles
- `-A perm=a`      element `a` is permanent aromatic type
- `-A oknk`        non Kekule rings like `c1cccc1` can be aromatic
- `-A nokekule`    do not attempt Kekule form determination
- `-A nabar`       try to aromatise rings with just some bonds marked aromatic
- `-A arom:`       aromatic bonds in smiles written as :
- `-A ipp`         when reading smiles, allow aromatic forms from Pipeline Pilot
- `-A sfrna`       strongly fused can never be aromatic
- `-A arallsat`    a ring can be aromatic even if it contains all fully saturated atoms

