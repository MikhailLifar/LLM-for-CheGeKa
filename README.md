# LLM-for-CheGeKa
The repository is dedicated to scoring LLM and prompt-engineering approaches on [CheGeKa](https://mera.a-ai.ru/ru/tasks/8) task from [MERA](https://mera.a-ai.ru/ru) benchmark.

## Features
* Querying SOTA LMs through replicate
* Asynchronous generation of responses to a large number of queries

## Approaches
Approaches tried to improve the responses of language models
* Simple Augment - Imposing additional restrictions on LMs output by supplementing the query with additional instructions like “Write in Russian”, “Answer briefly” and “Do not use punctuation”, as well as using a greedy output generation strategy.
* Chain of Thoughts (CoT) or "Think step by step" - adding a simple instruction to the end of the LM's prompt, encouraging it to process the query step by step.
* Extract Information - a CoT-like approach with multi-agent elements. The first agent tries to extract as much information about the answer as possible from the question text (whether the answer is an object, action, or property, to which time epoch he/she/it may belong, and so on). The second agent generates the final answer to the question based on the information from the first agent.
* Self-Consistency - multiple answer generation, the most popular answer among the generated is the final answer of the model.
We used Self Consistency in combination with Chain of Thoughts. The method parameter is the number of answer generations.
* Suggester-Discriminator - multiagent approach. The idea is the following: the first agent, a generator, generates an answer. The second agent evaluates the answer on a 10-point scale. If the score is below the threshold, the generation-evaluation cycle repeats. The method parameters are the threshold and the maximum number of generations. 
* SDwCA - Suggester-Discriminator with Critical Analysis. The idea is the following: the first agent, a generator, generates an answer. The second agent evaluates the answer on a 10-point scale. If the score is too low, then the third agent - the critic - explains why the answer did not fit. The generator then generates a new answer, taking into account all previous answers and criticism.
* Command-Discussion - multiagent approach, aiming to mimic discussion in the team. The idea is the following - the first agent generates an answer to a question with explanations. Another agent, the critic, puts forward a reasoned criticism of the proposed answer. The generator generates each next answer taking into account the previous answers and their criticism. After generating a fixed number of responses and criticism for each of them, a separate agent - the captain - analyzes the responses of all agents and gives the final answer. The method parameter is the number of response generations.

## Future
* Few shot
* Tree of Thoughts

## Results Table
Responses were generated with Llama3-405B-instruct model  
Note: so far the scores were measured not on 416 questions from the test/closed part of the benchmark, but on 416 questions from the train part. This should be OK since we didn't perform any training/fine-tunning to improve the quality of model responses.
| Approach    | Scores, f1 / exact match |
| -------- | ------- |
| As-is  | 0.444/0.139 |
| Simple Augment | 0.457/0.207 |
| CoT | 0.462/0.171 |
| Extract Information | 0.478/0.231 |
| Self-Consistency-4 | 0.474/0.233 |
| **Suggester-Discriminator-9-7** | **0.49 /0.236** |
| SDwCA-9-7 | 0.48 /0.233 |
| Command-Discussion-4 | 0.377/0.11 |

