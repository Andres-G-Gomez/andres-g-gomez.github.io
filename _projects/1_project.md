---
layout: page
title: Personalized Learning System
description: 
img: assets/img/system_overview.jpg
importance: 1
category: Research
related_publications: 
pdf_link_text: "Read the full paper here"
pdf_file_path: "assets/pdf/ASEE_v4.pdf"
---
**<u>Adaptive Affect-Aware Multimodal Learning Assessment System for Optimal Educational Interventions</u>**

**<u>Abstract</u>**

Researchers recognize the potential of affective, or emotional, features in enhancing learning systems, but many current systems use classifiers with limited emotions, limiting model flexibility [1]. This paper introduces a novel educational assessment tool that leverages 52 localized facial expression features, 39 pose landmarks, and time-series algorithms to discern user-specific affect-performance associations, enabling real-time interventions in e-learning environments. Our Assessment Unit evaluates users' subject-specific proficiency, the Affect Unit predicts real-time performance based on affect features, and a simple Intervention Unit offers targeted recommendations. In future work, we aim to shift from rule-based interventions to advanced methods integrating reinforcement learning, investigate adaptive data fusion, and develop an affect-performance dataset to train and evaluate performance predictors. This system, while still in development, points towards future research directions in engineering education, exploring users’ affect-performance associations to improve educational interventions, thereby offering more tailored and refined educational experiences. 

[{{ page.pdf_link_text }}]({{ page.pdf_file_path | relative_url }}){:target="_blank"}

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/system_overview.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the proposed adaptive multimodal learning system architecture.
</div>


**<u>Methods</u>** 

We introduce a novel learning system that utilizes individualized affect-performance patterns to guide educational interventions, with the goal of enhancing learning outcomes. Our method integrates computer vision and time-series algorithms, focusing on localized facial expressions for improved model adaptability and flexibility. Prior work often classifies emotions into a limited set, constraining the model's expressiveness and flexibility [2, 3, 4, 5]. Furthermore, our system is designed to seamlessly integrate into existing e-learning environments, requiring only a webcam for implementation.

The system's architecture, depicted in Figure 1, represents the user as a distinct blue square. At the start of their initial interaction, users establish a profile by responding to questions about demographics and relevant experience. This user profile/user interface data encompasses session durations and past session data. Users are recorded during their interaction with educational content, which can include lessons or assessments. The Affect unit extracts sequences of affect features from webcam footage, comprising 91 localized facial expressions and pose landmarks per frame. Performance scores are obtained either directly in the assessment or throughout the lessons. In our system, users' proficiency in signing ASL characters is evaluated. They are prompted to sign a character, which is then analyzed by a CNN to assess the accuracy of the sign. In contrast, other systems may employ traditional assessment methods.

During initial interactions, the Affect Unit utilizes time-series algorithms trained on affect sequences, user profiles, and assessment data to forecast user performance. Once the system undergoes sufficient tuning, the performance predictor becomes operational in subsequent sessions. With the system now equipped to provide real-time performance forecasts using affect features, the Intervention Unit can make more informed recommendations to the user. This capability enables real-time, targeted interventions within e-learning environments, aimed at accelerating learning and enhancing overall retention. 

**<u>I. Assessment Unit</u>** 

The Assessment Unit's purpose is to evaluate users' performance in a given task, essential for both fine-tuning the performance forecaster within the Affect Unit and informing the Intervention Unit. While traditional assessment methods can be used, our system employs a CNN to assess users' ASL signing proficiency. When prompted, users capture an image of themselves signing a character. Utilizing MediaPipe’s hand landmarks task [6], an isolated ASL sign is extracted and subsequently analyzed by a CNN to predict the accuracy of the sign. 

The CNN architecture, inspired by a high-performing Kaggle submission [7], is composed of two sequential Conv2D layers with 64 filters, kernel sizes of 4, and a Rectified Linear Unit (ReLU) activation function. The initial layer has a stride of 1, while the subsequent layer has a stride of 2. Following this, two additional convolutional layers are introduced, each with 128 filters, along with a dropout layer. Subsequently, the model includes two more convolutional layers with 256 filters, followed by a final dropout layer. In the dense layers, the first one comprises 512 units with a ReLU activation function, and the ultimate dense layer has units corresponding to the number of classes. This layer utilizes the SoftMax activation function to achieve a probability distribution across the classes.

This CNN was trained on a dataset consisting of 90,000 isolated ASL images spanning 29 classes [8]. The model was trained on 90% of the training data, with the remaining 10% reserved for validation. Various data augmentation techniques were explored, yielding optimal results with random brightness adjustments between 80% to 120% and rescaling by 1./255. Training accuracy peaked at 100%, indicating potential overfitting concerns. Validation accuracy, obtained from 10k bootstrap iterations, is around 86.9% ± 0.4%. The 95% confidence interval for accuracy ranges from 86.9% to 87.0%, indicating high confidence in the estimate. The model achieved a perfect testing accuracy of 100% on a limited held-out test set but struggled to generalize effectively to self-generated data. To improve performance on self-generated data, further experiments or image preprocessing techniques are essential for future ASL recognition applications.


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/fig1.JPG" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The left figure shows the facial and pose landmarks extracted by MediaPipe. The dashed red boundary on the right include the thirteen extraced pose features. The right figure depicts the CNN architecture used within the ASL recognition module.
</div>


