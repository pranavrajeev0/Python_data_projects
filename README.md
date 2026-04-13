# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It dives into the top-paying and in-demand skills to help find optimal job opportunities for data analyst.

The data sourced from [Luke barousse's Python Course](https://youtu.be/wUSDVGivd-8?si=o7efSCR1RvUafalN) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essentail skills. Through a series of Python scripts, I explore key questions such as the most demand skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts? 
4. What are the optimal skills for Data analysts to learn? (High demand & High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:
 - **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries
   - **Pandas Library**: This was used to analyze the data.
   - **Matplotlib Library**: I visualized the data.
   - **Seaborn Library**: Helped me create more advanced visuals.
- **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code**: My go-to For executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and Project Tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I Start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter US Jobs

To Focus my analysis on the US Job market, I apply filter to the dataset, narrowing down to roles based in the United States.

```python
df_US = df[df['job_country'] == 'United States']
```

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions which were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I am targeting.

View my notebook with detailed steps here: [2_skill_demand.ipynb](3_Projects\2_Skill_demand.ipynb)

### Visualize Data

```python
fig,ax = plt.subplots(3,1, figsize=(8,6))

for i, job_title in enumerate(job_titles):
    df_plot = df_perc[df_perc['job_title_short'] == job_title].head(5)
    sns.set_theme(style='ticks')
    sns.barplot(data = df_plot, x='skill_percentage', y='job_skills', ax=ax[i], hue='skill_percentage', palette='dark:b_r', legend= False)

plt.show()
```
### Results

![Visualisation of Top skills for data nerds](3_Projects\images\skill_demand_data_roles.png)

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
- SQL is the most requested skill for Data Analysts and Data Scientists, with it over half the job postings for both roles. For Data Engineers, Python is the most sought after skill, appearing in 68% of Job Postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tool (Excel, TAbleau)

## 2. How are in demand skills trending for Data Analyst Roles in the US?

### Visualize Data

```python
sns.lineplot(data=df_plot, palette='tab10',dashes=False,marker='o', legend=True)

from matplotlib.ticker import PercentFormatter
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Results ###
![Visualization of trending skills for data analysts in US ](3_Projects\images\trending_skills.png)
*Bar graph visualizing the trending skills for data analysts in the US.*

### Insights

- SQL remains the most demand skill  with a relatively stable demand throughout the year but shows a clear dip from October to November, followed by a slight reocovery in December
- Followed by excel and rest of the other skills, with a relatively stable demand throughout the year and a sudden increase in demand towards the end of the year
- Around the month of August, Python surpassed tableau in demand for data analyst before declining again in the following months

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Nerds

#### Visualize Data

```python
sns.boxplot(data=df_Data, x='salary_year_avg', y='job_title_short', order=df_top6)

plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K' if x >= 1000 else f'{int(x)}'))
plt.show()
```

#### Results

![Visualisation of Salary distribution for data roles](3_Projects\images\Salary_distribution_for_data_roles.png) *box plot visualizing the salary distributions for the top 6 data job titles*

#### Insights

1. The Visualization clearly shows that Data Scientists and Data Engineers earn more than Data Analysts at entry level. Even when we compare senior roles, a Senior Data Analyst still earns less than a Senior Data Scientist or Senior Data Engineer. This means that experience alone does not close the pay gap between those roles.
2. The middle salary (average range) for Data Scientists and Engineers is higher than for Analysts. There is a small overlap, but overall, these roles are paid more because they usually involve more technical work like coding, machine learning, or building data systems. Data Analysts, who often focus on reports and insights, are paid less in comparison.
3. The highest salaries (top end) are also much bigger for Data Scientists and Engineers. They have some very high-paying outliers, while Data Analysts, even senior ones have a smaller salary range. This shows that Data Analyst roles have a lower earning limit, while others have more chances to earn very high salaries.

### Highest paid and Most Demanded skills for Data Analysts in the US

#### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

sns.barplot(data=df_top_pay, x='median', y=df_top_pay.index, ax=ax[0],hue='median', palette='dark:b_r', legend=False)
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K' if x >= 1000 else f'{int(x)}'))

sns.barplot(data=df_top_skills, x='median', y=df_top_skills.index, ax=ax[1],hue='count', palette='light:b', legend=False) 
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K' if x >= 1000 else f'{int(x)}'))

plt.show()
```

#### Results

![Visualization of Tops paying skills vs most demanded skills](3_Projects\images\top_pay_most_demand_skills.png)

#### Insights

1. The top-paying skills are mostly advanced technical tools like *dplyr,git,solidity, and hugging face.* These skills are not always the most commonly used in daily analyst work, but they are specialized and harder to learn. That.s why companies are willing to pay more for them.
2. The most in-demand skills are basic and widely used tools like *SQL, Excel, Python, and Tableau.* These are essential for most Data Analyst jobs, which is why they appear frequently. However, because many people know these skills, they usually offer lower salaries compared to niche skills.
3. There is a clear gap between high-paying skills and high demand skills. Skills that are in high demand are not always the highest paid, and vice versa. This means that if a Data analyst wants to earn more, they may need to go beyond basic tools and learn more advanced or specialized skills.

## 4. What is Most Optimal skill to learn for Data Analysts?

### Visualize Data

```python
from adjustText import adjust_text
sns.scatterplot(
    data=df_scatter,
    x='percentage', 
    y='median',
    hue='Technology',
    )
texts = []
for i, txt in enumerate(df_scatter['Skills']):
    texts.append(plt.text(df_scatter['percentage'].iloc[i]+0.4,df_scatter['median'].iloc[i], txt, fontsize=9))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='red'))

plt.show() 
```

### Results

![Visualization of the most optimal skills for Data Analyst](3_Projects\images\Most_optimal_skills.png)*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US*

### Insights

1. The most "optimal" skills are those that balance good salary and high demand. From the chart, SQL and Python stand out, they have relatively high salaries and are also required in many jobs. This makes them very vulnerable skills for Data Analysts to focus on.
2. Some tools like Tableau and Power BI offer a good middle ground. They don't pay as high as python, but they are fairly common in job requirements and still provide solid salaries. This shows that visualization tools are important for steady career growth.
3. Skills like Excel, Word, and PowerPoint are used often (especially Excel), but they offer lower salaries compared to more technical skills. On the other hand, skills like SQL Server or R may pay well but are less commonly required. This means the best strategy is to combine high demand skills with higher paying technical skills.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance:** I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis:** The Project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

- **Skill demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skill command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies:** Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth:** Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of contionuous learning and adaptation in the data field.















