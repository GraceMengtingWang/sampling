# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Mengting Wang

```
Stages:
1. Event Attendance Sampling
Procedure: The code assigns individuals to two event types: "wedding" and "brunch." Specifically, there are 200 wedding attendees across two weddings (100 each) and 800 brunch attendees across 80 brunches (10 each).
Function Used: This is set up in the ppl DataFrame, where rows are labeled based on event type ("wedding" or "brunch").
Sample Size and Frame: The sampling frame here is all 1,000 individuals who attended events. This fixed distribution assigns each person to a specific event type.
Underlying Distribution: The population is divided based on event size, with weddings having more dense attendance than brunches.
Relation to Blog Post: This setup reflects the blog's example, where larger, well-defined events like weddings are easier to contact trace than smaller, dispersed gatherings like brunches.
2. Infection Sampling
Procedure: A random 10% of attendees are infected. This creates a uniform infection probability across all individuals, regardless of event type.
Function Used: The np.random.choice() function randomly selects about 100 people from the 1,000-person sample, representing a 10% infection rate.
Sample Size and Frame: The sample size is the 100 infected individuals, drawn from the full frame of 1,000 event attendees.
Underlying Distribution: This follows a binomial distribution, where each attendee independently has a 10% probability of infection.
Relation to Blog Post: The uniform infection rate models the blog's assumption that infection rates are consistent across event types, enabling an unbiased infection baseline for later tracing steps.
3. Primary Contact Tracing Sampling
Procedure: A subset of infected individuals is randomly chosen to be traced. Each infected person has a 20% chance of being successfully traced back to their event.
Function Used: The code uses np.random.rand() to generate a random value for each infected individual, then checks if it’s below TRACE_SUCCESS (0.2) to mark them as traced.
Sample Size and Frame: This tracing subset is drawn from the 100 infected individuals, with around 20 cases expected to be traced.
Underlying Distribution: This step also follows a binomial distribution, with a 20% tracing success rate applied to each infected individual.
Relation to Blog Post: This limited tracing success aligns with the blog's argument that contact tracing captures only a small, biased sample of cases, as only 20% of infections are identified, missing a large portion of cases.
4. Secondary Contact Tracing Sampling
Procedure: If two or more infections are traced to the same event, the code performs “secondary contact tracing,” where all infected individuals at that event are marked as traced.
Function Used: The code uses value_counts() on traced cases to identify events with multiple traced infections, then traces all infections at those events.
Sample Size and Frame: The sample includes all infected attendees at events with multiple primary traces, leading to complete tracing for those events. This frame includes events where infections are easier to identify due to guest list or attendance consistency.
Underlying Distribution: This is conditional sampling—secondary tracing only applies to events that already meet the threshold of two traced cases. Events with larger attendee groups, like weddings, have a higher chance of reaching this threshold and triggering secondary tracing.
Relation to Blog Post: This secondary tracing step closely mirrors the blog’s point that contact tracing tends to overrepresent structured events (like weddings) due to the ease of identifying attendees. This procedure highlights how larger events appear more frequently in traced data, creating the perception that they’re disproportionately responsible for spread.

Does the Code Reproduce the Blog Post's Findings?
No. The issue with the original code is that it doesn't assign individuals to specific events; instead, it only labels them as attending either a "wedding" or a "brunch." To accurately simulate the bias described in the article, we need to assign attendees to specific events (e.g., "Wedding 1," "Wedding 2," "Brunch 1," ..., "Brunch 80"). This way, we can perform secondary contact tracing when two or more infections are traced back to the same specific event. 

With only 1,000 iterations, the distributions are more sensitive to random fluctuations. In contrast, when the simulation was originally run with 50,000 iterations, the larger sample size resulted in a smoother and more consistent histogram. To achieve results that are more consistent and closer to those seen in the original blog post, increasing the number of iterations (e.g., to 10,000 or 50,000) would improve the stability of the histograms.

The original code labeled events generically as either "wedding" or "brunch," without distinguishing between specific instances of each type. I modified code to assign unique event IDs (e.g., "Wedding_1", "Wedding_2", "Brunch_1", ..., "Brunch_80") to differentiate between individual events. Each individual is assigned to a specific event rather than a general category. I also set a Fixed Random Seed to ensure the simulation is reproducible: 
np.random.seed(42)

This change ensures that the distributions of the proportions of infections and traced cases are identical across multiple runs; the histograms generated will always look the same, allowing for reproducible analysis and comparisons.

By modeling each wedding and brunch separately, we can accurately apply contact tracing rules to each event. This allows for secondary tracing to be triggered based on specific event instances, which is critical for reproducing the observed bias toward easily traced events like weddings.

Please refer to the file 'whitby_covod_tracing_modified.ipynb'for modified code.
```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
