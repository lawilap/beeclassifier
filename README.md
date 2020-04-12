# Predict Bee Species from Images

In this project, I've used the Python image library Pillow to load and manipulate image data, then build a model to identify honey bees and bumble bees given an image of these insects.

## Getting Started

<p>Can a machine identify a bee as a honey bee or a bumble bee? These bees have different <a href="https://www.thesca.org/connect/blog/bumblebees-vs-honeybees-what%E2%80%99s-difference-and-why-does-it-matter">behaviors and appearances</a>, but given the variety of backgrounds, positions, and image resolutions, it can be a challenge for machines to tell them apart.</p>
<p>Being able to identify bee species from images is a task that ultimately would allow researchers to more quickly and effectively collect field data. Pollinating bees have critical roles in both ecology and agriculture, and diseases like <a href="http://news.harvard.edu/gazette/story/2015/07/pesticide-found-in-70-percent-of-massachusetts-honey-samples/">colony collapse disorder</a> threaten these species. Identifying different species of bees in the wild means that we can better understand the prevalence and growth of these important insects.</p>

### Display image of each bee type

We will load our labels.csv file into a dataframe called labels, where the index is the image name (e.g. an index of 1036 refers to an image named 1036.jpg) and the genus column tells us the bee type. genus takes the value of either 0.0 (Apis or honey bee) or 1.0 (Bombus or bumble bee).

The function get_image converts an index value from the dataframe into a file path where the image is located, opens the image using the Image object in Pillow, and then returns the image as a numpy array.

### Scale feature matrix + PCA
<p>Our features aren't quite done yet. Many machine learning methods are built to work best with data that has a mean of 0 and unit variance. Luckily, scikit-learn <a href="http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html">provides a simple way</a> to rescale your data to work well using <code>StandardScaler</code>. They've got a more thorough explanation of why that is in the linked docs.</p>
<p>Remember also that we have over 31,000 features for each image and only 500 images total. To use an SVM, our model of choice, we also need to reduce the number of features we have using <a href="http://scikit-learn.org/stable/modules/decomposition.html#pca">principal component analysis</a> (PCA). </p>
<p>PCA is a way of linearly transforming the data such that most of the information in the data is contained within a smaller number of features called components. Below is a visual <a href="https://towardsdatascience.com/pca-using-python-scikit-learn-e653f8989e60">example</a> from an image dataset containing handwritten numbers. The image on the left is the original image with 784 components. We can see that the image on the right (post PCA) captures the shape of the number quite effectively even with only 59 components.</p>
<p><img src="https://miro.medium.com/max/1400/1*xL0FR9PGmu0GmZmyLCUyzw.png" alt="PCA"></p>
<p>In our case, we will keep 500 components. This means our feature matrix will only have 500 columns rather than the original 31,296.</p>

### Train model
<p>We'll use a <a href="http://scikit-learn.org/stable/modules/svm.html">support vector machine</a> (SVM), a type of supervised machine learning model used for regression, classification, and outlier detection." An <a href="https://en.wikipedia.org/wiki/Support_vector_machine">SVM model</a> is a representation of the examples as points in space, mapped so that the examples of the separate categories are divided by a clear gap that is as wide as possible. New examples are then mapped into that same space and predicted to belong to a category based on which side of the gap they fall."</p>

<p>Since we have a classification task -- honey or bumble bee -- we will use the support vector classifier (SVC), a type of SVM. We imported this class at the top of the notebook.</p>

### Model Scoring
<p> To see how well our model did, we'll calculate the accuracy by comparing our predicted labels for the test set with the true labels in the test set. Accuracy is the number of correct predictions divided by the total number of predictions. Scikit-learn's accuracy_score function will do math for us. Sometimes accuracy can be misleading, but since we have an equal number of honey and bumble bees, it is a useful metric for this problem.</p>

### Conclusion

<p> Our model was able to predict bee species with an overall accuracy of 68% and AUC score of 0.74