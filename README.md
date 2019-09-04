	 	 	
#Installation Guide for Plato Research Dialogue System

Clone this repository:
git clone git@github.com:uber-research/plato-research-dialogue-system.git

or by downloading zip and extract from https://github.com/uber-research/plato-research-dialogue-system


Create a virtual environment (suggested)
 To keep dependencies required by different projects separate by creating isolated python virtual environments for them

Install the requirements

sudo apt-get install python3-pyaudio
pip install -r requirements.txt

Install required libraries

sudo apt-get install libcurl4-gnutls-dev
sudo apt-get install libxslt1-dev
sudo apt-get install libxml2-dev
sudp apg-get install build-essential

Inorder to run plato agent, trun the following command with the appropriate configuration file run a simulation using the agenda based user simulator in the Cambridge Restaurants domain:

python runPlatoRDS.py -config Examples/config/simulate_agenda.yaml

To run a text based interaction using the agenda based simulator in the Cambridge Restaurants domain:
python runPlatoRDS.py -config Examples/config/simulate_text.yaml
To run a speech based interaction using the agenda based simulator in the Cambridge Restaurants domain:
python runPlatoRDS.py -config Examples/config/simulate_speech.yaml


To run multiple Plato agents on the benchmark Cambridge Restaurants domain, we run the following commands to train the agents’ dialogue policies and test them:
Training phase
python runPlatoRDS.py -config Examples/config/CamRest_MA_train.yaml
 
Testing phase

python runPlatoRDS.py -config Examples/config/CamRest_MA_test.yaml
examples of running a single Plato agent or multiple Plato agents in the generic module mode:
Single generic agent, used to implement custom architectures or to use existing, pre-trained statistical models:

python runPlatoRDS.py -config Examples/config/simulate_agenda_generic.yaml

Multiple generic agents, same as above but for multiple agents (assuming you have trained dialogue policies using Examples/config/CamRest_MA_train.yaml):

python runPlatoRDS.py -config Examples/config/MultiAgent_test_generic.yaml
To use learning algorithms that are implemented inside Plato, any external data, such as DSTC2 data, should be parsed into this Plato experience so that they may be loaded and used by the corresponding component under training.

To train from data, users simply need to load the experience they parsed from their dataset. As an example of offline training in Plato, we will use the DSTC2 dataset, which can be obtained from the 2nd Dialogue State Tracking Challenge website:
http://camdial.org/~mh521/dstc/downloads/dstc2_traindev.tar.gz

Once the download is complete, you need to unzip the file.
The runDSTC2DataParser.py script will parse the DSTC2 data for you, and save it as Plato experience. It will then load that experience and train a Supervised Policy:
python3 runDSTC2DataParser.py -data_path dstc2_traindev/data
You can test using the following configuration:
python runPlatoRDS.py -config Examples/config/simulate_agenda_supervised.yaml


Train your model using Ludwig and Plato

ludwig experiment --model_definition_file Examples/config/ludwig_nlg_train.yaml  --data_csv Data/data/DSTC2_NLG_sys.csv --output_directory Models/CamRestNLG/Sys/
	  
      
Load the model in Plato. Go to the simulate_agenda_nlg.yaml configuration file and update the path if necessary:
python runPlatoRDS.py -config Examples/config/simulate_agenda_nlg.yaml

Download MetalWoZ 
https://www.microsoft.com/en-us/research/project/metalwoz/
Run any model using WozParser
E.g
python3 runMetalWOZDataParser.py -data_path metalwoz-v1/dialogues/ORDER_PIZZA.txt

Train an end-to-end model
ludwig train 
       --data_csv Data/data/metalwoz.csv 
       --model_definition_file Examples/config/metalWOZ_seq2seq_ludwig.yaml
       --output_directory "Models/JointModels/"







