## community
The community I chose is 'Let's Talk Music' from the Reddit subdirectory r/LetsTalkMusic. 
This community is dedicated to passionate music enthusiasts engaging in stimulating, in-depth discussions. Members actively ask questions, share personal insights, offer recommendations, and analyze musical works. The discourse spans balanced critiques, positive appreciations, and direct criticisms of musicians and their art.

## labels
** criticism:** The text expresses a primarily negative, disapproving, or frustrated view of an artist's work, a specific genre, an industry trend, or common listener behaviors; while minor positive remarks may be present, they do not dominate the overall unfavorable tone.

** critique:** The text provides a balanced and neutral evaluation of the artist's work, incorporating both positive and negative remarks.  

** appreciation:** The text expresses a primarily positive or appreciative view of the artist's past or present albums and songs; while minor negative remarks may be present, they do not dominate the overall favorable tone.

## why this distinction matter
I have come up with three labels for this project. While there can be more labels to categorize the posts in this community, I found that most of the posts can be classified into three labels. I was thinking to add another label as `discussion_prompt` because some of the posts include questions rather than any suggestions, views, or opinions. But, I thought that this could confuse the model as almost all the posts contain at least one or more questions. Therefore, I decided not to use it and stuck with these three instead. The following is the reasoning behind these labels.

`criticism`: A label I came up with after analyzing many posts. Some threads carry relatively unfavorable, negative, or disapproving remarks toward a musician, band, or their work, sometimes even utilizing harsh language. While minor positive remarks may occasionally be present, they do not dominate the overall unfavorable, negative tone.

`appreciation`: This label captures posts that are distinctly favorable toward a particular artist or their work. While these posts might contain minor negative caveats, the overall tone and attitude remain highly positive, celebratory, and supportive of the musician or band.

`critique`: I assigned this label to posts that offer a balanced, neutral, and analytical way of presenting ideas. While I could have split this into two separate labels—`analysis` and `critique`—I decided to keep them under one unified label, `critique`. The reason for this is that while this community hosts highly interesting discussions, these posts are not necessarily at an academic level. Therefore, the analysis or critique can be more informal and may not have a clear boundary to distinctly identify them as separate categories. Nonetheless, as analysis and critique go hand in hand here, I decided to keep such posts under one label `critique`.

I believe these distinctions are important for community members so they can easily search for what others think of their favorite artists or discover different perspectives on an artist's work. Additionally, these labels can assist students in this community by helping them find intriguing ideas for their academic literacy courses.

### label definitions
<!-- add examples later -->
`criticism`: The text expresses a primarily negative, disapproving, or frustrated view of an artist's work, a specific genre, an industry trend, or common listener behaviors; while minor positive remarks may be present, they do not dominate the overall unfavorable tone.

`appreciation`: The text expresses a primarily positive, admiring, and appreciative view of the artist's past or present albums and songs; while minor negative remarks may be present, they do not dominate the overall favorable tone.

`critique`: The text provides an objective, analytical evaluation of the music's structural or artistic elements (such as production, lyricism, or historical context), utilizing systematic reasoning and evidence rather than purely emotional preferences or personal venting.


## Hard Edge Cases & Decision Rules

I have found that the most challenging posts to classify sit on the boundaries between `criticism` and `critique`, or between `appreciation` and `critique`. 

### The Core Problem: Sentiment Count vs. Textual Intent
We cannot accurately classify these posts by merely counting positive and negative words. For example, a high number of positive words does not automatically mean a post belongs to `appreciation`, nor does an equal balance of positive and negative words automatically make it a `critique`. 

### My Strategic Decision Rule: Core Argument Analysis
To handle these ambiguous cases consistently, my strategy is to isolate and evaluate the **main theme or central argument** of the post using a structured layout analysis:

1. **Locate the Thesis:** I will look primarily at the first sentence/paragraph (where the author usually introduces their core view) and the final paragraph (where they often paraphrase their conclusion). 
2. **Evaluate the Contextual Core:** I will treat the middle of the post as supporting evidence. If the middle sections use analytical language to pick apart musical mechanics, the post leans toward `critique`. If the middle sections are purely an emotional vent or a declaration of fandom, it will be guided toward `criticism` or `appreciation` respectively. 
3. **The Role of Word Counting:** While counting positive and negative words can serve as a supplementary feature to gauge the tone, the final classification decision will always prioritize the structural intent of the argument over raw word counts. However, as a general rule of thumb, `appreciation` posts typically have a higher concentration of words indicating a positive, admiring, and celebratory view of the artist's past or present work, whereas `criticism` posts have a higher density of words indicating a negative, disapproving, or frustrated perspective. Meanwhile, `critique` posts feature a balanced distribution of both positive and negative remarks, focusing heavily on objective, analytical elements.

