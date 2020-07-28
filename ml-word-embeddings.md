## What does the word "embedding" mean in the context of Machine Learning?

Broadly speaking, machine learning algorithms are “happiest” when presented training data in a vector space. The reasons are not surprising: in a vector space (including the infinite-dimensional Hilbert space generalization), one can compute angles between vectors, which allows doing “projections”, one of the fundamental operations underlying most machine learning algorithms.

But, the major challenge in machine learning is that data come from all over: text documents, images, graphs, sensor streams, and so on. Most of these entities do not “live” in a vector space. For example, a graph is not a vector. You cannot “multiply” a graph by a scalar, or add two graphs.

Similarly, words in English do not “live” in a vector space. I can’t multiply 3.5 times “happiness” to create more “happiness” (sometimes I wish I could!). Similarly, I cannot add “more” and “money” to increase my bank account.

So, whenever you see the word “embedding” in machine learning, what that means is that the authors are exploring some technique to take non-vectorized data and “embed” it into a vector space.

Let’s take a couple of examples to get the hang of the concept. For example, let’s say I want to “embed” a graph in a vector space. How would I go about doing that? A simple way to think about this concept is to imagine drawing the graph on paper. That exercise forces you to take every vertex and plonk it down somewhere on the paper. So, in effect, you have constructed a mapping from the set of vertices V of a graph G to pairs of real numbers (the (x,y) coordinate of the point in space with respect to some arbitrary coordinate system). In other words, you have constructed an embedding of the graph

Similarly, for word embeddings, suppose I want to know what the word “Democrat” means. Well, one way to ascribe meaning to this word is to find some way to map “Democrat” into an N-dimensional vector (say, N = 100), so that every instance of “Democrat” in a text document is searchd by that 100-dimensional vector.

In both these examples of graph embeddings and word embeddings, the trick of course is to construct an embedding that is “meaningful”. For example, if you map “Democrat” to some N-dimensional vector, and do so for every word in your vocabulary, then the embedding is useful if when I compute the nearest neighbors of “Democrat”, I get words like “Obama” or “Nancy Pelosi” (two famous Democrats!), and I should certainly not get “Trump” ( a famous “Republican”!).

How about for graphs? Well, suppose you are trying to infer some function defined on a graph (e.g., some property of users on a social network, which defines the graph). You are given the graph, but only a small number of vertices are labeled (say the social network labels some vertices “Democrat” and some vertices “Republican”, but most of the vertices are unlabeled, meaning the political affiliation of the individuals corresponding to these vertices is unknown).

So, how should we design an algorithm to “fill in” the labels of the unknown vertices, while exploiting the structure of the graph? A random method would be to plonk down the social network on paper, look at the (x,y) coordinate of every vertex with labels, create a training set of the labeled vertices, and then voila’, you have a simple 2D classification problem. This works rather poorly for the simple reason that it completely ignores the spatial connectivity of the graph.

So, you’d like to construct an embedding that respects the “geometry” or the “topology” of the graph, meaning that vertices that are “close” to each other in terms of the graph distance should receive “similar” labels. This captures the property that in a social network, people with similar tastes tend to hang around each other. So, “Democrats” tend to hang around with other “Democrats” and similarly, “Republicans” tend to hang out with other “Republicans”. For a real-world example of why this labeling is smooth on the graph, let's look at this image from the polling website FiveThirtyEight, which did a “what-if” scenario prediction of the midterm results from 2018, assuming that only women voted (that’s why it’s a “counterfactual”, meaning a “what-if’ type of analysis): as you can see, the red “Republican” labels are very smooth across spatial neighborhoods, as are the “Blue” democratic labels.

