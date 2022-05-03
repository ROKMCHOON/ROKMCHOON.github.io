```python
import os
os.chdir(r'C:\Users\ds990\Desktop\대학교\NLP\데이터')
```


```python
import warnings
warnings.filterwarnings('ignore')
```


```python
paper = pd.read_csv('raw_data.csv')
paper.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Authors</th>
      <th>Title</th>
      <th>Source</th>
      <th>Type</th>
      <th>Keywords</th>
      <th>Abstract</th>
      <th>Corresponding_Address</th>
      <th>Citation_Num</th>
      <th>Year</th>
      <th>WoS Research Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Truant, Elisa; Broccardo, Laura; Dana, Leo-Paul</td>
      <td>Digitalisation boosts company performance: an ...</td>
      <td>TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE</td>
      <td>Article</td>
      <td>Digitalization; Company performance; Listed co...</td>
      <td>Digitalisation has become embedded in products...</td>
      <td>Truant, E(?�당 ?�??, Univ Turin, Dept Managemen...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business; Regional &amp; Urban Planning</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcysiak, Agata; Pleskacz, Zanna</td>
      <td>DETERMINANTS OF DIGITIZATION IN SMES</td>
      <td>ENTREPRENEURSHIP AND SUSTAINABILITY ISSUES</td>
      <td>Article</td>
      <td>outsourcing; sustainable development; core com...</td>
      <td>The aim of the study is to present the conditi...</td>
      <td>Marcysiak, A(?�당 ?�??, Siedlce Univ Nat Sci &amp; ...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saniuk, Sebastian; Grabowska, Sandra</td>
      <td>The Concept of Cyber-Physical Networks of Smal...</td>
      <td>ENERGIES</td>
      <td>Article</td>
      <td>cyber-physical networks; Industry 4; 0; small ...</td>
      <td>The era of Industry 4.0 is characterized by th...</td>
      <td>Saniuk, S(?�당 ?�??, Univ Zielona Gora, Dept En...</td>
      <td>3</td>
      <td>2021</td>
      <td>Energy &amp; Fuels</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Shen, Lei; Sun, Cong; Ali, Muhammad</td>
      <td>Role of Servitization, Digitalization, and Inn...</td>
      <td>SUSTAINABILITY</td>
      <td>Article</td>
      <td>servitization; digitalization; dynamic capabil...</td>
      <td>The structure of the manufacturing industry ha...</td>
      <td>Ali, M(?�당 ?�??, Dongguan Univ Technol, Sch Ec...</td>
      <td>3</td>
      <td>2021</td>
      <td>Green &amp; Sustainable Science &amp; Technology; Envi...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Yang, Shuili; Yi, Yang</td>
      <td>Effect of Technological Innovation Inputs on G...</td>
      <td>JOURNAL OF GLOBAL INFORMATION MANAGEMENT</td>
      <td>Article</td>
      <td>GVC Embedding Position; Industrial Value-Added...</td>
      <td>Under the backdrop of the continuous escalatio...</td>
      <td>Yang, SL(?�당 ?�??, Xian Univ Technol, Xian, Pe...</td>
      <td>1</td>
      <td>2021</td>
      <td>Information Science &amp; Library Science</td>
    </tr>
  </tbody>
</table>
</div>




```python
paper_data = paper.drop(['Corresponding_Address'], axis=1)
paper_data.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Authors</th>
      <th>Title</th>
      <th>Source</th>
      <th>Type</th>
      <th>Keywords</th>
      <th>Abstract</th>
      <th>Citation_Num</th>
      <th>Year</th>
      <th>WoS Research Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Truant, Elisa; Broccardo, Laura; Dana, Leo-Paul</td>
      <td>Digitalisation boosts company performance: an ...</td>
      <td>TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE</td>
      <td>Article</td>
      <td>Digitalization; Company performance; Listed co...</td>
      <td>Digitalisation has become embedded in products...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business; Regional &amp; Urban Planning</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcysiak, Agata; Pleskacz, Zanna</td>
      <td>DETERMINANTS OF DIGITIZATION IN SMES</td>
      <td>ENTREPRENEURSHIP AND SUSTAINABILITY ISSUES</td>
      <td>Article</td>
      <td>outsourcing; sustainable development; core com...</td>
      <td>The aim of the study is to present the conditi...</td>
      <td>2</td>
      <td>2021</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saniuk, Sebastian; Grabowska, Sandra</td>
      <td>The Concept of Cyber-Physical Networks of Smal...</td>
      <td>ENERGIES</td>
      <td>Article</td>
      <td>cyber-physical networks; Industry 4; 0; small ...</td>
      <td>The era of Industry 4.0 is characterized by th...</td>
      <td>3</td>
      <td>2021</td>
      <td>Energy &amp; Fuels</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Shen, Lei; Sun, Cong; Ali, Muhammad</td>
      <td>Role of Servitization, Digitalization, and Inn...</td>
      <td>SUSTAINABILITY</td>
      <td>Article</td>
      <td>servitization; digitalization; dynamic capabil...</td>
      <td>The structure of the manufacturing industry ha...</td>
      <td>3</td>
      <td>2021</td>
      <td>Green &amp; Sustainable Science &amp; Technology; Envi...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Yang, Shuili; Yi, Yang</td>
      <td>Effect of Technological Innovation Inputs on G...</td>
      <td>JOURNAL OF GLOBAL INFORMATION MANAGEMENT</td>
      <td>Article</td>
      <td>GVC Embedding Position; Industrial Value-Added...</td>
      <td>Under the backdrop of the continuous escalatio...</td>
      <td>1</td>
      <td>2021</td>
      <td>Information Science &amp; Library Science</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 토큰화를 띄어쓰기 기준으로 할 때를 생각해서 중간에 공백 추가
paper_data['Title+Abstract'] = paper_data['Title'] + ' ' + paper_data['Abstract']
```


```python
pd.set_option('display.max_colwidth', -1)
pd.set_option('display.max_row', 500)
```


```python
df = pd.DataFrame(paper_data)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Authors</th>
      <th>Title</th>
      <th>Source</th>
      <th>Type</th>
      <th>Keywords</th>
      <th>Abstract</th>
      <th>Citation_Num</th>
      <th>Year</th>
      <th>WoS Research Area</th>
      <th>Title+Abstract</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Truant, Elisa; Broccardo, Laura; Dana, Leo-Paul</td>
      <td>Digitalisation boosts company performance: an overview of Italian listed companies</td>
      <td>TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE</td>
      <td>Article</td>
      <td>Digitalization; Company performance; Listed company; Benefit; Obstacle</td>
      <td>Digitalisation has become embedded in products and services, and it increasingly supports corporate business processes. However, few empirical studies have analysed the state of digitalisation and its implementation within companies, and the extant literature has painted an inconsistent picture concerning the effects of digitalisation. This survey-based study explores the diffusion of digitalisation, the advantages and difficulties in the practical transition to digitalisation, and its impact on performance. The sample includes Italian listed companies across diverse industries. The results highlight the still embryonic adoption of digital tools to support daily company operations; however, the impacts of digitalisation on company performance are noticeable. This research con-tributes to the literature on digitalisation and performance, breaks new ground by focusing on listed companies, and has implications for management investment in digitalisation for value creation.</td>
      <td>2</td>
      <td>2021</td>
      <td>Business; Regional &amp; Urban Planning</td>
      <td>Digitalisation boosts company performance: an overview of Italian listed companies Digitalisation has become embedded in products and services, and it increasingly supports corporate business processes. However, few empirical studies have analysed the state of digitalisation and its implementation within companies, and the extant literature has painted an inconsistent picture concerning the effects of digitalisation. This survey-based study explores the diffusion of digitalisation, the advantages and difficulties in the practical transition to digitalisation, and its impact on performance. The sample includes Italian listed companies across diverse industries. The results highlight the still embryonic adoption of digital tools to support daily company operations; however, the impacts of digitalisation on company performance are noticeable. This research con-tributes to the literature on digitalisation and performance, breaks new ground by focusing on listed companies, and has implications for management investment in digitalisation for value creation.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcysiak, Agata; Pleskacz, Zanna</td>
      <td>DETERMINANTS OF DIGITIZATION IN SMES</td>
      <td>ENTREPRENEURSHIP AND SUSTAINABILITY ISSUES</td>
      <td>Article</td>
      <td>outsourcing; sustainable development; core competencies; company management</td>
      <td>The aim of the study is to present the conditions for introducing digitization in enterprises of the Polish SME sector, with particular emphasis on the changes brought about by the pandemic. The study analyses the degree of digitization which has taken place in the Polish economy in comparison with the EU average, with particular emphasis on SMEs, by making recourse to the Digital Economy and Society Index (DESI).This study presents the results of my own research on the determinants of digitization in enterprises. The subject of the research was a group of enterprises classified as SMEs in Poland. The survey was conducted in March 2021 using electronic tools in the form of an online survey. After substantive and logical verification, 120 questionnaires were selected for further analysis.The analysis of enterprises from the SME sector showed that over 44% of enterprises operate on the basis of action plans not exceeding one year. This type of planning was particularly common in micro-enterprises employing up to 10 people and running a service activity. Digitization acted as an important process in the activities of the analysed enterprises. Every fifth surveyed enterprise had plans to invest in software and digital solutions for enterprises; and wanted to implement these plans within the next year. The most common area of activity and implementation of digital solutions was sales and distribution. This was due to the need during the pandemic to build new distribution channels for products or services through the increasingly important e-commerce market. Research has shown that the Covid situation has led to significant changes taking place in the economy of enterprises. More than half of the analysed enterprises indicated a lack of financial resources as a barrier when introducing cloud solutions.</td>
      <td>2</td>
      <td>2021</td>
      <td>Business</td>
      <td>DETERMINANTS OF DIGITIZATION IN SMES The aim of the study is to present the conditions for introducing digitization in enterprises of the Polish SME sector, with particular emphasis on the changes brought about by the pandemic. The study analyses the degree of digitization which has taken place in the Polish economy in comparison with the EU average, with particular emphasis on SMEs, by making recourse to the Digital Economy and Society Index (DESI).This study presents the results of my own research on the determinants of digitization in enterprises. The subject of the research was a group of enterprises classified as SMEs in Poland. The survey was conducted in March 2021 using electronic tools in the form of an online survey. After substantive and logical verification, 120 questionnaires were selected for further analysis.The analysis of enterprises from the SME sector showed that over 44% of enterprises operate on the basis of action plans not exceeding one year. This type of planning was particularly common in micro-enterprises employing up to 10 people and running a service activity. Digitization acted as an important process in the activities of the analysed enterprises. Every fifth surveyed enterprise had plans to invest in software and digital solutions for enterprises; and wanted to implement these plans within the next year. The most common area of activity and implementation of digital solutions was sales and distribution. This was due to the need during the pandemic to build new distribution channels for products or services through the increasingly important e-commerce market. Research has shown that the Covid situation has led to significant changes taking place in the economy of enterprises. More than half of the analysed enterprises indicated a lack of financial resources as a barrier when introducing cloud solutions.</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saniuk, Sebastian; Grabowska, Sandra</td>
      <td>The Concept of Cyber-Physical Networks of Small and Medium Enterprises under Personalized Manufacturing</td>
      <td>ENERGIES</td>
      <td>Article</td>
      <td>cyber-physical networks; Industry 4; 0; small and medium enterprises; personalization; servitization</td>
      <td>The era of Industry 4.0 is characterized by the use of new telecommunications ICT technologies and networking of the economy. This results in changes both in the way businesses operate and in customer expectations of products offered on the market. The use of modern ICT technologies has made it possible to create cyber-physical systems based on intelligent machines and devices that communicate with each other in real time and allow the integration of resources from different companies to carry out joint production projects. Today's consumer expects products tailored to their needs and expectations. These expectations can be met by leveraging the potential of highly specialized manufacturing service companies centered around e-business platforms. The article presents the results of research using bibliometric analysis and the results of surveys conducted among small and medium-sized enterprises. The concept of e-business platforms supporting rapid prototyping of temporary networks of companies capable of manufacturing personalized products in the environment of Industry 4.0 is presented. The task of the platform is to integrate a customer expecting personalized production with a network of companies having adequate production resources.</td>
      <td>3</td>
      <td>2021</td>
      <td>Energy &amp; Fuels</td>
      <td>The Concept of Cyber-Physical Networks of Small and Medium Enterprises under Personalized Manufacturing The era of Industry 4.0 is characterized by the use of new telecommunications ICT technologies and networking of the economy. This results in changes both in the way businesses operate and in customer expectations of products offered on the market. The use of modern ICT technologies has made it possible to create cyber-physical systems based on intelligent machines and devices that communicate with each other in real time and allow the integration of resources from different companies to carry out joint production projects. Today's consumer expects products tailored to their needs and expectations. These expectations can be met by leveraging the potential of highly specialized manufacturing service companies centered around e-business platforms. The article presents the results of research using bibliometric analysis and the results of surveys conducted among small and medium-sized enterprises. The concept of e-business platforms supporting rapid prototyping of temporary networks of companies capable of manufacturing personalized products in the environment of Industry 4.0 is presented. The task of the platform is to integrate a customer expecting personalized production with a network of companies having adequate production resources.</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Shen, Lei; Sun, Cong; Ali, Muhammad</td>
      <td>Role of Servitization, Digitalization, and Innovation Performance in Manufacturing Enterprises</td>
      <td>SUSTAINABILITY</td>
      <td>Article</td>
      <td>servitization; digitalization; dynamic capabilities; enterprise innovation performance; PSM-DID</td>
      <td>The structure of the manufacturing industry has forced manufacturing companies to understand the importance of digitalization and servitization transformation, in terms of production and R&amp;D. In this study, we examine the relationship between servitization, digitization, and enterprise innovation performance through the lens of dynamic capabilities within enterprises. We also discuss the impact of the transformation servitization strategy on business innovation, and the mechanisms by which it impacts business innovation performance. The study's findings indicate that servitization significantly contributes to innovation performance, and digitalization acts as a mediating mechanism between the proposed relationships. Thus, this article argues for the integration and growth of servitization and digitization.</td>
      <td>3</td>
      <td>2021</td>
      <td>Green &amp; Sustainable Science &amp; Technology; Environmental Sciences; Environmental Studies</td>
      <td>Role of Servitization, Digitalization, and Innovation Performance in Manufacturing Enterprises The structure of the manufacturing industry has forced manufacturing companies to understand the importance of digitalization and servitization transformation, in terms of production and R&amp;D. In this study, we examine the relationship between servitization, digitization, and enterprise innovation performance through the lens of dynamic capabilities within enterprises. We also discuss the impact of the transformation servitization strategy on business innovation, and the mechanisms by which it impacts business innovation performance. The study's findings indicate that servitization significantly contributes to innovation performance, and digitalization acts as a mediating mechanism between the proposed relationships. Thus, this article argues for the integration and growth of servitization and digitization.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Yang, Shuili; Yi, Yang</td>
      <td>Effect of Technological Innovation Inputs on Global Value Chains Status</td>
      <td>JOURNAL OF GLOBAL INFORMATION MANAGEMENT</td>
      <td>Article</td>
      <td>GVC Embedding Position; Industrial Value-Added; Industrialization; Integration of Information; Technological Innovation</td>
      <td>Under the backdrop of the continuous escalation of the Sino-U.S. trade friction, China's industrial development environment in global value chains (GVCs) has further deteriorated, research on the improvement GVC status of the Chinese manufacturing industry has become the focus of attention for industry and academia. The direction of R&amp;D inputs are of utmost importance to the improvement of GVC status. However, comparatively little attention has been paid to this topic in existing studies. Following the production activity decomposition framework and combining with the World Input-Output Tables, the action mechanism of R&amp;D inputs on GVC status from two aspects of industrial value-added and embedding position were analyzed, the moderating effect of digital servitization was demonstrated. Results show that: The input of applied research has inverted U-shaped influence on the industrial value-added, while it has U-shaped influence on embedding position; The input of basic research has U-shaped influence on the industrial value-added, while it has inverted U-shaped influence on embedding position; The moderating effect of digital servitization between R&amp;D inputs and GVC status is significant, in the short term, the digital servitization can not only magnify the promotion effect of the applied research inputs on GVC status, but also shorten the lag period of basic research inputs on GVC status. In the long run, the digital servitization can not only weaken the marginalization trend of the applied research inputs on GVC status, but also enhance the positive feedback effect of basic research inputs on GVC status. This study is important for China to improve the GVC status.</td>
      <td>1</td>
      <td>2021</td>
      <td>Information Science &amp; Library Science</td>
      <td>Effect of Technological Innovation Inputs on Global Value Chains Status Under the backdrop of the continuous escalation of the Sino-U.S. trade friction, China's industrial development environment in global value chains (GVCs) has further deteriorated, research on the improvement GVC status of the Chinese manufacturing industry has become the focus of attention for industry and academia. The direction of R&amp;D inputs are of utmost importance to the improvement of GVC status. However, comparatively little attention has been paid to this topic in existing studies. Following the production activity decomposition framework and combining with the World Input-Output Tables, the action mechanism of R&amp;D inputs on GVC status from two aspects of industrial value-added and embedding position were analyzed, the moderating effect of digital servitization was demonstrated. Results show that: The input of applied research has inverted U-shaped influence on the industrial value-added, while it has U-shaped influence on embedding position; The input of basic research has U-shaped influence on the industrial value-added, while it has inverted U-shaped influence on embedding position; The moderating effect of digital servitization between R&amp;D inputs and GVC status is significant, in the short term, the digital servitization can not only magnify the promotion effect of the applied research inputs on GVC status, but also shorten the lag period of basic research inputs on GVC status. In the long run, the digital servitization can not only weaken the marginalization trend of the applied research inputs on GVC status, but also enhance the positive feedback effect of basic research inputs on GVC status. This study is important for China to improve the GVC status.</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1559</th>
      <td>Niu, Xiaojing; Qin, Shengfeng</td>
      <td>Integrating crowd-/service-sourcing into digital twin for advanced manufacturing service innovation</td>
      <td>ADVANCED ENGINEERING INFORMATICS</td>
      <td>Article</td>
      <td>Digital twin; Digital thread; Internet of Beings (IoB); Service crowdsourcing; Smart product-service ecosystem; System intelligence</td>
      <td>In order to support advanced collaborations among smart products, services, users and service providers in a smart product and service ecosystem (S-PSS), this paper proposed a service-oriented hybrid digital twin (DT) and digital thread platform-based approach with embedded crowd-/service-sourcing mechanism for enabling advanced manufacturing services. This approach is well supported by the ecosystem interaction intelligence of digitally connected products, services, users, and service providers via Internet of Beings (IoB) (Things, Users and Service providers). First, driven by industrial application needs in heating industry, a conceptual model of the service-oriented hybrid platform integrated with crowdsourcing mechanism is developed, which supports the concepts of product DT, service DT and human user DT. Second, the key system realization techniques are developed to integrate service crowdsourcing and service recommendation for realizing smart services. Finally, a case study is carried out for evaluating and confirming its feasibility.</td>
      <td>0</td>
      <td>2021</td>
      <td>Computer Science, Artificial Intelligence; Engineering, Multidisciplinary</td>
      <td>Integrating crowd-/service-sourcing into digital twin for advanced manufacturing service innovation In order to support advanced collaborations among smart products, services, users and service providers in a smart product and service ecosystem (S-PSS), this paper proposed a service-oriented hybrid digital twin (DT) and digital thread platform-based approach with embedded crowd-/service-sourcing mechanism for enabling advanced manufacturing services. This approach is well supported by the ecosystem interaction intelligence of digitally connected products, services, users, and service providers via Internet of Beings (IoB) (Things, Users and Service providers). First, driven by industrial application needs in heating industry, a conceptual model of the service-oriented hybrid platform integrated with crowdsourcing mechanism is developed, which supports the concepts of product DT, service DT and human user DT. Second, the key system realization techniques are developed to integrate service crowdsourcing and service recommendation for realizing smart services. Finally, a case study is carried out for evaluating and confirming its feasibility.</td>
    </tr>
    <tr>
      <th>1560</th>
      <td>Chen, Wei; Chen, Yinzhong; Hao, Yifei; Chen, Sili</td>
      <td>Producer Services Openness and the Development of Servitization: The Perspective of Two-Way Openness</td>
      <td>DISCRETE DYNAMICS IN NATURE AND SOCIETY</td>
      <td>Article</td>
      <td>NaN</td>
      <td>This paper brings producer services "bringing in" and "going out" into the same analytical framework and explains the influence mechanism of producer services opening on the development of servitization from three aspects of import trade, FDI, and OFDI. On this basis, using the latest input-output data of WIOD, this paper constructs some indicators to measure the openness of producer services such as import trade penetration, FDI penetration, and OFDI penetration and then empirically tests the impact of producer services openness on the development of servitization in China. The results show that the openness of producer services has a significant positive impact on the development of China's servitization. In addition, the robustness analysis based on variable substitution and different estimation methods shows that the conclusions are robust. The heterogeneity test shows that the impact of producer services openness on servitization has heterogeneity. The specific performance is as follows: there is different impact of producer service sector openness on the development of servitization; the impact of producer service openness on the development of servitization with different factor intensities is also different; and there is also different impact of producer service sector openness on the development of servitization with different factor intensities. The policy implications of these research conclusions are as follows: firstly, taking co-construction of the "Belt and Road" as a chance to promote the new open pattern; secondly, focusing on expanding the openness of high-end producer services; and thirdly, taking innovation driven development as the guide to increase R&amp;D investment of producer services.</td>
      <td>1</td>
      <td>2021</td>
      <td>Mathematics, Interdisciplinary Applications; Multidisciplinary Sciences</td>
      <td>Producer Services Openness and the Development of Servitization: The Perspective of Two-Way Openness This paper brings producer services "bringing in" and "going out" into the same analytical framework and explains the influence mechanism of producer services opening on the development of servitization from three aspects of import trade, FDI, and OFDI. On this basis, using the latest input-output data of WIOD, this paper constructs some indicators to measure the openness of producer services such as import trade penetration, FDI penetration, and OFDI penetration and then empirically tests the impact of producer services openness on the development of servitization in China. The results show that the openness of producer services has a significant positive impact on the development of China's servitization. In addition, the robustness analysis based on variable substitution and different estimation methods shows that the conclusions are robust. The heterogeneity test shows that the impact of producer services openness on servitization has heterogeneity. The specific performance is as follows: there is different impact of producer service sector openness on the development of servitization; the impact of producer service openness on the development of servitization with different factor intensities is also different; and there is also different impact of producer service sector openness on the development of servitization with different factor intensities. The policy implications of these research conclusions are as follows: firstly, taking co-construction of the "Belt and Road" as a chance to promote the new open pattern; secondly, focusing on expanding the openness of high-end producer services; and thirdly, taking innovation driven development as the guide to increase R&amp;D investment of producer services.</td>
    </tr>
    <tr>
      <th>1561</th>
      <td>Sun, Xinbo; Zhang, Qingqiang</td>
      <td>Building digital incentives for digital customer orientation in platform ecosystems</td>
      <td>JOURNAL OF BUSINESS RESEARCH</td>
      <td>Article</td>
      <td>Customer orientation; Digital customer orientation; Digital incentives; Incentives; Platform ecosystem</td>
      <td>How does customer orientation turn to digital customer orientation in the platform ecosystem? To this end, in this qualitative study, we tracked four Chinese companies to explain this shift and examined the role of digital incentives in the transition. Our findings reveal two distinct patterns of digital customer orientation, namely digital customer orientation complement and refinement, a difference that is essentially triggered by differences in the establishment time of focal firms in the platform ecosystem. The study also disentangles the dynamic process of shifting from traditional customer orientation to digital customer orientation in platform ecosystems and shows that digital incentives incentive orchestration, incentive decentralization, and digital facilitation can be instrumental in accelerating this process. Important management implications suggest that different digital customer orientation patterns are choices that companies make based on the environment, which need to be jointly embraced by the participants in the platform ecosystem.</td>
      <td>1</td>
      <td>2021</td>
      <td>Business</td>
      <td>Building digital incentives for digital customer orientation in platform ecosystems How does customer orientation turn to digital customer orientation in the platform ecosystem? To this end, in this qualitative study, we tracked four Chinese companies to explain this shift and examined the role of digital incentives in the transition. Our findings reveal two distinct patterns of digital customer orientation, namely digital customer orientation complement and refinement, a difference that is essentially triggered by differences in the establishment time of focal firms in the platform ecosystem. The study also disentangles the dynamic process of shifting from traditional customer orientation to digital customer orientation in platform ecosystems and shows that digital incentives incentive orchestration, incentive decentralization, and digital facilitation can be instrumental in accelerating this process. Important management implications suggest that different digital customer orientation patterns are choices that companies make based on the environment, which need to be jointly embraced by the participants in the platform ecosystem.</td>
    </tr>
    <tr>
      <th>1562</th>
      <td>Zabala, Kristina; Antonio Campos, Jose; Narvaiza, Lorea</td>
      <td>Moving from a goods- to a service-oriented organization: a perspective on the role of corporate culture and human resource management</td>
      <td>JOURNAL OF BUSINESS &amp; INDUSTRIAL MARKETING</td>
      <td>Article; Early Access</td>
      <td>Servitization; Customization; Service infusion; Case study; Service climate</td>
      <td>Purpose This study aims to investigate the internal elements that help in the introduction of a service logic into a goods-oriented organization by focusing on corporate culture and human resource management (HRM) practices. Design/methodology/approach The study uses a qualitative single case study research design. Data have been collected through archival data and 14 semi-structured interviews to managers, employees and retailers of a bike manufacturer. Findings The research identifies the following three new internal elements affecting the service orientation of corporate culture of a company with a customization strategy: shared vision built up with the participation of the whole organization; rooting the service orientation into the past history; passion and collaborative study deployed through digital tools. Additionally, related to HRM, the research finds another two elements: emotional salary and that a collective way of understanding and sharing the service infusion is needed. Research limitations/implications Given that this is a qualitative research based on a single case study the identified key elements of corporate culture and HRM practices cannot be used as a predictive tool. However, the depth of evidence is significant and allows analytical generalizations, which enable us to put forward tentative propositions for future research. Practical implications For managers of industrial firms, the identified elements provide an insight on how to smooth the transition from goods-to service-oriented organization. The shift demands the development of an adequate corporate culture and distinctive management of human resources. Originality/value Building on previous literature, the research offers the academic community five new soft elements to be studied in the service infusion process and can guide top managers on how to engage the entire organisation in a service-oriented manner.</td>
      <td>0</td>
      <td>2021</td>
      <td>Business</td>
      <td>Moving from a goods- to a service-oriented organization: a perspective on the role of corporate culture and human resource management Purpose This study aims to investigate the internal elements that help in the introduction of a service logic into a goods-oriented organization by focusing on corporate culture and human resource management (HRM) practices. Design/methodology/approach The study uses a qualitative single case study research design. Data have been collected through archival data and 14 semi-structured interviews to managers, employees and retailers of a bike manufacturer. Findings The research identifies the following three new internal elements affecting the service orientation of corporate culture of a company with a customization strategy: shared vision built up with the participation of the whole organization; rooting the service orientation into the past history; passion and collaborative study deployed through digital tools. Additionally, related to HRM, the research finds another two elements: emotional salary and that a collective way of understanding and sharing the service infusion is needed. Research limitations/implications Given that this is a qualitative research based on a single case study the identified key elements of corporate culture and HRM practices cannot be used as a predictive tool. However, the depth of evidence is significant and allows analytical generalizations, which enable us to put forward tentative propositions for future research. Practical implications For managers of industrial firms, the identified elements provide an insight on how to smooth the transition from goods-to service-oriented organization. The shift demands the development of an adequate corporate culture and distinctive management of human resources. Originality/value Building on previous literature, the research offers the academic community five new soft elements to be studied in the service infusion process and can guide top managers on how to engage the entire organisation in a service-oriented manner.</td>
    </tr>
    <tr>
      <th>1563</th>
      <td>Capello, Roberta; Lenzi, Camilla</td>
      <td>Industry 4.0 and servitisation: Regional patterns of 4.0 technological transformations in Europe</td>
      <td>TECHNOLOGICAL FORECASTING AND SOCIAL CHANGE</td>
      <td>Article</td>
      <td>Technological transformations; Industry 4; 0; Servitisation; Regional transformation patterns</td>
      <td>Servitisation is usually associated to Industry 4.0, as it is conceived as the bundling of products and services by manufacturing firms, with the aim to compete on the market. This paper, instead, separates out the two concepts, claiming that, notwithstanding certain areas of overlap, these transformations involve different actors, different sources of value creation and deeply affect the economy and society with differentiated spatial development patterns. The territorial dimension of these transformations has been so far neglected in the literature. Instead, where these technological transformations take place is important, since they are sources of new growth opportunities as well as of new interregional inequalities. This paper aims at conceptually providing an operational definition of the different technological transformations, and empirically identifying them in the European territory. On conceptual grounds, the paper elaborates on why and in which territorial contexts these transformations are most likely to take place and, on empirical grounds, the paper documents the existence of such transformations in European NUTS-2 region over the period 2008-2016.</td>
      <td>1</td>
      <td>2021</td>
      <td>Business; Regional &amp; Urban Planning</td>
      <td>Industry 4.0 and servitisation: Regional patterns of 4.0 technological transformations in Europe Servitisation is usually associated to Industry 4.0, as it is conceived as the bundling of products and services by manufacturing firms, with the aim to compete on the market. This paper, instead, separates out the two concepts, claiming that, notwithstanding certain areas of overlap, these transformations involve different actors, different sources of value creation and deeply affect the economy and society with differentiated spatial development patterns. The territorial dimension of these transformations has been so far neglected in the literature. Instead, where these technological transformations take place is important, since they are sources of new growth opportunities as well as of new interregional inequalities. This paper aims at conceptually providing an operational definition of the different technological transformations, and empirically identifying them in the European territory. On conceptual grounds, the paper elaborates on why and in which territorial contexts these transformations are most likely to take place and, on empirical grounds, the paper documents the existence of such transformations in European NUTS-2 region over the period 2008-2016.</td>
    </tr>
  </tbody>
