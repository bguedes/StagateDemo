# StagateDemo

## Rest apis

Import on Postman the file 01-Astra Serverless Rest Movie.postman_collection.sstable2json

Import the CSV from Astra -> Movies and TV Shows

### Get Movie by Id

```sql
token@cqlsh:demo> select * from movies_and_tv where show_id=81036524;

@ Row 1
--------------+------------------------------------------------------------------------------------------------------------------------------------------------------
 show_id      | 81036524
 cast         | Chapman To, Charlene Choi, Gao Yunxiang, Shatina Chen, Alice Li, Jiayu Xie, Jiemeng Zhuang, Venus Wong, Ching-Wan Hui, Jennifer Zhang
 country      | Hong Kong
 date_added   | July 21, 2019
 description  | A debt collector takes over the management of a group of starlets after their talent agent defaults on a loan, but soon finds he's in over his head.
 director     | Chi Keung Fung
 duration     | 98 min
 listed_in    | Comedies, International Movies
 rating       | TV-14
 release_year | 2013
 title        | The Midas Touch
 type         | Movie

(1 rows)
token@cqlsh:demo>
```

The same using Rest :

https://{{databaseId}}-{{region}}.apps.astra.datastax.com/api/rest/v2/keyspaces/{{keyspaceName}}/movies_and_tv/80216541

### Get Movies by Country

Before we need to create the Storage Atteched Index (SAI) for the column country

```sql
CREATE CUSTOM INDEX movie_country_sai_idx ON demo.movies_and_tv (country) USING 'StorageAttachedIndex';
```

Then we can use this query :

```sql
token@cqlsh:demo> select * from movies_and_tv where country='Spain' limit 2;

@ Row 1
--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 show_id      | 80216541
 cast         | Clara Lago, Álex García, Carmen Maura, Alexandra Jiménez, Paula Malia, Fernando Guallar, Carlos Cuevas
 country      | Spain
 date_added   | May 10, 2019
 description  | After her partner cheats on her, an architect returns to her hometown to reassess her life with the help of her eccentric family. Based on the novel.
 director     | Patricia Font
 duration     | 98 min
 listed_in    | Comedies, Dramas, International Movies
 rating       | TV-MA
 release_year | 2018
 title        | In Family I Trust
 type         | Movie

@ Row 2
--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 show_id      | 81031939
 cast         | Blanca Suárez, Macarena García, Amaia Salamanca, Belén Cuesta, Maxi Iglesias, Juan Diego, Joaquín Climent, Carlos Bardem, Emilio Gutiérrez Caba, Tito Valverde, Teresa Rabal, Rossy de Palma, Marisa Paredes
 country      | Spain
 date_added   | May 3, 2019
 description  | After their mother's death, four sisters learn a shocking family secret and embark on an adventure to discover the truth about their genealogy.
 director     | Gabriela Tagliavini
 duration     | 79 min
 listed_in    | Comedies, International Movies, Romantic Movies
 rating       | TV-MA
 release_year | 2019
 title        | Despite Everything
 type         | Movie

(2 rows)
token@cqlsh:demo>
```

The same using Rest :

https://{{databaseId}}-{{region}}.apps.astra.datastax.com/api/rest/v2/keyspaces/{{keyspaceName}}/movies_and_tv?where={"country": {"$eq": "Spain"}}
