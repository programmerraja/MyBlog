+++
title = 'Code Against the Clock: How I Enhanced My PM Productivity'
date = 2024-08-24T21:05:17.1717+05:30
draft = true
tags =['code aganist the clock']
+++ 

Welcome back to **"Code Against the Clock!"** blog series where I’ll reveal how I turned my most boring tasks into streamlined, time-saving machines. I’ll share the exact steps I took to automate these chores and the cool tricks I discovered along the way. Ready to see how you can save time and make life a bit more exciting? Let’s dive in and get your tasks on autopilot!

## The backstory

I’m part of a startup that embraces Agile methodology, managing our work through two-week sprints with assigned tasks for the entire period. Each Jira ticket comes with sub-tasks, but let’s be honest—most of us developers, including myself 😅, often forget to update these sub-tasks. As a result, when it’s time to close out the sprint, Jira flags these unfinished sub-tasks even if the main task is completed.

This means our PM has to manually go through and close each sub-task or chase down the developers to update them. Plus, before closing a sprint, we hold a retro meeting to analyze which tasks took too long, which ones bounced back and forth between testing and development, and figure out why. This involves manually checking task histories to calculate the time spent on each task and how often they changed status. It’s a tedious process that can take up a lot of valuable time.

To tackle this, I decided to leverage my scripting skills to automate these repetitive tasks. Here’s how I approached it:

1. **Command Line Tool**: I created a command line tool that simplifies the process for our PM. The tool can either close all sub-tasks for a specific sprint or perform detailed calculations based on task history.
    
2. **Closing Sub-Tasks**: When the PM opts to close sub-tasks, the tool fetches all completed status tasks from the sprint. It then updates all associated sub-tasks to completed status, saving the PM from manual updates.
    
3. **Calculating Task Metrics**: If the PM wants to analyze task metrics, the tool extracts all tasks from the sprint and examines their history logs. It calculates the time taken for development, testing, and any back-and-forth movements. The results are then exported as a CSV file, which can be imported into Google Sheets for further analysis.
    

By automating these tasks, we not only save time but also improve accuracy and efficiency. It’s a win-win for everyone involved!

Note: If you want the source code feel free to ping me :)
### **Your Turn!**

Have you ever automated a task using code? Share your experiences and tips in the comments below! What tasks do you wish you could automate? Let's discuss!

Thanks for joining me on this automation journey. Don’t forget to subscribe to my blog for more tips and updates. Happy coding!















#### **1. Background:**

- **Setting:** A colorful and modern office environment with elements like computers, Jira dashboards, and charts in the background. This will set the scene for the tech and productivity theme.

#### **2. Main Characters:**

- **Product Manager (PM) Character:**
    - **Appearance:** A friendly cartoon character in business casual attire, looking relieved and happy. They’re interacting with a computer screen that shows an automated process.
    - **Expression:** Excited and relaxed, showcasing the relief from the automation.
- **Developer Character:**
    - **Appearance:** Another cartoon character of 22 year old looks , perhaps wearing casual tech attire, with a satisfied or content expression, working on a computer while automation handles the tasks.
    - **Expression:** Happy and relaxed, emphasizing the benefit of automated processes.

#### **3. Key Visual Elements:**

- **Jira Dashboard:**
    
    - **Display:** A cartoon version of the Jira dashboard with various tasks and sub-tasks, some marked as completed and others in progress.
    - **Automation Feature:** Show an animated effect or icon, like a gear or lightning bolt, symbolizing the automation process.
- **Command Line Tool:**
    
    - **Display:** Illustrate a computer screen or terminal window with cartoon code snippets and commands, highlighting the tool’s functionality.
- **Task Metrics:**
    
    - **Visuals:** Include a CSV file or a Google Sheets icon with cartoon-style graphs and charts, representing the data analysis feature of the tool.

#### **4. Title Text:**

- **Text:** "How I Enhanced My PM Productivity"
- **Font:** Use a bold, modern font with playful accents to match the cartoon theme. Consider adding some tech-related icons or elements around the text for added flair.

#### **5. Additional Details:**

- **Automation Icons:** Add small, cartoonish icons representing automation, like gears or robot hands, to reinforce the concept of efficiency and automation.
- **Relaxation Element:** Show a small detail, like a hammock or a beach scene in the background, subtly indicating that automation helps in gaining more leisure time.