</table>
<p>1564 rows × 10 columns</p>
</div>




```python
paper_data = paper_data.drop(['Title', 'Abstract'], axis=1)
paper_data.columns
```




    Index(['Authors', 'Source', 'Type', 'Keywords', 'Citation_Num', 'Year',
           'WoS Research Area', 'Title+Abstract'],
          dtype='object')




```python
# Title+Abstract의 결측치가 있는 항목은 모두 제거
paper_data = paper_data[paper_data['Title+Abstract'].notnull()].reset_index(drop=True)
paper_data.shape

```




    (1558, 8)




```python
from sklearn.feature_extraction.text import TfidfVectorizer
```


```python
# 불용어 제거
tfidf = TfidfVectorizer(stop_words='english')

# Title+Abstract에 대해서 TF-IDF 수행
tfidf_matrix = tfidf.fit_transform(paper_data['Title+Abstract'])
print(tfidf_matrix)
```

      (0, 2328)	0.04584379765006109
      (0, 9926)	0.0277955381966585
      (0, 5130)	0.0734741836491098
      (0, 5633)	0.038645223694858795
      (0, 4656)	0.040861755118726584
      (0, 3916)	0.0637740977859976
      (0, 4265)	0.09245782610392976
      (0, 6183)	0.031153725024521767
      (0, 1253)	0.10691575975175359
      (0, 9565)	0.11289252646373243
      (0, 7909)	0.026334454187898204
      (0, 6246)	0.09938592420561841
      (0, 4628)	0.05493537037546309
      (0, 6409)	0.05493537037546309
      (0, 2429)	0.08530118822377084
      (0, 9081)	0.03831396210392591
      (0, 9420)	0.05395696731526885
      (0, 2773)	0.04600061317620444
      (0, 501)	0.053039483289307146
      (0, 3207)	0.10691575975175359
      (0, 4419)	0.06515272055536442
      (0, 7985)	0.03171114445272064
      (0, 4800)	0.05080357352997789
      (0, 2941)	0.06574242000776945
      (0, 4712)	0.0637740977859976
      :	:
      (1557, 539)	0.06891348985600991
      (1557, 2131)	0.06690816767811368
      (1557, 3239)	0.06891348985600991
      (1557, 6604)	0.10468063166250889
      (1557, 3864)	0.03975304840160756
      (1557, 6738)	0.06891348985600991
      (1557, 2703)	0.032710260163856274
      (1557, 9255)	0.17863979219568663
      (1557, 4280)	0.058571818869038166
      (1557, 2753)	0.11243402187999664
      (1557, 4801)	0.07495601458666443
      (1557, 5663)	0.032325268302755165
      (1557, 5683)	0.04584105006570991
      (1557, 4665)	0.046051274079925986
      (1557, 8643)	0.06802231140977254
      (1557, 3106)	0.05032641005235715
      (1557, 6836)	0.15765886246989402
      (1557, 602)	0.05637220909073568
      (1557, 2328)	0.04849001020047712
      (1557, 9926)	0.029399962476318237
      (1557, 6183)	0.06590398359896377
      (1557, 5500)	0.035855659225937996
      (1557, 3238)	0.04662613654734177
      (1557, 8391)	0.029327388048096866
      (1557, 7189)	0.03479219860490983
    


```python
from sklearn.metrics.pairwise import cosine_similarity
cosine_matrix = cosine_similarity(tfidf_matrix, tfidf_matrix)
cosine_matrix.shape
```




    (1558, 1558)




```python
np.round(cosine_matrix, 4)
```




    array([[1.    , 0.0297, 0.0375, ..., 0.0377, 0.0604, 0.0115],
           [0.0297, 1.    , 0.108 , ..., 0.0604, 0.0308, 0.0397],
           [0.0375, 0.108 , 1.    , ..., 0.0646, 0.0261, 0.0274],
           ...,
           [0.0377, 0.0604, 0.0646, ..., 1.    , 0.1058, 0.0261],
           [0.0604, 0.0308, 0.0261, ..., 0.1058, 1.    , 0.0094],
           [0.0115, 0.0397, 0.0274, ..., 0.0261, 0.0094, 1.    ]])




```python
# keywords와 id를 매핑할 dictionary 생성
keyword2id = {}
for i, c in enumerate(paper_data['Keywords']): keyword2id[i] = c
    
# id와 keywords를 매핑할 dictionary 생성
id2keyword = {}
for i, c in keyword2id.items(): id2keyword[c] = i
```


