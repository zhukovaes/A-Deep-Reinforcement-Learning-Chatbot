# A Deep Reinforcement Learning Chatbot
Link https://arxiv.org/pdf/1709.02349.pdf

### Problem
Dialogue systems and conversational agents including chatbots, personal assistants and voice control interfaces are becoming ubiquitous in modern society. In 2016, Amazon.com Inc proposed an international university competition with the goal of building a socialbot: a spoken conversational agent capable of conversing coherently and engagingly with humans on popular topics, such as entertainment, fashion, politics, sports, and technology. Building intelligent conversational agents remains a major unsolved problem in artificial intelligence research.

### Solution
The idea is to create an ensemble of 22 response models with dialogue manager that choose the response.

Our system consists of 22 response models. The response models take as input a dialogue and output a response in natural language text. They have been engineered to generate responses on a diverse set of topics using a variety of strategies.

The dialogue manager is responsible for combining the response models together. As input, the dialogue manager expects to be given a dialogue history and confidence values of the automatic speech recognition system. To generate a response, the dialogue manager follows a three-step procedure. First, it uses all response models to generate a set of candidate responses. Second, if there exists a priority response in the set of candidate responses, this response will be returned by the system. Third, if there are no priority responses, the response is selected by the model selection policy.


![](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/dialog%20manager.png)
### Details

After generating the candidate response set by response models, the dialogue manager uses a model selection policy to select the response it returns to the user. The dialogue manager select response acccording to scoring model.


Scoring model architecture:

![alt text](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/Снимок%20экрана%202020-04-23%20в%2022.46.27.png)


5 approaches to train scoring model were used 
1. Supervised AMT: Learning with Crowdsourced Labels

We use Amazon Mechanical Turk (AMT) to collect data for training the scoring model. We show human evaluators a dialogue along with 4 candidate responses, and ask them to score how appropriate each candidate response is on a 1-5 Likert-type scale. 

We optimize the scoring model w.r.t. log-likelihood to predict the 4th layer, which represents the AMT label classes. We fix the parameters of the last layer to the vector [1.0, 2.0, 3.0, 4.0, 5.0]. as we do not have labels to train it.

2. Supervised Learned Reward: Learning with a Learned Reward Function

We used real scored dialogs from Alexa's users. Let ![formula](https://render.githubusercontent.com/render/math?math=h_t) be a dialogue history and let ![formula](https://render.githubusercontent.com/render/math?math=a_t) be the corresponding response, given by the system at time t. We learned linear regression model, ![formula](https://render.githubusercontent.com/render/math?math={g_{\phi}(h_t,a_t)\in[1,5]}) by minimizing the squared error between the model’s prediction and the corresponding return (Alexa user score) at the current dialogue turn
 

Then, we fine-tune the Supervised AMT scoring model to predict the reward model outputs by minimizing the squared error.

3. Off-policy REINFORCE

Update scoring model params as 

![formula](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/off-policy%20update.png)

where

![formula](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/off%20policy%20reward.png)
![formula](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/off%20policy%20coef.png)

4. Off-policy REINFORCE with Learned Reward Function

Same to Off-policy REINFORCE except


![formula](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/off%20policy%20reward%20learned.png)


5. Q-learning with the Abstract Discourse Markov Decision Process

### Results


![alt text](https://github.com/zhukovaes/A-Deep-Reinforcement-Learning-Chatbot/blob/master/score.png)

### What things could be improved
One important direction for future research is personalization, i.e. building a model of each user’s personality, opinions and interests. This will allow the system to provide a better user experience by adapting the response models to known attributes of the user. 

To mitigate the issues of speech recognition errors, we plan to evaluate the system with different policies through a text-based evaluation on Amazon Mechanical Turk. This would also help reduce other problems, such as errors due to incorrect turn-taking 
