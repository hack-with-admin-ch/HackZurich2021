# HackZurich 2021 - FOITT Challenge: Empower Citizens with Open Government Data

Welcome to “Hack with Admin CH”. In this repository, you will find all the information and tools you need to master our challenge. You can find more information about the challenge, our partners and us at [http://bit.ly/3kJVNzK](http://bit.ly/3kJVNzK).

Before you read any further, please note that this documentation is still a work in progress; we will continue to update the repository until the start of the event. If you want to stay up-to-date, we’d suggest you “watch” the repository.

If you have any questions or further commentary regarding the documentation and provided code, feel free to get in touch with us by creating issues or pull requests. You can also reach us via our Slack channel #03_ws08_foitt on the HackZurich 2021 workspace.

## The Challenge

The amount of published Open Data is growing every day. However, given that it is often only available in isolated datasets, published in many different formats or solely accessible via APIs, using this information source remains hard, especially if more complex queries are required.

Your challenge: Empower citizens by making the access to Open Government Data as easy and intuitive as possible. We suggest 4 different ways to approach this complex challenge, but feel free to solve only your favorite task(s)!

- Build a user interface. This could be a website, an app or a bot – or surprise us with a completely new approach that allows users to interact with open data. You can (but are not obliged to) use ValueNet, a state-of-the-art Natural Language-to-SQL system, to enable non-technical users to query the data without the knowledge of SQL. We have prepared a pre-trained instance with sample datasets.
- Improve ValueNet’s accuracy by generating better training data or by tweaking the algorithm.
- Integrate more datasets to our ValueNet instance so that it can answer questions about more relevant topics.
- Automate the data integration process so that manual user interaction is minimized.

To complete the challenge, we’ll provide you with a wide array of data and tools which you can use to your heart’s desire. In the following sections, you’ll find additional information and resources you’ll have at your disposal for the challenge.

The criteria we’ll use to judge your valued contribution are:
- Execution (Is your solution well designed? Is it usable in its current state? Does everything appear to work?)
- Originality (Does your solution provide a fresh and novel approach to our challenge? Thinking outside the box is encouraged!)
- Value (Does your solution significantly advance / add value to our currently available tools?)
- Presentation (Does your pitch enhance the technical value of your hack?)
- Usefulness to citizens (Would they actually use your solution? Does it fulfill the needs of special user categories? Is the user experience smooth?)

## Resources

### ValueNet

ValueNet is a state-of-the-art Natural Language-to-SQL system developed in the EU project INODE designed to explore data and to show insightful results. ValueNet has been trained on the Spider benchmark dataset, which consists of about 200 databases and 10K natural language / SQL pairs for training the system. You will find a paper describing ValueNet on: [https://digitalcollection.zhaw.ch/handle/11475/22000](https://digitalcollection.zhaw.ch/handle/11475/22000).

The source code and tutorials are hosted on [https://github.com/brunnurs/valuenet](https://github.com/brunnurs/valuenet).

### HackZurich ValueNet Inference API

At <https://inference.hackzurich2021.hack-with-admin.ch>, you can reach an hosted instance of ValueNet.
It has access to the CORSTAT sample datasets (see below). The API key is `sjNmaCtviYzXWlS`.

An Open API Specification (aka Swagger specification) documenting the ValueNet API is available at [spec.yaml](./api/spec.yaml) or [Swagger Editor](https://editor.swagger.io/?url=https://raw.githubusercontent.com/hack-with-admin-ch/HackZurich2021/main/api/spec.yaml).

Be aware that you use the model in a zero-shot way, so the model has never been fine-tuned on the specific database, but only on the ~200 databases provided in the Spider dataset (<https://github.com/taoyds/spider>). If you have created a better model or added interesting data, please let us know.

If you have any questions or problems then please contact us on our Slack channel #03_ws08_foitt.

Here is an example calling the API with curl:

```bash
curl -X 'PUT' \
  'https://inference.hackzurich2021.hack-with-admin.ch/api/question/hack_zurich' \
  -H 'X-API-KEY: sjNmaCtviYzXWlS' \
  -H 'Content-Type: application/json' \
  -d '{
  "question": "What is the share of electric cars in 2016 for Wetzikon?"
}'
```

### CORSTAT Sample Datasets and pre-trained ValueNet instance

For the challenge, CORSTAT provides us with various statistical data with interesting links about environmental protection focusing on transportation. It includes several datasets from the canton of Zurich and the canton of Basel-City, such as the number of registered electric cars or the use of and accessibility by public transport in certain regions. In addition, we added data from the Federal Office of Energy on charging stations for electric cars and converted it to a similar format. We have loaded the data into a relational database, analyzed it and identified interesting questions like:

- Which are the communes with the highest percentage of registered electric cars?
- What is the difference between urban and rural communes?
  Based on that, we created a script to generate training data (questions in natural language and corresponding SQL queries) and used its output to train ValueNet.

### Description of the Datasets currently used

For the most up-to-date version of the datasets see file EN_INDICATORS.csv, which is one of the input files for ValueNet. The sources of the following datasets are from canton Zurich, except when written otherwise.
| NAME                                                            | DESCRIPTION                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Access by bus \[% of inhabitants\]                              | Settlement areas are considered accessible within the meaning of the Ordinance on Public Transport Services if they are located within 750 meters of a station of the coarse public transport system (suburban train) or within 400 meters of a stop of the fine public transport system (bus, streetcar, light rail).  |
| Access by suburban train \[% ofinhabitants\]                    | Settlement areas are considered to be developed within the meaning of the Ordinance if they are located within a radius of 750 meters of a station of the coarse development (suburban train) or within 400 meters of a stop of the fine development (bus, streetcar, light rail).                                      |
| Passenger cars per 1000 inhabitants \[no.\]                     | Passenger cars registered on September 30, divided by the population in thousands. The value is also called motorization rate. Note: The value for the entire canton includes all cars with a Zurich license plate.  Data for Canton Basel-City has been added.                                                                                     |
| Distance to the next stop \[m\]                                     | Average distance to the next stop.                                                                                                                                                                                                                                                              |
| Accessibilityby suburban train+bus \[% ofinhabitants\]          | Settlement areas are considered accessible in the sense of the service ordinance if they are located within 750 meters of a station of the coarse service (suburban train) or within 400 meters of a stop of the fine service (bus, streetcar, light rail).                                                       |
| Public transport share (modal split) \[%\]                        | The basis for the calculation of the public transport share are the public transport routes of the source, destination and domestic traffic of the respective area on an average working day. With forecast condition.                                                                                              |
| MIV share (modal split) \[%\]                                   | The basis for the calculation of the MIV share are the MIV routes of the origin, destination and domestic traffic of the respective area on an average working day. With forecast condition.                                                                                                            |
| PW new registrations per 1000 inhabitants [amount]              | Passenger cars put on the road for the first time between October 1 of the previous year and September 30, divided by the population in thousands. Note: The value for the entire canton includes all cars with a Zurich license plate. Data for Canton Basel-City has been added.                                                                     |
| Hybrid motorcars stock \[%\]                                    | Share of passenger cars (PW) with hybrid drive in all passenger cars registered as of September 30.                                                                                                                                                                                          |
| Electric motor cars stock \[%\]                                  | Share of passenger cars (PW) with electric motor in all passenger cars registered on September 30. Note: The value for the entire canton includes all cars with a Zurich license plate.                                                                                                                |
| New registrations of hybrid motor cars [%]                      | Share of passenger cars (PW) with hybrid drive in all passenger cars put into circulation for the first time between October 1 of the previous year and September 30.                                                                                                                                  |
| New registrations electric motor cars [%]                        | Share of passenger cars (PW) with electric motor in all passenger cars put into circulation for the first time between October 1 of the previous year and September 30. Note: The value for the entire canton includes all cars with a Zurich license plate.                                                     |
| Charging stations of electronic cars [no.]                       | Data taken from the Swiss Federal Office for Energy and converted to communes.                                                                                                                                                                                                                   |
| Charging stations of electronic cars per 1000 inhabitants [no.] | Data taken from the Swiss Federal Office for Energy and converted to communes.                                                                                                                                                                                                                   |
| Total of electric and hybrid motor cars stock [%] | Share of passenger cars (PW) with either electric or hybrid drive in all passenger cars registered as of September 30; hybrid drive refers to all combinations of electric motor and combustion engine. Note: The value for the entire canton includes all cars with a Zurich license plate; these may also belong to persons or companies not domiciled in the canton of Zurich. Data for Canton Basel-City has been added.                                                                                                                                                                                                                  |
| Total new registrations of electric and hybrid motor cars [%] | Share of passenger cars (PW) with either electric or hybrid drive in all passenger cars put into circulation for the first time between October 1 of the previous year and September 30; hybrid drive refers to all combinations of electric motor and combustion engine. Note: The value for the entire canton includes all cars with a Zurich license plate; these may also belong to persons or companies not domiciled in the canton of Zurich. Data for Canton Basel-City has been added.                                                                                                                                                                                                                   |

### Data Structure of Input Files

The four input files used by ValueNet can be found in the folder `input_data`. The database generates queries based on these files.
| FILENAME                  | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EN\_INDICATORS.CSV        | The main list with the descriptions of all the datasets. Includes the IDs of the datasets (important for the joins), names and descriptions. Other columns are currently not used.                                                                                                                                                                                                                                                                   |
| EN\_INDICATOR\_VALUES.csv | This is the file that contains the actual “data”. Next to the values, it contains columns for the corresponding INDICATORS\_ID (thus, the dataset that this value corresponds to), the corresponding SPATIALUNIT\_ID (thus, the spatial unit, such as the commune name that this value corresponds to), the year of the value, and the last two columns can be ignored right now. Thus, every data point has value, spatial unit, and temporal unit. |
| EN\_T\_SPATIALUNIT.csv    | This file contains the SPATIALUNIT\_ID for every spatial unit (commune, region, canton, etc.). There are many columns in this file that might not be relevant for the challenge. TYPE\_ID means 1=Commune; 3=Bezirk/”District”; 4=Region;8=Canton;9=Old commune that does not exist anymore. COMMUNE\_TYPE\_ID corresponds to definitions that can be found in the file EN\_T\_COMMUNE\_TYPE.csv                                                     |
| EN\_T\_COMMUNE\_TYPE.csv  | It contains the definition of certain types of communes, for example if it is a rural or urban commune (but more fine grained).                                                                                                                                                                                                                                                                                                                      |

### Description of the Q&A pair-generator

We prepared two scripts that takes the data files and generates pairs of questions in natural language and answers as SQL-queries. The first python-script is documented and can be accessed under: generate_sql_statments_and_questions.ipynb
[https://github.com/statistikZH/statbot/tree/main/hackathon_hackzurich](https://github.com/statistikZH/statbot/tree/main/hackathon_hackzurich)
Examples of good questions can thus be accessed in the file.  

For instance, the following view simplifies access to electric cars:

```sql
CREATE OR REPLACE VIEW public.**share_electric_cars**
AS
SELECT v.value AS share_electric_cars,
    v.year,
    v.spatialunit_id
   FROM indicators i
     JOIN indicator_values2 v ON i.indicator_id = v.indicator_id
  WHERE btrim(i.name::text) = 'Electric motor cars stock [%]'::text;

ALTER TABLE public.share_electric_cars
    OWNER TO postgres;
```

Now, the following query “What is the share of electric cars in 2016 for Zuerich” can be simplified by using the above view:

```sql
SELECT T1.share_electric_cars  
FROM **share_electric_cars** AS T1  
JOIN spatialunit AS T2 ON T1.spatialunit_id = T2.spatialunit_id  
WHERE T2.name = 'Zuerich' and T1.year = 2016
```

The Postgres database including the new views can be downloaded from:
[https://github.com/brunnurs/valuenet/blob/hackzurich/data/hack_zurich/hack_zurich_database.dmp](https://github.com/brunnurs/valuenet/blob/hackzurich/data/hack_zurich/hack_zurich_database.dmp)

The second script then takes the Q&A pairs populated in the CSV-file questions_queries_python.csv and through a T5 machine learning model generates paraphrases in order to have a larger training set. This second, which can be found under scripts/paraphrases.py, generates the larger file under questions_queries_paraphrases.csv. Like this we have for example instead of 77 Q&A pairs then about 573 pairs. You can also try to tweak the script and generate more training data. The more (qualitatively good!) data we have, the better.

### User Interface Aspects

Building a good user interface is an important aspect of creating a user-friendly and intuitive experience when it comes to interacting with OGD. By thinking about possible user interfaces for our challenge, we identified the following aspects you could take into consideration. Please treat these as an inspiration and feel free to create issues / PRs if you find more aspects that you consider to be important.

#### Data discovery

We might have data about topics our users are not aware of and may be able to answer more complex queries than they imagine. How can we let them discover what we have to offer? 

#### Feedback

ML needs good training data to be accurate. Learning is a process, and making mistakes is part of the experience. Therefore, at least in the beginning, it is likely that error may occur from time to time. . What would be the best way to collect feedback? Could we even gamify the learning process?

#### Accessibility

How can we build inclusive solutions for people with impairments or other limitations such as:

- visual impairments;
- dyscalculia (difficulty learning or comprehending arithmetic, such as difficulty in understanding numbers);
- nearly no computer skills;
- no fluency in one of the official languages of Switzerland (i.e. German, French, Italian and Romansh) or English.

You can also explore solutions specifically targeting groups with disabilities.
