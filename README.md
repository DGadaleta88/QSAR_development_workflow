# AUTOMATED WORKFLOW FOR QSAR CLASSIFICATION MODEL DEVELOPMENT

The workflow exploit ‘QSAR-ready’ data to develop classification QSAR model. The following functionalities are incorporated int he workflow: 
1) **Descriptors calculation**: based on molecular descriptors called **Continuous and Data-Driven Descriptors (CDDD)** (https://github.com/jrwnter/cddd) [1]
2) **Descriptors selection**: R package **VSURF**, a RF algorithm working in three steps to detect variables related to the activity and eliminate those redundant or irrelevant [2] 
3) **Machine learning methods**: six classification algorithms (k Nearest Neighbors, Random Forest, Gradient Boosting, XGBoost, Support Vector Machine, Multi-Layer Perceptron) are incorporated in the workflow
5) **Hyperparameter tuning**: based on five-fold cross-validation
6) **Training data balancing**: the **Synthetic Minority Oversampling Technique (SMOTE)** is implemented to oversample the minority class by creating new synthetic samples until the two categories are balanced [3]
7) **Extarnal data prediction**: best models selected based on validation performance can be used to predict external chemicals


## Installation

### Install the workflow
1.  Install the last version of KNIME. It can be downloaded at https://www.knime.com/knime-analytics-platform (Windows version, 64bits).
2.	Open KNIME.
3.	Go to *“File -> Import KNIME Workflow”*.
4.	Tick *“Select File:”* and go to *“Browse…”*. Select the *.knwf file of the workflow.
5.	Click to *“Finish”*. 
6.	The QSAR workflow now is in your “KNIME Explorer” menu on the left of the screen. Double click on the workflow to open it.
7.	If some of the plugins used for the workflow are missing, a message will appear asking you to install the missing extensions. Click on *“Ok”*. The procedure will guide you in the installation of the missing extensions. Restart KNIME to make the new plugins working.

### Install cddd
1.	Detailed instruction to install cddd can be found at https://github.com/jrwnter/cddd
2.	From KNIME; go to *“File -> Preferences”*. In the new window, select *“KNIME -> Python”*. Select Python3 as the default version. In the python environment configuration, select *“Conda”*. Select *“cddd”* from the drop-down menu *“Name of the Python 3 Conda environment”*. Click *“Apply and close”*.