```python
keyword2id
```




    {0: 'Digitalization; Company performance; Listed company; Benefit; Obstacle',
     1: 'outsourcing; sustainable development; core competencies; company management',
     2: 'cyber-physical networks; Industry 4; 0; small and medium enterprises; personalization; servitization',
     3: 'servitization; digitalization; dynamic capabilities; enterprise innovation performance; PSM-DID',
     4: 'GVC Embedding Position; Industrial Value-Added; Industrialization; Integration of Information; Technological Innovation',
     5: nan,
     6: 'Industry studies; wood manufacturing; performance; value-added strategy; international competition',
     7: 'Digitalization; Inertia; Entrepreneurial orientation; Firm assets; Organizational legitimacy; Financial services',
     8: 'Value co-creation; Manufacturing firms; B2B markets; Digital servitization; Viable system approach',
     9: "Service transition; Top management team; Informational faultlines; Social faultlines; Tobin's Q",
     10: 'Servitization dynamics; Game theory; Oil industry; Business relationships; Mixed methods',
     11: 'Digital finance; Manufacturing industry; Servitization; Servitizing transformation',
     12: 'Service design; Creativity; Innovation; Digital servitization; Product-service-software systems (PSS); Digitalization; Routines; Institutional work theory',
     13: 'product-service innovation; multiple levels; theory integration; mixed methods; action research',
     14: 'industry servitization; global value chains; economic policy; new business models',
     15: 'human centric lighting; eco-science; manufacturing servitization; service innovation',
     16: 'Digitalization; E-commerce; Strategic development; Servitization; Third party logistics (TPL)',
     17: 'Servitization; Deservitization; History-based management theory; Industry lifecycle; Strategic pivoting; Strategy restoration',
     18: 'Industry 4; 0; SMEs; External knowledge search; Opportunity recognition; Technology adoption',
     19: 'India; Supply chain management; Green supply chains; Suppliers; Automotive industry; Global value chain',
     20: 'Servitization; International configuration; Centralization; Service characteristics; Industrial services; Digitalization',
     21: 'Deindustrialization; tertiarization; inter and intra sectorial links; structural changes',
     22: 'lodging sector; hotels; digital transformation; digital innovations',
     23: 'construction product; servitization; evidence-based design; level of evidence; cognitive buildings',
     24: 'Industry 4; 0; industrial IoT; human-cyber-physical systems; smart manufacturing; operations and services; smart and connected products; design engineering 4; 0; design automation; design for manufacturing; design integration; design methodology',
     25: 'energy services; energy transition; customer value proposition; servitization; framework; end use system; human system; service design',
     26: 'Service innovation; Digitalization; Servitization; Manufacturing companies; Medical device manufacturer; Digital servitization',
     27: 'Service inputs; Eco-efficiency; Manufacturing industries; Input-output linkage; U-shaped relationship',
     28: 'Internet of things; Supply chain management; Business ecosystems',
     29: 'Digital Servitization; Industry 4; 0; Prior Knowledge; Disruptive Technologies; Italy',
     30: 'Market entry; strategy; servitisation; private leasing; carsharing; sharing economy; disruptive innovation; ecosystem',
     31: 'Internet of things(IoT); Circular economy; Pythagorean fuzzy sets; Manufacturing sector; Fuzzy sets; Decision making; SWARA; CoCoSo',
     32: 'Process innovation; Process industry; Ecosystems; Ecosystem strategy; Coopetition; Digitalization; Digital servitization; IoT',
     33: 'Artificial intelligence; Digital servitization; Digital transformation; Digitalization; Business model innovation; Platform',
     34: 'Digitalization; Digital servitization; Dynamic capabilities; Value co-creation',
     35: 'Customer success management; Value in use; Hybrid offerings; Servitization; Solution; business',
     36: 'Responsible sourcing; Climate change; Net zero; Carbon neutral; Mining',
     37: 'Digital servitization; Business model innovation; Digital business model; Value creation; Value driver; Servitization; Digitalization',
     38: 'sustainability accounting; manufacturing; digitalization; triple layered business model canvas',
     39: 'stages of growth; SMEs; micro-sized enterprises; technology-based firm; service-based firm',
     40: 'RCA; CRCA; VWRCA; servitization; methods; Poland; global economy',
     41: 'Servitization; Organisational boundary; Servitization challenges',
     42: 'Servitization; Servitization level; Firm performance; Measurement; Servitization paradox; Configurational analysis',
     43: 'Servitization; Digitalization; Interaction; Manufacturing firms; Market performance',
     44: 'Complexity; Servitization; Digital; Service operations',
     45: 'servitization; service&#8208; oriented manufacturing; product&#8208; service system; capacity allocation; Markov decision process',
     46: 'Servitization; Global value chains (GVCs); Structure; Governance',
     47: 'Servitisation; Modularity; Paradox theory; Advanced services; Product upgrade services; Firm boundary; Boundary negotiation',
     48: 'Multi-dimension; ambidextrous open innovation; the 4th Industrial Revolution; financial sector',
     49: 'Entrepreneurial ecosystem; Business competitiveness; Benefit of the doubt model; KIBS firms; Manufacturing firms; International comparison',
     50: 'digitalization; smart services; innovation flexibility; cooperation; SMEs; electrical engineering',
     51: 'gig economy; platform economy; science mapping; WoS; servitization; digital economy',
     52: 'Digital servitization; Business model innovation; Sustainability; Networks',
     53: 'big data; action research; decision-making; value proposition; value co-creation; digital twins',
     54: 'Dependency Graph; IoT; Path Traversal Sequence; QoS; Service Composition; Top-K',
     55: 'healthcare manufacturing firms; organizational resilience; COVID-19; servitization; digitalization',
     56: 'Scale development; Eco-innovation; Natural resource-based view; Sustainable development commitment',
     57: 'Manufacturing; Industry 4; 0; Digital transformation; Digital servitization; Multilevel theory',
     58: 'Maintenance; Decision-making; Continuous improvement; Service operations; Servitization',
     59: 'Servitization; fsQCA; Experiments; Causal explanation; Case study',
     60: 'change management; circular economy; organizational behaviour; organizational culture; sustainability; sustainable development',
     61: 'Bundling; Innovation; Export; Servitization; SMEs',
     62: 'Maintenance; Servitization; Contract pricing; Predictive analytics; Calibration',
     63: 'game theory; modular production; OEM supply chain; service mode; servitization',
     64: 'Servitization; Case study; Consumer products; Business-to-consumer; Triads; Circular economy; Sustainability',
     65: 'Industry 4; 0; Smart Manufacturing; Smart Products and Services; Smart Working; Smart Supply Chain',
     66: 'Servitization; Topic modeling; Narratives; Literature review',
     67: 'Machine learning; Deep learning; Artificial intelligence; Artificial neural networks; Analytical model building',
     68: 'Smart products; Monitoring capabilities; Autonomous solutions; Servitization; SMEs',
     69: 'Servitization; supply chain integration; basic services; advanced services; supplier integration; customer integration',
     70: 'small and medium-sized ports; comprehensive ports; port ecosystem; European Green Deal; strategic management; environmental and digital transition; sustainable ports; green ports',
     71: 'Manufacturing; Cultural differences; Human resource management; Complexity theory; Technological innovation; Manufacturing industries; Cultural factor; firm performance; organizational design factors; servitization; service paradox',
     72: 'Smart products; Feature fatigue; Technology; Digital servitization; Optimal control',
     73: 'Product-service innovation; IT processes; R&D teams; Centralization; MIMIC',
     74: 'Business model innovation; china; dynamic capabilities; e&#64259; ciency-centred business model theme; manufacturing servitization barrier; novelty-centred business mode theme',
     75: 'knowledge-intensive business service (KIBS) sector; technology-based knowledge-intensive business service (T-KIBS); related diversification; territorial servitization; Fourth Industrial Revolution',
     76: 'Outcome-based contracts; Servitization; Solution selling; Means-end-chain analysis; Laddering',
     77: 'dual innovation; innovation performance; productization; servitization; singular innovation',
     78: 'Digital servitization; Business-model modularity; Product-service-software trajectories',
     79: 'Revenue models; Pacific Asia; digital servitization; smart services; value capture; value-based pricing',
     80: 'Circular economy; Condition-based maintenance; Digitalization; Internet of things; Case study',
     81: 'Creative industries; identity; occupational communities; servitization; video games',
     82: 'Organisational design theory; Organisational structure; Product-orientation; Service orientation; Project-service system; Servitisation',
     83: 'product&#8211; service innovation; servitization; sustainability; performance; product lifecycle',
     84: 'functional specialisation; gross export decomposition; occupations; economic upgrading; CEE countries',
     85: 'hybridity; territorial servitization; regional development; SMEs; innovation; dynamic capabilities; absorptive capabilities; collaboration; southern Germany',
     86: 'innovation strategies; productivity; environmental impact; manufacturing; size',
     87: 'Value added of services; Manufacturing exports; Network analysis; Servitization; PageRank algorithm',
     88: 'Servitization; Repurchase intention; Platform service supply chain; Food apps',
     89: 'Online consumer behavior; Customer value; Services marketing; Financial services; e-commerce; Consumer behaviour internet; Service quality; Information technology; Eservice quality; Blogs; Digitalizations',
     90: 'Information processing; Cognition; Task analysis; Manufacturing; Social networking (online); Complexity theory; Uncertainty; Culture; customer loyalty; information processing; information technology; servitization; social media',
     91: 'Companies; Business; Manufacturing; Pricing; Solid modeling; Context modeling; Technological innovation; Advanced services; business models; digital services; digital servitization; digitalization; revenue model; servitization',
     92: 'Internet of Things; business models; smart cities; big data; consumer data',
     93: 'Ecosystem innovation; Dynamic capabilities; Smart cities; Digitalization; Digital servitization',
     94: 'sharing economy; product launching; pricing strategy; sharing service transformation',
     95: 'rural economic development; shift-share analysis; industrial structure; competitive effect; China',
     96: 'New product development; Innovativeness; Top management service commitment; Dysfunctional competition; NPD speed',
     97: 'Business models innovation; Artificial intelligence; Digitalization; Ecosystem innovation; Value creation; Strategy',
     98: 'E-commerce; E-supply chain; Resource-based view; Dynamic capabilities; Fashion industry',
     99: 'Servitization; Tire manufacturing firm; Financial performance; Channel conflict; Service paradox; Cannibalization',
     100: 'Servitization; business-to-business; solution business models; product lifespan; competitive performance',
     101: 'Servitization; Resource Assignment Problem; Workers Assignment Problem; Metaheuristic Optimization; Whale Optimization Algorithm; Flower Pollination Algorithm',
     102: 'digital innovation; literature network analysis; business model innovation; digitisation; business model change; digital transformation; structured literature review; case study research; servitisation; digital business strategy',
     103: 'Discourse; Descending hierarchical classification; Theorization; Hybrid organizing; Hybrid organizations',
     104: 'prescriptive maintenance; sustainable business models innovation; Industry 4.0; technology systems; distributed ledger technologies; digital twin',
     105: 'Lean bundles; Servitization of manufacturing; Complementarity; Sustainable performance; Survey',
     106: 'business ecosystem; ecosystem-based business; collaboration; customer value; value co-creation; service; service business; servitisation; software industry; business model',
     107: 'Analytic hierarchy process (AHP); key factors; manufacturing servitization; industrial service innovation; service design methodology',
     108: nan,
     109: nan,
     110: 'information technology; lean supply chain; LSC; agile supply chain; ASC; SciMAT; science mapping; bibliometrics; literature review; co-citation',
     111: 'Servitization; Service Business Model; Industry 4.0; Digitalization; Service oriented',
     112: 'Platform; Value co-creation; Co-evolution; Orchestration capability',
     113: 'High-end equipment manufacturing; 4S; grey relational degree; servitization',
     114: 'Business model innovation; Digital ecosystem; Multi-sided platform; Digital technology; TRIZ',
     115: 'Servitization; Buyer-seller relationships; Delphi method; Tension; Territoriality; Oil gas industry',
     116: 'SCM; orientation; collaboration; performance',
     117: 'Strategic ambidexterity; Product-service innovation; Performance; Manufacturing multinational enterprises',
     118: 'Hidden costs; Performance based contracts; Case based research; Servitization; Multi actor systems; Engagement; S-D logic; Agency theory',
     119: 'digital economy; social perspective; social business model; railway sector',
     120: nan,
     121: 'Digital servitization; Risk management; Business model innovation; Industry 4; 0; Digitalization paradox',
     122: 'circular economy; household appliances; case studies; sustainable development; reduce; reuse; remanufacture; recycle; circular benefits',
     123: 'business model; circular economy; circular business model; product as service model; green innovation',
     124: 'Value-in-use management; Value in use; Hybrid offerings; Servitization; Solution business',
     125: 'Servitization; Supply chain integration; Firm performance; Empirical research; International manufacturing strategy survey',
     126: 'servitization; servitization context; capabilities; service-oriented strategy; case study',
     127: 'Solution business; Signaling theory; Experiment; Servitization',
     128: 'IT exploration; IT exploitation; Service innovation; Cross-functional integration; Manufacturing firms',
     129: 'Nonlinear evolutionary problem; knowledge governance; boundary-spanning search; machine learning',
     130: 'Industry 4; 0; digital transformation; manufacturing; framework; journey',
     131: 'Multi-scale; Intelligent manufacturing; Interconnected chemical engineering',
     132: 'Entrepreneurship; Small-medium enterprises; Startups; Entrepreneurial ecosystem',
     133: 'Capabilities; Performance; Manufacturing; Servitization; EMS',
     134: 'Industry 4.0; Innovation ecosystem; Technology providers; SMEs',
     135: 'Digitally connected services; Improvements; Customer-initiated feedback',
     136: 'Internet of Things; Cloning; Digital twin; Software engineering; Solid modeling; Software architecture; Manufacturing processing; Artificial intelligence (AI); business models; cyber physical systems (CPSs); digital twin (DT); Internet of Things (IoT); machine learning (ML); multiagent systems; network function virtualization; sensors; servitization; smart city; software architecture; softwarization; virtual and augmented reality',
     137: 'mobility as a service; technology acceptance model; mobility behaviour; mobile services; technology adoption',
     138: 'Smart products; Business-to-business; Physicality',
     139: 'Servitization; Service innovation; Collaborative innovation projects; mICT',
     140: 'Servitization; Lean; Agile; Mass customization capability; Product innovation capability',
     141: 'Servitization; Organizational change; Organizational design factors; Culture; Firm performance',
     142: 'Social manufacturing; Resource configuration; Dynamic community; Multi-level Optimization',
     143: 'servitization; business process; service actors; service value; service process value model; process mining',
     144: 'value co-creation; technology and innovation management; mapping; bibliographic coupling analysis; open innovation; servitization; sharing economy',
     145: 'COVID-19 pandemic; customer solutions; performance-based contracting; servitization',
     146: 'Digital transformation; Digitalization; Innovative business models; Digital ecosystem; f and b industry',
     147: 'Smart service; Platform; Interdisciplinary research; Manufacturing company; Smart service provider; Platform economics; Information systems; Multi-sided markets; Business-to-business (B2B) markets',
     148: 'Manufacturing strategy; Human capital; Labour productivity; Product-service innovation; Size',
     149: 'OSA&#8211; CBM; rolling element bearing fault; the internet of things; arrowhead framework',
     150: 'Microfoundations; Servitization; Motives; Value strategies; Small firms; Abduction',
     151: 'Digital Transformation; Digitalization; Digitization; Manufacturing; Literature Review',
     152: 'circular economy; business models; paradoxical tensions',
     153: 'Servitization; Strategy; Accounting tools; Accounting machine; Pragmatic constructivism',
     154: 'Manufacturing; Multi-employer worksite; Safety management; Servitization',
     155: 'Servitization; Industry 4.0; Goods-Services Dichotomy; Goods-Services Convergence; Dual Nature; Hybrid Measures; Domestic Regulation; Legal Silos',
     156: 'IoT; Advanced services; Servitization',
     157: 'RFM analysis; Industrial services; Servitization; CRM; Customer segmentation',
     158: 'transformative cultural tourism; post-industrial economy; servitization; paradigm innovation; rural development policy; business model',
     159: 'data-driven; cyber-physical-social system; social manufacturing; cyber-physical system',
     160: 'Circular economy; Battery leasing; Battery regulation; Circular economy in EV batteries; EV policy; Lithium ion batteries',
     161: 'Intellectual capital; Healthcare; Market access; Innovation process; Digital servitization; Value co-creation',
     162: 'Servitization; variability; modularity; additive manufacturing; supply chain management',
     163: 'Manufacturing servitization; carbon emission embodied in export; regional space; global value chain',
     164: 'Industry 4.0; Value chain; Scenario planning; Delphi study; Customization; Servitization',
     165: 'Innovation; Technological change; Industrial organization; Services; Animal nutrition; Animal health',
     166: 'Innovation; Service-dominant logic; Value proposition; Service ecosystem; Logistics innovation',
     167: 'IOT; Digitialization; Servitization; Business models; BtoB; Italy',
     168: 'Digital servitization; Digital transformation; Organizational culture; Agile mindset; Data-centric business model; Big data monetization',
     169: 'Customer experience management; Customer journey; Market strategy; B2B; Touchpoint',
     170: 'Shared manufacturing; Blockchain; Resource sharing; Smart contract; Socialization manufacturing',
     171: 'Banking; Qualitative research; Service innovation; Thematic analysis',
     172: 'digital revolution; Industry 4; 0; systems thinking',
     173: 'Partner selection; Platform-based innovation ecosystems; Servitization',
     174: 'Servitization; Uncertainty management; Engineering service; Engineering service development; Co-creation',
     175: 'Servitization; Advanced services; Competitive advantage; Manufacturing strategy',
     176: 'WWIC information services; maritime Arctic; co-production; expertise; servitization; user-producer interactions',
     177: 'Smart Products; Innovation ecosystem; Cooperation; Internet of things; Capabilities; Industry 4.0',
     178: 'Business-to-business; Cooperation; Coopetition; Product innovation; Service innovation',
     179: 'COVID-19; Coronavirus; Servitization; Digitalization; Service operations; Solutions; Resilience',
     180: 'Business model; Industry 4.0; Taxonomy; Patterns; Case study; Internet of things (IoT)',
     181: 'Business model; Servitization; Service centres; Top performer; Medium-heavy commercial vehicle industry',
     182: 'Servitization; digitalization; firm performance; structural equation modelling',
     183: 'Decision making process; Servitization; Risk occurrence',
     184: 'Industry 4.0; cyber-physical system; lean; six sigma; lean six sigma; fourth industrial revolution',
     185: 'supply chain relationships; integrated project delivery; integration',
     186: 'Wine tourism; Wine tourists; "Blue ocean" strategy; Visitors\' wine experience preferences',
     187: 'Manufacturing system; Social network; Finite state machine; Peer-to-peer network; Trie-based structure',
     188: 'Cloud manufacturing; Industrial revolution; Diffusion of innovation; Technology-organization-environment; Technology acceptance model; Advanced manufacturing technology',
     189: 'Fintech; Strategic alliance; Make; buy; or ally; Entrepreneurial finance; Banks',
     190: 'agility; business model innovation; digitalization; digital transformation; dynamic capabilities; ecosystems; services; strategy; value capture; value creation',
     191: 'socio-technical transition; sustainability transition; multilevel perspective; recycling; upcycling; downcycling; biorefinery; servitization; circular business model',
     192: 'CSR; Certification; Sustainability; Business logic; Real estate industry',
     193: 'Servitization; service delivery system; service triads; employee training; panel data',
     194: 'Value proposition architecture; Circular Economy; Innovation; Sustainability; Resource efficiency; Environmental',
     195: 'Ecology; Management framework; Sharing economy; Society; Sustainable practice; Urban',
     196: 'Small to medium sized enterprises; Financial performance; Servitization; Manufacturing companies; Non-financial performance; Small and medium enterprises; SMEs',
     197: 'customer satisfaction measurements; non-financial performance measurements; customer satisfaction information usage; improvements',
     198: 'Storytelling; Big data analytics; Smart service; Customer reference; Customer-supplier relationships',
     199: 'Digitization; Digitalization; Business model; Business-to-business marketing',
     200: 'Strategy; Servitization; Business platform; Copier; Technology platform',
     201: 'Servitization; Advanced services; Process; Transition; Transformation; Organisational change',
     202: nan,
     203: 'Agile development; innovation practices; solution development; new service development (NSD); solution business; servitization and digital servitization; open innovation',
     204: 'Circular economy; Consumer protection; Servitization; Information obligations',
     205: 'Servitization; Business Model Innovation; Digital Platforms; Corporate Entrepreneurship',
     206: 'Servitization; Case studies; Service operations; SMEs; Maturity model',
     207: 'Supply chain integration; food manufacturer; transformation performance; case study',
     208: 'services; growth drivers; innovation',
     209: 'Nature-inspired engineering; Nonequilibrium thermodynamics; Nonlinear control; Self-organization; Virtual modular factory',
     210: 'comparative analysis; enterprise performance; IT industry; product/service integration; service; Servitization of IT companies',
     211: 'Servitization; digitization; servitization paradox; publishing industry',
     212: 'Knowledge-intensive business services; African firms; Conditional knowledge; Contextual paradoxes; Paradoxes of learning; Relational knowledge',
     213: 'Industry 4; 0; strategy; manufacturing industry; manufacturing strategy; cyber technologies',
     214: 'Territorial servitization; county competitiveness; industry configuration; Costa Rica',
     215: 'product-service innovation; servitisation; digital capabilities; digitization; digital transformation; Spanish manufacturing companies; logistic regression; digital technological capabilities; advanced manufacturing technologies; information and communication technology; ICT',
     216: 'business-to-business relations; dynamic models; governance matching; governance mechanisms; services; servitization; solutions',
     217: 'servitisation; pay-per-use; earnings models; financialisation; industrial asset management; accountancy; financial entities; variants of capitalism',
     218: nan,
     219: 'additive manufacturing; 3D printing; digitalisation; servitisation; product-service innovation; case study',
     220: 'Servitization; Self-Reinforcing Mechanism; Regional Policies',
     221: 'cloud computing; servitisation; SMEs; cloud service provision; IT governance; cloud governance',
     222: 'servitization capacity; knowledge-intensive business service (KIBS); geographical distance',
     223: 'KIBS; Manufacturing; Innovation; Latin America; Firm Location',
     224: 'Servitization; SSPs; SSCs; machining and equipment manufacturers; digital transformation; Industrial services; IIoT; Artificial intelligence',
     225: 'Servitization; knowledge-intensive business services; innovation; typology',
     226: 'Services trade; Export sophistication; Manufacturing sector; Trade restrictiveness; Panel data analysis',
     227: 'Servitization; Service supply network; Tie strength; Structural holes; Economies of scope; Performance',
     228: 'Servitization; Customization; Structural equation modelling; Competition; Supply chain collaboration; Feature-based production capability',
     229: 'Servitization; transformation; organisational change; organisational context',
     230: 'Servitization; IoT; buyer-supplier relationships; manufacturing; services',
     231: 'Servitization; Financial performance; Dependence; Service providers; Relational embeddedness',
     232: 'Multi-sided platforms; Technological trajectories; Servitization; Platform evolution; Mobility services; Mobility platform',
     233: 'Flexibility; Internet of Things; Modularity; Canvas',
     234: 'Servitization; Case-based research; Home-country institutions',
     235: 'Solutions; pricing; value-based pricing; condition-based maintenance; buyer-supplier relationship',
     236: 'Business model innovation; Strategic response; Digital innovation; Incremental and radical innovation; Incumbents',
     237: 'Europe; Documentary; Logistics services; Logistics industry',
     238: 'Innovation; Servitization; Technological innovation',
     239: 'Servitization; Customer relations; Customer requirements',
     240: 'High performance work systems; Servitization; Service-providing capability; Environmental conditions',
     241: 'Servitization; Advanced services; Integrated solution; Integrated products and service; Transformation; Case study',
     242: 'Solutions; Servitization; Business networks; Interorganizational; Interdependence; Networks of solutions',
     243: 'Servitization; Service capability development; Internal service ecosystem; Front- and back-office',
     244: 'Service innovation; Dynamic capability; Ecosystem; Open innovation; Servitization; Energy utilities',
     245: 'Value; Servitization; Advanced services; Manufacturing',
     246: 'New service development; Servitization; Open innovation; Customer participation; Competitive intensity; Customer needs; Service innovation',
     247: 'Servitization; Business model; Practice; Change; Contestation',
     248: 'service intensity; new ventures; survival; liability of newness; resource-advantage theory',
     249: 'Digitalization; Servitization; Service ecosystem; Digitization; Centralization; Integration',
     250: 'Solution business; Digital platforms; Control; Network orchestration; B2B solutions; Modularity',
     251: 'lean; agile; servitization; digitalisation',
     252: 'Demand-side search; Market capability; Service organizing; Service-oriented HRM; Top management service commitment',
     253: 'Servitization; Risk pattern; Risk control; Decision making logic; Effectuation theory',
     254: 'Gamification; Human-centered design; Service design',
     255: 'Business performance; Servitization; Environmental uncertainty; Adjustment cost; Coordination cost',
     256: "China's accession to the WTO; input trade liberalisation; manufacturing servitisation",
     257: 'Business model innovation; Service systems; Automotive industry; Reference framework; Digitalization',
     258: 'Value co-creation; Digital platforms; Internet of things; Case study; Boundary resources; Standardization',
     259: 'Servitization; Capabilities; Modularity; Ambidextrous performance; Integrated solution',
     260: 'Performance; Manufacturing firms; Servitization; Mediation; Digitalization',
     261: 'Public services; market services; innovation; networks',
     262: 'Relationship logic; Relationship transition; Servitization; Service-dominant logic; Relationship marketing; Business relationships',
     263: 'Performance; Manufacturing; Servitization; Service paradox',
     264: 'Servitization; Data-driven servitization; Remote monitoring technologies',
     265: 'Italy; Manufacturing; Europe; Survey; Servitization; Strategy',
     266: nan,
     267: 'Costing; investment; management; equipment; cost benefit',
     268: 'ICT; GNSS; error mitigation; cloud-based service; integrity; scalability; market trends; servitization',
     269: 'Social manufacturing; manufacturing service; order allocation; Stackelberg game; multi-objective optimization',
     270: 'Food industry; re-distributed manufacturing; new makespaces; production networks; new product development',
     271: 'servitization; smart operation and maintenance service; value-based contract; revenue-sharing; equitable entropy',
     272: 'Industry 4.0; industrial internet; internet of things (IoT); business network; resource dependence',
     273: 'Manufacturing strategy; Servitization; Customization',
     274: nan,
     275: 'Product-service solutions; servitization; platform strategies; knowledge platforms; product-service development',
     276: 'Customer knowledge development; Service infusion; Servitization; Service innovation; New service development; Incremental innovation; Radical innovation',
     277: 'Service; Quality; Professional services; Signal; Screening',
     278: nan,
     279: 'Knowledge-intensive business services (KIBS); panel data analysis with fixed effect; knowledge intensity; economic performance',
     280: 'Pricing strategy; Channel effort level; Wind turbine aftermarket service',
     281: 'Digitalisation; service ecosystems; resource integration; servitization; strong and weak ties; operant resources',
     282: 'Servitization; Industry 4.0; Digitization; Digital transformation; Digital innovation business model innovation',
     283: 'Industry 4.0; Smart Manufacturing; digital transformation; manufacturing companies',
     284: 'Performance; Co-creation; Service design; Servitization; Business-to-business marketing',
     285: 'Service-dominant logic; Firm performance; Service innovation; Servitization; Strategic orientation; Manufacturing system',
     286: 'industrial district; new manufacturing; knowledge-intensive business services (KIBS); territorial servitization; local productive configuration',
     287: 'servitization; regional economies; manufacturing; knowledge-intensive business services (KIBS)',
     288: 'territorial innovation; servitization; entrepreneurial finance; manufacturing; knowledge-intensive; entrepreneurship',
     289: 'territorial servitization; wind energy; industry life cycle; European regions',
     290: 'servitization; product-service innovation; knowledge-intensive business services (KIBS); territorial servitization; knowledge; regional development',
     291: 'territorial servitization; entrepreneurial ecosystem; regional entrepreneurship and development index (REDI); knowledge-intensive business services (KIBS)',
     292: 'territorial servitization; knowledge-intensive business services (KIBS); product companies; collaborative partnerships; Germany; case studies',
     293: 'Marshallian industrial districts; territorial servitization; place-based servitization',
     294: 'New product development (NPD); New service development (NSD); New product success; New product and service portfolio advantage; Measurement development; Servitization',
     295: 'servitization; service support; operational performance; Cross-Function Integration; social responsibility',
     296: 'Servitization; Leadership; Strategic management; Japanese manufacturers',
     297: 'Factor analysis; Validity; Service quality; Structural equation modelling; Small and medium-sized enterprises; Distributor',
     298: nan,
     299: 'Internet of things (IoT); intelligent manufacturing systems; industry 4.0; smart appliance',
     300: 'business models; case study; circular economy; manufacturing; sustainability; servitization',
     301: 'industrial services; performance-based services; outcome-based services; maintenance services; systems engineering; IDEF0',
     302: 'Product lifecycles; Project-based organisations; Interface gaps; Project management offices',
     303: 'servitization; sustainable cities; steel; circles of sustainability; sustainable urban metabolism',
     304: 'Servitization; Periodic admission; Repair capacity policy; Collecting policy',
     305: 'manufacturing service ecosystem (MSE); model mapping; competition and cooperation; computational experiment; ecosystem evolution',
     306: 'Value co-creation; Place marketing; Territory; Attractiveness',
     307: 'Brexit; foreign direct investment; global value chains employment; job quality; regions; industrial policy',
     308: 'servitization; components; framework; innovation; aeronautic; AHP',
     309: 'Testing; servitization; virtual environment; utility computing; cloud computing; security; testing as service',
     310: nan,
     311: 'Servitization; manufacturing firms; strategy; Spain',
     312: 'business model innovation; value proposition; deservitization; learning; human-centred design',
     313: 'product-service development; solutions; servitization; attention; attention-based view',
     314: 'service delivery; organisational change; buyer-seller relationships; servitisation',
     315: 'Innovation performance; Costa Rica; Organizational learning capability; Knowledge-intensive business services',
     316: 'servitisation; convergence; strategy; solution; systematic literature review; thematic analysis; productisation',
     317: 'China; Product quality; FDI; International marketing agility; Service intensity',
     318: 'Manufacturing; agglomeration; territorial servitisation; spatial spill-overs; China',
     319: 'Network embeddedness; Joint innovation; Relative attention; Service innovation competence',
     320: 'customer care; after-sales; business model; services management; operations management; servitization',
     321: 'Service design; NSD; industrial services; maturity model',
     322: 'Digital technologies; Manufacturing industry; Product design; Servitization',
     323: 'Big data; cloud computing; cyber-physical integration; Internet of Things (IoT); manufacturing service; smart manufacturing',
     324: 'Service adoption; servitization; supply chain; manufacturing; salesforce',
     325: 'Healthcare service; Process design; Modularization; Design Structure Matrix; Genetic algorithm',
     326: 'servitization transformation; manufacturing enterprises; human resources management mode; competency; performance',
     327: 'Servitization; Service; Humanitarian logistics; Research agenda',
     328: 'Servitization; Service infusion; Service transition; Manufacturer; Service offering',
     329: 'innovation policy; service innovation; capabilities; mission-oriented policy; business models',
     330: 'Social manufacturing; social community; socialized production network; self-organization; operation',
     331: 'servitization; services; company development management; small business; modern concepts of management',
     332: 'Performance-based contracting; business interruption insurance; risk management; manufacturing servitisation',
     333: 'Servitization; Service transformation; Gamification; Advanced services',
     334: nan,
     335: 'Uncertainty; Servitization; Case study',
     336: 'Industry 4.0; Digitization; Advanced manufacturing; Industrial performance; Emerging countries',
     337: 'Service networks; Servitization; Success factors; Quantitative study; Hypothesis model',
     338: 'Input servitization; Onshore input servitization; Offshore input servitization; Export technological complexity',
     339: 'Electric utilities; Energy transition; De-carbonization; Decentralization; Servitization; System integration',
     340: nan,
     341: nan,
     342: 'e-books; publishers; publishing supply chain; disintermediation; digitization',
     343: 'International partnerships; Cross-border alliances; Servitization; Product-service innovation; Human resources; Expertise decision centralization',
     344: 'Maintenance; Condition-based maintenance; Servitisation',
     345: 'Cyber-physical systems; Internet of things; microelectromechanical systems; Machinery Information Management Open System Alliance; Open System Architecture for Condition-Based Maintenance',
     346: 'IT work; autonomy; IT Service Management; de-skilling; control; servitisation; job quality',
     347: 'Chinese exports; export duration; manufacturing servitisation',
     348: 'Servitization strategy; Financial performance; Customer orientation; Organizational design; Configuration; Manufacturing firms',
     349: 'sustainability; digital servitization; green servitization; performance benefits',
     350: 'value proposition; customer value in use; value-in-use dimension; servitization; service transition',
     351: 'Digital Transformation; Internet of Things (IoT); Manufacturing Industry; Service Design; System Design',
     352: 'Service market strategy; Open business models; Outcome-based contracts; Service innovation; Servitization',
     353: 'Software service engineering; Big service; Software reuse; Requirement pattern; Service pattern',
     354: 'Service BOM; Transformation; PLM; Host manufacturers; Complex products',
     355: 'Craftsmanship; Entrepreneurship; Competitiveness; Digital technologies; 3D printing; Foot and body scanner',
     356: nan,
     357: nan,
     358: nan,
     359: nan,
     360: nan,
     361: nan,
     362: nan,
     363: nan,
     364: 'Container-based Sanitation; advanced services; Internet of Things; Base of the Pyramid; WASH; sanitation',
     365: 'Servitization; service transformation; survey; business model; capital goods',
     366: 'Financial performance; Organizational design; Environmental factors; Servitization strategy',
     367: 'Service-dominant logic; Distribution; Servitization',
     368: 'service-orientated manufacturing systems; service transformation; digital capabilities; Internet of things; cloud computing; predictive analytics; DIKW',
     369: 'New product development; Servitization; Service design; Business model innovation; Design thinking; Design capabilities',
     370: 'product-service innovation; PSI; servitisation; performance',
     371: 'Servitization; Case study; Buyer-supplier relationships; Boundary spanners',
     372: 'Servitization; Strategic intent; Managerial tensions; Organizational strategic intent; Servitization intent',
     373: 'postponement; customer value; supply timing; Alderson; transvection; servitisation',
     374: 'Service operations; Service supply networks; Service operations performance; Customer and employee behavior; Servitization; Knowledge-based services; Participation roles and responsibilities; Sustainable services; Social impact services; Sharing economy',
     375: 'Servitization; Internet of Things; Business intelligence; Resource-based view; Digitalization; Industrial internet',
     376: nan,
     377: 'integrated solutions; servitisation; product-service offerings; service; product-service; addressing the customer; integration; customisation; product-service development',
     378: 'Servitization; Barriers; Factor analysis; Remote service; Industrial service; Smart service',
     379: 'Servitization; organizational characteristics; basic services; advanced services',
     380: "B2B manufacturing; customer co-creation; customer-perceived value; service deployment; Taiwan; the People's Republic of China",
     381: 'Service logic; Value co-creation; Service dominant logic; Remote service; Service platform; Service modularity',
     382: 'supply chain; globalization; servitization; trust; Information Technology; Internet of Things',
     383: 'Marketing; Innovation; Services; Organizational processes; Value added',
     384: 'KIBS; innovation outcomes; machine tool industry; size of the firm',
     385: 'solution selling; blueprinting; relationship selling; servitisation',
     386: 'Servitization; Case study; Front- and back-end units; Integrated project teams (IPTs); Organizational design; Solutions',
     387: 'servicizing; card sorting; cluster analysis; product designer; product-service integration',
     388: 'Start-up accelerator; entrepreneurship education; human capital; social capital; entrepreneurial learning; entrepreneurial self-efficacy; networks; mentors; design thinking; lean start-up approach; Australia',
     389: 'Profitability; Servitization; Customer lifetime value; Industrial services; Sales forecast; Installed base',
     390: 'Taiwan; small and medium-sized enterprises; innovations',
     391: 'Servitization; Strategy; Resource-based view; Industrial services; IoT; Service infusion',
     392: 'Servitization; B2B; Technology readiness; Service acceptance; Service adoption process; Service readiness',
     393: 'Service management; SMEs; Firm performance; Servitization; Capabilities; Software firms',
     394: 'business model; experimentation; value capture; music industry; format density; FD',
     395: 'Service operations; Service supply networks; Service operations performance; Customer and employee behaviour; Servitization; Knowledge-based services; Participation roles and responsibilities; Sustainable services; Social impact services; Sharing economy; Delphi study; JOSM literature review',
     396: 'after-sales management; aftermarket; relationship constellations; archetypes; case study research',
     397: 'intelligent system; garden ecology; greenization; robot; application',
     398: nan,
     399: 'Servitisation; Maintenance; Condition-based maintenance; Spare parts inventory management; Advance demand information',
     400: 'Optical packet switching networks; Traffic tolerance; Reinforcement learning; Lightpath requests; Monte Carlo simulation',
     401: 'Service; Strategic service marketing; Relationship marketing; Standardization; Personalization; Transactions; Customer relationships; Automated technology; Cognitive technology; Emotional technology; Servitization; Adaptive personalization',
     402: 'Servitization; longitudinal research; service revenues; EMS Croatia; Manufacturing',
     403: 'Servitization; Outcome-based service; Complex services; Viable service systems',
     404: "Servitization challenges; New service development processes; Manufacturers' services strategies",
     405: 'Servitization; Transformation; Service implementation',
     406: 'Pay-per-use services; Capabilities; Business model innovation; Financing; Knowledge-based view of the firm; Product usage',
     407: 'Servitization; Service-dominant logic; Internet of things; Business model; Value; Customer Co-Created Servitization',
     408: 'Territorial servitization; KIBS; New manufacturing firms; Industry configuration',
     409: 'Service transition; Servitization; Service innovation; Barriers; Energy utilities',
     410: 'Service journey; Servitization; Service transitions; Change management; Services',
     411: 'Service-led growth; Servitization; Capabilities; Strategy; Analytical equipment; Solutions',
     412: 'Servitization; Outcome-based contracts (OBCs); Outcome business models (OBMs); Value drivers',
     413: 'Servitization; M&A; China; Germany; Integration mode; Absorptive capacity',
     414: nan,
     415: 'Cloud; Software as a Service; SaaS; Servitization; Quality of service',
     416: 'service infusion; configurations; business model; dyadic perspective; service capabilities; servitization; hybrid offering',
     417: 'Machine tools; Servitization; Advanced manufacturing; Smartization; Buyer-supplier relationships',
     418: 'Servitization; Manufacturer; Global distribution; Distribution channels; Marketing channels; Business-to-business',
     419: 'EU law; Digital Single Market; digital goods; digital content; internal market; copyright; exhaustion; taxation; consumers',
     420: 'Pre-market functions; Automobile manufacturing; Quality assurance; Servitisation',
     421: 'servitization; case studies; literature review; research-practice gap; research agenda',
     422: 'Energy services; Local and regional energy companies; EPC; ESCO; Energy efficiency; Barrier; Driving force',
     423: 'Revenue management; Pricing; Game theory; Maintenance; Contracts; Servitization',
     424: 'Service modularity; Services; Research agenda; Modularity; Service architecture',
     425: 'Newspapers; servitisation; service dominant logic; customisation; resources; communication',
     426: 'CSR; ERP; Design management; Servitization; Lean-agile; Supply orchestration',
     427: 'supply chain management; product-service supply chain; maintenance and repair services; supply chain integration; obstacles; Middle East',
     428: nan,
     429: 'Servitization; Design; Power-by-the-Hour; Rolls-Royce',
     430: 'Solutions; Servitization; Resource-based view; Solution business; Strategic capability',
     431: 'factories of the future; industry 4.0; manufacturing systems; intelligent manufacturing; cyber-physical systems; virtual organisations; servitisation',
     432: 'Competitiveness; KIBS; Servitization',
     433: 'Servitization; Resource-based view; Capabilities; Process industry',
     434: 'Abductive case study; Service performance; Relationship influences; Service triads',
     435: 'Service experience; servitization; service design; industrial companies; method development',
     436: 'Aftermarket service; New energy; Revenue sharing contract; Service effort level; Wind farm',
     437: 'Key performance indicators; Servitization; Management accounting; Management control; Industrial services',
     438: 'Servitization; Resources; Buyer-supplier relationships; Dynamic capabilities; Operational capabilities; Dyad',
     439: 'China; Servitization; MOA; Service network',
     440: 'Case study; Servitization; Business model; Advanced services',
     441: "Performance measurement; Service; Network; Value; Industrial service; Manufacturers' strategies",
     442: 'Manufacturing strategy; Servitization; Capabilities; Financial performance',
     443: 'Products; Service; Servitization; Product biography; Circular economy; Internet of Things; Repair',
     444: 'Servitization; Advanced services; Capabilities; Complementary capabilities; Network actors',
     445: 'Servitization; Emerging economies; Economic context; Service provision; Survey; Financial performance; Service return; Service paradox',
     446: 'Servitization; Deservitization; Integrated solutions; Knowledge-based view; Capabilities; Theoretical framework',
     447: 'Servitization; Digitization; Interdependences; dPublishing industry; Payment card',
     448: 'retailer; internationalisation; satisfaction; loyalty; manufacturer',
     449: 'manufacturing service ecosystem; virtual enterprise; servitisation; manufacturing engineering; service operation; product design',
     450: 'Business service; information and communication technologies (ICTs); manufacturer and supplier collaboration; UK telecommunication manufacturing',
     451: 'servitisation; social capital; knowledge management; supply chain management; empirical study',
     452: 'Service strategy; Demand-side search; Service implementation; Service-orientated human resource management practices',
     453: 'B2B services; B2B customer experience; Outcomes-based measures',
     454: nan,
     455: 'Servitization; Text mining; Form 10-K; Annual report; Positioning map; Self-organizing map',
     456: 'Manufacturing; Servitization; Outsourcing; Global value chain',
     457: 'Software pricing; Servitization; Competitive strategy; Cloud computing; SaaS',
     458: 'Performance-based contracting; Contracts; Servitization; Relationships; Integrated solutions; Procuring complex performance',
     459: 'Servitization of manufacturing industries; Cloud-based manufacturing business model; Service-oriented manufacturing; Cloud manufacturing; Down-to-earth path; Enabling technologies',
     460: 'Aerospace; Business solution; Market shaping; Relationship; Servitization',
     461: 'Servitization; Business model innovation; Service innovation; Value chain',
     462: 'servitization; service infusion; service network; strategy; dynamic competition',
     463: 'Servitization; Resource realignment; Dynamic capabilities',
     464: 'Relationship dynamics; Business-to-business relationships; Servitization; Strategy-as-practice; Adaptation',
     465: 'Energy efficiency; Servitization; Contracting; ESCo; LED; Lighting',
     466: 'Servitization; Service infusion; Network orchestration; Platform; Service network; Solution network',
     467: 'Servitization; Service transition strategy; Case study',
     468: 'Servitization; Systems integrators; Activity theory; Construction industry',
     469: 'Social manufacturing; manufacturing service; enterprise relationship network; network clustering and analysis; manufacturing community',
     470: 'utility; EUCo; Business model innovation; Servitization; Energy service; Asset transformation',
     471: 'Product-centric service supply; Knowledge-centric service supply; Customer perceived value; Customer involvement',
     472: 'manufacturing servitization; TAM Model; product development model; intention to use; attitude; perceived ease of use',
     473: 'Servitization; Channel competition; Game theory',
     474: 'Market-linking capability; Market turbulence; New product performance; Servitization service innovation',
     475: 'business; knowledge management; project management',
     476: 'Statistical process control (SPC); Service; Responsive process',
     477: 'Cloud manufacturing (CMfg); Service selection and scheduling; TQCS; Relative superiority degree; Analytic hierarchy process (AHP); Ant colony optimization (ACO)',
     478: 'Servitization; Industrial services; Operations risk management and resilience',
     479: 'Servitization; Co-production; Ecology; Green operations; Suppliers; Green manufacturing',
     480: 'Servitization; Manufacturing; Multiple-case study; Remote monitoring technology',
     481: 'Product servitised supply chain (PSSC); product-service value (PSV); e(3)value modelling; servitisation paradox',
     482: 'resources servitisation; on-demand supply; service object-oriented architecture; multidimensional cloud service semantics',
     483: 'Interfunctional Coordination (IFC); Market Orientation; Services Provided by Manufacturers; Service Offering; Servitization; Manufacturers of Electrical Equipment and Electronic Components; Czech Republic',
     484: 'Motivation; Capabilities; Complexity; Resources; Servitization; CoPS',
     485: 'Information; Mass customization; Smart appliance',
     486: nan,
     487: 'Product-enabled services; Service value attributes; Principal component analysis; K-means clustering; Latent semantic analysis; Online textual data',
     488: 'servitization; challenges; alignment; case studies',
     489: 'Delphi study; transformation; servitization',
     490: 'value network; servitisation; challenges; service manoeuvres; manufacturing firms; case study',
     491: 'product system; servitisation; innovation; manufacturing firm; service system',
     492: 'data presentation; PSS; product avatar; PLM; individualised service generation; servitisation',
     493: 'Through-life engineering services; maintenance; repair and overhaul; service knowledge feedback',
     494: 'Servitization strategies; Value chain; Organizational structure',
     495: 'internationalisation; outsourcing/offshoring; supply chain; servitisation; business models',
     496: 'Life-cycle service offering; Industrial service business; Service-dominant logic; Service infusion; Service business models',
     497: 'R&D alliances; R&D institutes; policy dilemma',
     498: 'Servitization; Big Data; Manufacturing; Competitive advantage; Value; Information',
     499: 'Service transition; Solutions; Manufacturing companies; Service strategy; Problematization methodology',
     500: 'Service management; Servitization; Maturity model; Work organization',
     501: 'Service; Organizational change; Servitization; Operations strategy',
     502: 'Business model; sociomateriality; open innovation; actor-network theory',
     503: 'Servitization; Dynamic capabilities; Competitive advantage; Resource-based view; Relational view; Service infusion',
     504: 'Building information models (BIM); Case studies; Digital infrastructure; Information systems; Life cycle; Systems management',
     505: 'Performance; Servitization; Industrial services; Service orientation; Service strategy; Service structure',
     506: 'Servitization; Knowledge management; Case study; Knowledge management systems',
     507: 'family business; family firms; services; servitisation; SEW; socio-emotional wealth; manufacturing; Europe',
     508: 'Servitization; Capabilities; Resources; Service infusion; Manufacturer; Resource configurations',
     509: 'Public sector; Manufacturing; Intellectual capital; Industry policy; Servitisation',
     510: 'Customer orientation; Business process management; Servitization; Customer centricity; Service blueprint',
     511: 'Servitization; Procurement; Marketing purchasing integration',
     512: 'Operations management; Literature review; Theory',
     513: 'Servitisation; Business-to-business; Domino effect; Service failure',
     514: 'servitization; third-party performance; manufacturing; image risk; external partner',
     515: 'e-store; small- and medium-sized enterprise; supply chain; order-delivery process; modular service architecture; service process design; modularization; variation; reuse',
     516: 'Foresight; Trend analysis; Service-dominant logic; Value co-creation; Magazine publishing',
     517: 'Business models; Technology shift; Electric road system (ERS); Servitization; Business model dilemma; Truck manufacturers; Strategy',
     518: 'Manufacturing Servitization; Service Design; Service Innovation Model; Clustering Model',
     519: 'servitisation; knowledge intensive services; structural change',
     520: 'business marketing; B2B; industrial marketing; manufacturer; service offerings; service infusion; servitization; strategy',
     521: 'Service-dominant logic; Networks; Servitization; Value propositions; Solutions; Truck industry',
     522: 'Business model; Service innovation; Capabilities; Servitization; Product-centric firms; Service infusion',
     523: 'service occupations; servitization; intermediate service inputs; global sourcing; manufacturing sector',
     524: 'Servitization; Service infusion; Industrial service; Case study; Organizational ecology',
     525: 'service; disintermediation; industrial marketing; intermediaries; servitization; industrial marketing; business marketing',
     526: 'Servitization; Open service innovation; Business model; Performance',
     527: 'Servitizing; Networking; Large-scale survey findings; Cluster analysis; Multifactorial regression',
     528: 'Network design; Queueing; Remanufacturing; Contracting',
     529: 'Servitization; Customer linking; Demand management; Supply chain management; United Kingdom',
     530: 'servitization; integrated solutions; operations strategy; financial performance',
     531: 'Service supply chains; Triads; Industrial services; Manufacturing industries; Supply chain management',
     532: 'R&D services; R&D collaboration; Customer relationship management; Value co-creation; Customer involvement',
     533: 'Industrial services; Information technology; Communication technologies; Management strategy; Service business orientation; Service orientation; Differentiation; Servitization',
     534: 'Service innovation; Network; Penrose; Service factory; Servitisation',
     535: 'R&D consortia; Survival analysis; Taiwanese firms',
     536: 'Brand equity; Customer satisfaction; Internal service quality; Servitization; Social networking',
     537: 'Service complexity; Task discretion; Co-creative operations',
     538: 'Integrated product-service (IPS); Servitization; New product-service development (NPSD); Taxonomy; Typology',
     539: 'Manufacturing technology adoption; Service; Servitization; Manufacturing industries; Technology management',
     540: 'Innovation; Manufacturing; Service; Servitisation',
     541: 'Buyer-supplier relationships; Servitization; Supply chain management; Integrated solutions; Buyer-seller relationships',
     542: 'Manufacturing industries; Process management; Operations and production management; Process mapping; Service organization; Manufacturing companies; Servitisation',
     543: 'Repertory grid technique; Qualitative methods; Manufacturer-supplier relationships; Servitization; Supply chain management; Supplier relations',
     544: 'Management accounting; Decision making; Manufacturing industries; Servitisation; Service; Accounting object(s)',
     545: 'Servitization; Business game concept; Change management; Customer awareness; Quality management',
     546: 'Capacity management; Industrial services; Operations strategy; Sales and operations planning; Servitization',
     547: 'Servitisation; Co-production; Music industry',
     548: 'product extension services; configuration; ontology; OWL; SWRL',
     549: 'servitization; service delivery; ICT',
     550: 'Microfluidics; Microfluidic devices; Services; Service-orientation',
     551: 'Machine tools; Servitization; Prognostics and health management; Modelling; Life-cycle costs',
     552: 'Change; environment; globalization; organizational transformation; servitization; competitive advantage; strategic service management; services as profit centers',
     553: 'Product centric; Services; Product attached; Operations; Servitization',
     554: nan,
     555: 'survey; servitization; service strategy; manufacturing; B2B',
     556: 'manufacturing; products; service; servitization; model; casestudy',
     557: 'Manufacturing industries; History; Service industries; Operations management',
     558: 'Building technology; Skills shortages; Construction industry',
     559: 'supply chain; products; services; servitisation; case study',
     560: nan,
     561: 'Industrial Product-Service System; risk management; dynamic Bayesian network; entropy reduction value; risk analysis',
     562: 'CNC machine tool; product service system; supplier involvement; service planning; remanufacturing',
     563: 'Digital twin; large-scale automated high-rise warehouse; warehouse product-service system; storage assignment; cyber-physical systems',
     564: nan,
     565: 'product service system; product service system board; generator set',
     566: 'Product service system; Service evaluation; Decision-making; PSS opportunities; PSS drawbacks',
     567: 'manufacturing outsourcing; cutting-tool delivery; industrial product service system; economic order quantity model; genetic algorithm',
     568: 'Product-Service Systems; Product design; Design knowledge; Knowledge reuse; Knowledge management',
     569: 'product-service systems; diversity of terms; practical examples; research projects',
     570: 'Remanufactured products; Sustainable system; Competition; Product design; Game theory',
     571: 'Product-service system; PSS strategy; PSS classification; PSS matrix',
     572: 'systematic review; product service systems; PSS frameworks; PSS case studies; PSS trends',
     573: 'Product design; task planning; supplier participation; product service system',
     574: 'CNC; product-service system; module division; configuration modeling; design structure matrix; integration model',
     575: 'Product service system; product modular design; life cycle; cluster analysis; CNC machine tools',
     576: 'Sustainable product-service system; Triple bottom line; Fuzzy set theory; Decision-making trial and evaluation; laboratory (DEMATEL); Analytical network process',
     577: 'Product-service system; lean; leanness; assessment; comparison',
     578: 'Forecasting; Product-service systems (PSS); Revenue model; Revenue sources; Service delivery; Co-location; Collaboration',
     579: 'Service innovation; Sustainable product service systems; Fuzzy delphi method; Fuzzy importance and performance analysis; Analytical network process',
     580: 'capability; capability readiness; product-service systems; system readiness',
     581: 'Product service systems; existing product; spiral evolutionary design methodology; system evolution chain; system performance',
     582: 'battery swapping; electric scooters; Internet of Things; product service system; visual mapping method',
     583: 'Product-service systems; service supply chain; supply chain model; service providing; service-oriented manufacturing',
     584: 'business model innovation; circular economy; product-service system (PSS); configuration; action research',
     585: 'Manufacturing areas; Product-service systems; KIS; Industry 4; 0; Place-based development',
     586: 'Product-Service Systems; Value network configuration; Simulation; Performance evaluation; Servitization',
     587: 'Smart product-service systems; Digital technology; Sustainable innovation; Fuzzy delphi method; Diffusion of innovation theory; Decision-making trial and evaluation laboratory (DEMATEL)',
     588: 'Product service system; oil consumption customer churn; support vector machine; gray TOPSIS group; optimize algorithm',
     589: 'hybrid value creation; hybrid products; product-service systems; engineering methodology; machine and plant construction; technical customer services; mobile application systems',
     590: 'Industrial Product-Service Systems; Product-service systems; Business models; Service delivery; Organization',
     591: 'Product-service systems; Concept generation; TRIZ; QFD; Car sharing service',
     592: 'Sustainability; Service; Value; Impact; Evaluation; Efficiency',
     593: 'Servitization; Product-service systems development; Process characteristics; Stakeholders; Collaboration',
     594: 'Sustainable product-service systems; Sustainability; Consumption; Practice theory; Sustainable design',
     595: 'Product-Service System; Mass Customization; Ontology; Product Configuration',
     596: 'Drying and storage of grains; Product-service system; Survey; Farmer wish to invest; Perceived value; Green energy',
     597: 'product-service systems design; manufacturing servitization; representation framework; classification of product-service systems',
     598: 'Product service system; radio frequency identification device; knowledge management; risk',
     599: 'Product-service systems; Health care; Drug-device combination; Market establishment',
     600: 'intermediate devices; product-service system; smartphone application',
     601: 'Smart product-service systems; Service innovation; TRIZ (Theory of Inventive Problem Solving); Service blueprint; Smart beauty service',
     602: 'product-service-systems; communication materials; stakeholder network; solution design; strategic design',
     603: 'product service system; PSS; circular economy; LCA; merino wool',
     604: 'Warehouse product service system; inventory items; configuration scheme; safety stock',
     605: 'Delphi method; Fuzzy analytic hierarchy process; Sustainability',
     606: 'Circular economy; Product-service system; Obsolescence; Resource flows; Closed-loop; Function analysis',
     607: 'Sustainable Product-Service System; Product-Service System; Small and Medium-sized Enterprises; Servitization; Literature review',
     608: 'Logistics service providers; new product development; outsourcing; product-service system; early supplier involvement',
     609: 'Product service system; Domain mapping; Quality function deployment; House of quality; CNC machine tools',
     610: 'Product service system (PSS); PSS design methodologies (PSS-DM); PSS evaluation methodologies (PSS-EM); PSS operation methodologies (PSS-OM)',
     611: 'smart home; smart home appliance (SHA); product-service system (PSS)',
     612: 'Preliminary design; Product Service Systems; Color-coding; Value visualization; Decision making; Lifecycle value',
     613: 'Product service system; Multi-agent system; Personalisation; Pervasive computing',
     614: 'Environmental sustainability; Maturity model; Best practices; Product/service-systems; Ecodesign',
     615: 'Renting; Product service system; Simulation; Decision making',
     616: 'Product service system; analytical hierarchic process; case-based reasoning',
     617: 'product-service system (PSS); PSS architecture; system interface modeling; system of systems engineering',
     618: 'Product innovation; Product-Service System; Customer acceptance; Electric vehicles; Car sharing',
     619: 'Service; Lifecycle; Industrial Product-Service Systems',
     620: 'Reverse logistics; Product service system; Servitization; Service-dominant logic; Supply chain collaboration',
     621: 'co-creation; product-service systems; circular product design; washing machines; access models; impact',
     622: 'product-service paradoxes; sustainable product service; product-service system innovation; product-service systems design methodology; product-service systems business model',
     623: 'product-service system (PSS); quality function deployment for product service system (QFDforPSS); medical devices; fuzzy analytic hierarchy process (FAHP); screening life cycle modeling (SLCM); customization; life cycle assessment (LCA); circular economy (CE)',
     624: 'Joint optimization; reliability design; spares inventory; replacement maintenance; availability; product-service system',
     625: 'Product-service systems; Design for sustainability; Consumer perception; Clothing',
     626: 'design for sustainability; Product Service Systems; ICT; SME; design; green operations; organizational transformation',
     627: 'product service system; multi-attribute utility analysis; carbon dioxide reduction; maintenance service level',
     628: 'Product-service system (PSS); Sustainability; Dynamic; Multidimensional; Triple bottom line (TBL); System dynamics (SD)',
     629: 'Product-service-systems (PSS); Design process; Design method',
     630: 'solution oriented partnership; product service systems design methodology; representation of Product service systems',
     631: 'Product-service System; Conceptual Model; Life Cycle; Methods and Tools',
     632: 'Product-Service System; concept design; manufacturing; servitization; player interaction; prioritization',
     633: 'Product service system; Interpretive structural model; Remanufacturing; Reverse logistics; Maintenance',
     634: 'product service systems; policy measures; functional program',
     635: 'product-service system; life cycle assessment; rental clothing; environmental impact; sustainable business model; consumer behaviour',
     636: 'Distributed generation (DG); Distributed Renewable Energy (DRE); Product-Service Systems (PSS); Classification system; Business model; Design',
     637: 'PSS; Environmental performances; User satisfaction; Innovative eco-design; Functional analysis',
     638: 'innovation management; product service system; transition path; sustainable mobility; dynamic capabilities; car-sharing',
     639: 'Information flow; Product-service systems; Diagrammatic modelling; Function-oriented design',
     640: 'product-service system (PSS); service design; quality function deployment (QFD); customer requirements; medical devices; regulated market',
     641: 'Product-service system; System Dynamics; assessment model; dynamic behavior; guidelines',
     642: 'joint optimization; maintenance; multiple failure modes; product-service system; spares ordering policy',
     643: 'Service innovation; Sustainable product service systems; Fuzzy delphi method; Fuzzy importance and performance analysis; Analytical network process',
     644: 'Smart product service system; Natural language processing; Machine learning; Recommendation system',
     645: 'Service Design; System Design; Product-service System; New Product Development; Stakeholder Involvement',
     646: 'Product-Service System; PSS; New Product Development; New Service Development; Stakeholder Engagement; PSS Characterization; Early Stage; ICT; Healthcare',
     647: 'product service system; PSS; PSS design; co-creation; PSS redesign; PSS business model',
     648: 'product service system; configuration; customer perception; rough set; rule extraction',
     649: 'Product Service Systems (PSS); design process; evaluation',
     650: 'Design for human safety; Work situation; Product-Service System; Function-Behavior-Structure',
     651: 'Business development; cooperation; design process; information and communication technology (ICT); innovation; product service systems (PSS); small and medium enterprises (SMEs)',
     652: 'Product-service systems; Customer value; Servitization; Framework',
     653: 'sustainability; product-service systems; design process; circular economies',
     654: 'Electric mobility; Electric vehicle; Smart city; Platform service; Business model; Product service system',
     655: 'Design game; Ultra-Personalised Product Service System; shoe-making; research through design; models of production',
     656: 'Product service system; scheme selection; digital twin; fuzzy assessment',
     657: 'Manufacturing; Servitization; Product-service systems; Product-related services',
     658: 'constraint satisfaction problem; ontology; product-service systems; product-service system customization; recommender systems',
     659: 'product-service systems; customer preference; clustering; data mining; design for service; machine learning; big data',
     660: 'Product-service system (PSS); Customer value; Customer experience cycle; Evaluation; Analytic network process (ANP); Niche theory',
     661: 'Servitization; Digitalization; Product-service systems',
     662: 'adaptability; adaptive processes; goal-oriented process modeling; intelligent agents; engineering change management (ECM); industrial product service systems (IPS2)',
     663: 'product-service systems; sustainability; functional economy',
     664: 'customer journey; servitisation; product service systems; PSS; business-to-business; B2B',
     665: 'Value bundle; Product-service system; Conceptual modeling; Reference model; Modeling language',
     666: 'Product-Service System; sustainability; SDG 9; SDG 12; barriers; Brazil',
     667: 'knowledge management; new product development; stage-gate; aerospace; product-service system',
     668: 'Product-service system; Software engineering; Model driven; Tool integration; Integrated Development Environment; Manufacturing',
     669: 'Data-driven services; digital servitization; conceptual framework; PSS; typology',
     670: 'Product-service systems; Circular economy; Leasing; Consumer behavior',
     671: 'Business model innovation; servitisation; product-service system; digitalisation; typology; taxonomy',
     672: 'Product-Service Systems (PSS); customisation; manufacturing systems; customisation degree; modelling; quantification',
     673: 'sustainable innovation; production platforms; production consumption; product-service system; partnership building',
     674: 'product service system; institutional isation; sustainable consumption; sharing systems; collective use; socio-cultural context',
     675: 'product-service system; servitization; review',
     676: 'Product-service system; Recommender system; Case representation; Case indexing; Similarity measurement',
     677: 'product-service system; PSS; technological chance; KeyGraph; text mining; business method patent; BM patent; chance identification; chance discovery; database-centred approach; PSS innovation; mobile industry',
     678: 'circular economy; decoupling; industrial ecology; life cycle thinking; product-service system (PSS); rebound effect',
     679: 'firm performance; process innovation; product innovation; product-service system; servitization; technological innovation',
     680: 'Service Engineering; Product-Service System; Discrete-event simulation; Service design; Customer value; Service development',
     681: 'Product service system; Association rules; Multi-dimension and multi-objective double; group discrete Firefly Algorithm (MODGDFA); Pareto rules',
     682: 'Computer-aided design; integrated product service system; integration design; knowledge engineering',
     683: 'Product service system; sustainable design and development; plastic mold; analytical hierarchy process; dematerialization',
     684: 'household waste prevention; waste electrical and electronic equipment; product service systems',
     685: 'Product service system; Configuration rule; Rule mining; Imperialist competition algorithm',
     686: 'Ballast water; Shipping; Product-service systems; Eco-innovation; Eco-efficient value creation',
     687: 'circular economy; mining industry; product-service systems; servitization; sustainability',
     688: 'Product-service system; Customer requirement; Sustainability; Multi-dimensional value; Grey DEMATEL-ANP',
     689: 'Product-service systems (PSS); servitization; business model (BM); framework; SMEs',
     690: 'Design alternative evaluation; Product service systems; Variable precision rough set; Decision analysis; Rough weighted geometric mean',
     691: 'Product-service systems; Service delivery; Uncertainty; Cost estimation',
     692: 'Product-service systems; Critical success factors; Electric car',
     693: 'Product-service system (PSS); PSS Board; PSS visualization',
     694: 'product-service system; event-related potentials (ERPs); decision making; perceived value',
     695: 'Digital transformation; Rehabilitation assistive devices; Smart product-service systems; Synthetical design; Rehabilitation assistive smart product-service; systems',
     696: 'Software agent; Service enabler; Product service systems',
     697: 'Circular supply chains; product-service systems; business model innovation; circular business models; circular economy',
     698: 'Functional economy; Product-service system; Service simulation; Servitization; G-DEVS; HLA federation',
     699: 'Product service systems; Value creation; Value-in-use; Access; Sharing economy; Car sharing',
     700: 'circular business model; business model innovation; customer development; product-service systems; circular economy; remanufacturing',
     701: 'Material Footprint; sustainability strategies; social limits to growth; individual welfare; sustainable design; product service-systems',
     702: 'Product-service systems; Digitization; Adoption of innovations; Institutional theory; Business-to-business marketing; Internet of things',
     703: 'Product-service systems; Servitization; Service transformation; Industry 4; 0; Digitalisation; Research agenda',
     704: nan,
     705: 'Smart Product Service System; Text Analytics; BERT; Service Blueprint',
     706: 'business model; business modelling; morphological analysis; morphological chart; Product-Service System (PSS); case-based system',
     707: 'product-service system; customer requirements; systematic literature review',
     708: 'product service system; configuration; customer needs; customer perception; support vector machine',
     709: 'original equipment manufacturer; operational practices; servitization; maintenance',
     710: 'sustainable consumption; consumption pattern; product service systems (PSS); emotional design',
     711: 'product-service systems; design for service; manufacturing strategy; servitisation; upscaling',
     712: 'Servitization; Servitization scale; Servitization level; Service infusion; Product-service systems',
     713: 'design support; design knowledge; knowledge management; product-service systems',
     714: 'manufacturing SMEs; product-service systems; an iterative design method; existing components; acceptability and sustainability',
     715: 'product-service systems; territory; sustainability; circular economy; literature review',
     716: 'Smart product-service system; Sustainability; Knowledge management; Reversible design; Context-awareness',
     717: 'Industrial product-service systems; Condition monitoring; Fault diagnostics; Support vector machines',
     718: 'Design method; Service; Evaluation',
     719: 'Model-based systems engineering; Traceability; Product service systems; Model integration',
     720: 'Grey-DEMATEL; Product-service system; Circular economy; Resource-based view; Stakeholder theory; End-of-Life; Resource-dependence theory',
     721: 'Technical Product-Service Systems; Cumulative Energy Demand; Sustainability',
     722: 'product-service systems; new product development; after-sales service; word',
     723: 'product-service system (PSS); product-service relationship (PSR); product-service performance (PSP); evaluating',
     724: 'Sustainable product service system; Servitization; Life-cycle cost analysis; Design for environment; Product selling and leasing model design',
     725: 'economic assessment; system approach; algorithmic; platform; product-service systems; sensitivity',
     726: 'model-driven design; Product-Service Systems; conceptual design; design exploration; visualization; decision-making; digitalization',
     727: 'Community transformation; Soft systems methodology; Product-service systems; Synergism; Sustainability; Service innovation',
     728: 'Service Engineering; Product-Service System; Design support; Knowledge; Computer-aided design',
     729: 'Service; Conceptual design; Design experiment',
     730: 'Production planning; Coordination; Service',
     731: 'circular economy; materialities; product service system; prosumer',
     732: 'Product-service system; Business game; Game-based learning; Engineering education; Facilitation tool',
     733: 'social capital; innovation; needs; product service system; LETS; sustainable consumption',
     734: 'Sustainability; Life cycle modelling; Product-service system; Customer satisfaction; Maintenance management; Medical devices',
     735: 'Product-service system; Aerospace manufacturing; Knowledge technology; Ontology-based representation; Requirement analysis',
     736: 'IoT; product-service system; business model; capabilities',
     737: 'Infant care products; Consumer culture theory; Diffusion; Product service systems; Sustainable futures',
     738: 'vending machine PSS; product-service system (PSS); integrative framework; innovation management',
     739: 'product service systems; functional product development; integrated solutions; engineering methods',
     740: 'Product/service systems; Design navigator; Design method; Circular economy; Lifecycle perspective; Life cycle assessment',
     741: 'Sustainable product service systems; Circular economy; Mobile phones; Practices',
     742: 'Product-Service-System Configuration; Mass Customization; Solution Space Modeling',
     743: 'territorial servitisation; TS; place leadership; local productive systems; LPSs; lock-in condition; rerouting; product-service systems',
     744: 'product-service system (PSS); servitisation; supply chain management; supply chain design',
     745: 'Service Engineering; Product-Service Systems; Literature review',
     746: 'Networked collaborative manufacturing; Mass customization; Mass personalization; Product service system',
     747: 'circular design; circular economy; product lifecycle; intervention mapping; scenario matrix; sustainable futures',
     748: 'Product/service-systems; Life cycle assessment; Environmental evaluation; Rebound effects; Environmental impact; Circular economy',
     749: 'Product-service system; Life cycle; Requirements engineering; Dynamic system environment',
     750: 'Product Service System; Maintenance, repair and overhaul (MRO); Service model; Knowledge representation; Aerospace manufacturing',
     751: 'Lifecycle; Modular design; Reconfiguration',
     752: 'Product-service system; Functional performance; Dynamics; System dynamics',
     753: 'Knowledge management; Web 2.0; Product-service systems; Knowledge life cycle',
     754: 'Product-service system; Impatient customers; Markov chain; Additional service capacity',
     755: 'Advanced service; performance-based service; product-service systems; FMEA; rating AHP',
     756: nan,
     757: 'mobile payment services; product-service system; PSS; continuance intention; empirical study',
     758: 'Co-creation; co-synthesis; emergence; public product service system',
     759: 'Product service systems; Design structure matrix; Engineering change management; Data-driven design; Three-way decision making; Digitalization',
     760: 'Product-service systems; Typology; Functional decomposition; Functional Hierarchy Modeling',
     761: 'product service system (PSS); servitisation; service engineering (SE); failure mode and effects analysis (FMEA); grey relational analysis (GRA)',
     762: 'Servitization; Product-service system; Emerging countries; Contingent factors; Business model; Financial aspects',
     763: 'Staff well-being; reward system; servitization; staff motivation; behavioural operations; organisation culture',
     764: nan,
     765: 'new service development; service innovation; maturity model; case studies; product-service systems; product-centric companies',
     766: 'Environmental preservation; Sustainability; Sustainable product service system; Water management practice; Water resource',
     767: 'off-site manufacturing; servitization; sustainability; industrial construction; product-service system; business model innovation',
     768: 'Product service systems; Sustainability; Environmental consciousness; Sharing economy; Perceived stockout risk',
     769: 'automotive industry; sustainability; product-service systems; micro-factory retailing',
     770: 'Quality management; Industrial product service systems (IPS2); Product-service systems (PS2); Value creation chain',
     771: 'service design; product-service systems; design methodology; design research',
     772: 'Manufacturing firms; Innovation management; Eco-innovation; Open-innovation; Product-Service Systems',
     773: 'product architecture; PSS; Product-Service Systems; modularization; MBSE; architecture optimization; data management; versioning; data continuity',
     774: 'Smart Product Service System; Sustainability; Rough Set theory; Best Worst Method; Criteria Importance Though Inter-criteria; Correlation',
     775: 'Sustainability; Industrial product service system (iPSS); Resource service scheduling; NSGA-II',
     776: 'Product-service systems; systems engineering; integrated design; interface modelling approach; multi-disciplinary integration',
     777: 'Product-service system (PSS); sustainable transports; electric car sharing; measurement; sustainability',
     778: 'Smartphones; Leasing; Product-service system; Discrete choice experiment',
     779: 'Uncertain demand; warehouse product service system; interval number optimization; service cost; maximum profit',
     780: 'Complexity; Customization; Product-service systems (PSS); Industry 4.0',
     781: 'Sustainable product service system; Warranty; End of life; Genetic algorithm',
     782: 'Product-Service System (PSS); Fashion; Waste; Dematerialization; Sustainability; European waste hierarchy',
     783: 'demand estimation algorithm; revealed preference; distributed systems; product service systems bike share; data-driven design',
     784: 'Requirements interaction; Product-service system (PSS); Group DEMATEL; Rough set theory; Group decision making',
     785: 'Product-service system; integrated design; conceptual framework; service engineering; product-service integration',
     786: 'Product-Service System (PSS); Evaluation method; Service innovation; Model feasibility',
     787: 'Dynamic cellular manufacturing system (DCMS); Product-service system (PSS); Configuration and operation architecture; Function block (FB); Web services',
     788: 'Product-service systems; Servitization; TRIZ; Sustainable product-service systems; Systematic innovation; Eco-innovation; Literature review',
     789: 'Life cycle analysis; Product-service system; Waste electrical and electronic equipment',
     790: 'Product-service systems; Text mining; Latent dirichlet allocation; Topic landscape; Literature review',
     791: 'product-service systems; PSSs; compressed air; energy efficiency; multi-criteria decision making; MCDM; conflicting objectives',
     792: 'requirement evaluation; industrial product-service system (IPS2); industrial customer activity cycle (I-CAC); rough group analytic hierarchy process (AHP)',
     793: 'Smart product-service system; User satisfaction; Design iteration; Digitalization; Machine learning',
     794: 'Product service system; Case study; Avionics; Availability; Defence; Conceptual model',
     795: 'Collaborative consumption; Pro-environmental behaviour change; Product-service systems; Social practice theory; Social psychology; Values',
     796: 'Product service system; data mining; product monitoring; knowledge management; servitization',
     797: 'Product-service system; Business model; Machine tool manufacturer; Case study',
     798: 'Product-service system; Integrated design; Engineering models',
     799: 'Product-service systems; Design; Reliability; FMEA; Business opportunity',
     800: nan,
     801: 'Smart product-service systems; Concept evaluation; User experience; Information axiom; Context-awareness',
     802: 'product-service system (PSS); ecosystem theory; multiple stakeholder; fuzzy analytic network process (fuzzy ANP); quality function deployment (QFD)',
     803: 'Prototyping; Collaborative Design Tools; Product Service Systems; Healthcare',
     804: 'Product-Service System; Closed-loop-design; PSS Conceptual Design',
     805: 'Product-service system; PSS; Sustainable business models; Sustainable value; PSS archetypes; Servitization',
     806: 'Smart product-service system design; Physical Internet; Intelligent interoperable logistics; Service-orientation; Logistics-as-a-Service; Sustainability',
     807: nan,
     808: 'Life Cycle Simulation; Product-Service Systems; Reference architecture',
     809: 'Product-service system; Eco-design; Waste management; Machine-to-machine; Life cycle analysis',
     810: 'product-service systems; design; interoperability; ontology',
     811: 'Industrial services; Contracts; Uncertainty; Sustainability; Transformation',
     812: 'Smart product-service system (PSS); Innovation value propositions (IVP); Neutrosophic set; DEMATEL; CRITIC',
     813: 'African HEIs; Curriculum development; Sustainable Product-Service System; Distributed Renewable Energy; Learning resources; Open source and copyleft',
     814: 'Sustainable product service system; Re-use; Waste hierarchy; Social enterprise; Eco-efficiency; WISEs',
     815: 'Product-service system (PSS); Quality function deployment for product; service system (QFDforPSS); Analytic network process (ANP); Customer requirements; Decision making; Medical devices',
     816: 'personal construct psychology; product service systems; pushchairs; sustainable consumption; repertory grid technique',
     817: 'Product Service System; Service Engineering; Product development; Lean design; Service systems; Knowledge-based systems',
     818: 'user-generated contents; quality determinants; product-service systems; topic modeling; car-sharing',
     819: 'Logistics services; Product service systems; Auction; Cloud service allocation and sharing',
     820: 'Industrial big data; Big data; Product service system; System engineering; Fuzzy DEMATEL; Smart manufacturing',
     821: 'Service Engineering; Methodology; Industrial application; Simulation; Product service system; Case study',
     822: 'product-service systems; Industry 4.0; circular economy; digital services; innovation; socio-technical systems; digitalization',
     823: 'product service system; PSS classification; transition method; rough set theory',
     824: 'Data-driven design; Smart; Connected product; Digital twin; Product-service systems; Value creation',
     825: 'Diffusion; Innovation; Laundry; Lighting; Social practice; Sustainable product-service system',
     826: 'Trademark; Patent; Product-service system; Link prediction; Business ecology network',
     827: 'Infant mobility products; Product Service System; Resource efficiency; Socio-technical experiments; Strategic Niche Management',
     828: 'product service system; configuration evaluation; cost estimation; life cycle costing; PSS configuration',
     829: 'product-service systems; circular economy; concept design; multi-criteria decision making; sustainability',
     830: 'Prefabricated housing construction; Smart product-service systems; Blockchain; Internet of things; Sustainability',
     831: 'Life Cycle Assessment; Product/Service-Systems; Environmental impacts; Reference system; Functional unit; System boundaries',
     832: 'product-service systems; sustainability; sustainable business models; business model innovation',
     833: 'Product-service system; Ethical; Sustainable; Preventive maintenance; Sharing; Remaining effective life',
     834: 'Product Service System (PSS); service-oriented PSS development process; English education; Analytic Hierarchy Process (AHP); Quality Function Deployment (QFD)',
     835: 'product-service system; distributed manufacturing; future scenarios; design tool; design research methodology',
     836: 'Chinese Manufacturing; Servitization; Product service system; Co-creation; Service innovation; Belt and road initiative',
     837: 'product service system (PSS); availability; field repair kit',
     838: 'collaborative networked organisation; product-service systems; virtual enterprise; value co-creation; complex networks; conceptual modelling; graph theory',
     839: 'design; innovation management; product development; design for service; design for manufacture; knowledge management',
     840: 'Product-Service Systems; Design; Neural network',
     841: 'Large technical systems; Business model; Technology diffusion; Product/Service System; Ecodesign',
     842: 'Innovation; Multilevel design process; Transition management; Sustainable transport; Electric vehicle; Product-service system',
     843: 'Product-Service System (PSS); sustainability assessment; multi-criteria analysis; decision-making; early design',
     844: 'Product-service system (PSS); Evaluation scheme; Evaluation criteria',
     845: 'data analytics; data warehousing; decision support systems; product-service systems (PSSs); product-service systems customization; product usage data; recommender systems (RSs); sensors',
     846: 'product lifecycle; system lifecycle; lifecycle management; circular economy; product-service system; multi-disciplinarity; product classes and instances; closing material and information loops',
     847: 'mt-iPSS; Machine tool; Machining capability; Service',
     848: 'actor networks; institutions; resource productivity; sociotechnical',
     849: 'information design; protocol analysis; color-coded 3D models; conceptual design',
     850: 'Business; Companies; Manufacturing; Interviews; Faces; Context modeling; Encoding; Advanced services; alignment; business model; product-service systems (PSS); servitization; value destruction; value leakage',
     851: 'Sustainability; Design; Food system; Agriculture',
     852: 'Product-Service System; Prognostics and Health Management; Online simulation; Dynamic behaviour',
     853: 'Product-service systems; crowd sensing; value co-creation; decision-theoretic rough set; data-driven design; servitization',
     854: 'Operational data; Value creation; Smart Product-Service System; Operational context; Systematic Literature Review',
     855: 'Product-service systems; Solar PV; System Dynamics Modeling; Bass Diffusion Model; Diffusion theory',
     856: 'sustainable consumption; product-service systems (PSSs); value proposition; practice definition; sustainable fashion; waste; sustainable business models; circular economy',
     857: 'Sustainable product-service systems; Product-service value; Fuzzy delphi method; Fuzzy interpretive structural modeling; Best-worst method',
     858: 'Product Service System (PSS); servitization; productization; symbiosis; enterprise management; conceptual framework; strategic change',
     859: 'Product service systems; Customer characteristics; Local Cluster Neural Network; Rules extraction',
     860: 'circular economy; business model innovation; product-service systems',
     861: 'Product-service system; Requirement interactions; Rough-fuzzy DEMATEL; 2-additive fuzzy measures; Choquet integral',
     862: 'Product-service systems; Sustainability; Actor networks; Territory; Network resources; Circular economy',
     863: 'Product-Service System; customer satisfaction; Quality Function Deployment; Axiomatic Design; modularisation',
     864: 'Servitization; Systems development; Service systems',
     865: 'case study; design process; human factors; service design system design',
     866: 'Carsharing service; Simulation tool; Fuzzy classification; Service prioritization; Product-service systems (PSS)',
     867: 'product service systems; modular development; PSS development; service design',
     868: 'Energy Demand Management; adaptive Scheduling; manufacturing; product Service Systems; servitization',
     869: 'Singapore; Transition',
     870: 'Product-service system (PSS); Engineering characteristics (EC); Fuzzy pairwise comparison; Data envelopment analysis (DEA); Kano model; Non-linear programming',
     871: 'product-service system; key performance indicator; assessment software; life-cycle performance management; PSS evaluation',
     872: 'Comprehensive design framework; Design conflict resolving; Modularization; Configuration',
     873: nan,
     874: 'Multi-stakeholder requirements; Product-service system; Design; Life cycle costing; Multi-domain matrix',
     875: 'product-service systems; work systems; collaboration; participation; organisational design',
     876: 'product-service systems; leasing; remanufacturing; prams; durable products; eco-design',
     877: 'conceptual solution decision; trade-off decision; rough number; Shapley value; PSS design',
     878: 'industrial product service system; machining capacity; model; measurement',
     879: 'ontology; product service systems; systems of systems',
     880: 'Business models; Sustainability; Servitization; Industrial ecosystems; Product-service system; Circular economy; Ecosystem; Inter-organizational relationships; Orchestration',
     881: 'Industrial product-service system; Tool condition monitoring; Cutting tool services; Product-oriented mode; Use-oriented mode',
     882: 'Servitization; Industrial product service systems (IPSS); Resource planning; Collaboration; Environmental impact',
     883: 'Cloud Manufacturing; Product-Service System; Industry 4; 0; distributed manufacturing; resource sharing; fourth industrial revolution',
     884: 'product-service systems; physical asset management; service contracts; channel coordination',
     885: 'product-service system; servitisation; circular economy; sustainability; bibliometric; systematic review',
     886: 'Value Driven Design; Product-Service Systems; Preliminary design; Systems Engineering; Engineering design; Product development; Servitization',
     887: 'product-service system (PSS); screening life-cycle modelling (SLCM); circular economy (CE); supply chain management; aftermarket services; PSS functional matrix; life-cycle engineering; customer demand; customization',
     888: 'User-Generated Contents; quality management; Product-Service Systems; topic modeling; Structural Topic Model; quality determinants; Quality 4; 0',
     889: 'Product-service system; Modular design; Service solution layer; Utility; Portfolio planning',
     890: 'Smart product-service systems; Configuration system; Hypergraph; Context-aware; User-centric design',
     891: 'Circular business model; Circular economy; Consumer behavior; Digitalization; Shared mobility; Product-service system (PSS)',
     892: 'consumer acceptance; consumer perceived value; use-oriented product-service systems; consumer goods; Sweden',
     893: 'User-centric design; Smart product-service system; Medication management; Multimodal user analysis; Providers integration network; BCE model',
     894: 'Requirement elicitation; Product-service systems; Knowledge management; Data-driven design; Value co-creation',
     895: 'Technology roadmap; design structure matrix; product-service integrated system; planning; relationship',
     896: 'VIKOR approach; variable precision rough set; supplier selection; product service system; vague set theory',
     897: 'Product-service system; Sharing; Production machine; Lifecycle; Big data; Fault diagnosis',
     898: 'Smart product-service system; Service logic; Conceptual factors; Hybrid approach; Sensitivity analysis',
     899: 'Sustainable consumption; circular business models; product-service system; sustainability; environmental awareness',
     900: 'Product-Service System; Personalization; Recommendation; Search cost; DEMATEL',
     901: 'product-service systems; SysML; systems design; requirements engineering; business modeling',
     902: 'Servitisation; Decision-making; Product-service systems; PSS business model; TraPSS (transition along the PSS continuum) methodology',
     903: 'collaboration; co-design; concurrency; product-service systems',
     904: 'Mass customization; Personalization; Quality function deployment; Software renting; Product service system',
     905: 'Supply chain management; Product service system (PSS); Supply chain contract; Information asymmetry',
     906: 'supply chain management; product service systems; logistics; productisation; 3PL',
     907: 'design for environmental sustainability; strategic design; system innovation; life cycle design; product-service system',
     908: 'Smart product-service systems; Sustainability assessment; Prospect theory; Behavioral decision making; Rough set theory',
     909: 'Service; Decision making; Business model',
     910: 'Servitization; Product-service systems; Internet of Things; Machine learning; Metrics',
     911: 'leasing; reuse; electrical and electronic equipment; Product Service System; material use; waste prevention',
     912: 'LivingLab; Sustainable product service systems; Experiments; Open innovation; Sustainable consumption and production; Resource efficiency and protection',
     913: 'Data-driven services; Customer value; Product-service systems; Servitization; Smart connected products',
     914: 'product-service systems; requirement elicitation; information modelling; graph embedding; context-awareness; ontology',
     915: 'product service system(PSS); servitization; topicmodeling; Latent Dirichlet Allocation (LDA)',
     916: 'Smart product-service system; Co-implementation; Process-chain-network; Extended-FMECA; Rough-entropy-ELECTRE TRI',
     917: 'Product-service system; Customer satisfaction; Balanced scorecard; DEMATEL; ANP',
     918: 'After-sales service; Egypt; Emerging markets; Field service; Internationalization; Product-service systems',
     919: 'Product-service systems; Edge-cloud orchestration; Cyber-physical systems; Industrial Internet of Things; Data-driven value co-creation',
     920: 'product service system; concept evaluation; Information Axiom; fuzzy random variables',
     921: 'Smart product service system; Design alternatives evaluation; Smart capability; Intrapersonal and interpersonal uncertainty; Rough-fuzzy data envelopment analysis',
     922: 'Waste prevention; Sustainable urban environments; Product service systems',
     923: 'Customer preference; product service system (PSS); public warehouse; service-oriented manufacturing (SOM); service satisfaction evaluation (SSE)',
     924: 'Product-service system (PSS); Conceptual design; Quality function deployment (QFD); Analytic network process (ANP); Fuzzy set theory; Data envelopment analysis (DEA)',
     925: 'Use-oriented; Product-service systems; Configuration; Optimization; Sustainability',
     926: 'Requirements engineering; Product service system; Hybrid product; State of the art; Product engineering; Software engineering; Service engineering',
     927: 'product-service system; innovation; tools; digitalisation; servitisation',
     928: 'Product lifecycle management; Global production networks; Reference ontologies; Interoperability; Product service systems',
     929: 'access model; bicycle sharing; clothing sharing; conjoint analysis; product-service system (PSS); sustainable business model; sustainable consumption; temporality',
     930: 'Product-service system (PSS); New PSS concept; Chance discovery; KeyGraph; Text mining; Web news',
     931: 'Product-Service Systems; Cost engineering; Concept design; Product development; Life cycle cost; Aerospace',
     932: 'product-service system; sustainability; design-centric complexity; TRIZ',
     933: 'Uncertainty; Competitive bidding; Decision making; Product-Service Systems; Service contracts; Information',
     934: 'product-service systems; office furniture; product lifetime extension; remanufacturing; PSS scenario',
     935: 'Product-service system; Intermediary PSS venture; Solar service company; Solar photovoltaic industry; Solar third-party ownership',
     936: 'design method; product-service system; personalization',
     937: 'Outcome-based contracts; Servitization; Product-service system; Remote monitoring technology; Power-by-the-hour',
     938: 'Servitization; Sustainability; Operations strategy; Product-service system; Survey',
     939: 'Servitization; Product-service system; PSS; Service offerings; Profitability; Financial performance',
     940: 'product-service systems; information systems; business process reengineering; value analysis; structural equation modelling, responsiveness',
     941: 'Servitization; Product service systems; industrial operator model; fleet management',
     942: 'Product-service system; Perceived value; Consumer knowledge; Service experience',
     943: 'product service system; configuration optimization; multilayer network; service activities selection; resource allocation',
     944: 'product-service system; modular master structure; modular design; conjoint analysis',
     945: 'product-service systems (PSS); corporate environmental management (CEM); fashion industry; environmental sustainability',
     946: 'Design theory; Smart product-service systems; Information theory; Design entropy; Value co-creation; User experience',
     947: 'Requirements engineering; Requirements data model; Artifact model; Product service systems; PSS; Hybrid products; Requirements concretization',
     948: 'Business process redesign; Internet of Things (IoT); Product-service system (PSS); Machinery industry',
     949: 'Clothing; Servitization; Textile/clothing supply chains; PSS; Extended responsibility; Product-service system',
     950: 'Product-service systems (PSS); Agency theory; Trust; Adverse behaviour; Agency mechanisms; Servitization',
     951: 'product-service system; cleaner production; sustainable development; cutting tool; machining',
     952: 'Revalorisation; Product-service systems; Consumer behaviour; Circular economy; Packaging; Obsolescence',
     953: 'Circular business models; Circular economy; Decoupling; Sustainable business models; Institutional theory; Product-service-systems',
     954: 'life cycle engineering; process modularization; integration of product and service design processes; investment goods industry',
     955: 'Multimodal transport intercity transport; personalized service design mobility as a; service (MaaS) smart product service system&nbsp; (SPSS)',
     956: 'Servitization; Risk management; Manufacturing industries; Product-service systems (PSS); PSS delivery',
     957: 'product-service systems; business model; archetypes; method; tool',
     958: 'product service system (PSS); concept configuration; support vector machine (SVM); principal component analysis (PCA); quantum particle swarm optimization (QPSO)',
     959: 'social value; innovation; sustainability; customer context; value creation; product-service system',
     960: 'services; Product Service Systems; technology transfer; receptivity',
     961: 'Physical Internet; logistics; smart box; product-service system; cloud computing',
     962: "Product Service System (PSS); Sustainable business model; Government 'demand pull' policy; Energy Service Company (ESCo); Innovation system",
     963: 'Product-service system; Order scheduling; Time window; Metaheuristics',
     964: 'Internet of Things; IoT; Servitization; Manufacturing; Value; Creation',
     965: nan,
     966: 'Smart product service system; User activity-oriented smart service requirement; Requirement identification; Requirement evaluation; Rough-fuzzy best-worst method',
     967: 'product-service system; product design; service design; computer-aided design; service blueprint',
     968: 'strategic alignment; service transition; product-service system; strategy; strategic objective; strategy map; business model',
     969: 'Manufacturing; Lean rules; Product-service system (PSS); Key performance indicators (KPIs)',
     970: 'clothing product-service systems; sustainable clothing consumption; sustainable fashion; Finland; United States',
     971: 'Design for Sustainability; Product-Service System; Radical Innovations; Socio-technical Experiment; Strategic Niche Management; Transition Management',
     972: 'microalgae; future superfoods; practice-based design research; product-service systems; open-source',
     973: 'Automated surface inspection; Smart product-service system; Convolutional neural networks; Cloud-edge computing',
     974: 'product-service model; dynamic behaviour; PSS modelling; PSS simulation',
     975: 'Customization; Product/service system; PSS; Integrated solution; Design science; Platform; Literature review; Modularity',
     976: 'Sustainability; Product-service systems; Supply network risks; Multi-criteria decision-making',
     977: 'circular economy; product-service systems; public acceptance; alternative ownership models; responsibility; ownership',
     978: 'remanufacture; product service system; sustainability; operations management; environmental issues; reverse logistics',
     979: 'Product-service systems; Quality function deployment; QFD; Sustainability; Design process; Conceptual design',
     980: 'Value uncaptured; Business model innovation; Sustainable business models; Product-service systems',
     981: 'PSS; supply chain contracts; information asymmetry; service-oriented manufacturing mode',
     982: 'travel experience; transportation vehicle; Kansei engineering; cognitive; emotional',
     983: 'building information modeling (BIM); product service system (PSS); maintenance management; facility management; building equipment; industry 4; 0',
     984: 'Virtual reality; User-centric design; Smart product-service systems; Value-added services; User experience',
     985: 'Kano model; product service system; result-oriented; servitization; use-oriented',
     986: nan,
     987: 'servitisation; office printing industry; digitisation; paperless office; product service system; PSS; rebound effect',
     988: nan,
     989: 'Smart product-service systems; Engineering design; Design methodologies; Review',
     990: 'knowledge graph; concept-knowledge model; smart product-service system; knowledge evolution; conceptual design; creativity and concept generation',
     991: 'product-service system; life cycle sustainability assessment; product-service flow',
     992: 'IoT (Internet of things); ROS (route optimization system); SPSS (smart product-service system); City logistics',
     993: 'Consumer Experience; Product Design; Product-Service System; Service Design; Value Creation',
     994: 'Product development; Long-life cycle; Sustainability; Product Service Systems; Ambidexterity; Conceptual framework',
     995: 'Circular economy; Jobs-to-be-done theory; Sustainable value proposition; Outcome-driven innovation; Sustainable business models; Product-service systems',
     996: 'OR in service industries; Production service system; Replacement policies; Inventory control',
     997: 'Cyber-physical systems (CPSs); machine health monitoring (MHM); ontology-based framework; product-service system (PSS); smart sensing system',
     998: 'product-service system; service oriented architecture; service identification; service specification; recycling',
     999: 'Product-service systems engineering; Service support systems; Agile software engineering; Information needs; Knowledge needs',
     ...}




