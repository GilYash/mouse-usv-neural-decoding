# Neural/session data

> **Data not included.** The neural/session recordings used in this project were **provided by Dr. Shai Netser from the Lab for Neurobiology of Social Behavior, Sagol Department of Neurobiology, University of Haifa**. The original research files are not redistributed in this repository.

This directory is kept as a placeholder so that users with **authorized access** to the original data can reproduce the notebook's expected local file structure.

## Data access

The source `.mat` files remain subject to the permissions of the originating research group and data owners. They should not be copied, published, or redistributed without authorization.

This public repository contains only the project code, documentation, and final report.

Lab website: https://shlomowagner-lab.haifa.ac.il/

## Expected local files

If you have authorized access to the source data, the notebook expects the following session files in this directory:

```text
M2_Female_1.mat
M2_Female_2.mat
M2_Female_3.mat
M3_Female_1.mat
M3_Female_2.mat
M3_Female_3.mat
M3_Female_4.mat
M6_Female_1.mat
M7_Female_1.mat
M7_Female_2.mat
M7_Female_3.mat
M7_Female_4.mat
M8_Female_1.mat
M8_Female_2.mat
M8_Female_3.mat
M8_Female_4.mat
```

The session files provide the metadata and neural data structures used by the analysis, including `sessionParams`, `vocalizationTimes`, `spikeCounts100ms`, DLC timestamps, and recorded-cell brain-area labels.

For the matching audio-file layout, see [`../vocal_data/README.md`](../vocal_data/README.md).