![img](https://qph.fs.quoracdn.net/main-qimg-03134e071f18d7117a8212c05aeca3a4)

OK, how does one construct an embedding of a graph such that smoothness of the label function across the graph topology is preserved. This leads us to one of the most beautiful results in machine learning of the last 25 years: the graph Laplacian. The Laplacian has been termed “the most beautiful object in all of mathematics and physics” (Nelson, Tensor Analysis). The reasons are not hard to see: every major equation in physics has the Laplacian operator (sometimes called the div-grad operator).

In a discrete graph, the graph Laplacian is simply L = D - A, where A is the adjacency matrix of an undirected graph, D is a diagonal matrix of the valencies of the graph (meaning the degree of each vertex). It is about as simple a matrix as one can define, but my, what beauties does it reveal! One never wants to analyze a graph by looking at the adjacency matrix! That tells you very little. Now, the graph Laplacian, ah, that's an entirely different kind of operator. It’s as revealing as a high precision microscope into everything that you'd like to know about a graph!

It would take too long to get into the beauties of this topic, but for those interested, I highly recommend Fan Chung’s brilliant monograph on “Spectral Graph Theory”. This brings the full bore power of differential geometry to the discrete setting, and Fan Chung’s book goes into much depth on this topic. I also recommend Spielman’s lecture notes at Yale, which have some nice examples of graph embeddings.

So, how does one construct graph embeddings using the Laplacian L = D-A. Simple. Just compute the eigenvectors of L, where each eigenvector is a vector of dimensionality equal to the number of vertices. The first eigenvector is all 1’s, and corresponds to the constant function. The associated eigenvalue is 0. The second eigenvector is a thing of beauty, the so-called “Fiedler” eigenvector after the mathematician who first studied it. There are many beautiful theorems about the second eigenvalue associated with the second eigenvector (it tells you how fast a random walk on the graph “mixes”, which is useful for so many things, like routing, or how information spreads in a social network, or how diseases spread across populations).

With this example out of the way, let's get to constructing word embeddings. How do we embed “Democrat” into a 100-dimensional space? Well, now we are dealing with sequential datasets, namely text documents. Suppose we read through all of Wikipedia (many gigabytes!), but a fast PC can scan all of Wikipedia in several hours now. So, every time the word “Democrat” appears in Wikipedia, we keep track of the surrounding “context” words (say, the 5 words preceding it and the 5 words succeeding it).

So, we build a gigantic matrix M, whose rows correspond to words like “Democrat” and whose columns correspond to all possible contexts C. This is a very large matrix, since there are hundreds of thousands of words in Wikipedia (including proper nouns), and millions of contexts. No matter, we have lots of memory and powerful machines. We take this giant matrix M, and find its singular value decomposition M = U S V’, where S is the diagonal matrix of singular values. The matrix U gives us an embedding of words.

Of course, we’d like an incremental way of computing word embeddings, which is far more efficient. Can’t we use a “neural network” to do this, and approximate what our linear algebraic approach does? This was exactly the approach taken by Mikolov in his beautiful work on “word2vec”, and you can download his rather elegant C code that computes embeddings of all the words in Wikipedia in one afternoon on a reasonably fast PC. This is an amazing piece of work, and has brought new life into NLP and ML. Many extensions have been explored, but Mikolov’s original paper is still worth reading.

https://arxiv.org/pdf/1301.3781.pdf

Here’s a nice picture of the concept of word embeddings and how to reason with them (spatial vector displacements allows one to compute analogical relationships, like King is to Queen as Man is to X, where X is of course “woman”).

![img](https://qph.fs.quoracdn.net/main-qimg-8259498effde9dc11cfd9a5535fbb308)

The Tensorflow web page explains the basic ideas nicely.

[Vector Representations of Words | TensorFlow](https://www.tensorflow.org/tutorials/representation/word2vec)

Now, are word embeddings Euclidean? That would be amazing, if true. A few years ago, while on sabbatical at IBM Research, when I was working with the Watson Deep Learning group, I explored how to reason about word embeddings using a non-Euclidean “manifold” approach. This produces far better results than Mikolov’s original approach. My paper on Arxiv on this topic can be downloaded here (warning: the math is a bit more heavy going than Mikolov’s work!).

[Reasoning about Linguistic Regularities in Word Embeddings using Matrix Manifolds](https://arxiv.org/abs/1507.07636)

Hope this has been helpful! Good luck exploring the wonderful world of embeddings. What does the brain do? How are meanings stored in our brains? Philosophers have been pondering this question for centuries. This is what ML research on embeddings is trying to also explain.