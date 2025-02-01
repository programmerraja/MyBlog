

- [ ] Best practice that i follow on node and react
- [ ] A series of blog how i automate the boring task 
	- [ ] interview helper
- [ ] Folder exposing
- [ ] Weekend learning 
- [x] Leason learned from 2 year dev
	- [x] Final advice no push on friday
- [ ] My Bashrc setup 
- [ ] List of questions need to ask before deisgn some thing
	- [ ] Can i use any design pattern
	- [ ] Alwasys create a wrapper when using thirdparty such that it easy to swap
- [ ]  3 Lessons from the Smartest Developers I’ve Worked With
- [x] Exposed phone number on leaderboard
- [ ] What we learns from migrating heroku to kuberneted
	- [ ] Probe
	- [ ] Staing
- [ ] The question need to answer before design a system
	- [ ] Rollback 
	- [ ] disaster recovery (what if down how you handle)
	- [ ] performance and optimization (not required but need to check current load will happens)
- [ ] The thing wont tell by anyone when i am studying college
	- [ ] FInance 
	- [ ] tax
	- [ ] Leadership 
	- [ ] communication 

- How to integrate 11labs with twilio
- How db.admin() make our db downtime for a 30 min
# Mistakes engineers make in large established codebases


Some code bad smell that i see
- const SALESFORCE_SANDBOX_URL = process.env.SALESFORCE_SANDBOX_URL;
- console log (how i can optimize) lot of logs
- logs without any indent
- Nested funtion defining at top
- unwanted try catch
- using constant that need only one place
- default false in schema
- harded mail template and replace which is not efficent
- multiple state in frontend
- using destruct the object from front end in backend query
- promise.all
- Not adding comments for specfic thing
- usecallback and use memo
- Doing atomic operation in code level

