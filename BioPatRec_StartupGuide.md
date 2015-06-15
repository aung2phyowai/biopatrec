# Starup Guide #

**1.- Open MATLAB as administrator.** This is especially important for input-output functionality (e.g. recordings, VRE, etc). If you are just interested in the Offline PatRec, you can skip this step. To be sure you will always run MATLAB as administrator (Windows 7):

  * Right click on the MATLAB executable file
  * _Properties_
  * _Compatibility_
  * Check _Run this program as administrator_

**2.- Set path...** You need to add the path to all functions so they can be found by MATLAB when running BioPatRec

  * _File_
  * _Set path..._
  * _Add with subfolders_
  * Selected the path to BioPatRec
  * _Save_

**3.- Change the “Current Folder” to that of BioPatRec.** This is because some temporal files will be stored in the current folder during execution. This step is not absolutely necessary, however, you will have temporal files everywhere if you skip it.

**4.- Install your DAQ card.** In order to use BioPatRec for real-time PatRec, and therefore recording bioelectric signals, you need to install your DAQ card drivers. The system is currently using the "session-based interface" paradigm, and it has been tested with the NI-USB6009 card (one of the most simplest and affordable DAQ cards from National Instruments).

  * For the USB-6009 you will need to install the [NI-DAQmx](http://joule.ni.com/nidu/cds/view/p/id/2604/lang/en)
  * Other devices using SCI have been used as well. These routines and the "legacy" routines for MATLAB can be made available upon request.
  * See [Comm](Comm.md) and [SigRecordings](SigRecordings.md) for more details.

# How To's #

The following are quick guidelines on how to use BioPatRec. It is assumed that you have already setup your workspace and paths in MATLAB to work with BioPatRec.

# How to treat the raw signals and extract the signal features? #

At the MATLAB command window

  * Type BioPatRec
  * Click _Pattern Recognition_
  * Click _Get Sig. Features_
  * Select the file containing the [recSession](recSession.md) structure (e.g. any `*.mat` file in [Data Repository](Data_Repository.md))

The latter will take you to the Signal Treatment GUI, now:

  * Select the movements to be used by highlighting the name (all are selected by default)
  * Tick the _Add "rest" as a movement_ if you want to have "no movement" or "rest" as a pattern/class/movement.
  * Select the percentage of the contraction time that you will like to use. If you select _1_, you will include the transient period of the movement. If you chose _0.5_, you will probably only use the signals corresponding to the steady state of the movement or the isometric contraction. A 0.7 cTp is recommended see [BioPatRec article](http://www.scfbm.org/content/8/1/11)
  * Select the channels to be used by highlighting the number (all are selected by default)
  * Click _Pre-processing_

Now that the movements and channels have been selected you can

  * Select frequency or spatial filters (See SigTreatment for more info)
  * Select the way to extract time windows
    * _Overlapped Cons_ is normally used with a time window length of 0.2 seconds and overlap of 0.05 seconds
    * The number of available windows with the selected method will be given in _No. windows_
  * Select how many windows will be use for the training, validation and testing set
    * This can be done by changing the percentage or directly the number of windows
    * Before continue be sure that the _No. windows_ is the same as the _Total_ windows, otherwise you will get an error! (This is automatically done from TVÅ version)
  * Click _Treat_

The structure [sigFeatures](sigFeatures.md) will be generated as a result, and it can be saved using the menu _Data_ then _Save Features_. This will avoid that you go through the whole signal processing process next time.

Demo on how to:
  * Setup the path to BioPatRec in Matlab (only has to be done once)
  * Process a recording session
  * Train a classifier (offline pattern recognition, see below)

<a href='http://www.youtube.com/watch?feature=player_embedded&v=CG8hSI6wssc' target='_blank'><img src='http://img.youtube.com/vi/CG8hSI6wssc/0.jpg' width='425' height=344 /></a>

# How to run offline Pattern Recognition? #

Coming from the MATLAB command window

  * Type BioPatRec
  * Click _Pattern Recognition_
  * Select _Get Sig. Features_ to load the file with the [sigFeatures](sigFeatures.md) structure. If you only have the [recSession](recSession.md) structure file, see "How to pre-process ...".

Coming from the Signal Treatment GUI

  * Select the signal features to be used by highlighting their ID or selecting _TopX_ (default: Top 4)
  * Modify the number of sets to be used (if required)
  * Select _Algorithm_
  * Select _Training Method_ (if required)
  * Select _Normalization_ (if required)
  * Select _Movements_ (default: _Individual Mov_)
  * Select _Topology_ (default: _Single Classifier_, see [PatRec\_Topologies](PatRec_Topologies.md))
  * Tick _Randomized sets_ if you want the training, validation and testing sets to be mixed randomly before training (normally checked)
  * Click _Run Off-line Training_

The structure [patRec](patRec.md) will be generated as a result, and it can be saved using the menu _Data_ and then _Save PatRec_. For more details see PatRec.

# How to run Real-time Pattern Recognition? #

In order to do this you need to have trained a classifier as in the previous step, or have the structure [patRec](patRec.md) available. You can load [patRec](patRec.md) from a file using the open menu.

A simple real-time test:

  * Select for how long you want to test the algorithm in _Testing time_
  * Click _Test Real-time PatRec_
  * The predicted motion will be highlighted in the listbox
  * The box _Avg. Proc. Time_ will display the average processing time for your algorithm once the testing time is finished.

More advanced test:

  * [Motion\_Test](Motion_Test.md)
  * [TAC\_Test](TAC_Test.md)

See PatRec for further details.

# How to control the Virtual Reality Environment? #

In order to do this you need to have trained a classifier as in the previous step, or have the structure [patRec](patRec.md) available. You can load [patRec](patRec.md) from a file using the open menu.

The movements are loaded into a list upon start, thus changes will not take effect until the next run.

  * Select for how long you want to run the test in _Testing time_
  * Click the _VRE_ button
  * Select configurations for the environment in the _Ogre3D_ window. Click OK.
  * Click _Test Real-time PatRec_

The VRE can also be controlled using the buttons next to each movement, this can be done without correct [patRec](patRec.md) structure.

**Note:** The corresponding movement in the dropdown-list next to the classified movement is sent to the VRE, if you want to change the movements, you can do so in the dropdown-list. The distance each movement is moved is also retrieved from the textboxes next to each movement.

You can also run a so called Target Achievement Control ([TAC\_Test](TAC_Test.md)) test, read more about that under the dedicated wiki page.

For more information about the environment, see [VRE](VRE.md).

# How to record signals? #

Recordings can be done as a single time-defined event for fast signal quality evaluation, or as a recording session for further processing and PatRec. In the following steps, it is assumed that you have already configured your DAQ card.

## One-shot recordings ##

At the MATLAB command window

  * Type BioPatRec
  * Click _Recordings_
  * Select how fast you want to sample the signals (sampling frequency)
  * Select for how long you want record (sampling time)
  * Select the time window to be displayed in the GUI (peeking time)
  * Select the recording channels by ticking the channels check-boxes
  * Click _Start Recording_

Once the recording is completed, you can also:

  * Apply different filters to the displayed time windows. Use the panel on the left-side, or more filters are available in the menu _Filters_
  * You can navigate in the graphs using the tools on the top of the GUI
  * You can zoom in a specific time using the boxes on the top of the graphs.

NOTE:
  * You can use this GUI to open any recording session or _cdata.mat_
  * At this point you can only do recordings using this GUI if you are using the _session-based interface_, e.g. you are using the NI-USB6009.

## Recording Session ##

At the MATLAB command window

  * Type BioPatRec
  * Click _Recording Session_
  * Write the number of repetitions of each movement (nR)
  * Write the number the contraction time (cT) in seconds
  * Write the number the relaxation time (rT) in between contractions
  * Select the movements by highlighting the name
  * Push _Record_

Now you will be redirected to the Analog Front-End Selection.

  * Select the device name
  * Select the number of channels
  * Select sampling frequency (Default 2k)
  * Tick the option of dummy contraction if wanted
  * Push _Record_

The structure [recSession](recSession.md) will be generated as a result.

NOTE: Leave the default options for _Active, COM/port, and Show_. Unless you have the required hardware and software.

See [sEMG](sEMG.md) for tips on how to perform surface electromyography