<!-- MRD Support Prompts (DIN-SQL, MAC-SQL, Xiyan-SQL) -->

# MRD Support Prompts

> Sub-queries are grouped based on their query ID, ensuring proper sequence and contextual consistency.


  here we present the MRD support prompt used by DIN-SQL, MAC-SQL, Xiyan-SQL evaluated in our paper.

  Sub-queries are grouped based on their query ID, ensuring proper sequence and contextual consistency. The prompt for Query1 is as follows:
  
<details open>
  <summary><strong>Prompt — Query 1</strong></summary>

  
  ```text

  As a data analysis expert, you are to extract the necessary information from the data provided and output the corresponding SQL query based on the user's question. Let us consider the problem step by step. The current question is {Query1}.
  ```
  
</details>

  , the prompt for Query2 is shown below:
  
<details>
  <summary><strong>Prompt — Query 2</strong></summary>

  
  ```text

  As a data analysis expert, you are to extract the necessary information from the data provided and output the corresponding SQL query based on the user's question. Let us consider these problems step by step. The last question is {Query1}, and the current question is {Query2}.
  ```
  
</details>

  , and the prompt for Query3 is also provided as follows:
  
<details>
  <summary><strong>Prompt — Query 3</strong></summary>

  
  ```text

  As a data analysis expert, you are to extract the necessary information from the data provided and output the corresponding SQL query based on the user's question. Let us consider these problems step by step. The last two question are {Query1} and {Query2}, and the current question is {Query3}.
  ```
  
</details>
