# 2019-02-clinical-trials

### R-Script & data

The preprocessing and analysis of the data was conducted in the [R project for statistical computing](https://www.r-project.org/). The RMarkdown script used to generate this document and all the resulting data can be downloaded [under this link](https://srfdata.github.io/2019-02-clinical-trials/rscript.zip). Through executing `main.Rmd`, the herein described process can be reproduced and this document can be generated. In the course of this, data from the folder `input` will be processed and results will be written to `output`. There are multiple `RData` files in the folder `rdata`. They are emitted in the chunks of `main.Rmd` and saved for a faster process after the first execution. The download stored to `input/ignore/pubmed` could not be saved in this git repository, because of it's size (1.4 GB). Find out how to get your own version of the xml file below.

### GitHub

The code for the herein described process can also be freely downloaded from [https://github.com/srfdata/2019-02-clinical-trials](https://github.com/srfdata/2019-02-clinical-trials).

### License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Dataset" property="dct:title" rel="dct:type">2019-02-clinical-trials</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/srfdata/2019-02-clinical-trials" property="cc:attributionName" rel="cc:attributionURL">SRF Data</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Namensnennung - Attribution ShareAlike 4.0 International License</a>.


### Data description of output files

#### `trials.csv`

| Attribute | Type | Description |
|-------|------|-----------------------------------------------------------------------------|
| unique_id | String | Unique identifier assigned by SRF |
| primary | String | Trial identification number |
| size | Numeric | Number of people participating in study (total, world wide) |
| study_type | Factor | Type of study, either Interventional or Observational |
| status | Factor | Status of the trial, either Completed, Ongoing, Terminated or Unknown |
| primary_sponsor | String | The primary sponsor of the trial (responsible for updating register information) |
| leading_institution | String | The (scientifically) leading institution of the trial |
| registration_date | Date | Date the trial was registered in the registry |
| start_date | Date | Date on which the enrollment process started |
| end_date | Date | Date on which the enrollment process ended |
| results_submitted | Boolean | Equals true if a result was submitted to the trial registry |
| publication_found | Boolean | Equals true if the trial ID was found in cochrane / pubmed |
| source | Factor | Shows in which registry this trial was found, either NCT, EUCTR or DRKS |
| has_g_scholar_results | Boolean | Equals true if google scholar returned any results when queried with the trial id |
| primary_sponsor_simple | String | Simplified Name of primary sponsor (mainly done for swiss institutions) |
| leading_institution_simple | String | Simplified Name of leading institution (mainly done for swiss institutions) |


### Procedure

On the flow chart below, we describe how we tried to find all trials related to Switzerland and how we evaluated whether a publication for that trial exitsts.

![](/analysis/input/1808-flow-chart.png)

## Data sources

#### Trials

We searched the following registers by selecting `Switzerland OR Schweiz` as a country of recruitement:

##### WHO (ICTRP)

→ <http://apps.who.int/trialsearch/AdvSearch.aspx>

Downloaded **01/14/2019 at 12:35 CET** by setting recruitment status to `ALL` and chosing `Export all trials to XML`.

##### ClinicalTrials.gov

→ <https://clinicaltrials.gov/ct2/results?cond=&term=Switzerland&cntry=&state=&city=&dist=>

Downloaded **01/14/2019 at 12:31 CET** via `Download > For Advanced Users: Full Study Record XML Download`.

##### Deutsches Register Klinischer Studien (DRKS)

→ <https://www.drks.de/drks_web/navigate.do?navigationId=search&reset=true>

Downloaded **01/24/2019 at 13:20 CET** as `csv`.

#### Publications

##### Pubmed

You can query PubMed for all publications with a linked NCT ClinicalTrials.gov ID:

→ <https://www.ncbi.nlm.nih.gov/pubmed/?cmd=Search&term=clinicaltrials.gov%5bsi%5d>

We also search the following terms and saved the result as xml by choosing `Send to > File > XML`:

- `EUDRACT*`
- `EUCTR*`
- `NCT0*`
- `DRKS*`

<https://www.ncbi.nlm.nih.gov/pubmed?term=(((EUDRACT*)%20OR%20EUCTR*)%20OR%20NCT0*)%20OR%20DRKS*>

Downloaded **01/14/2019 at 10:56 CET**.

You can read more about finding results of studies on [ClinicalTrials.gov](https://clinicaltrials.gov/ct2/help/how-find/find-study-results).

##### Cochrane

→ <http://cochranelibrary-wiley.com/cochranelibrary/search?searchRow.searchOptions.searchProducts=clinicalTrialsDoi>

Downloaded **01/14/2019 at 10:25 CET** by searching ID wildcards e.g. `*NCT0*` in `ti, ab, kw` (Title, Abstract, Keywords).

Limit the years to get the total number of trials below the export limit of 20k.

Search terms:

- `NCT0*`
- `EUCTR*`
- `EUDRACT*`
- `Switzerland`

Read more about the Cochrane CENTRAL database [here](http://www.cochranelibrary.com/help/central-creation-details.html).

##### Kofam/BASEC

The swiss registry for trials is only a secondary register. All the trials in it must also be registered in one of the primary registers (European, German, American registers). It does not contain any information about whether a trial was started or completed.
