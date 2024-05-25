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
### Result:

| CapabilityName | Gemini_Score | GPT4_score |
| -------------- | ------------ | ---------- |
| General        | 88.2         | 86.4       |
| Reasoning      | 85.5         | 86.4       |
| Math           | 73.1         | 72.5       |
| Code           | 72.6         | 70.5       |
| Image          | 74           | 70.9       |
| Video          | 58.7         | 51.1       |
| Audio          | 23.8         | 23.4       |


**2. Which benchmarks does Gemini Ultra outperform GPT-4 in terms of scores?**
````sql
select Benchmarkname, 
       ScoreGemini, 
       ScoreGPT4 
from Benchmarks
where ScoreGemini> ScoreGPT4
order by ScoreGemini desc
````
### Result:

| Benchmarkname        | ScoreGemini | ScoreGPT4 |
| -------------------- | ----------- | --------- |
| GSM8K                | 94.4        | 92        |
| DocVQA               | 90.9        | 88.4      |
| MMLU                 | 90          | 86.4      |
| Big-Bench Hard       | 83.6        | 83.1      |
| DROP                 | 82.4        | 80.9      |
| TextVQA              | 82.3        | 78        |
| Infographic VQA      | 80.3        | 75.1      |
| VQAv2                | 77.8        | 77.2      |
| Natura12Code         | 74.9        | 73.9      |
| HumanEval            | 74.4        | 67        |
| VATEX                | 62.7        | 56        |
| MIMMU                | 59.4        | 56.8      |
| Perception Test MCQA | 54.7        | 46.3      |
| MATH                 | 53.2        | 52.9      |
| MathVista            | 53          | 49.9      |
| CoV0ST 2             | 40.1        | 29.1      |


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
### Result:

| CapabilityName | BenchmarkName   | ScoreGemini | ScoreGPT4 |
| -------------- | --------------- | ----------- | --------- |
| Image          | DocVQA          | 90.9        | 88.4      |
| Image          | TextVQA         | 82.3        | 78        |
| Image          | Infographic VQA | 80.3        | 75.1      |
| Image          | VQAv2           | 77.8        | 77.2      |
| Image          | MIMMU           | 59.4        | 56.8      |
| Image          | MathVista       | 53          | 49.9      |


**4. Calculate the percentage improvement of Gemini Ultra over GPT-4 for each benchmark?**
````sql
select BenchmarkName,
       round(((scoregemini-scoregpt4)/scoregpt4)*100,1) as Percentage_Improvement
from Benchmarks
group by Benchmarkname, 
       ScoreGemini, 
       ScoreGPT4
having Percentage_Improvement >0
````
### Result:

| BenchmarkName        | Percentage_Improvement |
| -------------------- | ---------------------- |
| MMLU                 | 4.2                    |
| Big-Bench Hard       | 0.6                    |
| DROP                 | 1.9                    |
| GSM8K                | 2.6                    |
| MATH                 | 0.6                    |
| HumanEval            | 11                     |
| Natura12Code         | 1.4                    |
| MIMMU                | 4.6                    |
| VQAv2                | 0.8                    |
| TextVQA              | 5.5                    |
| DocVQA               | 2.8                    |
| Infographic VQA      | 6.9                    |
| MathVista            | 6.2                    |
| VATEX                | 12                     |
| Perception Test MCQA | 18.1                   |
| CoV0ST 2             | 37.8                   |


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
### Result:

| BenchmarkName   | ScoreGemini | ScoreGPT4 |
| --------------- | ----------- | --------- |
| MMLU            | 90          | 86.4      |
| Big-Bench Hard  | 83.6        | 83.1      |
| DROP            | 82.4        | 80.9      |
| HellaSwag       | 87.8        | 95.3      |
| GSM8K           | 94.4        | 92        |
| HumanEval       | 74.4        | 67        |
| Natura12Code    | 74.9        | 73.9      |
| VQAv2           | 77.8        | 77.2      |
| TextVQA         | 82.3        | 78        |
| DocVQA          | 90.9        | 88.4      |
| Infographic VQA | 80.3        | 75.1      |


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
### Result:

| benchmarkname        |
| -------------------- |
| FLEURS               |
| CoV0ST 2             |
| MathVista            |
| MATH                 |
| Perception Test MCQA |
| MIMMU                |
| VATEX                |
| HumanEval            |
| Natura12Code         |
| VQAv2                |
| Infographic VQA      |
| TextVQA              |
| DROP                 |
| Big-Bench Hard       |
| MMLU                 |
| DocVQA               |
| GSM8K                |


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
### Result:

