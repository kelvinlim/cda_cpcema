# cda_cpcema

```
# For linux
# at VA
source /home/train/Venv/cdatools2/bin/activate
export PATH=~/Projects/cda_tools2:$PATH

# For VA avd windows
..\cda_tools2\.venv\Scripts\activate.ps1
# to  run a cda_tools2 program

# parse the data
python ..\cda_tools2\run_parse2.py --mdir .\proj_cpcrun1 --yaml config.yaml --verbose

# run the causal discovery analysis
python ..\cda_tools2\causal2.py --mdir .\proj_cpcrun1 --yaml config.yaml --verbose

# create the label plots
python ..\cda_tools2\plot_model.py --mdir .\proj_cpcrun1 --yaml config.yaml --stub "*.txt"

# create the path distribution data
python ..\cda_tools2\check_paths2.py --mdir .\proj_cpcrun1 --yaml config.yaml 

# create the png distribution plots for pmax of 10 (up to 10 edges)
python ..\cda_tools2\paths_plotdist2.py --mdir .\proj_cpcrun1 --yaml config.yaml

# create the docx summary file including all the png files
python ..\cda_tools2\create_docx_proj.py --mdir .\proj_cpcrun1 --yaml config.yaml --log logfile.txt --stub .png
```

## exporting data

For initial download, we need the web downloaded csv file with number values since it has the actual questions. The download done with LNPIQualtrics has the same data as the web version but doesn't have the questions.

LNPIQualtrics uses the webfile to generate a study_descr.txt file that contains a section of code that run_parse uses for renaming variables.

```
-
  op: rename
  arg:
    StartDate: Start Date
    EndDate: End Date
    Status: Response Type
    IPAddress: IP Address
    Progress: Progress
    Duration (in seconds): Duration (in seconds)
    Finished: Finished
    RecordedDate: Recorded Date
    ResponseId: Response ID
    RecipientLastName: Recipient Last Name
    RecipientFirstName: Recipient First Name
    RecipientEmail: Recipient Email
    ExternalReference: External Data Reference
    LocationLatitude: Location Latitude
    LocationLongitude: Location Longitude
    DistributionChannel: Distribution Channel
    UserLanguage: User Language
    Q_RecaptchaScore: Q_RecaptchaScore
    Browser_info_Browser: Click to write the question text - Browser
    Browser_info_Version: Click to write the question text - Version
    Browser_info_Operating System: Click to write the question text - Operating System
    Browser_info_Resolution: Click to write the question text - Resolution
    Q01_Fatigue: Right now, I am having trouble starting things because I'm tired.
    Q54: How accomplished do you feel in your activities in the last 3 hours?
    Q55: Right now, how much is pain interfering with your day to day activities?
    Q56: Right now, I have the strategies to cope with my pain.
    Q57: Right now, I worry all the time about whether the pain will end.
    Q58: Right now, I wonder whether something serious may happen.
    Q59: Right now, I can't seem to keep my pain out of my mind.
    Q60: Right now, how much pain are you in?
    Q61: Right now, how stressed do you feel?
    Q62: Right now, how down/depressed do you feel?
    Q63: Right now, how rested do you feel?
    Q64: Right now, of the strategies you have to cope with your pain, how helpful do you think they will be?
    Q65: Right now, I feel supported by those around me.
    Q67: Right now, I feel my life is heading in a good direction.
    Q66: Right now, I feel like a burden on others.
    Q68: Right now, I have trust in my VA pain providers.
    Q69: Right now, I think I will get better.
    TrailsAB-int: Did anything interrupt you during the previous task?
    SymmetrySpan-int: Did anything interrupt the previous task?
    SpatialSpan: SpatialSpan
    TrailsAB: TrailsAB

```

## Steps

```
# get the index for desired study
./LNPIQualtrics.py --config config_qualtrics_va.yaml

Surveyindex: 20 Title: CPC EMA Survey


# retrieve the most recent date using the webfile
# to create the study_descr.txt file. This only has to 
# be done once or if a survey is changed.
./LNPIQualtrics.py --config config_qualtrics_va.yaml --index 20 --webfile CPC+EMA+Survey_February+27,+2025_12.14.csv

# avd windows
python .\LNPIQualtrics.py  --config .\config_qualtrics_va.yaml --index 20 --webfile '..\cda_cpcema\CPC+EMA+Survey_August+26,+2025_16.32.csv'

# On subsequent downloads - doesn't work, variable names change!
./LNPIQualtrics.py --config config_qualtrics_va.yaml --index 20

```