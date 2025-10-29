---
title: "Catch me, Yes we can! - Pwning Social Engineers using Natural Language Processing Techniques in Real-Time"
collection: publications
category: conferences
permalink: /publication/2018-catch-me-yes-we-can
excerpt: 'This paper presents an approach which analyzes attack content to detect inappropriate statements which are indicative of social engineering attacks. The approach focuses on natural language text analysis performing semantic analysis to detect malicious intent, making it applicable to detect attacks using non-email vectors including texting, chat applications, and phone/in-person attacks.'
date: 2018-08-01
venue: 'Black Hat USA 2018'
slidesurl: 'https://www.youtube.com/watch?v=i0f9T3aEvM0'
citation: 'Myeongsoo Kim, Changheon Song, Hyeji Kim, Deahyun Park, Yeeji Kwon, Eun Namkung, Ian G Harris, Marcel Carlsson. (2018). &quot;Catch me, Yes we can! - Pwning Social Engineers using Natural Language Processing Techniques in Real-Time.&quot; <i>Black Hat USA 2018</i>.'
---

Social engineering attacks are one of the most common and least defended security threats today. This paper presents an approach which analyzes attack content to detect inappropriate statements which are indicative of social engineering attacks.

## Key Contributions

The approach is novel compared to previous work because it focuses on natural language text contained in the attack, performing semantic analysis of the text to detect malicious intent. This makes the approach applicable to detect social engineering attacks using non-email attack vectors, including:
- Texting applications
- Chat applications  
- Phone/in-person attacks (converted to text using speech-to-text)

The system identifies sentences with malicious intent by detecting:
- **Questions** whose answers are private
- **Commands** to perform forbidden operations

The approach leverages question answering systems to determine privacy status of answers, and uses verb-object pairs to evaluate whether commands describe forbidden operations. The effectiveness was demonstrated using a large benchmark set of phishing emails.

**Authors:** Myeongsoo Kim, Changheon Song, Hyeji Kim, Deahyun Park, Yeeji Kwon, Eun Namkung, Ian G Harris, Marcel Carlsson

**Video Presentation:** [Watch on YouTube](https://www.youtube.com/watch?v=i0f9T3aEvM0)
