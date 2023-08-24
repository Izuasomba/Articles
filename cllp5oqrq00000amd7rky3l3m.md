---
title: "Guardians of Transactions"
seoDescription: "A hands on guide to building your very own credit card fraud detection model"
datePublished: Thu Aug 24 2023 12:44:41 GMT+0000 (Coordinated Universal Time)
cuid: cllp5oqrq00000amd7rky3l3m
slug: credit-card-fraud-detection
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692872389081/40ea806f-2946-47f2-a284-b6e4a059e745.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692881010222/3e7f01c8-dcba-4a99-bcf7-281fa3c03c20.png
tags: machine-learning, finance, cybersecurity-1

---

## Overview

Not too long ago, early in the morning, I got an email that caught me off guard. It said the money had left my bank account – a debit alert. Do you know how heartbreaking debit alerts are? Now imagine one you had no idea about. I was so confused at the moment. I immediately called my bank for answers. But no one picked up the phone. Frustrated, I had to go to the bank branch. I stood in line for what felt like forever, finally getting to explain the situation to a customer service representative. She checked my account and asked if I had any subscriptions or if I made an online order. Confused, I said I didn't and asked why she was asking those things. That's when she dropped what seemed like a bomb – my card got charged multiple times for online purchases. It seemed scammers had gotten their hands on my card details. She advised me to block the card, but it was too late. There wasn't much in my account. Everything was gone. So, why am I sharing this story? Credit card fraud is a huge problem. Maybe you've had a similar experience or know someone who has. I want to make you realize how serious it is. People are losing money left and right. To fight this, measures have been taken to catch and detect these frauds. In this article, I'll show you how I built a simple credit card fraud detector using machine learning. Stick with me.

## Importing dependencies and loading your dataset

Let's start this journey together. Like any machine learning project, we'll begin by getting the tools we need. Our main tool for this adventure is the Jupyter Notebook. If you're using Linux, open your terminal and type **"jupyter lab"** or **"jupyter notebook"**. If you don't have it, you can install it using a simple pip command.