```python
id2keyword
```




    {'Digitalization; Company performance; Listed company; Benefit; Obstacle': 0,
     'outsourcing; sustainable development; core competencies; company management': 1,
     'cyber-physical networks; Industry 4; 0; small and medium enterprises; personalization; servitization': 2,
     'servitization; digitalization; dynamic capabilities; enterprise innovation performance; PSM-DID': 3,
     'GVC Embedding Position; Industrial Value-Added; Industrialization; Integration of Information; Technological Innovation': 4,
     nan: 1554,
     'Industry studies; wood manufacturing; performance; value-added strategy; international competition': 6,
     'Digitalization; Inertia; Entrepreneurial orientation; Firm assets; Organizational legitimacy; Financial services': 7,
     'Value co-creation; Manufacturing firms; B2B markets; Digital servitization; Viable system approach': 8,
     "Service transition; Top management team; Informational faultlines; Social faultlines; Tobin's Q": 9,
     'Servitization dynamics; Game theory; Oil industry; Business relationships; Mixed methods': 10,
     'Digital finance; Manufacturing industry; Servitization; Servitizing transformation': 11,
     'Service design; Creativity; Innovation; Digital servitization; Product-service-software systems (PSS); Digitalization; Routines; Institutional work theory': 12,
     'product-service innovation; multiple levels; theory integration; mixed methods; action research': 13,
     'industry servitization; global value chains; economic policy; new business models': 14,
     'human centric lighting; eco-science; manufacturing servitization; service innovation': 15,
     'Digitalization; E-commerce; Strategic development; Servitization; Third party logistics (TPL)': 16,
     'Servitization; Deservitization; History-based management theory; Industry lifecycle; Strategic pivoting; Strategy restoration': 17,
     'Industry 4; 0; SMEs; External knowledge search; Opportunity recognition; Technology adoption': 18,
     'India; Supply chain management; Green supply chains; Suppliers; Automotive industry; Global value chain': 19,
     'Servitization; International configuration; Centralization; Service characteristics; Industrial services; Digitalization': 20,
     'Deindustrialization; tertiarization; inter and intra sectorial links; structural changes': 21,
     'lodging sector; hotels; digital transformation; digital innovations': 22,
     'construction product; servitization; evidence-based design; level of evidence; cognitive buildings': 23,
     'Industry 4; 0; industrial IoT; human-cyber-physical systems; smart manufacturing; operations and services; smart and connected products; design engineering 4; 0; design automation; design for manufacturing; design integration; design methodology': 24,
     'energy services; energy transition; customer value proposition; servitization; framework; end use system; human system; service design': 25,
     'Service innovation; Digitalization; Servitization; Manufacturing companies; Medical device manufacturer; Digital servitization': 26,
     'Service inputs; Eco-efficiency; Manufacturing industries; Input-output linkage; U-shaped relationship': 27,
     'Internet of things; Supply chain management; Business ecosystems': 28,
     'Digital Servitization; Industry 4; 0; Prior Knowledge; Disruptive Technologies; Italy': 29,
     'Market entry; strategy; servitisation; private leasing; carsharing; sharing economy; disruptive innovation; ecosystem': 30,
     'Internet of things(IoT); Circular economy; Pythagorean fuzzy sets; Manufacturing sector; Fuzzy sets; Decision making; SWARA; CoCoSo': 31,
     'Process innovation; Process industry; Ecosystems; Ecosystem strategy; Coopetition; Digitalization; Digital servitization; IoT': 32,
     'Artificial intelligence; Digital servitization; Digital transformation; Digitalization; Business model innovation; Platform': 33,
     'Digitalization; Digital servitization; Dynamic capabilities; Value co-creation': 34,
     'Customer success management; Value in use; Hybrid offerings; Servitization; Solution; business': 35,
     'Responsible sourcing; Climate change; Net zero; Carbon neutral; Mining': 36,
     'Digital servitization; Business model innovation; Digital business model; Value creation; Value driver; Servitization; Digitalization': 37,
     'sustainability accounting; manufacturing; digitalization; triple layered business model canvas': 38,
     'stages of growth; SMEs; micro-sized enterprises; technology-based firm; service-based firm': 39,
     'RCA; CRCA; VWRCA; servitization; methods; Poland; global economy': 40,
     'Servitization; Organisational boundary; Servitization challenges': 41,
     'Servitization; Servitization level; Firm performance; Measurement; Servitization paradox; Configurational analysis': 42,
     'Servitization; Digitalization; Interaction; Manufacturing firms; Market performance': 43,
     'Complexity; Servitization; Digital; Service operations': 44,
     'servitization; service&#8208; oriented manufacturing; product&#8208; service system; capacity allocation; Markov decision process': 45,
     'Servitization; Global value chains (GVCs); Structure; Governance': 46,
     'Servitisation; Modularity; Paradox theory; Advanced services; Product upgrade services; Firm boundary; Boundary negotiation': 47,
     'Multi-dimension; ambidextrous open innovation; the 4th Industrial Revolution; financial sector': 48,
     'Entrepreneurial ecosystem; Business competitiveness; Benefit of the doubt model; KIBS firms; Manufacturing firms; International comparison': 49,
     'digitalization; smart services; innovation flexibility; cooperation; SMEs; electrical engineering': 50,
     'gig economy; platform economy; science mapping; WoS; servitization; digital economy': 51,
     'Digital servitization; Business model innovation; Sustainability; Networks': 52,
     'big data; action research; decision-making; value proposition; value co-creation; digital twins': 53,
     'Dependency Graph; IoT; Path Traversal Sequence; QoS; Service Composition; Top-K': 54,
     'healthcare manufacturing firms; organizational resilience; COVID-19; servitization; digitalization': 55,
     'Scale development; Eco-innovation; Natural resource-based view; Sustainable development commitment': 56,
     'Manufacturing; Industry 4; 0; Digital transformation; Digital servitization; Multilevel theory': 57,
     'Maintenance; Decision-making; Continuous improvement; Service operations; Servitization': 58,
     'Servitization; fsQCA; Experiments; Causal explanation; Case study': 59,
     'change management; circular economy; organizational behaviour; organizational culture; sustainability; sustainable development': 60,
     'Bundling; Innovation; Export; Servitization; SMEs': 61,
     'Maintenance; Servitization; Contract pricing; Predictive analytics; Calibration': 62,
     'game theory; modular production; OEM supply chain; service mode; servitization': 63,
     'Servitization; Case study; Consumer products; Business-to-consumer; Triads; Circular economy; Sustainability': 64,
     'Industry 4; 0; Smart Manufacturing; Smart Products and Services; Smart Working; Smart Supply Chain': 65,
     'Servitization; Topic modeling; Narratives; Literature review': 66,
     'Machine learning; Deep learning; Artificial intelligence; Artificial neural networks; Analytical model building': 67,
     'Smart products; Monitoring capabilities; Autonomous solutions; Servitization; SMEs': 68,
     'Servitization; supply chain integration; basic services; advanced services; supplier integration; customer integration': 69,
     'small and medium-sized ports; comprehensive ports; port ecosystem; European Green Deal; strategic management; environmental and digital transition; sustainable ports; green ports': 70,
     'Manufacturing; Cultural differences; Human resource management; Complexity theory; Technological innovation; Manufacturing industries; Cultural factor; firm performance; organizational design factors; servitization; service paradox': 71,
     'Smart products; Feature fatigue; Technology; Digital servitization; Optimal control': 72,
     'Product-service innovation; IT processes; R&D teams; Centralization; MIMIC': 73,
     'Business model innovation; china; dynamic capabilities; e&#64259; ciency-centred business model theme; manufacturing servitization barrier; novelty-centred business mode theme': 74,
     'knowledge-intensive business service (KIBS) sector; technology-based knowledge-intensive business service (T-KIBS); related diversification; territorial servitization; Fourth Industrial Revolution': 75,
     'Outcome-based contracts; Servitization; Solution selling; Means-end-chain analysis; Laddering': 76,
     'dual innovation; innovation performance; productization; servitization; singular innovation': 77,
     'Digital servitization; Business-model modularity; Product-service-software trajectories': 78,
     'Revenue models; Pacific Asia; digital servitization; smart services; value capture; value-based pricing': 79,
     'Circular economy; Condition-based maintenance; Digitalization; Internet of things; Case study': 80,
     'Creative industries; identity; occupational communities; servitization; video games': 81,
     'Organisational design theory; Organisational structure; Product-orientation; Service orientation; Project-service system; Servitisation': 82,
     'product&#8211; service innovation; servitization; sustainability; performance; product lifecycle': 83,
     'functional specialisation; gross export decomposition; occupations; economic upgrading; CEE countries': 84,
     'hybridity; territorial servitization; regional development; SMEs; innovation; dynamic capabilities; absorptive capabilities; collaboration; southern Germany': 85,
     'innovation strategies; productivity; environmental impact; manufacturing; size': 86,
     'Value added of services; Manufacturing exports; Network analysis; Servitization; PageRank algorithm': 87,
     'Servitization; Repurchase intention; Platform service supply chain; Food apps': 88,
     'Online consumer behavior; Customer value; Services marketing; Financial services; e-commerce; Consumer behaviour internet; Service quality; Information technology; Eservice quality; Blogs; Digitalizations': 89,
     'Information processing; Cognition; Task analysis; Manufacturing; Social networking (online); Complexity theory; Uncertainty; Culture; customer loyalty; information processing; information technology; servitization; social media': 90,
     'Companies; Business; Manufacturing; Pricing; Solid modeling; Context modeling; Technological innovation; Advanced services; business models; digital services; digital servitization; digitalization; revenue model; servitization': 91,
     'Internet of Things; business models; smart cities; big data; consumer data': 92,
     'Ecosystem innovation; Dynamic capabilities; Smart cities; Digitalization; Digital servitization': 93,
     'sharing economy; product launching; pricing strategy; sharing service transformation': 94,
     'rural economic development; shift-share analysis; industrial structure; competitive effect; China': 95,
     'New product development; Innovativeness; Top management service commitment; Dysfunctional competition; NPD speed': 96,
     'Business models innovation; Artificial intelligence; Digitalization; Ecosystem innovation; Value creation; Strategy': 97,
     'E-commerce; E-supply chain; Resource-based view; Dynamic capabilities; Fashion industry': 98,
     'Servitization; Tire manufacturing firm; Financial performance; Channel conflict; Service paradox; Cannibalization': 99,
     'Servitization; business-to-business; solution business models; product lifespan; competitive performance': 100,
     'Servitization; Resource Assignment Problem; Workers Assignment Problem; Metaheuristic Optimization; Whale Optimization Algorithm; Flower Pollination Algorithm': 1509,
     'digital innovation; literature network analysis; business model innovation; digitisation; business model change; digital transformation; structured literature review; case study research; servitisation; digital business strategy': 102,
     'Discourse; Descending hierarchical classification; Theorization; Hybrid organizing; Hybrid organizations': 103,
     'prescriptive maintenance; sustainable business models innovation; Industry 4.0; technology systems; distributed ledger technologies; digital twin': 104,
     'Lean bundles; Servitization of manufacturing; Complementarity; Sustainable performance; Survey': 105,
     'business ecosystem; ecosystem-based business; collaboration; customer value; value co-creation; service; service business; servitisation; software industry; business model': 106,
     'Analytic hierarchy process (AHP); key factors; manufacturing servitization; industrial service innovation; service design methodology': 107,
     'information technology; lean supply chain; LSC; agile supply chain; ASC; SciMAT; science mapping; bibliometrics; literature review; co-citation': 110,
     'Servitization; Service Business Model; Industry 4.0; Digitalization; Service oriented': 111,
     'Platform; Value co-creation; Co-evolution; Orchestration capability': 112,
     'High-end equipment manufacturing; 4S; grey relational degree; servitization': 113,
     'Business model innovation; Digital ecosystem; Multi-sided platform; Digital technology; TRIZ': 114,
     'Servitization; Buyer-seller relationships; Delphi method; Tension; Territoriality; Oil gas industry': 115,
     'SCM; orientation; collaboration; performance': 116,
     'Strategic ambidexterity; Product-service innovation; Performance; Manufacturing multinational enterprises': 117,
     'Hidden costs; Performance based contracts; Case based research; Servitization; Multi actor systems; Engagement; S-D logic; Agency theory': 118,
     'digital economy; social perspective; social business model; railway sector': 119,
     'Digital servitization; Risk management; Business model innovation; Industry 4; 0; Digitalization paradox': 121,
     'circular economy; household appliances; case studies; sustainable development; reduce; reuse; remanufacture; recycle; circular benefits': 122,
     'business model; circular economy; circular business model; product as service model; green innovation': 123,
     'Value-in-use management; Value in use; Hybrid offerings; Servitization; Solution business': 124,
     'Servitization; Supply chain integration; Firm performance; Empirical research; International manufacturing strategy survey': 125,
     'servitization; servitization context; capabilities; service-oriented strategy; case study': 126,
     'Solution business; Signaling theory; Experiment; Servitization': 127,
     'IT exploration; IT exploitation; Service innovation; Cross-functional integration; Manufacturing firms': 128,
     'Nonlinear evolutionary problem; knowledge governance; boundary-spanning search; machine learning': 129,
     'Industry 4; 0; digital transformation; manufacturing; framework; journey': 130,
     'Multi-scale; Intelligent manufacturing; Interconnected chemical engineering': 131,
     'Entrepreneurship; Small-medium enterprises; Startups; Entrepreneurial ecosystem': 132,
     'Capabilities; Performance; Manufacturing; Servitization; EMS': 133,
     'Industry 4.0; Innovation ecosystem; Technology providers; SMEs': 134,
     'Digitally connected services; Improvements; Customer-initiated feedback': 135,
     'Internet of Things; Cloning; Digital twin; Software engineering; Solid modeling; Software architecture; Manufacturing processing; Artificial intelligence (AI); business models; cyber physical systems (CPSs); digital twin (DT); Internet of Things (IoT); machine learning (ML); multiagent systems; network function virtualization; sensors; servitization; smart city; software architecture; softwarization; virtual and augmented reality': 136,
     'mobility as a service; technology acceptance model; mobility behaviour; mobile services; technology adoption': 137,
     'Smart products; Business-to-business; Physicality': 138,
     'Servitization; Service innovation; Collaborative innovation projects; mICT': 139,
     'Servitization; Lean; Agile; Mass customization capability; Product innovation capability': 140,
     'Servitization; Organizational change; Organizational design factors; Culture; Firm performance': 141,
     'Social manufacturing; Resource configuration; Dynamic community; Multi-level Optimization': 142,
     'servitization; business process; service actors; service value; service process value model; process mining': 143,
     'value co-creation; technology and innovation management; mapping; bibliographic coupling analysis; open innovation; servitization; sharing economy': 144,
     'COVID-19 pandemic; customer solutions; performance-based contracting; servitization': 145,
     'Digital transformation; Digitalization; Innovative business models; Digital ecosystem; f and b industry': 146,
     'Smart service; Platform; Interdisciplinary research; Manufacturing company; Smart service provider; Platform economics; Information systems; Multi-sided markets; Business-to-business (B2B) markets': 147,
     'Manufacturing strategy; Human capital; Labour productivity; Product-service innovation; Size': 148,
     'OSA&#8211; CBM; rolling element bearing fault; the internet of things; arrowhead framework': 149,
     'Microfoundations; Servitization; Motives; Value strategies; Small firms; Abduction': 150,
     'Digital Transformation; Digitalization; Digitization; Manufacturing; Literature Review': 151,
     'circular economy; business models; paradoxical tensions': 152,
     'Servitization; Strategy; Accounting tools; Accounting machine; Pragmatic constructivism': 153,
     'Manufacturing; Multi-employer worksite; Safety management; Servitization': 154,
     'Servitization; Industry 4.0; Goods-Services Dichotomy; Goods-Services Convergence; Dual Nature; Hybrid Measures; Domestic Regulation; Legal Silos': 155,
     'IoT; Advanced services; Servitization': 156,
     'RFM analysis; Industrial services; Servitization; CRM; Customer segmentation': 157,
     'transformative cultural tourism; post-industrial economy; servitization; paradigm innovation; rural development policy; business model': 158,
     'data-driven; cyber-physical-social system; social manufacturing; cyber-physical system': 159,
     'Circular economy; Battery leasing; Battery regulation; Circular economy in EV batteries; EV policy; Lithium ion batteries': 160,
     'Intellectual capital; Healthcare; Market access; Innovation process; Digital servitization; Value co-creation': 161,
     'Servitization; variability; modularity; additive manufacturing; supply chain management': 162,
     'Manufacturing servitization; carbon emission embodied in export; regional space; global value chain': 163,
     'Industry 4.0; Value chain; Scenario planning; Delphi study; Customization; Servitization': 164,
     'Innovation; Technological change; Industrial organization; Services; Animal nutrition; Animal health': 165,
     'Innovation; Service-dominant logic; Value proposition; Service ecosystem; Logistics innovation': 166,
     'IOT; Digitialization; Servitization; Business models; BtoB; Italy': 167,
     'Digital servitization; Digital transformation; Organizational culture; Agile mindset; Data-centric business model; Big data monetization': 168,
     'Customer experience management; Customer journey; Market strategy; B2B; Touchpoint': 169,
     'Shared manufacturing; Blockchain; Resource sharing; Smart contract; Socialization manufacturing': 170,
     'Banking; Qualitative research; Service innovation; Thematic analysis': 171,
     'digital revolution; Industry 4; 0; systems thinking': 172,
     'Partner selection; Platform-based innovation ecosystems; Servitization': 173,
     'Servitization; Uncertainty management; Engineering service; Engineering service development; Co-creation': 174,
     'Servitization; Advanced services; Competitive advantage; Manufacturing strategy': 175,
     'WWIC information services; maritime Arctic; co-production; expertise; servitization; user-producer interactions': 176,
     'Smart Products; Innovation ecosystem; Cooperation; Internet of things; Capabilities; Industry 4.0': 177,
     'Business-to-business; Cooperation; Coopetition; Product innovation; Service innovation': 178,
     'COVID-19; Coronavirus; Servitization; Digitalization; Service operations; Solutions; Resilience': 179,
     'Business model; Industry 4.0; Taxonomy; Patterns; Case study; Internet of things (IoT)': 180,
     'Business model; Servitization; Service centres; Top performer; Medium-heavy commercial vehicle industry': 181,
     'Servitization; digitalization; firm performance; structural equation modelling': 182,
     'Decision making process; Servitization; Risk occurrence': 183,
     'Industry 4.0; cyber-physical system; lean; six sigma; lean six sigma; fourth industrial revolution': 184,
     'supply chain relationships; integrated project delivery; integration': 185,
     'Wine tourism; Wine tourists; "Blue ocean" strategy; Visitors\' wine experience preferences': 186,
     'Manufacturing system; Social network; Finite state machine; Peer-to-peer network; Trie-based structure': 187,
     'Cloud manufacturing; Industrial revolution; Diffusion of innovation; Technology-organization-environment; Technology acceptance model; Advanced manufacturing technology': 188,
     'Fintech; Strategic alliance; Make; buy; or ally; Entrepreneurial finance; Banks': 189,
     'agility; business model innovation; digitalization; digital transformation; dynamic capabilities; ecosystems; services; strategy; value capture; value creation': 190,
     'socio-technical transition; sustainability transition; multilevel perspective; recycling; upcycling; downcycling; biorefinery; servitization; circular business model': 191,
     'CSR; Certification; Sustainability; Business logic; Real estate industry': 192,
     'Servitization; service delivery system; service triads; employee training; panel data': 193,
     'Value proposition architecture; Circular Economy; Innovation; Sustainability; Resource efficiency; Environmental': 194,
     'Ecology; Management framework; Sharing economy; Society; Sustainable practice; Urban': 195,
     'Small to medium sized enterprises; Financial performance; Servitization; Manufacturing companies; Non-financial performance; Small and medium enterprises; SMEs': 196,
     'customer satisfaction measurements; non-financial performance measurements; customer satisfaction information usage; improvements': 197,
     'Storytelling; Big data analytics; Smart service; Customer reference; Customer-supplier relationships': 198,
     'Digitization; Digitalization; Business model; Business-to-business marketing': 199,
     'Strategy; Servitization; Business platform; Copier; Technology platform': 200,
     'Servitization; Advanced services; Process; Transition; Transformation; Organisational change': 201,
     'Agile development; innovation practices; solution development; new service development (NSD); solution business; servitization and digital servitization; open innovation': 203,
     'Circular economy; Consumer protection; Servitization; Information obligations': 204,
     'Servitization; Business Model Innovation; Digital Platforms; Corporate Entrepreneurship': 205,
     'Servitization; Case studies; Service operations; SMEs; Maturity model': 206,
     'Supply chain integration; food manufacturer; transformation performance; case study': 207,
     'services; growth drivers; innovation': 208,
     'Nature-inspired engineering; Nonequilibrium thermodynamics; Nonlinear control; Self-organization; Virtual modular factory': 209,
     'comparative analysis; enterprise performance; IT industry; product/service integration; service; Servitization of IT companies': 210,
     'Servitization; digitization; servitization paradox; publishing industry': 211,
     'Knowledge-intensive business services; African firms; Conditional knowledge; Contextual paradoxes; Paradoxes of learning; Relational knowledge': 212,
     'Industry 4; 0; strategy; manufacturing industry; manufacturing strategy; cyber technologies': 213,
     'Territorial servitization; county competitiveness; industry configuration; Costa Rica': 214,
     'product-service innovation; servitisation; digital capabilities; digitization; digital transformation; Spanish manufacturing companies; logistic regression; digital technological capabilities; advanced manufacturing technologies; information and communication technology; ICT': 215,
     'business-to-business relations; dynamic models; governance matching; governance mechanisms; services; servitization; solutions': 216,
     'servitisation; pay-per-use; earnings models; financialisation; industrial asset management; accountancy; financial entities; variants of capitalism': 217,
     'additive manufacturing; 3D printing; digitalisation; servitisation; product-service innovation; case study': 219,
     'Servitization; Self-Reinforcing Mechanism; Regional Policies': 220,
     'cloud computing; servitisation; SMEs; cloud service provision; IT governance; cloud governance': 221,
     'servitization capacity; knowledge-intensive business service (KIBS); geographical distance': 222,
     'KIBS; Manufacturing; Innovation; Latin America; Firm Location': 223,
     'Servitization; SSPs; SSCs; machining and equipment manufacturers; digital transformation; Industrial services; IIoT; Artificial intelligence': 224,
     'Servitization; knowledge-intensive business services; innovation; typology': 225,
     'Services trade; Export sophistication; Manufacturing sector; Trade restrictiveness; Panel data analysis': 226,
     'Servitization; Service supply network; Tie strength; Structural holes; Economies of scope; Performance': 227,
     'Servitization; Customization; Structural equation modelling; Competition; Supply chain collaboration; Feature-based production capability': 228,
     'Servitization; transformation; organisational change; organisational context': 229,
     'Servitization; IoT; buyer-supplier relationships; manufacturing; services': 230,
     'Servitization; Financial performance; Dependence; Service providers; Relational embeddedness': 231,
     'Multi-sided platforms; Technological trajectories; Servitization; Platform evolution; Mobility services; Mobility platform': 232,
     'Flexibility; Internet of Things; Modularity; Canvas': 233,
     'Servitization; Case-based research; Home-country institutions': 234,
     'Solutions; pricing; value-based pricing; condition-based maintenance; buyer-supplier relationship': 235,
     'Business model innovation; Strategic response; Digital innovation; Incremental and radical innovation; Incumbents': 236,
     'Europe; Documentary; Logistics services; Logistics industry': 237,
     'Innovation; Servitization; Technological innovation': 238,
     'Servitization; Customer relations; Customer requirements': 239,
     'High performance work systems; Servitization; Service-providing capability; Environmental conditions': 240,
     'Servitization; Advanced services; Integrated solution; Integrated products and service; Transformation; Case study': 241,
     'Solutions; Servitization; Business networks; Interorganizational; Interdependence; Networks of solutions': 242,
     'Servitization; Service capability development; Internal service ecosystem; Front- and back-office': 243,
     'Service innovation; Dynamic capability; Ecosystem; Open innovation; Servitization; Energy utilities': 244,
     'Value; Servitization; Advanced services; Manufacturing': 245,
     'New service development; Servitization; Open innovation; Customer participation; Competitive intensity; Customer needs; Service innovation': 246,
     'Servitization; Business model; Practice; Change; Contestation': 247,
     'service intensity; new ventures; survival; liability of newness; resource-advantage theory': 248,
     'Digitalization; Servitization; Service ecosystem; Digitization; Centralization; Integration': 249,
     'Solution business; Digital platforms; Control; Network orchestration; B2B solutions; Modularity': 250,
     'lean; agile; servitization; digitalisation': 251,
     'Demand-side search; Market capability; Service organizing; Service-oriented HRM; Top management service commitment': 252,
     'Servitization; Risk pattern; Risk control; Decision making logic; Effectuation theory': 253,
     'Gamification; Human-centered design; Service design': 254,
     'Business performance; Servitization; Environmental uncertainty; Adjustment cost; Coordination cost': 255,
     "China's accession to the WTO; input trade liberalisation; manufacturing servitisation": 256,
     'Business model innovation; Service systems; Automotive industry; Reference framework; Digitalization': 257,
     'Value co-creation; Digital platforms; Internet of things; Case study; Boundary resources; Standardization': 258,
     'Servitization; Capabilities; Modularity; Ambidextrous performance; Integrated solution': 259,
     'Performance; Manufacturing firms; Servitization; Mediation; Digitalization': 260,
     'Public services; market services; innovation; networks': 261,
     'Relationship logic; Relationship transition; Servitization; Service-dominant logic; Relationship marketing; Business relationships': 262,
     'Performance; Manufacturing; Servitization; Service paradox': 263,
     'Servitization; Data-driven servitization; Remote monitoring technologies': 264,
     'Italy; Manufacturing; Europe; Survey; Servitization; Strategy': 265,
     'Costing; investment; management; equipment; cost benefit': 267,
     'ICT; GNSS; error mitigation; cloud-based service; integrity; scalability; market trends; servitization': 268,
     'Social manufacturing; manufacturing service; order allocation; Stackelberg game; multi-objective optimization': 269,
     'Food industry; re-distributed manufacturing; new makespaces; production networks; new product development': 270,
     'servitization; smart operation and maintenance service; value-based contract; revenue-sharing; equitable entropy': 271,
     'Industry 4.0; industrial internet; internet of things (IoT); business network; resource dependence': 272,
     'Manufacturing strategy; Servitization; Customization': 273,
     'Product-service solutions; servitization; platform strategies; knowledge platforms; product-service development': 275,
     'Customer knowledge development; Service infusion; Servitization; Service innovation; New service development; Incremental innovation; Radical innovation': 276,
     'Service; Quality; Professional services; Signal; Screening': 277,
     'Knowledge-intensive business services (KIBS); panel data analysis with fixed effect; knowledge intensity; economic performance': 279,
     'Pricing strategy; Channel effort level; Wind turbine aftermarket service': 280,
     'Digitalisation; service ecosystems; resource integration; servitization; strong and weak ties; operant resources': 281,
     'Servitization; Industry 4.0; Digitization; Digital transformation; Digital innovation business model innovation': 282,
     'Industry 4.0; Smart Manufacturing; digital transformation; manufacturing companies': 283,
     'Performance; Co-creation; Service design; Servitization; Business-to-business marketing': 284,
     'Service-dominant logic; Firm performance; Service innovation; Servitization; Strategic orientation; Manufacturing system': 285,
     'industrial district; new manufacturing; knowledge-intensive business services (KIBS); territorial servitization; local productive configuration': 286,
     'servitization; regional economies; manufacturing; knowledge-intensive business services (KIBS)': 287,
     'territorial innovation; servitization; entrepreneurial finance; manufacturing; knowledge-intensive; entrepreneurship': 288,
     'territorial servitization; wind energy; industry life cycle; European regions': 289,
     'servitization; product-service innovation; knowledge-intensive business services (KIBS); territorial servitization; knowledge; regional development': 290,
     'territorial servitization; entrepreneurial ecosystem; regional entrepreneurship and development index (REDI); knowledge-intensive business services (KIBS)': 291,
     'territorial servitization; knowledge-intensive business services (KIBS); product companies; collaborative partnerships; Germany; case studies': 292,
     'Marshallian industrial districts; territorial servitization; place-based servitization': 293,
     'New product development (NPD); New service development (NSD); New product success; New product and service portfolio advantage; Measurement development; Servitization': 294,
     'servitization; service support; operational performance; Cross-Function Integration; social responsibility': 295,
     'Servitization; Leadership; Strategic management; Japanese manufacturers': 296,
     'Factor analysis; Validity; Service quality; Structural equation modelling; Small and medium-sized enterprises; Distributor': 297,
     'Internet of things (IoT); intelligent manufacturing systems; industry 4.0; smart appliance': 299,
     'business models; case study; circular economy; manufacturing; sustainability; servitization': 300,
     'industrial services; performance-based services; outcome-based services; maintenance services; systems engineering; IDEF0': 301,
     'Product lifecycles; Project-based organisations; Interface gaps; Project management offices': 302,
     'servitization; sustainable cities; steel; circles of sustainability; sustainable urban metabolism': 303,
     'Servitization; Periodic admission; Repair capacity policy; Collecting policy': 304,
     'manufacturing service ecosystem (MSE); model mapping; competition and cooperation; computational experiment; ecosystem evolution': 305,
     'Value co-creation; Place marketing; Territory; Attractiveness': 306,
     'Brexit; foreign direct investment; global value chains employment; job quality; regions; industrial policy': 307,
     'servitization; components; framework; innovation; aeronautic; AHP': 308,
     'Testing; servitization; virtual environment; utility computing; cloud computing; security; testing as service': 309,
     'Servitization; manufacturing firms; strategy; Spain': 311,
     'business model innovation; value proposition; deservitization; learning; human-centred design': 312,
     'product-service development; solutions; servitization; attention; attention-based view': 313,
     'service delivery; organisational change; buyer-seller relationships; servitisation': 314,
     'Innovation performance; Costa Rica; Organizational learning capability; Knowledge-intensive business services': 315,
     'servitisation; convergence; strategy; solution; systematic literature review; thematic analysis; productisation': 316,
     'China; Product quality; FDI; International marketing agility; Service intensity': 317,
     'Manufacturing; agglomeration; territorial servitisation; spatial spill-overs; China': 318,
     'Network embeddedness; Joint innovation; Relative attention; Service innovation competence': 319,
     'customer care; after-sales; business model; services management; operations management; servitization': 320,
     'Service design; NSD; industrial services; maturity model': 321,
     'Digital technologies; Manufacturing industry; Product design; Servitization': 322,
     'Big data; cloud computing; cyber-physical integration; Internet of Things (IoT); manufacturing service; smart manufacturing': 323,
     'Service adoption; servitization; supply chain; manufacturing; salesforce': 324,
     'Healthcare service; Process design; Modularization; Design Structure Matrix; Genetic algorithm': 325,
     'servitization transformation; manufacturing enterprises; human resources management mode; competency; performance': 326,
     'Servitization; Service; Humanitarian logistics; Research agenda': 327,
     'Servitization; Service infusion; Service transition; Manufacturer; Service offering': 328,
     'innovation policy; service innovation; capabilities; mission-oriented policy; business models': 329,
     'Social manufacturing; social community; socialized production network; self-organization; operation': 330,
     'servitization; services; company development management; small business; modern concepts of management': 331,
     'Performance-based contracting; business interruption insurance; risk management; manufacturing servitisation': 332,
     'Servitization; Service transformation; Gamification; Advanced services': 333,
     'Uncertainty; Servitization; Case study': 335,
     'Industry 4.0; Digitization; Advanced manufacturing; Industrial performance; Emerging countries': 336,
     'Service networks; Servitization; Success factors; Quantitative study; Hypothesis model': 337,
     'Input servitization; Onshore input servitization; Offshore input servitization; Export technological complexity': 338,
     'Electric utilities; Energy transition; De-carbonization; Decentralization; Servitization; System integration': 339,
     'e-books; publishers; publishing supply chain; disintermediation; digitization': 342,
     'International partnerships; Cross-border alliances; Servitization; Product-service innovation; Human resources; Expertise decision centralization': 343,
     'Maintenance; Condition-based maintenance; Servitisation': 344,
     'Cyber-physical systems; Internet of things; microelectromechanical systems; Machinery Information Management Open System Alliance; Open System Architecture for Condition-Based Maintenance': 345,
     'IT work; autonomy; IT Service Management; de-skilling; control; servitisation; job quality': 346,
     'Chinese exports; export duration; manufacturing servitisation': 347,
     'Servitization strategy; Financial performance; Customer orientation; Organizational design; Configuration; Manufacturing firms': 348,
     'sustainability; digital servitization; green servitization; performance benefits': 349,
     'value proposition; customer value in use; value-in-use dimension; servitization; service transition': 350,
     'Digital Transformation; Internet of Things (IoT); Manufacturing Industry; Service Design; System Design': 351,
     'Service market strategy; Open business models; Outcome-based contracts; Service innovation; Servitization': 352,
     'Software service engineering; Big service; Software reuse; Requirement pattern; Service pattern': 353,
     'Service BOM; Transformation; PLM; Host manufacturers; Complex products': 354,
     'Craftsmanship; Entrepreneurship; Competitiveness; Digital technologies; 3D printing; Foot and body scanner': 355,
     'Container-based Sanitation; advanced services; Internet of Things; Base of the Pyramid; WASH; sanitation': 364,
     'Servitization; service transformation; survey; business model; capital goods': 365,
     'Financial performance; Organizational design; Environmental factors; Servitization strategy': 366,
     'Service-dominant logic; Distribution; Servitization': 367,
     'service-orientated manufacturing systems; service transformation; digital capabilities; Internet of things; cloud computing; predictive analytics; DIKW': 368,
     'New product development; Servitization; Service design; Business model innovation; Design thinking; Design capabilities': 369,
     'product-service innovation; PSI; servitisation; performance': 370,
     'Servitization; Case study; Buyer-supplier relationships; Boundary spanners': 371,
     'Servitization; Strategic intent; Managerial tensions; Organizational strategic intent; Servitization intent': 372,
     'postponement; customer value; supply timing; Alderson; transvection; servitisation': 373,
     'Service operations; Service supply networks; Service operations performance; Customer and employee behavior; Servitization; Knowledge-based services; Participation roles and responsibilities; Sustainable services; Social impact services; Sharing economy': 374,
     'Servitization; Internet of Things; Business intelligence; Resource-based view; Digitalization; Industrial internet': 375,
     'integrated solutions; servitisation; product-service offerings; service; product-service; addressing the customer; integration; customisation; product-service development': 377,
     'Servitization; Barriers; Factor analysis; Remote service; Industrial service; Smart service': 378,
     'Servitization; organizational characteristics; basic services; advanced services': 379,
     "B2B manufacturing; customer co-creation; customer-perceived value; service deployment; Taiwan; the People's Republic of China": 380,
     'Service logic; Value co-creation; Service dominant logic; Remote service; Service platform; Service modularity': 381,
     'supply chain; globalization; servitization; trust; Information Technology; Internet of Things': 382,
     'Marketing; Innovation; Services; Organizational processes; Value added': 383,
     'KIBS; innovation outcomes; machine tool industry; size of the firm': 384,
     'solution selling; blueprinting; relationship selling; servitisation': 385,
     'Servitization; Case study; Front- and back-end units; Integrated project teams (IPTs); Organizational design; Solutions': 386,
     'servicizing; card sorting; cluster analysis; product designer; product-service integration': 387,
     'Start-up accelerator; entrepreneurship education; human capital; social capital; entrepreneurial learning; entrepreneurial self-efficacy; networks; mentors; design thinking; lean start-up approach; Australia': 388,
     'Profitability; Servitization; Customer lifetime value; Industrial services; Sales forecast; Installed base': 389,
     'Taiwan; small and medium-sized enterprises; innovations': 390,
     'Servitization; Strategy; Resource-based view; Industrial services; IoT; Service infusion': 391,
     'Servitization; B2B; Technology readiness; Service acceptance; Service adoption process; Service readiness': 392,
     'Service management; SMEs; Firm performance; Servitization; Capabilities; Software firms': 393,
     'business model; experimentation; value capture; music industry; format density; FD': 394,
     'Service operations; Service supply networks; Service operations performance; Customer and employee behaviour; Servitization; Knowledge-based services; Participation roles and responsibilities; Sustainable services; Social impact services; Sharing economy; Delphi study; JOSM literature review': 395,
     'after-sales management; aftermarket; relationship constellations; archetypes; case study research': 396,
     'intelligent system; garden ecology; greenization; robot; application': 397,
     'Servitisation; Maintenance; Condition-based maintenance; Spare parts inventory management; Advance demand information': 399,
     'Optical packet switching networks; Traffic tolerance; Reinforcement learning; Lightpath requests; Monte Carlo simulation': 400,
     'Service; Strategic service marketing; Relationship marketing; Standardization; Personalization; Transactions; Customer relationships; Automated technology; Cognitive technology; Emotional technology; Servitization; Adaptive personalization': 401,
     'Servitization; longitudinal research; service revenues; EMS Croatia; Manufacturing': 402,
     'Servitization; Outcome-based service; Complex services; Viable service systems': 403,
     "Servitization challenges; New service development processes; Manufacturers' services strategies": 404,
     'Servitization; Transformation; Service implementation': 405,
     'Pay-per-use services; Capabilities; Business model innovation; Financing; Knowledge-based view of the firm; Product usage': 406,
     'Servitization; Service-dominant logic; Internet of things; Business model; Value; Customer Co-Created Servitization': 407,
     'Territorial servitization; KIBS; New manufacturing firms; Industry configuration': 408,
     'Service transition; Servitization; Service innovation; Barriers; Energy utilities': 409,
     'Service journey; Servitization; Service transitions; Change management; Services': 410,
     'Service-led growth; Servitization; Capabilities; Strategy; Analytical equipment; Solutions': 411,
     'Servitization; Outcome-based contracts (OBCs); Outcome business models (OBMs); Value drivers': 412,
     'Servitization; M&A; China; Germany; Integration mode; Absorptive capacity': 413,
     'Cloud; Software as a Service; SaaS; Servitization; Quality of service': 415,
     'service infusion; configurations; business model; dyadic perspective; service capabilities; servitization; hybrid offering': 416,
     'Machine tools; Servitization; Advanced manufacturing; Smartization; Buyer-supplier relationships': 417,
     'Servitization; Manufacturer; Global distribution; Distribution channels; Marketing channels; Business-to-business': 418,
     'EU law; Digital Single Market; digital goods; digital content; internal market; copyright; exhaustion; taxation; consumers': 419,
     'Pre-market functions; Automobile manufacturing; Quality assurance; Servitisation': 420,
     'servitization; case studies; literature review; research-practice gap; research agenda': 421,
     'Energy services; Local and regional energy companies; EPC; ESCO; Energy efficiency; Barrier; Driving force': 422,
     'Revenue management; Pricing; Game theory; Maintenance; Contracts; Servitization': 423,
     'Service modularity; Services; Research agenda; Modularity; Service architecture': 424,
     'Newspapers; servitisation; service dominant logic; customisation; resources; communication': 425,
     'CSR; ERP; Design management; Servitization; Lean-agile; Supply orchestration': 426,
     'supply chain management; product-service supply chain; maintenance and repair services; supply chain integration; obstacles; Middle East': 427,
     'Servitization; Design; Power-by-the-Hour; Rolls-Royce': 429,
     'Solutions; Servitization; Resource-based view; Solution business; Strategic capability': 430,
     'factories of the future; industry 4.0; manufacturing systems; intelligent manufacturing; cyber-physical systems; virtual organisations; servitisation': 431,
     'Competitiveness; KIBS; Servitization': 432,
     'Servitization; Resource-based view; Capabilities; Process industry': 433,
     'Abductive case study; Service performance; Relationship influences; Service triads': 434,
     'Service experience; servitization; service design; industrial companies; method development': 435,
     'Aftermarket service; New energy; Revenue sharing contract; Service effort level; Wind farm': 436,
     'Key performance indicators; Servitization; Management accounting; Management control; Industrial services': 437,
     'Servitization; Resources; Buyer-supplier relationships; Dynamic capabilities; Operational capabilities; Dyad': 438,
     'China; Servitization; MOA; Service network': 439,
     'Case study; Servitization; Business model; Advanced services': 440,
     "Performance measurement; Service; Network; Value; Industrial service; Manufacturers' strategies": 441,
     'Manufacturing strategy; Servitization; Capabilities; Financial performance': 442,
     'Products; Service; Servitization; Product biography; Circular economy; Internet of Things; Repair': 443,
     'Servitization; Advanced services; Capabilities; Complementary capabilities; Network actors': 444,
     'Servitization; Emerging economies; Economic context; Service provision; Survey; Financial performance; Service return; Service paradox': 445,
     'Servitization; Deservitization; Integrated solutions; Knowledge-based view; Capabilities; Theoretical framework': 446,
     'Servitization; Digitization; Interdependences; dPublishing industry; Payment card': 447,
     'retailer; internationalisation; satisfaction; loyalty; manufacturer': 448,
     'manufacturing service ecosystem; virtual enterprise; servitisation; manufacturing engineering; service operation; product design': 449,
     'Business service; information and communication technologies (ICTs); manufacturer and supplier collaboration; UK telecommunication manufacturing': 450,
     'servitisation; social capital; knowledge management; supply chain management; empirical study': 451,
     'Service strategy; Demand-side search; Service implementation; Service-orientated human resource management practices': 452,
     'B2B services; B2B customer experience; Outcomes-based measures': 453,
     'Servitization; Text mining; Form 10-K; Annual report; Positioning map; Self-organizing map': 455,
     'Manufacturing; Servitization; Outsourcing; Global value chain': 456,
     'Software pricing; Servitization; Competitive strategy; Cloud computing; SaaS': 457,
     'Performance-based contracting; Contracts; Servitization; Relationships; Integrated solutions; Procuring complex performance': 458,
     'Servitization of manufacturing industries; Cloud-based manufacturing business model; Service-oriented manufacturing; Cloud manufacturing; Down-to-earth path; Enabling technologies': 459,
     'Aerospace; Business solution; Market shaping; Relationship; Servitization': 460,
     'Servitization; Business model innovation; Service innovation; Value chain': 461,
     'servitization; service infusion; service network; strategy; dynamic competition': 462,
     'Servitization; Resource realignment; Dynamic capabilities': 463,
     'Relationship dynamics; Business-to-business relationships; Servitization; Strategy-as-practice; Adaptation': 464,
     'Energy efficiency; Servitization; Contracting; ESCo; LED; Lighting': 465,
     'Servitization; Service infusion; Network orchestration; Platform; Service network; Solution network': 466,
     'Servitization; Service transition strategy; Case study': 467,
     'Servitization; Systems integrators; Activity theory; Construction industry': 468,
     'Social manufacturing; manufacturing service; enterprise relationship network; network clustering and analysis; manufacturing community': 469,
     'utility; EUCo; Business model innovation; Servitization; Energy service; Asset transformation': 470,
     'Product-centric service supply; Knowledge-centric service supply; Customer perceived value; Customer involvement': 471,
     'manufacturing servitization; TAM Model; product development model; intention to use; attitude; perceived ease of use': 472,
     'Servitization; Channel competition; Game theory': 473,
     'Market-linking capability; Market turbulence; New product performance; Servitization service innovation': 474,
     'business; knowledge management; project management': 475,
     'Statistical process control (SPC); Service; Responsive process': 476,
     'Cloud manufacturing (CMfg); Service selection and scheduling; TQCS; Relative superiority degree; Analytic hierarchy process (AHP); Ant colony optimization (ACO)': 477,
     'Servitization; Industrial services; Operations risk management and resilience': 478,
     'Servitization; Co-production; Ecology; Green operations; Suppliers; Green manufacturing': 479,
     'Servitization; Manufacturing; Multiple-case study; Remote monitoring technology': 480,
     'Product servitised supply chain (PSSC); product-service value (PSV); e(3)value modelling; servitisation paradox': 481,
     'resources servitisation; on-demand supply; service object-oriented architecture; multidimensional cloud service semantics': 482,
     'Interfunctional Coordination (IFC); Market Orientation; Services Provided by Manufacturers; Service Offering; Servitization; Manufacturers of Electrical Equipment and Electronic Components; Czech Republic': 483,
     'Motivation; Capabilities; Complexity; Resources; Servitization; CoPS': 484,
     'Information; Mass customization; Smart appliance': 485,
     'Product-enabled services; Service value attributes; Principal component analysis; K-means clustering; Latent semantic analysis; Online textual data': 487,
     'servitization; challenges; alignment; case studies': 488,
     'Delphi study; transformation; servitization': 489,
     'value network; servitisation; challenges; service manoeuvres; manufacturing firms; case study': 490,
     'product system; servitisation; innovation; manufacturing firm; service system': 491,
     'data presentation; PSS; product avatar; PLM; individualised service generation; servitisation': 492,
     'Through-life engineering services; maintenance; repair and overhaul; service knowledge feedback': 493,
     'Servitization strategies; Value chain; Organizational structure': 494,
     'internationalisation; outsourcing/offshoring; supply chain; servitisation; business models': 495,
     'Life-cycle service offering; Industrial service business; Service-dominant logic; Service infusion; Service business models': 496,
     'R&D alliances; R&D institutes; policy dilemma': 497,
     'Servitization; Big Data; Manufacturing; Competitive advantage; Value; Information': 498,
     'Service transition; Solutions; Manufacturing companies; Service strategy; Problematization methodology': 499,
     'Service management; Servitization; Maturity model; Work organization': 500,
     'Service; Organizational change; Servitization; Operations strategy': 501,
     'Business model; sociomateriality; open innovation; actor-network theory': 502,
     'Servitization; Dynamic capabilities; Competitive advantage; Resource-based view; Relational view; Service infusion': 503,
     'Building information models (BIM); Case studies; Digital infrastructure; Information systems; Life cycle; Systems management': 504,
     'Performance; Servitization; Industrial services; Service orientation; Service strategy; Service structure': 505,
     'Servitization; Knowledge management; Case study; Knowledge management systems': 506,
     'family business; family firms; services; servitisation; SEW; socio-emotional wealth; manufacturing; Europe': 507,
     'Servitization; Capabilities; Resources; Service infusion; Manufacturer; Resource configurations': 508,
     'Public sector; Manufacturing; Intellectual capital; Industry policy; Servitisation': 509,
     'Customer orientation; Business process management; Servitization; Customer centricity; Service blueprint': 510,
     'Servitization; Procurement; Marketing purchasing integration': 511,
     'Operations management; Literature review; Theory': 512,
     'Servitisation; Business-to-business; Domino effect; Service failure': 513,
     'servitization; third-party performance; manufacturing; image risk; external partner': 514,
     'e-store; small- and medium-sized enterprise; supply chain; order-delivery process; modular service architecture; service process design; modularization; variation; reuse': 515,
     'Foresight; Trend analysis; Service-dominant logic; Value co-creation; Magazine publishing': 516,
     'Business models; Technology shift; Electric road system (ERS); Servitization; Business model dilemma; Truck manufacturers; Strategy': 517,
     'Manufacturing Servitization; Service Design; Service Innovation Model; Clustering Model': 518,
     'servitisation; knowledge intensive services; structural change': 519,
     'business marketing; B2B; industrial marketing; manufacturer; service offerings; service infusion; servitization; strategy': 520,
     'Service-dominant logic; Networks; Servitization; Value propositions; Solutions; Truck industry': 521,
     'Business model; Service innovation; Capabilities; Servitization; Product-centric firms; Service infusion': 522,
     'service occupations; servitization; intermediate service inputs; global sourcing; manufacturing sector': 523,
     'Servitization; Service infusion; Industrial service; Case study; Organizational ecology': 524,
     'service; disintermediation; industrial marketing; intermediaries; servitization; industrial marketing; business marketing': 525,
     'Servitization; Open service innovation; Business model; Performance': 526,
     'Servitizing; Networking; Large-scale survey findings; Cluster analysis; Multifactorial regression': 527,
     'Network design; Queueing; Remanufacturing; Contracting': 528,
     'Servitization; Customer linking; Demand management; Supply chain management; United Kingdom': 529,
     'servitization; integrated solutions; operations strategy; financial performance': 530,
     'Service supply chains; Triads; Industrial services; Manufacturing industries; Supply chain management': 531,
     'R&D services; R&D collaboration; Customer relationship management; Value co-creation; Customer involvement': 532,
     'Industrial services; Information technology; Communication technologies; Management strategy; Service business orientation; Service orientation; Differentiation; Servitization': 533,
     'Service innovation; Network; Penrose; Service factory; Servitisation': 534,
     'R&D consortia; Survival analysis; Taiwanese firms': 535,
     'Brand equity; Customer satisfaction; Internal service quality; Servitization; Social networking': 536,
     'Service complexity; Task discretion; Co-creative operations': 537,
     'Integrated product-service (IPS); Servitization; New product-service development (NPSD); Taxonomy; Typology': 538,
     'Manufacturing technology adoption; Service; Servitization; Manufacturing industries; Technology management': 539,
     'Innovation; Manufacturing; Service; Servitisation': 540,
     'Buyer-supplier relationships; Servitization; Supply chain management; Integrated solutions; Buyer-seller relationships': 541,
     'Manufacturing industries; Process management; Operations and production management; Process mapping; Service organization; Manufacturing companies; Servitisation': 542,
     'Repertory grid technique; Qualitative methods; Manufacturer-supplier relationships; Servitization; Supply chain management; Supplier relations': 543,
     'Management accounting; Decision making; Manufacturing industries; Servitisation; Service; Accounting object(s)': 544,
     'Servitization; Business game concept; Change management; Customer awareness; Quality management': 545,
     'Capacity management; Industrial services; Operations strategy; Sales and operations planning; Servitization': 546,
     'Servitisation; Co-production; Music industry': 547,
     'product extension services; configuration; ontology; OWL; SWRL': 548,
     'servitization; service delivery; ICT': 549,
     'Microfluidics; Microfluidic devices; Services; Service-orientation': 550,
     'Machine tools; Servitization; Prognostics and health management; Modelling; Life-cycle costs': 551,
     'Change; environment; globalization; organizational transformation; servitization; competitive advantage; strategic service management; services as profit centers': 552,
     'Product centric; Services; Product attached; Operations; Servitization': 553,
     'survey; servitization; service strategy; manufacturing; B2B': 555,
     'manufacturing; products; service; servitization; model; casestudy': 556,
     'Manufacturing industries; History; Service industries; Operations management': 557,
     'Building technology; Skills shortages; Construction industry': 558,
     'supply chain; products; services; servitisation; case study': 559,
     'Industrial Product-Service System; risk management; dynamic Bayesian network; entropy reduction value; risk analysis': 561,
     'CNC machine tool; product service system; supplier involvement; service planning; remanufacturing': 562,
     'Digital twin; large-scale automated high-rise warehouse; warehouse product-service system; storage assignment; cyber-physical systems': 563,
     'product service system; product service system board; generator set': 565,
     'Product service system; Service evaluation; Decision-making; PSS opportunities; PSS drawbacks': 566,
     'manufacturing outsourcing; cutting-tool delivery; industrial product service system; economic order quantity model; genetic algorithm': 567,
     'Product-Service Systems; Product design; Design knowledge; Knowledge reuse; Knowledge management': 568,
     'product-service systems; diversity of terms; practical examples; research projects': 569,
     'Remanufactured products; Sustainable system; Competition; Product design; Game theory': 570,
     'Product-service system; PSS strategy; PSS classification; PSS matrix': 571,
     'systematic review; product service systems; PSS frameworks; PSS case studies; PSS trends': 572,
     'Product design; task planning; supplier participation; product service system': 573,
     'CNC; product-service system; module division; configuration modeling; design structure matrix; integration model': 574,
     'Product service system; product modular design; life cycle; cluster analysis; CNC machine tools': 575,
     'Sustainable product-service system; Triple bottom line; Fuzzy set theory; Decision-making trial and evaluation; laboratory (DEMATEL); Analytical network process': 576,
     'Product-service system; lean; leanness; assessment; comparison': 577,
     'Forecasting; Product-service systems (PSS); Revenue model; Revenue sources; Service delivery; Co-location; Collaboration': 578,
     'Service innovation; Sustainable product service systems; Fuzzy delphi method; Fuzzy importance and performance analysis; Analytical network process': 643,
     'capability; capability readiness; product-service systems; system readiness': 580,
     'Product service systems; existing product; spiral evolutionary design methodology; system evolution chain; system performance': 581,
     'battery swapping; electric scooters; Internet of Things; product service system; visual mapping method': 582,
     'Product-service systems; service supply chain; supply chain model; service providing; service-oriented manufacturing': 583,
     'business model innovation; circular economy; product-service system (PSS); configuration; action research': 584,
     'Manufacturing areas; Product-service systems; KIS; Industry 4; 0; Place-based development': 585,
     'Product-Service Systems; Value network configuration; Simulation; Performance evaluation; Servitization': 586,
     'Smart product-service systems; Digital technology; Sustainable innovation; Fuzzy delphi method; Diffusion of innovation theory; Decision-making trial and evaluation laboratory (DEMATEL)': 587,
     'Product service system; oil consumption customer churn; support vector machine; gray TOPSIS group; optimize algorithm': 588,
     'hybrid value creation; hybrid products; product-service systems; engineering methodology; machine and plant construction; technical customer services; mobile application systems': 589,
     'Industrial Product-Service Systems; Product-service systems; Business models; Service delivery; Organization': 590,
     'Product-service systems; Concept generation; TRIZ; QFD; Car sharing service': 591,
     'Sustainability; Service; Value; Impact; Evaluation; Efficiency': 592,
     'Servitization; Product-service systems development; Process characteristics; Stakeholders; Collaboration': 593,
     'Sustainable product-service systems; Sustainability; Consumption; Practice theory; Sustainable design': 594,
     'Product-Service System; Mass Customization; Ontology; Product Configuration': 595,
     'Drying and storage of grains; Product-service system; Survey; Farmer wish to invest; Perceived value; Green energy': 596,
     'product-service systems design; manufacturing servitization; representation framework; classification of product-service systems': 597,
     'Product service system; radio frequency identification device; knowledge management; risk': 598,
     'Product-service systems; Health care; Drug-device combination; Market establishment': 599,
     'intermediate devices; product-service system; smartphone application': 600,
     'Smart product-service systems; Service innovation; TRIZ (Theory of Inventive Problem Solving); Service blueprint; Smart beauty service': 601,
     'product-service-systems; communication materials; stakeholder network; solution design; strategic design': 602,
     'product service system; PSS; circular economy; LCA; merino wool': 603,
     'Warehouse product service system; inventory items; configuration scheme; safety stock': 604,
     'Delphi method; Fuzzy analytic hierarchy process; Sustainability': 605,
     'Circular economy; Product-service system; Obsolescence; Resource flows; Closed-loop; Function analysis': 606,
     'Sustainable Product-Service System; Product-Service System; Small and Medium-sized Enterprises; Servitization; Literature review': 607,
     'Logistics service providers; new product development; outsourcing; product-service system; early supplier involvement': 608,
     'Product service system; Domain mapping; Quality function deployment; House of quality; CNC machine tools': 609,
     'Product service system (PSS); PSS design methodologies (PSS-DM); PSS evaluation methodologies (PSS-EM); PSS operation methodologies (PSS-OM)': 610,
     'smart home; smart home appliance (SHA); product-service system (PSS)': 611,
     'Preliminary design; Product Service Systems; Color-coding; Value visualization; Decision making; Lifecycle value': 612,
     'Product service system; Multi-agent system; Personalisation; Pervasive computing': 613,
     'Environmental sustainability; Maturity model; Best practices; Product/service-systems; Ecodesign': 614,
     'Renting; Product service system; Simulation; Decision making': 615,
     'Product service system; analytical hierarchic process; case-based reasoning': 616,
     'product-service system (PSS); PSS architecture; system interface modeling; system of systems engineering': 617,
     'Product innovation; Product-Service System; Customer acceptance; Electric vehicles; Car sharing': 618,
     'Service; Lifecycle; Industrial Product-Service Systems': 619,
     'Reverse logistics; Product service system; Servitization; Service-dominant logic; Supply chain collaboration': 620,
     'co-creation; product-service systems; circular product design; washing machines; access models; impact': 621,
     'product-service paradoxes; sustainable product service; product-service system innovation; product-service systems design methodology; product-service systems business model': 622,
     'product-service system (PSS); quality function deployment for product service system (QFDforPSS); medical devices; fuzzy analytic hierarchy process (FAHP); screening life cycle modeling (SLCM); customization; life cycle assessment (LCA); circular economy (CE)': 623,
     'Joint optimization; reliability design; spares inventory; replacement maintenance; availability; product-service system': 624,
     'Product-service systems; Design for sustainability; Consumer perception; Clothing': 625,
     'design for sustainability; Product Service Systems; ICT; SME; design; green operations; organizational transformation': 626,
     'product service system; multi-attribute utility analysis; carbon dioxide reduction; maintenance service level': 627,
     'Product-service system (PSS); Sustainability; Dynamic; Multidimensional; Triple bottom line (TBL); System dynamics (SD)': 628,
     'Product-service-systems (PSS); Design process; Design method': 629,
     'solution oriented partnership; product service systems design methodology; representation of Product service systems': 630,
     'Product-service System; Conceptual Model; Life Cycle; Methods and Tools': 631,
     'Product-Service System; concept design; manufacturing; servitization; player interaction; prioritization': 632,
     'Product service system; Interpretive structural model; Remanufacturing; Reverse logistics; Maintenance': 633,
     'product service systems; policy measures; functional program': 634,
     'product-service system; life cycle assessment; rental clothing; environmental impact; sustainable business model; consumer behaviour': 635,
     'Distributed generation (DG); Distributed Renewable Energy (DRE); Product-Service Systems (PSS); Classification system; Business model; Design': 636,
     'PSS; Environmental performances; User satisfaction; Innovative eco-design; Functional analysis': 637,
     'innovation management; product service system; transition path; sustainable mobility; dynamic capabilities; car-sharing': 638,
     'Information flow; Product-service systems; Diagrammatic modelling; Function-oriented design': 639,
     'product-service system (PSS); service design; quality function deployment (QFD); customer requirements; medical devices; regulated market': 640,
     'Product-service system; System Dynamics; assessment model; dynamic behavior; guidelines': 641,
     'joint optimization; maintenance; multiple failure modes; product-service system; spares ordering policy': 642,
     'Smart product service system; Natural language processing; Machine learning; Recommendation system': 644,
     'Service Design; System Design; Product-service System; New Product Development; Stakeholder Involvement': 645,
     'Product-Service System; PSS; New Product Development; New Service Development; Stakeholder Engagement; PSS Characterization; Early Stage; ICT; Healthcare': 646,
     'product service system; PSS; PSS design; co-creation; PSS redesign; PSS business model': 647,
     'product service system; configuration; customer perception; rough set; rule extraction': 648,
     'Product Service Systems (PSS); design process; evaluation': 649,
     'Design for human safety; Work situation; Product-Service System; Function-Behavior-Structure': 650,
     'Business development; cooperation; design process; information and communication technology (ICT); innovation; product service systems (PSS); small and medium enterprises (SMEs)': 651,
     'Product-service systems; Customer value; Servitization; Framework': 652,
     'sustainability; product-service systems; design process; circular economies': 653,
     'Electric mobility; Electric vehicle; Smart city; Platform service; Business model; Product service system': 654,
     'Design game; Ultra-Personalised Product Service System; shoe-making; research through design; models of production': 655,
     'Product service system; scheme selection; digital twin; fuzzy assessment': 656,
     'Manufacturing; Servitization; Product-service systems; Product-related services': 657,
     'constraint satisfaction problem; ontology; product-service systems; product-service system customization; recommender systems': 658,
     'product-service systems; customer preference; clustering; data mining; design for service; machine learning; big data': 659,
     'Product-service system (PSS); Customer value; Customer experience cycle; Evaluation; Analytic network process (ANP); Niche theory': 660,
     'Servitization; Digitalization; Product-service systems': 661,
     'adaptability; adaptive processes; goal-oriented process modeling; intelligent agents; engineering change management (ECM); industrial product service systems (IPS2)': 662,
     'product-service systems; sustainability; functional economy': 663,
     'customer journey; servitisation; product service systems; PSS; business-to-business; B2B': 664,
     'Value bundle; Product-service system; Conceptual modeling; Reference model; Modeling language': 665,
     'Product-Service System; sustainability; SDG 9; SDG 12; barriers; Brazil': 666,
     'knowledge management; new product development; stage-gate; aerospace; product-service system': 667,
     'Product-service system; Software engineering; Model driven; Tool integration; Integrated Development Environment; Manufacturing': 668,
     'Data-driven services; digital servitization; conceptual framework; PSS; typology': 669,
     'Product-service systems; Circular economy; Leasing; Consumer behavior': 670,
     'Business model innovation; servitisation; product-service system; digitalisation; typology; taxonomy': 671,
     'Product-Service Systems (PSS); customisation; manufacturing systems; customisation degree; modelling; quantification': 672,
     'sustainable innovation; production platforms; production consumption; product-service system; partnership building': 673,
     'product service system; institutional isation; sustainable consumption; sharing systems; collective use; socio-cultural context': 674,
     'product-service system; servitization; review': 675,
     'Product-service system; Recommender system; Case representation; Case indexing; Similarity measurement': 676,
     'product-service system; PSS; technological chance; KeyGraph; text mining; business method patent; BM patent; chance identification; chance discovery; database-centred approach; PSS innovation; mobile industry': 677,
     'circular economy; decoupling; industrial ecology; life cycle thinking; product-service system (PSS); rebound effect': 678,
     'firm performance; process innovation; product innovation; product-service system; servitization; technological innovation': 679,
     'Service Engineering; Product-Service System; Discrete-event simulation; Service design; Customer value; Service development': 680,
     'Product service system; Association rules; Multi-dimension and multi-objective double; group discrete Firefly Algorithm (MODGDFA); Pareto rules': 681,
     'Computer-aided design; integrated product service system; integration design; knowledge engineering': 682,
     'Product service system; sustainable design and development; plastic mold; analytical hierarchy process; dematerialization': 683,
     'household waste prevention; waste electrical and electronic equipment; product service systems': 684,
     'Product service system; Configuration rule; Rule mining; Imperialist competition algorithm': 685,
     'Ballast water; Shipping; Product-service systems; Eco-innovation; Eco-efficient value creation': 686,
     'circular economy; mining industry; product-service systems; servitization; sustainability': 687,
     'Product-service system; Customer requirement; Sustainability; Multi-dimensional value; Grey DEMATEL-ANP': 688,
     'Product-service systems (PSS); servitization; business model (BM); framework; SMEs': 689,
     'Design alternative evaluation; Product service systems; Variable precision rough set; Decision analysis; Rough weighted geometric mean': 690,
     'Product-service systems; Service delivery; Uncertainty; Cost estimation': 691,
     'Product-service systems; Critical success factors; Electric car': 692,
     'Product-service system (PSS); PSS Board; PSS visualization': 693,
     'product-service system; event-related potentials (ERPs); decision making; perceived value': 694,
     'Digital transformation; Rehabilitation assistive devices; Smart product-service systems; Synthetical design; Rehabilitation assistive smart product-service; systems': 695,
     'Software agent; Service enabler; Product service systems': 696,
     'Circular supply chains; product-service systems; business model innovation; circular business models; circular economy': 697,
     'Functional economy; Product-service system; Service simulation; Servitization; G-DEVS; HLA federation': 698,
     'Product service systems; Value creation; Value-in-use; Access; Sharing economy; Car sharing': 699,
     'circular business model; business model innovation; customer development; product-service systems; circular economy; remanufacturing': 700,
     'Material Footprint; sustainability strategies; social limits to growth; individual welfare; sustainable design; product service-systems': 701,
     'Product-service systems; Digitization; Adoption of innovations; Institutional theory; Business-to-business marketing; Internet of things': 702,
     'Product-service systems; Servitization; Service transformation; Industry 4; 0; Digitalisation; Research agenda': 703,
     'Smart Product Service System; Text Analytics; BERT; Service Blueprint': 705,
     'business model; business modelling; morphological analysis; morphological chart; Product-Service System (PSS); case-based system': 706,
     'product-service system; customer requirements; systematic literature review': 707,
     'product service system; configuration; customer needs; customer perception; support vector machine': 708,
     'original equipment manufacturer; operational practices; servitization; maintenance': 709,
     'sustainable consumption; consumption pattern; product service systems (PSS); emotional design': 710,
     'product-service systems; design for service; manufacturing strategy; servitisation; upscaling': 711,
     'Servitization; Servitization scale; Servitization level; Service infusion; Product-service systems': 712,
     'design support; design knowledge; knowledge management; product-service systems': 713,
     'manufacturing SMEs; product-service systems; an iterative design method; existing components; acceptability and sustainability': 714,
     'product-service systems; territory; sustainability; circular economy; literature review': 715,
     'Smart product-service system; Sustainability; Knowledge management; Reversible design; Context-awareness': 716,
     'Industrial product-service systems; Condition monitoring; Fault diagnostics; Support vector machines': 717,
     'Design method; Service; Evaluation': 718,
     'Model-based systems engineering; Traceability; Product service systems; Model integration': 719,
     'Grey-DEMATEL; Product-service system; Circular economy; Resource-based view; Stakeholder theory; End-of-Life; Resource-dependence theory': 720,
     'Technical Product-Service Systems; Cumulative Energy Demand; Sustainability': 721,
     'product-service systems; new product development; after-sales service; word': 722,
     'product-service system (PSS); product-service relationship (PSR); product-service performance (PSP); evaluating': 723,
     'Sustainable product service system; Servitization; Life-cycle cost analysis; Design for environment; Product selling and leasing model design': 724,
     'economic assessment; system approach; algorithmic; platform; product-service systems; sensitivity': 725,
     'model-driven design; Product-Service Systems; conceptual design; design exploration; visualization; decision-making; digitalization': 726,
     'Community transformation; Soft systems methodology; Product-service systems; Synergism; Sustainability; Service innovation': 727,
     'Service Engineering; Product-Service System; Design support; Knowledge; Computer-aided design': 728,
     'Service; Conceptual design; Design experiment': 729,
     'Production planning; Coordination; Service': 730,
     'circular economy; materialities; product service system; prosumer': 731,
     'Product-service system; Business game; Game-based learning; Engineering education; Facilitation tool': 732,
     'social capital; innovation; needs; product service system; LETS; sustainable consumption': 733,
     'Sustainability; Life cycle modelling; Product-service system; Customer satisfaction; Maintenance management; Medical devices': 734,
     'Product-service system; Aerospace manufacturing; Knowledge technology; Ontology-based representation; Requirement analysis': 735,
     'IoT; product-service system; business model; capabilities': 736,
     'Infant care products; Consumer culture theory; Diffusion; Product service systems; Sustainable futures': 737,
     'vending machine PSS; product-service system (PSS); integrative framework; innovation management': 738,
     'product service systems; functional product development; integrated solutions; engineering methods': 739,
     'Product/service systems; Design navigator; Design method; Circular economy; Lifecycle perspective; Life cycle assessment': 740,
     'Sustainable product service systems; Circular economy; Mobile phones; Practices': 741,
     'Product-Service-System Configuration; Mass Customization; Solution Space Modeling': 742,
     'territorial servitisation; TS; place leadership; local productive systems; LPSs; lock-in condition; rerouting; product-service systems': 743,
     'product-service system (PSS); servitisation; supply chain management; supply chain design': 744,
     'Service Engineering; Product-Service Systems; Literature review': 745,
     'Networked collaborative manufacturing; Mass customization; Mass personalization; Product service system': 746,
     'circular design; circular economy; product lifecycle; intervention mapping; scenario matrix; sustainable futures': 747,
     'Product/service-systems; Life cycle assessment; Environmental evaluation; Rebound effects; Environmental impact; Circular economy': 748,
     'Product-service system; Life cycle; Requirements engineering; Dynamic system environment': 749,
     'Product Service System; Maintenance, repair and overhaul (MRO); Service model; Knowledge representation; Aerospace manufacturing': 750,
     'Lifecycle; Modular design; Reconfiguration': 751,
     'Product-service system; Functional performance; Dynamics; System dynamics': 752,
     'Knowledge management; Web 2.0; Product-service systems; Knowledge life cycle': 753,
     'Product-service system; Impatient customers; Markov chain; Additional service capacity': 754,
     'Advanced service; performance-based service; product-service systems; FMEA; rating AHP': 755,
     'mobile payment services; product-service system; PSS; continuance intention; empirical study': 757,
     'Co-creation; co-synthesis; emergence; public product service system': 758,
     'Product service systems; Design structure matrix; Engineering change management; Data-driven design; Three-way decision making; Digitalization': 759,
     'Product-service systems; Typology; Functional decomposition; Functional Hierarchy Modeling': 760,
     'product service system (PSS); servitisation; service engineering (SE); failure mode and effects analysis (FMEA); grey relational analysis (GRA)': 761,
     'Servitization; Product-service system; Emerging countries; Contingent factors; Business model; Financial aspects': 762,
     'Staff well-being; reward system; servitization; staff motivation; behavioural operations; organisation culture': 763,
     'new service development; service innovation; maturity model; case studies; product-service systems; product-centric companies': 765,
     'Environmental preservation; Sustainability; Sustainable product service system; Water management practice; Water resource': 766,
     'off-site manufacturing; servitization; sustainability; industrial construction; product-service system; business model innovation': 767,
     'Product service systems; Sustainability; Environmental consciousness; Sharing economy; Perceived stockout risk': 768,
     'automotive industry; sustainability; product-service systems; micro-factory retailing': 769,
     'Quality management; Industrial product service systems (IPS2); Product-service systems (PS2); Value creation chain': 770,
     'service design; product-service systems; design methodology; design research': 771,
     'Manufacturing firms; Innovation management; Eco-innovation; Open-innovation; Product-Service Systems': 772,
     'product architecture; PSS; Product-Service Systems; modularization; MBSE; architecture optimization; data management; versioning; data continuity': 773,
     'Smart Product Service System; Sustainability; Rough Set theory; Best Worst Method; Criteria Importance Though Inter-criteria; Correlation': 774,
     'Sustainability; Industrial product service system (iPSS); Resource service scheduling; NSGA-II': 775,
     'Product-service systems; systems engineering; integrated design; interface modelling approach; multi-disciplinary integration': 776,
     'Product-service system (PSS); sustainable transports; electric car sharing; measurement; sustainability': 777,
     'Smartphones; Leasing; Product-service system; Discrete choice experiment': 778,
     'Uncertain demand; warehouse product service system; interval number optimization; service cost; maximum profit': 779,
     'Complexity; Customization; Product-service systems (PSS); Industry 4.0': 780,
     'Sustainable product service system; Warranty; End of life; Genetic algorithm': 781,
     'Product-Service System (PSS); Fashion; Waste; Dematerialization; Sustainability; European waste hierarchy': 782,
     'demand estimation algorithm; revealed preference; distributed systems; product service systems bike share; data-driven design': 783,
     'Requirements interaction; Product-service system (PSS); Group DEMATEL; Rough set theory; Group decision making': 784,
     'Product-service system; integrated design; conceptual framework; service engineering; product-service integration': 785,
     'Product-Service System (PSS); Evaluation method; Service innovation; Model feasibility': 786,
     'Dynamic cellular manufacturing system (DCMS); Product-service system (PSS); Configuration and operation architecture; Function block (FB); Web services': 787,
     'Product-service systems; Servitization; TRIZ; Sustainable product-service systems; Systematic innovation; Eco-innovation; Literature review': 788,
     'Life cycle analysis; Product-service system; Waste electrical and electronic equipment': 789,
     'Product-service systems; Text mining; Latent dirichlet allocation; Topic landscape; Literature review': 790,
     'product-service systems; PSSs; compressed air; energy efficiency; multi-criteria decision making; MCDM; conflicting objectives': 791,
     'requirement evaluation; industrial product-service system (IPS2); industrial customer activity cycle (I-CAC); rough group analytic hierarchy process (AHP)': 792,
     'Smart product-service system; User satisfaction; Design iteration; Digitalization; Machine learning': 793,
     'Product service system; Case study; Avionics; Availability; Defence; Conceptual model': 794,
     'Collaborative consumption; Pro-environmental behaviour change; Product-service systems; Social practice theory; Social psychology; Values': 795,
     'Product service system; data mining; product monitoring; knowledge management; servitization': 796,
     'Product-service system; Business model; Machine tool manufacturer; Case study': 797,
     'Product-service system; Integrated design; Engineering models': 798,
     'Product-service systems; Design; Reliability; FMEA; Business opportunity': 799,
     'Smart product-service systems; Concept evaluation; User experience; Information axiom; Context-awareness': 801,
     'product-service system (PSS); ecosystem theory; multiple stakeholder; fuzzy analytic network process (fuzzy ANP); quality function deployment (QFD)': 802,
     'Prototyping; Collaborative Design Tools; Product Service Systems; Healthcare': 803,
     'Product-Service System; Closed-loop-design; PSS Conceptual Design': 804,
     'Product-service system; PSS; Sustainable business models; Sustainable value; PSS archetypes; Servitization': 805,
     'Smart product-service system design; Physical Internet; Intelligent interoperable logistics; Service-orientation; Logistics-as-a-Service; Sustainability': 806,
     'Life Cycle Simulation; Product-Service Systems; Reference architecture': 808,
     'Product-service system; Eco-design; Waste management; Machine-to-machine; Life cycle analysis': 809,
     'product-service systems; design; interoperability; ontology': 810,
     'Industrial services; Contracts; Uncertainty; Sustainability; Transformation': 811,
     'Smart product-service system (PSS); Innovation value propositions (IVP); Neutrosophic set; DEMATEL; CRITIC': 812,
     'African HEIs; Curriculum development; Sustainable Product-Service System; Distributed Renewable Energy; Learning resources; Open source and copyleft': 813,
     'Sustainable product service system; Re-use; Waste hierarchy; Social enterprise; Eco-efficiency; WISEs': 814,
     'Product-service system (PSS); Quality function deployment for product; service system (QFDforPSS); Analytic network process (ANP); Customer requirements; Decision making; Medical devices': 815,
     'personal construct psychology; product service systems; pushchairs; sustainable consumption; repertory grid technique': 816,
     'Product Service System; Service Engineering; Product development; Lean design; Service systems; Knowledge-based systems': 817,
     'user-generated contents; quality determinants; product-service systems; topic modeling; car-sharing': 818,
     'Logistics services; Product service systems; Auction; Cloud service allocation and sharing': 819,
     'Industrial big data; Big data; Product service system; System engineering; Fuzzy DEMATEL; Smart manufacturing': 820,
     'Service Engineering; Methodology; Industrial application; Simulation; Product service system; Case study': 821,
     'product-service systems; Industry 4.0; circular economy; digital services; innovation; socio-technical systems; digitalization': 822,
     'product service system; PSS classification; transition method; rough set theory': 823,
     'Data-driven design; Smart; Connected product; Digital twin; Product-service systems; Value creation': 824,
     'Diffusion; Innovation; Laundry; Lighting; Social practice; Sustainable product-service system': 825,
     'Trademark; Patent; Product-service system; Link prediction; Business ecology network': 826,
     'Infant mobility products; Product Service System; Resource efficiency; Socio-technical experiments; Strategic Niche Management': 827,
     'product service system; configuration evaluation; cost estimation; life cycle costing; PSS configuration': 828,
     'product-service systems; circular economy; concept design; multi-criteria decision making; sustainability': 829,
     'Prefabricated housing construction; Smart product-service systems; Blockchain; Internet of things; Sustainability': 830,
     'Life Cycle Assessment; Product/Service-Systems; Environmental impacts; Reference system; Functional unit; System boundaries': 831,
     'product-service systems; sustainability; sustainable business models; business model innovation': 832,
     'Product-service system; Ethical; Sustainable; Preventive maintenance; Sharing; Remaining effective life': 833,
     'Product Service System (PSS); service-oriented PSS development process; English education; Analytic Hierarchy Process (AHP); Quality Function Deployment (QFD)': 834,
     'product-service system; distributed manufacturing; future scenarios; design tool; design research methodology': 835,
     'Chinese Manufacturing; Servitization; Product service system; Co-creation; Service innovation; Belt and road initiative': 836,
     'product service system (PSS); availability; field repair kit': 837,
     'collaborative networked organisation; product-service systems; virtual enterprise; value co-creation; complex networks; conceptual modelling; graph theory': 838,
     'design; innovation management; product development; design for service; design for manufacture; knowledge management': 839,
     'Product-Service Systems; Design; Neural network': 840,
     'Large technical systems; Business model; Technology diffusion; Product/Service System; Ecodesign': 841,
     'Innovation; Multilevel design process; Transition management; Sustainable transport; Electric vehicle; Product-service system': 842,
     'Product-Service System (PSS); sustainability assessment; multi-criteria analysis; decision-making; early design': 843,
     'Product-service system (PSS); Evaluation scheme; Evaluation criteria': 844,
     'data analytics; data warehousing; decision support systems; product-service systems (PSSs); product-service systems customization; product usage data; recommender systems (RSs); sensors': 845,
     'product lifecycle; system lifecycle; lifecycle management; circular economy; product-service system; multi-disciplinarity; product classes and instances; closing material and information loops': 846,
     'mt-iPSS; Machine tool; Machining capability; Service': 847,
     'actor networks; institutions; resource productivity; sociotechnical': 848,
     'information design; protocol analysis; color-coded 3D models; conceptual design': 849,
     'Business; Companies; Manufacturing; Interviews; Faces; Context modeling; Encoding; Advanced services; alignment; business model; product-service systems (PSS); servitization; value destruction; value leakage': 850,
     'Sustainability; Design; Food system; Agriculture': 851,
     'Product-Service System; Prognostics and Health Management; Online simulation; Dynamic behaviour': 852,
     'Product-service systems; crowd sensing; value co-creation; decision-theoretic rough set; data-driven design; servitization': 853,
     'Operational data; Value creation; Smart Product-Service System; Operational context; Systematic Literature Review': 854,
     'Product-service systems; Solar PV; System Dynamics Modeling; Bass Diffusion Model; Diffusion theory': 855,
     'sustainable consumption; product-service systems (PSSs); value proposition; practice definition; sustainable fashion; waste; sustainable business models; circular economy': 856,
     'Sustainable product-service systems; Product-service value; Fuzzy delphi method; Fuzzy interpretive structural modeling; Best-worst method': 857,
     'Product Service System (PSS); servitization; productization; symbiosis; enterprise management; conceptual framework; strategic change': 858,
     'Product service systems; Customer characteristics; Local Cluster Neural Network; Rules extraction': 859,
     'circular economy; business model innovation; product-service systems': 860,
     'Product-service system; Requirement interactions; Rough-fuzzy DEMATEL; 2-additive fuzzy measures; Choquet integral': 861,
     'Product-service systems; Sustainability; Actor networks; Territory; Network resources; Circular economy': 862,
     'Product-Service System; customer satisfaction; Quality Function Deployment; Axiomatic Design; modularisation': 863,
     'Servitization; Systems development; Service systems': 864,
     'case study; design process; human factors; service design system design': 865,
     'Carsharing service; Simulation tool; Fuzzy classification; Service prioritization; Product-service systems (PSS)': 866,
     'product service systems; modular development; PSS development; service design': 867,
     'Energy Demand Management; adaptive Scheduling; manufacturing; product Service Systems; servitization': 868,
     'Singapore; Transition': 869,
     'Product-service system (PSS); Engineering characteristics (EC); Fuzzy pairwise comparison; Data envelopment analysis (DEA); Kano model; Non-linear programming': 870,
     'product-service system; key performance indicator; assessment software; life-cycle performance management; PSS evaluation': 871,
     'Comprehensive design framework; Design conflict resolving; Modularization; Configuration': 872,
     'Multi-stakeholder requirements; Product-service system; Design; Life cycle costing; Multi-domain matrix': 874,
     'product-service systems; work systems; collaboration; participation; organisational design': 875,
     'product-service systems; leasing; remanufacturing; prams; durable products; eco-design': 876,
     'conceptual solution decision; trade-off decision; rough number; Shapley value; PSS design': 877,
     'industrial product service system; machining capacity; model; measurement': 878,
     'ontology; product service systems; systems of systems': 879,
     'Business models; Sustainability; Servitization; Industrial ecosystems; Product-service system; Circular economy; Ecosystem; Inter-organizational relationships; Orchestration': 880,
     'Industrial product-service system; Tool condition monitoring; Cutting tool services; Product-oriented mode; Use-oriented mode': 881,
     'Servitization; Industrial product service systems (IPSS); Resource planning; Collaboration; Environmental impact': 882,
     'Cloud Manufacturing; Product-Service System; Industry 4; 0; distributed manufacturing; resource sharing; fourth industrial revolution': 883,
     'product-service systems; physical asset management; service contracts; channel coordination': 884,
     'product-service system; servitisation; circular economy; sustainability; bibliometric; systematic review': 885,
     'Value Driven Design; Product-Service Systems; Preliminary design; Systems Engineering; Engineering design; Product development; Servitization': 886,
     'product-service system (PSS); screening life-cycle modelling (SLCM); circular economy (CE); supply chain management; aftermarket services; PSS functional matrix; life-cycle engineering; customer demand; customization': 887,
     'User-Generated Contents; quality management; Product-Service Systems; topic modeling; Structural Topic Model; quality determinants; Quality 4; 0': 888,
     'Product-service system; Modular design; Service solution layer; Utility; Portfolio planning': 889,
     'Smart product-service systems; Configuration system; Hypergraph; Context-aware; User-centric design': 890,
     'Circular business model; Circular economy; Consumer behavior; Digitalization; Shared mobility; Product-service system (PSS)': 891,
     'consumer acceptance; consumer perceived value; use-oriented product-service systems; consumer goods; Sweden': 892,
     'User-centric design; Smart product-service system; Medication management; Multimodal user analysis; Providers integration network; BCE model': 893,
     'Requirement elicitation; Product-service systems; Knowledge management; Data-driven design; Value co-creation': 894,
     'Technology roadmap; design structure matrix; product-service integrated system; planning; relationship': 895,
     'VIKOR approach; variable precision rough set; supplier selection; product service system; vague set theory': 896,
     'Product-service system; Sharing; Production machine; Lifecycle; Big data; Fault diagnosis': 897,
     'Smart product-service system; Service logic; Conceptual factors; Hybrid approach; Sensitivity analysis': 898,
     'Sustainable consumption; circular business models; product-service system; sustainability; environmental awareness': 899,
     'Product-Service System; Personalization; Recommendation; Search cost; DEMATEL': 900,
     'product-service systems; SysML; systems design; requirements engineering; business modeling': 901,
     'Servitisation; Decision-making; Product-service systems; PSS business model; TraPSS (transition along the PSS continuum) methodology': 902,
     'collaboration; co-design; concurrency; product-service systems': 903,
     'Mass customization; Personalization; Quality function deployment; Software renting; Product service system': 904,
     'Supply chain management; Product service system (PSS); Supply chain contract; Information asymmetry': 905,
     'supply chain management; product service systems; logistics; productisation; 3PL': 906,
     'design for environmental sustainability; strategic design; system innovation; life cycle design; product-service system': 907,
     'Smart product-service systems; Sustainability assessment; Prospect theory; Behavioral decision making; Rough set theory': 908,
     'Service; Decision making; Business model': 909,
     'Servitization; Product-service systems; Internet of Things; Machine learning; Metrics': 910,
     'leasing; reuse; electrical and electronic equipment; Product Service System; material use; waste prevention': 911,
     'LivingLab; Sustainable product service systems; Experiments; Open innovation; Sustainable consumption and production; Resource efficiency and protection': 912,
     'Data-driven services; Customer value; Product-service systems; Servitization; Smart connected products': 913,
     'product-service systems; requirement elicitation; information modelling; graph embedding; context-awareness; ontology': 914,
     'product service system(PSS); servitization; topicmodeling; Latent Dirichlet Allocation (LDA)': 915,
     'Smart product-service system; Co-implementation; Process-chain-network; Extended-FMECA; Rough-entropy-ELECTRE TRI': 916,
     'Product-service system; Customer satisfaction; Balanced scorecard; DEMATEL; ANP': 917,
     'After-sales service; Egypt; Emerging markets; Field service; Internationalization; Product-service systems': 918,
     'Product-service systems; Edge-cloud orchestration; Cyber-physical systems; Industrial Internet of Things; Data-driven value co-creation': 919,
     'product service system; concept evaluation; Information Axiom; fuzzy random variables': 920,
     'Smart product service system; Design alternatives evaluation; Smart capability; Intrapersonal and interpersonal uncertainty; Rough-fuzzy data envelopment analysis': 921,
     'Waste prevention; Sustainable urban environments; Product service systems': 922,
     'Customer preference; product service system (PSS); public warehouse; service-oriented manufacturing (SOM); service satisfaction evaluation (SSE)': 923,
     'Product-service system (PSS); Conceptual design; Quality function deployment (QFD); Analytic network process (ANP); Fuzzy set theory; Data envelopment analysis (DEA)': 924,
     'Use-oriented; Product-service systems; Configuration; Optimization; Sustainability': 925,
     'Requirements engineering; Product service system; Hybrid product; State of the art; Product engineering; Software engineering; Service engineering': 926,
     'product-service system; innovation; tools; digitalisation; servitisation': 927,
     'Product lifecycle management; Global production networks; Reference ontologies; Interoperability; Product service systems': 928,
     'access model; bicycle sharing; clothing sharing; conjoint analysis; product-service system (PSS); sustainable business model; sustainable consumption; temporality': 929,
     'Product-service system (PSS); New PSS concept; Chance discovery; KeyGraph; Text mining; Web news': 930,
     'Product-Service Systems; Cost engineering; Concept design; Product development; Life cycle cost; Aerospace': 931,
     'product-service system; sustainability; design-centric complexity; TRIZ': 932,
     'Uncertainty; Competitive bidding; Decision making; Product-Service Systems; Service contracts; Information': 933,
     'product-service systems; office furniture; product lifetime extension; remanufacturing; PSS scenario': 934,
     'Product-service system; Intermediary PSS venture; Solar service company; Solar photovoltaic industry; Solar third-party ownership': 935,
     'design method; product-service system; personalization': 936,
     'Outcome-based contracts; Servitization; Product-service system; Remote monitoring technology; Power-by-the-hour': 937,
     'Servitization; Sustainability; Operations strategy; Product-service system; Survey': 938,
     'Servitization; Product-service system; PSS; Service offerings; Profitability; Financial performance': 939,
     'product-service systems; information systems; business process reengineering; value analysis; structural equation modelling, responsiveness': 940,
     'Servitization; Product service systems; industrial operator model; fleet management': 941,
     'Product-service system; Perceived value; Consumer knowledge; Service experience': 942,
     'product service system; configuration optimization; multilayer network; service activities selection; resource allocation': 943,
     'product-service system; modular master structure; modular design; conjoint analysis': 944,
     'product-service systems (PSS); corporate environmental management (CEM); fashion industry; environmental sustainability': 945,
     'Design theory; Smart product-service systems; Information theory; Design entropy; Value co-creation; User experience': 946,
     'Requirements engineering; Requirements data model; Artifact model; Product service systems; PSS; Hybrid products; Requirements concretization': 947,
     'Business process redesign; Internet of Things (IoT); Product-service system (PSS); Machinery industry': 948,
     'Clothing; Servitization; Textile/clothing supply chains; PSS; Extended responsibility; Product-service system': 949,
     'Product-service systems (PSS); Agency theory; Trust; Adverse behaviour; Agency mechanisms; Servitization': 950,
     'product-service system; cleaner production; sustainable development; cutting tool; machining': 951,
     'Revalorisation; Product-service systems; Consumer behaviour; Circular economy; Packaging; Obsolescence': 952,
     'Circular business models; Circular economy; Decoupling; Sustainable business models; Institutional theory; Product-service-systems': 953,
     'life cycle engineering; process modularization; integration of product and service design processes; investment goods industry': 954,
     'Multimodal transport intercity transport; personalized service design mobility as a; service (MaaS) smart product service system&nbsp; (SPSS)': 955,
     'Servitization; Risk management; Manufacturing industries; Product-service systems (PSS); PSS delivery': 956,
     'product-service systems; business model; archetypes; method; tool': 957,
     'product service system (PSS); concept configuration; support vector machine (SVM); principal component analysis (PCA); quantum particle swarm optimization (QPSO)': 958,
     'social value; innovation; sustainability; customer context; value creation; product-service system': 959,
     'services; Product Service Systems; technology transfer; receptivity': 960,
     'Physical Internet; logistics; smart box; product-service system; cloud computing': 961,
     "Product Service System (PSS); Sustainable business model; Government 'demand pull' policy; Energy Service Company (ESCo); Innovation system": 962,
     'Product-service system; Order scheduling; Time window; Metaheuristics': 963,
     'Internet of Things; IoT; Servitization; Manufacturing; Value; Creation': 964,
     'Smart product service system; User activity-oriented smart service requirement; Requirement identification; Requirement evaluation; Rough-fuzzy best-worst method': 966,
     'product-service system; product design; service design; computer-aided design; service blueprint': 967,
     'strategic alignment; service transition; product-service system; strategy; strategic objective; strategy map; business model': 968,
     'Manufacturing; Lean rules; Product-service system (PSS); Key performance indicators (KPIs)': 969,
     'clothing product-service systems; sustainable clothing consumption; sustainable fashion; Finland; United States': 970,
     'Design for Sustainability; Product-Service System; Radical Innovations; Socio-technical Experiment; Strategic Niche Management; Transition Management': 971,
     'microalgae; future superfoods; practice-based design research; product-service systems; open-source': 972,
     'Automated surface inspection; Smart product-service system; Convolutional neural networks; Cloud-edge computing': 973,
     'product-service model; dynamic behaviour; PSS modelling; PSS simulation': 974,
     'Customization; Product/service system; PSS; Integrated solution; Design science; Platform; Literature review; Modularity': 975,
     'Sustainability; Product-service systems; Supply network risks; Multi-criteria decision-making': 976,
     'circular economy; product-service systems; public acceptance; alternative ownership models; responsibility; ownership': 977,
     'remanufacture; product service system; sustainability; operations management; environmental issues; reverse logistics': 978,
     'Product-service systems; Quality function deployment; QFD; Sustainability; Design process; Conceptual design': 979,
     'Value uncaptured; Business model innovation; Sustainable business models; Product-service systems': 980,
     'PSS; supply chain contracts; information asymmetry; service-oriented manufacturing mode': 981,
     'travel experience; transportation vehicle; Kansei engineering; cognitive; emotional': 982,
     'building information modeling (BIM); product service system (PSS); maintenance management; facility management; building equipment; industry 4; 0': 983,
     'Virtual reality; User-centric design; Smart product-service systems; Value-added services; User experience': 984,
     'Kano model; product service system; result-oriented; servitization; use-oriented': 985,
     'servitisation; office printing industry; digitisation; paperless office; product service system; PSS; rebound effect': 987,
     'Smart product-service systems; Engineering design; Design methodologies; Review': 989,
     'knowledge graph; concept-knowledge model; smart product-service system; knowledge evolution; conceptual design; creativity and concept generation': 990,
     'product-service system; life cycle sustainability assessment; product-service flow': 991,
     'IoT (Internet of things); ROS (route optimization system); SPSS (smart product-service system); City logistics': 992,
     'Consumer Experience; Product Design; Product-Service System; Service Design; Value Creation': 993,
     'Product development; Long-life cycle; Sustainability; Product Service Systems; Ambidexterity; Conceptual framework': 994,
     'Circular economy; Jobs-to-be-done theory; Sustainable value proposition; Outcome-driven innovation; Sustainable business models; Product-service systems': 995,
     'OR in service industries; Production service system; Replacement policies; Inventory control': 996,
     'Cyber-physical systems (CPSs); machine health monitoring (MHM); ontology-based framework; product-service system (PSS); smart sensing system': 997,
     'product-service system; service oriented architecture; service identification; service specification; recycling': 998,
     'Product-service systems engineering; Service support systems; Agile software engineering; Information needs; Knowledge needs': 999,
     'circular economy; product service systems; smart services; systems engineering; model-based systems engineering; product development; sustainability; engineering methods; service economy': 1000,
     'product-services; product-service systems; PSS; sustainability; eco-efficient; Factor 4; business development; sharing; renting; pooling; result-oriented services; product-oriented services; system innovation': 1001,
     'Product-service systems (PSS); Servitization; Risk management; Service delivery system design; Sharing economy; Trust': 1002,
     'analytical target cascading; product service system; manufacturing alliance; collaborative production; service oriented architecture (SOA)': 1003,
     'System architecture; Computer aided design; Service': 1005,
     'smart product-service systems; interactivity; ubiquity; network externality; substitutability; technology adoption': 1006,
     'Capacity building; systems design approach; sustainable product-service system; distributed renewable energy; complex societal problem': 1007,
     'chemical management services; product service system; total cost accounting; chemical use reduction; supply chain management; chemical strategies partnership': 1008,
     'human factors/ergonomics discipline; human factors/ergonomics profession; future of ergonomics; work systems; product/service systems; performance': 1009,
     'Product service system; Life cycle assessment; Life cycle costing; High energy-consuming equipment; Environmental burden': 1010,
     'Product-service system (PSS); Key performance indicators (KPIs); Lean rules; Context sensitivity; Sentiment analysis; Manufacturing': 1011,
     "Product-service system (PSS); Importance performance analysis (IPA); Kano's model; Decision making trial and evaluation laboratory (DEMATEL); Vague sets": 1012,
     'Product-service systems; Digital twin; Systematic review; Systematic mapping': 1013,
     'Product-Service Systems; Digitization; Customer; Maritime industry': 1014,
     'product-service systems; behavioural changes; system innovation; automobile industry; environmental benefits': 1015,
     'Cyber-physical system (CPS); Product-service system (PSS); Production monitoring; Cloud-edge orchestration; Collaborative manufacturing': 1016,
     'product service system; circular economy; collaboration; concrete industry; case study': 1018,
     'baby clothing; leasing system; product service systems in fashion and dress; sustainability; practices of use': 1019,
     'Impact-measurement; Quantitative-empirical methodology; Generic model; Adaptation-process over time; Carsharing; Shared mobility': 1020,
     'action-specific warning; multimodality; cross-generational interface design; industrial product-service systems': 1021,
     'product service system (PSS); business model (BM); multiple criteria decision making (MCDM); technique for order preference by similarity to ideal solution (TOPSIS); analytic hierarchy process (AHP)': 1022,
     'Product service system (PSS); Service-oriented manufacturing; Manufacturing-oriented services; Mold and die; Collaboration platform; Service-oriented architecture (SOA)': 1023,
     'consumer benefit; consumer innovativeness; product&#8211; service system; sustainable development; sustainable production and consumption': 1024,
     'service implementation; service perspective; servitization; technology-dominant service development; product-service systems': 1025,
     'product-service systems (PSS); human resource management (HRM); fashion industry; sustainable business models; sustainable retail': 1026,
     'Product-service system (PSS); Conceptual design; Parameter translating; Association rule mining; Incremental updating': 1027,
     'Integrated product-service offerings; servitization; sustainable PSS design; lifecycle; resource decoupling; sustainability': 1028,
     'Sustainable Product-Service Systems; Design for sustainability; System innovation': 1029,
     'Business model design; Strategic sustainable development; Sustainable business model; Sustainable product-service systems': 1030,
     'water supply; water quality; energy consumption; material consumption; product service system; PSS': 1031,
     'Energy Product&#8211; Service Systems; renewable energy; energy transition; retail market': 1032,
     'End of life management; game theory; stackelberg; sustainability; warranty': 1033,
     'PSS; critical success factors; empirical study; innovation management; development phases; product-oriented; organisational capabilities; FFE': 1034,
     'service activity design; context-based activity modeling; design thinking; visual reasoning model; design for circular economy; reuse product-service systems': 1035,
     'Competitive advantage; Differentiation; Servitization; Product-service systems; Services management; Value-in-use': 1036,
     'PSS engineering; Conceptual modelling; Modelling language; Domain-specific modelling; Model-based system engineering': 1037,
     'Sustainable products and services; sustainability; eco-design; Environmental Management; life cycle thinking; Product service systems': 1038,
     "unintended environmental consequences; incremental innovation; consumption rebound effect; causal loop diagram; systems' structure and behavior": 1039,
     'public warehouse; wPSS; service capability; maturity; analytical target cascading': 1040,
     'remote maintenance; monitoring; control and automation': 1041,
     ...}




