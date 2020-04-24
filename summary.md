# A Deep Reinforcement Learning Chatbot
Link https://arxiv.org/pdf/1709.02349.pdf

### Problem
Dialogue systems and conversational agents - including chatbots, personal assistants and voice- control interfaces - are becoming ubiquitous in modern society. Building intelligent conversational agents remains a major unsolved problem in artificial intelligence research. In 2016, Amazon.com Inc proposed an international university competition with the goal of building a socialbot: a spoken conversational agent capable of conversing coherently and engagingly with humans on popular topics, such as entertainment, fashion, politics, sports, and technology. 

### Solution

Our system consists of an ensemble of 22 response models. The response models take as input a dialogue and output a response in natural language text. In addition, the response models may also output one or several scalar values, indicating their internal confidence. As will be explained later, the response models have been engineered to generate responses on a diverse set of topics using a variety of strategies.

The dialogue manager is responsible for combining the response models together. As input, the dialogue manager expects to be given a dialogue history and confidence values of the automatic speech recognition system. To generate a response, the dialogue manager follows a three-step procedure. First, it uses all response models to generate a set of candidate responses. Second, if there exists a priority response in the set of candidate responses, this response will be returned by the system. Third, if there are no priority responses, the response is selected by the model selection policy.

### Details

After generating the candidate response set, the dialogue manager uses a model selection policy to select the response it returns to the user. The dialogue manager must select acccording to scoring model


Scoring model architecture:

![alt text](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/Снимок%20экрана%202020-04-23%20в%2022.46.27.png)


5 approaches was used 
1. Supervised AMT: Learning with Crowdsourced Labels
scoring model, which is based on estimating the action-value function using supervised learning on crowdsourced labels

We optimize the scoring model w.r.t. log-likelihood (cross-entropy) to predict the 4th layer, which represents the AMT label classes. Unfortunately, we do not have labels to train the last layer. Therefore, we fix the parameters of the last layer to the vector [1.0, 2.0, 3.0, 4.0, 5.0].

2. Supervised Learned Reward: Learning with a Learned Reward Function
Let ht be a dialogue history and let at be the corresponding response, given by the system at time t. We aim to learn a linear regression model, ![formula](https://render.githubusercontent.com/render/math?math=g_{\phi}), which predicts the corresponding return (Alexa user score) at the current dialogue turn:
 ![formula](https://render.githubusercontent.com/render/math?math={g_{\phi}(h_t,a_t)\in[1,5]})
 
 Let ![formula](https://render.githubusercontent.com/render/math?math=h_{dt},a_{dt},R_d}) be a set of examples, where t denotes the time step and d denotes the dialogue.
 We learn φ by minimizing the squared error between the model’s prediction and the observed return R

Then, we first initialize the model with the parameters of the Supervised AMT scoring model, and then fine-tune it with the reward model outputs to minimize the squared error:


3. Off-policy REINFORCE
4. Off-policy REINFORCE with Learned Reward Function
5. Q-learning with the Abstract Discourse Markov Decision Process

### Results


![alt text](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/score.png)

### What things could be improved
One important direction for future research is personalization, i.e. building a model of each user’s personality, opinions and interests. This will allow the system to provide a better user experience by adapting the response models to known attributes of the user. 

To mitigate the issues of speech recognition errors, we plan to evaluate the system with different policies through a text-based evaluation on Amazon Mechanical Turk. This would also help reduce other problems, such as errors due to incorrect turn-taking 
