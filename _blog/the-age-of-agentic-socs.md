---
title: The Age of Agentic SOCs
date: 2026-04-17 13:00:00 -0400
image: /assets/images/thumbnails/agentic_workflow.png
---

A few days ago I had major realization on the way AI will impact future SOC workflows.

I'm now nearly certain that the future SOC will be as close to fully automated as technically possible with human analysts primarily proofreading and approving AI generated findings and remediations.

And might dismiss me as another "AI bro", but this mindset is completely new to me. I genuinely didn't believe a fully automated SOC was feasible until earlier this week. Now I not only believe it's possible, I believe it will arrive much faster than most people expect.

In this post, I'll walk you through the exact problem that flipped my perspective.

And it starts with a scenario that's trivial for any human analyst to solve but extremely difficult to handle programmatically.

## Problems easy for humans but hard for code

Take this domain as an example:

`g0ogle[.]com`

To an analyst, and hopefully most people, they would look at this domain and immediately be able to tell that something is wrong with it.

This is a typosquatted version of `google.com` where one of the letter "o"s has been replaced with a zero.

If this domain appeared in an alert dashboard, a SOC analyst would instantly recognize it as suspicious and flag it.

Now imagine writing a program that can automatically detect domains like this at scale. You would need to:

- Generate `every possible` misspelling and homoglyph variation of "google"
- Do the same for `every`*` legitimate domain your organization cares about
- Reliably distinguish malicious variants from legitimate ones

This is a genuinely hard problem!

Existing tools called **domain permutation engines** attempt to solve this by generating common misspellings. However, building one from scratch requires deep expertise, significant tuning, constant maintenance and if done incorrectly it could cause lots of false positives and false negatives.

## Programmatically using AI to analyze for you

So, instead of writing hundreds of lines of brittle code, try this.

Go onto your preffered generative AI website and simply give it this prompt:

> i found this domain embedded inside an email:
> g0ogle[.]com 
> is this legitimate, or should i flag it as suspicious?

Its extremely likely that your model said something on the lines of "this is not legitimate".

And if the model you use allows you to view its "chain of reasoning" it might of replied with a very similar sounding thought process a human might have. 

My model's "reasoning" was:

> "The domain g0ogle[.]com is a classic typosquatting/phishing domain. It replaces 'o' with '0' (zero) to mimic google.com."

This is pretty impressive, and you can probably give your model any misspelling or variation of google.com and the vast majority of times it will return to you with the correct answer.

So, how can we do this programmatically?

Well most frontier models expose an API, where if you pass it a payload containing a prompt, you'll get the model's response back.

A simple Python implementation of this may look like:

```python
import requests

def model_a(prompt):
    response = requests.post(
        AI_API,
        json={'prompt': prompt},
    )

    return response.json()

model_a_response = model_a('''
i found this domain embedded inside an email:
g0ogle.com 
is this legitimate? or should i flag it as suspicious?''')['response']
```

Where we have a simple function `model_a` that takes a `str` input and returns the JSON response from our model.

Now instead of feeding it one strict prompt with a domain "hardcoded", we can use an `f-string` and a for loop to iteratively prompt the model over a sequence of domains.

```python
domains = [
    "g00gle.com",
    "google.com",
    "paypa1.com",
    "amazon.com",
    "rnicrosoft.com"
]

for domain in domains:
    model_a_response = model_a(f'''
    i found this domain embedded inside an email:
    {domain}
    is this legitimate? or should i flag it as suspicious?''')['response']

    print (model_a_response)
```

There is a problem with this though. How do we know what the model's response is? Obviously we don't want to manually read the response for each item. Instead, we will do some "prompt engineering" to ensure that the model always responds with a boolean value of `True = suspicious` or `False = legit`.

```python
model_results = []

for domain in domains:
    prompt = f'''
        i found this domain embedded inside an email:
        {domain}
        is this domain suspicious or legit.
        only respond with 'true' if suspicious or 'false' if legit, do not include any other text
        '''
    
    response = None
    while True:
        
        if model_a(prompt)['response'] == "true":
            response = True
            break
        elif model_a(prompt)['response'] == "false":
            response = False
            break
        else:
            # some more clearer direction if it fails the first time
            prompt += "\n\nIMPORTANT: you MUST respond with ONLY the word 'true' or 'false'. Nothing else."
        
    model_results.append(response)
```

From this we will have two lists of equal length, one containing the domains we wanted to check and another containing the model's responses indicating whether the corresponding domain is suspicious or not. Since the two lists are aligned by their indices, we can combine them into a dictionary or table using the `pandas.DataFrame` object.

```python
import pandas as pd

models_response = pd.DataFrame({'domain': domains, 'is_suspicious': model_results})
```

When displaying the DataFrame I got the following table as an output.

| domain           | is_suspicious |
|------------------|---------------|
| g00gle.com       | True          |
| google.com       | False         |
| paypa1.com       | True          |
| amazon.com       | False         |
| rnicrosoft.com   | True          |

Now, this particular implementation can obviously have some performance issues when deployed at scale. Even so, we just solved a problem that, if done traditionally, would require hundreds of lines of code and several hours of work to achieve the same level of accuracy as the script we just made in five minutes.

Also, the possiblities of model based detections like this are vast.

Like examining the command-line output of a running process to determine if it was malicious, analyzing executed PowerShell scripts, understanding the semantics of email bodies to detect social engineering attempts, suppressing false-positive alerts, and more.

So why does the majority of the industry not use these methods?

I think there are two general reasons:

- **First**, not a lot of people in the industry are exposed to AI use cases outside of the typical chat bot models most people use now a days. This is mainly because of just how new AI is and especially how new Agentic AI is so not a lot of people have fully grasped what AI is capable of.

- **Second**, using LLMs at scale like this is expensive, at least if you're using any of the frontier models. It may be worth setting up your own locally hosted open source model, where you would not be constrained by API rate limits and costs.

The only trade off with doing this is that the model you'll be using will be "less smart." One way to mitigate this is by running multiple open source models and forming a consensus based on their votes for a particular prompt. For example, suppose you have three different open source models, each asked the same question in the same way. If the majority agree on the same outcome, you take that outcome as the final opinion. In the example provided in this blog, we would ask each locally hosted model the same question: "Given this domain {domain}, is it malicious? Respond only with yes or no." Each model would then cast its vote, and we would take the majority's opinion as the final result.