Once that's set up, it's time to bring in the team. Just import the stuff you need like this:

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score
import seaborn as sns
import joblib
```

If you don't have any of these modules installed, you can install them using this command in your notebook

```python
!pip install <module_name>
```

Now that we're all set up, let's get the dataset into our notebook. I got the dataset from Kaggle, and you can find it here: [Kaggle Credit Card Fraud Dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud).

To add the data to our notebook, use this code

```python
transaction_data = pd.read_csv("/your_directory/creditcard.csv")
```

## Data preprocessing

Back in the late 1980s, there was a plane, an Airbus A320, attempting to land. However, things didn't go as planned. The reason? Some of the data from the plane's sensors got mixed up before reaching its computer system. You know those sensors that measure how high or low the plane's nose is? Well, they collected information in a way that didn't match what the computer was expecting. This mismatch caused the computer to think the plane's nose was in a different position than it was. Because of this confusion, the plane's controls got all mixed up. The pilots started getting different signals and warnings, and they couldn't figure out what was going on. Despite their best efforts, the confusion caused the plane to crash. Now, why am I telling you all this? Just to drive home the point of how important it is to get your data ready before using it. If data isn't correct or clean, it can cause real trouble when you're training a model. That's why we're really careful about getting the data in good shape before we start building our models.

In this section, we are going to carry out proper preprocessing of our data before inputting it into our model.

```python
transaction_data.info()
```

The code above gives us a clear picture of our dataset. It shows what type of data is in each column, how many entries are complete, and the names of the columns. This helps us understand what's in the dataset and how it's organized.

When you run this code in your notebook, you'll notice something important – there aren't any missing values. This simplifies our work. With that in mind, let's move on to something else: Check out the `Class` column.

Now, the `class` column is where each transaction is categorized. A `0` means it's a regular transaction, while a `1` means it's a fraudulent one.

```python
transaction_data["Class"].value_counts()
```

Back in the early 2000s, a hospital decided to use machine learning to help doctors spot a rare medical condition. They trained a model using real patient records, which included both healthy people and those dealing with the condition. But here's the thing – there were way more records of healthy folks compared to cases of the medical condition in the dataset.

During the model's test runs, it seemed pretty accurate, even doing better than human doctors at diagnosing the condition. Everyone was excited about how this model could make diagnosing rare illnesses better.

But when they put the model to work in real-life situations, they hit a bump. The model kept saying patients were healthy, even when their symptoms pointed to the medical condition. After digging into it, they found something. The impressive accuracy was sort of fooling them. The model had figured out that saying "healthy" most of the time was an easy way to get a high score, thanks to the uneven data. It found it much simpler to look good by mostly ignoring the smaller group.

Now, here's the twist – this caused a lot of trouble. Patients who had the condition often got the wrong label and didn't get the right treatment in time. Waiting for proper care just made things worse for them. The moral here? Training a model with uneven data is a big no-no.

So, what's this "uneven data" thing? It's when one group is way bigger than the others. To put it simply, when you run that code in your notebook, you'll see `0` appears a huge 284,315 times, while `1` shows up way fewer times – just 492 instances. I've whipped up a chart using `Seaborn.` For better understanding

Run in your notebook

```python
sns.countplot(x = "Class", data = transaction_data)
```

This was the output

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692834883282/4ef4c449-6bc2-4c20-969b-ac793e51669d.png align="center")

That's a high margin. Now how do we go about this imbalance in our data? By using a statistical technique called **"Under-sampling".**

Now, what is this under-sampling? Undersampling is a technique used in machine learning to address class imbalance in datasets. Class imbalance occurs when one class in a binary classification problem has significantly fewer examples than the other class. This imbalance can lead to skewed model performance, where the model might perform well on the majority class but poorly on the minority class.

In undersampling, you intentionally reduce the number of instances in the majority class to create a more balanced distribution between the classes. That's exactly what we will be doing on this dataset.

But before that, We'd separate both classes into different variables

```python
legitimate_transactions = transaction_data[transaction_data.Class==0]
fraudulent_transactions = transaction_data[transaction_data.Class==1]
```

Now we'd go ahead and take a sample of the legitimate transactions:

```python
sample_legitimate = legitimate_transactions.sample(n=492)
```

Concatenate both data frames

```python
new_data = pd.concat([sample_legitimate,fraudulent_transactions], axis=0)
```

Here's what's going on:

**legitimate\_transactions:** We're sifting through the transaction data to grab those rows where the "Class" is 0 – those are legit transactions, the good stuff.

**fraudulent\_transactions:** A similar manoeuvre, but now we're zeroing in on rows where the "Class" is 1. These are the fraudulent transactions, the sneaky ones.

**sample\_legitimate:** This is the fun part. We're dipping into the pile of legitimate transactions and pulling out a random batch, like grabbing candies from a jar. We're talking about 492 of them, to be precise. This move is all about balance - making sure we have equal parts of legitimate and fraudulent transactions.

**new\_data:** Bringing things together! We're taking that random sample of legitimate transactions from before and mixing it up with the fraudulent ones. The result? A fresh batch of data called "new\_data" where we've got the same number of legit and tricky transactions (492 of each).

Now, I don't want this article to get too long, so I will quickly go over the remaining steps, the full code is available on my [GitHub](https://github.com/Izuasomba/credit-card-fraud-detection).

## Splitting the dataset into features and targets

Imagine you're an architect designing a new house. You have a detailed blueprint that outlines everything, from the house's layout to its special features. The blueprint is your dataset, a bunch of information describing different parts of your project.

To build the house, you split the tasks among different teams. Some teams will handle the foundation, another the walls, and another will focus on the interior design. Each team has its job, and they all need to follow the blueprint.

In machine learning, the blueprint is the dataset, and the teams are the machine learning algorithms. The algorithms learn from the data to do specific tasks, such as predicting whether a customer will churn or classifying an image as cat or dog.

To train a machine learning model, you need to split the dataset into two parts: features and targets.

* **Features** are the traits or qualities that go into your model. They describe each piece of data and help the algorithm learn patterns and connections. For example, in the house example, the features could be the number of rooms, the sizes of the spaces, and the location of the doors and windows.
    
* **Targets** are the outcomes you're after. In data, the target (also called the label) is what you want your model to predict or sort into. For example, in the house example, the target could be the type of house (e.g., bungalow or two-storey).
    

Just like it would be chaotic if the architectural teams didn't know their roles or have a plan to follow, your machine learning model needs a clear divide between features (the input) and targets (the output). This split lets the model grasp how features and targets relate. It helps the model make predictions or decisions when it sees new data.

So, when working with machine learning, it's important to remember the analogy of the architect and the building project. The dataset is the blueprint, the algorithms are the teams, and the features and targets are the roles and responsibilities. By understanding this analogy, you can better understand how to train and use machine learning models. That about sums it up

In the code block below, we'd be splitting the dataset into **features** and **targets** where `X` represents features and `Y` represents targets

```python
X= new_data.drop(columns="Class", axis=1)
Y= new_data["Class"]
```

## Training the model

Now that we've got our data separated into features and targets, it's time to prep for training our model. But before jumping in, there's a step we need to handle – splitting our data into training and test sets. And for this, we've got the `train_test_split` function from the `sklearn` toolkit.

Now, let's chat a bit about this test-train split. Usually, the data gets divided into two parts – around 80% for training and 20% for testing. This setup has a purpose – it ensures the model has a good chunk of data to learn from during training. And the remaining bit? Well, that's to check how well the model learned when we put it to the test. Now let's get right ahead with that

```python
X_train, X_test, y_train, y_test = train_test_split(X, Y, test_size=0.2, stratify=Y, random_state=4)
```

Here's what's happening step by step:

1. `train_test_split`: This is like a tool we're using from a toolbox called `sklearn`. It helps us split our data into two parts for training and testing our model.
    
2. `X` and `Y`: These are the features and targets.
    
3. `test_size=0.2`: Imagine we have a bag of stuff. We're deciding to take out 20% of that stuff and set it aside for testing our model later. The rest, which is 80%, we'll use to train the model.
    
4. `stratify=Y`: We're being careful here. If some types of stuff are rare, we want to make sure we have enough of those types in both the training and testing groups. This keeps things fair.
    
5. `random_state=4`: This is like a secret code that makes sure every time we run this code, we get the same split. It's good for comparing results later.
    

Now, what do `X_train`, `X_test`, `y_train`, and `y_test` mean?

* `X_train`: This holds a portion of our stuff (features) that we'll use to teach the model.
    
* `X_test`: This is another portion of our stuff that we'll keep hidden. Later, we'll ask the model to predict the labels for this hidden stuff and see how well it does.
    
* `y_train`: These are the labels that go with the stuff in `X_train`. They help the model learn.
    
* `y_test`: These are the labels for the stuff in `X_test`. We'll use these to check if the model's predictions are accurate.
    

So, this code does the job of splitting our data into groups for training and testing our model, making sure everything's fair and consistent.

Now, let's move on to the next step: training the model.

We're going with the logistic regression model. Wondering why? Allow me to break it down:

**Logistic Regression for Binary Classification:** Logistic regression is a perfect match for situations where you're dealing with just two outcomes – like, in our case, classifying credit card transactions as either legitimate or fraudulent, which translates to 0 or 1.

**Interpretable Insights:** One cool thing about logistic regression is that it's not a mystery box. It's like a window into how things work. It lets us see which features matter and how they influence the final prediction. In fraud detection, knowing what features contribute to the likelihood of fraud is pure gold. It helps us make smarter decisions and fine-tune the whole detection process.

**Handling Imbalanced Data:** Logistic regression plays well with imbalanced data, especially when you team it up with techniques like under-sampling and oversampling. Speaking of oversampling, it's a method where you artificially increase the number of instances in the minority class to balance things out. You might want to do some digging to get the full scope.

So, there you have it. These are a few reasons we're picking the logistic regression model. It's practical, transparent, and just what we need to tackle this fraud detection puzzle. Let's roll

```python
model = LogisticRegression()
model.fit(X_train,y_train)
```

Here's what's going on here:

1. **Creating a Model:** We're creating a container or a "blank canvas" called `model`. Inside this container, we plan to build our logistic regression model.
    
2. **LogisticRegression():** We're using the logistic regression tool from `sklearn`. We're putting it inside our model container. Think of this tool as a framework for our model to learn from the data.
    
3. [**model.fit**](http://model.fit)**(X\_train, y\_train):** This is where the real learning takes place. We're telling our model to learn from the training data we prepared earlier. The `X_train` holds the features (stuff we're using to teach), and `y_train` holds the labels (answers) that match those features. So, the model will study this data, understand patterns, and learn how to predict whether a transaction is legit or fraudulent.
    

In a nutshell, this code block sets up our logistic regression model and trains it with the training data. It's like building the framework and then filling it with knowledge so that the model can make accurate predictions later.

## Model Evaluation

Imagine you're gearing up for a speech competition. You spend hours rehearsing in front of a mirror, perfecting every word and gesture. But when you step onto the stage in front of an audience, your nerves get the best of you. You forget parts of your speech, and the delivery is far from flawless. You were so fixated on getting the words right that you couldn't adapt to the real situation – distractions and all.

Alternatively, you decide to wing it and give a speech without prior practice. You stumble over words, your thoughts are all over the place, and your message falls flat. Without putting in the effort to refine your speech, your performance suffers.

In the first case, you overfitted your preparation to a controlled environment (mirror practice). You nailed the practice, but you couldn't handle the real-world scenario.

In the second case, your under-preparation led to a messy and ineffective performance.

So, what's the essence here? When training a machine learning model, it can perform well on training data but fail miserably on real-world or test data – that's overfitting. Conversely, an underfit model lacks the training needed to perform well in familiar and unfamiliar situations(train and test data).

Now, how do we uncover these irregularities in our model? That's where model evaluation comes in. It's like checking the model's report card. Model evaluation is all about measuring how well the model performs. We use evaluation metrics to measure accuracy, precision, recall, and other aspects of the model's performance.

For our current model, we're going with the accuracy\_score as our evaluation metric. This metric is like a ruler for measuring how well a classification model (like ours) does its job of labelling transactions as legit or fraudulent.

Let's dive into the model evaluation process and see how well our model is doing!

```python
#Training accuracy
X_train_pred=model.predict(X_train)
training_accuracy = accuracy_score(X_train_pred, y_train)
print("Training accuracy : ", training_accuracy)

