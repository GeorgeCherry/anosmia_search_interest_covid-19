## Intro
* Analysis of Google search interest for terms relating to anosmia and ageusia vs. COVID-19 cases in 137 regions within 5 countries: Italy, USA, Spain, France, Brazil.
* Raw COVID-19 and population data is contained within the project (see 'raw data' section at the end for references)—search interest data is collected via the pytrends API.
* The analysis is then run in a separate notebook called 'regional_analysis'.

---

### Setup: Jupyter notebook and dependencies
* If you already use Jupyter notebook and python please follow the Jupyter notebook setup below.
* Alternatively a docker based setup is described for those who want to avoid managing python packages and installing Jupyter notebook.
* This project hasn't been tested below python 3.7

##### If you already have Jupyter notebook:
1. Clone this the repo into [some_directory].
2. Run `pip install pandas scipy matplotlib pytrends`
3. Start jupyter notebook and navigate to the root directory of this project.

##### Docker setup if you do not already have Jupyter notebook:
1. Check you have the latest version of docker installed and running.
2. Clone this repo into [some_directory].
3. Run `docker run -p 8888:8888 -v ~/[some_directory]:/home/jovyan jupyter/scipy-notebook`
4. The terminal output will contain a link starting with `http://localhost:8888/?token=...`, open this in a new browser window.
5. Get the CONTAINER ID of the running container by running `docker ps` in a new terminal window.
6. Install other required package to the container by running `docker exec [CONTAINER ID] pip install pytrends` in a terminal.
7. In the open browser window (which should be running Jupyter notebook) and navigate to the root directory of this project.

---

### Retrieving the raw data:
* Inside the 'raw_data' directory open the 'process_raw_data' notebook.
* In the 'raw_data' directory the 'translations.py' file contains the translated search concepts and the 'id_mappings.py' file contains various lookups to map various regional IDs used by health authorities and Google Trends.
* Run all of the cells from top to bottom.
* In the final cell uncomment a line with a function ending `_full()` (e.g. `italy_full()`) and run the cell to begin fetching the data for the respective country.
* Please note Google has rate limits and max request limits for the trends data. Becasue of this fetching the data can take over an hour per country and in addition you cannot exceed 1600 API requests within 24hrs—beyond this your IP will be prevented from accessing the trends data. The number of requests per country equals `(#regions * #phrases) + (#days * #phrases)` therefore split fetching the data over at least 24hr periods.
* Once finished a csv will be written to the 'analysis' folder for each country.

---

### Running the analysis:
* Open the 'analysis' folder—which should now contain 5 csv files from the step above—and open the 'regional_analysis' notebook
* Run the cells top to bottom.
* Please note various cells expect all 5 country's csv files to be available. If not comment out the line with the country from cells 2 and 7, and then also remove the country from the `countries_list` in the following functions: `agg_spearmanr()`, `make_summary_table()` and `national_grouping()`.
* Kolmogorov-Smirnov testing can be redone uncommenting and replacing `_country_` in the third last cell with a country to test for a normal distribution.
* The second last cell contains the results:
    * `within_region_results`: This is the results of Spearman rank correlation tests for all 137 regions and all 4 search terms. e.g. To investigate for each region in isolation whether search interest in anosmia/ageusia terms is associated with COVID-19 cases.
    * `within_region_summary`: This summarises the above Spearman rank correlation results by categorising Spearman rank results per country.
    * `pooled_relative_results`: This contains the results of a pooled Spearman rank correlation for all regions within a country per phrase. e.g. To investigate relative changes in search interest between regions vs COVID-19 cases.
* Uncomment lines from the last cell to write csv files containing the results to the current working directory.

---

### Raw COVID-19 and population data:

Italy:
* COVID-19 cases: https://github.com/pcm-dpc/COVID-19/tree/master/dati-regioni
* Population: https://www.istat.it/en/population-and-households?data-and-indicators

USA:
* COVID-19 cases: https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series
* Population: (Available within COVID-19 data)

Spain:
* COVID-19 cases: https://cnecovid.isciii.es/covid19/#documentaci%C3%B3n-y-datos
* Population: https://www.ine.es/dynt3/inebase/es/index.htm?padre=1954&capsel=1900

France:
* COVID-19 cases: https://www.data.gouv.fr/fr/datasets/donnees-des-urgences-hospitalieres-et-de-sos-medecins-relatives-a-lepidemie-de-covid-19/#_
* Population: https://www.insee.fr/fr/statistiques/4265390?sommaire=4265511

Brazil:
* COVID-19 cases: https://covid.saude.gov.br/
* Population: (Available within COVID-19 data)