| Benchmarkname        | scoregemini | scoregpt4 | Gemini_Performance | GPT4_Performance |
| -------------------- | ----------- | --------- | ------------------ | ---------------- |
| GSM8K                | 94.4        | 92        | Excellent          | Excellent        |
| DocVQA               | 90.9        | 88.4      | Excellent          | Excellent        |
| MMLU                 | 90          | 86.4      | Excellent          | Excellent        |
| HellaSwag            | 87.8        | 95.3      | Excellent          | Excellent        |
| Big-Bench Hard       | 83.6        | 83.1      | Excellent          | Excellent        |
| DROP                 | 82.4        | 80.9      | Excellent          | Excellent        |
| TextVQA              | 82.3        | 78        | Excellent          | Excellent        |
| Infographic VQA      | 80.3        | 75.1      | Excellent          | Excellent        |
| VQAv2                | 77.8        | 77.2      | Excellent          | Excellent        |
| Natura12Code         | 74.9        | 73.9      | Good               | Good             |
| HumanEval            | 74.4        | 67        | Good               | Good             |
| VATEX                | 62.7        | 56        | Good               | Good             |
| MIMMU                | 59.4        | 56.8      | Good               | Good             |
| Perception Test MCQA | 54.7        | 46.3      | Not Bad            | Not Bad          |
| MATH                 | 53.2        | 52.9      | Not Bad            | Not Bad          |
| MathVista            | 53          | 49.9      | Not Bad            | Not Bad          |
| CoV0ST 2             | 40.1        | 29.1      | Bad                | Poor             |
| FLEURS               | 7.6         | 17.6      | Poor               | Poor             |


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
### Result:

| CapabilityName | ScoreGemini | Capability_ranking |
| -------------- | ----------- | ------------------ |
| Reasoning      | 95.3        | 1                  |
| Math           | 94.4        | 2                  |
| Math           | 92          | 3                  |
| Image          | 90.9        | 4                  |
| General        | 90          | 5                  |
| Reasoning      | 87.8        | 6                  |
| General        | 86.4        | 7                  |
| Reasoning      | 83.6        | 8                  |
| Reasoning      | 83.1        | 9                  |
| Reasoning      | 82.4        | 10                 |
| Image          | 82.3        | 11                 |
| Reasoning      | 80.9        | 12                 |
| Image          | 80.3        | 13                 |
| Image          | 77.8        | 14                 |
| Code           | 74.9        | 15                 |
| Code           | 74.4        | 16                 |
| Code           | 73.9        | 17                 |
| Code           | 67          | 18                 |
| Video          | 62.7        | 19                 |
| Image          | 59.4        | 20                 |
| Video          | 54.7        | 21                 |
| Math           | 53.2        | 22                 |
| Image          | 53          | 23                 |
| Math           | 52.9        | 24                 |
| Audio          | 40.1        | 25                 |
| Audio          | 7.6         | 26                 |


**9. Convert the Capability and Benchmark names to uppercase?**
````sql
Select upper(c.Capabilityname) as `Capability Name`, 
       upper(b.benchmarkname) as `Benchmark Name`
from Capabilities c
join Benchmarks b
using (capabilityid)
````
### Result:

| Capability Name | Benchmark Name       |
| --------------- | -------------------- |
| GENERAL         | MMLU                 |
| GENERAL         | MMLU                 |
| REASONING       | BIG-BENCH HARD       |
| REASONING       | BIG-BENCH HARD       |
| REASONING       | DROP                 |
| REASONING       | DROP                 |
| REASONING       | HELLASWAG            |
| REASONING       | HELLASWAG            |
| MATH            | GSM8K                |
| MATH            | GSM8K                |
| MATH            | MATH                 |
| MATH            | MATH                 |


**10. Can you provide the benchmarks along with their descriptions in a concatenated format?**
````sql
Select 
Concat(BenchmarkName , " - " , Description) as `Benchmark Descriptions`
from Benchmarks
````
### Result:

| Benchmark Descriptions                                                                            |
| ------------------------------------------------------------------------------------------------- |
| MMLU - Representation of questions in 57 subjects                                                 |
| Big-Bench Hard - Diverse set of challenging tasks requiring multi-step reasoning                  |
| DROP - Reading comprehension (Fl Score)                                                           |
| HellaSwag - Commonsense reasoning for everyday tasks                                              |
| GSM8K - Basic arithmetic manipulations, incl. Grade School math problems                          |
| MATH - Challenging math problems, incl. algebra, geometry, pre-calculus, and others               |
| HumanEval - Python code generation                                                                |
| Natura12Code - Python code generation. New held out dataset HumanEval-like, not leaked on the web |
| Natura12Code - Python code generation                                                             |
