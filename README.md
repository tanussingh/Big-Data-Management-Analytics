# CS 6350.001 - Big Data Project

Project 2: De-duplication of Spanish Language Articles using UDPipe. 

### Team Members:
1. Ishan Sharma - IXS171130
2. Mavis Francia - MCF140030
3. Tanushri Singh - TTS150030
4. Vyaas Shenoy - VNS170230

## Running The Crawler

There are two crawlers: News Please, RSS Crawler. 

### News Please

News Please can be installed by running `pip install news-please` and run by using `news-please -c Config/config.cfg`

By default, it will use `config.cfg` file inside `Crawler` folder. Some websites tend to crash it and there are hjson 
comments that mark those websites. 

The crawler writes to disk and these files can be indexed using `python misc/mongo_index.py`. The file also has some 
extra configuration options that can be changed on top. 

### RSS Crawler

RSS Crawler uses `Crawler/config/sitelist_rss.hjson` config file. This file can be automatically generated by running 
`python3 misc/crawler_config_transformer.py` if sites in `sitelist.hjson` have changed. 


## Doc2Vec

The model can be trained using `misc/doc2vec.py`. It reads data from mongo database `big_data` with collection 
`spanish_articles`. It will be saved to `misc/models/`. 

After model training, `Pandas/doc2vec.py` needs to be run. It will read from `spanish_articles` collection and
write to `d2v_calculated` collection. 

## Spark Streaming for UDPipe

Download a Universal Dependencies model for Spanish. Here are two different ones:
* [AnCora](https://lindat.mff.cuni.cz/repository/xmlui/bitstream/handle/11234/1-2898/spanish-ancora-ud-2.3-181115.udpipe?sequence=75&isAllowed=y)
* [GSD](https://lindat.mff.cuni.cz/repository/xmlui/bitstream/handle/11234/1-2898/spanish-gsd-ud-2.3-181115.udpipe?sequence=74&isAllowed=y)

You can find a full list of models [here](https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-2898).

Install the ufal.udpipe library by running: `pip install ufal.udpipe`. You can read more about this library [here](https://pypi.org/project/ufal.udpipe/).
 
 Once UDPipe is installed, Spark job can be run using `spark-submit Streaming/streamToSpark.py`. It will write the 
 results to mongo collection `udpipe_parse`.
 
 ## UDPipe Similarity Calculation
 
 Similarity from UDPipe can be calculated by running `python UDPipe/runningJaccSim.py`. This will write the results to 
 collection `jacc_sim_calculated`. 
 
 ## Data Analysis
 
 iPython notebook for graphing and seeing some statistics is included in `Analysis/data_analysis.ipnyb`. 
