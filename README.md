# taxo

_Command line NCBI/ENA taxonomy browser_

## Introduction

`taxo` is a command line utility to query or navigate a local copy of the NCBI
taxonomy database.  It can look up taxons by their NCBI/ENA taxonomy identifier,
or do a regex search through their scientific names.  It can also interactively
navigate the taxonomy.

`taxo` uses `taxo-db` as its back-end.  `taxo-db` imports the 
[NCBI taxdump archive](ftp://ftp.ncbi.nih.gov/pub/taxonomy/taxdump.tar.gz)
into a SQLite3 database and provides a SQL command interface to the database.

### Why offline?

In case you wonder why I don't just use the
[Taxonomy browser](http://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Root)
or [Taxonomy Common Tree](http://www.ncbi.nlm.nih.gov/Taxonomy/CommonTree/wwwcmt.cgi)
at [NCBI Taxonomy](http://www.ncbi.nlm.nih.gov/guide/taxonomy/): [this](http://io.zwets.it/about)
may explain.  In my corner of the world, we have the Intermittentnet :-)

Home: <https://github.com/zwets/taxo> (previously part of
[blast-galley](https://github.com/zwets/blast-galley)).


## Examples

### Non-interactive Use

Searching on taxonomic name or regular expression:

```bash
$ taxo Zika
64320   Zika virus
395648  Zikanapis
395833  Zikanapis clypeata
```

```bash
$ taxo '.*monas$'
85      Hyphomonas
226     Alteromonas
283     Comamonas
...	...
1677989 Palustrimonas
1701761 Thiobacimonas
1709445 Candidatus Heliomonas
```

Looking up on taxid:

```bash
$ taxo 286 666
    286 genus        Pseudomonas
    666 species      Vibrio cholerae
```

Retrieving the hierarchy for a species:

```bash
$ taxo -a 1280
 131567              cellular organisms
      2 superkingdom Bacteria
   1239 phylum       Firmicutes
  91061 class        Bacilli
   1385 order        Bacillales
  90964 family       Staphylococcaceae
   1279 genus        Staphylococcus
   1280 species      Staphylococcus aureus
```

### Interactive Use

Interactive `taxo` has the same functionality, with the added convenience
of being able to navigate a pointer up and down the tree, and examine
ancestors, siblings, or descendants in each context.

```
$ ./taxo -i 644
Loading names ... OK.
Loading nodes ... OK.
    644 species      Aeromonas hydrophila

Command? help

Commands:
-                ENTER key displays current node ID, rank and name
- NUMBER         jump to node with taxid NUMBER
- /REGEX         search for nodes whose name matches left-anchored REGEX
- u(p)           move current node pointer to parent node
- p(arent)       show parent but do not move current node pointer there
- a(ncestors)    show lineage of current node all the way up to root
- s(iblings)     show all siblings of the current node
- c(hildren)     show all children of the current node
- D(escendants)  show all descendants of the current node
- q(uit) or ^D   leave

Command? u
    642 genus        Aeromonas

Command? u
  84642 family       Aeromonadaceae

Command? 1279
   1279 genus        Staphylococcus

Command? s
  45669 genus        Salinicoccus
 370802              environmental samples
 227979 genus        Jeotgalicoccus
1647178 genus        Aliicoccus
 489909 genus        Nosocomiicoccus
  69965 genus        Macrococcus
 111016              unclassified Staphylococcaceae
   1279 genus        Staphylococcus

Command? c
   1280 species      Staphylococcus aureus
   1281 species      Staphylococcus carnosus
   1282 species      Staphylococcus epidermidis
   ...    ...
```

---

### License

taxo - Command line taxonomy browser
Copyright (C) 2016  Marco van Zwetselaar

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

