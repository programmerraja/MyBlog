

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
- [ ] Exposed phone number on leaderboard
- [ ] What we learns from migrating heroku to kuberneted
	- [ ] Probe
	- [ ] Staing

Sherlock Holmes
- [ ] Econnreset
- [ ] HOw nodejs update 18 restart the pod unhabled rejection
- [ ] Links server slow nginx multiple ssl
- [x] debug ecconreset 15sec to update
- [ ] How we fix dns error in pod

How to 
- [ ] How to use wireshark
- [ ] How to use zap scan
- [ ] How to use lets encrypt
- [ ] how to stream mongodb request 
- [ ] Post about klenty dns
- [ ] Rabbitmq load test

Learn from mistakes or from expereinces
- [ ]  Migration to 
- [ ] How Our devops accedntly deelted the production kube cluster

Unsolved mysteries
- [ ] Vault timeout 
- [ ] Meilisearch issue `Permission denied (os error 13)`

A series for internal wokring
- [ ] Mongodb

How i solve X problem
- [ ] Pod to pod communication

Generative AI series of what i know
- [ ] https://ullyer.medium.com/outlines-make-llm-structured-outputs-controllable-and-improve-the-stability-of-llm-applications-584ae9db3789 
- [ ] How to get structure output
- [ ] EVAl and Observation



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