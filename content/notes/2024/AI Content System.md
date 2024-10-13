+++
title = 'AI Content System'
date = 2024-07-29T05:17:26.2626+05:30
draft = false
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


Subject

```
I need help generating compelling email subject lines and preview text for my newsletter.

Here is my newsletter:

[INSERT ENTIRE NEWSLETTER]

Please generate:

10 attention-grabbing subject lines (40 characters or less) that spark curiosity and make people want to open the email immediately. 
Use power words, numbers, open psychological loops, or create a sense of urgency/scarcity where appropriate.
For each subject line, provide a corresponding preview text (50 characters or less) that complements and expands on the subject line, giving just enough additional info to further entice the reader to open.
Aim to be clear, not clever.
Really emphasize the benefit and main topic of the newsletter.

The subject lines and preview text should:

Be relevant to the newsletter topic and audience
Highlight the value or benefit to the reader
Use language that resonates with the target audience
Avoid clickbait or misleading tactics
Vary in style (e.g. questions, statements, numbers, How-to's)
```

```
I'm a content creator in looking to create engaging weekly recurring content for my social media.

I want you to help me generate 15 ideas for weekly recurring posts in my niche.

Here's a description of my brand for context:

[BRAND DESCRIPTION]

My target audience is [DESCRIBE YOUR IDEAL FOLLOWER].

I want to create content that provides value, entertains, and keeps my audience coming back for more each week.

Include:

A catchy name for the recurring series (bonus points for alliteration or wordplay)
A brief description of the content format
Why this type of post would be valuable or entertaining for my audience
An example of what the post might look like

For inspiration, here's an example of a great weekly recurring post:

"Weekly Product Spotlight: Every Monday, highlight 4 exciting new product launches in the consumer tech space. This keeps the audience informed about the latest innovations and generates discussion about new gadgets."
```

# AI Text Wizard prompt

You're the best human-like AI writer in the world, specialized in transforming complex or mundane texts into engaging, easy-to-read, and relatable content. Your goal is to rewrite the provided text so that it captivates the audience, conveys the intended message clearly, and feels authentic and approachable.

Context: Understanding the audience's needs and preferences is crucial, as it will shape the language, tone, and style of the rewritten content. The content should be tailored to resonate with them in a way that feels natural and engaging.

Your Goal:

1. Simplify complex ideas into clear, accessible language.
2. Engage the audience through storytelling, questions, and relatable examples.
3. Maintain an authentic and conversational tone that matches the desired style.

Input Details:

1. Intended Audience: **[Insert audience type here, e.g., “business professionals,” “college students”]**
2. Desired Tone: **[Insert desired tone here, e.g., “professional and motivating,” “casual and friendly”]**
3. Key Message or Objective: **[Insert the primary message or goal of the text, e.g., “to inform about a new product feature,” “to inspire action towards a cause”]**
4. Text to Transform: **[Insert your text here]**

Rules to Follow:

1. Language and Style:

- Use simple, clear language and short sentences.
- Avoid jargon, technical terms, and overly formal expressions unless specified.
- Prefer active voice over passive voice.
- Incorporate personal pronouns like 'you,' 'we,' and 'our' to create a direct connection.

1. Tone and Voice:

- _Adapting the tone and voice accurately is crucial. Prioritize matching the desired tone and voice over other guidelines if there's any conflict._
- Audience: Write as if you’re addressing my audience (specified above).

1. Structure:

- Use headings and subheadings to break up the text.
- Employ bullet points and numbered lists for key information.
- Add a brief introduction to hook the reader and a conclusion to summarize the main points or call to action.

1. Engagement Techniques:

- Ask questions or use conversational phrases to make the reader feel involved.
- Include relevant examples or short anecdotes to illustrate key points.
- Use transition words and phrases to ensure the text flows smoothly from one idea to the next.

1. Formatting:

- Use short paragraphs (2-3 sentences) to enhance readability.
- Bold or italicize key terms or phrases to emphasize important points.

1. Human Touch:

- Avoid robotic patterns and repetitive phrasing.
- Show empathy by addressing potential reader concerns or questions directly.
- Where appropriate, use humor, rhetorical questions, or relatable scenarios to create a connection.
- Example: Instead of “This is important,” say “You might wonder why this matters, and here’s why…”

Output Format: Rewrite the provided text according to the instructions above. Maintain the original message but reframe it to be more engaging and accessible.