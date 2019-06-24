# Daily Notes
## 12th of June 2018

### Task 1: Installing an automated image collector using an XU4 
The following is a description of the work done for the day. 

#### Task 1.1 : Installing linux on an XU4

##### 1.1.1 Downloading the XU4 Image
- Download the linux image for the XU4 through: https://odroid.in/ubuntu_16.04lts/
- A compatible version of our choosing can be used.
  eg.: https://odroid.in/ubuntu_16.04lts/ubuntu-16.04-minimal-odroid-xu3-20160706.img.xz <br />
**(Make sure to download a minimal version for the XU4)**

##### 1.1.2 Installing an Image flasher software 
- GUI based image flasher software 'etcher' was downloaded via https://etcher.io/
- Once downlaoded, extract and run the said file. 

##### 1.1.3 Flashing an SD Card 
- Running the resulting file from the previous task gives a GUI directing on how to flash the sd card.
-- Need to chose the image file and the destination of the SD Card 

##### 1.1.4 Installing Screen on Ubuntu 
- run the following commands on the linux command window 
```
sudo apt-get update
sudo apt-get install screen
```
##### 1.1.5 Setting up the USB-UART Module Kit
- Initially connect the UART through one of the USB ports on the PC. 
- Check wehather its properly connected through by typing in ```lsusb ```  on the command window. The vendor and the product keys of the UART would be visible when done so. 
- Then type in ```sudo modprobe usbserial vendor=0x1d09 product=0x4000 ```
in this example 1d09 is the vendor key and the product key is 4000. <br />
**Make sure to replace the vendor key and the product key with what you have**   

- To figure out which USB port the UART is plugged in, use the command ``` dmesg ```
- Then type in ``` screen /dev/ttyUSB0 115200 ``` 
- The resulting screen is to be utilized as the main interface for the XU4. <br />
**Helpful Tutorial: https://www.youtube.com/watch?v=HuCh7o7OsGk**

##### 1.1.6 Booting the XU4
- Connect the SD card and the UART to the XU4. 
- Connect Power. After the initial process, it will ask for a user name and password. Typically they will be as follows.
-- User Name: odroid 
-- Password: odroid <br/>
**(In some cases the username will be 'root')**

##### 1.1.7 Updating the XU4 
- After logging in, type in ```sudo halt``` and provide the password if needed.
- The interface will let you know once the system is halted. Afterwards, remove power and connect the ethernet cable as well as the webcam. 
- Reboot the XU4 by reconnecting power. Log in via the usual manner and update the system by running the following commands.
``` 
sudo apt-get update        # Fetches the list of available updates
sudo apt-get upgrade       # Strictly upgrades the current packages
sudo apt-get dist-upgrade  # Installs updates (new ones)
````
### Task 2 : Installing other dependancies for the Automated Image Collector 

#### 2.1 Installing Git 
- run the following commands on the XU4 interface
```
sudo apt-get update
sudo apt-get install git-core
```
- to check the installation you can run ```git --version ```

#### 2.2 Installing fswebcam 
- run the following commands on the XU4 interface
```
sudo apt-get update
sudo apt-get install fswebcam
```
### Task 3 :Setting up the Repository and setiing up a Cronjob 
within the folder named 'offline image collector'
#### 3.1 Cloning the Git Repository
- Navigate to the home directory of the XU4 
- Make a new directory named 'repos' via ``` mkdir repos ```
- Navigate to the cretaed directory via ``` cd repos```
- Clone the GIT Hub Repository via ``` git clone https://github.com/waggle-sensor/summer2018.git```

#### 3.2 Modifying the acquire_image.sh script 
 - Navigate to the directory where the shell script for the image colloector is placed <br/>
        ```cd /home/repos/summer2018/wijeratne/codes/offline_image_collector/```
- Make a new folder to store the files ``` mkdir Photos ```within the folder named 'offline image collector'.
- Open the shell script through nano by typing in ``` nano acquire_images.sh ``` again within the folder named 'offline image collector'. 
- Replace the whole script by the follwoing code on the file 'acquire_images.sh'
```
#!/bin/sh
fswebcam --png 0 --device /dev/video0 --skip 4 --input 0 --resolution 640x480 --no-banner --save /home/waggle/photos/Cloud_Pic_%Y_%m_%d_%H_%M_%S.png
```
- Save the modified file on the same folder and run the command ``` chmod +x acquire_images.sh``` to have execute permissions for the script.

#### 3.3 Setting up the cronjob 
- Type in the follwing code to set up a crontab:
    ``` crontab -e ```
- Give in the following cronjob in the resulting window: <br/>
```  */5 * * * * cd /home/repos/summer2018/wijeratne/codes/offline_image_collector && ./acquire_image.sh ```
- Finally save the cron job <br/>
**(Check up on the folder named 'photos' every 5 minutes to see if the images are being saved.)**  <br/>  <br/>

## 14th of June 2018 

The follwing tasks are geared towards having XU4 outside so that it will have a collection of photos that will be used to verify the cloud detection script.

### Task 4: Placing the XU4 which is connected to a camera outside. 

#### 4.1 Doing a temporory housing for the XU4 and placing it outside
- Use a plastic tool holder to work as the housing. 
- Leave a dome which is used in the actual sensor ontop of the camera for consistancy.
- Place the temporory housing outside. Reboot the XU4 and leave eit out for a few hours.<br/>
**(Make sure to check the weather before placing it outside)** 

#### 4.2 Installing nmap
- run the following commands on the Linux Command Line
```
sudo apt-get update
sudo apt-get install nmap
```
#### 4.3 Finding the ip address of the Desktop
- Type in ```ifconfig``` on the Desktop to figure out the ip address of the current machine. The output will be as follows.
 ```
 eno1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500   
 inet 10.10.10.144 ....      
```
The set of numbers infront  of the key inet gives the ip of the current machine. In this example the ip of the current machine is '10.10.10.144'. <br />

#### 4.4 Scan the network for the XU4
- Connect the  XU4 to the router which the desktop is hooked up.
- On the linux Command line type in ```nmap 192.168.1.0-255 -p 80 ```. This will scope through all the ip addresses connected to the router.``` - p 80 ``` added to avoid listing all ports of a given ip. 
- Record the IP address for the XU4

#### 4.5 Connect to the XU4 though SSH 
- On the command line of the PC type in ```ssh root@192.168.1.55```
in this example the user name and the password for the XU4 are 'root' and 'odroid'. <br />
**Make sure to replace the username and the password with what you have** <br />
- type ``` exit``` to come back to the local machine.   


#### 4.6 Downloading the folder with the photos from the XU4

- On the command line of the PC type in <br/>
```
scp -r root@192.168.1.55:/home/repos/summer2018/wijeratne/codes/offline_image_collector/photos /home/lakithaomal/repos/summer2018/wijeratne/codes/cloud_detection
```
In this example the XU4 ip address is *192.168.1.55* ,the XU4 Directory is which the files are copied from is  */home/repos/summer2018/wijeratne/codes/offline_image_collector/photos* and the XU4 user name is *root*. The desination of the host PC is */home/lakithaomal/repos/summer2018/wijeratne/codes/cloud_detection*. <br />
**Make sure to modify the usernames and the directories with the appropriate credentials**
- Check to see if the files are copied into the local directory.

## 15th of June 2018 
The folling tasks are geared upon the initiation of the Bike Bottle Sensor Project.

### Task 5 : Documentation of the Bike Bottle Sensor Design

