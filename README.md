<img src="images/IBM.png">

# DLaaSWorkshop
# Table of Contents
1. [Provision IBM Cloud Services](#provision)
2. [Create Watson Studio Project / Connect Services](#create)
3. [Add Data](#add)
4. [Build Neural Networks](#build)
	1. [With Neural Network Modeler](#wnnm1)
	2. [With Keras](#wkeras)
5. [Run Experiments](#run)
	1. [With Neural Network Modeler](#wnnm2)
	2. [With Hyperparameter Optimization](#whpo)
6. [Predict With Model](#predict)


## Provision IBM Cloud Services  <a name="provision"></a>

Sign into IBM Cloud to see the welcome page below. New services are provisioned by selecting “Catalog.” Search for and select “Watson Studio.”

<img src="images/Picture01.jpg">
<br>
<img src="images/Picture02.jpg">

Make sure that a Lite plan is being selected for all services. Click “Create,” then search for and add Lite “Machine Learning” and “Object Storage” services in the Catalog. Once finished, the IBM Cloud logo in the top left should bring you to a page shown below.

<img src="images/Picture03.jpg">

## Create Watson Studio Project / Connect Services <a name="create"></a>

Now a new Watson Studio Project will be created that utilizes all the IBM Cloud services just provisioned. Access Watson Studio by selecting the created Watson Studio service from the IBM Cloud dashboard, then select “Get Started” click through prompts. Once in Watson Studio select “New project” on the right. The type of project needed is “Complete.”

<img src="images/Picture04.jpg">

<img src="images/Picture05.jpg">

Name the new project “DLaaS Workshop.” The IBM Cloud Object Storage service that was provisioned is already associated with our project. Select “Create” at the bottom right.

<img src="images/Picture06.jpg">

Once the new project is created, select “Settings” in the far-right of the top menu. Scroll down until “Associated services” is visible and under “Add service” select “Machine Learning” to add the existing Watson Machine Learning service that was provisioned. Now all over the services are associated with each other in the Watson Studio project and are ready for Deep Learning models.

<img src="images/Picture07.jpg">

## Add Data <a name="add"></a>

Buckets will now be created in the IBM Cloud Object storage service to so that the mnist training data and the results of deep learning training algorithms can be stored. The data is loaded in two different formats, demonstrated in “AddMNistData.ipynb”. Under assets, we will add a “New notebook”, chose "From URL," name the notebook "AddMnistData", and to point it to https://raw.githubusercontent.com/PubChimps/DLaaSWorkshop/master/AddMnistData.ipynb as the "Notebook URL" while keeping the default runtime already specified.

<img src="images/Picture08.jpg">
<br>
<img src="images/Picture09.jpg">

| ![Picture10.jpg](images/Picture10.jpg) | 
|:--:| 
| *To add the mnist data to Cloud Object Storage, press Shift+Enter through the cells of the notebook and observe output* |

| ![Picture11.jpg](images/Picture11.jpg) | 
|:--:| 
| *The Notebook will ask for Cloud Object Storage credentials, these can be found be selecting the provisioned service in the IBM Cloud dashboard, then "Service Credentials, then coping "WDP-Admin-..." to clipboard and pasting into notebook* |

## Build Neural Networks <a name="build"></a>

Two neural networks will be created, a convolutional neural network (cnn) by using Watson Studio’s Neural Network Modeler along with a multilayer perceptron (mlp) in Keras.

### With Neural Network Modeler <a name="wnnm1"></a>
The steps to create a cnn using Neural Network Modeler are shown in the following annotated images.

| ![Picture12.jpg](images/Picture12.jpg) | 
|:--:| 
| *In the “Assets” section of the “DLaaS Workshop” project, scroll and select “New Flow” from “Modeler flows”* |

| ![Picture13.png](images/Picture13.jpg) | 
|:--:| 
| *Name the new model “mnist-nnm” and select “Neural Network Modeler” as the flow type. Then select “Create”* |

| ![Picture14.jpg](images/Picture14.jpg) | 
|:--:| 
| *Use the Palette to the left to drag and drop the 10 neural network nodes shown above and connect them as follows.* |

Now the nodes must be configured for the mnist data, a node’s settings are visible by double-clicking it or selecting ⋮ then “Open.”

|![Picture15.jpg](images/Picture15.jpg) | 
|:--:| 
| *Edit the “Data” section of the "Input Data" node by selecting "Create a Connection", then point to the pickle object that was created and stored in the **"mnist-nnm-…"** bucket from the AddMnistData Notebook, then select “Settings”* |

|![Picture16.jpg](images/Picture16.jpg) | 
|:--:| 
| *Configure the “Image Data” settings. Images are 28x28, **10 classes** and is in **Python Pickle** format. Select “Save”* |

|![Picture17.jpg](images/Picture17.jpg) | 
|:--:| 
| *The settings for the “Conv 2D” layer are on the left. The settings that cannot be seen in the screenshot are left to their default. Make sure the **Trainable** box is checked. Select "Save"* |

|![Picture18.jpg](images/Picture18.jpg) | 
|:--:| 
| *The “ReLU” activation layer will have a “Negative slope” of 0.3* |

|![Picture19.jpg](images/Picture19.jpg) | 
|:--:| 
| *The “Pool 2D” settings. Make sure the **Trainable** box is checked for this node and the “Conv 2D” node* |

|![Picture20.jpg](images/Picture20.jpg) | 
|:--:| 
| *The “Dense” node before the softmax classifier must have the same number of nodes as classes in the data, in this case 10.* |

|![Picture21.jpg](images/Picture21.jpg) | 
|:--:| 
| *All other nodes are left in default settings. The “Validation Success” notification should now be seen. Select “Publish training definition” in the top right.* |

|![Picture22.jpg](images/Picture22.jpg) | 
|:--:| 
| *Name the training definition “mnist-nnm-training-def” and associate it with the WML instance. Select "Publish"* |

|![Picture23.jpg](images/Picture23.jpg) | 
|:--:| 
| *Once the training definition is successfully created, “train it in an experiment.”* |

|![Picture24.jpg](images/Picture24.jpg) | 
|:--:| 
| *Name the experiment “mnist-nnm-experiment” and click “Select” under “Cloud Object Storage…”* |

|![Picture25.jpg](images/Picture25.jpg) | 
|:--:| 
| *Select the “mnist-nnm-…” bucket containing training data. Create a new object storage bucket to store results. This has to be a globally unique bucket name, so I added my last name. Click “Select,” then "Add training definition" .* |

|![Picture26.jpg](images/Picture26.jpg) | 
|:--:| 
| *Select an "Existing training definition“ and use the "mnist-nnm-training-def" created earlier. 1 X NVIDIA Tesla K80 (2 GPU)” will be the "Compute plan" (Others are not available on the IBM Cloud Lite plan). Click “Select” and "Create and run."  The neural network will now begin training on the mnist data. As it is training continue to the next section.* |

### With Keras <a name="wkeras"></a>
Under the assets section of the “DLaaS Workshop” project, add a new notebook with "From URL" as the source, name it “mnist-hpo” and copy/paste https://raw.githubusercontent.com/PubChimps/DLaaSWorkshop/master/mnist-hpo.ipynb as the Notebook URL. **"Default Anaconda S (4vCPU and 16GB RAM)** will allow the Keras model to train faster. Select "Create Notebook"

![Picture27.jpg](images/Picture27.jpg)

## Run Experiments <a name="run"></a>
### With Neural Network Modeler <a name="wnnm2"></a>
The experiment that was created in the previous section should be done training. Find “mnist-nnm-experiment” under the Experiments section of the “DLaaS Workshop” project. 

|![Picture28.jpg](images/Picture28.jpg) | 
|:--:| 
| *“mnist-nnm-training-def” should be in the Completed section. Its output logs can be view by selecting it.* |

### With Hyperparameter Optimization <a name="whpo"></a>
A new experiement will now be added to illustrate Watson Studio’s Hyperparameter Optimization. Select “New experiment” in the assets section of the “DLaaS Workshop,” name the new experiment “mnist-hpo-experiment” and click “Select.”

![Picture29.jpg](images/Picture29.jpg)
<br>

|![Picture30.jpg](images/Picture30.jpg) | 
|:--:| 
| *The buckets that were created in part 2 of “AddMnistData.ipynb” will be used here* |

|![Picture31.jpg](images/Picture31.jpg) | 
|:--:| 
| *Add a training definition and name it “mnist-hpo-training-def” and upload [this zip file](https://github.com/PubChimps/DLaaSWorkshop/blob/master/MNIST.zip). It contains the model written in Keras earlier, as well as other code.* |

|![Picture32.jpg](images/Picture32.jpg) | 
|:--:| 
| *Choose the "Framework" and "Execution command" illustrated above, the same Compute plan from earlier, and “random” as the Hyperparameter optimization method, fill out the rest of the page as follows before selecting “Add Hyperparameter”* |

|![Picture33.jpg](images/Picture33.jpg) | 
|:--:| 
| *Instruct Watson Studio to vary the hyperparameter "learning_rate" as follows. Select "Add"* |

|![Picture34.jpg](images/Picture34.jpg) | 
|:--:| 
| *The experiment is ready. Select "Create" then "Create and run"* |

## Predict With Model <a name="predict"></a>
Pick a model to save and select it by clicking ⋮ then "Save Model"

<img src="images/Picture35.jpg">

|![Picture36.jpg](images/Picture36.jpg) | 
|:--:| 
| *Name the model "mnist-hpo-saved-model". Select "Save"* |

Use the saved model in the notebook [PredictingWithModel](https://raw.githubusercontent.com/PubChimps/DLaaSWorkshop/master/PredictWithModel.ipynb)