Sherlock Holmes
- [x] Econnreset
- [ ] HOw nodejs update 18 restart the pod unhabled rejection
- [ ] Links server slow nginx multiple ssl
- [x] debug ecconreset 15sec to update
- [x] How we fix dns error in pod
- [ ] hOW I SPEED UP OUR CI (pull shell code)
- [ ] How invalod elho
	- [ ] 550 Requested action not taken: Invalid HeloHost.(#5.5.0)","responseCode":550,"command":"MAIL FROM

Code aganist clock
- [ ] interview helper 
How to 
- [ ] How to use wireshark
- [ ] How to use zap scan
- [ ] How to use lets encrypt
- [ ] how to stream mongodb request 
- [ ] Post about klenty dns
- [ ] Rabbitmq load test
- [ ] hOW I

Learn from mistakes or from expereinces
- [ ]  Migration to 
- [ ] How Our devops accedntly deelted the production kube cluster
- [ ] dbhash run cmd block total db by our db developer

Unsolved mysteries
- [ ] Vault timeout 
- [ ] Meilisearch issue `Permission denied (os error 13)`

System case study
- [ ] Vault migration
- [ ] Links migration
- [ ] 
A series for internal wokring
- [ ] Mongodb

How i solve X problem
- [ ] Pod to pod communication

Generative AI series of what i know
- [ ] https://ullyer.medium.com/outlines-make-llm-structured-outputs-controllable-and-improve-the-stability-of-llm-applications-584ae9db3789 
- [ ] How to get structure output
- [ ] EVAl and Observation


# Create blog of my second brain setup and github tempalte to get started

## Buy me coffe copy text


> _If you liked the above story, you can_ [**_buy me a coffee_**](https://buymeacoffee.com/programmerraja) _to keep me energized for writing stories like this for you and to support me because as of writing this story, I’m not eligible for the Medium Partner Program._


> If you enjoyed this digital detective adventure and want to support more investigations like this, consider [**_buy me a coffee_**](https://buymeacoffee.com/programmerraja) . Your support fuels my curiosity and helps me keep digging into the unseen threats!


>If you found these notes on "A Philosophy of Software Design" insightful and want to support more content like this, consider [**_buying me a coffee_**](https://buymeacoffee.com/programmerraja). Your support fuels my passion for exploring and sharing software design principles, helping us all become better developers! 

### Enjoyed This Story?

_[Subscribe for free](https://medium.com/subscribe/@programmerraja) to get notified when I publish a new story._

_Find me on_ [LinkedIn](https://www.linkedin.com/in/programmerraja/), [Twitter](https://twitter.com/programmerraja)_, and_ github_!_

https://giscus.app/ -> to add comments feature

**Finally, if the article was helpful, please clap 👏and follow, thank you!**

### Intro for sharing notes
I decided to have a poke around and see if I could figure out how the HTTP streaming APIs from the various hosted LLM providers actually worked. Here are my notes so far.

## Prompts

```
Write a thorough yet concise summary of [BOOK TITLE] by [AUTHOR].

Concentrate on only the most important takeaways and primary points from the book that together will give me a solid overview and understanding of the book and its topic.

Include all of the following in your summary:

- 3 of the best Quotes from this Book that change the way we think
- Main topic or theme of the book
- Why should someone read this book (Be specific in this Heading)
- 7–10 Key ideas or arguments presented
- Chapter titles or main sections of the book
- Key takeaways or conclusions
- Any Techniques or special processes told by the author in the book
- Author's background and qualifications
- Comparison to other books on the same subject
- 5–7 Target audience groups or intended readership
- Reception or critical response to the book
- Recommendations [Other similar books on the same topic] in detail
- To sum up: The book's biggest Takeaway and point in a singular sentence.
```

```
Act as a professional writing assistant. I will provide you with text and you will do the following:

1. Check the text for any spelling, grammatical, and punctuation errors and correct them.
2. Check for any grammatical errors and correct them
3. Remove any unnecessary words or phrases to improve the conciseness of the text
4. Provide an analysis of the tone of the text. Include this analysis beneath the corrected version of the input text. Make a thorough and comprehensive analysis of the tone.
5. Re-write any sentences you deem to be hard to read or poorly written to improve clarity and make them sound better.
6. Assess the word choice and find better or more compelling/suitable alternatives to overused, cliche or weak word choices
7. Replace weak word choices with stronger and more sophisticated vocabulary.
8. Replace words that are repeated too often with other suitable alternatives.
9. Rewrite or remove any sentences, words or phrases that are redundant or repetitive.
10. Rewrite any poorly structured work in a well-structured manner
11. Ensure that the text does not waffle or ramble pointlessly. If it does, remove or correct it to be more concise and straight to the point. The text should get to the point and avoid fluff.
12. Remove or replace any filler words
13. Ensure the text flows smoothly and is very fluent, rewrite it if it does not.
14. Use varying sentence lengths.
15. Have a final read over the text and ensure everything sounds good and meets the above requirements. Change anything that doesn't sound good and make sure to be very critical even with the slightest errors. The final product should be the best possible version you can come up with. It should be very pleasing to read and give the impression that someone very well-educated wrote it. Ensure that during the editing process, you make as little change as possible to the tone of the original text input.
```


```
Take the following piece of writing that feels disconnected or choppy and improve its flow by applying the following techniques: contrast, reasons, exceptions, examples, and details. and write a blog like human

**Instructions:**

1. **Contrast**: Identify places where contrasting ideas can help clarify your point.
2. **Reasons**: Connect statements by adding reasons that explain why something happened or why it's important.
3. **Exceptions**: Anticipate possible counterpoints and add exceptions to create a more nuanced argument.
4. **Examples**: Use concrete examples to clarify abstract ideas.
5. **Details**: Add additional details or elaboration where necessary to ensure the writing feels complete but not overwhelming.

**Text**:  

```

Summary
```
“Summarize the key points from the provided content, whether it’s a video, blog post, or article. Include details about the main topics and themes discussed. Summarize the key insights and findings, highlighting any significant advantages, limitations, or implications. Include any practical recommendations or advice for applying the discussed concepts or methods. Add any notable quotes that capture key points or significant perspectives. What is the one-sentence takeaway or central message from the content? List any references, tools, or additional resources mentioned that support the discussion.”
```

For rough notes to blog

```
You was tech blogger who have expereinced in writing the blog in techy way and interactive funny way to attract all type of audience today i wll share by rough work of the blog help me to enhance the blog with proper title and try add more humor Answer a question given in a natural, human-like manner
```


Write a blog like this

https://funkbytetech.substack.com/p/i-fought-a-ddos-and-lived-to-tell?ref=dailydev


## Prompt for blog writting 

```

**(C) Context:** In today’s tech-driven world, many readers crave accessible content that simplifies complex ideas, making technology enjoyable and relatable.

**(O) Objective:** Write an engaging tech blog post that explains a current trend or innovation in simple terms, captivating and encouraging readers to explore further.

**(S) Style:** Use a friendly, conversational style akin to a popular tech influencer, employing relatable analogies and anecdotes for clarity.

**(T) Tone:** Keep the tone light-hearted and enthusiastic, incorporating humor to enhance the reading experience.

**(A) Audience:** Target tech enthusiasts and casual readers with limited knowledge, ensuring the content resonates with beginners and offers insights for seasoned readers.

**(R) Response:** Structure the post with:
1. **Introduction** – Introduce the topic and its relevance.
2. **Main Content** – Explain the trend using simple language and engaging examples.
3. **Visuals** – Suggest where to include images or infographics.
4. **Conclusion** – Summarize key points and encourage reader engagement.

```

```

**(C) Context:** In the ever-evolving world of technology, many readers seek accessible content that simplifies complex concepts. The goal is to create a tech blog that breaks down intricate topics into relatable and enjoyable articles, making technology approachable for everyone.

**(O) Objective:** Write an engaging tech blog post that explains a current tech trend or innovation in simple terms. The post should captivate readers, spark their curiosity, and encourage them to explore further.

**(S) Style:** Adopt a friendly and conversational writing style, similar to that of a popular tech influencer. Use relatable analogies and anecdotes to illustrate points, while maintaining clarity and precision in explanations.

**(T) Tone:** Keep the tone light-hearted and enthusiastic. Infuse humor where appropriate to create an enjoyable reading experience, while still conveying valuable information.

**(A) Audience:** The intended audience includes tech enthusiasts, casual readers with limited technical knowledge, and anyone curious about technology. The writing should resonate with beginners and provide insights that even seasoned readers can appreciate.

**(R) Response:** Format the blog post as a structured article with the following sections:
1. **Introduction** – Briefly introduce the topic and its relevance.
2. **Main Content** – Explain the tech trend or innovation using simple language, relatable examples, and engaging anecdotes.
3. **Visuals** – Suggest where to include images, infographics, or videos to enhance understanding.
4. **Conclusion** – Summarize key points and encourage readers to explore the topic further or share their thoughts.

```

```
> # CONTEXT # I want to advertise my company's new product. My company's name is Alpha and the product is called Beta, which is a new ultra-fast hairdryer.

> # OBJECTIVE # Create a Facebook post for me, which aims to get people to click on the product link to purchase it.

> # STYLE # Follow the writing style of successful companies that advertise similar products, such as Dyson.

> # TONE # Persuasive

> # AUDIENCE # My company's audience profile on Facebook is typically the older generation. Tailor your post to target what this audience typically looks out for in hair products.

> # RESPONSE # The Facebook post, kept concise yet impactful.
```

### COpy pasting
```
Please analyze the following piece of content in detail:

"[INSERT CONTENT]"

What key points, arguments, or themes are being explored? Summarize them.

How is the content structured? Break down the key sections or components.

What is the tone, style, and voice of the writing? How would you characterize it?

What makes this piece compelling or engaging to the reader?

What specific writing techniques (storytelling, humor, data, etc.) are employed?

How could the core ideas be remixed or built upon to create a new, distinct piece of content? Brainstorm 3 high-level ideas.

Based on your analysis, please create an outline for each of the 3 new pieces of content inspired by the original piece, but with a fresh angle or application of the core ideas.

The ideas should be primarily inspired by the structure, hook, and idea explored.

The outlines should include:

A working title
3-5 key points or arguments to explore
Examples or stories to illustrate the points
A unique insight, perspective, or opinion on the topic

The new piece should be clearly distinct from the original (no copying), but riff on the same themes.
```

### COntent creation
```
I want to create a 4-week content strategy for my [NICHE] business that incorporates the 4 key content pillars: Educational, Entertaining, Thought-Provoking, and Empathetic.

Here's examples of Educational:

- Step-by-step tutorials
- Quick tips and hacks
- Myth-busting threads
- Industry insights and analysis

Here's examples of Entertaining:

- Witty observations and hot takes
- Behind-the-scenes glimpses
- Inspiring stories and anecdotes
- Controversial opinions that spark discussion

Here's examples of Thought-Provoking:

- Philosophical musings and reflections
- Contrarian opinions and devil's advocate stances
- "Big picture" industry predictions
- Nuanced deep-dives into complex topics

Here's examples of Empathetic:

- Relatable memes and "it me" moments
- Vulnerable stories of struggle and triumph
- "You're not alone" messages of support
- Uniquely personal insights into your audience's world

My target audience is: [DESCRIBE YOUR IDEAL CUSTOMER]
For each week, please generate 4 content ideas (1 from each pillar) that would resonate with my audience.
For each idea, include:

The content pillar it belongs to
The key message or takeaway
The format (text post, image, video, etc.)
A catchy headline or hook
A brief outline of the content (2-3 bullet points)

Ensure the ideas are highly relevant to my niche and address my target audience's specific pain points, desires, and interests.

Aim for a diverse mix of formats and angles to keep the content fresh and engaging throughout the month.
```

### Hook Prompt
```
I need your help to make my business more addictive and habit forming for my audience and customers.

Here's context about my business:

[INSERT CONTEXT]

Based on the context provided, please create a step-by-step plan for how I can implement Nir Eyal's "Hooked" model to make my product/content more habit-forming and addictive for my audience.

Here's Eyal's Hooked Model:

1. Trigger

A trigger is the actuator of behavior — the spark plug in the engine. Triggers come in two types: external and internal. Habit-forming products start by alerting users with external triggers like an email, a website link, or the app icon on a phone.

For example, suppose Barbra, a young woman in Pennsylvania, happens to see a photo in her Facebook newsfeed taken by a family member from a rural part of the state. It’s a lovely picture and since she is planning a trip there with her brother Johnny, the external trigger’s call-to-action intrigues her and she clicks. By cycling through successive hooks, users begin to form associations with internal triggers, which attach to existing behaviors and emotions.

When users start to automatically cue their next behavior, the new habit becomes part of their everyday routine. Over time, Barbra associates Facebook with her need for social connection.

2. Action

Following the trigger comes the action: the behavior done in anticipation of a reward. The simple action of clicking on the interesting picture in her newsfeed takes Barbra to a website called Pinterest, a “pinboard-style photo-sharing” site.

This phase of the hook, as described in chapter three, draws upon the art and science of usability design to reveal how products drive specific user actions. Companies leverage two basic pulleys of human behavior to increase the likelihood of an action occurring: the ease of performing an action and the psychological motivation to do it.

Once Barbra completes the simple action of clicking on the photo, she is dazzled by what she sees next.

3. Variable Reward

What distinguishes the Hook Model from a plain vanilla feedback loop is the hook’s ability to create a craving. Feedback loops are all around us, but predictable ones don’t create desire. The unsurprising response of your fridge light turning on when you open the door doesn’t drive you to keep opening it again and again. However, add some variability to the mix — say a different treat magically appears in your fridge every time you open it — and voila, intrigue is created.

Variable rewards are one of the most powerful tools companies implement to hook users; chapter four explains them in further detail. Research shows that levels of the neurotransmitter dopamine surge when the brain is expecting a reward. Introducing variability multiplies the effect, creating a focused state, which suppresses the areas of the brain associated with judgment and reason while activating the parts associated with wanting and desire. Although classic examples include slot machines and lotteries, variable rewards are prevalent in many other habit-forming products.

When Barbra lands on Pinterest, not only does she see the image she intended to find, but she is also served a multitude of other glittering objects. The images are related to what she is generally interested in — namely things to see on her upcoming trip to rural Pennsylvania — but there are other things that catch her eye as well. The exciting juxtaposition of relevant and irrelevant, tantalizing and plain, beautiful and common, sets her brain’s dopamine system aflutter with the promise of reward. Now she’s spending more time on Pinterest, hunting for the next wonderful thing to find. Before she knows it, she’s spent 45 minutes scrolling.

4. Investment

The last phase of the Hook Model is where the user does a bit of work. The investment phase increases the odds that the user will make another pass through the hook cycle in the future. The investment occurs when the user puts something into the product or service such as time, data, effort, social capital, or money.

However, the investment phase isn’t about users opening up their wallets and moving on with their day. Rather, the investment implies an action that improves the service for the next go-around. Inviting friends, stating preferences , building virtual assets, and learning to use new features are all investments users make to improve their experience. These commitments can be leveraged to make the trigger more engaging, the action easier, and the reward more exciting with every pass through the hook cycle. …

As Barbra enjoys endlessly scrolling through the Pinterest cornucopia, she builds a desire to keep the things that delight her. By collecting items, she’ll be giving the site data about her preferences. Soon she will follow, pin, re-pin, and make other investments, which serve to increase her ties to the site and prime her for future loops through the hook.

For each of the four phases (Trigger, Action, Variable Reward, Investment), include:

A clear definition of the phase and its importance in the "Hooked" cycle
2-3 specific, actionable strategies for implementing that phase in my business, based on my unique context
An explanation of the psychological principles at play and why this phase is so effective in creating addiction

Provide your suggestions in a clear, numbered list format. Offer tangible, real-world examples wherever possible to illustrate the concepts.

The plan should be written in an engaging, conversational tone that speaks directly to me as a solopreneur looking to boost my audience retention and build my authority.

Use persuasive language and strong analogies to drive home the importance of each phase and inspire me to take action.
```


Viral content prompt
```
I want you to generate viral content ideas for my content business.

Here's some context about my brand:

[INSERT BRIEF CONTEXT]

Please help me brainstorm 5 content ideas that incorporate all 6 elements of the STEPPS model:

Social Currency: How can I make my audience feel like insiders?
Triggers: What environmental cues could I tie my content to?
Emotion: What strong emotions could my content evoke?
Public: How can I make my idea more visible and shareable?
Practical Value: What useful information can I provide?
Stories: What compelling narrative could I use to convey my message?

The output should be like this:

Explain each idea about what the post will be about in a full sentence.

Then underneath, explain in full sentences how it hits each STEPPS element.

Avoid using hashtags or exclamation points in the content ideas.
```

###   Readability Refiner GPT

```
You are The Readability Refiner GPT, an AI assistant specialized in enhancing the clarity and readability of written content. Your expertise lies in simplifying complex text, shortening lengthy sentences, and improving flow, while maintaining the original tone and style. You focus on making the content more accessible and engaging for the intended audience.

Goal:
Your task is to rewrite the text below to improve its readability, making it suitable for the target audience. You will simplify language where possible, break down long sentences, keep paragraphs concise, and use effective transitions. Your goal is to enhance clarity without altering the original meaning or tone.

Readability Enhancement Structure:
1. Audience Understanding: Consider the specific needs and reading level of the target audience.
2. Simplification: Replace complex words with shorter, simpler synonyms.
3. Sentence Structure: Break down long, convoluted sentences into shorter, more digestible ones.
4. Paragraph Optimization: Keep paragraphs brief and focused on one idea at a time.
5. Transition Improvement: Use clear and effective transitions to maintain flow and coherence.
6. Tone Preservation: Ensure the rewritten text retains the original tone and style.

Questions to Ask:
- Who is the intended audience for this text, and what is their reading level?
- Are there any specific guidelines or preferences for tone and style that I should follow?
- What key points or ideas should be emphasized in the text?
- Are there any sections that are particularly challenging or complex that require extra attention?
- How much flexibility do I have in rephrasing to improve readability?

Criteria for Effective Readability Improvement:
- The text should be easy to read and understand for the target audience.
- Simplified language without losing the original meaning or nuances.
- Sentences and paragraphs should be concise and clear.
Transitions should be smooth, enhancing the overall flow.
- The tone and style should remain consistent with the original content.

Information About Me:
- Audience description:[INSERT CONTEXT]
- Original tone:[INSERT CONTEXT]
- Key points:[INSERT CONTEXT]
- Specific challenges:[INSERT CONTEXT]
```
## Prompt for creating book

1. _**Introduction (about 500 words long)**_

- _**Hook**__: Start with an engaging story, anecdote, or interesting fact._
- _**Context**__: Provide context for why this story or fact is relevant to the chapter's main theme._
- _**Thesis Statement**__: Clearly state the main point or rule that the chapter will cover._

_**2. Background Information (about 500 words long)**_

- _**Historical/Social Context**__: Explain the background related to the chapter's theme. This might include scientific explanations, historical context, or social implications._

_**3. Main Arguments (about 1000 words long)**_

- _**Argument 1**__: Introduce the first major argument or point._
- _**Explanation**__: Elaborate on the point with details and examples._
- _**Evidence**__: Provide supporting evidence, such as studies, quotes, or case studies._
- _**Argument 2**__: Introduce the second major argument or point._
- _**Explanation**__: Elaborate on the point with details and examples._
- _**Evidence**__: Provide supporting evidence, such as studies, quotes, or case studies._
- _**Argument 3**__: Introduce the third major argument or point._
- _**Explanation**__: Elaborate on the point with details and examples._
- _**Evidence**__: Provide supporting evidence, such as studies, quotes, or case studies._

_**4. Practical Advice (about 1000 words long)**_

- _**Guidance**__: Offer practical advice or steps that the reader can take to apply the chapter's main point in their own life._
- _**Examples**__: Include real-life examples or scenarios where the advice has been successfully applied._

_**5. Conclusion (about 300 words long)**_

- _**Summary**__: Summarize the key points discussed in the chapter._
- _**Final Thoughts**__: Provide a closing thought or call to action that reinforces the chapter's theme._
- _**Transition**__: (If applicable), provide a hint or transition to the next chapter._




## Prompt for summary using COSTAR framework

**Context**: I have a research paper that I would like summarized. The paper is on a technical topic and contains various findings, methods, and conclusions. I need the summary to focus on the main ideas without losing important details.

**Objective**: Summarize the research paper, focusing on the key findings, methods used, and conclusions drawn. Please make sure to include any relevant statistics or key terms that were pivotal in the research.

**Style**: Write in a clear, concise style similar to that used by an academic researcher when explaining findings to non-experts.

**Tone**: Use a neutral, informative, and professional tone, suitable for readers who are interested but not deeply familiar with the topic.

**Audience**: The summary is for a general audience of professionals and students who are not experts in the field but are interested in the subject matter.

**Response**: Provide the summary in a structured paragraph format, clearly separated into sections for methods, findings, and conclusions. If necessary, include short bullet points to emphasize important details.


## 1 3 1 technique

1. Start with one declarative sentence
2. Follow it with three sentences that explain or build on the idea.
3. End with another declarative sentence that wraps it up neatly.

Prompt
```


<TEMPLATE>
# Subhead
[1 declarative sentence.]
2 line feeds.
[3 sentences of explanation.]
2 line feeds.
[1 declarative sentence.]
</TEMPLATE>




## Notes for blog content

Performing attacks possibly on our honeypot network. (Email Spam, Automated Signups, Comment and Forum spamming, Login Attempts).

```


## Notes

```
{
    "status": "ok",
    "178.20.45.182": {
        "asn": "AS48282",
        "range": "178.20.45.0/23",
        "hostname": "host-178-20-45-182.hosted-by-vdsina.ru",
        "provider": "Hosting technology LTD",
        "organisation": "Hosting technology LTD",
        "continent": "Europe",
        "continentcode": "EU",
        "country": "Russia",
        "isocode": "RU",
        "region": "Moscow",
        "regioncode": "MOW",
        "timezone": "Europe/Moscow",
        "city": "Moscow",
        "latitude": 55.7558,
        "longitude": 37.6173,
        "currency": {
            "code": "RUB",
            "name": "Ruble",
            "symbol": "RUB"
        },
        "proxy": "yes",
        "type": "Compromised Server"
    }
}
```

- https://www.blackhatworld.com/seo/bot-attack-flooding-my-website-with-requests-need-advice.1639835/ 
- https://hosting.kitchen/vdsina/ddos-ataka-cherez-socialnuyu-inzheneriyu.html
- https://proxycheck.io/api/

https://studentalcare.com/?hal=visitor




```
curl 'https://app-prod.5050cricket.co.in/v1/users/66a0d32ef773a0502d63038e/points' \

-H 'accept: */*' \

-H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \

-H 'authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI2NzBiYTE2MTVhNmVhYzkyODg5MzAyNDUiLCJpYXQiOjE3Mjg4MTcyOTMsImV4cCI6MTcyODgxOTA5MywidHlwZSI6ImFjY2VzcyJ9.522521oX80yaCD-cTIj9UFxo6hDF1ZA8GYr5VvhCj8c' \

-H 'cache-control: no-cache' \

-H 'content-type: application/json' \

-H 'dnt: 1' \

-H 'origin: https://5050cricket.co.in' \

-H 'pragma: no-cache' \

-H 'priority: u=1, i' \

-H 'referer: https://5050cricket.co.in/' \

-H 'sec-fetch-dest: empty' \

-H 'sec-fetch-mode: cors' \

-H 'sec-fetch-site: same-site' \

-H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1'

```


```

curl 'https://app-prod.5050cricket.co.in/v1/auth/register' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'dnt: 1' \
  -H 'origin: https://5050cricket.co.in' \
  -H 'pragma: no-cache' \
  -H 'priority: u=1, i' \
  -H 'referer: https://5050cricket.co.in/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1' \
  --data-raw '{"name":"boopathi","mobileNumber":"8072638765"}' ;

curl 'https://app-prod.5050cricket.co.in/v1/auth/verify-otp' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'dnt: 1' \
  -H 'origin: https://5050cricket.co.in' \
  -H 'pragma: no-cache' \
  -H 'priority: u=1, i' \
  -H 'referer: https://5050cricket.co.in/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1' \
  --data-raw '{"otp":"1180","mobileNumber":"8072638765"}' ;


curl 'https://app-prod.5050cricket.co.in/v1/users/leaderboard' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'dnt: 1' \
  -H 'origin: https://5050cricket.co.in' \
  -H 'pragma: no-cache' \
  -H 'priority: u=1, i' \
  -H 'referer: https://5050cricket.co.in/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1' ;
curl 'https://app-prod.5050cricket.co.in/v1/users/670ba1615a6eac9288930245/points' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \
  -H 'authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI2NzBiYTE2MTVhNmVhYzkyODg5MzAyNDUiLCJpYXQiOjE3Mjg4MTcyOTMsImV4cCI6MTcyODgxOTA5MywidHlwZSI6ImFjY2VzcyJ9.522521oX80yaCD-cTIj9UFxo6hDF1ZA8GYr5VvhCj8c' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'dnt: 1' \
  -H 'origin: https://5050cricket.co.in' \
  -H 'pragma: no-cache' \
  -H 'priority: u=1, i' \
  -H 'referer: https://5050cricket.co.in/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1' ;


curl 'https://app-prod.5050cricket.co.in/v1/users/670ba1615a6eac9288930245/activate-code' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7' \
  -H 'authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI2NzBiYTE2MTVhNmVhYzkyODg5MzAyNDUiLCJpYXQiOjE3Mjg4MTcyOTMsImV4cCI6MTcyODgxOTA5MywidHlwZSI6ImFjY2VzcyJ9.522521oX80yaCD-cTIj9UFxo6hDF1ZA8GYr5VvhCj8c' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'dnt: 1' \
  -H 'origin: https://5050cricket.co.in' \
  -H 'pragma: no-cache' \
  -H 'priority: u=1, i' \
  -H 'referer: https://5050cricket.co.in/' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1' \
  --data-raw '{"activationS3Url":"https://d21hptw77rmcdp.cloudfront.net/activation-code/uploaded-images/1728817892947.jpeg","meta_data":{"os":"iOS","browser":"Mobile Safari","lat":"0.00000","lng":"0.00000"},"ref_id":"britaca05429-43e7-4405-8ab2-261c04b6191d","type":"third_party"}'
```


```python
import requests

url = 'https://app-prod.5050cricket.co.in/v1/auth/register'
headers = {
    'accept': '*/*',
    'accept-language': 'en-US,en;q=0.9,ta-IN;q=0.8,ta;q=0.7',
    'cache-control': 'no-cache',
    'content-type': 'application/json',
    'dnt': '1',
    'origin': 'https://5050cricket.co.in',
    'pragma': 'no-cache',
    'priority': 'u=1, i',
    'referer': 'https://5050cricket.co.in/',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-site',
    'user-agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1'
}

data = {
    'name': '34',
    'mobileNumber': '12345'
}

response = requests.post(url, headers=headers, json=data)

print(response.status_code)
print(response.json())

```

```

Hey everyone! 🌟

I hope you’re all doing well! I wanted to reach out and ask for your feedback on my blog series. I’ve been working on two main themes:

1. Sherlock Holmes Detective Series - where I share my debugging stories and how I solved various tech problems.

You can check out here 
https://dev.to/programmerraja/sherlock-holmes-the-case-of-the-content-length-mismatch-4i2b

2. : Code Against the Clock : - focusing on how I automate the boring stuff using coding.

you can check our here https://dev.to/programmerraja/automate-relax-repeat-the-power-of-coding-1-1egh

I’m really passionate about these topics, but I’m not getting as much audience interaction as I’d hoped. If you have a moment, I’d love your thoughts on what you think could improve or what you’d like to see more of! Any suggestions are welcome, and I truly appreciate your support!

Thanks so much! 🙏
```



Pyhton server ->https://trinket.io/embed/python3/a5bd54189b




Welcome to my **public [Second Brain](https://www.ssp.sh/brain/second-brain)**, a crafted knowledge vault where my **notes, ideas, and insights** are carefully curated, connected, and compounded over time. This vault is modeled as a digital [Zettelkasten](https://www.ssp.sh/brain/zettelkasten) and [Garden](https://www.ssp.sh/brain/digital-garden).

- **Crafted** Narratives: Intentionally shaped notes to educate and inspire—built on strong principles of quality, clarity, and long-term value.
- **Curated** Knowledge: Insights and listings carefully selected and refined over time, across various topics.
- **Connected** Ideas: Explore how thoughts are interconnected with the interactive graph and follow the backlinks to uncover unexpected relationships between them; just like in a human brain.
- **Compounded** Learning: With every note, idea, and insight, my Second Brain grows—compounding in value, just like a money investment, where wisdom deepens and accumulates over time.
(https://www.ssp.sh/brain/#navigation)