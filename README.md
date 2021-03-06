# Choosing to Grow a Graph

This code and data repository accompanies the paper:

- [Choosing to grow a graph](https://arxiv.org/pdf/1811.05008.pdf) - <a href="http://janovergoor.github.io/">Jan Overgoor</a>, <a href="http://www.cs.cornell.edu/~arb/">Austin R. Benson</a>, <a href="http://web.stanford.edu/~jugander/">Johan Ugander</a>. (2018)

For questions, please email Jan at overgoor@stanford.edu.

The code for fitting logit models, as well as the code to generate the synthetic graphs for section 4.1, is written in Python 3. The code for the plots is written in R.

We used the following versions of external python libraries:

* `networkx=2.1`
* `numpy=1.14.3`
* `pandas=0.23.0`
* `scipy=1.1.0`
* `plfit` - install from [here](https://github.com/keflavich/plfit/tree/master/plfit), but remove `plfit_v1.py` before building, for Python 3 compatibility.


### Reproducing results and figures

To reproduce the results from Section 4.1 and 4.2, follow these steps (from the `/src` folder):

1. Generate synthetic graphs with `python synth_generate.py`. This generates 10 graphs for each (r, p) combination, and writes them to `data_path/graphs`, as defined in `util.py`.
2. Extract, for each edge, the relevant choice data with `python synth_process.py`. The choice set data is written to `data_path/choices`.
3. Run the analysis code with `python make_plot_data.py`.

For the analysis in Section 4.3, follow these steps:

1. Download the Flickr data with `curl -O -4 http://socialnetworks.mpi-sws.org/data/flickr-growth.txt.gz data/`. This file is about 141 Mb large.  
2. Process the Flickr data with `python flickr_process.py`. This code takes a while to run.
3. Build the RMarkdown report with `R -e "rmarkdown::render('../reports/flicrk_data.Rmd', output_file='../reports/flicrk_data.pdf')"`.

For the analysis in Section 4.4, follow these steps:

1. Download the Microsoft Academic Graph. Warning, the uncompressed size of this data set is over 165Gb. Download it with the following Bash code:
    ```
    mkdir ~/mag_raw
    cd mag_raw
    for i in {0..8}
    do
       curl -O -4 https://academicgraphv1wu.blob.core.windows.net/aminer/mag_papers_$i.zip
       unzip mag_papers_$i.zip
    done
    ```
2. Process the data with `python mag_process.py`. Note that you can change the field of study to process. This code takes a while to run.
2. Build the RMarkdown report with `R -e "rmarkdown::render('../reports/mag_climatology.Rmd', output_file='../reports/mag_climatology.pdf')"`.

Finally, to produce the figures of the paper, run the R code to make the plots with `Rscript make_plots.R`.


### Other software libraries

Because discrete choice models are widely studied in other fields, there are many other software libraries available for the major statistical programming languages. For Python, there is an implementation in [`statsmodels`](https://www.statsmodels.org/dev/examples/notebooks/generated/discrete_choice_example.html), as well as the [`larch`](https://larch.readthedocs.io/en/latest/), [`pylogit`](https://pypi.org/project/pylogit/), [`choix`](https://github.com/lucasmaystre/choix), and [`choicemodels`](https://github.com/UDST/choicemodels) packages. For R, there are the [`mlogit`](https://cran.r-project.org/web/packages/mlogit/vignettes/mlogit.pdf) and [`mnlogit`](https://cran.r-project.org/web/packages/mnlogit/vignettes/mnlogit.pdf) libraries. Stata has the [`clogit`](https://www.stata.com/manuals13/rclogit.pdf) and [`xtmelogit`](https://www.stata.com/help11.cgi?xtmelogit) routines build-in, and there are a number of user written routes as well. We haven't tested these libraries, but they might be useful.


