---
title: "Scam Detection Assistant: Automated Protection from Scammers"
collection: publications
category: conferences
permalink: /publication/2019-scam-detection-assistant
excerpt: 'This paper presents a Scam Detection Assistant (SDA) that analyzes attack content through semantic analysis to detect social engineering attacks. The approach focuses on natural language content analysis, making it applicable to various attack vectors including phone, text, chat, and in-person communications.'
date: 2019-09-04
venue: '2019 First International Conference on Societal Automation (SA)'
paperurl: 'https://par.nsf.gov/servlets/purl/10167943'
citation: 'M. Kim et al., &quot;Scam Detection Assistant: Automated Protection from Scammers,&quot; 2019 First International Conference on Societal Automation (SA), Krakow, Poland, 2019, doi: 10.1109/SA47457.2019.8938036.'
github: 'https://github.com/codingsoo/Archive/tree/main/social-engineering-defense'
---

Scams, also known as social engineering attacks, are an extremely common and dangerous threat today. This paper presents a tool called Scam Detection Assistant (SDA) which analyzes attack content to detect inappropriate statements indicative of social engineering attacks.

## Abstract

Scams typically result in financial loss by convincing a victim to perform an ill-advised action such as sending money, or convincing them to provide private information. In this paper we present an approach to detect scams, focusing on scams which are conveyed in-person, over the phone, or via text/chat message. The Scam Detection Assistant (SDA) analyzes attack content to detect inappropriate statements which are indicative of social engineering attacks.

## Key Innovation

SDA is novel compared to previous work because it focuses on the natural language contained in the attack, performing semantic analysis of the content to detect malicious intent. Previous research on scam detection heavily relied on metadata specific to email attack vectors (header information, URL links). 

### Advantages of Content-Based Analysis

Focusing on content analysis makes our approach applicable to detect scams using non-email attack vectors, including:
- **Texting applications**
- **Chat applications**
- **Phone communications** (using speech-to-text conversion)
- **In-person attacks** (using speech-to-text conversion)

## Background

A critical threat to the security of individuals and organizations is the increasing rate of scams being perpetrated each year. In 2016 alone, a total of 9.5 billion dollars was lost in the USA due to phone scams. The more formal term for scamming is social engineering - the psychological manipulation of people to gain unauthorized system access.

Modern communication technologies, including cellular phones and the internet, have greatly increased both the reach of attackers and the effectiveness of their attacks.

**Authors:** Myeongsoo Kim, Changheon Song, Hyeji Kim, Deahyun Park, Yeeji Kwon, Eun Namkung

**Published in:** 2019 First International Conference on Societal Automation (SA), Krakow, Poland

**Date of Conference:** 04-06 September 2019

**DOI:** [10.1109/SA47457.2019.8938036](https://doi.org/10.1109/SA47457.2019.8938036)

**Resources:**
- [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/8938036)
- [Free Paper (NSF PAR)](https://par.nsf.gov/servlets/purl/10167943)
- [GitHub Repository](https://github.com/codingsoo/Archive/tree/main/social-engineering-defense)

## BibTeX

```bibtex
@inproceedings{kim2019scam,
  title={Scam detection assistant: Automated protection from scammers},
  author={Kim, Myeongsoo and Song, Changheon and Kim, Hyeji and Park, Deahyun and Kwon, Yeeji and Namkung, Eun and Harris, Ian G and Carlsson, Marcel},
  booktitle={2019 first international conference on societal automation (SA)},
  pages={1--8},
  year={2019},
  organization={IEEE}
}
```