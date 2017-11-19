## Paper Summary of “Controllable Invariance through Adversarial Feature Learning”

### I. Introduction

This paper aims to tackle the problem of learning representations invariant to a specific factor or trait of data. For example, when training an image classifier, we may want to remove the lighting condition to get a more solid representation of an image. To achieve this goal, the learning process is formulated as an adversarial minimax game, where the discriminator tries to detect the specific factor of the data while the encoder learns to conceal it. In other words, given the representation of the data, the game reach an optimal equilibrium when minimizing the certainty of inferring the specific factor and maximizing the certainty of making the predictions. Three tasks are introduced in this paper, including fair and bias-free classification, language-independent generation, and lighting-independent image classification. We show that the proposed framework not only results in an invariant representation of the data, but also achieves better performance.

### II. Adversarial Invariant Feature Learning

Given input <x, s>, where x is the input data and s is a detrimental attribute of x, we want to predict the output y. 

To achieve the goal, we first employ a deterministic encoder E to extract the representation h from x such that the representation h preserves x’s variations that are necessary to predict y while eliminating information from s. The representation h is denoted as E(x,s). In the previous example, x is the input image and s is the lighting condition. We can expect the encoder to generate an image representation h=E(x,s) regardless of the lighting condition of the original picture.

With the input representation h from the encoder E, we then use a predictor M to model the probability distribution qM(y | h) and predict the target y.

To effectively enforce the encoder to eliminate the information from s and result in an invariant representation, we set up a discriminator D, which is trained to predict s based on the encoded representation h.

Now, there are three main components in the adversarial invariant feature learning process, the encoder E, the predictor M, and the discriminator D. While the discriminator D tries to maximize the likelihood qD(s | h), the encoder tries to minimize it, which forms a minimax game. Finally, the game results are calculated by the subtraction of log likelihood of qD(s | h) and qM(y | h) as shown below.

![formula](/image/minimax.png)

### III. Experiments and Results

#### Fair Classification
Use three datasets to predict whether a person has savings of 50,000 dollars, whether a person is of good credit ratings, and whether a person will have good health conditions next year with nuisance variables being age, gender, and age respectively.

To test how much information about s is retained in the learned representation h, we use logistic regression to predict factor s. If the accuracy is similar to the percentage of the majority category(the black line), then the representation h is considered unbiased toward s.

![fair_1](/image/fair_1.png)

While removing information from s is important, we also want to ensure the accuracy of making predictions on y are not harmed. The closer the accuracy is to that of x, the better the prediction is. While we achieve better accuracy than other models in the first two cases, the last one remains same as the majority, since the dataset is highly imbalanced.

![fair_2](/image/fair_2.png)

Finally, we want to measure how fair the models are. For example, when predicting the savings with the nuisance factor being age, as the stereotype suggests: people of advanced age have fewer savings, a biased model would tend to predict insufficient savings given an advanced age while a bias-free model will not. To better understand the fairness of the models, we calculate the accuracy achieved in the overall performance and in biased categories. As the results show, the bias-free models achieve better accuracies on the biased categories while remaining overall high accuracies.

![fair_3](/image/fair_3.png)

#### Multi-lingual Machine Translation

Use French to English and German to English datasets simultaneously, in which s indicates the language of the source sentence.

The results show that both multi-lingual models perform better than the bilingual model, implying that learning invariant representation leads to better generalization by transferring the strength between two languages. In addition, the performance drops without the discriminator, which eliminates the possibility that the better results come from the extra parameters in two encoders. Moreover, when adopting only one encoder, the performance is deteriorated as well, which implies that the encoder needs sufficient space to learn the invariant representation.

![multi-lin](/image/multi-lin.png)

#### Image Classification

The dataset is composed of face images of 38 people in 5 different lighting conditions. The variable s is the lighting condition.

By removing the lighting condition s, the accuracy of classifying y is increased from 85% to 89%. To assure that the lighting information is eliminated in the representation, we try to classify s. The accuracy is as low as 57%. In addition, we visualize the distributions of image representations using the original image and using the invariant representations. We find that the model trained with original images cluster based on the lighting conditions while the model trained with invariant representations clustered based on the image’s identity.

![faces_1](/image/faces_1.png)

![faces_2](/image/faces_2.png)


### IV. Conclusion

To sum up, we proposed a generic framework to learn invariant representations to specific factors in the data by formulating the learning process as a minimax game among an encoder, a discriminator, and a predictor. We applied the method to three different tasks and show that the invariant representation is learned and results in a better generalization and a better performance.

