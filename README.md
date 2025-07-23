# ANDROIDE_Project_HCI_Fitts2.0
Sorbonne University ANDROIDE Master project made by BELLUT Quentin and LING Juliette. README by PINTON Mariam.
This software will let you run some Fitts experiences, and analyse the data you get from them. Choose the language, the device you're using, and click on the targets as fast and precisely as you can.


## Table of Contents
- [Installation]
- [Organisation]
- [Usage]
- [DataAnalysis]


## Installation
Download this project from GitHub. You may also need to install libraries and modules such as pygame, selenium and tkinter.

## Organisation
The main folder is named : **ANDROIDE_Project_HCI_Fitts2.0-main**. Inside it, along some miscellaneous JSON files, a PDF file which contains a report on the software, and this README, there are three folders : 
- the **src** folder contains the main portion of code. 
    The **class** folder contains the various Python files which implement the classes and functions necessary for an experiment. 
    The **experiments** folder contains text files which list experiment setups. You may use them, or write your own, in order to quickly set up numerous experiments at once.
    The **tools** folder contains Python files which are useful for creating experiments from a text file, a pkl file or a web URL.
    The **images** folder is self-explanatory.
    
    The Python file **editor.py** helps configure the graphical user interface.
    The Python file **Fitts2.0.py** is the main file from which you can run the program.
    The three pkl files contain data from past experiments. The **fichier_configTEST.txt** file is an old test file for generating experiments from text files.

- the **users_data** folder contains data from past experiments in JSON format.

- the **data_extractor** folder contains exported data, usable for analysis.
    
    Only the **DATA** folder is of interest to us, more specifically the **DATA/export** folder. It contains the data in csv format, after having been aggregated and/or fused with other data. This is what you'll be looking for once you're done running all your experiments : it will allow you to analyse the obtained results.
    The three Python files contain functions and tools to correctly put the data together.
    The rest is of little importance.


## Usage
The starting point of the project is located in the file : **Fitts2.0.py**, in the **src** folder.
Depending on what kind of experiment you're looking to run, here are the commands :

1. Randomly generated targets :
```bash
python3 Fitts2.0.py --random
```
The targets will pop up randomly on the screen.

2. Horizontal Line :
```bash
python3 Fitts2.0.py --lineH
```
The targets are located on either end of a horizontal line.

3. Vertical Line :
```bash
python3 Fitts2.0.py --lineV
```
The targets are located on either end of a vertical line.

4. Circle, clockwise :
```bash
python3 Fitts2.0.py --circleH
```
The targets are located on a circle and will appear clockwise.

5. Circle, anti-clockwise :
```bash
python3 Fitts2.0.py --circleAH
```
The targets are located on a circle and will appear anti-clockwise.

6. Generate an experiment from a URL :
If you wish to generate targets from the clickable links of a given web page, simply enter its URL like so :
```bash
python3 Fitts2.0.py --url https://example.com
```
Note : this feature doesn't work as of now.

7. Generate an experiment from a pickle model :
```bash
python3 Fitts2.0.py --model myModel
```
Note : I haven't figured out how this works yet.

8. Generate an experiment from a text file :
```bash
python3 Fitts2.0.py --file 'fileName.txt'
```
This command is useful for generating many experiments in one go.
Replace fileName by your actual file name. Your text file must be located in the **src/experiments** folder, and have the following format (example below) :

    pause : time #optional ; adds a pause time between two experiments
    device name #replace name by your actual device name : mouse, touchpad, stylus, or controller

    #for an experiment generated from a URL :
    EXPERIENCE_NAME url https://www.example.com NUMBER_OF_MOUVEMENTS

    #for a line experiment :
    EXPERIENCE_NAME line DISTANCE RADIUS NUMBER_OF_MOUVEMENTS ANGLE

    #for a vertical line experiment :
    EXPERIENCE_NAME lineV DISTANCE RADIUS NUMBER_OF_MOUVEMENTS

    #for a horizontal line experiment :
    EXPERIENCE_NAME lineH DISTANCE RADIUS NUMBER_OF_MOUVEMENTS

    #for a circle experiment :
    EXPERIENCE_NAME circle DISTANCE RADIUS NUMBER_OF_TARGETS DIRECTION #for DIRECTION, add AH for anti-clockwise, H for clockwise

    #for a random disposition experiment :
    EXPERIENCE_NAME random DISTANCE RADIUS NUMBER_OF_MOUVEMENTS

For example :

    pause:20
    device mouse
    fitts_exp0 circle 440 66 21 AH
    device controller
    pvp_exp1 random 1160 14 21
    
This creates an experiment named fitts_exp0 with a mouse device and a circle disposition, followed by another experiment named pvp_exp1, with a controller device and a random disposition. There will be a 20-second-long pause between the two experiments, as specified at the beginning of the file.
You may not write more than 20 experiments in a single text file, as it may cause a crash (reason undertermined).

Note : for reasons undetermined, the random disposition causes a crash. The pause function does not work.

Bonus : if you wish for the experiment not to be in fullscreen, you may add the ```--windowed``` option to the command. Be careful however as it may render some experiments more difficult/impossible to fulfill (targets may appear outside of a smaller screen).

## DataAnalysis
This section explains how to use and analyse the data you've stored up.
Every experiment, even those aborted, are saved as a JSON file in the **users_data** folder. The review left by the subject at the end of the experiment can be found at the very bottom of the file. To extract your data :

* Put xp data in **users_data/saved**
* Go to **data_extractor**
* Run :
```bash
python3 mainExt.py PVP_Project &
python3 mainExt.py PVP_Centroid &
python3 mainExt.py PVP_det &
```

* When finished, run :
```bash
python3 aggregate.py
```

* The data is located in : **data_extractor/DATA/export**
