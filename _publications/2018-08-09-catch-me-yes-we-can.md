---
title: "Catch me, Yes we can! - Pwning Social Engineers using Natural Language Processing Techniques in Real-Time"
collection: publications
category: conferences
permalink: /publication/2018-catch-me-yes-we-can
excerpt: 'White paper presenting an approach to detect social engineering attacks through natural language processing and semantic analysis. Applicable to multiple attack vectors including email, texting, chat applications, and phone/in-person communication.'
date: 2018-08-09
venue: 'Black Hat USA 2018 (White Paper)'
paperurl: 'https://codingsoo.github.io/files/catch-me-yes-we-can-blackhat2018.pdf'
videourl: 'https://www.youtube.com/watch?v=i0f9T3aEvM0'
citation: 'Myeongsoo Kim, Changheon Song, Hyeji Kim, Deahyun Park, Yeeji Kwon, Eun Namkung, Ian G Harris, Marcel Carlsson. (2018). &quot;Catch me, Yes we can! - Pwning Social Engineers using Natural Language Processing Techniques in Real-Time.&quot; <i>Black Hat USA 2018 White Paper</i>.'
---

Social engineering attacks are one of the most common and least defended security threats today. This white paper presents an approach which analyzes attack content to detect inappropriate statements which are indicative of social engineering attacks.

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

**Resources:**
- [Download White Paper (PDF)](https://codingsoo.github.io/files/catch-me-yes-we-can-blackhat2018.pdf)
- [Video Presentation (YouTube)](https://www.youtube.com/watch?v=i0f9T3aEvM0)
- [Black Hat USA 2018 Session](https://www.blackhat.com/us-18/briefings/schedule/#catch-me-yes-we-can--pwning-social-engineers-using-natural-language-processing-techniques-in-real-time-9946)

## BibTeX

```bibtex
@misc{kim2018catch,
  title={Catch me, yes we can!-pwning social engineers using natural language processing techniques in real-time},
  author={Kim, Myeongsoo and Song, Changheon and Kim, Hyeji and Park, Deahyun and Kwon, Yeeji and Namkung, Eun and Harris, Ian G and Carlsson, Marcel},
  year={2018},
  howpublished={Black Hat USA 2018 White Paper},
  url={https://www.blackhat.com/us-18/briefings/schedule/#catch-me-yes-we-can--pwning-social-engineers-using-natural-language-processing-techniques-in-real-time-9946}
}
```
