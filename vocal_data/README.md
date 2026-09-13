# Raw USV audio data

> **Data not included.** The raw ultrasonic-vocalization recordings used in this project were **provided by Dr. Shai Netser from the Lab for Neurobiology of Social Behavior, Sagol Department of Neurobiology, University of Haifa**. The original audio files are not redistributed in this repository.

This directory is kept as a placeholder so that users with **authorized access** to the original data can reproduce the notebook's expected local file structure.

## Data access

The source `.wav` recordings remain subject to the permissions of the originating research group and data owners. They should not be copied, published, or redistributed without authorization.

This public repository contains only the project code, documentation, and final report.

Lab website: https://shlomowagner-lab.haifa.ac.il/

## Expected local files

If you have authorized access to the source data, the notebook expects the following recordings in this directory:

```text
M2_prb4_Free_Female1_usv.wav
M2_prb4_Free_Female2_usv.wav
M2_prb4_Free_Female3_usv.wav
M3_prb5_Free_Female1_usv.wav
M3_prb5_Free_Female2_usv.wav
M3_prb5_Free_Female3_usv.wav
M3_prb5_Free_Female4_usv.wav
M6_prb7_Free_Female1_usv.wav
M7_Prb7_Freefemale1_usv.wav
M7_Prb7_FreeFemale2_usv.wav
M7_Prb7_FreeFemale3_usv.wav
M7_Prb7_FreeFemale4_usv.wav
M8_Prb8_FreeFemale1_usv.wav
M8_Prb8_FreeFemale2_usv.wav
M8_Prb8_FreeFemale3_usv.wav
M8_Prb8_FreeFemale4_usv.wav
```

The notebook verifies the audio sampling rate and aligns the audio clock with the neural/DLC clock using each session's `vocSyncTime` value.

For the matching neural/session-file layout, see [`../data/README.md`](../data/README.md).