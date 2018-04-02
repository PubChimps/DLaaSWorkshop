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


## Provision IBM Cloud Services  <a name="provision"></a>

Sign into IBM Cloud to see the welcome page below. New services are provisioned by selecting “Catalog.” Search for and select “Watson Studio.”

<img src="images/Picture1.png">
<br>
<img src="images/Picture2.png">

Make sure that a Lite plan is being selected for all services. Click “Create,” then search for and Lite “Machine Learning” and “Object Storage” services in the Catalog. Once finished, the IBM Cloud logo in the top left should bring you to a page shown below.

<img src="images/Picture3.png">

## Create Watson Studio Project / Connect Services <a name="create"></a>

Now a new Watson Studio Project will be created that utilizes all the IBM Cloud services just provisioned. Access Watson Studio by selecting the created Watson Studio service from the IBM Cloud dashboard, then select “Get Started” and “New project” on the right. The type of project needed is “Complete.”

<img src="images/Picture4.png">

<img src="images/Picture5.png">

Name the new project “DLaaS Workshop.” The IBM Cloud Object Storage service that was provisioned is already associated with our project. Select “Create” at the bottom right.

<img src="images/Picture6.png">

Once the new project is created, select “Settings” in the far-right menu. Scroll down until “Associated services” is visible and under “Add service” select “Machine Learning” to add the existing Watson Machine Learning service that was provisioned. New all over our services are associated with each other in our Watson Studio project and are ready for Deep Learning models.

<img src="images/Picture7.png">

## Add Data <a name="add"></a>

Buckets will now be created in the IBM Cloud Object storage service to so that the mnist training data and the results of deep learning training algorithms can be stored. The data is loaded in two different formats, demonstrated in “AddMNistData.ipynb”. Under assets, we will add a “New notebook” and point it to the notebook hosted on GitHub while keeping the default runtime already specified.

<img src="images/Picture8.png">
<br>
<img src="images/Picture9.png">

## Build Neural Networks <a name="build"></a>

Two neural networks will be created, a convolutional neural network (cnn) by using Watson Studio’s Neural Network Modeler along with a multilayer perceptron (mlp) in Keras.

### With Neural Network Modeler <a name="wnnm1"></a>
The steps to create a cnn using Neural Network Modeler are shown in the following annotated images.

| ![image10.png](images/Picture10.png) | 
|:--:| 
| *In the “Assets” section of the “DLaaS Workshop” project, scroll and select “New Flow” from “Modeler flows”* |

| ![image11.png](images/Picture11.png) | 
|:--:| 
| *Name the new model “mnist-nnm” and select “Neural Network Modeler” as the flow type. Then select “Create”* |

| ![image12.png](images/Picture12.png) | 
|:--:| 
| *Use the Palette to the left to add the 10 neural network nodes shown above and connect them as follows.* |

Now the nodes must be configured for our mnist data, a node’s settings are visible by double-clicking it or selecting ⋮ then “open.”

|![image13.png](images/Picture13.png) | 
|:--:| 
| *Edit the “Data” section to point to the pickle object that were created and stored in the mnist-nnm-… bucket from our Notebook, then select “Settings”* |

### With Keras <a name="wkeras"></a>


## Run Experiments <a name="run"></a>
### With Neural Network Modeler <a name="wnnm2"></a>
### With Hyperparameter Optimization <a name="whpo"></a>
  
