# Gemini-Vs-ChatGPT-SQL-Mini-Project

![Black Violet Pink 3D Company Internal Deck Business Presentation](https://github.com/puliraghavi/Gemini-Vs-ChatGPT---SQL-Mini-Project/assets/119037510/553c4d26-fe2b-4f0b-bf07-77147ba39ae4)

## 📚Contents
- [Problem Statement](#problem-statement)
- [Questions and Solution](#question-and-solution)

***
## Problem Statement

This project aims to compare the capabilities of two large language models (LLMs): Gemini Ultra and ChatGPT 4. Through a data analytics approach using SQL, we will analyze their performance across various benchmarks to determine their strengths and weaknesses.

Objectives:
1. Identify key capabilities of Gemini Ultra and ChatGPT 4.
2. Evaluating the above capabilities using relevant benchmarks using a scoring system.
3. Compare the performance of both models across the chosen benchmarks.
4. Gain insights into the relative strengths and weaknesses of each LLM.

***
## Questions and Solution

**1. What are the average scores for each capability on both the Gemini Ultra and GPT-4 models?**
````sql
select c.capabilityname,          
       round(avg(b.scoregemini),1) as Gemini_Score, 
       round(avg(b.scoregpt4),1) as GPT4_score
from Capabilities c
join Benchmarks b
using (capabilityid)
group by 1
````

**2. Which benchmarks does Gemini Ultra outperform GPT-4 in terms of scores?**
````sql
select Benchmarkname, 
       ScoreGemini, 
       ScoreGPT4 
from Benchmarks
where ScoreGemini> ScoreGPT4
order by ScoreGemini desc
````

**3. What are the highest scores achieved by Gemini Ultra and GPT-4 for each benchmark in the Image capability?**
````sql
select c.CapabilityName,
       b.BenchmarkName, 
       b.ScoreGemini,
       b.ScoreGPT4 
from Benchmarks b 
join  Capabilities c
using (capabilityid)
where c.CapabilityName = "Image"
order by scoregemini desc, scoregpt4 desc
````

**4. Calculate the percentage improvement of Gemini Ultra over GPT-4 for each benchmark?**
````sql
select BenchmarkName,
       round(((scoregemini-scoregpt4)/scoregpt4)*100,1)              as Percentage_Improvement
from Benchmarks
group by Benchmarkname, 
       ScoreGemini, 
       ScoreGPT4
having Percentage_Improvement >0
````

**5. Retrieve the benchmarks where both models scored above the average for their respective models?**
````sql
select BenchmarkName, 
       ScoreGemini, 
       ScoreGPT4
from Benchmarks
where ScoreGemini > 
      (select Avg(Scoregemini) from Benchmarks)
      and
      ScoreGPT4 >
      (select Avg(ScoreGPT4) from Benchmarks)
````

**6. Which benchmarks show that Gemini Ultra is expected to outperform GPT-4 based on the next score?**
````sql
with cte as(
select
BenchmarkName,
ScoreGemini,
lead(ScoreGemini) over (order by ScoreGemini) as leadgemini,
ScoreGPT4
from Benchmarks
)
select benchmarkname
from cte
where scoregpt4 < leadgemini and scoregpt4 is not null
````

**7. Classify benchmarks into performance categories based on score ranges?**
````sql
select Benchmarkname,
       scoregemini,
case when scoregemini >= 75 then "Excellent"
     when scoregemini >= 55 and scoregemini < 75 then "Good"
     when scoregemini >= 45 and scoregemini < 55 then "Not Bad"
     when scoregemini >= 35 and scoregemini < 45 then  "Bad"
     else"Poor" end as Gemini_Performance,
scoregpt4,
case when scoregpt4 >= 75 then "Excellent"
     when scoregpt4 >= 55 and scoregemini < 75 then "Good"
     when scoregpt4 >= 45 and scoregemini < 55 then "Not Bad"
     when scoregpt4 >= 35 and scoregemini < 45 then "Bad"
     else "Poor" end as GPT4_Performance
From Benchmarks
where scoregpt4 is not null
order by scoregemini desc, scoregpt4 desc
````
     
**8. Retrieve the rankings for each capability based on Gemini Ultra scores?**
````sql
select c.CapabilityName, 
       b.ScoreGemini,
       rank()over(order by scoregemini desc) as              Capability_ranking
from Capabilities c
join Benchmarks b
using (capabilityid)
order by Capability_ranking
````

**9. Convert the Capability and Benchmark names to uppercase?**
````sql
Select upper(c.Capabilityname), 
       upper(b.benchmarkname)
from Capabilities c
join Benchmarks b
using (capabilityid)
````

**10. Can you provide the benchmarks along with their descriptions in a concatenated format?**
````sql
Select 
Concat(BenchmarkName , " - " , Description) as `Benchmark Descriptions`
from Benchmarks
````
