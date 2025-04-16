# Overview

Welcome to my Analysis of the data job market, focusing on data analyst roles. This project was created out of desire to understand the job market more effectively. This project examines the highest-paying and most sought-after skills, offering insights into securing optimal job prospects in data analysis.

Data Source: 
https://huggingface.co/datasets/lukebarousse/data_jobs

# The Analysis

## 1. What are the most indemand skills for the top 3 most popular data roles?

To find the most indemand skills for the top 3 roles, I filtered out those positions by which onces were the most popular, and get the top 5 skills for the top 3 roles. By doing this it will show which skill should focus on based on the role.

Refer to Jupyter Notebook for the detailed steps: [2_Skills_Demand.ipynb](./3_Project/2_Skills_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(top_jobs), 1, figsize= (12,8))
sns.set_theme(style='ticks')

for i,job in enumerate(top_jobs):
    top_skills = merge_skill_job[merge_skill_job['job_title_short'] ==  job].head(5)
    sns.barplot(
        data= top_skills
        ,x= 'skill_perc'
        ,y= 'job_skills'
        ,ax = ax[i]
        ,hue= 'skill_perc'
        ,palette= 'dark:b_r'
        ,legend= False
        )
    ax[i].set_ylabel(f'{job}')
    ax[i].set_xlim(0,100)
    
    if i < len(top_jobs)-1:
        ax[i].set_xlabel('')
        ax[i].set_xticks([])
    else: 
        ax[i].set_xlabel('Skill Occurence rate(%)')
        ax[i].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,i: f'{x}%'))

    for v, n in enumerate(top_skills['skill_perc']):
        ax[i].text(x= n+1,y= v,s= f'{n:.0f}%',va= 'center')

fig.suptitle('Top 5 Indemand Skills', fontsize= 18)
fig.tight_layout()

plt.show()
```

### Results
![Visualization of Top Skills for Top Roles](./3_Project/Images/Skill_Demand.png)
*Bar graph visualizing the In-demand skills for trending jobs in 2023.*

### Insights

- Python is highly requested across the roles, but prominently for Data Scientists(66%).

- SQL is the most requested skill, highly demanded across all three roles.

- Data Engineer requires more specialized technical skills (AWS, Azure, Spark) compared to Data Analyst and Data Scientist who are expected to be proficient in more general data management and data visualization tools (SQL, Excel, Tableau).

## 2. How are In-demand skills trending for Data Analysts in United States?

To identify the trend of in-demand skills for Data Analyst role in United States, I filtered the dataset for Posted Jobs of Data Analyst in United States then get the monthly posted jobs count and compare it to monthly skill counts to get the likelihood of skills in job posting.

Refer to Jupyter Notebook for the detailed steps: [3_Skill_Trend.ipynb](./3_Project/3_Skill_Trend.ipynb)

### Visualize Data

```python
ax = sns.lineplot(
        data=skill_perc[top_skills_list]
        ,dashes= False
        )
sns.set_theme(style= 'ticks')
sns.move_legend(ax, "upper left", bbox_to_anchor=(1,1))
sns.despine()
ax.set_title('Trending Skills for Data Analyst in US')
ax.set_xlabel(2023)
ax.set_ylabel('Likelihood in Job Posting')
ax.set_ylim(0,60)
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda x,i: f'{x:.0f}%'))
```

### Results

![Skill Trend for Data Analyst in US](./3_Project/Images/Skill_Trend_DA_US.png)
*Line chart visualizing the monthly trend of top skills for Data Analyst in US in 2023.*

### Insights

- SQL remains the indemand skills throughout hte year, altough it shows a gradual decrease in demand.

- Excel experience a decrease from Sep to Nov but increased in Dec.

- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations.

- Although SAS is less demanded compared to the others it also show stable demand throughout the year.

- There's a sudden increase of demand in December for all trending skills.

## 3. How well do jobs and skills pay for Data Analysts?

Compare the salary distribution of Data Analyst to other indemand job.

Refer to Jupyter Notebook for the detailed steps: [4_Salary_Analysis](./3_Project/4_Salary_Analysis.ipynb)

### Visualize Data

```python
job_order = df_US_top6.groupby('job_title_short')['salary_year_avg'].median().sort_values(ascending=False).index

sns.boxplot(data=df_US_top6,x='salary_year_avg',y='job_title_short', order=job_order)

ax = plt.gca()
ax.set_xlim(0,700_000)
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x,i : f'{int(x/1000)}K'))
ax.set_xlabel('Yearly Salary ($USD)')
ax.set_ylabel('')
ax.set_title('Salary Distribution in United States', fontsize= 18)
```

### Results

![Salary Distribution for Indemand Jobs](./3_Project/Images/Salary_Distribution_US.png)
*Boxplot visualizing the distribution of salary for in-demand jobs in United States.*

### Insights

- Senior Roles for Data Scientist and Data Engineer have the hight yearly salary compared to all.

- Senior role for data analyst have a lower median yearly salary compared to Data Scientist and Data Engineer.

- Median salaries increases with the seniority and specialization of the roles.