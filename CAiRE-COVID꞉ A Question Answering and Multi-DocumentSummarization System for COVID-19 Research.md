---
tags: [Bioinfomatic UET Assignment, NLP UET]
title: 'CAiRE-COVID: A Question Answering and Multi-DocumentSummarization System for COVID-19 Research'
created: '2020-09-30T12:45:29.343Z'
modified: '2020-09-30T15:53:34.116Z'
---

# CAiRE-COVID: <br>A Question Answering and Multi-DocumentSummarization System for COVID-19 Research

## Abstract
To  address  the  need  for  refined  informationin  COVID-19  pandemic,  we  propose  a  deeplearning-based  system  that  uses  state-of-the-art  natural  language  processing  (NLP)  ques-tion  answering   (QA)  techniques  combinedwith  summarization  for  mining  the  availablescientific literature.  Our system leverages theInformation  Retrieval  (IR)  system  and  QAmodels  to  extract  relevant  snippets  from  theexisting literature given a query.  Fluent sum-maries  are  also  provided  to  help  understandthe content in a more efficient way.  In this pa-per,  we  describe  our  CAiRE-COVID  systemarchitecture and methodology for building thesystem.    To  bootstrap  the  further  study,  thecode for our system is available at https://github.com/HLTCHKUST/CAiRE-COVID.

>Để đáp ứng yêu cầu về thông tin trong đại dịch COVID-19, chúng tôi đã phát triển 1 hệ thống Deep learing, sử dụng kĩ thuật state-of-the-art NLP trong bài toán Question-Answering (QA) kết hợp với `summeriaztion` dùng để mining các bài báo khoa học. Hệ thống của chúng tôi khai thác lợi thế từ `Information Retrieval (IR) system` và `QA models` để đánh dấu các đoạn trích liên quan từ bài báo hiện tại tương ứng với 1 `query`. `Fluent summeries` cũng giúp cho việc hiểu được nội dung hiệu quả hơn. Trong paper này, chúng tôi trình bày về kiến trúc hệ thống CAiRE-COVID của mình và phương pháp xây dựng hệ thống. Để giúp cho việc nghiên cứu sâu hơn, code của hệ thống được cung cấp ở [đây](https://github.com/HLTCHKUST/CAiRE-COVID)

## 1. Introduction
In response to the COVID-19 pandemic, a lot ofscholarly articles have been published recently andmade  freely  available.   At  the  same  time,  thereare emerging requests from both the medical re-search community and the broader society to findanswers to various questions regarding COVID-19.   A system that can provide reliable answersto the COVID-19 related questions from the lat-est academic resources is crucial, especially for themedical community in the current time-critical raceto treat patients and to find a cure for the virus.

> Nói về tính cần thiết cấp bách của hệ thông QA về COVID 19 đáng tin cậy 

To address the aforementioned requests by themedical community, we propose a deep learning-based system that uses state-of-the-art natural lan-guage processing (NLP) question answering (QA)techniques combined with summarization for min-ing the available scientific literature. The system isan end-to-end neural network-based open-domainQA  system  that  can  answer  COVID-19  relatedquestions,  such  as  those  questions  proposed  inthe CORD-19 Kaggle task<sup>1</sup>. Through our system,users can get two versions of the outcome:
* A ranked list of relevant snippets from theliterature given a query;
* Fluent summaries of the relevant results. Weprovide the paragraph-level summaries, whichtakes the paragraphs where the top three rele-vant snippets are located as input, to enable amore efficient way of understanding the con-tent.

> Hệ thống sử dụng state-of-the-art NLP **QA kết hợp Summerization**... Là 1 hệ thống QA `end-to-end` `neural network-based` `open-domain` có thể trả lời các câu hỏi liên quan đến COVID-19, như những câu hỏi đã được đăng tại [CORD-19 Kaggle task<sup>1</sup>](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge). Từ hệ thống của chúng tôi, người dùng nhận được 2 phiên phản output:
* 1 bảng xếp hạng đoạn trích liên quan đến `query`
* `Fluent summeries` của các kết quả liên quan. Chúng tôi cung cấp *tóm tắt mức đoạn văn* (paragraph-level summeries), cụ thể tóm tắt các đoạn văn từ top 3 đoạn trích liên quan nhất, để hiệu quả hơn trong việc hiểu nội dung.

Our system consists of three different modules:1) Document Retriever, 2) Relevant Snippet Selec-tor, and 3) Multi-Document Summarizer. The firstmodule pre-processes a user’s query and retrievesthe most relevantnnumber of academic publica-tions. The second module outputs a list of the mostrelevant answer snippets from the retrieved doc-uments.  It also highlights the relevant keywords.The last module is for generating the second out-put, namely a concise summary of the top-rankedretrieved relevant paragraphs from the two formermodules.

