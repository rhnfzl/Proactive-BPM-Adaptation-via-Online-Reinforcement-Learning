# Proactive-BPM-Adaptation-via-Online-Reinforcement-Learning

The repository goes step by step process to recreate the Experiment of [Triggering Proactive Business Process Adaptations via Online Reinforcement Learning](https://doi.org/10.1007/978-3-030-58666-9_16) paper.

To recreate the experiments, you need to use docker and that will lead not to install any of the requirements on your machine.

The report realted to the experiment and understanding of the paper can be found here [Experiment Report on Proactive-BPM-Adaptation-via-Online-Reinforcement-Learning](https://www.dropbox.com/s/hlccrw7t9gzlpzn/Proactive-BPM-Adaptation-via-Online-Reinforcement-Learning_Experiment-Report.pdf?dl=1)

Setup the docker in your machine, clone the repository and replace models-bpic, models-bpic17, models-traffic with the file from https://uni-duisburg-essen.sciebo.de/s/iFy6y0BsAWTgWLV?path=%2F. The CSV file for each model is too large to be pushed to Git, those will be used during RL execution. Once the files are updated, Run the "00_docker_builder" scripts. The script will build two docker images for you, named "threshold-python" and "threshold-java". Within the root of this repository, execute the "docker-compose up" command, to run both images.

Afterwards, you can use "docker ps" to find the containers' docker ids, which are now running your images.

Then:

1. Run "docker exec -it ```<java container id>``` sh" to open an interactive shell within the java-container.

2. Execute "java -jar 00_ExperimentMain.jar" to load all the models into memory and set up four experiments with the four datasets. You can also give a list within swirly brackets (ex. java -jar 00_ExperimentMain.jar {2,3,4}) to the jar in order to only create experiments with those specific datasets. Here  1 - models-c2k, 2 - models-traffic, 3 - models-bpic and 4 - models-bpic17.  Proceeded to the next step, as soon as you see the output "Waiting for RL connection..."

3. Run "docker exec -it ```<python container id>``` bash" in a new terminal to open an interactive shell within the python-container.

4. Change the directory to python_src using "cd python_src" and then execute "python runexperiments.py". If you want to execute the runexperiments.py file from home directory then use "python python_src/runexperiments.py" then after the completion of execution execute "cp *.csv /threshold-learning/python_src" in the Python Container. You can tinker with the runexperiments.py script to modify steps and range based on the number of the dataset you are choosing in step 2.

5. The experiments will generate/ copy .csv files (e.g. MasterStateMasterReward_<experiment number>_metrics_of_episodes.csv), numbered in the order of the experiments, within the python_src folder. You can run "java -jar 00_GnuPlotter.jar" inside the java-container to plot pngs for all those generated CSV files which you can find inside python_src folder.


You can copy the result to the Host system using below command from either of the docker containers:
docker cp ```<containerId>```:/file/path/within/container /host/path/target