**<u>II. Affect Unit</u>** 

The affect unit serves a dual purpose: firstly, to extract localized facial expressions, and secondly, to predict real-time user performance based on these features. As the user interacts with the system, MediaPipe [6] is employed to extract 52 localized facial expressions and 39 pose features. Localized facial expressions are quantified based on their presence in a given frame, while pose attributes encompass (x, y) coordinates, along with a presence value. All features scaled to the [0,1] range, resulting in a total of 91 normalized affect features. To enable real-time prediction of user-specific performance, the system undergoes initial tuning. During initial interactions, sequences of affect features and corresponding scores are collected. If these interactions take the form of lessons, the student's video is segmented into sequences, delineated by in-lesson assessments. Affect sequences and scores are used to tune the performance predictor, and we plan to both train and test Transformers, Recurrent Neural Networks (RNN) and Long Short-Term Memory (LSTM) models. After adequate tuning, the performance predictor becomes operational in subsequent sessions. Consequently, the performance predictor can alert the Intervention Unit of the presence of features associated with subpar performance, facilitating real-time monitoring.

Our next step involves constructing an affect-performance dataset. We have devised a method to gather video footage from users completing a brief online questionnaire, with the intention of using this data to train and evaluate the performance of our RNN and LSTM predictors. Our goal is to identify the algorithm that most efficiently learns affect-performance patterns and to investigate both transfer learning techniques for pretraining the performance predictor and user-specific fine-tuning methods.

**<u>III. Intervention Unit</u>** 

Once the affect unit has undergone initial tuning, it becomes instrumental in intervening during subsequent learning cycles, facilitating real-time, targeted interventions. For instance, if affect features associated with substandard performance are detected (i.e., a low performance is predicted by the affect unit), the system triggers a set of questions. We are contemplating the implementation of an initial query to the user, seeking feedback on the appropriateness of the system trigger—whether they comprehend the content recently reviewed or not. If the user deems the system trigger unwarranted, we will proceed to re-tune the performance predictor in the affect unit, integrating the new data. Conversely, if they acknowledge the trigger's relevance, an evaluation on the content covered during the system-triggered phase follows. Depending on their performance in this evaluation, we can propose an appropriate course of action. Currently, the action recommendation module operates on a rule-based system, relying solely on the score of the assessment questions. If the score for the evaluated sign exceeds 80% accuracy, the module advises to 'continue to the next module'; for scores between 50% and 80%, the recommendation is to 'take a break'; and if it falls below 50%, the module suggests to 'review the module.'

As our work progresses, we will explore more sophisticated methods for optimal intervention selection. This exploration may encompass refining the existing rules or integrating reinforcement learning to formulate a policy that maximizes user performance and adapts to content. Additionally, we are considering the incorporation of a fixed set of domain/lesson specific prompts for feeding into a language model, enabling us to discern which prompts contribute most effectively to accelerated student learning. 

**<u>References</u>**

[1] W. Chango, J. A. Lara, R. Cerezo, and C. Romero, “A review on Data Fusion in multimodal learning analytics and educational data mining,” WIREs Data Mining and Knowledge Discovery, vol. 12, issue. 4, Apr. 2022. doi:10.1002/widm.1458. 

[2] M. Pourmirzaei, G. A. Montazer, E. Mousavi, “Customizing an Affective Tutoring System Based on Facial Expression and Head Pose Estimation”, arXiv [Preprint], Nov. 2021. Available: arXiv:2111.14262. (accessed Feb. 5, 2024). 

[3] M. S. Hussain, O. AlZoubi, R. A. Calvo, and S. K. D’Mello, “Affect detection from multichannel physiology during learning sessions with AutoTutor,” in International Conference on Artificial Intelligence in Education, AIED 2011, Auckland, New Zealand, June 28-July 1, 2011, G. Biswas, S. Bull, J. Kay, A. Mitrovic, Eds. Springer, Berlin, Heidelberg, 2011. pp. 131-138. doi:10.1007/978-3-642-21869-9_19. 

[4] N. Thompson and T. J. McGill, “Genetics with Jean: The design, development and evaluation of an affective tutoring system,” Educational Technology Research Dev, vol. 65, pp. 279–299, Aug. 2016. doi:10.1007/s11423-016-9470-5. 

[5] B. Nye, S. Karumbaiah, S.T. Tokel, M. G. Core, G. Stratou, D. Auerbach, and K. Georgila, “Analyzing learner affect in a scenario-based intelligent tutoring system,” in International Conference on Artificial Intelligence in Education, AIED 2017, Wuhan, China, June 28-July 1, 2017, E. André, R. Baker, X. Hu, M. Rodrigo, B. du Boulay, Eds. Springer, Cham, 2017. pp. 544-547. doi:10.1007/978-3-319-61425-0_60. 

[6] Google LLC. 2023. "MediaPipe." https://developers.google.com/mediapipe (accessed Feb. 5, 2024). 

[7] Akash Nagaraj. 2018. “Asl alphabet,” Kaggle.com. https://www.kaggle.com/datasets/grassknoted/asl-alphabet/data (accessed Nov. 14, 2023). 



{% raw %}

{% endraw %}