> Hệ thống bao gồm 3 module riêng biệt:
> 1. Document Retriever:
> * Tiền xử lý `query`  từ người dùng và trả về `n` bài đăng khoa học liên quan nhất
> 2. Relevent Snippet Selector
> * Trả về 1 danh sách các đoạn trích liên quan nhất từ các document nhận được.  
> 3. Multi-Document Summerizer
> * Dùng để tạo ra output thứ 2, bản tóm tắt ngắn gọn từ output của module 2

We have launched our CAiRE-Covid website2(as shown in Figure 2). The website showcases ourresults for each user query in real-time for peoplewho may further experiment with our system.

> Hệ thống đã được lauch [online](https://caire.ust.hk/covid)

## 2 Related Work

With the release of the COVID-19 Open ResearchDataset  (CORD-19)<sup>3</sup> by  Allen  Institute  for  AI,multiple  systems  have  been  built  to  make  bothresearchers and public explore valuable informa-tion  related  to  COVID-19.    CORD-19  Search<sup>4</sup> is  a  search  engine  that  utilized  the  CORD-19dataset processed by Amazon Comprehend Med-ical.  Covidex<sup>5</sup> applied multi-stage search archi-tectures which can extract different features from data.  A NLP medical relationship engine named WellAI COVID-19 Research Tool6is able to createa structured list of medical concepts with ranked probabilities related to COVID-19. tmCOVID7isa bioconcept extraction and summarization tool forCOVID-19 literature.

> Dataset: [COVID-19 Open ResearchDataset (CORD-19)](https://pages.semanticscholar.org/coronavirus-research) by  Allen  Institute  for  AI
> Nói về các hệ thống khác sử dụng dataset trên.
> * [CORD-19  Search](https://cord19.aws/) - Amazon Comprehend Medical
> * [Covidex](https://covidex.ai/)
> * WellAI COVID-19 Research Tool
> * tmCOVID

All  these  systems  either  focus  on  search  en-gine  such  as  CORD-19  Search  or  medical  con-cepts such as WellAI COVID-19 Research Tooland tmCOVID. However, our system, in addition to information retrieval, gives high quality relevantsnippets and summarization results based on the user query. We provide a more versatile system for public use, which can display various informationabout COVID-19 in a well structured and concise manner.

> Tất cả các hệ thống trên tập trung vào Search Engine (máy tìm kiếm), hệ thống của chúng tôi hơn thế, cho ra các đoạn trích liên quan và tóm tắt với chất lược cao từ `query` của người dùng. Chúng tôi cung cấp 1 hệ thống linh hoạt hơn mà cộng đồng có thể sử dụng, hiện thị lượng lớn thông tin về COVID 19 được cấu trúc và rút gọn.

## 3 System Architecture Overview

Figure 1 illustrates the architecture of our system,CAiRE-COVID which consists of three major mod-ules.   We first paraphrase long and complicatedqueries to simpler queries, which are easier for oursystem to comprehend. Then the updated queriesare fed into our IR module,  which returns para-graphs from the full text of the articles having thetopnhighest matching score to the query, wherenis a hyper-parameter. Our QA models are thenapplied to each of the topnparagraphs to selectrelevant sentences from each paragraph as answers.After re-ranking the retrieved para-graphs with highlighted answers that best fit ourquery, we pass topkparagraphs (k < n) to oursummarization models, to get an extractive and anabstractive summary of the paragraphs.

> Hình 1 <sub>*(được cập nhật sau)*</sub> miêu tả hệ thống của tôi, bao gồm 3 module chính. Đầu tiên, chúng tôi diễn giải các `query` dài và phức tạp thành các  `query` đơn giản hơn, dễ xử lí hơn. Các `query` này được đưa vào `IR module`, trả về các đoạn văn từ bản text đầy đủ của các article có `n top` `matching score` cao nhất đối với `query`, trong đó `n` là siêu tham số (`hyper-parameter`). Các `QA model` của chúng tôi, sau đó, được sử dụng với từng đoạn trong n đoạn văn để lựa ra các câu liên quan từ mỗi đoạn để làm `answer` cho `query`. Sau khi xếp hạng lại các đoạn có `answer` được đánh dấu, chúng tôi đưa `top k` đoạn (k < n) đến các `summarization model` để lấy `1 extractive và 1 abtractive summary` của các đoạn.

> Google 'extractive abstractive summarization' để tìm hiều thêm

## 4 Methodology
### 4.1 Document Retrieval
#### 4.1.1 Query Paraphasing
The objective of this sub-module is to break downa user’s query and rephrase complex query sen-tences into several shorter and simpler questionsthat convey the same meaning.   As shorter sen-tences are generally better processed by NLP sys-tems (Narayan et al., 2017), it could be used as apre-processing step to facilitates and improves theperformance of the whole system, since the searchengine and the question answering modules willbe able to find more relevant and less redundantresults.  Its effectiveness have been proved in ourKaggle tasks, and we show some examples in Ap-pendix B. Automatic methods (Min et al., 2019;Perez et al., 2020) will be explored to handle morecomplicated compound questions in the future.

> Mục tiêu của module phụ, tiền xử lí này là phân giải `query` dài và phức tạp thành nhiều câu ngắn và đơn giản. Vì các câu ngắn thường được xử lý tốt hơn bởi các hệ thống NLP ([Narayan et al., 2017](https://www.aclweb.org/anthology/P17-1147/)), `search engine` và `QA modules` sẽ có khả năng thu được kết quả liên quan hơn và ít dư thừa. Hiệu quả đã được chứng minh bằng các kết quả Kaggle task, 1 vài vd ở phần Phụ lục B. Các thức tự động hóa ((Min et al., 2019;Perez et al., 2020) sẽ được khai thác cho xử lí nhiều hỗn hợp câu hỏi phức tập trong tương lai

#### 4.1.2 Search Engine
We use Anserini (Yang et al., 2018a) to create the search engine to retrieve a preliminary candidateset of documents.  Anserini is an information re-trieval module wrapped around the open sourcesearch engine Lucene8. Although Lucene has beenused widely to build industry standard search en-gine applications, its complex indexing and lack ofdocumentation for ad-hoc experimentation and test-ing on standard test sets, has made it less popular

> **Chưa xong** 
> Sử dụng Anserini để tạo search engine lấy các tài liệu sơ bộ. 
> (Giới thiệu về Anserini và Lucene)
> Các thuật toán xếp hạng tiêu chuần (vd `bag of words`, `BM25`) được cài đặt trong module này, cho phép cúng tôi sử dụng Lucene.

### 4.2 Relevent Snippet Selector
#### 4.2.1 Question Answering
For the question answering (QA) module, we haveleveraged  the  BioBERT  (Lee  et  al.,  2020)  QAmodel which is finetuned on the SQuAD datasetand the generalized MRQA model from Su et al.(2019).  Instead of fine-tuning the QA models onCOVID-19  related  datasets,  we  focus  more  onmaintaining the generalization ability of our systemand performing zero-shot question answering. Forthe MRQA model, the authors leverage multi-tasklearning over six datasets is used to fine-tune largepre-trained language model XLNet (Yang et al.,2019) to reduce over-fitting to the training datain order to enable generalization to out-of-domaindata and it helped achieve promising results.  Tomake the answers more readable, instead of pro-viding small spans of answers, we provide the sen-tences that contain the predicted answers as theoutputs.

> Với `QA module`, chúng tôi đã tận dụng `BioBERT` (Lee  et  al.,  2020) `QA model` đã được `finetuned` trên `SQuAD dataset` và `generalized MRQA model` từ Su et al.(2019). Thay vì `fine-tuning` các `QA model` này trên `COVID-19 dataset`, chúng tôi tập trung nhiều hơn vào duy trì khả năng khái quát hóa của hệ thống và thực hiện `zero-shot QA`. Đối với `MRQA model`, tác giả sử dụng `multi-task learning` trên 6 dataset để `fine-tune` `largepre-trained language model XLNet` (Yang et al.,2019) để giảm `over-fitting` đối với `training dât` nhầm `generalization to out-of-domain data` và giúp đạt được kết quả hứa hẹn. Để làm `answer` dễ đọc hơn, thay vì đưa ra các khoảng nhỏ của các câu trả lời, chúng tôi đưa ra câu hoàn chỉnh chứa `predicted answers` làm `output`.

To better display the question answering results,we leverage the prediction probability of the QA models as the confidence score, which will be uti-lized when re-ranking all the answers.  To obtainthe confidence score of an ensemble of two QA models, assuming the confidence score from eachmodel are annotated assmandsb, we follow therules as follows:
(công thức)

> Để hiển thị các kết quả QA tốt hơn, chúng tôi tận dụng `pred probability` của các  `QA model` như `confidence score`, mà sau đó sẽ được sử dụng khi xếp hạng lại tất cả các `answer`. Để nhận được `confidence score` từ 1 *tập hợp* (ensemble) 2 QA model, giả sử `confidence score` từ mỗi model tương ứng là s<sub>m</sub> và s<sub>b</sub>, chúng tôi làm theo quy tắc như sau:
>
>$s_{conf}=\begin{cases}0.5min(|s_m|,|s_b|) &\text{if } s_m, s_b < 0\\-max(|s_m|,|s_b|)&\text{if } s_m, s_b < 0 \small  \quad(\text{giống trường hợp trên})\\s_m + s_b \text{ otherwise}\end{cases}$  
$e^i$
