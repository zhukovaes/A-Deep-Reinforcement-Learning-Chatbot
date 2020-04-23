# A Deep Reinforcement Learning Chatbot
Link https://arxiv.org/pdf/1709.02349.pdf

### Problem
Dialogue systems and conversational agents - including chatbots, personal assistants and voice- control interfaces - are becoming ubiquitous in modern society. Building intelligent conversational agents remains a major unsolved problem in artificial intelligence research. In 2016, Amazon.com Inc proposed an international university competition with the goal of building a socialbot: a spoken conversational agent capable of conversing coherently and engagingly with humans on popular topics, such as entertainment, fashion, politics, sports, and technology. 

### Solution

Our system consists of an ensemble of 22 response models. The response models take as input a dialogue and output a response in natural language text. In addition, the response models may also output one or several scalar values, indicating their internal confidence. As will be explained later, the response models have been engineered to generate responses on a diverse set of topics using a variety of strategies.

The dialogue manager is responsible for combining the response models together. As input, the dialogue manager expects to be given a dialogue history and confidence values of the automatic speech recognition system. To generate a response, the dialogue manager follows a three-step procedure. First, it uses all response models to generate a set of candidate responses. Second, if there exists a priority response in the set of candidate responses, this response will be returned by the system. Third, if there are no priority responses, the response is selected by the model selection policy.

### Details

![alt text](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/Снимок%20экрана%202020-04-23%20в%2022.46.27.png)