### Install VSURF
1.	Download and install the last versions of R for Windows (https://cran.r-project.org/bin/windows/base/) and RStudio (https://www.r-studio.com/it/).
2.	From RStudio, go to the list of packages, the to *“Install”* and import the required packages: *doParallel, foreach, parallel, randomForest, rpart*. Click *“Install”*.
3.	From KNIME; go to *“File”* and to *“Preferences”*. In the new window, select *“KNIME -> R”*. Browse the path to R home directory. Click *“Apply and close”*.


## Use the Workflow

### How to use the workflow – part 1
1.	Prepare the input file. The file should be an Excel file (*.xlsx) with exactly three column. The columns can be in any order and should have an header:
  - SMILES. SMILES should be previously cleaned and normalized. The user can use the workflow from https://github.com/DGadaleta88/data_curation_workflow to perform the data curation. 
  - Categorical binary activities. Ative/inactive, positive/negative, 0/1.
  - List of IDs. IDs should be only numeric.

2.	Load the input file in the *“1. Select training data”* node. Double click on the metanode. Click on *“Browse…“* and select the input file path. Clik *“Ok”*. Right-click on the metanode and then on *"Execute"" in the drop-down menu. If the input file has been correctly loaded, the node will change from red <img src="https://drive.google.com/uc?id=1plZcqFt1GA6ITUJPAmCrvnx_lfijgJYT" alt="alt text" width="30" height="15"> to green <img src="https://drive.google.com/uc?id=1nQHbYya0A9MksQKUKIoUtboMduuqnhmR" alt="alt text" width="30" height="15">. If the node return an error <img src="https://drive.google.com/uc?id=1F1sRAttzeC2iDO9Vww0qBOCLtgwKx8cC" alt="alt text" width="30" height="15">, some problems occurred during the loading of the input file.

3.	Double click on the node "2. Select columns". Select the columns of the input file including the SMILES, the activity and the numeric ID from the drop-down menus. Clik *“Ok”*. Right click on the metanode, then on "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" in the drop-down menu. The node will change from red <img src="https://drive.google.com/uc?id=1plZcqFt1GA6ITUJPAmCrvnx_lfijgJYT" alt="alt text" width="30" height="15"> to green <img src="https://drive.google.com/uc?id=1nQHbYya0A9MksQKUKIoUtboMduuqnhmR" alt="alt text" width="30" height="15">.

4.	Double click on the node "3. Settings dataset". 
 - Select the positive and negative classes in the dataset.
 - Select *“duplicate removal”* if duplicates are present in the dataset. Duplicate records are checked at SMILES level, and activities are aggregated. If conflicting activities are present for duplicates, then they are removed from the dataset.
 - Select *“descriptors selection”* to apply VSURF to narrow the number of descriptors.
 - Select the percentage of samples to include in the training set. By default, the 80% of samples is included in the training set.
Click *“Ok”*. Right click on the metanode, then on "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" in the drop-down menu. The node will change from red <img src="https://drive.google.com/uc?id=1plZcqFt1GA6ITUJPAmCrvnx_lfijgJYT" alt="alt text" width="30" height="15"> to the "running" state <img src="https://drive.google.com/uc?id=1V1Rkzz9Al7XXmLAnd-goz8_6wCqzfp_7" alt="alt text" width="30" height="15">. If the first part of the workflow is successfully executed, the node will change to green <img src="https://drive.google.com/uc?id=1nQHbYya0A9MksQKUKIoUtboMduuqnhmR" alt="alt text" width="30" height="15">.

5.	Double click on the node "4. Settings ML". 
 - Select *“SMOTE”* to artificially balance the samples of the training set between the two categories.
 - Select the ML methods to use to develop QSAR models.
 - Select the statistical parameter to be optimized during model selection between *Balanced Accuracy (BA)* and *Matthew’s Correlation Coefficient (MCC)* from the drop-down menu. 
Click *“Ok”*. Right click on the metanode, then on "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" in the drop-down menu. The node will change from red <img src="https://drive.google.com/uc?id=1plZcqFt1GA6ITUJPAmCrvnx_lfijgJYT" alt="alt text" width="30" height="15"> to the "running" state <img src="https://drive.google.com/uc?id=1V1Rkzz9Al7XXmLAnd-goz8_6wCqzfp_7" alt="alt text" width="30" height="15">. If the first part of the workflow is successfully executed, the node will change to green <img src="https://drive.google.com/uc?id=1nQHbYya0A9MksQKUKIoUtboMduuqnhmR" alt="alt text" width="30" height="15">.

6.	Right click on the *“Validation performance”* Interactive Table node and to *“View: Table View"* in the drop-down menu to visualize the performance of the developed models on the test set and the values assigned after the hyperparameter tuning. Models are sorted based on the performance described by the statistical parameter selected above.

### Prediction of externa chemicals

1.	The user can load a list of SMILES to predict with the top-performing model previously developed. Double click on *“5. Select external data”* node to open the settings. Click on *“Browse…”* to load the list of compounds to predict. The input should be an Excel file with two columns including the list of SMILES to predict and numerical IDs. Click on *“Ok”*. Right click on the *“Select input”* node and then on "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" from the drop-down menu. 
2.	Double click the node *“6. Select columns”* to select the columns of the input file including the ID, the SMILES from the drop-down menu. Click *“Ok”*, then right click on the node and to "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" in the drop-down menu to run the node. 
3.	Right click on the node *“7. Predict external data”* and to "<img src="https://drive.google.com/uc?id=1i-FU-Z8GCLbQ2-R64MeriohDkauqKySg" alt="alt text" width="20" height="20"> Execute" in the drop-down menu to run the node. 
4.	Right click on *“8. External prediction”* Interactive Table node and to *“View: Table View"* in the drop-down menu to visualize the predictions made by the best model for external chemicals. Probabilities associated to the predictions are also provided, with predictions associated to higher probabilities being more reliable.

### Tips

GB and XGB are highly computationally and time demanding. It is suggested to run them individually if the workflow is run on a moderately performing PC. It is possible to edit the knime.ini file (in the KNIME folder) and change the row starting with -Xmx to increase the RAM available in KNIME.

### References

[1] Winter R, Montanari F, Noé F, et al (2019) Learning continuous and data-driven molecular descriptors by translating equivalent chemical representations. 10:1692–1701

[2] Genuer R, Poggi JM, and Tuleau-Malot C (2010) Variable selection using random forests. Pattern Recognit. Lett 31:2225–2236

[3] Chawla NV, Bowyer KW, Hall LO et al (2002) Smote: Synthetic minority over-sampling technique. J Artif Intell. Res 16:321–357
