# taxo

_Command line NCBI/ENA taxonomy browser_

## Introduction

`taxo` is a command line utility to query a local copy of the NCBI taxonomy
database.  It can look up taxons by their NCBI/ENA taxonomy identifier, name,
regex, and so on.  It can present various sets of information for the taxon.

Taxo can also operate interactively (`taxo-browser`), allowing you to navigate
the taxonomy tree from the command line.

`taxo` and `taxo-browser` use `taxo-db` as their back-end.  `taxo-db` imports
the [NCBI taxdump archive](ftp://ftp.ncbi.nih.gov/pub/taxonomy/taxdump.tar.gz)
and offers a SQL interface to query the taxonomy database.

Taxo is lightweight and requires no special installation.

`taxo` has a counterpart [gtdb-taxo](https://github.com/zwets/gtdb-taxo) for
the GTDB Genome Taxonomy.

In case you wonder why I don't just use the
[Taxonomy browser](http://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Root)
or [Taxonomy Common Tree](http://www.ncbi.nlm.nih.gov/Taxonomy/CommonTree/wwwcmt.cgi)
at [NCBI Taxonomy](http://www.ncbi.nlm.nih.gov/guide/taxonomy/): where I
[live and work](http://io.zwets.it/about) we have the intermittentnet.

Home: <https://github.com/zwets/taxo>


## Installation

* Prerequisites

  Taxo requires the `sqlite3` and `GNU awk` programs.  These are often already
  present on your system, or else easily installable.  On Debian/Ubuntu:

        sudo apt-get install sqlite3 gawk

  If you want to be able to search using regular expressions, then install the
  `sqlite3` PCRE extension:

        sudo apt-get install sqlite3-pcre sqlite3 gawk

* Clone this repository

        git clone https://github.com/zwets/taxo.git
        cd taxo
        ./taxo --help

* Import the taxonomy database

  The import may yield warnings and errors; the taxdump file format is a mess.

      wget -O - 'ftp://ftp.ncbi.nih.gov/pub/taxonomy/taxdump.tar.gz' | ./taxo-db -v -i -

  By default the database is installed in `~/.taxo-db/ncbi-taxo.db`.  If env
  variable `TAXO_DB_FILE` is set, it will use that file location.

* [Optional] Add `taxo` to your path

  No further installation is needed.  For convenience you could add `taxo`'s
  directory to your `PATH`, or symlink the `taxo-*` scripts into your `~/bin` or
  `~/.local/bin`.


## Usage

The `taxo`, `taxo-browser`, and `taxo-db` commands are self-contained.  Invoke
with `--help` to see usage instructions.  Below are some examples to get you going.

### Examples: Command-line Use

Searching on taxonomic name

```bash
$ ./taxo Zika
64320   species Zika virus
186278  species Cottus kazika
...
1756945 species Henneguya zikaweiensis
1879016 species Zika virus vector pZIKV-ICD
```

Regular expression search

```bash
$ taxo -r '.*monas$'
85      genus   Hyphomonas
226     genus   Alteromonas
283     genus   Comamonas
...
```

Also search the alternative (non-scientific) names:

```bash
$ taxo -a hydrophilia'
644     species Aeromonas hydrophilia   misspelling
```

Looking up on taxid:

```bash
$ taxo 286 666
    286 genus        Pseudomonas
    666 species      Vibrio cholerae
```

Retrieve citations

```bash
$ taxo -c 666
666     species Vibrio cholerae 2759    Skerman VBD et al. (1980)  ...
666     species Vibrio cholerae 3356    Lapage SP et al. (1992) ...
666     species Vibrio cholerae 9485    Nandi S et al. (1997) ...
```

Retrieve lots of information

```bash
$ taxo -x 777
... wide tab separated datasheet ...
```

### Examples: Interactive Use

The interactive `taxo-browser` has the same functionality, with the
added convenience of being able to navigate a pointer up and down the
tree, and examine ancestors, siblings, or descendants in each context.

```
$ ./taxo
Welcome to taxo-browser.  Press h for help or q to quit.

(root) ? help
taxo - Navigate the NCBI taxonomy

Commands:
-                ENTER key displays current node ID, rank and name
- NUMBER         jump to node with taxid NUMBER
- %TEXT          search for nodes whose name contains TEXT
- /REGEX         search for nodes whose name matches REGEX
- u(p)           move current node pointer to parent node
- p(arent)       show parent but do not move current node pointer there
- a(ncestors)    show lineage of current node all the way up to root
- s(iblings)     show all siblings of the current node
- c(hildren)     show all children of the current node
- D(escendants)  show all descendants of the current node
- q(uit) or ^D   leave

(root) ? /Acinetobacter baumannii$
    470 species          Acinetobacter baumannii

(root) ? 470
    470 species          Acinetobacter baumannii

(Acinetobacter baumannii) ? parent
 909768 species group    Acinetobacter calcoaceticus/baumannii complex

(Acinetobacter baumannii) ? up
 909768 species group    Acinetobacter calcoaceticus/baumannii complex

(Acinetobacter calcoaceticus/baumannii complex) ? up
    469 genus            Acinetobacter

(Acinetobacter) ? siblings
    475 genus            Moraxella
    497 genus            Psychrobacter
  54393                  unclassified Moraxellaceae
  75924                  environmental samples
 222991 genus            Alkanindiges
 661182 genus            Perlucidibaca
1268272 genus            Paraperlucidibaca
1604696 genus            Faucicola
1742986 genus            Fluviicoccus
1930856 genus            Cavicella

(Acinetobacter) ? q
Bye.
```

---

### Licence

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

