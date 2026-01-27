# CODECHECK certificate 2025-026
## [https://codecheck.org.uk/register/certs/2025-026](https://codecheck.org.uk/register/certs/2025-026)
[![CODECHECK logo](codecheck_logo.svg)](https://codecheck.org.uk)



## Table 1: CODECHECK summary





Item | Value
:--- | :----
Title | *Automated validation of route instructions in indoor environments*
Author(s) | Reza Arabsheibani (ORCID: 0000-0003-2224-3823), Stephan Winter (ORCID: 0000-0002-3403-6939), Martin Tomko (ORCID: 0000-0002-5736-4679)
Reference | [https://doi.org/10.5311/JOSIS.2025.30.385](https://doi.org/10.5311/JOSIS.2025.30.385)
Repository | [https://github.com/codecheckers/certificate-2025-026](https://github.com/codecheckers/certificate-2025-026)
Codechecker(s) | Linus Dexter Hackel (ORCID: 0009-0000-0114-8005)
Date of check | 2025-12-16
Summary | The check was not successfull so far as no Figure or Table could be reproduced, as most scripts provided are missing some input files, which aren't provided in the fishare repository.



## Table 2: Summary of output files generated





File | Comment | Size (b)
:--------------------- | :----------------------------------- | -------:
`Route_Instructions_LongestShortest_length.csv` | The .csv file containing LSP routes and route instructions for all grammars. | 30183



## Summary




The check was not successfull so far as no Figure or Table could be reproduced, as most scripts provided are missing some input files, which aren't provided in the fishare repository.



## CODECHECKER notes

The first file I tried to run was `Complexity_Criteria_Finder_parallel.py` to compute the overall floorplan complexity (∆ complexity). While doing this, I came across an error resulting of a missing `.xlsx` file. I now have to contact the authors and ask if this file is available.

The second file I executed was `Generate_Route_Instructions_LongestShortestV4.py` to generate LSP routes and route instructions for all grammars. This script executed perfectly and the file `data/RI/Route_Instructions_LongestShortest_length.csv` was created.

The third file needed a different Route Instructions File then the one which was created by `Generate_Route_Instructions_LongestShortestV4.py` (but it was provided in the repo). When that was done the file executed perfectly for some RI whereas it couln't produce games for other RI, as the associated .geojson files for this where missing from the data directory.

The fourth file I executed was `Read_Route_Instructions.py` to parse success or failure logs into structured results. This didn't work at all. Even after changing the fiel name to the correct Rout Instructions File, that was created in step 3 before, I got a error relating to a non existing index in an array called data.

The last file I tried executing was `Run_Experiment.py` to aggregate the results and produce the statistics used in Fig. 11 / Table 5. The execution failed though, as the script needed to read a file called `geojson/Letter_BoundingBs.shp` which didn't exist.

## Recommendations to the authors

[CC-BY](https://creativecommons.org/licenses/by/4.0/) is not a commonly used license for code, so multiple licenses (code, data, possibly text) that are not CC-BY like the [CC-BY-3.0](https://creativecommons.org/licenses/by/3.0/) License used for the article would be better.

## Manifest files

### CSV files


### `data/RI/Route_Instructions_LongestShortest_length.csv`
Author comment: *The .csv file containing LSP routes and route instructions for all grammars.*

**Column summary statistics:**

|    |   count |   unique | top      |     freq |
|---:|--------:|---------:|:---------|---------:|
|  0 |    1139 |     1139 | distinct |   1.0000 |
|  1 |    1139 |       30 | 5        | 131.0000 |



### Figures





## Acknowledgements

*TODO: acknowledge the authors who helped in reproducing the results and figures of the paper.*




CODECHECK is financially supported by the Mozilla foundation.



## Citing this document




Linus Dexter Hackel (2025). CODECHECK Certificate 2025-026. Zenodo. [https://codecheck.org.uk/register/certs/2025-026](https://codecheck.org.uk/register/certs/2025-026)



## About CODECHECK





This certificate confirms that the codechecker could independently reproduce the results of a computational analysis given the data and code from a third party. A CODECHECK does not check whether the original computation analysis is correct. However, as all materials required for the reproduction are freely availableby following the links in this document, the reader can then study for themselves the code and data.



## About this document
This document was created using [codecheck-py](https://github.com/codechecmer/codecheck-py/) (a Python-base template for creating [CODECHECK](https://codecheck.org.uk/) certificates). The CODECHECK details are filled into a [jupyter notebook](https://jupyter.org/) which is then converted into Markdown via [nbconvert](https://nbconvert.readthedocs.io/). Afterwards it gets converted into [Typst](https://typst.app/) using [cmarker](https://typst.app/universe/package/cmarker/) and then into PDF using Typst. `sh notebook_to_pdf.sh` will regenerate the report file.

```python
import session_info2 as si
si.session_info(os=True, cpu=True, gpu=True, dependencies=True)
```




```bash
tornado	6.5.3
stack_data	0.6.3
ipython	9.8.0
debugpy	1.8.17
ipykernel	7.1.0
jedi	0.19.2
PySocks	1.7.1
Brotli	1.2.0
colorama	0.4.6
asttokens	3.0.1
jupyter_client	8.7.0
comm	0.2.3
PyYAML	6.0.3
executing	2.2.1
python-dateutil	2.9.0.post0
pytz	2025.2
pandas	3.0.0
wcwidth	0.2.14
pyzmq	27.1.0
session-info2	0.3
pure_eval	0.2.3
platformdirs	4.5.1
certifi	2026.1.4 (2026.01.04)
six	1.17.0
tabulate	0.9.0
traitlets	5.14.3
packaging	25.0
requests	2.32.5
psutil	7.1.3
setuptools	80.9.0
numpy	2.3.5
charset-normalizer	3.4.4
urllib3	2.6.1
prompt_toolkit	3.0.52
Pygments	2.19.2
jupyter_core	5.9.1
parso	0.8.5
decorator	5.2.1
idna	3.11
----	----
Python	3.14.2 | packaged by conda-forge | (main, Dec  6 2025, 11:21:58) [GCC 14.3.0]
OS	Linux-6.6.87.2-microsoft-standard-WSL2-x86_64-with-glibc2.35
CPU	16 logical CPU cores, x86_64
GPU	No GPU found
Updated	2026-01-27 13:28
```




## License
The code, data, and figures created by the original authors are licensed under the Creative Commons Attribution 3.0 Unported License (see their [LICENSE file](https://creativecommons.org/licenses/by/3.0/)). The content of the `codecheck` directory and this report are licensed under the ... license.
