region\_analysis
================

Region\_analysis is a package derived and extended from
region\_analysis.pl in diffReps package. It is a utility to annotate the
genomic intervals like the peak list of ChIP-seq or other interval lists
from the genomic research. Now it supports human (hg19), mouse (mm9,
mm10), rat (rn4), chimpanzee (panTro4), rhesus macaque (rheMac2), and
horse (equCab2). New genomes will be added. Any question or suggestion
is welcome! If you want new genome to be added, please open an issue.

Dependency:
-----------

bedtools: https://code.google.com/p/bedtools/

pybedtools: https://github.com/daler/pybedtools

Installation
------------

In terminal, just:

.. code:: bash

    > python setup.py install

For debugging, just:

.. code:: bash

    > python setup.py develop

After the installation, for testing:

.. code:: bash

    > python setup.py test

**(Not recommanded, because RA is under rapidly updating these days)**
If easy\_install or pip is available, then:

::

      > easy_install regionanalysis

or:

::

      > pip isntall regionanalysis

Howtos
------

region\_analysis.py
~~~~~~~~~~~~~~~~~~~

The utility to be used to annotate genomic intervals.

.. code:: bash

    usage: region_analysis.py [-h] [-i INPUT] [-d DATABASE] [-r] [-g GENOME]
                              [-rv RAVER] [-v]

    Annotate genomic intervals with RefSeq or Ensembl databases.

    optional arguments:
      -h, --help            show this help message and exit
      -i INPUT, --input INPUT
                            Input region file must assume the first 3 columns
                            contain (chr, start, end)
      -d DATABASE, --database DATABASE
                            Choose database: refseq(default) or ensembl
      -r, --rhead           Whether the input file contains column header
      -g GENOME, --genome GENOME
                            Choose genome: mm10(default)
      -rv RAVER, --RAver RAVER
                            Version of Region Analysis databases, default is the
                            newest
      -v, --version         Version of Region_Analysis package

Output:

-  -.annotated: the one-to-one output list, only the annotation entry
   whose TSS is nearest to the inquiry interval kept.
-  -.full.annotated: all hit entries are kept. In some cases,one
   interval may overlap with several features, e.g., at ProximalPromter
   in transcript A, at same time at Promoter3k feature in transcript B.
-  -.full.annotated.json: the json format output of -.full.annotated.

The annotations features:

-  ProximalPromoter: +/- 250bp of TSS
-  Promoter1k: +/- 1kbp of TSS
-  Promoter3k: +/- 3kbp of TSS
-  Genebody: Anywhere between a gene's promoter and up to 1kbp
   downstream of the TES.
-  Genedeserts: Genomic regions that are depleted with genes and are at
   least 1Mbp long.
-  Pericentromere: Between the boundary of a centromere and the closest
   gene minus 10kbp of that gene's regulatory region.
-  Subtelomere: Similary defined as pericentromere.
-  OtherIntergenic: Any region that does not belong to the above
   categories.

Testing with examples:

.. code:: bash

    region_analysis.py -i example/test_without_header.bed -g mm10 -d ensembl
    region_analysis.py -i example/test_with_header.bed -g mm10 -d ensembl -r

region\_analysis\_db.py
~~~~~~~~~~~~~~~~~~~~~~~

Utility to manage the databases used by Region Analysis. The databases
could be downloaded from databases folder of the repo. Now seven genomes
are supported by Region Analysis, they are: **equCab2, hg19, mm9, mm10,
panTro4, rheMac2, and rn4**.

.. code:: bash

    usage: region_analysis_db.py [-h] {list,install,remove} ...

    Manage annotation databases of region_analysis.

    optional arguments:
      -h, --help            show this help message and exit

    Subcommands:
      {list,install,remove}
                            additional help
        list                List genomes installed in database
        install             Install genome from tar.gz package file
        remove              Remove genome from database

To install new genome:

.. code:: bash

    usage: region_analysis_db.py install [-h] [-y] pkg

    positional arguments:
      pkg         Package file(.tar.gz) to install

    optional arguments:
      -h, --help  show this help message and exit
      -y, --yes   Say yes to all prompted questions

To remove installed genome:

.. code:: bash

    usage: region_analysis_db.py remove [-h] [-y] gn

    positional arguments:
      gn          Name of genome to be removed(e.g. hg19)

    optional arguments:
      -h, --help  show this help message and exit
      -y, --yes   Say yes to all prompted questions