```python
# 첫 번째 키워드의 id 추출
idx = id2keyword['Digitalization; Company performance; Listed company; Benefit; Obstacle'] # 0번 인덱스
sim_scores = [(i,c) for i, c in enumerate(cosine_matrix[idx]) if i != idx]
sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True) # 유사도가 높은 순서대로 정렬
sim_scores[:10]
```




    [(281, 0.2775046260698838),
     (891, 0.205907997753756),
     (1285, 0.20486331911792321),
     (104, 0.19839686310545473),
     (30, 0.16615486446364855),
     (224, 0.12101876011041224),
     (1096, 0.11458985487373918),
     (1537, 0.11091775602581064),
     (1348, 0.10661128588676126),
     (1141, 0.09491653406418596)]




```python
# 인덱스를 keywords로 변환
sim_scores = [(keyword2id[i], score) for i, score in sim_scores[0:10]]
sim_scores
```




    [('Digitalisation; service ecosystems; resource integration; servitization; strong and weak ties; operant resources',
      0.2775046260698838),
     ('Circular business model; Circular economy; Consumer behavior; Digitalization; Shared mobility; Product-service system (PSS)',
      0.205907997753756),
     ('digital product-service innovation; product-service system; PSS; sustainability; supply-chain integration; triple bottom line; case study; basic services; advanced services; digital services',
      0.20486331911792321),
     ('prescriptive maintenance; sustainable business models innovation; Industry 4.0; technology systems; distributed ledger technologies; digital twin',
      0.19839686310545473),
     ('Market entry; strategy; servitisation; private leasing; carsharing; sharing economy; disruptive innovation; ecosystem',
      0.16615486446364855),
     ('Servitization; SSPs; SSCs; machining and equipment manufacturers; digital transformation; Industrial services; IIoT; Artificial intelligence',
      0.12101876011041224),
     ('Integrated products and services; Sustainable development; Disclosure; Environmental indicators; Performance; Energy consumption',
      0.11458985487373918),
     ('digital transformation; sustainable development goals; digital technologies; agenda 2030',
      0.11091775602581064),
     ('circular economy; product-service systems (PSS); leasing and renting; sustainable development in Romania',
      0.10661128588676126),
     ('brand communities; culture; ethnography; ownership; product service system; sustainable consumption',
      0.09491653406418596)]




```python

```


```python

```