## Data Collection Plan

### Where will you collect examples?
I will manually collect public posts and comments directly from the `r/LetsTalkMusic` subreddit on Reddit. To keep close to the data and ensure strict quality control during the initial collection phase, I will copy and paste these examples into a single, master CSV file.

### How many per label?
I aim to collect a total of at least 200 examples. To ensure the dataset remains balanced and satisfies the project requirement that no single class exceeds 70% of the data, I will target an even distribution of roughly **65 to 70 examples per label** across my three categories (`appreciation`, `criticism`, and `critique`). 

### What will you do if a label is underrepresented after 200 examples?
If I reach 200 total examples and discover that a specific label (such as `criticism`) is heavily underrepresented, I will implement a targeted search strategy instead of pulling random posts. I will actively query the subreddit using specific keyword filters designed to surface the missing category:
*   **For `criticism` gaps:** Search for negative or controversial terms like *"overrated"*, *"dislike"*, *"ruined"*, *"disappointed"*, or *"worst"*.
*   **For `appreciation` gaps:** Search for highly positive terms like *"masterpiece"*, *"underrated"*, *"incredible"*, or *"favorite album"*.
*   **For `critique` gaps:** Search for analytical terms like *"structural"*, *"production analysis"*, *"discography review"*, or *"evolution"*.

I will continue harvesting these targeted posts until every single label accounts for at least 20% to 30% of the final CSV dataset before letting the notebook execute the 70% / 15% / 15% train/validation/test split.

## Evaluation metrics
### Accuracy
Accuracy is an important metric for any multi-class AI system. However, accuracy alone can be misleading. When a dataset has an uneven distribution, a model can achieve high overall accuracy simply by correctly identifying what a post is *not* (generating high True Negatives for a specific label), while consistently failing to catch actual instances of that label (resulting in low True Positives and high False Negatives). 

For example, if the system encounters a post that is not a `critique`, it might correctly identify it as "not a critique" most of the time. Because those negative instances make up a large portion of the test data, the overall accuracy score will look deceptively high—even if the model is completely blind to genuinely true `critique` posts. Therefore, relying on higher accuracy alone can misrepresent the true performance of the system. To prevent this, we must look at individual class dynamics and strive for a strong F1-score.

### F1-Score
The F1-score provides a harmonic mean that balances precision and recall:
*   **Precision:** Ensures that when the model predicts a specific label (e.g., `critique`), it is highly likely to be truly positive, preventing false alarms.
*   **Recall:** Ensures that the model misses very few actual positive cases, preventing important posts from being left out.

Therefore, for this community system to be genuinely effective and balanced, achieving a high F1-score ($\ge 0.70$) across all three labels is my primary target.

## Definition of Success

For this classification system, I am defining success based on realistic, meaningful performance thresholds tailored to the highly subjective nature of informal forum discussions on `r/LetsTalkMusic`. 

My specific target criteria for deployment in a real community tool are:
*   **Overall Accuracy:** Must be significantly higher than **0.33 (33%)**, which represents the baseline of a purely random guesser for a 3-class task. 
*   **Target F1-Score:** Achieving a per-class F1-score between **0.65 and 0.75 (65% – 75%)** across all three labels (`appreciation`, `criticism`, and `critique`). 

Given the nuanced boundaries between emotional fan expression and analytical music theory in user-generated content, reaching an F1-score within this range will serve as a robust indication of a high-performance system that is genuinely useful and ready for deployment.

## AI Tool Plan

### 1. Label Stress-Testing
To ensure my label taxonomy (`appreciation`, `criticism`, and `critique`) is robust and mutually exclusive before manually labeling 200 posts, I will prompt an LLM with my exact definitions and edge case guidelines. I will ask the model to generate 5–10 highly ambiguous music discussion posts that sit precisely on the boundaries between these labels. If the AI generates scenarios that I cannot easily and consistently classify using my current rules, I will immediately refine and tighten my definitions before starting the annotation phase.

### 2. Annotation Assistance
I have decided to handle the 200-row dataset collection completely manually without LLM pre-labeling. This ensures I stay as close to the text as possible and maintain strict, uncompromised quality control over the initial dataset. 

### 3. Failure Analysis
Once both the fine-tuned DistilBERT model and the Llama-3.3 baseline complete their evaluations on the test set, I will gather all misclassified rows. I will pass this list of wrong predictions to an LLM to perform an automated error pattern analysis. I will prompt the AI to look for systematic linguistic trends (e.g., whether the model struggles with sarcasm, short sentences, or specific musical vocabulary). I will then manually review and verify these patterns for my final evaluation report..