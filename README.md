# Analysis of educational outcomes and opportunities across U.S. public high schools
Final Project for Data Engineering 200 course (Winter'25)

Group Members: Paula Fregene, Viktoriia Sokolenko, Evangeline Gong

This project analyzed datasets from the U.S. Department of Education to explore demographic, socioeconomic, and structural characteristics of U.S. public schools and school districts. By examining factors such as district location, SAT/ACT participation, and access to advanced coursework, particularly in STEM subjects, we aimed to identify patterns in educational disparities.
## Research Questions
#### How does the quality of school resources impact student performance?
#### How does a school district's location impact the availability and quality of education resources?
#### How does a school district's location impact the educational outcomes of its students?

## Python Libraries Used
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Plotly**
- **Scikit-learn**
  
## Data Collection and Preprocessing
We retrieved two datasets from the U.S. Department of Education:
#### [Civil Rights Data Collection (CRDC) for the 2021-2022 school year](https://civilrightsdata.ed.gov/data)
Administered biannually by the Office for Civil Rights, this dataset comprises multiple datasets within it and contains detailed information about schools’ enrollment, courses, and programs available for **17,704** Local Educational Agencies and **98,010** schools. To evaluate student outcomes per school, we calculated the percentage of high school students who took the SAT/ACT, using data from the SAT/ACT dataset and the Enrollment dataset. As proxies for school resources, we used the Advanced Placement, Advanced Mathematics, and Computer Science datasets to gather information about advanced class offerings, particularly the number of AP courses and classes in advanced mathematics and computer science offered in a school. 

#### [School District Characteristics (2022-23)](https://catalog.data.gov/dataset/school-district-characteristics-current-4aa03)
Provided by The National Center for Education Statistics (NCES) Common Core of Data (CCD) program, this dataset contains information on **13239** school districts in the US, including district type, school count, student-teacher ratio, and locale code text description. The format of locale values was “{numeric code}-{location category}: {size},” so to analyze the potential impact of school district location on school resources and college preparation, we divided all of the school districts into four categories: rural, town, city, and suburban, and two broader categories: rural and urban. Notably, the distribution of the schools in our final dataset across those categories was somewhat uneven, with suburban and city schools being overrepresented. This distribution, however, reflects the true distribution of schools in the US if compared to data from the National Center for Education Statistics in 2015-2016.

Finally, we related the two datasets based on **LEAID**, a unique code for Local Educational Agencies that helped us match data for each school district. To combine data for high schools, we used unique merged IDs (**SCHID**) specified in each CRDC dataset. This allowed us to analyze how district characteristics relate to school resources and student performance.

To ensure the effectiveness of our analysis, we dropped the rows where any of the numeric values (except latitude and longitude) were negative as negative codes in Civil Rights Data Collection's User’s Manual corresponded to invalid or non-applicable data. We also eliminated any schools where the percentage of students taking the SAT/ACT was higher than 100% or the  number of AP courses was higher than 40, since only 40 AP courses are registered on the College Board. Initially, 98,010 schools were included in the dataset, but after merging and checking the data for validity, only **8,637** were considered.

## Data Analysis
The variables used were the following:
#### School district location:

- LOCALE_CAT: category of the district's location (rural vs urban)
- LOCALE_CAT4: category of the district's location (rural, town, suburban, urban)
  
#### Student outcomes indicators:

- SCH_SAT_ACT_PERCENT: the percentage of high school students taking SAT/ACT at each school

#### School resources availability indicators:

- SCH_APCOURSES: the number of AP courses offered at each school

- SCH_MATHCLASSES_ADVM: the number of Advanced Math classes offered at each school

- SCH_COMPCLASSES_CSCI: the number of Computer Science classes offered at each school

- STUTERATIO: the student-teacher ratio at each school district

### Classification models
We used **Logistic Regression** and **Random Forest Classifier** with location category as the target variable to evaluate the relationship between the level of the school’s educational services and the district’s location. Across various feature subsets, Logistic Regression consistently resulted in higher accuracy scores after 5-fold cross-validation, so we chose this model for further analysis. To select the features leading to the highest accuracy scores, we ran 5-fold cross-validations for all the possible combinations of the following features: student-teacher ratio, number of AP courses, advanced math classes, computer science classes, and SAT/ACT participation rates. The two models with the highest average cross-validation scores were chosen to evaluate the relationship between the district’s location and school resources. The Logistic Regression model that classified a school as either rural or urban had the highest average accuracy of **0.7283** when it used the following features: number of AP courses, student-teacher ratio, number of advanced math classes, and number of computer science classes. The second most accurate model, with a mean accuracy of **0.7280**, had mostly the same features but did not consider computer science classes. The results of model testing suggested there is a correlation between the school’s location and the teacher support, as well as the rigor of curriculum offerings. 

### Visual representations
To better visualize the impact of school resources on student outcomes, we transformed several numerical variables (number of AP courses offered, number of Advanced Math classes offered, and number of Computer Science classes offered) into categorical variables by dividing them into buckets based on their 5-number summary statistics. A series of bar graphs was used to compare student academic outcomes, measured by the percentage of students taking SAT/ACT, across schools with different resource availability, which was quantified using the number of Computer Science, Advanced Math, and Advanced Placement (AP) courses offered. All bar graphs showed a clear positive correlation between the number of class offerings and the median SAT/ACT participation rates. 

The bar graphs were also used to visualize how different location types (rural, town, suburban, urban) influenced the academic support available in the school and the rigor of the school curriculum. The median number of Advanced Placement courses was more than twice as high in urban and suburban areas as in rural and town ones, and the median number of advanced mathematics and computer science classes was also shown to be higher in urban and suburban high schools. However, the median student-teacher ratio was shown to be higher in suburban and urban areas, which suggests that students there receive less individualized attention. A potential explanation for this trend is that the areas with higher population density have higher student enrollment, which would contribute to a higher student-teacher ratio.

We utilized a map to visually represent the distribution of the median SAT/ACT participation percentages in each high school across the United States, with larger and darker markers indicating higher participation rates. Based on visual observations, the distribution of markers is substantially clustered in the South, Midwest, and East Coast regions of the US. We also used a bar graph to compare the percentage of students who took the SAT/ACT in rural, town, suburban, and urban school districts. It showed a similar median across all school district location types, which indicates that there is no direct correlation between these two variables.

## Summary
This project examined disparities in educational support and opportunities across U.S. public high schools. Through machine learning methods and statistical analysis, several conclusions were drawn. First, there is a positive correlation between school resource availability and student SAT/ACT participation rates, indicating that schools with a wider selection of rigorous classes offer greater college preparation. Second, a strong relationship exists between a school district's location type and the academic support and resources it offers, which suggests that geographic factors (i.e., urbanization of the area) influence the learning opportunities students receive. Third, no clear correlation was found between district location and student outcomes, a surprising result that raises further questions about the interplay of factors not considered in this study.
