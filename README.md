## 🔍Project Overview

### 📌 Title
- Rag-based-Chatbot


### 📌 Introduction
- This chatbot is designed to assist Kwangwoon University students by providing information on graduation requirements, food recommendations, course evaluations, and school announcements. It processes student data to determine graduation eligibility, recommends nearby restaurants, and offers insights into course evaluations based on Everytime reviews. Additionally, it retrieves and delivers school announcements to keep students informed. The system is built on an **API-based GPT-4o model** and utilizes a **FAISS vectorstore** for efficient information retrieval. To ensure accurate and contextually relevant responses optimized for the Korean language, it employs **Hugging Face's jhgan/ko-sroberta-nli** for embeddings. The website reflecting this RAG-based chatbot can be found at **https://github.com/yangmunseok/KWChatBot**.


### 📌 Objective
- The goal of this project is to develop a chatbot that provides personalized academic information to students at Kwangwoon University by utilizing Retrieval-Augmented Generation (RAG) technology. The chatbot analyzes each student's progress toward fulfilling graduation requirements and offers detailed guidance for areas needing improvement. Additionally, it aims to enhance students' academic and daily convenience by providing both academic information and lifestyle information.


### 📌 Structure
![Image](https://github.com/user-attachments/assets/9f1b6372-eb7f-4e88-b16c-e157e98c4855)
![Image](https://github.com/user-attachments/assets/7c43a3da-a7e4-41ed-b13f-9253739a1594)


### 📌 Methodology
#### 1. Structured Datastore Construction
To ensure precise retrieval of relevant information, a structured datastore was designed using a hierarchical folder system. Markdown files from _2025-1 Course Registration Guidelines_ in the upload folder were categorized into domain-specific subfolders. The graduation requirements category was further subdivided due to its inherent complexity, where criteria vary significantly across departments. Other categories were populated via web crawling.

Each subfolder’s content was converted into vector embeddings using FAISS and stored in corresponding db subfolders. This structure ensures that retrieval operations are confined to the target category, eliminating interference from semantically similar but irrelevant chunks in other domains.

#### 2. Query Processing Pipeline
1) **Category Classification:** For an incoming query, GPT-4o first identifies its associated category.

2) **Targeted Retrieval:** The system accesses only the FAISS index of the identified category to retrieve contextually relevant chunks, ensuring domain-specific accuracy. From the second visit onward, it does not revisit--repetition.

3) **Response Generation:** GPT-4o synthesizes the final response using the query, retrieved chunks, and the top-5 most relevant chat history chunks (retrieved via hybrid search).

#### 3. Handling Complex Tabular Data
To address GPT-4o’s limitations in parsing markdown-formatted tables, a dual-model approach was implemented for graduation-related queries:

1) **Schema Extraction:** GPT-4o instance extracts structured schemas from markdown tables.
2) **Student-Specific Comparison:** A separate GPT-4o instance cross-references the extracted schema with the student’s academic information.

This approach significantly improved accuracy in evaluating complex graduation criteria, utilizing 9 specialized GPT-4o instances for diverse sub-tasks.

#### 4. Chat History Management
- coming soon...


### 📌 Duration
- 2025.01 ~ 2025.03


### 🧑‍🤝‍🧑Team Member
#### RAG based LLM
- 최유종: Me
- 최지원: https://github.com/Jiwon-Choi0315

#### Web
- 양문석: https://github.com/yangmunseok
- 윤서정: jihyan01@naver.com


## 📝 Details

### 📌 Setting Up and Running
- **Setting Up:** To store the vectorstore, create an 'upload' folder and upload the necessary md files -> Run create_md_files.py -> Run save_vectorstore.py.
- **Running:** Run kw_chat_bot.py  (Note: the vectorstore must already exist). 

### 📌 Reminder
#### Don't Forget to Enter ID and PW
- **make_md_via_crawling function (create_md_files.py):** enter Everytime ID, PW. 
- **get_personal_info function (kw_chat_bot.py):** enter klas ID, PW

#### Crawling takes a Long Time
- **lectureEval_everytime function (crawling.py):** contains _break_ to stop early. Removing this line will allow scraping all lectures, but it may take several hours.
- **food_naver_maps functionIn (crawling.py):** includes _page_down(10)_. Increasing this number will crawl more restaurants. 10 crawls 20 restaurants, 40 crawls 60 restaurants.

### 📌 Limitations and Reflections
The proposed methodology, while effective in navigating complex academic policies, faces notable limitations. A key challenge lies in processing markdown-formatted tables, which currently requires a multi-step approach: one GPT-4o instance extracts structured schemas from tables, leading to another instance cross-referencing them with student profiles. This fragmented workflow increases computational overhead and complicates code maintenance. A unified model capable of directly interpreting tabular data and comparing it with user information would significantly streamline the system. 

Additionally, the system’s multimodal capabilities remain underutilized. However, integrating this feature into the web was postponed as project's requirements had already been met. (It's a bit of TMI, but after testing it with my timetable, I was complimented for making a good schedule. I'm not sure if it compliments everyone, though...)

Reflecting on the project, the partial success of multimodal experiments highlights the transformative potential of combining textual and visual data in academic advising. Future efforts could expand this capability to interpret diverse inputs, reducing reliance on manual data entry. Developing the chatbot also underscored the real-world impact of AI in education. Even small improvements in accessibility--such as simplifying policy navigation for Kwangwoon University students--can meaningfully alleviate administrative burdens. This experience reinforced the importance of balancing technical innovation with practical usability, ensuring that advanced features like multimodal support are paired with intuitive interfaces. Moving forward, refining these aspects will be critical to fully harnessing AI’s potential while maintaining system simplicity and scalability.

