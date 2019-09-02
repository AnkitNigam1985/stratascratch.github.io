[![strata scratch](../assets/sslogo.jpg)](https://stratascratch.com)

# Strata Scratch

## Medical Appointments Case Study

Case study for practicising your skills in statistical thinking in python. We analyze a dataset which is a list of patients which suffer from one or more illnesses and draw a series of valid conclussions from the data by utilizing hypothesis tests and contigency tables.

#### Accessing The Data Resources
- Medical appointments dataset can be found under `medical_appointments`
- Access the data at www.stratascratch.com
- You can use SQL on Strata Scratch to answer the questions or connect to the datasets and answer the questions using other tools like python
- You can now run Jupyter notebooks on Google CoLab so there’s no software installation needed. Use this [introductory notebook](https://colab.research.google.com/drive/1tHxAbgbxM60VUIrVQW508EwB1b3wFk5g) as a template to start the analytics case.


### Dataset Description
This dataset contains a list of patients suffering from one or more of hypertension, diabetes, alcoholism and handcap, all of them binary variables, along with patient information (gender, age, neighbourhood) and information about appointment and scheduling times. There is also a no_show column which tells us if the patient visited the doctor.

### Business Case

- You are designing the healthcare software and want to know the impact of showing for appointments and receiving reminder SMS message. How many patients received an SMS and didn't show? What about the number of those who didn't show but also didn't receive a SMS. Use a contigency table to find if receiving a SMS and showing are dependent. 

- Plot the age distribution for each neighbourhood for patients with diabetes. What does it tell you? Find the mode of age for each neighbourhood.

- From the plots and modes you saw above you are lead to believe that mean age for diabetes patients is one of 40, 45, 50, 55, 60, 65, 70, 75, 80. Test each of these values using a z-test and explain the p-values you get for each of them.

- Plot the age distribution for each neighbourhood for patients with diabetes. What does it tell you? Find the mode of age for each neighbourhood.

- Draw the ECDF for day difference for female alcoholics. What does it tell you? What is the median number of days between scheduling and appointment?
