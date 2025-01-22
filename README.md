# Are Generative AI Agents Effective Personalized Financial Advisors?

This repository provides resources such as additional explanations of our experimental design, scripts for data analysis, and data collected through a user study.

---

## Additional Experimental Settings

### LLM Advisor Configurations
In our user study, we employ [`llama-3.1-8B-Instruct`](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct) as our base LLM model. 
- **Temperature:** 0.7  
- **Top-p:** 0.7  
- **Top-k:** 50  
- **Context window size:** 25 most recent turns from the conversation history.  
- **Token limit:**  
  - 128 tokens for preference elicitation and advisory discussion  
  - 256 tokens for response summarization  

All prompts used in the user study are stored in `./study_data/_static/prompt/`.

### Asset Information
As explained in Section 3.3 of our paper, we prepared a standard asset descriptor block after consulting a financial expert.

- **Stock Prices & Business Summaries**  
  Data retrieved from [Yahoo! Finance](https://finance.yahoo.com/).  
- **Recent Performance & Key Financial Indicators**  
  Earnings conference call transcripts from [Seeking Alpha](https://seekingalpha.com/) for the last quarter of 2023.  
- **Monthly Stock Prices for 2023**  
  Also sourced from [Yahoo! Finance](https://finance.yahoo.com/), assuming the study took place on December 31, 2023.  

Because the conference call transcripts can be long (about 8,000 tokens), we first summarize them using [`llama-3.1-8B-Instruct`](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct)[^1], reducing them to 700–800 tokens. This summarized text is used as the **stock context** in our study.

### Investment Preferences and Ground Truth Ranking
We consulted financial professionals to build a rule-based approach for scoring the relevance of [S&P&nbsp;500](#fn1) companies to each investor’s preferences. Scores range from **0** to **3** based on how many key preferences the company meets:

- **Industry Preference**  
  Matched by comparing the company’s industry classification to the preferred industry in the investor’s narrative[^2].
- **Stock Style**  
  Distinguished between **growth** and **value** stocks, following the [Fama and French (1998)](#fn3) definition.  
- **Dividend Payment Preference**  
  Identified **dividend aristocrats** for participants seeking consistent dividend growth[^4].
- **Sensitivity to Global Market Changes**  
  Used the [Morningstar stock sector structure](#fn5) to classify stocks as cyclical or non-cyclical.

For each investor narrative, we selected four companies with varying relevance scores (0–3). We ensured no overlap of companies across participants and consulted a financial expert for verification.


## Data
Data from our user study is stored in `study_data/user_study_data`.

- **`advisor_assessment.csv`**  
  Contains users’ assessments of the advisor on a 7-point Likert scale during the *Asset Rating and Feedback* phase.

- **`advisory_discussion.csv`**  
  Includes conversations between participants and the advisor during *Advisory Discussion*.

- **`final_ranking.csv`**  
  Provides participants’ rankings of stocks during the *Asset Rating and Feedback* phase.

- **`preference_elicitation.csv`**  
  Contains conversations between participants and the advisor during *Preference Elicitation*.

- **`user_profile_and_annotation.csv`**  
  Includes the generated user profile after preference elicitation and a manual annotation indicating how accurately the profile was captured.


## Statistical Analysis

### Setup
**Install required packages**:
   pip install -r requirements.txt

### `analysis_script/stats_analysis.py`
This script performs statistical analysis. It includes the following key functions:

- **`final_perception_statistical_analysis(pair_df, final_survey, control_experiment_id)`**  
  Performs statistical analysis on the final evaluation.

- **`main()`**  
  Loads and preprocesses data, then performs statistical analysis.

**Run the statistical analysis**:
   python analysis_script/stats_analysis.py


## License
This project is licensed under the MIT License.