#### **Visual Composition:**

- **Foreground:** Position the main characters prominently in the center, interacting with the Jira dashboard and automation tool.
- **Background:** Include the Jira dashboard, command line tool, and data analysis elements in the background, ensuring they’re clear but not overwhelming.
- **Title Placement:** Place the title text at the top or bottom of the image, ensuring it’s easily readable against the background.














Hello Techies welcome back another intersting blog series of `Automate Relax Repeat The Power of Coding` today i wil share how i save my product manger time my automating the jira task 

so i am basically working in startup where we following the Agile and we have a 2 weak sprint where we assingned the task for entire 2 week and we where work on the task and each jira ticket have a sub task in jira ticket but mostly no dev will update the subtask status including me :sweat_smile  so after 2 weak of sprint where they try to close compelete the sprint where jira will list all task that subtask status not moved to compeleted but parent task is in compeleted so our PM mostly try to close each subtack manually or ask the dev to do  and before closing the sprint we have a retro where they need to find out whihc task take more time and which task go back and foth between tester and they choose the task and invistegate why it is like and how we can avoid and why the task is taken so much time does that required time or any blocker faced by dev team is there is any way we can avoid on further sprint so to find no of days it take for dev and test and no of time it moved back and forth they manually check each task status moved date and time and status change frequency to calculate these and this is boring task so i want to save time to picked my scipting skill and started coding the algo will be like below
- A cmd line tool where ask PM what they want to do close all subtask in specific sprint or calculate above mention thing for a specific sprint
- Based on PM input if close all subtask i fetch all compelted status task from the sprint and go through all subtask and move the status to compeleted
- if they want calcuation i will fetch all task from the sprint and based on history log of task i will calclate amount of day taken for dev ,test and backend forth and write that as CSV where they can import in google sheet for further analysis







**Background**:

- **Setting:** A colorful and modern office environment with elements like computers,

**Main Character (Developer)**:

- **Appearance**: The small boy, who is the developer, should look smart with a clean face, no beard, and without glasses.
- **Clothing**: Outfit him in a stylish hoodie or smart casual attire like a tech-themed t-shirt and jeans.
- **Expression**: A confident and focused look with a slight smile.

**Pose**:

- **Position**: Have the boy seated at a laptop with an active coding pose. The laptop screen could show lines of code or a tech interface.

**Marketing Team**:

- **Appearance**: Include a few diverse cartoon characters representing the marketing team. They should look engaged and in action.
    - **Character 1**: A person with a notepad or tablet, taking notes or discussing ideas.
    - **Character 2**: Someone pointing at charts or graphs, indicating they’re analyzing data.
    - **Character 3**: Another person with a coffee cup or smartphone, looking enthusiastic.

**Interactions**:

- **Positioning**: Place the marketing team characters around the developer. They could be looking over the developer’s shoulder, discussing, or brainstorming together.
- **Expressions**: They should have expressions of interest, excitement, and collaboration.

**Additional Elements**:

- **Clock**: Include a cartoonish clock to represent the "against the clock" theme. Place it in the background or subtly integrated into the design.
- **Automation Symbols**: Add visual elements like gears, circuit lines, or icons representing data flow around the characters to emphasize the automation aspect.
- **Data and Charts**: You can include simple graphs or charts near the marketing team to signify their data-driven role.

**Text**:

- **Title Placement**: Position "Code Against the Clock!" prominently at the top or bottom of the image in a bold, modern font with contrasting colors.

**Color Palette**:

- **Primary Colors**: Shades of blue, white, and gray for a tech feel.
- **Accent Colors**: Bright colors like orange or green to make key elements stand out.

### **Example Layout**:

1. **Top Section**:
    
    - Title: “Code Against the Clock!” in bold, eye-catching font.
2. **Central Focus**:
    
    - Cartoon Developer: Small boy coding on a laptop with a dynamic pose.
    - Marketing Team: Diverse characters engaged around the developer, showing collaboration.
3. **Background Elements**:
    
    - Tech-themed patterns or shapes.
    - Cartoon clock and automation symbols.
    - Data charts or graphs near the marketing team.
4. **Bottom Section**:
    
    - Additional tech elements or icons (optional).
