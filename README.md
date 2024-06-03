# Project Report
## Source Data and Statistics

### Data Overview
***Project goal***: to develop a predictive model that can estimate the value of a flats using historical data, including past flats prices and their specifications.

We have data from [Yandex.Realty](https://realty.yandex.ru) containing real estate listings for apartments in St. Petersburg and Leningrad Oblast from 2016 till the middle of August 2018. The dataset has a mix of data types, including integers, floating-point numbers, booleans, and strings. It occupies approximately 49.9 MB of memory.

### Data Types 
* bool: 2 columns (open_plan, studio)
* float64: 6 columns (last_price, area, kitchen_area, living_area, agent_fee, renovation)
* int64: 6 columns (floor, rooms, offer_type, category_type, building_id)
* object: 3 columns (first_day_exposition, last_day_exposition, unified_address)

### Missing Values
* kitchen_area: 87,596 missing values
* living_area: 64,643 missing values
* agent_fee: 279,958 missing values
* renovation: 133,528 missing values

### Data Statistics
 ***Clear outliers are observed in the columns: last_price, floor, rooms, area, kitchen_area, living_area, and renovation.*** For example, having 22 rooms in a residential property is rare and indicates a possible outlier, or an area of 1,000 square meters is uncommon for regular housing and is also an outlier. 

![alt text](statistic.PNG) 

### Data Visualization
#### Correlation
The highest correlation with the target variable is observed with the number of rooms (0.47) and area of ​​the apartment (0.76).
![alt text](Correlation.PNG) 
#### Visualization of data distribution
![alt text](hh.PNG)

## Model Information

***Features of our data set***: floor, open_plan, rooms, studio, area, renovation  
***Target of our data set***: last_price

The dataset was divided into three samples in a ratio of 3:1:1 into a training, validation and test set.

The models were developed using the scikit-learn framework, a widely used machine learning library in Python. As part of this project, 4 models were developed: LinerRegression, DecisionTreeRegressor, CatBoostRegressor and LGBMRegressor. The best results in terms of training speed and metrics were demonstrated by the DecisionTreeRegressor model, which was subsequently used on the test sample. 
* **Model Type:** DecisionTreeRegressor
* **Hyperparameters:** Hyperparameters were tuned using Grid Search with Cross-Validation (GridSearchCV). The parameter grid included:

  - splitter: ['best', 'random']
  - random_state: [42]
  - max_depth: range(1, 40)
    
  ***Best hyperparams***: {'max_depth': 7, 'random_state': 42, 'splitter': 'best'}

* **Learning Rate:** wall time: 3.68 s

The performance of the models was evaluated using three metrics:

![alt text](metrics.PNG)

## Installation and Running Instructions virtual environment
How to create and activate virtual environment
```
python3 -m venv env
source env/bin/activate 
```
List all pip installed packages you can find in the file requirements.txt. You can also download all the necessary libraries and packages for the project using the following command
```
pip install -r requirements.txt
```
Run the app using 
```
python app.py
```
## Dockerfile

### Dockerfile Content
```
FROM ubuntu:20.04
MAINTAINER Mariia Varlamova
RUN apt-get update -y
COPY . /opt/gsom_predictor
WORKDIR /opt/gsom_predictor
RUN apt install -y python3-pip
RUN pip3 install -r requirements.txt
CMD python3 app.py
```
This Dockerfile creates a Docker image that:

* Uses Ubuntu 20.04 as the base image.
* Updates the package list.
* Copies the current directory's contents into /opt/gsom_predictor inside the image.
* Sets /opt/gsom_predictor as the working directory.
* Installs python3-pip for managing Python packages.
* Installs the necessary Python packages listed in requirements.txt.
* Runs the Python application app.py when the container starts.

## How to open the port in remote VM
Connect to your remote VM using SSH. Use the following command to open the port: 
```
ssh <login>@<your_vm_address>
```
## How to run app using docker and which port it uses
```
docker run --network host -d varlamova28/gsom_e2e24:v.0.1
sudo ufw allow 7778
python app.py
```
The results:
![alt text](file.PNG)  ![alt text](file2.PNG)
