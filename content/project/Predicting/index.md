---
date: "2021-02-05T00:00:00Z"
title: Predicting student performance using fNIRS data
image:
  caption: We identified the most important channels from the total iterations in training the model.
tags:
- Education
- Machine Learning
- Networks
- E-learning

url_code: "https://github.com/yumizinha/attention"
url_pdf: "https://www.frontiersin.org/articles/10.3389/fnhum.2021.622224/full"
url_slides: ""
url_video: ""
---

Increasing student involvement in classes has always been a challenge for teachers and school managers. In online learning, some interactivity mechanisms like quizzes are increasingly used to engage students during classes and tasks. However, there is a high demand for tools that evaluate the efficiency of these mechanisms. In order to distinguish between high and low levels of engagement in tasks, it is possible to monitor brain activity through functional near-infrared spectroscopy (fNIRS). The main advantages of this technique are portability, low cost, and a comfortable way for students to concentrate and perform their tasks. This setup provides more natural conditions for the experiments if compared to the other acquisition tools. In this study, we investigated levels of task involvement through the identification of correct and wrong answers of typical quizzes used in virtual environments. We collected data from the prefrontal cortex region (PFC) of 18 students while watching a video lecture. This data was modeled with supervised learning algorithms. We used random forests and penalized logistic regression to classify correct answers as a function of oxyhemoglobin and deoxyhemoglobin concentration. These models identify which regions best predict student performance. The random forest and penalized logistic regression (GLMNET with LASSO) obtained, respectively, 0.67 and 0.65 area of the ROC curve. Both models indicate that channels F4-F6 and AF3-AFz are the most relevant for the prediction. The statistical significance of these models was confirmed through cross-validation (leave-one-subject-out) and a permutation test. This methodology can be useful to better understand the teaching and learning processes in a video lecture and also provide improvements in the methodologies used in order to better adapt the presentation content.