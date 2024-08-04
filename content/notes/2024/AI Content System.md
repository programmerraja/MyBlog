+++
title = 'AI Content System'
date = 2024-07-29T05:17:26.2626+05:30
draft = true
tags =[]
+++ 

### Ikigai method

```txt
Your goal is to help the user identify their core content pillars for posting on social media. Core content pillars are the key topics or areas that the user will consistently create content about to build their brand and engage their audience. To determine the user’s core content pillars, gather information about their passions, strengths, and relevant market trends (Ikigai method) from the provided inputs:

​  
<Ikigai>  
1. What topics do you find yourself constantly thinking about or eager to discuss with others?
	tech and programing, devops, microservcices,hacking etc
2. What activities or hobbies do you lose track of time while doing?
	Learning about tech like ai,devops,hacking etc
3. If you could spend a year learning about anything, what would it be? 
    Same as above
4. What skills or abilities have others frequently complimented you on?
    Same as above
5. What comes easily or naturally to you that others often struggle with?  (Same as above)
6. In what areas do you tend to outperform your peers or colleagues? (Same as above)
7. What challenges or pain points do you see many people struggling with? (Same as above)
8. What topics or issues seem to be getting a lot of attention or engagement on social media lately? (Same as above)
9. What emerging technologies, tools, or platforms seem promising?(Same as above)

</Ikigai>  
​  
<analysis>

Analyze the information to identify common threads and connections between the user's passions, strengths, and market trends. Write a detailed analysis, considering the following points:

​

- Which passions align with the user's strengths, and how can they be leveraged to create unique and engaging content?

- What market trends or audience pain points relate to the user's areas of expertise, and how can they address these in their content?

- Are there any recurring themes or topics that the user seems particularly knowledgeable or enthusiastic about?

- How can the user's background and experiences contribute to a distinctive perspective or approach to their content?

​

Summarize your key findings and insights, and use them to inform the content pillar suggestions in the next step.

</analysis>  
​  
<core_content_pillars>

Based on your analysis, suggest 5 potential topics for each of the 3 core content pillars:

​

Main Topic (The main, niched down topic the user will be known for. It should be appropriately niched down for a specific target audience and allow the user to demonstrate their expertise and authority in a topic (e.g. copywriting for busy founders, prompting for AI beginners, graphic design for tech startups)):

- [Topic] for [Target audience]

​

Broad Topic (This is a broader topic that speaks to a larger audience and also gives the user a bit more flexibility with their content strategy (e.g. productivity, mindset, entrepreneurship)):

- [Topic]

​

Personal Topic (A deeply personal topic, highlighting unique perspectives and background which are important to create trust and relatability with a personal brand (e.g. being a dad, lessons learned)):

- [Topic]

</core_content_pillars>

​

Structure your response thoughtfully with line breaks for readability. Do not output XML tags, just use headers with markdown.
```


### Content Ideas Factory

```txt
You will be generating a set of 20 subtopics related to a given topic, tailored for a specific target audience.

Topic: [Enter your topic]
Audience: [Enter your audience]

The subtopics should be outcome-oriented and cover various angles that the audience might be thinking about or areas where they would likely need help from an expert on the topic.

Avoid generic or overly broad subtopics.

Don't frame the subtopic using any specific content format, such as: Tips, Tricks, Steps, Hacks, Advice, Examples, Tools/Resources, Frameworks, Guides, Case study, Trends, Reasons, Common Mistakes, Observations, Comparisons, Questions, Lessons, Stories, Fears, Failures
```

### Content ideas

```txt
You will be generating content ideas for various categories based on a given topic and a target audience.

Topic: [Enter your topic] # topic from prev answer
Audience: [Enter your audience]

Each content idea should include a number (either 3, 5, or 7) and be phrased as an engaging headline or title. For example:

- Tips & Tricks: 5 Simple Tips to Craft Crystal-Clear Prompts for AI

- Hacks: 7 Clever Hacks to Increase Your Productivity At Work

- Mistakes: 3 Common Mistakes Beginners Make When Trying To Lose Weight


To generate the ideas, follow these steps:  
1. First, analyze the given topic or question and consider the target audience.

2. Then, brainstorm content ideas for each category that would be relevant, interesting, and valuable to the target audience. Ensure the ideas are specific, actionable, and engaging.

3. Output the ideas in the following formats (1 line with the content idea for each format): Tips, Tricks, Steps, Hacks, Advice, Examples, Tools/Resources, Frameworks, Guides, Case study, Trends, Reasons, Common Mistakes, Observations, Comparisons, Questions, Lessons, Stories, Fears, Failures
```


 create 2 posts per day, in a week that comes out as:

- 11 short-form posts
- 3 long-form posts

But this is just a guideline. Ultimately you need to find the balance that works well for you.

### Prompt to generate content like human

```txt
I will provide you a TEMPLATE and an INPUT below. Your job is to write a new text by filling in the variables in the TEMPLATE using the INPUT context. You should aim to follow the TEMPLATE, but you can make minor adaptations to the template if it makes sense for the context.

This is EXTREMELY important: Every output must be less than 280 characters.

This is also important:

-Use short punchy sentences.

-Refrain from puffery writing at all cost.

-Avoid salesy words like “game-changing, unlock, master, skyrocket, revolutionize, etc”.


<TEMPLATE>

{Input the template here}

</TEMPLATE>

​  
<INPUT>

{Input the topic here}

</INPUT>
```

Template
```txt
Do [Something]

[Explanation]

- [Example or Action 1]
- [Example or Action 2]
- [Example or Action 3]
- [Example or Action 4]
- [Example or Action 5]
- [Example or Action 6] 
- [Example or Action 7]

Simple playbook to [achieve desired outcome or state of being].
```


TWITTER 
```txt
I will provide you a TWITTER TEMPLATE and an INPUT below.
Your job is to write a new text by filling in the variables in the TWITTER TEMPLATE using the INPUT context.
You should aim to follow the TWITTER TEMPLATE, but you can make minor adaptations to the template if it makes sense for the context.

This is very important: 
-Every tweet must be less than 280 characters.
-Refrain from puffery writing at all cost. 
-Avoid salesy words like "game-changing, unlock, master, skyrocket, revolutionize, etc".

<TWITTER TEMPLATE>

{Input the template here}

</TWITTER TEMPLATE>

<INPUT> 

{Input the topic here}

</INPUT> 
```

CONTENT TEMPLATE
```txt
--Hook--

[Doing something] is life-changing.

[Main benefit of doing something].

Here's exactly how you [do something] in 5 simple step:

--Tweet 1-5 can be either one of the following 3 alternatives--
--Alternative 1--

[Step X Headline]:

[Description of the step's primary action or focus]

[Additional context or details about implementing the step]

[Key takeaway or lesson learned from this step]

--Alternative 2--

[Step X Headline]:

[Description of the step's primary action or focus]

[Some examples that illustrate the action]

- [Example 1]
- [Example 2]
- [Example 3]

[Conclude with a concise, impactful statement that reinforces the step's importance.]

--Alternative 3--

[Step X Headline]:

[Description of the step's primary action or focus]

1. [Step 1 to do the action]
2. [Step 2 to do the action]
3. [Step 3 to do the action]

--Tweet 6--

TL;DR:

1: [Step 1 Title]
2: [Step 2 Title]
3: [Step 3 Title]
4: [Step 4 Title]
5: [Step 5 Title]
```


https://moritzkremb.notion.site/The-AI-Content-System-Worksheet-71c34857d7374cec9f7a9ac65e99547c