#### 5.1 Doing the Bike Bottle Design Overveiw
- Define the constraints and the necessary objectives of the proposed project<br />
**The link to the said docuement is found [here](https://github.com/waggle-sensor/summer2018/tree/master/wijeratne/codes/bike_bottle_design)**
(Further modifications were made after contatcting Prof. Lary)

The folling tasks are geared towards checking the Accuracy of the Cloud Detection App.

### Task 6 : Verification of the Cloud Detector App
####  6.1 Getting the Images ready for verification 

- For the verification process an ordered set of images are needed. The Images downloaded from the XU4 are named randomly. As such the said images are renamed in an orderly manner using the code given **[here](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/python/Renaming.py)**

- The renamed files contain a timestamp which is displayed on the lower right hand corner of each image. This which will interefear with the prediction. As such the resulting files are croped via the code found **[here](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/python/Image_Cropper.py)**


## 18th - 22nd of June 2018

#### 6.2 Downloading pre classified data for verification
- A data set which is pre classified for the the task at hand can be found [here](http://vintage.winklerbros.net/swimseg.html).
On this link, fill in a seprate form as par a request for the data set featured. Once a request is sent, and email with the data set requested will be sent. 


### Task 7 : Writing a separate App to Distinguish between Clouds and Sky pixels
The paper provided in this [link](http://vintage.winklerbros.net/Publications/jstars2017.pdf) gives a distinct way in which the classification can be done. The study is based on a method where each pixel is handled individually contarary to the usual means of image segmentation. The following is a description of the implimentation discussed in the paper.

**The data set provided by the [Singapore Whole sky IMaging SEGmentation Database](http://vintage.winklerbros.net/swimseg.html)
contained 1000 images. Due to resource constraints, the initial study is doen using only 300 images.**


#### 7.1 Training a Random Forest Classifier

The python script written to train a Random Forest Classifier is described here.

- Initially the color spaces are separated for all the images. These color spaces used are listed below.

   - RGB Color SPace 
   - HSV Color Space 
   - LAB Colour Space 
  
  The following scripts were used to gain seprated colour space images:
  ``` 
    Input_Image_RGB = cv2.cvtColor(Input_Image, cv2.COLOR_BGR2RGBA)
    # Gaining HSV Images
    Input_Image_HSV = cv2.cvtColor(Input_Image, cv2.COLOR_BGR2HSV)
    # Gaining LAB Images
    RGB_for_LAB = io.imread(input_path)
    Input_Image_LAB = color.rgb2lab(RGB_for_LAB)
  
  ```
- These were converted into Arrays using numpy via the script ``` np.array()```. In order to get separate feature vectors for a given pixel each dimention of all color spaces are converted into 1D arrays via these scripts.

```
    # Converting Each Image into 1D Vectors
    One_D_Image_Red = np.transpose(np.matrix(Image_Array_RGB[:, :, 0].ravel()))
    One_D_Image_Green = np.transpose(np.matrix(Image_Array_RGB[:, :, 1].ravel()))
    One_D_Image_Blue = np.transpose(np.matrix(Image_Array_RGB[:, :, 2].ravel()))

    One_D_Image_H = np.transpose(np.matrix(Image_Array_HSV[:, :, 0].ravel()))
    One_D_Image_S = np.transpose(np.matrix(Image_Array_HSV[:, :, 1].ravel()))
    One_D_Image_V = np.transpose(np.matrix(Image_Array_HSV[:, :, 2].ravel()))

    One_D_Image_L = np.transpose(np.matrix(Image_Array_LAB[:, :, 0].ravel()))
    One_D_Image_A = np.transpose(np.matrix(Image_Array_LAB[:, :, 1].ravel()))
    One_D_Image_B = np.transpose(np.matrix(Image_Array_LAB[:, :, 2].ravel()))

```
-The final feature vector composed of these inputs as well as the ratio of Red to Blue Pixel values, the difference of Red to blue pixels and two other features which comprised of red and blue colour pixel values. 

The follwing code is used to gain the complete feature vector for a given pixel
```
 # Writing the full Feature Vector
    One_D_Image = np.hstack((One_D_Image_Red, One_D_Image_Green, One_D_Image_Blue,\
                             One_D_Image_H, One_D_Image_S, One_D_Image_V, \
                             One_D_Image_L, One_D_Image_A, One_D_Image_B, \
                             One_D_Image_Red/One_D_Image_Blue, np.subtract(One_D_Image_Red, One_D_Image_Blue), \
                             (One_D_Image_Blue-One_D_Image_Red)/(One_D_Image_Blue+One_D_Image_Red),\
                             One_D_Chroma
                             ))
```
**Due to resource constraints a lesser number of features may be used for the final study**

- Using the target images a target feature vector is aquired. The code was designed in a manner where in which each cloud pixel will have a value 1 and each sky pixel will have value 0. The follwing is the script used:

```
 # For a Given Image a Target Vector is Generated
    Input_Image_Binary = cv2.imread(input_path)
    Image_Array_Binary = np.array(Input_Image_Binary)
    Image_Shape = Image_Array_Binary.shape
    One_D_Binary = np.transpose(np.matrix(Image_Array_Binary[:, :, 1].ravel()))
    One_D_Binary = One_D_Binary.astype(float) / 255
```

- Once all feature vectors and target vectors are concatinated, A random Tree Classifier is trained and saved for later use. The implimentation is as follows:

```
    # Instantiate model with 10 decision trees
    Random_Forest_Model = RandomForestRegressor(n_estimators=10, random_state=42)
    # Train the model on training data
    Random_Forest_Model.fit(All_Features_Training, np.ravel(All_Targets_Training, order='C'))
    # Saving the Model 
    Model_Save_File_Name = 'Cloud_App_Via_Sklearn_Model.sav'
    pickle.dump(Random_Forest_Model, open(Model_Save_File_Name, 'wb'))

```
#### 7.2 Verification of the Random Forest Classifier 

- On the previous step the around 300 images provided were divided into training and testing sets using
```Train_Index, Test_Index = train_test_split((Indexes), test_size = 0.3, random_state = 42) ```

For this study the testing data set contained around 30% percent of the total data set. 

- On this module initially the trained Random Forest Classifier is loaded via 
```
Model_Save_File_Name = 'Cloud_App_Via_Sklearn_Model.sav'
loaded_Random_Tree = pickle.load(open(Model_Save_File_Name, 'rb'))
```

- Following is a description of the Feature Vectors and Target Vectors gained within the script:
  -- Each pixel of the Training data set images are saved as ```All_Features_Training``` 
  -- Each pixel of the Testing data set images are saved as ```All_Features_Testing``` 
  -- True classifications of the Training images are saved as ```All_Targets_Training ```
  -- True classifications of the Testing images are saved as ```All_Targets_Testing```
  
  - Using the loaded Random Forest Classifier and the featscreen /dev/ttyUSB0 115200ure Vectors Specified, Predictions are made usind the following scripts. Separate studies were done for the Training and Testing data sets.
  
  ```
   Predictions_Pre_Training = loaded_Random_Tree.predict(All_Features_Training)
   Predictions_Training = np.transpose(np.matrix(np.array(Predictions_Pre_Training)))
  
   Predictions_Pre_Testing = loaded_Random_Tree.predict(All_Features_Testing)
   Predictions_Testing = np.transpose(np.matrix(np.array(Predictions_Pre_Testing)))
  
  ```
  - Since the predictions needed to be binary for verication, they were modified accordingly.
  
  ```
    Predictions_Training[Predictions_Training < 0.5] = 0
    Predictions_Training[Predictions_Training >= 0.5] = 1
  
    Predictions_Testing[Predictions_Testing < 0.5] = 0
    Predictions_Testing[Predictions_Testing >= 0.5] = 1
  
  ```

  The Accuracies for the Training and Testing data are recorded using the folling scripts.
  
  ```
  Correct_Predictions_Training = sum(Predictions_Training == All_Targets_Training)
  print("Training Accuracy Sklearn:", Correct_Predictions_Training/len(All_Targets_Training))

  Correct_Predictions_Testing = sum(Predictions_Testing == All_Targets_Testing)
  print("Testing Accuracy Sklearn:", Correct_Predictions_Testing / len(All_Targets_Testing))
  ```
  
#### 7.3 Figuring out the most importaint features 

The Random Forest Classifier is capable of giving a rank to each Feature with respective to there imporatance in the classification. The following script is used to obtain the importance rank of each feature.

```
   Importances = list(loaded_Random_Tree.feature_importances_)

    Features_Description = ["Red", "Green", "Blue", \
                            "H", "S", "V", \
                            "L", "A", "B", \
                            "Red/Blue", "Red-Blue", \
                            "(Blue-Red)/(Blue+Red)", \
                            "Chroma"]

    Features_Description_Array = np.array(Features_Description)
    Features_Description_List = list(Features_Description)

    # List of tuples with variable and importance
    Feature_Importances = [(feature, round(importance, 2)) for feature, importance in
                           zip(Features_Description_List, Importances)]

    Feature_Importances = sorted(Feature_Importances, key=lambda x: x[1], reverse=True)

```
The follwing Graph depicts the Importance each feature. 
![Feature Importaince](https://raw.githubusercontent.com/waggle-sensor/summer2018/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Feature_Importaince.png?token=AYVA9Ez9wVMksjyBNi5N0Odu5IyGfC1kks5bXeyDwA%3D%3D)

**These results may be useful, if lessor number of features are to be used due to resasource constraints**

#### 7.4 Comparison to the Caffe Classifier

A separate classification is done using the Caffe Classifier. Further details on the Caffe Classification can be found [here](https://github.com/waggle-sensor/plugin_manager/tree/master/plugins/openCV). Using the images gained from the caffe classification , a separate veification is done using the SWMSEG Data set. The verication is done using the same means discussed for the Sklearn(Random Forest Classifier).

The Following table compares the predictions made through either classifier. 

|     Accuracy Estimations     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |    80%   |   84%   |
| Sklearn (Random Forest)      |    90%   |   85%   |


|     [Confusion Matrices]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |  ![Training Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Training_Confusion_Matrix_Caffe_Classifier.png)        |  ![Testing Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Testing_Confusion_Matrix_Caffe_Classifier.png)        |
| Sklearn (Random Forest)      |![Training Sklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Training_Confusion_Matrix_Sklearn_Classifier.png)       | ![TestingSklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Testing_Confusion_Matrix_Sklearn_Classifier.png)          |


|     [Confusion Matrices Normalized]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |  ![Training Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Testing_Confusion_Matrix_Normalized_Caffe_Classifier.png)        |  ![Testing Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Testing_Confusion_Matrix_Normalized_Caffe_Classifier.png)        |
| Sklearn (Random Forest)      |![Training Sklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Training_Confusion_Matrix__Normalized_Sklearn_Classifier.png)       | ![TestingSklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_Vs_Caffe_Reduced_Classification/Testing_Confusion_Matrix__Normalized_Sklearn_Classifier.png)          |


#### 7.5 Writing the final Module 
The post processing via the resulting image of the Sklearn Classifier is done through this model. The eventual results that are to be obtained are as follows.
- Cloud Percentage of the sky 
- For the Sky pixels,
  - Average pixel value in Blue 
  - Average pixel value in Green 
  - Average pixel value in Red

- For the Cloud pixels,
  - Average pixel value in Blue 
  - Average pixel value in Green 
  - Average pixel value in Red
  
  
A description of how these were calculated are given below:

- Initially the image to be studied is loaded via a system Argument. 
``` 
path_src = sys.argv[1]
Original_Image = cv2.imread(path_src)
```

- From the premade Sklearn module, the prediction is made isitailly done. The module is loaded via pickle. Afterward , the usual preprocessing of the prediction is done.
```
    print("Loading the Random Forest Model")
    Model_Save_File_Name = 'Cloud_App_Via_Sklearn_Model_Reduced.sav'
    loaded_Random_Tree = pickle.load(open(Model_Save_File_Name, 'rb'))
    print("Done Loading")


    # Gaining The Prediction
    print("Gaining the Prediction...")
    Prediction = loaded_Random_Tree.predict(Features)
    Prediction = np.transpose(np.matrix(np.array(Prediction)))

    Prediction[Prediction < 0.5] = 0
    Prediction[Prediction >= 0.5] = 1

```

- At this point a the cloud percentate of the sky is gained via ```Cloud_Percentage = sum(Prediction)/len(Prediction)``` 
- A separate mask of similar size of the original image is made. The said mask will have 1's for cloud pixels and 0's for sky pixels. Then its mulitplied by the orginal image via ```  Only_Clouds = cv2.multiply(Original_Image_float, Cloud_Pixels_Normalized) ``` . The resulting image is used to gain the average pixel values of clouds.
The follwing scripts illustrates the process,

```
# Summing up the pixel values for each Pallaette 
Color_Sum_Blue_Sky  = Only_Sky[:,:,0].sum()
Color_Sum_Green_Sky = Only_Sky[:,:,1].sum()
Color_Sum_Red_Sky   = Only_Sky[:,:,2].sum()

Cloud_Pixel_Count  = np.sum(Binary_Image[:,:,0] == 255  
    
Average_Pixel_Value_Blue_Sky = Color_Sum_Blue_Sky/Sky_Pixel_Clount
Average_Pixel_Value_Green_Sky = Color_Sum_Green_Sky/Sky_Pixel_Clount
Average_Pixel_Value_Red_Sky = Color_Sum_Red_Sky/Sky_Pixel_Clount 
```
- Similar Measures were taken to gain the Average Pixels values of the sky. 

- An example of an imlpimentation of the module is given below,
```
python CloudApp_Final_Module_Reduced.py 0001.png
Loading the Random Forest Model
Done Loading
Gaining the Prediction...
Writing Resulting Images ...
Percentage of Clouds:  0.7765027777777778 %
Average Pixel Value in Blue for the Sky:  139.52020283622716
Average Pixel Value in Green for the Sky:  108.23629426167365
Average Pixel Value in Red for the Sky: 86.56886115909967
Average Pixel Value in Blue for the Clouds:  154.18068190354904
Average Pixel Value in Green for the Clouds:  130.44013579403378
Average Pixel Value in Red for the Clouds: 114.31494127873907

```
## 25th of June 2018
Continuing on the Cloud Detection Verification


### Task 8 : Writing a modified version of the Cloud/Sky Classifier 
 
The training set contained 1013 images, each having 600* 600 pixels. As such the feature data set contained 600 * 600 * 1013 rows and 14 columns. To have a managable data set for training the number of features used were reduced using the study done for the most importaint features on [task 7.3](#user-content-73-figuring-out-the-most-importaint-features). For the final training feature set only 6 features were used. They are listed below,
- Blue Pixel Value
- Blue Pixel Value(From LAB)
- Red Pixel Value/Blue Pixel Value
- Red Pixel Value - Blue Pixel Value
- Blue Pixel Value-Red Pixel Value)/(Blue Pixel Value+Red Pixel Value),
- Chroma Value - (Max(RGB)-Min(RGB))

No further modification to the initial study was made. 

- Updated Results for the complete data set are as follows:

|     Accuracy Estimations     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |    79%   |   80%   |
| Sklearn (Random Forest)      |    89%   |   89%   |


|     [Confusion Matrices]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |  ![Training Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Training_Caffe_Not_Normalized.png)        |  ![Testing Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Testing_Caffe_Not_Normalized.png)        |
| Sklearn (Random Forest)      |![Training Sklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Training_Sklearn_Not_Normalized.png)       | ![TestingSklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Testing_Sklearn_Not_Normalized.png)          |


|     [Confusion Matrices Normalized]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |  ![Training Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Training_Caffe_Normalized.png)        |  ![Testing Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Testing_Caffe_Normalized.png)        |
| Sklearn (Random Forest)      |![Training Sklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Training_Sklearn_Normalized.png)       | ![TestingSklearn](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Testing_Sklearn_Normalized.png)          |

For the updated feature set, the Importance of each feature is also obtained for completeness. They are given on the following graph: 
![Feature Importaince Updated](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Random_Forest_vs_Caffe_Complete_Data_Set/Feature_Importaince.png)

Following are some sample images 


## 26th-29th of June 2018
Thefolloing tasks are geared towards Importing the Classifier to an XU4 
### Task 9 : Installing dependancies on the XU4 

#### 9.1 Intalling pip 
```
sudo apt-get install python3-pip
```

#### 9.2 Installing numpy and scipy 
```
sudo apt-get install python3-numpy 
   sudo apt-get install python3-scipy 
```
#### 9.3 Installing sklearn 
```
sudo apt-get install scikit-learn 
```

#### 9.4 Installing skimage 
```
sudo apt-get install python3-skimage
```

#### 9.5 Installing open CV 

- Unfortunately open CV had to be installed from ground up(Probably because of the use of the reduced image).
This [link](http://www.python36.com/how-to-install-opencv340-on-ubuntu1604/) was used as a reference here. 
```
sudo apt-get install build-essential 
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk-3-dev
sudo apt-get install libatlas-base-dev gfortran pylint
```

- On the follwoing command make sure to match the python installations within the XU4 
```
sudo apt-get install python2.7-dev python3.5-dev
```
For the current install of the Ubuntu image python 2.7 and python 3.5 was embedded within.

- Download OpenCV 3.4.0  
```
wget https://github.com/opencv/opencv/archive/3.4.0.zip -O opencv-3.4.0.zip
wget https://github.com/opencv/opencv_contrib/archive/3.4.0.zip -O opencv_contrib-3.4.0.zip
```
- Install Unzip 
```
sudo apt-get install unzip
```
- Extracting the files 
```
unzip opencv-3.4.0.zip
unzip opencv_contrib-3.4.0.zip
```
- Creating the build directory inside OpenCV-3.4.0:
```
cd  opencv-3.4.0
mkdir build
cd build
```

- Configuring cmake:
```
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.0/modules -DOPENCV_ENABLE_NONFREE=True ..
```
- Finally doing the install 
```
make -j8
```
Here 8 is the number of cores present(Since XU4 has 8 cores).  

Concluding the installation
```
sudo make install
```
- Reinitialize static libs 
```
sudo ldconfig
```

#### 9.6 Exporting the classifier into the XU4 

- Creating a directory for the classifier to stored
On the home directory , type in the follwoing commands 
```
mkdir waggle 
cd waggle 
mkdir RT_Claassifier
```

- The next step is done on the host machine where the cloudApp Final module is saved. On the directory where the cloudapp is save type in and execute the follwing command
```
scp CloudApp_Final_Module_for_NB.py root@10.10.10.136:/home/waggle/RT_Classifier
```
- Exporting a sample image 
 ```
 scp 0003.png root@10.10.10.136:/home/waggle/RT_Classifier
```
For the last two steps approprate ip addess for the XU4 must be used. On this instance, the ip address of the XU4 is 10.10.10.136. **Make sure to have the XU4 connected to the same network as the PC**

### Task 10: Testing the Classifier on the XU4 

On the XU4 command window type in 
```
python3 CloudApp_Final_Module_for_NB.py 0003.png
```
Unfortunately this yeilded an error message which ended with 
```
Traceback (most recent call last):
 File "CloudApp_Final_Module_for_Pkl.py", line 154, in <module>
   loaded_Random_Tree = joblib.load(Model_Save_File_Name)
 File "/usr/local/lib/https://www.ebay.com/itm/202218965829python3.5/dist-packages/sklearn/externals/joblib/numpy_pickle.py", line 578, in load
   obj = _unpickle(fobj, filename, mmap_mode)
 File "/usr/local/lib/python3.5/dist-packages/sklearn/externals/joblib/numpy_pickle.py", line 508, in _unpickle
   obj = unpickler.load()
 File "/usr/lib/python3.5/pickle.py", line 1039, in load
   dispatch[key[0]](self)
 File "/usr/lib/python3.5/pickle.py", line 1394, in load_reduce
   stack[-1] = func(*args)
 File "sklearn/tree/_tree.pyx", line 601, in sklearn.tree._tree.Tree.__cinit__
ValueError: Buffer dtype mismatch, expected 'SIZE_t' but got 'long long'
```
A bit of research into the said problem revealed that the classifier had to be trainined in a machine which matched the internal architecture of the XU4(32 bit). Unfortunately this was a complication which couldnt be resolved. 


### Task 11 : Trying out other Classifiers. 

Using the same means as the Random Forest Classifier, a Naive baised Classification was done. The Naive baised classification was implimented within the XU4 with the use of the preceeding steps speicified. The follwing represent the validation study done for the said Classifier. 


|     [Confusion Matrices]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Sklearan (Naive Baiysed) |  ![Training](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Naive_Baised_Classifier_Complete_Data_Set/Figure_1.png)        |  ![Testing](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Naive_Baised_Classifier_Complete_Data_Set/Figure_3.png)        |



|     [Confusion Matrices Normalized]     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Sklearan (Naive Baiysed) |  ![Training Caffe](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Naive_Baised_Classifier_Complete_Data_Set/Figure_2.png)        |  ![Testing](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Results_All/Results_Naive_Baised_Classifier_Complete_Data_Set/Figure_4.png)        |

The updated accuracies for all 3 classifiers studied.

|     Accuracy Estimations     | Training | Testing |
|:----------------------------:|:--------:|:-------:|
| Caffe (Superpixel Algorythm) |    80%   |   84%   |
| Sklearn (Random Forest)      |    90%   |   89%   |
| Sklearn(Naive Baised)        |    87%   |   88%   |



## July 2 - July 10th 
Getting back on track with the bike waggle project.

### Task 11 Starting off with the Bike Bottle Design  

#### Task 11.1 : Deciding on the Sensor Brain 
The initail decision was made to esu the https://www.particle.io/ platform for the sensor brain. They provide two options. 
 
- Particle Photon : Works with Wi - Fi (https://docs.particle.io/datasheets/photon-(wifi)/photon-datasheet/)
- Particle Electron : Works on Cellular Data (https://docs.particle.io/datasheets/electron-(cellular)/electron-datasheet/)
 
Finalized on taking on the Photon, since we wont have to pay for Cellular.

A modification of the code done by the high school students for the Photon can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/sensogram-main-1.ino)

#### Task 11.2: Deciding on the Sensors to Use 

The information on the decided sensors can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/Technical_Specs.md). 

#### Task 11.3: Writing the base code

The iterations on the codes done can be found [here](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle/sensorgram). 

## July 11

Getting back on the Cloud Detection App
### Task 12 : Cloud App Conclusion  

#### Task 12.1: Instaling a Waggle image on the XU4 

The Waggle Image was downloaded to the local PC with the help of Chris. 
The rest of the steps were done using the guide provided [here](https://github.com/waggle-sensor/summer2018/tree/master/chen/waggle_image#burning-image-to-microsd)

#### Task 12.2: Pushing the CloudyApp into the XU4 

Make sure to have an ethernet adaptor in this case. Usual ethernet connection wont work since the waggle image is programmed only to listen to Beehive.

Using an ethernet Adaptor you may scp the sample image,the module file and the python script file onto the XU4.
```
scp Cloud_App_Final_Module_for_NB.py  root@10.10.10.101:/home/waggle/Cloud_Detection_App
scp 0937.png root@10.10.10.101:/home/waggle/Cloud_Detection_App
```

#### Task 12.3: Installing Dependancies 

##### Task 12.3.1 Installing numpy and scipy 
```
sudo apt-get install python3-numpy 
   sudo apt-get install python3-scipy 
```
##### Task 12.3.2 Installing sklearn 
```
sudo apt-get install scikit-learn 
```
##### Task 12.3.4 Installing skimage 
```
sudo apt-get install python3-skimage
```

#### Task 12.4: Testing the program on the XU4 

The cloudy app progam based on the Naive Baised Classifier was successfully tested out on the waggle image. The next step is to test out the program within the Waggle Image pipeline on the Edge Processor. 


## July 12

Testing the Cloud Detection App within the Waggle image Pipeline

#### Task 12.5: Research on the Waggle Image Pipeline

The follwing set of links were examined to gain an idea of the inner processors within Waggle Image Pipeline.

- [Pipeline architecture](https://github.com/waggle-sensor/edge_processor/tree/master/image)
- [Image Detector Plugin](https://github.com/waggle-sensor/plugin_manager/tree/master/plugins/image_detector)


#### Task 12.6: Embedding Cloudy App Code within the Current Image Exporter Tool
The Image pipeline works on 3 main components,
- [Image Producer](https://github.com/waggle-sensor/edge_processor/tree/master/image/producer)
- [Image Pipeline](https://github.com/waggle-sensor/edge_processor/tree/master/image/pipeline) 
- [Image Exporter](https://github.com/waggle-sensor/edge_processor/tree/master/image/exporter)

The image exporter was to be the vaible candidate for the Cloud App to be pluged in as at this stage the image data was actually converted into an image. Using the code done for the [image exporter](https://github.com/waggle-sensor/edge_processor/blob/master/image/exporter/image_exporter.py), a test code was written. 

The test code can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/cloud_detection/Sklearn/Naive_Baised_Classifier/Sklearn_Image_Maker_for_SWMSEG_NB.py).



## July 13 - 15

Continuing on the Bike Waggle Project

### Task 13 - Writing Quotation for the Bike Waggle Project

A Quotation was made to get a shipment of the devices needed for the bike Waggle Sensor. The said quatation can be found [here](https://docs.google.com/spreadsheets/d/13GMrc0lDrT8yg4jRUNnsqN4OhRasOGfK-BKTgsxrhDE/edit#gid=0)

- A detailed list of the devices to be used can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/Technical_Specs.md)

### Task 14 - Modyfing the Sensogram Code for the Bike Waggle Sensor

Iteration 3 of the bike waggle code was completed. The summary of the said iteration is given below. These descriptions can also be found within the [bike waggle](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle) project folder. 

###### Verision Date : 07/15/2018

Additions: On this iteration the case where there is no connectivity is considered. On Iteration 2 each sensorgram was published individually. The current iteration works on publishing multiple sensorgrams instantaneously(Through individual 
publish statements).  

**NOTE: It was uncovered for each event around 9 sensograms[255 charactors] can be published. As such live data only takes 5 seconds
to publish(since we only have 42 sensor inputs).**

On iteration 4, The mechanism in which the sampling time is read may be modified. Wheather the packing time differs from the time in which the data is read will be invistigated. This may have to wait till the physical device is made. 

**The Source Code can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/sensogram-main-3.ino)**

## July 16 - 19

### Task 15: Setting up a SD card logger for the Bike Waggle Project.


#### Task 15.1 - Research on the "SdCardLogHandlerRK" Lybrary
The lybrary worked quite well at first glance. The initial code had the capabilities of creating and making logs for the data inputs given. However the said lybrary is meant to be used for debugging the Photon. As such the programmed photon logged a lot of unnecessary data such as infromation on wi fi connectity and error messages. The code also utilized the SDfat Lyrbrary used in photon devices. 

The sample code used is given below. 
 ```
 // This #include statement was automatically added by the Particle IDE.
#include <SdCardLogHandlerRK.h>

// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>


#include "Particle.h"

#include "SdFat.h"
#include "SdCardLogHandlerRK.h"

SYSTEM_THREAD(ENABLED);

const int SD_CHIP_SELECT = D5;
SdFat sd(1);
SdCardLogHandler logHandler(sd, SD_CHIP_SELECT, SPI_FULL_SPEED,LOG_LEVEL_INFO);
//STARTUP(logHandler.withNoSerialLogging());// Dont have serial loging 
STARTUP(logHandler.withDesiredFileSize(50000).withMaxFilesToKeep(5).withLogsDirName("Bike_Waggle_Logs_LAKITHA_3").withNoSerialLogging());
Logger LogToSD("app.sd");

size_t counter = 0;

void setup() {
	Serial.begin(9600);
}

void loop() {
	Log.info("testing counter=%d", counter++);
	LogToSD.info("write to SD counter=%d", counter);
	delay(100000);

}
  
 ```
 The Git repository for the said lybrary can be found [here](https://github.com/rickkas7/SdCardLogHandlerRK)

The pins used under the SPI(1) interface is defined below.

| Photon | SPI Name | SD Reader | 
| ------ | -------- | --------- | 
| D5     | SS       | CS        | 
| D4     | SCK      | SCK       | 
| D3     | MISO     | DO        | 
| D2     | MOSI     | DI        | 
| 3V3    |          | VCC       | 
| GND    |          | GND       | 
|        |          | CD        | 

Due to the lack of control of which messages to log a more primitive Sd Card logging lyrary was looked up. 


#### Task 15.2 - Research on the "SDFat" Lybrary
SDFat lybrary turned out to be extremely stable and it provided the control that the user lacked on the previous lybraries.
The sample code inititally used is found below.
```
// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>

#include "SdFat.h"

// Pick an SPI configuration.
// See SPI configuration section below (comments are for photon).
#define SPI_CONFIGURATION 0
//------------------------------------------------------------------------------
// Setup SPI configuration.
#if SPI_CONFIGURATION == 0
// Primary SPI with DMA
// SCK => A3, MISO => A4, MOSI => A5, SS => A2 (default)
SdFat sd;
const uint8_t chipSelect = A2;
#elif SPI_CONFIGURATION == 1
// Secondary SPI with DMA
// SCK => D4, MISO => D3, MOSI => D2, SS => D1
SdFat sd(1);
const uint8_t chipSelect = D1;
#elif SPI_CONFIGURATION == 2
// Primary SPI with Arduino SPI library style byte I/O.
// SCK => A3, MISO => A4, MOSI => A5, SS => A2 (default)
SdFatLibSpi sd;
const uint8_t chipSelect = SS;
#elif SPI_CONFIGURATION == 3
// Software SPI.  Use any digital pins.
// MISO => D5, MOSI => D6, SCK => D7, SS => D0
SdFatSoftSpi<D5, D6, D7> sd;
const uint8_t chipSelect = D0;
#endif  // SPI_CONFIGURATION
//------------------------------------------------------------------------------

File myFile;
int lk=1;
void setup() {
  Serial.begin(9600);
  // Wait for USB Serial 
  while (!Serial) {
    SysCall::yield();
  }
  
  Serial.println("Type any character to start");
  while (Serial.read() <= 0) {
    SysCall::yield();
  }

  // Initialize SdFat or print a detailed error message and halt
  // Use half speed like the native library.
  // Change to SPI_FULL_SPEED for more performance.
  if (!sd.begin(chipSelect, SPI_HALF_SPEED)) {
    sd.initErrorHalt();
  }


}

void loop() {
 // open the file for write at end like the "Native SD library"
  if (!myFile.open("test.txt", O_RDWR | O_CREAT | O_AT_END)) {
    sd.errorHalt("opening test.txt for write failed");
  }
  
// if the file opened okay, write to it:
Serial.print("Printing Line:");
myFile.println(String(lk));

// close the file:
lk=lk+1;
myFile.close();
Serial.println(String(lk));

delay(1000);
// close the file:
//myFile.close();  
}
```

The Git repository for the said lybrary can be found [here](https://github.com/greiman/SdFat-Particle)

Due to the success on the testing phase for the said lybrary, the eventual code was plugged into the main program for the Bike Waggle Project.

### Task 16 - Setting up the SD card Logger for the Bike Waggle Project

Iteration 4 of the bike waggle code was completed. The summary of the said iteration is given below. These descriptions can also be found within the [bike waggle](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle) project folder. 

#### Verision Date : 07/19/2018

Additions: This iteration looks at how the SD card can save data logs. A few lybraries were tried out. SDfat lybrary appeared to be the one most useful and the most reliable. The code is designed to print sensor information and headers each time the device is powered on. On each loop the sensor outputs will be printed out on a new line. These logs will be printed out on a text file named Lakithas_Photon.txt within a folder named Lakithas_Photon. The names of the directory and and the text file will be uniquely identified for each bike waggle. If the said directory and the said text file does not exist, the photon is formulated to create the said folder and the said file when the device is inititally turned on.
The credentials for the MPU9250 sensor was also added.

A Sample of the SD card log can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/SD_Card_Log.txt).

On iteration 5, The means in which the LCD Display is set up will be looked into.

The pins used under the SPI(1) interface is defined below.

| Photon | SPI Name | SD Reader | 
| ------ | -------- | --------- | 
| A2     | SS       | CS        | 
| A3     | SCK      | SCK       | 
| A4     | MISO     | DO        | 
| A5     | MOSI     | DI        | 
| 3V3    |          | 3V        | 
| GND    |          | GND       | 
|        |          | CD        | 



**The Source Code can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/sensogram-main-4.ino)**


## July 20

### Task 17: Setting up the LCD Display for the Bike Waggle Project.

#### Task 17.1 - Research on the "LiquidCrystal_I2C_Spark" Lybrary
The initial tests done on the said lybrary turned out to be fruitful. The initial test code can be found below. 
```
// This #include statement was automatically added by the Particle IDE.
#include <LiquidCrystal_I2C_Spark.h>

LiquidCrystal_I2C *lcd;

void startupLCD(){
   
   lcd->setCursor(5 /*columns*/,1 /*rows*/);
   lcd->print("BIKE");
   delay(1000) ;
 
   lcd->setCursor(8 /*columns*/,2 /*rows*/);
   lcd->print("WAGGLE");
   delay(1000) ;
   lcd->clear();

   // Give Device Data 
   
   lcd->setCursor(0 /*columns*/,0 /*rows*/);
   lcd->print("Project Bike Waggle");
   lcd->setCursor(0 /*columns*/,1 /*rows*/);
   lcd->print("Version 1.0");
   lcd->setCursor(0 /*columns*/,2 /*rows*/);
   lcd->print("LAKITHAs Photon");
   delay(1000) ;
   lcd->clear();

for (int count=0;count<=19;count++)
{  lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3/*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
   
}
                               
for (int count=19;count>=0;count--)               
{  lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3/*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
}


   
for (int count=0;count<=19;count++)
{  lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3 /*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
   
}
                               
for (int count=19;count>=0;count--)               
{  
   lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3 /*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
}
 
 lcd->setCursor(5 /*columns*/,1 /*rows*/);
 lcd->print("BIKE");
 delay(1000) ;
 
 lcd->setCursor(8 /*columns*/,2 /*rows*/);
 lcd->print("WAGGLE");
 delay(1000) ;
 lcd->clear();
}




void OperationalLCD(){
 lcd->clear();
 lcd->setCursor(0 /*columns*/,0 /*rows*/);
 lcd->print("Bike Waggle|"+ Time.format(Time.now(),"%H:%M:%S"));
 lcd->setCursor(0 /*columns*/,1 /*rows*/);
 lcd->print("PM1:2.342");
 lcd->setCursor(0 /*columns*/,2 /*rows*/);
 lcd->print("PM2.5:4.33");
 lcd->setCursor(0 /*columns*/,3 /*rows*/);
 lcd->print("PM10:10.340");
 lcd->setCursor(11 /*columns*/,0 /*rows*/);
 lcd->print("|");
 lcd->setCursor(11 /*columns*/,1 /*rows*/);
 lcd->print("|");
 lcd->setCursor(11 /*columns*/,2 /*rows*/);
 lcd->print("|");
 lcd->setCursor(11 /*columns*/,3 /*rows*/);
 lcd->print("|");
 lcd->setCursor(12 /*columns*/,1 /*rows*/);
 lcd->print("TM:29.42");
 lcd->setCursor(12 /*columns*/,2 /*rows*/);
 lcd->print("HM:50.33");
 delay(1000);   
}


void setup()
{
   Serial.begin(9600);
   
   // The address is typically 0x27. I2C Address: 0x3F
   // https://www.sainsmart.com/new-sainsmart-iic-i2c-twi-1602-serial-lcd-module-display-for-arduino-uno-mega-r3.html
   
   lcd = new LiquidCrystal_I2C(0x3F /*address*/, 20/*columns*/, 4/*rows*/);
   lcd->init();
   lcd->backlight();
   lcd->clear();
   
   startupLCD();
}
    

void loop()
{
OperationalLCD();
}

```
The Git repository for the said lybrary can be found [here](https://github.com/BulldogLowell/LiquidCrystal_I2C_Spark)

Due to the success on the testing phase for the said lybrary, the eventual code was plugged into the main program for the Bike Waggle Project.


#### Task 17.2  - Plugging in the LCD Display code for the Bike Waggle Project

Iteration 5 of the bike waggle code was completed. The summary of the said iteration is given below. These descriptions can also be found within the [bike waggle](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle) project folder. 

#### Verision Date : 07/20/2018

Additions: On this iteration the LCD display is set up. The LiquidCrystal_I2C_Spark lybrary was utilised here. The photon was programmed to display a startup message as well as a live coverage of the sensor Outputs with a refresh interval matching the loop interval.

On iteration 6,The GPS and the RTC will be hooked up to the photon.

The pins used under the I2C interface is defined below.

| Photon |    LCD    | 
| ------ | --------- | 
| DO     | SDA       | 
| D1     | SCL       | 
| VIN    | VCC       | 
| GND    | GND       | 

On this [link](https://docs.particle.io/reference/firmware/photon/#wire-i2c-) Particle.io advises to use pull up resisters when using the I2C interaface.  As such **Two pull up resisters were connected between the D0 and 3V3 and between D1 and 3V3 respectively.

Some helpful inks are psted below.
- [Link 1](https://www.hackster.io/ingo-lohs/what-s-my-i2c-address-0a097e) 
- [Link 2](https://community.particle.io/t/resolved-i2c-lcd-on-photon-core-struggle/23981)

**The Source Code can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/sensogram-main-5.ino)**


## July 23 -24


### Task 18 - Setting Up the Serial Port for Debugging the Particle Photon

The Serial Connectivity can be set up in one of 3 ways for the Photon. 
- Through the particle CLI
- Using Screen 
- Through the Arduino IDE 

#### 18.1 Through the Particle CLI 
- Installing Particl CLI 
On the Linux command line type in ``` bash <( curl -sL https://particle.io/install-cli )```

- Reading Serial Commands
Once Particle CLI is installed, on the command line type ``` particle serial monitor```

#### 18.2 Using Screen
- Installing Screen
On the Linux command line type in ``` sudo apt-get install screen```

- Figuring out the USB Port
On the command line tyinterface type ```ls /dev/ttyACM*```Once the comand is ran, a list of USB inputs will display. Usually you will be presented with something like ```/dev/ttyACM0```. 

- Open Screen
Copy the Device ID from the previos task and type in ```sudo screen [device ID]```
Your command shoul look like ```sudo screen  /dev/ttyACM0```. You may need to type in the Super User Credintials. 

The following [link](https://community.particle.io/t/serial-tutorial/26946) will give a description of other means of Serial Communication for the Particle Photon.


### Task 19: Managing the WI-FI Connectivity on the Photon

Working on the Particle photon, it was found out that the the Photon didnt initiate itself when there was no conncetivity. Due to the remote environments the Photon was to be used, this can cause a serious problem. As such the different modes of Photon cloud connectivity was looked up. 
The Photon provided 3 system modes
- AUTOMATIC : By default the Photon will be in Automatic Mode. And the device will not start its base code without a connction to the cloud(Without Wi-Fi)
- SEMI_AUTOMATIC: Will not need Wi-Fi Connection unless the user initiates the Wi-Fi Connection. However it retains most feeatures from the Automatic Mode.
- MANUAL : The User has all control over most essential functions of the photon. Often  risky since some essential features will not be ran unless called upon  by the user. 

A precise description of each mode can be found [here](https://docs.particle.io/reference/firmware/photon/#system-modes).

For the purpose of the Bike Waggle Project the SEMI AUTOMATIC Mode was implimented. In order to impliment the said mode the follwing code is considered to be patched within the main Code.

```
SYSTEM_MODE(SEMI_AUTOMATIC)
SYSTEM_THREAD(ENABLED)

const uint32_t msRetryDelay = 2*60000; // retry every 5min
const uint32_t msRetryTime  =   30000; // stop trying after 30sec

bool   retryRunning = false;
Timer retryTimer(msRetryDelay, retryConnect);  // timer to retry connecting
Timer stopTimer(msRetryTime, stopConnect);     // timer to stop a long running try

void retryConnect()
{
  if (!Particle.connected())   // if not connected to cloud
  {
    Serial.println("reconnect");
    stopTimer.start();         // set of the timout time
    WiFi.on();
    Particle.connect();        // start a reconnectino attempt
  }
  else                         // if already connected
  {
    Serial.println("connected");
    retryTimer.stop();         // no further attempts required
    retryRunning = false;
  }
}

void stopConnect()
{
    Serial.println("stopped");

    if (!Particle.connected()) // if after retryTime no connection
      WiFi.off();              // stop trying and swith off WiFi
    stopTimer.stop();
}

void setup()
{
  Serial.begin(115200);
 // pinMode(D7, OUTPUT);
  Particle.connect();
  if (!waitFor(Particle.connected, msRetryTime))
    WiFi.off();                // no luck, no need for WiFi
}

void loop()
{
  // MAIN LOOP CODE 
  
  //digitalWrite(D7, !digitalRead(D7));
  
  time_t time = Time.now();

  
  Serial.println(Time.format(time, TIME_FORMAT_DEFAULT));
  
  delay(10000);
  
  
  // Recheck Wifi Functions 
  if (!retryRunning && !Particle.connected())
  { // if we have not already scheduled a retry and are not connected
    Serial.println("schedule");
    stopTimer.start();         // set timeout for auto-retry by system
    retryRunning = true;
    retryTimer.start();        // schedula a retry
  }
  delay(500);
}
```
The wifi connectivity is managed by two timers set  on two function calls. The *retryConnect* Function initates the Wi-Fi Connectivity. The *retryTimer* calls up this function every 5 minutes when no Wi-Fi Connctivity is found. Everytime the *retryConnect* function is called up it will check for Wi-Fi for 30 seconds before innitation of the code in queue. This is controlled by the *stopTimer* via the call up function *stopConnect*. 
The said times of each function implimentation is under the users control. On the final version of the bike waggle the retry timer will probably be set to 30 minutes or 1 hour. 

This [link](https://community.particle.io/t/solved-make-photon-run-code-without-being-necessarily-connected-to-the-particle-cloud/25953) will emphasize on the inner workings of the code.


## July 25

### Task 20 - Setting Up the Real Time Clock 
The follwing tasks are aimed towards building an on board RTC.

#### 20.1 Looking up the *RTCLybrary* in Particle Photon  
The Bike Waggle photon uses the [PCF8523](https://www.adafruit.com/product/3295) as its RTC CLock. Luckily the *RTClybrary* has an example dedicated towards the ADAfruit device. The said code can be found [here](https://build.particle.io/libs/RTClibrary/1.2.1/tab/example/pcf8523.ino). Based off the given code a separate base code for the RTC functionality is written.

```
// Date and time functions using a DS1307 RTC connected via I2C and Wire lib
#include <Wire.h>
#include "RTClibrary.h"

RTC_PCF8523 rtc;

//char daysOfTheWeek[7][12] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};

// Initializing the Serial 
void Initialize_Serial(){

while (!Serial) {
    delay(1);  
        }
  Serial.begin(57600);
}

void Initialize_RTC(){
  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    while (1);
        }
  if (! rtc.initialized()) {
    Serial.println("RTC is NOT running!");
    } 
 }

void Update_RTC(){
    // Update the RTC
if (Particle.connected()){
  rtc.adjust(DateTime(Time.year(), Time.month(), Time.day(), Time.hour(), Time.minute(), Time.second()));
   }
}

void Display_Date_Time(){
    
    DateTime now = rtc.now();
    Serial.print(now.year(), DEC);
    Serial.print('/');
    Serial.print(now.month(), DEC);
    Serial.print('/');
    Serial.print(now.day(), DEC);
    Serial.print("-");
    //Serial.print(daysOfTheWeek[now.dayOfTheWeek()]);
    //Serial.print(") ");
    Serial.print(now.hour(), DEC);
    Serial.print(':');
    Serial.print(now.minute(), DEC);
    Serial.print(':');
    Serial.print(now.second(), DEC);
    Serial.println();
}



void setup () {

Initialize_Serial();
Initialize_RTC();
// Everytime the Particle is Turned on the RTC is Updated 
Update_RTC();


}

void loop () {
Display_Date_Time();
delay(5000);
}
```
The code given initally updates the RTC via the Particle cloud. And oneach loop it reads off the current time via the onbaord RTC. As such the device will work even without WIFI Connectivity. 

Since the RTC works works off an I<sup>2</sup>C interface, it utilizes the D0 and the D1 pins of the Photon. 

| Photon  |    LCD    | 
| ------  | --------- | 
| DO      | SDA       | 
| D1      | SCL       | 
| VIN/3V3 | VCC       | 
| GND     | GND       | 

## July 26-27 

### Task 21 - Lookback at the SD card Logger

On the particle photon a string variable can only hold maximum string length of 622 bytes. If Wi-Fi connectivity is out for a considerable amount of time, the string variable keeping the data may run out of space. As such it was decided that the back logged data should be saved within the SD Card.

However this brought up a couple of serios problems. The *SDFat* library or any other lybrary used for SD Card Data loggin in the Photon did not allow manipulation of written data within SD Card text files. As such it was mearly impossible to save all backlogged data within one text files(No way of Deleting Published Data). The only possibilty was to save each sensogram packet in different textfiles defined with respect to an index which reflects the time of creation. 

This inturn demaneded two other txt files to keep the last published index of sensogram packet as well as the last index backlogged within the SD card. 

Each time backlogged data is being published to the cloud the text file which held the sensogram packet will be deleted. Inturn, the index file having the last pulished sensgram packets will be updated. 

The base code for the SDCard check is as follows.
```
// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>

// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>

// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>


// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>

#include "SdFat.h"

// Pick an SPI configuration.
// See SPI configuration section below (comments are for photon).
#define SPI_CONFIGURATION 0
//------------------------------------------------------------------------------
// Setup SPI configuration.
#if SPI_CONFIGURATION == 0
// Primary SPI with DMA
// SCK => A3, MISO => A4, MOSI => A5, SS => A2 (default)
SdFat sd;
const uint8_t chipSelect = A2;
#elif SPI_CONFIGURATION == 1
// Secondary SPI with DMA
// SCK => D4, MISO => D3, MOSI => D2, SS => D1
SdFat sd(1);
const uint8_t chipSelect = D1;
#elif SPI_CONFIGURATION == 2
// Primary SPI with Arduino SPI library style byte I/O.
// SCK => A3, MISO => A4, MOSI => A5, SS => A2 (default)
SdFatLibSpi sd;
const uint8_t chipSelect = SS;
#elif SPI_CONFIGURATION == 3
// Software SPI.  Use any digital pins.
// MISO => D5, MOSI => D6, SCK => D7, SS => D0
SdFatSoftSpi<D5, D6, D7> sd;
const uint8_t chipSelect = D0;
#endif  // SPI_CONFIGURATION
//------------------------------------------------------------------------------

File myFile;
File myFile2;
size_t n;
int lk=1;
int num_of_logs=0;
String Next_Publish_Index ;

void Initialize_Serial(){
    
  Serial.begin(9600);
  // Wait for USB Serial 
  while (!Serial) {
    SysCall::yield();
  }
}



void Initialize_SD(){
  // Use half speed like the native library.
  // Change to SPI_FULL_SPEED for more performance.
  if (!sd.begin(chipSelect, SPI_HALF_SPEED)) {
    sd.initErrorHalt();
  }
 }

void Initialize_Backlogger(){
 // Create a file in Folder1 using a path.
if (!sd.mkdir("Backlogs")) {
Serial.println("Backlogs not created");
  }
 
if (!myFile.open("Backlogs/Index_File.txt", O_CREAT | O_WRITE | O_AT_END)) {
Serial.println("File Opening Failed");
}else{
// if the file opened okay, write to it:
Serial.println("Index_Created");
myFile.println("1");
myFile.close();
   }
}


void Create_Initial_Logger_Files(){
 // Create a file in Folder1 using a path.
for (int e = 1; e < 10; e++) {        
Create_Text_Files("Backlogs/Backlog_Num_"+String(e)+".txt", "Sensogram_"+String(e));
delay(1000);
}
} 


void Create_Text_Files(String Text_Name,String Text_Data){
 // Create a file in Folder1 using a path.
if (!myFile.open(Text_Name, O_CREAT | O_WRITE | O_AT_END)) {
 Serial.println("File Creation Failed");
}else{
// if the file opened okay, write to it:
Serial.println("File Created, Putting Text");
myFile.println(Text_Data);
myFile.close();
  }
}


String Backlog_Read(String File_Index){
 // re-open the file for reading:
 String Read_Data ;
 
  //Particle.publish("Index begining",File_Index.replace("\r",""));
  Particle.publish("BR",File_Index);
  Particle.publish("br2","Backlogs/Backlog_Num_"+ File_Index +".txt");
  
  if (!myFile.open("Backlogs/Backlog_Num_"+ File_Index +".txt", O_READ)) {
    sd.errorHalt("opening test.txt for read failed");
    Read_Data = "# No Data Available";
    }else{
        Particle.publish("br3","Reading_Data_Log_inback log");
        Read_Data = myFile.readStringUntil('\n');
        myFile.close();
        Particle.publish("BR4",Read_Data);
    }
    return Read_Data;
    }
    

void Sensogram_Publisher()
{
// Read index from the Index File 
  if(!myFile.open("Backlogs/Index_File.txt", O_READ)) 
    {
    sd.errorHalt("opening test.txt for read failed");
    }else
        {
            Serial.println("Reading_Index in");
            Next_Publish_Index = (myFile.readStringUntil('\n')).replace("\r","");
            myFile.close();
            
            Serial.println(Next_Publish_Index);
            String Sen = Backlog_Read(String(Next_Publish_Index));
            Particle.publish("Index",Next_Publish_Index);
            delay(2000);
            Particle.publish("Sensorgram",Sen);
            //Serial.println(Sen);
            
            // Getting the Data 
            if(Particle.publish("Lakitha",Sen))
            {
                //Deleting Text File     
                // Remove files from current directory.
                if (sd.remove("Backlogs/Index_File.txt") && sd.remove("Backlogs/Backlog_Num_"+ Next_Publish_Index +".txt"))
                {
                Serial.println("Files Removed, Recretaing Index");
                //Recreating Index  
                    if (!myFile.open("Backlogs/Index_File.txt", O_CREAT | O_WRITE | O_AT_END)) 
                    {
                    Serial.println("File Opening Failed");
                    }else{
                            // if the file opened okay, write to it:
                            Serial.println("Index_Created");
                            myFile.println(String(Next_Publish_Index.toInt()+1));
                            myFile.close();
                         }
                }   
            }
        }
}







void setup() {

//  delay(1000);
//  // Initialise Serail Communication    
 Initialize_Serial();

delay(10000);

 Initialize_SD();
//   delay(1000);

// Initialize_Backlogger();

// delay(5000);

// Create_Initial_Logger_Files();

// // Read Logger Files 
 //Backlog_Read("5");

delay(1000);
Sensogram_Publisher();
//Index_Updater();
} 
void loop() {
}
```
Make sure to add anoter text file to save the #of created sensogram packets.
**This method maybe too aggressive on the battery. The Bike Waggle may need to be switched off with care due to this addition** 

## July 30-31 

### Task 22 - Setting up the GPS 
 
Setting up the GPS was somewhat troublesome since debugging is usually done indoors. The GPS had to be taken out to check if the code was properly functional.As such a module was written to utilize the LCD togeather with the GPS. The folloinng code demostrates that. 
```

// This #include statement was automatically added by the Particle IDE.
#include <Particle-GPS.h>
// This #include statement was automatically added by the Particle IDE.
#include <LiquidCrystal_I2C_Spark.h>

#include "Particle-GPS.h"

SYSTEM_MODE(SEMI_AUTOMATIC)
SYSTEM_THREAD(ENABLED)


const uint32_t msRetryDelay = 2*60000; // retry every 5min
const uint32_t msRetryTime  =   30000; // stop trying after 30sec

bool   retryRunning = false;
Timer retryTimer(msRetryDelay, retryConnect);  // timer to retry connecting
Timer stopTimer(msRetryTime, stopConnect);     // timer to stop a long running try



// ***
// *** Create a Gps instance. The RX an TX pins are connected to
// *** the TX and RX pins on the electron (Serial1).
// ***
Gps _gps = Gps(&Serial1);
LiquidCrystal_I2C *lcd;

// ***
// *** Create a timer that fires every 1 ms to capture
// *** incoming serial port data from the GPS.
// ***
Timer _timer = Timer(1, onSerialData);






void retryConnect()
{
  if (!Particle.connected())   // if not connected to cloud
  {
    Serial.println("reconnect");
    stopTimer.start();         // set of the timout time
    WiFi.on();
    Particle.connect();        // start a reconnectino attempt
  }
  else                         // if already connected
  {
    Serial.println("connected");
    retryTimer.stop();         // no further attempts required
    retryRunning = false;
  }
}

void stopConnect()
{
    Serial.println("stopped");

    if (!Particle.connected()) // if after retryTime no connection
      WiFi.off();              // stop trying and swith off WiFi
    stopTimer.stop();
}

void startupLCD(){

   
   lcd->setCursor(5 /*columns*/,1 /*rows*/);
   lcd->print("BIKE");
   delay(1000) ;
 
   lcd->setCursor(8 /*columns*/,2 /*rows*/);
   lcd->print("WAGGLE");
   delay(1000) ;
   lcd->clear();


for (int count=0;count<=19;count++)
{  
   lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3/*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
   
}
                               
for (int count=19;count>=0;count--)               
{  
   lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3/*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
}


   
for (int count=0;count<=19;count++)
{  
   lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3 /*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
   
}
                               
for (int count=19;count>=0;count--)               
{  
   lcd->setCursor(count /*columns*/,0 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,1 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,2 /*rows*/);
   lcd->print("#");
   lcd->setCursor(count /*columns*/,3 /*rows*/);
   lcd->print("#");
   delay(40);
   lcd->clear();
}
 
 
 
  lcd->setCursor(5 /*columns*/,1 /*rows*/);
   lcd->print("BIKE");
   delay(1000) ;
 
   lcd->setCursor(8 /*columns*/,2 /*rows*/);
   lcd->print("WAGGLE");
   delay(1000) ;
   lcd->clear();
 
 
 
   // Give Device Data 
   
   lcd->setCursor(0 /*columns*/,0 /*rows*/);
   lcd->print("Project Bike Waggle");
   lcd->setCursor(0 /*columns*/,1 /*rows*/);
   lcd->print("Version 1.0");
   lcd->setCursor(0 /*columns*/,2 /*rows*/);
   lcd->print("Initializing");
   lcd->setCursor(3 /*columns*/,3 /*rows*/);
   lcd->print("Lakithas Photon");
   delay(1000) ;
   lcd->clear();

 
 }





void OperationalLCD(){

 
 
    Gga gga = Gga(_gps);
    gga.parse();
  
    Serial.println("2) Global Positioning System Fixed Data ($GPGGA)");
    Serial.println("======================================================");
    Serial.print("UTC Time: "); Serial.println(gga.utcTime);
    Serial.print("Latitude: "); Serial.println(gga.latitude);
    Serial.print("North/SouthIndicator: "); Serial.println(gga.northSouthIndicator);
    Serial.print("Longitude: "); Serial.println(gga.longitude);
    Serial.print("East/WestIndicator: "); Serial.println(gga.eastWestIndicator);
    Serial.print("Position Fix Indicator: "); Serial.println(gga.positionFixIndicator);
    Serial.print("Satellites Used: "); Serial.println(gga.satellitesUsed);
    Serial.print("Horizontal Dilution of Precision: "); Serial.println(gga.hdop);
    Serial.print("Altitude: "); Serial.print(gga.altitude); Serial.print(" "); Serial.println(gga.altitudeUnit);
    Serial.print("Geoidal Separation: "); Serial.print(gga.geoidalSeparation); Serial.print(" "); Serial.println(gga.geoidalSeparationUnit);
    Serial.print("Age of Diff. Corr.: "); Serial.println(gga.ageOfDiffCorr);
    Serial.println("");
  

  // ***
  // *** Get the Recommended Minimum Navigation Information ($GPRMC).
  // ***
  Rmc rmc = Rmc(_gps);
  rmc.parse();
  
    Serial.println("1) Recommended Minimum Navigation Information ($GPRMC)");
    Serial.println("======================================================");
    Serial.print("UTC Time: "); Serial.println(rmc.utcTime);
    Serial.print("Latitude: "); Serial.println(rmc.latitude);
    Serial.print("North/SouthIndicator: "); Serial.println(rmc.northSouthIndicator);
    Serial.print("Longitude: "); Serial.println(rmc.longitude);
    Serial.print("East/WestIndicator: "); Serial.println(rmc.eastWestIndicator);
    Serial.print("Speed Over Ground: "); Serial.println(rmc.speedOverGround);
    Serial.print("Course Over Ground: "); Serial.println(rmc.courseOverGround);
    Serial.print("Date: "); Serial.println(rmc.date);
    Serial.print("Magnetic Variation: "); Serial.print(rmc.magneticVariation); Serial.print(" "); Serial.println(rmc.magneticVariationDirection);
    Serial.print("Mode: "); Serial.println(rmc.mode);
    Serial.println("");
 
 
 
 delay(1000);
      String Latitude  = String(rmc.latitude);
      String Longitude  = String(rmc.longitude);
      String Altitude   = String(gga.altitude) ;
      String Speed      = String(rmc.speedOverGround);
    lcd->clear();
    lcd->setCursor(0 /*columns*/,0/*rows*/);
    lcd->print("Lat: "+Latitude);
    lcd->setCursor(0 /*columns*/,1/*rows*/);
    lcd->print("Lon: "+Longitude);
    lcd->setCursor(0 /*columns*/,2/*rows*/);
    lcd->print("Alt: "+Altitude);
    lcd->setCursor(0 /*columns*/,3/*rows*/);
    lcd->print("Spd: "+Speed);
 
 
 
 
 
 
 delay(1000);
    
}




void Initialize_LCD(){
   // The address is typically 0x27. I2C Address: 0x3F
   // https://www.sainsmart.com/new-sainsmart-iic-i2c-twi-1602-serial-lcd-module-display-for-arduino-uno-mega-r3.html
   
   lcd = new LiquidCrystal_I2C(0x3F /*address*/, 20/*columns*/, 4/*rows*/);
   lcd->init();
   lcd->backlight();
   lcd->clear();
   
 startupLCD();

}


void setup()
{
  delay(2000);

  // ***
  // *** Initialize the USB Serial for debugging.
  // ***
  Serial.begin();
  Initialize_LCD();
  Serial.println("Initializing...");

  // ***
  // *** Initialize the GPS.
  // ***
  _gps.begin(9600);

  // ***
  // *** Start the timer.
  // ***
  _timer.start();
  
    lcd->clear();
    lcd->setCursor(2 /*columns*/,1/*rows*/);
    lcd->print("GPS Inititited");
  
}

void onSerialData()
{
  _gps.onSerialData();
}

void loop()
{

  Serial.print("Data[0] = "); Serial.println(_gps.data[0]);
  Serial.print("Data[1] = "); Serial.println(_gps.data[1]);
  Serial.print("Data[2] = "); Serial.println(_gps.data[2]);
  Serial.print("Data[3] = "); Serial.println(_gps.data[3]);
  Serial.print("Data[4] = "); Serial.println(_gps.data[4]);
  Serial.print("Data[5] = "); Serial.println(_gps.data[5]);
  Serial.print("Data[6] = "); Serial.println(_gps.data[6]);

  Gga gga = Gga(_gps);
  gga.parse();
  
    Serial.println("2) Global Positioning System Fixed Data ($GPGGA)");
    Serial.println("======================================================");
    Serial.print("UTC Time: "); Serial.println(gga.utcTime);
    Serial.print("Latitude: "); Serial.println(gga.latitude);
    Serial.print("North/SouthIndicator: "); Serial.println(gga.northSouthIndicator);
    Serial.print("Longitude: "); Serial.println(gga.longitude);
    Serial.print("East/WestIndicator: "); Serial.println(gga.eastWestIndicator);
    Serial.print("Position Fix Indicator: "); Serial.println(gga.positionFixIndicator);
    Serial.print("Satellites Used: "); Serial.println(gga.satellitesUsed);
    Serial.print("Horizontal Dilution of Precision: "); Serial.println(gga.hdop);
    Serial.print("Altitude: "); Serial.print(gga.altitude); Serial.print(" "); Serial.println(gga.altitudeUnit);
    Serial.print("Geoidal Separation: "); Serial.print(gga.geoidalSeparation); Serial.print(" "); Serial.println(gga.geoidalSeparationUnit);
    Serial.print("Age of Diff. Corr.: "); Serial.println(gga.ageOfDiffCorr);
    Serial.println("");
  

  // ***
  // *** Get the Recommended Minimum Navigation Information ($GPRMC).
  // ***
  Rmc rmc = Rmc(_gps);
  rmc.parse();
  
    Serial.println("1) Recommended Minimum Navigation Information ($GPRMC)");
    Serial.println("======================================================");
    Serial.print("UTC Time: "); Serial.println(rmc.utcTime);
    Serial.print("Latitude: "); Serial.println(rmc.latitude);
    Serial.print("North/SouthIndicator: "); Serial.println(rmc.northSouthIndicator);
    Serial.print("Longitude: "); Serial.println(rmc.longitude);
    Serial.print("East/WestIndicator: "); Serial.println(rmc.eastWestIndicator);
    Serial.print("Speed Over Ground: "); Serial.println(rmc.speedOverGround);
    Serial.print("Course Over Ground: "); Serial.println(rmc.courseOverGround);
    Serial.print("Date: "); Serial.println(rmc.date);
    Serial.print("Magnetic Variation: "); Serial.print(rmc.magneticVariation); Serial.print(" "); Serial.println(rmc.magneticVariationDirection);
    Serial.print("Mode: "); Serial.println(rmc.mode);
    Serial.println("");
  

  OperationalLCD();

  
  // Recheck Wifi Functions 
  if (!retryRunning && !Particle.connected())
  { // if we have not already scheduled a retry and are not connected
    Serial.println("schedule");
    stopTimer.start();         // set timeout for auto-retry by system
    retryRunning = true;
    retryTimer.start();        // schedula a retry
  }



  delay(2500);
}

```

The given code prints out the Speed, Longtitude, Attitude and the Altitude of the device. The pin DIagram for the device is given below. 


| Photon | GPS |
|--------|-----|
| 3.3V   | VCC |
| GND    | GND |
| TX     | RX  |
| RX     | TX  |


## Aug 1-3 

### Task 23 - Setting up the Gyroscope
 
For the gyroscope the follwing code was used to read data.   
 
```
#include "quaternionFilters.h"
#include "MPU9250.h"

#define AHRS true         // Set to false for basic data read
#define SerialDebug true  // Set to true to get Serial output for debugging


MPU9250 myIMU;

void Initialize_Gyro(){
    
 // Read the WHO_AM_I register, this is a good test of communication
   byte c = myIMU.readByte(MPU9250_ADDRESS, WHO_AM_I_MPU9250);

  if (c == 0x73) // WHO_AM_I should always be 0x68
  {
    Serial.print("Check 3");  
    Serial.println("MPU9250 is online...");

    // Start by performing self test and reporting values
    myIMU.MPU9250SelfTest(myIMU.SelfTest);

    Serial.print("x-axis self test: acceleration trim within : ");
    Serial.print(myIMU.SelfTest[0],1); Serial.println("% of factory value");
    Serial.print("y-axis self test: acceleration trim within : ");
    Serial.print(myIMU.SelfTest[1],1); Serial.println("% of factory value");
    Serial.print("z-axis self test: acceleration trim within : ");
    Serial.print(myIMU.SelfTest[2],1); Serial.println("% of factory value");
    Serial.print("x-axis self test: gyration trim within : ");
    Serial.print(myIMU.SelfTest[3],1); Serial.println("% of factory value");
    Serial.print("y-axis self test: gyration trim within : ");
    Serial.print(myIMU.SelfTest[4],1); Serial.println("% of factory value");
    Serial.print("z-axis self test: gyration trim within : ");
    Serial.print(myIMU.SelfTest[5],1); Serial.println("% of factory value");


    // Calibrate gyro and accelerometers, load biases in bias registers
    myIMU.calibrateMPU9250(myIMU.gyroBias, myIMU.accelBias);



    myIMU.initMPU9250();
    // Initialize device for active mode read of acclerometer, gyroscope, and
    // temperature
    Serial.println("MPU9250 initialized for active data mode....");

    // Read the WHO_AM_I register of the magnetometer, this is a good test of
    // communication
    byte d = myIMU.readByte(AK8963_ADDRESS, WHO_AM_I_AK8963);
    Serial.print("AK8963 "); Serial.print("I AM "); Serial.print(d, HEX);
    Serial.print(" I should be "); Serial.println(0x48, HEX);

    // Get magnetometer calibration from AK8963 ROM
    myIMU.initAK8963(myIMU.magCalibration);
    // Initialize device for active mode read of magnetometer
 
      //  Serial.println("Calibration values: ");
      Serial.print("X-Axis sensitivity adjustment value ");
      Serial.println(myIMU.magCalibration[0], 2);
      Serial.print("Y-Axis sensitivity adjustment value ");
      Serial.println(myIMU.magCalibration[1], 2);
      Serial.print("Z-Axis sensitivity adjustment value ");
      Serial.println(myIMU.magCalibration[2], 2);
 

  } // if (c == 0x73)
  else
  {
    Serial.print("Could not connect to MPU9250: 0x");
    Serial.println(c, HEX);
   // while(1) ; // Loop forever if communication doesn't happen
        }
}// End Initialize Gyro 



void Read_Gyro()
{
    
    if (myIMU.readByte(MPU9250_ADDRESS, INT_STATUS) & 0x01)
  {  
    myIMU.readAccelData(myIMU.accelCount);  // Read the x/y/z adc values
    myIMU.getAres();

    // Now we'll calculate the accleration value into actual g's
    // This depends on scale being set
    myIMU.ax = (float)myIMU.accelCount[0]*myIMU.aRes; // - accelBias[0];
    myIMU.ay = (float)myIMU.accelCount[1]*myIMU.aRes; // - accelBias[1];
    myIMU.az = (float)myIMU.accelCount[2]*myIMU.aRes; // - accelBias[2];

    myIMU.readGyroData(myIMU.gyroCount);  // Read the x/y/z adc values
    myIMU.getGres();

    // Calculate the gyro value into actual degrees per second
    // This depends on scale being set
    myIMU.gx = (float)myIMU.gyroCount[0]*myIMU.gRes;
    myIMU.gy = (float)myIMU.gyroCount[1]*myIMU.gRes;
    myIMU.gz = (float)myIMU.gyroCount[2]*myIMU.gRes;

    myIMU.readMagData(myIMU.magCount);  // Read the x/y/z adc values
    myIMU.getMres();
    // User environmental x-axis correction in milliGauss, should be
    // automatically calculated
    myIMU.magbias[0] = +470.;
    // User environmental x-axis correction in milliGauss TODO axis??
    myIMU.magbias[1] = +120.;
    // User environmental x-axis correction in milliGauss
    myIMU.magbias[2] = +125.;

    // Calculate the magnetometer values in milliGauss
    // Include factory calibration per data sheet and user environmental
    // corrections
    // Get actual magnetometer value, this depends on scale being set
    myIMU.mx = (float)myIMU.magCount[0]*myIMU.mRes*myIMU.magCalibration[0] -
               myIMU.magbias[0];
    myIMU.my = (float)myIMU.magCount[1]*myIMU.mRes*myIMU.magCalibration[1] -
               myIMU.magbias[1];
    myIMU.mz = (float)myIMU.magCount[2]*myIMU.mRes*myIMU.magCalibration[2] -
               myIMU.magbias[2];
  } // if (readByte(MPU9250_ADDRESS, INT_STATUS) & 0x01)

  // Must be called before updating quaternions!
  myIMU.updateTime();

  // Sensors x (y)-axis of the accelerometer is aligned with the y (x)-axis of
  // the magnetometer; the magnetometer z-axis (+ down) is opposite to z-axis
  // (+ up) of accelerometer and gyro! We have to make some allowance for this
  // orientationmismatch in feeding the output to the quaternion filter. For the
  // MPU-9250, we have chosen a magnetic rotation that keeps the sensor forward
  // along the x-axis just like in the LSM9DS0 sensor. This rotation can be
  // modified to allow any convenient orientation convention. This is ok by
  // aircraft orientation standards! Pass gyro rate as rad/s
  // MadgwickQuaternionUpdate(ax, ay, az, gx*PI/180.0f, gy*PI/180.0f, gz*PI/180.0f,  my,  mx, mz);

  MahonyQuaternionUpdate(myIMU.ax, myIMU.ay, myIMU.az, myIMU.gx*DEG_TO_RAD,
                         myIMU.gy*DEG_TO_RAD, myIMU.gz*DEG_TO_RAD, myIMU.my,
                         myIMU.mx, myIMU.mz, myIMU.deltat);


    myIMU.delt_t = millis() - myIMU.count;
    if (myIMU.delt_t > 500)
    {
        // Print acceleration values in milligs!
        String Acclerartion_X = String(1000*myIMU.ax);
        String Acclerartion_Y = String(1000*myIMU.ay);
        String Acclerartion_Z = String(1000*myIMU.az);
        String  Orientation_X = String(myIMU.gx, 3);
        String  Orientation_Y = String(myIMU.gy, 3);
        String  Orientation_Z = String(myIMU.gz, 3);
        String  Temprature    = String(((float) myIMU.tempCount) / 333.87 + 21.0);
        
        Serial.print("X-acceleration: "); Serial.print(1000*myIMU.ax);
        Serial.print(" mg ");
        Serial.print("Y-acceleration: "); Serial.print(1000*myIMU.ay);
        Serial.print(" mg ");
        Serial.print("Z-acceleration: "); Serial.print(1000*myIMU.az);
        Serial.println(" mg ");

        // Print gyro values in degree/sec
        Serial.print("X-gyro rate: "); Serial.print(myIMU.gx, 3);
        Serial.print(" degrees/sec ");
        Serial.print("Y-gyro rate: "); Serial.print(myIMU.gy, 3);
        Serial.print(" degrees/sec ");
        Serial.print("Z-gyro rate: "); Serial.print(myIMU.gz, 3);
        Serial.println(" degrees/sec");

        // Print mag values in degree/sec
        Serial.print("X-mag field: "); Serial.print(myIMU.mx);
        Serial.print(" mG ");
        Serial.print("Y-mag field: "); Serial.print(myIMU.my);
        Serial.print(" mG ");
        Serial.print("Z-mag field: "); Serial.print(myIMU.mz);
        Serial.println(" mG");

        myIMU.tempCount = myIMU.readTempData();  // Read the adc values
        // Temperature in degrees Centigrade
        myIMU.temperature = ((float) myIMU.tempCount) / 333.87 + 21.0;
        // Print temperature in degrees Centigrade
        Serial.print("Temperature is ");  Serial.print(myIMU.temperature, 1);
        Serial.println(" degrees C");

    // Define output variables from updated quaternion---these are Tait-Bryan
    // angles, commonly used in aircraft orientation. In this coordinate system,
    // the positive z-axis is down toward Earth. Yaw is the angle between Sensor
    // x-axis and Earth magnetic North (or true North if corrected for local
    // declination, looking down on the sensor positive yaw is counterclockwise.
    // Pitch is angle between sensor x-axis and Earth ground plane, toward the
    // Earth is positive, up toward the sky is negative. Roll is angle between
    // sensor y-axis and Earth ground plane, y-axis up is positive roll. These
    // arise from the definition of the homogeneous rotation matrix constructed
    // from quaternions. Tait-Bryan angles as well as Euler angles are
    // non-commutative; that is, the get the correct orientation the rotations
    // must be applied in the correct order which for this configuration is yaw,
    // pitch, and then roll.
    // For more see
    // http://en.wikipedia.org/wiki/Conversion_between_quaternions_and_Euler_angles
    // which has additional links.
    
      myIMU.yaw   = atan2(2.0f * (*(getQ()+1) * *(getQ()+2) + *getQ() *
                    *(getQ()+3)), *getQ() * *getQ() + *(getQ()+1) * *(getQ()+1)
                    - *(getQ()+2) * *(getQ()+2) - *(getQ()+3) * *(getQ()+3));
      myIMU.pitch = -asin(2.0f * (*(getQ()+1) * *(getQ()+3) - *getQ() *
                    *(getQ()+2)));
      myIMU.roll  = atan2(2.0f * (*getQ() * *(getQ()+1) + *(getQ()+2) *
                    *(getQ()+3)), *getQ() * *getQ() - *(getQ()+1) * *(getQ()+1)
                    - *(getQ()+2) * *(getQ()+2) + *(getQ()+3) * *(getQ()+3));
      myIMU.pitch *= RAD_TO_DEG;
      myIMU.yaw   *= RAD_TO_DEG;
      // Declination of SparkFun Electronics (40°05'26.6"N 105°11'05.9"W) is
      // 	8° 30' E  ± 0° 21' (or 8.5°) on 2016-07-19
      // - http://www.ngdc.noaa.gov/geomag-web/#declination
       myIMU.yaw   -= 8.5;
       myIMU.roll  *= RAD_TO_DEG;
       Serial.print("Yaw, Pitch, Roll: ");
       Serial.print(myIMU.yaw, 2);
       Serial.print(", ");
       Serial.print(myIMU.pitch, 2);
       Serial.print(", ");
       Serial.println(myIMU.roll, 2);

       Serial.print("rate = ");
       Serial.print((float)myIMU.sumCount/myIMU.sum, 2);
       Serial.println(" Hz");

    // Print acceleration values in milligs!
        String Euler_Yaw = String(myIMU.yaw, 2);
        String Euler_Pitch = String(myIMU.pitch, 2);
        String Euler_Roll = String(myIMU.roll, 2);
        String Euler_Frequency = String((float)myIMU.sumCount/myIMU.sum, 2);
        
        // String  Orientation_X = String(myIMU.gx, 3);
        // String  Orientation_Y = String(myIMU.gy, 3);
        // String  Orientation_Z = String(myIMU.gz, 3);

       myIMU.count = millis();
       myIMU.sumCount = 0;
       myIMU.sum = 0;
 
    }
}

void setup()
{
Serial.begin(9600);
delay(10000);

Initialize_Gyro();

}


void loop()
{
 Read_Gyro();
 delay(5000);
  // If intPin goes high, all data registers have new data
  // On interrupt, check if data ready interrupt
}

```

The code resulted in outputting the 3 axis acceleration, 3 axis orientation, and 3  axis megnetic feild readings. The following pin diagram was used. 

| Due  | MPU9250 | Description |
|------|---------|-------------|
| 5V   | VCC     | This board can also use 3.3V but first needs the solder jumper to be bridged, located next to the regulator.  
| GND  | GND     |
| A1   | SCL     | Pin 21 is the Due's default SCL pin  
| A0   | SDA     | Pin 20 is the Due's default SDA pin  
|      | EDA     | Auxiliary pin, unused, frankly I did not figure out exactly what it does, very confident it's unused  
|      | ECL     | Auxiliary pin, unused, frankly I did not figure out exactly what it does, very confident it's unused  
|      | AD0     | Changes the LSB of the address  
|      | INT     | Interrupt output to controller, not used! 
| 3V3  | NCS     | Pull to high to enable I2C mode, to low for SPI
| GND  | FSYNC   | Frame sync, for use with a camera, not used! Should be grounded


## Aug 6

### Task 24 - Setting up the Temprature and Humidity Sensor

The HTU2ID is anither sensor which communicated using the I^2C Bus. The following code was implimented to get the Temprature and Humidity Readings. 

```
// This #include statement was automatically added by the Particle IDE.
#include <HTU21D.h>

HTU21D htu = HTU21D();
// Taking care of the HTU sensor 
void Initialize_HTU21D(){
    	if(! htu.begin()){
	    Serial.println("HTU21D not found");
	    delay(1000);
	}else{
	Serial.println("HTU21D OK");
	}
}



void Read_HTU21D(){
	Serial.print("Hum:"); Serial.println(htu.readHumidity());
	Serial.print("Temp:"); Serial.println(htu.readTemperature());
	// Reading HTU2ID Data - Temparature and Humidity * 100

}

void setup() {
Initialize_HTU21D();
}

void loop() {
Read_HTU21D();
delay(1000);
}

```

Since the RTC works works off an I<sup>2</sup>C interface, it utilizes the D0 and the D1 pins of the Photon. 

| Photon  |    LCD    | 
| ------  | --------- | 
| DO      | SDA       | 
| D1      | SCL       | 
| VIN/3V3 | VCC       | 
| GND     | GND       | 


## Aug 7-8

### Task 25 - Running the Particle with a Collection of Sensors

Since many sensors have been tried out thus far, it was time to check wheather they worked togeather on the photon. The devices to be connected togeather for this iteration of the main code is listed here. 

| Device | Serial data link | Device Address |
|--------|------------------|----------------|
| RTC(PCF8523)		       | I<sup>2</sup>C |   0x68       |
| LCD Module(Smraza 2004)      | I<sup>2</sup>C |   0x3F       |
| Gyroscope(MPU9250)  	       | I<sup>2</sup>C |   0x68/0x69  |
| GPS(NEO-6M)   	       | RS232 TTL     	|      -       |
| SD Module(Adafruit)          | SPI          	|      -       |
| Temprature/Humidity(HTU2ID)  | I<sup>2</sup>C |    0x40      |
| Battery Pack                 |        -       |       -      |


Unfortunately, in default settings both the RTC and the Gyroscope happened to have the same setting. As such the AD0 pin on the is Gyroscope is pulled high to change its address to 0x69(from 0x68). To cater this modification the code snippet taken from the initial MPU9250 code was revised. 

The revision is stated below.

The library of the said senor(SPARKFUN_MPU-9250) holds a file named *'MPU9250.h'*. This peice of code can be found there. 

```
// Using the MPU-9250 breakout board, ADO is set to 0
// Seven-bit device address is 110100 for ADO = 0 and 110101 for ADO = 1
#define ADO 0
#if ADO
#define MPU9250_ADDRESS 0x69  // Device address when ADO = 1
#else
#define MPU9250_ADDRESS 0x68  // Device address when ADO = 0
#define AK8963_ADDRESS  0x0C   // Address of magnetometer
#endif // AD0
```
 Since this code is meant to used in an arduino, the follwing modification was done for it to be ran on the photon.
  
 ```
// Using the MPU-9250 breakout board, ADO is set to 0

#define MPU9250_ADDRESS 0x69  // 
#define AK8963_ADDRESS  0x0C   // Address of magnetometer
```

Once these modifications were done the code found in this **[link](https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/sensorgram/sensogram-main-8.ino)** was used to cater all the devices thus far used. 

## Aug 9-10

### Task 25 - Running the Alpha Sensor on the Particle
#### Task 25.1 - Iteration 1
At this point having all the sensors connected to the Photon itself was considered. This would lead to the bike waggle being less expensive and less bulky. The Photon provided two SPI interfaces. The second SPIinterface was used to connect the Alpha Sense to the photon. 

The following code was used in the implimentation. 
```

// Alpha Sensor histogram

// Alphasensor
#define ALPHA_SLAVE_PIN D5
boolean flagON = false;


    String AlphaBin0 ;
    String AlphaBin1 ;
    String AlphaBin2 ;
    String AlphaBin3 ;
    String AlphaBin4 ;
    String AlphaBin5 ;
    String AlphaBin6 ;
    String AlphaBin7 ;
    String AlphaBin8 ;
    String AlphaBin9 ;
    String AlphaBin10;
    String AlphaBin11;
    String AlphaBin12;
    String AlphaBin13;
    String AlphaBin14 ;
    String AlphaBin15 ;
    String AlphaBin16 ;
    String AlphaBin17 ;
    String AlphaBin18 ;
    String AlphaBin19 ;
    String AlphaBin20 ;
    String AlphaBin21 ;
    String AlphaBin22 ;
    String AlphaBin23 ;
    String AlphaBin24 ;

// Reading the PM values - PM*1000
    String AlphaPM1 ;
    String AlphaPM2_5; 
    String AlphaPM10 ;
    
    
//      
    
    
bool Alpha_Status_Main = false;
bool Alpha_Status_Fan = false;


void Initialize_SPI1()
{
    Serial.println("Initialization");
    delay(20000);
    Serial.println("Init Alpha ");
    SPI1.begin(ALPHA_SLAVE_PIN);	
    SPI1.setBitOrder(MSBFIRST);
    SPI1.setDataMode(SPI_MODE1);
    SPI1.setClockSpeed(5000000);
 	delay(1000);
	pinMode(ALPHA_SLAVE_PIN, OUTPUT);
    Serial.println("Begining SPI");
}



bool Check_Alpha_Status(){
    
    byte Alpha_Status_Byte;
    int Repeats=0;
    SPI1.beginTransaction(__SPISettings(5000000,MSBFIRST, SPI_MODE1));
 	digitalWrite(ALPHA_SLAVE_PIN, LOW);
    SPI1.transfer(0xCF);
    delay(10);
    Alpha_Status_Byte = SPI1.transfer(0xCF);


// Rechecking    
while(Alpha_Status_Byte!=0xF3&&(Repeats<10)){
    delay(10);
    Alpha_Status_Byte = SPI1.transfer(0xCF); 
    Repeats=Repeats+1;
    }
    digitalWrite(ALPHA_SLAVE_PIN, HIGH);
    SPI1.endTransaction();

// Give Status
Serial.println("Checking Status");
Serial.println(Alpha_Status_Byte,HEX);

if (Alpha_Status_Byte == 0xF3){
Serial.println("Sensor Ready to Transmit");
	return true;
		}else{
Serial.println("Sensor Failed to Initiate");
    return false;

    }
}


bool Turn_Alpha_On(){
byte Alpha_Status_Bytes[3];
byte expected[] = {0xF3, 0x03, 0x03};
Serial.println("Turning Alpha On");
digitalWrite(ALPHA_SLAVE_PIN, LOW);
Alpha_Status_Bytes[0] = SPI1.transfer(0x03);
digitalWrite(ALPHA_SLAVE_PIN, HIGH);
delay(10);
digitalWrite(ALPHA_SLAVE_PIN, LOW);
Alpha_Status_Bytes[1] = SPI1.transfer(0x03);
digitalWrite(ALPHA_SLAVE_PIN, HIGH);
delay(10);
digitalWrite(ALPHA_SLAVE_PIN, LOW);
Alpha_Status_Bytes[2] = SPI1.transfer(0x03);
digitalWrite(ALPHA_SLAVE_PIN, HIGH);

Serial.println(Alpha_Status_Bytes[0],HEX);
Serial.println(Alpha_Status_Bytes[1],HEX);
Serial.println(Alpha_Status_Bytes[2],HEX);
return (Alpha_Status_Bytes==expected);

}


void Read_Alpha()
    {
byte Alpha_Status_Bytes[2];
byte Alpha_Readings[86];
Serial.println("Reading Alpha Sensor Values");
digitalWrite(ALPHA_SLAVE_PIN, LOW);
Alpha_Status_Bytes[0] = SPI1.transfer(0x30);
digitalWrite(ALPHA_SLAVE_PIN, HIGH);
delay(10);
digitalWrite(ALPHA_SLAVE_PIN, LOW);
Alpha_Status_Bytes[1] = SPI1.transfer(0x30);
digitalWrite(ALPHA_SLAVE_PIN, HIGH);
delay(10);
digitalWrite(ALPHA_SLAVE_PIN,LOW);
for (int i = 0; i<86; i++){
Alpha_Readings[i] = SPI1.transfer(0x30);
delayMicroseconds(10);
}
digitalWrite(ALPHA_SLAVE_PIN,HIGH);
Serial.println("Alpha Bytes");

Serial.println(Alpha_Status_Bytes[0],HEX);
Serial.println(Alpha_Status_Bytes[1],HEX);

Serial.println("Done Reading Alpha Values");

delay(1000);
for (int i = 0; i<86; i++){
Serial.println("Alplha Byte "+String(i)+" : "+String(Alpha_Readings[i]));
delay(10);
}
delay(1000);


for (int i = 0; i<48; i=i+2){
Serial.println("Bin Value "+String(i/2)+" : "+String(Get_Int_From_Bytes(Alpha_Readings[i],Alpha_Readings[i+1])));
delay(100);
}

Serial.println("Temprature: " +String(Alpha_Readings[56],HEX)+"|"+String(Alpha_Readings[57],HEX));
Serial.println("Humidity: " +String(Alpha_Readings[58],HEX)+"|"+String(Alpha_Readings[59],HEX));
Serial.println("SP: " +String(Alpha_Readings[52],HEX)+"|"+String(Alpha_Readings[53],HEX));
 
 
delay(1000); 

int Temprature = Get_Int_From_Bytes(Alpha_Readings[56],Alpha_Readings[57]);
delay(1000);

int Humidity = Get_Int_From_Bytes(Alpha_Readings[58],Alpha_Readings[59]);
delay(1000);

int Sampling_Time = Get_Int_From_Bytes(Alpha_Readings[52],Alpha_Readings[53]);


delay(1000);
Serial.println("Temprature: " + String(Temprature));
delay(1000);
Serial.println("Humidity: " + String(Humidity));
delay(1000);
Serial.println("Sampling Time: " + String(Sampling_Time));


}

int Get_Int_From_Bytes(byte LSB, byte MSB)
{
    int Least_Significant_Int = int(LSB);
    int Most_Significant_Int = int(MSB);
    
    // Combine two bytes into a 16-bit unsigned int
     return ((Most_Significant_Int << 8) | Least_Significant_Int);
}

float Get_float_From_Bytes(byte val0, byte val1, byte val2, byte val3)
{
  // Return an IEEE754 float from an array of 4 bytes
  union u_tag {
    byte b[4];
    float val;
  } u;

  u.b[0] = val0;
  u.b[1] = val1;
  u.b[2] = val2;
  u.b[3] = val3;

  return u.val;
}


// EXAMPLE USAGE
void setup()
{
Serial.begin(9600);
Serial.println("Hello Computer");
delay(2000);
Serial.println("Serial Opening 1");
delay(2000);
Initialize_SPI1();
delay(2000);
Turn_Alpha_On() ;
delay(5000);
}

void loop() {
    
    
Read_Alpha();
delay(5000);    
}


```

The Current code for the alpha sense reads byte values for the Histograms, PM, Sampling times, Temprature and Humidity. The next iteration will be implimented to convert this data to human readable form. 



**NOTE: While the Temprature values and the Bin counts derived from the SPI interface made sense, the Humidity values did not match what was displayed on the Alpha Sense software. However this may not be of our concern, since we are using a seprate Humidity Sensor.**


**NOTE: On this iteration more often than not the Alpha Sensor read zeros as histogram counts. This concern will be addressed on the coming iterations**

## Aug 13-17

#### Task 25.2 - Iteration 2

Here the problem of alpha sensor reading zeros was looked into. It was found out that even the fan turned on, the laser withing the Alpha sense was not initited. This was solved using the code given below. Infact Alpha sense OPC N3, gave more control to the user so that the fan, the laser and the high/low gain options were user programmable. 

```
void Initialize_Alpha(){

    while (!Set_Fan_Digital_Pot_State(true)) {
        Serial.println("Still trying to turn on device...");
        delay(2500);
    }
    
       delay(5000);
    
    // while (!Set_Laser_Digital_Pot_State(true)) {
    //     Serial.println("Still trying to turn on device...");
    //     delay(2500);
    // }
    
    //   delay(5000);

    while (!Set_Laser_Power_Switch_State(true)) {
        Serial.println("Still trying to turn on device...");
     delay(2500);
    }
    
    delay(5000);

    // while (!Set_Gain_State(true)) {
    //     Serial.println("Still trying to turn on device...");
    //     delay(2500);
    // }
    
    //   delay(5000);  
    
    // Resetting Histogram
    Read_PM();
}

```
with this addition, a separate program was done to test the implimentation of the alpha sense. 

```

#include "math.h"

// Alpha sensor histogram

// Alphasensor
#define ALPHA_SLAVE_PIN D5

boolean flagON = false;


String AlphaBin0 ;
String AlphaBin1 ;
String AlphaBin2 ;
String AlphaBin3 ;
String AlphaBin4 ;
String AlphaBin5 ;
String AlphaBin6 ;
String AlphaBin7 ;
String AlphaBin8 ;
String AlphaBin9 ;
String AlphaBin10;
String AlphaBin11;
String AlphaBin12;
String AlphaBin13;
String AlphaBin14 ;
String AlphaBin15 ;
String AlphaBin16 ;
String AlphaBin17 ;
String AlphaBin18 ;
String AlphaBin19 ;
String AlphaBin20 ;
String AlphaBin21 ;
String AlphaBin22 ;
String AlphaBin23 ;
String AlphaBin24 ;

// Reading the PM values - PM*1000
String AlphaPM1 ;
String AlphaPM2_5; 
String AlphaPM10 ;
    
    
bool Alpha_Status_Fan = false;
byte Firmware_Major;
byte Firmware_Minor;


// EXAMPLE USAGE
void setup()
{
    Serial.begin(9600);
    delay(10000);
    Serial.println("------------------");
    
    Initialize_SPI1();
    Initialize_Alpha();
    
    delay(2000);
}

void loop() {
    if (!Read_Alpha()) {
        Serial.println("Failed to read histogram.");
        delay(2500);
        return;
    }

    delay(20000);
}

void Initialize_SPI1()
{
    Serial.println("Initialization");
    delay(2000);
    Serial.println("Init SPI");
    delay(2000);
    SPI1.begin(ALPHA_SLAVE_PIN);
    SPI1.setBitOrder(MSBFIRST);
    SPI1.setDataMode(SPI_MODE1);
    SPI1.setClockSpeed(300000);
 	delay(1000);
	pinMode(ALPHA_SLAVE_PIN, OUTPUT);
    Serial.println("Begining SPI");
}


void Initialize_Alpha(){

    while (!Set_Fan_Digital_Pot_State(true)) {
        Serial.println("Still trying to turn on device...");
        delay(2500);
    }
    
       delay(5000);
 
    
    
    // while (!Set_Laser_Digital_Pot_State(true)) {
    //     Serial.println("Still trying to turn on device...");
    //     delay(2500);
    // }
    
    //   delay(5000);
 
 
     
    while (!Set_Laser_Power_Switch_State(true)) {
        Serial.println("Still trying to turn on device...");
     delay(2500);
    }
    
    delay(5000);

    // while (!Set_Gain_State(true)) {
    //     Serial.println("Still trying to turn on device...");
    //     delay(2500);
    // }
    
    //   delay(5000);  
    
    // Resetting Histogram
    Read_PM();
}

void beginTransfer() {
    digitalWrite(ALPHA_SLAVE_PIN, LOW);
    delay(1);
}

void endTransfer() {
    delay(1);
    digitalWrite(ALPHA_SLAVE_PIN, HIGH);
}


bool transferUntilMatch(byte send, byte want, unsigned long timeout) {
    unsigned long startTime = millis();
    
    while (millis() - startTime < timeout) {
        if (SPI1.transfer(send) == want) {
            return true;
        }

        delay(50);
    }
    
    return false;
}


byte transfer(byte send) {
    return SPI1.transfer(send);
}


bool Set_Fan_Digital_Pot_State(bool powered) {
    Serial.println("Setting Fan Digital Pot State");

    beginTransfer();
    
    if (!transferUntilMatch(0x03, 0xf3, 1000)) {
        Serial.println("Power control timed out.");
        return false;
    }

    delayMicroseconds(10);
    
    if (powered) {
        transfer(0x03);
    } else {
        transfer(0x02);
    }

    endTransfer();

    return true;
}


bool Set_Laser_Digital_Pot_State(bool powered) {
    Serial.println("Setting Laser Digital Pot State");

    beginTransfer();
    
    if (!transferUntilMatch(0x03, 0xf3, 1000)) {
        Serial.println("Power control timed out.");
        return false;
    }

    delayMicroseconds(10);
    
    if (powered) {
        transfer(0x05);
    } else {
        transfer(0x02);
    }

    endTransfer();

    return true;
}



bool Set_Laser_Power_Switch_State(bool powered) {
    Serial.println("Setting Laser Power Switch State");

    beginTransfer();
    
    if (!transferUntilMatch(0x03, 0xf3, 1000)) {
        Serial.println("Power control timed out.");
        return false;
    }

    delayMicroseconds(10);
    
    if (powered) {
        transfer(0x07);
    } else {
        transfer(0x02);
    }

    endTransfer();

    return true;
}



bool Set_Gain_State(bool powered) {
    Serial.println("Setting Laser Gain State");

    beginTransfer();
    
    if (!transferUntilMatch(0x03, 0xf3, 1000)) {
        Serial.println("Power control timed out.");
        return false;
    }

    delayMicroseconds(10);
    
    if (powered) {
        transfer(0x09);
    } else {
        transfer(0x02);
    }

    endTransfer();

    return true;
}

bool Read_Alpha() {
    byte Alpha_Readings[86];

    beginTransfer();

    if (!transferUntilMatch(0x30, 0xf3, 1000)) {
        Serial.println("Histogram command failed.");
        return false;
    }
    
    delay(10);

    for (int i = 0; i<86; i++){
        Alpha_Readings[i] = transfer(0x30);
        delayMicroseconds(10);
    }
    
    endTransfer();
    
    Serial.println("Histogram:");

    for (int i = 0 ; i<86; i++) {
        Serial.print(Alpha_Readings[i], HEX);

        if (i % 10 == 9) {
            Serial.print("\n");
        } else {
            Serial.print(" ");
        }
    }
    
    Serial.println();
    
    Serial.println("--------");
    
  
    int Temprature = Get_Int_From_Bytes(Alpha_Readings[56],Alpha_Readings[57]);
    int Humidity = Get_Int_From_Bytes(Alpha_Readings[58],Alpha_Readings[59]);
    int Sampling_Time = Get_Int_From_Bytes(Alpha_Readings[52],Alpha_Readings[53]);
    Serial.println("Temprature: " + String(Temprature));
    Serial.println("Humidity: " + String(Humidity));
    Serial.println("Sampling Time: " + String(Sampling_Time));
    
    String pm1   = Get_Float_From_Bytes_Single_Precision(Alpha_Readings[60],Alpha_Readings[61],Alpha_Readings[62],Alpha_Readings[63]);
    String pm2_5 = Get_Float_From_Bytes_Single_Precision(Alpha_Readings[64],Alpha_Readings[65],Alpha_Readings[66],Alpha_Readings[67]);
    String pm10  = Get_Float_From_Bytes_Single_Precision(Alpha_Readings[68],Alpha_Readings[69],Alpha_Readings[70],Alpha_Readings[71]);
    
    Serial.println("pm1  : " + String(pm1));
    Serial.println("pm2_5: " + String(pm2_5));
    Serial.println("pm10 : " + String(pm10));
    Serial.println("-----------------------------------");
    
    if(!(Alpha_Readings[0]== 0x00)){ 
    return true;}
    else{ 
    return false;
    }
}



bool Read_PM() {
    byte Alpha_Readings[86];

    beginTransfer();

    if (!transferUntilMatch(0x32, 0xf3, 1000)) {
        Serial.println("Histogram command failed.");
        return false;
    }
    
    delay(10);

    for (int i = 0; i<14; i++){
        Alpha_Readings[i] = transfer(0x32);
        delayMicroseconds(10);
    }
    
    endTransfer();
    
    if(!(Alpha_Readings[0]== 0x00)){ 
    return true;
    Serial.println("Resetting Histogram");
    
    }
    else{ 
    return false;
    }
}




int Get_Int_From_Bytes(byte LSB, byte MSB)
{
    int Least_Significant_Int = int(LSB);
    int Most_Significant_Int = int(MSB);
    
    // Combine two bytes into a 16-bit unsigned int
     return ((Most_Significant_Int << 8) | Least_Significant_Int);
}


float Get_Exponent_Single_Precision(String Binary_String){
    
    int Power_Pre=0; 
    int Power;
    float Result; 
    for(int n=(Binary_String.length()-1);n>=0;n--){
    
        if(Binary_String.charAt(n)=='1'){
            Power_Pre = Power_Pre+pow(2,Binary_String.length()-1-n);    
        }
    }
    
    Power = Power_Pre -127;
    
    
    if (Power>16){
        
        Result = -1 ; 
        
    }else if (Power<-16){
        
        Result = -2;
        
        
    }else {
        
        Result = pow(2,Power);
        
    }
    
    return Result;
    
}




float Get_Fraction_Single_Precision(String Binary_String){
    
    float Final=1; 
    
    for(int n=0;n<Binary_String.length();n++){
    
        if(Binary_String.charAt(n)=='1'){
            Final = Final+pow(2,-(n+1));
        }
    }
    
    return Final;
    
}






String Add_Leading_Bits_8(String Bits){
    
      String  Zeros = "00000000";
      String  Final;

    if (Bits.length()<8)
        {
        Final =  Zeros.substring(0,8-Bits.length())+Bits;
        }
        else{
        Final =  Bits ;
    
        }
    
    return Final;
    
}


String Get_Float_From_Bytes_Single_Precision(byte val0, byte val1, byte val2, byte val3)
{
    

String All_Binary = Add_Leading_Bits_8(String(val3,BIN))+ Add_Leading_Bits_8(String(val2,BIN))+ Add_Leading_Bits_8(String(val1,BIN))+ Add_Leading_Bits_8(String(val0,BIN));

// //String All_Binary ="01000000110110011001100110011010";
 Serial.println("Binary Value :"+ All_Binary)    ;

String Sign_Binary = All_Binary.substring(0,1);
String Exponent_Binary = All_Binary.substring(1,9);
String Fraction_Binary = All_Binary.substring(9,32);

float Exponent = Get_Exponent_Single_Precision(Exponent_Binary);
float Fraction = Get_Fraction_Single_Precision(Fraction_Binary);


if (Exponent>0){
    float Float_Value = Exponent*Fraction;
    return String(Float_Value);
    }
    else if (Exponent==-1){
    Serial.println("VTL")    ;
    return "VTL";
    }
    else if(Exponent==-2)
    {
    Serial.println("VTS")    ;
    return "0";
    }else{
    Serial.println("Error")    ;
    return "Error";
    }

}    




bool Compare(byte array1[], byte array2[], int length) {
  for (int i = 0; i < length; i++){
    if (array1[i] != array2[i]) {
      return false;
    }
  }
    return true;
}



```


## Aug 20-23

#### Task 26 - Finalized SD Card Implilmentation

The previos code for the handling of the SD works only on publishing data to the SD card. For this iteration the SD has the capability to publish the data to the cloud while deleting whats being published. The final code for the SD Card implimentation is given here.

```
// This #include statement was automatically added by the Particle IDE.
#include <SdFat.h>


// Pick an SPI configuration.
// See SPI configuration section below (comments are for photon).
#define SPI_CONFIGURATION 0
SdFat sd;
SdFile file;
String Bike_Waggle_ID =  "Lakithas_Photon"; 
const uint8_t chipSelect = A2;

//------------------------------------------------------------------------------
// SCK => A3, MISO => A4, MOSI => A5, SS => A2 (default)

File myFile;

// File about to be Publishedd
int Sensorgram_Publish_Index;
// File about to be read 
int Sensorgram_Reading_Index;




void setup() {
Initialize_Serial();
delay(10000);
Initialize_SD();
delay(1000);
//Sensogram_Publisher();

}

void loop() {

Serial.println("Reading Index"+String(Sensorgram_Reading_Index));
Write_Data_To_Sensorgram_Waggle(" Hello Sensorgam");
Write_Data_To_Sensorgram_Waggle(" Hello Sensorgam");
Write_Data_To_Sensorgram_Waggle(" Hello Sensorgam");
Publish_Data_To_Cloud_Waggle();
//Publish_Data_To_Cloud_Waggle();
delay(5000);


}


void Initialize_Serial(){
    
  Serial.begin(9600);
  // Wait for USB Serial 

}



// SD Card functions
void Initialize_SD(){

  if (sd.begin(chipSelect, SPI_HALF_SPEED)) {
    Serial.println("SD Initiated");
      }else{
      Serial.println("SD Initiated");      
      
  }
  
   // Create a new folder.
  if (sd.mkdir(Bike_Waggle_ID+"_Data")) {
Serial.println("Data Folder Created");
  }else{
      Serial.println("Data Folder not Created");
      
  }
  
   // Create a new folder for sensorgram.
  if (sd.mkdir(Bike_Waggle_ID+"_Sensorgram")) {
        Serial.println("Sensorgram Folder Created");
          }else{
            Serial.println("Sensorgram Folder Creation Failed");
                }
    //Printing Headers 
        Serial.println("Data File Opended");
        Write_Data_To_SD_Waggle("# Project Bike Waggle");
        Write_Data_To_SD_Waggle("# Version 1.0");
        Write_Data_To_SD_Waggle("# Bike Waggle - "+Bike_Waggle_ID);
        // DateTime now = rtc.now();
        Write_Data_To_SD_Waggle("# Logs begin at " + Time.timeStr());
        Write_Data_To_SD_Waggle("# <Date_Time>,<Sensor>,<Parametor>,<Data>");




// Doing the Index for the Reading File
  if (sd.exists(Bike_Waggle_ID+"_Sensorgram/Reading_Index.txt"))
  {
      Serial.println("File Already Exists");

       Sensorgram_Reading_Index = atoi(Read_Data_from_SD(Bike_Waggle_ID+"_Sensorgram/Reading_Index.txt"));
       
      } else{
      
        Serial.println("File Does Not Exist");
        myFile.open(Bike_Waggle_ID+"_Sensorgram/Reading_Index.txt", O_CREAT | O_WRITE | O_AT_END) ;
        Serial.println("Reading Index File Opended");
        myFile.println("1");
        myFile.close(); 
        Sensorgram_Reading_Index = 1;
    }
    
    
    // Doing the Index for the Publish File
  if (sd.exists(Bike_Waggle_ID+"_Sensorgram/Publish_Index.txt"))
  {
      Serial.println("File Already Exists");
       
       Sensorgram_Publish_Index= atoi(Read_Data_from_SD(Bike_Waggle_ID+"_Sensorgram/Publish_Index.txt"));
       
       
      } else{
      
        Serial.println("Publish File Does Not Exist");
        myFile.open(Bike_Waggle_ID+"_Sensorgram/Publish_Index.txt", O_CREAT | O_WRITE | O_AT_END) ;
        Serial.println("Publish Index File Opended");
        myFile.println("1");
        myFile.close(); 
        Sensorgram_Publish_Index = 1;
    }
  }

  
String Read_Data_from_SD(String Path){
 // Create a file in Folder1 using a path.
      // re-open the file for reading:
  myFile = sd.open(Path);
  if (myFile) {
    Serial.println("Reading:"+Path);
    // read from the file until there's nothing else in it:
    String Current_Byte; 
    String Out = ""; 
    while (myFile.available()) {
    Current_Byte =(char) myFile.read();    
    Out.concat(Current_Byte);
    }
    // close the file:
    myFile.close();
    return Out;
  } else {
    // if the file didn't open, print an error:
    Serial.println("No Such File");
    return "-1";
  }
  
}


  
void Write_Data_To_SD_Waggle(String Data){
 // Create a file in Folder1 using a path.
  if (myFile.open(Bike_Waggle_ID+"_Data/Readings.txt", O_CREAT | O_WRITE | O_AT_END)) {
        Serial.println("Data File Opended");
        myFile.println(Data);
        myFile.close(); 
    }else
    {
     Serial.println("Data File Not Open");  
    } 
}


  
void Write_Data_To_Sensorgram_Waggle(String Data){
 // Create a file in Folder1 using a path.
  if (myFile.open(Bike_Waggle_ID+"_Sensorgram/"+Add_Leading_Zeros_8(Sensorgram_Reading_Index)+".txt", O_CREAT | O_WRITE)) {
        Serial.println("Sensorgram File Opended");
        myFile.print(Data);
        myFile.close(); 
        Sensorgram_Reading_Index =  Sensorgram_Reading_Index+1;
        // 
           if (myFile.open(Bike_Waggle_ID+"_Sensorgram/Reading_Index.txt",  O_CREAT | O_WRITE )){
            Serial.println("Sensorgram File Opended");
            myFile.print(String(Sensorgram_Reading_Index));
            myFile.close(); 
            }
         
     
     
    }else
    {
     Serial.println("Data File Not Open");  
    } 
}



void Publish_Data_To_Cloud_Waggle(){
 // Create a file in Folder1 using a path.
 String  Path =Bike_Waggle_ID+"_Sensorgram/"+Add_Leading_Zeros_8(Sensorgram_Publish_Index)+".txt";
 
 if(Sensorgram_Publish_Index<Sensorgram_Reading_Index)
    {
      if (sd.exists(Path))
      {
        String Sensorgram_Str = Read_Data_from_SD(Path);
        Serial.println("Sensorgram Read for Publishing");
            
            if(!Sensorgram_Str.equals("-1"))
            {
                if(Particle.publish("Sensorgram",Sensorgram_Str))
                {
                    Sensorgram_Publish_Index =  Sensorgram_Publish_Index+1;
                    // Remove files from current directory.
                    sd.remove(Path); 
                    
                    if (myFile.open(Bike_Waggle_ID+"_Sensorgram/Publish_Index.txt",  O_CREAT | O_WRITE )){
                        Serial.println("Publish Index File Opened");
                        myFile.print(String(Sensorgram_Publish_Index));
                        myFile.close(); 
                    }    
                }
            }
        }
    }
}



// Take from Conversions 
String Add_Leading_Zeros_8(int val){

      String Zeros    = "00000000" ;
      String Val_Str  = String(val);
      String Final;

    if (Val_Str.length()<Zeros.length())
        {
        Final  =  Zeros.substring(0,Zeros.length()-Val_Str.length())+Val_Str;
        }
        else{
        Final =  Val_Str;
    
        }
    
    return Final;
    
}

```

## September 27-31

#### Task 27 - Seeking Device Stabilty through the Gyrscope

The Bike Waggle device is meant to impliment a code which seeks the devices stabilty on each implimeted loop. The bike waggle will attemp to publish more data while the device is stable and on the otherhand it will read data more frequently while in motion. 

The following code will make use of the Gyrorates read by the Gyroscope to provide devices' stabilty states. 

```
// Making this bool to sort oyut whather the Device is stable or not  
void Read_MPU9250_BW()
{
    
    if (myIMU.readByte(MPU9250_ADDRESS, INT_STATUS) & 0x01)// Check of the sensor is working 
  {
    MPU9250_Online = true;
    myIMU.readAccelData(myIMU.accelCount);  // Read the x/y/z adc values
    myIMU.getAres();

    // Now we'll calculate the accleration value into actual g's
    // This depends on scale being set
    
    DateTime nowAcc = rtc.now();
    myIMU.ax = (float)myIMU.accelCount[0]*myIMU.aRes; // - accelBias[0];
    myIMU.ay = (float)myIMU.accelCount[1]*myIMU.aRes; // - accelBias[1];
    myIMU.az = (float)myIMU.accelCount[2]*myIMU.aRes; // - accelBias[2];

    myIMU.readGyroData(myIMU.gyroCount);  // Read the x/y/z adc values
    myIMU.getGres();

    // Calculate the gyro value into actual degrees per second
    // This depends on scale being set
    DateTime nowGyro = rtc.now();
    myIMU.gx = (float)myIMU.gyroCount[0]*myIMU.gRes;
    myIMU.gy = (float)myIMU.gyroCount[1]*myIMU.gRes;
    myIMU.gz = (float)myIMU.gyroCount[2]*myIMU.gRes;

    myIMU.readMagData(myIMU.magCount);  // Read the x/y/z adc values
    myIMU.getMres();
    // User environmental x-axis correction in milliGauss, should be
    // automatically calculated
    myIMU.magbias[0] = +470.;
    // User environmental x-axis correction in milliGauss TODO axis??
    myIMU.magbias[1] = +120.;
    // User environmental x-axis correction in milliGauss
    myIMU.magbias[2] = +125.;

    // Calculate the magnetometer values in milliGauss
    // Include factory calibration per data sheet and user environmental
    // corrections
    // Get actual magnetometer value, this depends on scale being set
    
    DateTime nowMag = rtc.now();
    myIMU.mx = (float)myIMU.magCount[0]*myIMU.mRes*myIMU.magCalibration[0] -
               myIMU.magbias[0];
    myIMU.my = (float)myIMU.magCount[1]*myIMU.mRes*myIMU.magCalibration[1] -
               myIMU.magbias[1];
    myIMU.mz = (float)myIMU.magCount[2]*myIMU.mRes*myIMU.magCalibration[2] -
               myIMU.magbias[2];
    
     
               
               
   // if (readByte(MPU9250_ADDRESS, INT_STATUS) & 0x01)

  // Must be called before updating quaternions!
  myIMU.updateTime();

  // Sensors x (y)-axis of the accelerometer is aligned with the y (x)-axis of
  // the magnetometer; the magnetometer z-axis (+ down) is opposite to z-axis
  // (+ up) of accelerometer and gyro! We have to make some allowance for this
  // orientationmismatch in feeding the output to the quaternion filter. For the
  // MPU-9250, we have chosen a magnetic rotation that keeps the sensor forward
  // along the x-axis just like in the LSM9DS0 sensor. This rotation can be
  // modified to allow any convenient orientation convention. This is ok by
  // aircraft orientation standards! Pass gyro rate as rad/s
  // MadgwickQuaternionUpdate(ax, ay, az, gx*PI/180.0f, gy*PI/180.0f, gz*PI/180.0f,  my,  mx, mz);

  MahonyQuaternionUpdate(myIMU.ax, myIMU.ay, myIMU.az, myIMU.gx*DEG_TO_RAD,
                         myIMU.gy*DEG_TO_RAD, myIMU.gz*DEG_TO_RAD, myIMU.my,
                         myIMU.mx, myIMU.mz, myIMU.deltat);


    myIMU.delt_t = millis() - myIMU.count;
    // if (myIMU.delt_t > 500) Make sure to have a delay between sensors 
    // {
        // Print acceleration values in milligs!
        float MPU9250AccelerationX = 1000*myIMU.ax;
        float MPU9250AccelerationY = 1000*myIMU.ay;
        float MPU9250AccelerationZ = 1000*myIMU.az;
        float MPU9250GyroRateX = myIMU.gx;
        float MPU9250GyroRateY = myIMU.gy;
        float MPU9250GyroRateZ = myIMU.gz;
        float MPU9250MagFeildX = myIMU.mx;
        float MPU9250MagFeildY = myIMU.my;
        float MPU9250MagFeildZ = myIMU.mz;
        float MPU9250Temprature    = (((float) myIMU.tempCount) / 333.87 + 21.0);
        
        MPU9250_Accelaration_X.Pack(Get_Current_Time_HEX(nowAcc),Float_32_to_Hex(&MPU9250AccelerationX),Get_Current_Time_SD(nowAcc),String(MPU9250AccelerationX));
        MPU9250_Accelaration_Y.Pack(Get_Current_Time_HEX(nowAcc),Float_32_to_Hex(&MPU9250AccelerationY),Get_Current_Time_SD(nowAcc),String(MPU9250AccelerationY));
        MPU9250_Accelaration_Z.Pack(Get_Current_Time_HEX(nowAcc),Float_32_to_Hex(&MPU9250AccelerationZ),Get_Current_Time_SD(nowAcc),String(MPU9250AccelerationZ));
        
        MPU9250_Gyro_Rate_X.Pack(Get_Current_Time_HEX(nowGyro),Float_32_to_Hex(&MPU9250GyroRateX),Get_Current_Time_SD(nowGyro),String(MPU9250GyroRateX));
        MPU9250_Gyro_Rate_Y.Pack(Get_Current_Time_HEX(nowGyro),Float_32_to_Hex(&MPU9250GyroRateY),Get_Current_Time_SD(nowGyro),String(MPU9250GyroRateY));
        MPU9250_Gyro_Rate_Z.Pack(Get_Current_Time_HEX(nowGyro),Float_32_to_Hex(&MPU9250GyroRateZ),Get_Current_Time_SD(nowGyro),String(MPU9250GyroRateZ));
        
        MPU9250_Magnetic_Feild_X.Pack(Get_Current_Time_HEX(nowMag),Float_32_to_Hex(&MPU9250MagFeildX),Get_Current_Time_SD(nowMag),String(MPU9250MagFeildX));
        MPU9250_Magnetic_Feild_Y.Pack(Get_Current_Time_HEX(nowMag),Float_32_to_Hex(&MPU9250MagFeildY),Get_Current_Time_SD(nowMag),String(MPU9250MagFeildY));
        MPU9250_Magnetic_Feild_Z.Pack(Get_Current_Time_HEX(nowMag),Float_32_to_Hex(&MPU9250MagFeildZ),Get_Current_Time_SD(nowMag),String(MPU9250MagFeildZ));

        MPU9250_Temperature.Pack(Get_Current_Time_HEX(nowMag),Float_32_to_Hex(&MPU9250MagFeildX),Get_Current_Time_SD(nowMag),String(MPU9250MagFeildX));
        
        // Serial.print("X-acceleration: "); Serial.print(1000*myIMU.ax);
        // Serial.print(" mg ");
        // Serial.print("Y-acceleration: "); Serial.print(1000*myIMU.ay);
        // Serial.print(" mg ");
        // Serial.print("Z-acceleration: "); Serial.print(1000*myIMU.az);
        // Serial.println(" mg ");

        // // Print gyro values in degree/sec
        // Serial.print("X-gyro rate: "); Serial.print(myIMU.gx, 3);
        // Serial.print(" degrees/sec ");
        // Serial.print("Y-gyro rate: "); Serial.print(myIMU.gy, 3);
        // Serial.print(" degrees/sec ");
        // Serial.print("Z-gyro rate: "); Serial.print(myIMU.gz, 3);
        // Serial.println(" degrees/sec");

        // // Print mag values in degree/sec
        // Serial.print("X-mag field: "); Serial.print(myIMU.mx);
        // Serial.print(" mG ");
        // Serial.print("Y-mag field: "); Serial.print(myIMU.my);
        // Serial.print(" mG ");
        // Serial.print("Z-mag field: "); Serial.print(myIMU.mz);
        // Serial.println(" mG");

      
        // // Print temperature in degrees Centigrade
        // Serial.print("Temperature is ");  Serial.print(myIMU.temperature, 1);
        // Serial.println(" degrees C");

    // Define output variables from updated quaternion---these are Tait-Bryan
    // angles, commonly used in aircraft orientation. In this coordinate system,
    // the positive z-axis is down toward Earth. Yaw is the angle between Sensor
    // x-axis and Earth magnetic North (or true North if corrected for local
    // declination, looking down on the sensor positive yaw is counterclockwise.
    // Pitch is angle between sensor x-axis and Earth ground plane, toward the
    // Earth is positive, up toward the sky is negative. Roll is angle between
    // sensor y-axis and Earth ground plane, y-axis up is positive roll. These
    // arise from the definition of the homogeneous rotation matrix constructed
    // from quaternions. Tait-Bryan angles as well as Euler angles are
    // non-commutative; that is, the get the correct orientation the rotations
    // must be applied in the correct order which for this configuration is yaw,
    // pitch, and then roll.
    // For more see
    // http://en.wikipedia.org/wiki/Conversion_between_quaternions_and_Euler_angles
    // which has additional links.
    
      myIMU.yaw   = atan2(2.0f * (*(getQ()+1) * *(getQ()+2) + *getQ() *
                    *(getQ()+3)), *getQ() * *getQ() + *(getQ()+1) * *(getQ()+1)
                    - *(getQ()+2) * *(getQ()+2) - *(getQ()+3) * *(getQ()+3));
      myIMU.pitch = -asin(2.0f * (*(getQ()+1) * *(getQ()+3) - *getQ() *
                    *(getQ()+2)));
      myIMU.roll  = atan2(2.0f * (*getQ() * *(getQ()+1) + *(getQ()+2) *
                    *(getQ()+3)), *getQ() * *getQ() - *(getQ()+1) * *(getQ()+1)
                    - *(getQ()+2) * *(getQ()+2) + *(getQ()+3) * *(getQ()+3));
      myIMU.pitch *= RAD_TO_DEG;
      myIMU.yaw   *= RAD_TO_DEG;
      // Declination of SparkFun Electronics (40°05'26.6"N 105°11'05.9"W) is
      // 	8° 30' E  ± 0° 21' (or 8.5°) on 2016-07-19
      // - http://www.ngdc.noaa.gov/geomag-web/#declination
       myIMU.yaw   -= 8.5;
       myIMU.roll  *= RAD_TO_DEG;
    //   Serial.print("Yaw, Pitch, Roll: ");
    //   Serial.print(myIMU.yaw, 2);
    //   Serial.print(", ");
    //   Serial.print(myIMU.pitch, 2);
    //   Serial.print(", ");
    //   Serial.println(myIMU.roll, 2);

    //   Serial.print("rate = ");
    //   Serial.print((float)myIMU.sumCount/myIMU.sum, 2);
    //   Serial.println(" Hz");

    // Print acceleration values in milligs!
        String Euler_Yaw = String(myIMU.yaw, 2);
        String Euler_Pitch = String(myIMU.pitch, 2);
        String Euler_Roll = String(myIMU.roll, 2);
        String Euler_Frequency = String((float)myIMU.sumCount/myIMU.sum, 2);
        

    myIMU.count = millis();
    myIMU.sumCount = 0;
    myIMU.sum = 0;
    
    Device_Stable = ((pow(MPU9250GyroRateX,2)+pow(MPU9250GyroRateY,2)+pow(MPU9250GyroRateZ,2)<10));// if this is true the device is stable 
    }else
    {
    MPU9250_Online = false;
    Device_Stable = false;  
    }


}// Read MPU 9250 

```
Further modifications of a unified BikeWaggle implementation was done during the week. 

## September 3-7

#### Task 28 - Completion of the Bike Waggle Code - Phase 1

The modules worked on the previuos weeks were put to one unified code. The following [link](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle/firmware) gives the said implimentation.


#### Task 30 -Completion of the Wiring Diagram
In order to complete the PCB for the Bike Waggle(Micro Waggle for bikes) the following Diagram was done. 

<img src="https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/Bike-Waggle-Wiring-Layout.svg">



Further modifications to the said diagram can be done through this [link](https://www.digikey.com/schemeit/project/bike-waggle-3-OEFH9F8401JG/).

## September 10-14

#### Task 31 - The Completion of the PCB

The PCB was done using [Eagle](https://www.autodesk.com/products/eagle/overview). The statud design files can be found [here](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle/PCB_Design).


##### The final Bike Waggle PCB

<img src="https://github.com/waggle-sensor/summer2018/blob/master/bike_waggle/PCB_Design/Bike_Waggle_PCB_Image.png">

## September 17th -21st 

#### Task 32 - Addition of the control elements for the bike waggle. 
The bike waggle is a spin off of Waggle Micro waggle architecture. The micro waggle supports several control elements of the nodes. An example code can be found [here](https://github.com/waggle-sensor/summer2018/tree/master/bike_waggle/controller).

To make way to such control options the Bikewaggle code was modified. T
The BW surrports the following controller options:
- Enabling and Disabling Sensors.
- Control of timing options in sensing and publishing data

The modified bike waggle code can be found [here](https://github.com/waggle-sensor/summer2018/blob/master/wijeratne/codes/Bike_Waggle_Current/firmware_4/firmware.ino).