# Test accuracy
X_test_pred =model.predict(X_test)
test_accuracy = accuracy_score(X_test_pred, y_test)
print("Test accuracy is : ", test_accuracy)

#Output
Training accuracy :  0.9542566709021602
Test accuracy :  0.934010152284264
```

Now let's take a look at our model's performance. Our training accuracy stands at around 95.4%, and the test accuracy is about 93.4%. A noticeable factor is the narrow gap between them, indicating our model's strong performance. To put it differently, if our training accuracy was 95.4% and the test accuracy was just 60%, a potential case of overfitting might be at play. To shed light on this, imagine a scenario with 1000 transactions – our model would correctly classify 600, leaving 400 misclassified ones. Can you sense the issue now?

So, when training accuracy is high and test accuracy is low, it's often a signal of overfitting. Now, what about high test accuracy and low training accuracy? This scenario doesn't precisely fit the underfitting or overfitting scenario. Let me break down potential situations where this pattern might apply:

* **Data Issue:** The training data might be problematic. If it doesn't represent the test data or contains errors, the model could struggle to perform well.
    
* **Randomness:** In some cases, randomness can come into play. High randomness in the training data or process might lead to unexpected outcomes.
    
* **Data Leakage:** Unintentional data leakage could be in the mix. This involves the test data influencing the training process, yielding high test accuracy.
    

To tackle this, a deep dive into the data, model complexity, and training process is key. Employing techniques like cross-validation and rigorous debugging helps pinpoint the root cause behind unusual accuracy patterns. While this situation might not directly apply to the current model, the knowledge gained can be invaluable for future endeavours.

With this, we conclude our journey through this article. For the complete code, check out my [GitHub](https://github.com/Izuasomba/credit-card-fraud-detection).

## Conclusion

Fighting credit card fraud is like an eternal battle that's been around forever. I mean, it's like the nemesis that never seems to give up! I cooked up this model because I went through some serious trauma when I got hit by it. Like, my card went solo shopping without me, and it wasn't even window shopping!

So, here I am, sharing my tale of woe and triumph. You know, just in case any of you future finance superheroes land in a big-shot financial firm and decide to cook up some anti-fraud system. This model might be like the basic training wheels version, but trust me, it can point you in the right direction when you're crafting your supercharged fraud-busting system. A bit like Batman's utility belt, but for data nerds!

Now, before I wrap up, a heartfelt shoutout to all you legends who stuck around till the very end. Seriously, you're the real MVPs here. If you enjoyed the journey, drop a like (because, hey, even machine learning nerds need some love), and don't be shy – hit up the comments section with your thoughts. And hey, keep your radar tuned for the next articles dropping on this blog. Until then, keep it real, keep it safe, and catch you on the flip side! Ciao for now! 🚀🤖💥