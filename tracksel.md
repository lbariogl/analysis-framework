# Track selection in O2

## Concept

The track selection in O2 is based on the concept of derived tables created in dedicated tasks from available AOD contents.
The task responsible for the track selection is [`Analysis/Tasks/trackselection.cc`](https://github.com/AliceO2Group/AliceO2/blob/dev/Analysis/Tasks/trackselection.cxx). It produced a derived track selection table [TrackSelectionTable](https://github.com/AliceO2Group/AliceO2/blob/dev/Analysis/DataModel/include/Analysis/TrackSelectionTables.h), which is joinable with _Tracks_ table.

## Usage in user tasks

One can see _o2-analysis-trackqa_ task as reference: [`Analysis/Tasks/trackqa.cxx`](https://github.com/AliceO2Group/AliceO2/blob/dev/Analysis/Tasks/trackqa.cxx). To use the track-selection task, the following steps must be followed:

* add [`TrackSelectionTable.h`](https://github.com/AliceO2Group/AliceO2/blob/dev/Analysis/DataModel/include/Analysis/TrackSelectionTables.h) header:
    ``` c++
    #include "Analysis/TrackSelectionTable.h"
    ```

* join _Tracks_ (if needed also _TracksExtra_) and _TrackSelection_ tables and use corresponding iterator as an argument of the process function:
    ``` c++
    void process(soa::Join<aod::Tracks, ...  aod::TrackSelection>::iterator const& track)
    ```
* run your tasks in stack with timestamp, event-selection, (multiplicity and centrality tasks, if needed) and track-selection task:
    ``` bash
    o2-analysis-timestamp --aod-file AO2D.root -b | o2-analysis-event-selection -b | o2-analysis-trackselection -b | o2-analysis-user-task -b
    ```
    _o2-analysis-timestamp_ task [`Analysis/Tasks/timestamp.cxx`](https://github.com/AliceO2Group/AliceO2/blob/dev/Analysis/Tasks/timestamp.cxx) is required to create per-event timestamps necessary to access relevant CCDB objects in the event selection and/or centrality tasks. 
