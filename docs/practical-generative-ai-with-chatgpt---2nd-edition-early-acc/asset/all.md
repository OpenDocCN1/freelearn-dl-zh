![](media/cover.jpeg)

![cover.jpegs](media/cover.jpeg)

1.  [Cover](text/cover.xhtml)
2.  [Table of contents](#nav.xhtml_toc)

# Practical Generative AI with ChatGPT, Second Edition: Start your generative AI journey by using ChatGPT to maximize your productivity and creativity

**Welcome to Packt Early Access**. We’re giving you an exclusive preview of this book before it goes on sale. It can take many months to write a book, but our authors have cutting-edge information to share with you today. Early Access gives you an insight into the latest developments by making chapter drafts available. The chapters may be a little rough around the edges right now, but our authors will update them over time.

You can dip in and out of this book or follow along from start to finish; Early Access is designed to be flexible. We hope you enjoy getting to know more about the process of writing a Packt book.

1.  Chapter 1: Introduction to Generative AI
2.  Chapter 2: Introducing OpenAI and ChatGPT
3.  Chapter 3: From Prompt Design to Prompt Engineering
4.  Chapter 4: Boosting Day-to-Day Productivity with ChatGPT
5.  Chapter 5: Developing the Future with ChatGPT
6.  Chapter 6: Mastering Marketing with ChatGPT
7.  Chapter 7: Research Reinvented with ChatGPT
8.  Chapter 8: Unleashing Creativity Visually with ChatGPT
9.  Chapter 9: Exploring GPTs
10.  Chapter 10: Leveraging OpenAI’s Models for Enterprise- Scale Applications with Models’ APIs
11.  Chapter 11: Trending Use Cases
12.  Chapter 12: Epilogue and Final Thoughts

# 1 Introduction to Generative AI

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

Hello! Welcome to *The Ultimate Guide to ChatGPT and OpenAI!* In this book, we will explore the fascinating world of **Generative** **Artificial Intelligence** (**GAI**) and its groundbreaking applications. Generative AI has transformed the way we interact with machines, enabling computers to create, predict, and learn without explicit human instruction. Since the launch of ChatGPT in November 2023, we have witnessed unprecedented advances in natural language processing, image and video synthesis, and many other fields. Whether you are a curious beginner or an experienced practitioner, this guide will equip you with the knowledge and skills to navigate the exciting landscape of generative AI. So, let’s dive in and start with some definitions of the context we are moving in.

This chapter provides an overview of the field of generative AI, which consists of creating new and unique data or content using **machine learning** (**ML**) algorithms.

It focuses on the applications of generative AI to various fields, such as image synthesis, text generation, and music composition, highlighting the potential of generative AI to revolutionize various industries. This introduction to generative AI will provide context for where this technology lives, as well as the knowledge to collocate it within the wide world of AI, ML, and **Deep Learning** (**DL**). Then, we will dwell on the main areas of applications of generative AI with concrete examples and recent developments so that you can get familiar with the impact it may have on businesses and society in general.

Also, being aware of the research journey toward the current state of the art of generative AI will give you a better understanding of the foundations of recent developments and state-of-the-art models.

All this, we will cover with the following topics:

*   Understanding generative AI
*   Exploring the domains of generative AI
*   The history and current status of research on generative AI

By the end of this chapter, you will be familiar with the exciting world of generative AI, its applications, the research history behind it, and the current developments, which could have – and are currently having – a disruptive impact on businesses.

## Introducing generative AI

AI has been making significant strides in recent years, and one of the areas that has seen considerable growth is generative AI. Generative AI is a subfield of AI and DL that focuses on generating new content, such as images, text, music, and video, by using algorithms and models that have been trained on existing data using ML techniques.

In order to better understand the relationship between AI, ML, DL, and generative AI, consider AI as the foundation, while ML, DL, and generative AI represent increasingly specialized and focused areas of study and application:

*   AI represents the broad field of creating systems that can perform tasks, showing human intelligence and ability and being able to interact with the ecosystem.
*   ML is a branch that focuses on creating algorithms and models that enable those systems to learn and improve themselves with time and training. ML models learn from existing data and automatically update their parameters as they *grow*.
*   DL is a sub-branch of ML, in the sense that it encompasses deep ML models. Those deep models are called **neural networks** and are particularly suitable in domains such as **computervision** or **Natural Language Processing** (**NLP**). When we talk about ML and DL models, we typically refer to discriminative models, whose aim is that of making predictions or inferencing patterns on top of data.
*   And finally, we get to generative AI, a further sub-branch of DL, which doesn’t use deep Artificial Neural Networks to cluster, classify, or make predictions on existing data: it uses those powerful Artificial Neural Network models to generate brand new content, from images to natural language, from music to video.

The following figure shows how these areas of research are related to each other:

![Figure 1.1 – Relationship between AI, ML, DL, and generative AI](media/file1.jpg)

Figure 1.1 – Relationship between AI, ML, DL, and generative AI

Generative AI models can be trained on vast amounts of data and then they can generate new examples from scratch using patterns in that data. This generative process is different from discriminative models, which are trained to predict the class or label of a given example.

The type of Neural Networks that feature Generative AI are called Large Foundation Models LFMs) and, in the case of language models like ChatGPT, we talk about **Large Language Models** (**LLMs**).

> Large Language Models are a type of Artificial Neural Network featured by a transformer architecture. They are characterized by a huge number of parameters (in the order of trillions of parameters) and have been trained on billions of words. Given the training set, LLMs are capable of understanding and generating natural language given user’s input.

Even though text understanding and generation is probably one of the most outstanding features of Generative AI, this field covers many domains.

### Domains of generative AI

In recent years, generative AI has made significant advancements and has expanded its applications to a wide range of domains, such as art, music, fashion, architecture, and many more. In some of them, it is indeed transforming the way we create, design, and understand the world around us. In others, it is improving and making existing processes and operations more efficient.

The fact that generative AI is used in many domains also implies that its models can deal with different kinds of data, from natural language to audio or images. Let us understand how generative AI models address different types of data and domains.

### Text generation

One of the greatest applications of generative AI—and the one we are going to cover the most throughout this book—is its capability to produce new content in natural language. Indeed, generative AI algorithms can be used to generate new text, such as articles, poetry, and product descriptions.

For example, a language model such as GPT-4o, developed by OpenAI, can be trained on large amounts of text data and then used to generate new, coherent, and grammatically correct text in different languages (both in terms of input and output), as well as extracting relevant features from text such as keywords, topics, or full summaries.

Here is an example of working with GPT-3:

![Figure 1.2 – Example of ChatGPT responding to a user prompt, also adding references](media/file2.png)

Figure 1.2 – Example of ChatGPT responding to a user prompt, also adding references

Next, we will move on to image generation.

### Image generation

One of the earliest and most well-known examples of generative AI in image synthesis is the **Generative Adversarial Network** (**GAN**) architecture introduced in the 2014 paper by I. Goodfellow et al., *Generative Adversarial Networks*. The purpose of GANs is to generate realistic images that are indistinguishable from real images. This capability had several interesting business applications, such as generating synthetic datasets for training computer vision models, generating realistic product images, and generating realistic images for virtual reality and augmented reality applications.

Here is an example of faces of people who do not exist since they are entirely generated by AI:

![Figure 1.3 – Imaginary faces generated by GAN StyleGAN2 at https://this-person-does-not-exist.com/en](media/file3.jpg)

Figure 1.3 – Imaginary faces generated by GAN StyleGAN2 at https://this-person-does-not-exist.com/en

Then, in 2021, a new generative AI model was introduced in this field by OpenAI, **DALL-E**. Different from GANs, the DALL-E model is designed to generate images from descriptions in natural language (GANs take a random noise vector as input) and can generate a wide range of images, which may not look realistic but still depict the desired concepts.

DALL-E has great potential in creative industries such as advertising, product design, and fashion, among others, to create unique and creative images.

Since its first release to date (December 2024), DALL-E has improved dramatically, as you can see in the following examples. Let’s see below an artistic creation authored by DALL-E at the dawn of its life:

![Figure 1.4 – Images generated by DALL-E with a natural language prompt as input](media/file4.jpg)

Figure 1.4 – Images generated by DALL-E with a natural language prompt as input

Let’s now see what DALL-E3, the most recent version of the model at the time of writing this book, can produce (here I’m using Microsoft Image Creator powered by DALL-E3\. You can try it at [https://copilot.microsoft.com/images/create](https://copilot.microsoft.com/images/create)):

![Figure 1.5 – Images generated by DALL-E3 with a natural language prompt as input](media/file5.png)

Figure 1.5 – Images generated by DALL-E3 with a natural language prompt as input

It’s impressive to see the level of improvement of this model in less than 18 months. Funny enough, we are just scraping the surface of the massive improvements occurring over the last months in the field of Generative AI.

### Music generation

The first approaches to generative AI for music generation trace back to the 50s, with research in the field of algorithmic composition, a technique that uses algorithms to generate musical compositions. In fact, in 1957, Lejaren Hiller and Leonard Isaacson created the Illiac Suite for String Quartet ([https://www.youtube.com/watch?v=n0njBFLQSk8](https://www.youtube.com/watch?v=n0njBFLQSk8)), the first piece of music entirely composed by AI. Since then, the field of generative AI for music has been the subject of ongoing research for several decades. Among recent years’ developments, new architectures and frameworks have become widespread among the general public, such as the WaveNet architecture introduced by Google in 2016, which has been able to generate high-quality audio samples, or the Magenta project, also developed by Google, which uses **Recurrent Neural Networks** (**RNNs**) and other ML techniques to generate music and other forms of art. Then, in 2020, OpenAI also announced Jukebox, a neural network that generates music, with the possibility to customize the output in terms of musical and vocal style, genre, reference artist, and so on.

Those and other frameworks became the foundations of many AI composer assistants for music generation. An example is Flow Machines, developed by Sony CSL Research. This generative AI system was trained on a large database of musical pieces to create new music in a variety of styles. It was used by French composer Benoît Carré to compose an album called *Hello World* ([https://www.helloworldalbum.net/](https://www.helloworldalbum.net)), which features collaborations with several human musicians.

Here, you can see an example of a track generated entirely by Music Transformer, one of the models within the Magenta project:

![Figure 1.6 – Music Transformer allows users to listen to musical performances generated by AI](media/file6.jpg)

Figure 1.6 – Music Transformer allows users to listen to musical performances generated by AI

Another incredible application of generative AI within the music domain is speech synthesis. It is indeed possible to find many AI tools that can create audio based on text inputs in the voices of well-known singers.

For example, if you have always wondered how your songs would sound if Kanye West performed them, well, you can now fulfill your dreams with tools such as FakeYou.com ([https://fakeyou.com/](https://fakeyou.com/)), Deep Fake Text to Speech, or UberDuck.ai ([https://uberduck.ai/](https://uberduck.ai/)).

![Figure 1.7 – Text-to-speech synthesis with UberDuck.ai](media/file7.jpg)

Figure 1.7 – Text-to-speech synthesis with UberDuck.ai

I have to say, the result is really impressive. If you want to have fun, you can also try voices from your all your favourite cartoons as well, such as Winnie The Pooh...

Let’s go even further. What if we could generate a song from scratch, just asking the GenAI to do that for us in natural language? Well, today we can do that seamlessly and without any knowledge about music. Among the GenAI products that are rising in the musing market today, a great example is Suno, whose mission is *“[...] is building a future where anyone can make great music. Whether you're a shower singer or a charting artist, we break barriers between you and the song you dream of making. No instrument needed, just imagination. From your mind to music.”* (source: [https://suno.com/about](https://suno.com/about)).

![](media/file8.png)

Can you believe that it became my summer 2024 hit? If you want to create your summer hit too, you can try it for free at [https://suno.com/create](https://suno.com/create).

### Video generation

Generative AI for video generation shares a similar timeline of development with image generation. In fact, one of the key developments in the field of video generation has been the development of GANs. Thanks to their accuracy in producing realistic images, researchers have started to apply these techniques to video generation as well. One of the most notable examples of GAN-based video generation is DeepMind’s **Motion to Video**, which generated high-quality videos from a single image.

and a sequence of motions. Another great example is NVIDIA’s **Video-to-Video Synthesis** (**Vid2Vid**) DL-based framework, which uses GANs to synthesize high-quality videos from input videos.

The Vid2Vid system can generate temporally consistent videos, meaning that they maintain smooth and realistic motion over time. The technology can be used to perform a variety of video synthesis tasks, such as the following:

*   Converting videos from one domain into another (for example, converting a daytime video into a nighttime video or a sketch into a realistic image)
*   Modifying existing videos (for example, changing the style or appearance of objects in a video)
*   Creating new videos from static images (for example, animating a sequence of still images)

In September 2022, Meta’s researchers announced the general availability of **Make-A-Video** ([https://makeavideo.studio/](https://makeavideo.studio/)), a new AI system that allows users to convert their natural language prompts into video clips. Behind such technology, you can recognize many of the models we mentioned for other domains so far – language understanding for the prompt, image and motion generation with image generation, and background music made by AI composers.

Now, everything we’ve mentioned above pales in comparison with the latest text-to-video models. To name one, OpenAI announced in February 2024 a new text-to-video model called SORA, releasing some early experiments that were something unexpected:

![](media/file9.png)![Figure 1: Videos generated by SORA from a prompt in natural language. Source: https://openai.com/index/sora/](media/file10.png)

Figure 1: Videos generated by SORA from a prompt in natural language. Source: https://openai.com/index/sora/

I do encourage you to visit the SORA webpage to have a look at the amazing videos it created. At the time of writing, SORA is not publicly available, as it is going through several tests ran by the OpenAI Red Team.

Overall, generative AI has impacted many domains for years, and some AI tools already consistently support artists, organizations, and general users. The future seems very promising; however, before jumping to the ultimate models available on the market today, we first need to have a deeper understanding of the roots of generative AI, its research history, and the recent developments that eventually lead to the current OpenAI models.

## The history and current status of research

In previous sections, we had an overview of the most recent and cutting-edge technologies in the field of generative AI, all developed in recent years. However, the research in this field can be traced back decades ago.

We can mark the beginning of research in the field of generative AI in the 1960s, when Joseph Weizenbaum developed the chatbot ELIZA, one of the first examples of an NLP system. It was a simple rules-based interaction system aimed at entertaining users with responses based on text input, and it paved the way for further developments in both NLP and generative AI. However, we know that modern generative AI is a subfield of DL and, although the first **Artificial Neural Networks** (**ANNs**) were first introduced in the 1940s, researchers faced several challenges, including limited computing power and a lack of understanding of the biological basis of the brain. As a result, ANNs hadn’t gained much attention until the 1980s when, in addition to new hardware and neuroscience developments, the advent of the **backpropagation** algorithm facilitated the training phase of ANNs. Indeed, before the advent of backpropagation, training Neural Networks was difficult because it was not possible to efficiently calculate the gradient of the error with respect to the parameters or weights associated with each neuron, while backpropagation made it possible to automate the training process and enabled the application of ANNs.

Then, by the 2000s and 2010s, the advancement in computational capabilities, together with the huge amount of available data for training, yielded the possibility of making DL more practical and available to the general public, with a consequent boost in research.

In 2013, Kingma and Welling introduced a new model architecture in their paper *Auto-Encoding Variational Bayes*, called **Variational Autoencoders** (**VAEs**). VAEs are generative models that are based on the concept of variational inference. They provide a way of learning with a compact representation of data by encoding it into a lower-dimensional space called **latent space** (with the *encoder* component) and then decoding it back into the original data space (with the *decoder* component).

The key innovation of VAEs is the introduction of a probabilistic interpretation of the latent space. Instead of learning a deterministic mapping of the input to the latent space, the encoder maps the input to a probability distribution over the latent space. This allows VAEs to generate new samples by sampling from the latent space and decoding the samples into the input space.

For example, let’s say we want to train a VAE that can create new pictures of cats and dogs that look like they could be real.

To do this, the VAE first takes in a picture of a cat or a dog and compresses it down into a smaller set of numbers into the latent space, which represent the most important features of the picture. These numbers are called **latent variables**.

Then, the VAE takes these latent variables and uses them to create a new picture that looks like it could be a real cat or dog picture. This new picture may have some differences from the original pictures, but it should still look like it belongs in the same group of pictures.

The VAE gets better at creating realistic pictures over time by comparing its generated pictures to the real pictures and adjusting its latent variables to make the generated pictures look more like the real ones.

VAEs paved the way toward fast development within the field of generative AI. In fact, only 1 year later, GANs were introduced by Ian Goodfellow. Differently from VAEs architecture, whose main elements are the encoder and the decoder, GANs consist of two Neural Networks – a generator and a discriminator – which work against each other in a zero-sum game.

The generator creates fake data (in the case of images, it creates a new image) that is meant to look like real data (for example, an image of a cat). The discriminator takes in both real and fake data, and tries to distinguish between them – it’s the *critic* in our art forger example.

During training, the generator tries to create data that can fool the discriminator into thinking it’s real, while the discriminator tries to become better at distinguishing between real and fake data. The two parts are trained together in a process called **adversarial training**.

Over time, the generator gets better at creating fake data that looks like real data, while the discriminator gets better at distinguishing between real and fake data. Eventually, the generator becomes so good at creating fake data that even the discriminator can’t tell the difference between real and fake data.

Here is an example of human faces entirely generated by a GAN:

![Figure 1.8 – Examples of photorealistic GAN-generated faces (taken from Progressive Growing of GANs for Improved Quality, Stability, and Variation, 2017: https://arxiv.org/pdf/1710.10196.pdf)](media/file11.png)

Figure 1.8 – Examples of photorealistic GAN-generated faces (taken from Progressive Growing of GANs for Improved Quality, Stability, and Variation, 2017: https://arxiv.org/pdf/1710.10196.pdf)

Both models – VAEs and GANs – are meant to generate brand new data that is indistinguishable from original samples, and their architecture has improved since their conception, side by side with the development of new models such as PixelCNNs, proposed by Van den Oord and his team, and WaveNet, developed by Google DeepMind, leading to advances in audio and speech generation.

Another great milestone was achieved in 2017 when a new architecture, called *Transformer*, was introduced by Google researchers in the paper, – *Attention Is All You Need*, was introduced in a paper by Google researchers. It was revolutionary in the field of language generation since it allowed for parallel processing while retaining memory about the context of language, outperforming the previous attempts of language models founded on RNNs or **Long Short-Term Memory** (**LSTM**) frameworks.

Transformers were indeed the foundations for massive language models called **Bidirectional Encoder Representations from Transformers** (**BERT**), introduced by Google in 2018, and they soon become the baseline in NLP experiments.

Transformers are also the foundations of all the **Generative Pre-Trained** (**GPT**) models introduced by OpenAI, including GPT-3, the model behind ChatGPT, as well as other Large Foundation Models.

Although there was a significant amount of research and achievements in those years, it was not until the second half of 2022 that the general attention of the public shifted toward the field of generative AI.

Not by chance, 2022 has been dubbed the *year of generative AI*. This was the year when powerful AI models and tools became widespread among the general public: diffusion-based image services (MidJourney, DALL-E 2, and Stable Diffusion), OpenAI’s ChatGPT, text-to-video (Make-a-Video and Imagen Video), and text-to-3D (DreamFusion, Magic3D, and Get3D) tools were all made available to individual users, sometimes also for free.

This had a disruptive impact for two main reasons:

*   Once generative AI models have been widespread to the public, every individual user or organization had the possibility to experiment with and appreciate its potential, even without being a data scientist or ML engineer.
*   The output of those new models and their embedded creativity were objectively stunning and often concerning. An urgent call for adaptation—both for individuals and governments—rose.

Henceforth, in the very near future, we will probably witness a spike in the adoption of AI systems for both individual usage and enterprise-level projects.

## 18 months later: main trends and innovations

Fast forward from November 2022 to today, we have witnessed a huge amount of innovation in the field of GenAI. Many of these innovations are linked to the brand-new models developed and released to the public, like the OpenAI’s GPT-4o and DALL-E3, but also Google Gemini, Meta Llama 3, Microsoft Phi3 and many others.

However, the most remarkable achievements probably lie in the way we interact with and build applications around those models. In this section, we are going to explore three main advancements that have marked the most popular reference architectures for GenAI-powered applications.

### Retrieval Augmented Generation

One of the first limitations of ChatGPT and, generally speaking, of LLMs, was that of the knowledge base cutoff. In fact, the knowledge of LLMs is limited to the training set (made od public information from the internet) they have been trained on and, as long as this can be exhaustive, it’s not up to date. Plus, it is missing the proprietary knowledge base that might be relevant for us or our organization. For example, if you ask ChatGPT “what are my company’s policy for employees health insurance?”, the model won’t be able to answer since it has no access to this information.

To bypass this limitation, a new framework was designed to allow LLMs to navigate through customized documentations that we can provide. This framework is called Retrieval Augmented Generation (RAG).

The idea behind RAG is that of decoupling the LLM from the knowledge base we want to navigate through. To make it possible, the external knowledge base needs to be transformed into numerical vectors through a process called embedding and stored into a specialized Vector Database.

> An embedding is a way of representing high-dimensional, non-numeric data, such as words or sentences, in a lower-dimensional space, such as vector. A text embedding can capture the semantic and syntactic features of the text, such as meaning, context, and similarity.
> 
> > Each embedding is a vector of floating-point numbers, such that the distance between two embeddings in the vector space is correlated with semantic similarity between two inputs
> > in the original format.

For example, if two concepts are similar, then their vector representations should also be similar.

![](media/file12.png)

RAG is made of three phases:

*   **Retrieval**: given a user’s query and its corresponding vector, the most similar pieces of documents (those corresponding to the vectors that are closer to the user query’s vector) are retrieved and used as the base context for the LLM.

    ![](media/file13.png)
*   **Augmentation**: the retrieved context is enriched through additional instructions, rules, safety guardrails and similar practices that are typical of prompt engineering techniques (we will cover the topic of prompt engineering in Chapter 3).

    ![](media/file14.png)
*   **Generation**: based on the augmented context, the LLM generates the response to the user’s query.

![](media/file15.png)

The overall process looks as follows:

![](media/file16.png)

RAG combines the strengths of generative models and information retrieval systems to enhance the quality and relevance of generated content. Traditional generative models rely solely on their training data to produce responses, which can sometimes result in outdated or irrelevant information. RAG addresses this limitation by integrating external knowledge bases during the generation process.

For example, OpenAI's ChatGPT, when enhanced with RAG, can pull in current data from reliable sources to answer a query about recent events, which would be beyond the scope of its training cut-off. This synergy of retrieval and generation broadens the applicability of generative AI, making it more robust and useful in dynamic, information-rich environments.

### Multimodality

In the first paragraph of this Chapter, we saw the various domains of Generative AI, ranging from text to images, from videos to music. Typically, Large Foundation Models tend to me domain-specific, as we saw for Large Language Models in the case of language understanding and generation, or DALL-E3 in case of image generation.

However, the recent advances in Generative AI have enabled the development of Large Multimodal Models (LMMs) that can process and generate different types of data, such as text, images, audio, and video.

LMMs share with “standard” Large Language Models (LLMs) the capability of generalization and adaptation typical of Large Foundation Models. However, LMMs are capable of processing heterogeneous data with the idea of mirroring the way humans interact with the surrounding ecosystem – that is, with all our senses.

A great example of multimodal model is OpenAI’s GPT-4o, which is able to interact with users via text, images and audio. Let’s see the following example:

![](media/file17.png)

As you can see, the model was able to analyze the image and reason over it. Let’s now go ahead and ask the model to generate an illustration:

![](media/file18.png)

The most interesting fact about LMMs is that they preserve their reasoning capabilities, making them suitable for complex reasoning in heterogeneous data contexts. Let’s consider this last example (showing only the first lines of the response):

![](media/file19.png)

As you may imagine, this opens a landscape of applications in various industries, and we are going to see some concrete examples in the upcoming chapters.

### AI Agents

In previous sections, we uncovered how LFMs are great when it comes to resonate and generate content. However, they lack one ability, that is the one of taking actions and interact with the surrounding ecosystem that goes beyond the single user. For example, what if we want our LLM to be able not only to generate an amazing LinkedIn post, but also publish it on our page?

To overcome this limitation, AI agents emerge as key players. But what exactly are they? Agents can be seen as AI systems powered by Large Language Models that, given a user’s query, are able to interact with the surrounding ecosystem to the extent to which we allow them to. The perimeter of the ecosystem is delimited by the tools (or plug-ins) we provide the agents with (in our previous example, we might provide the agent with a LinkedIn plug-in, so that it is able to post the generated content)

Agents are made of the following ingredients:

*   An LLM which acts as the reasoning engine of the AI system
*   A system message which instructs the Agent to behave and think in a given way. For example, you can design an Agent as a teaching assistant for students with the following system message: “You are a teaching assistant. Given a student’s query, NEVER provide the final answer, but rather provide some hints to get there”.
*   A set of tools the Agent can leverage to interact with the surrounding ecosystem.

AI Agents are a perfect representation of the meaning of “LLM as reasoning engine of an application”. In fact, the beauty of Agents is that they can pick the best tool to use to accomplish a user’s request. For example, let’s say we have an AI agent to produce LinkedIn content, and we provide it with two tools: a LinkedIn plug-in and a web search plug-in (each one with a proper description of its functionality). Then, let’s explore the behaviour of the agent upon two different questions:

*   Generate a story about a little dog walking around the mountains🡪the Agent will generate the story without involving any plug-in.
*   Generate a story about the current weather in Milan🡪the Agent will invoke the web search plug-in to get the current weather in Milan.
*   Generate a LinkedIn post about the current weather in Mialn and publish it on my profile:the Agent will invoke the web search plug-in to get the current weather in Milan and the LinkedIn plug-in to post it on my profile.

The combinations of instructions and set of plug-ins make AI Agents extremely versatile, and you can create highly specialized entities to address specific scenarios.

And that’s not all.

Why having only one Agents if you can create your own crew of Agents talking to and cooperating among each other? Imagine multiple agents, each one with a specific expertise and goal, communicating and interacting among each other to accomplish a task. This is how **multi-agent applications** look like, and in the last few months, this pattern started showing emergent behaviors.

Let’s consider the following example. We want to generate an elevator pitch about the climate change. We need up to date information to do so (latest trends and research, future perspectives and so on), as well as solid research grounded by academic papers. Plus, we need to be concise yet sharp and effective, delivering all the key information in a very short pitch.

Now, we could ask all of that to a single agent, providing it with all the needed tools and with long instructions to accomplish the task. However, it is proven that LLMs tend to perform worse when we give them “too many things to do”. Instead, let’s use a multi-agent approach, creating a team with the following AI professionals:

*   A market analyst who can search the web for the latest news about climate change this will be an Agent with a web search plug-in and specific instructions to search for news;
*   An expert researcher who can easily navigate through academic research papers about climate change this will be an Agent with an Arxiv plug-in and specific instructions on how to retrieve relevant information;
*   A expert in public speaking who can easily consolidate all the information in one elevator pitch this will be an Agent with proper instructions on how to deliver perfect pitches;
*   A critic who will review the pitch and propose some changes to the expert in public speaking, if needed this will be an Agent with proper instructions on how to deliver perfect pitches;

So, upon the user’s question “Generate an elevator pitch about the current issue of climate change”, all the agents can start working on the project.

There are many frameworks that can help developer with multi-agent applications (including AutoGen, LangGraph, CrewAI), especially when it comes to the “flow” that we want our agents to follow. For example, we might want to enforce a specific number of iterations; or that all agents are invoked at least once; or even to involve us, as users, every iteration to provide further feedback to be incorporated in the upcoming iteration.

At the time of writing, multi-agent framework is showing promising advancements, yet it is still in an experimentation phase and probably still far from being enterprise-ready. Nevertheless, it is a glimpse of the outstanding reasoning capabilities behind LLMs and how they can unlock new ways of problem solving and creation.

### Small Language Models

Large Language Models are, unsurprisingly, large. It means that the architecture of the Artificial Neural Network featuring LLMs is made of a huge amount of parameters, of the order of trillions. Ideally, the higher the number of parameters, the better the reasoning capabilities of the LLM. However, with high numbers of parameters come high cost of training and hosting, since a powerful AI infrastructure is needed. Plus, the energy consumption required by those models raise serious questions about the environmental impact of LLMs training and their overall sustainability in the long run. To give you an idea, these are some numbers related to open-source LLMs with different sizes (measured in number of parameters):

![Figure 2: Carbon footprint and compute power required to train different models in the same datacenter. Source: https://arxiv.org/pdf/2302.13971](media/file20.png)

Figure 2: Carbon footprint and compute power required to train different models in the same datacenter. Source: https://arxiv.org/pdf/2302.13971

Luckily, over the last months, we witnessed a raising interest and investment in the research of smaller models, with the goal of maintaining decent reasoning capabilities while reducing the number of parameters. The idea is that smaller models, even if “less intelligent”, can still reach the desired requirements if specialized in more tailored activities. After all, do we really need huge and super powerful models of trillions of parameter to solve all the tasks that we have in mind?

These smaller models are called Small Language Models (SMLs) and, beside being lighter and less demanding in terms of infrastructure, they are also showing surprisingly high performance.

For example, if we consider the smaller version of Phi-3, part of the popular SLM Phi family developed by Microsoft, with “only” 7 billion parameters, we can see that it beats GPT-3.5-turbo in all the most popular LLMs benchmarks:

![Figure 3: benchmark table of Phi-3 Small capabilities compared to other SLMs and LLMs. Source: https://azure.microsoft.com/en-us/blog/introducing-phi-3-redefining-whats-possible-with-slms/](media/file21.png)

Figure 3: benchmark table of Phi-3 Small capabilities compared to other SLMs and LLMs. Source: https://azure.microsoft.com/en-us/blog/introducing-phi-3-redefining-whats-possible-with-slms/

Now, we might think that GPT-3.5-turbo is kind of “deprecated”, however we have to remember that, with its 175B parameters, it used to be the most powerful model in the market just 1 year ago, and it is remarkable to see that a 7B model is capable of better results.

That of SLMs is definitely a research stream to keep an eye on, especially when it comes to those scenarios where I might want to deploy my model locally or even customizing it with fine-tuning (we will cover fine tuning in the next chapter).

## Summary

In this chapter, we explored the exciting world of generative AI and its various domains of application, including image generation, text generation, music generation, and video generation. We learned how generative AI models such as ChatGPT and DALL-E, trained by OpenAI, use DL techniques to learn patterns in large datasets and generate new content that is both novel and coherent. We also discussed the history of generative AI, its origins, and the current status of research on it.

The goal of this chapter was to provide a solid foundation in the basics of generative AI and to inspire you to explore this fascinating field further.

In the next chapter, we will focus on one of the most promising technologies available on the market today, ChatGPT: we will go through the research behind it and its development by OpenAI, the architecture of its model, and the main use cases it can address as of today.

## References

*   [https://arxiv.org/abs/1406.2661](https://arxiv.org/abs/1406.2661)
*   [https://www.youtube.com/watch?v=Iy9vRvyRf_E](https://www.youtube.com/watch?v=Iy9vRvyRf_E)
*   [https://arxiv.org/abs/1912.04958](https://arxiv.org/abs/1912.04958)
*   This person does not exist: [https://this-person-does-not-exist.com](https://this-person-does-not-exist.com)
*   [https://arxiv.org/abs/1808.06601](https://arxiv.org/abs/1808.06601)
*   [https://www.microsoft.com/en-us/research/blog/a-deep-generative- model-trifecta-three-advances-that-work-towards-harnessing- large-scale-power/](https://www.microsoft.com/en-us/research/blog/a-deep-generative-)
*   [https://tcwang0509.github.io/vid2vid/](https://tcwang0509.github.io/vid2vid/)
*   [https://arxiv.org/pdf/2302.13971](https://arxiv.org/pdf/2302.13971)
*   [https://azure.microsoft.com/en-us/blog/introducing-phi-3-redefining-whats-possible-with-slms/](https://azure.microsoft.com/en-us/blog/introducing-phi-3-redefining-whats-possible-with-slms/)

# 2 OpenAI and ChatGPT: Beyond the Market Hype

## Join our book community on Discord

![](media/file22.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

This chapter provides an overview of OpenAI and its most notable development—ChatGPT, highlighting its history, technology, and capabilities.

The overall goal is to provide a deeper knowledge of how ChatGPT can be used in various industries and applications to improve communication and automate processes and, finally, how those applications can impact the world of technology and beyond.

We will cover all this with the following topics:

*   What is OpenAI?
*   Overview of OpenAI model families
*   Road to ChatGPT: the math of the model behind it
*   ChatGPT: the state of the art

## Technical requirements

In order to be able to test the example in this chapter, you will need the following:

*   An OpenAI account to access the Playground and the Models API ([https://openai.com/api/login20](https://openai.com/api/login20))
*   Your favorite IDE environment, such as Jupyter or Visual Studio
*   Python 3.7.1+ installed ([https://www.python.org/downloads](https://www.python.org/downloads))
*   `pip` installed ([https://pip.pypa.io/en/stable/installation/](https://pip.pypa.io/en/stable/installation/))
*   OpenAI Python library ([https://pypi.org/project/openai/](https://pypi.org/project/openai/))

## What is OpenAI?

OpenAI is a research organization founded in 2015 by Elon Musk, Sam Altman, Greg Brockman, Ilya Sutskever, Wojciech Zaremba, and John Schulman. As stated on the OpenAI web page, its mission is “to ensure that Artificial General Intelligence (AGI) benefits all of humanity”. As it is general, AGI is intended to have the ability to learn and perform a wide range of tasks, without the need for task-specific programming.

Since 2015, OpenAI has focused its research on **Deep** **Reinforcement** **Learning** (**DRL**), a subset of **machine** **learning** (**ML**) that combines **Reinforcement** **Learning** (**RL**) with deep neural networks. The first contribution in that field traces back to 2016 when the company released OpenAI Gym, a toolkit for researchers to develop and test **RL** algorithms.

![Figure 2.1 – Landing page of Gym documentation (https://www.gymlibrary.dev/)](media/file23.jpg)

Figure 2.1 – Landing page of Gym documentation (https://www.gymlibrary.dev/)

OpenAI kept researching and contributing in that field, yet its most notable achievements are related to generative models—**Generative** **Pre-trained** **Transformers** (**GPT**).

After introducing the model architecture in their paper “Improving Language Understanding by Generative Pre-Training” and baptizing it **GPT-1**, OpenAI researchers soon released, in 2019, its successor, the GPT-2\. This version of the GPT was trained on a corpus called **WebText**, which at the time contained slightly over 8 million documents with a total of 40 GB of text from URLs shared in Reddit submissions with at least 3 upvotes. It had 1.2 billion parameters, ten times as many as its predecessor.

Here, you can see the landing page of a UI of GPT-2 published by HuggingFace ([https:// transformer.huggingface.co/doc/distil-gpt2](https://transformer.huggingface.co/doc/distil-gpt2)):

![Figure 2.2 – GPT-2 writing a paragraph based on a prompt. Source: https://transformer.huggingface.co/doc/distil-gpt2](media/file24.jpg)

Figure 2.2 – GPT-2 writing a paragraph based on a prompt. Source: https://transformer.huggingface.co/doc/distil-gpt2

Then, in 2020, OpenAI first announced and then released GPT-3, which, with its 175 billion parameters, dramatically improved benchmark results over GPT-2\. It was with the GPT-3 model – more precisely, with its fine-tuned version called GPT-3.5 – that we entered the era of ChatGPT: in fact, at its first release, ChatGPT was powered by GPT-3.5.

From that moment (November 2022) to today, OpenAI released many new version of its GPT series: GPT-4, GPT-4-turbo, GPT-4vision (first multimodal model) and GPT-4o, where the “o” stands for “omni”, referring to its multimodal capabilities.

In addition to natural language generative models, OpenAI also developed in other fields of Generative AI, including:

*   **DALL-E**🡪 A text-to-image model that generates detailed and creative images from textual descriptions. It can produce a wide variety of images, from realistic scenes to imaginative and abstract art.
*   **Whisper**🡪 A speech recognition model designed to transcribe spoken language into text. It excels in understanding various accents, dialects, and noisy environments, making it highly effective for real-time transcription and voice-controlled applications.
*   **SORA**🡪A text-to-video model that generates realistic and imaginative videos from text instructions. SORA can create videos up to a minute long, maintaining visual quality and adherence to the user’s prompt. At the time of writing, SORA is not available to the general public yet.

Although OpenAI has invested in many fields of Generative AI, its contribution to text understanding and generation has been outstanding, thanks to the development of the foundation GPT models we are going to explore in the following paragraphs.

## An overview of OpenAI Playground

Today, OpenAI offers a set of pre-trained, ready-to-use models that can be consumed by the general public. This has two important implications:

*   Powerful foundation models can be consumed without the need for long and expensive training
*   It’s not necessary to be a data scientist or an ML engineer to manipulate those models

Users can test OpenAI models in two ways:

*   OpenAI Playground, a friendly user interface where you can interact with models without the need to write any code.
*   Via REST API with a pro-code approach. In fact, all OpenAI models – and, generally speaking, LLMs – comes as pre-built components that can be consumed by applications via endpoints and keys.

In this chapter, we will explore the first approach, while we will see how to incorporate OpenAI models’ APIs in Chapter 11.

Before jumping into the Playground, let’s first have an overview of OpenAI’s models families.

### OpenAI Models Families

Over the last years, OpenAI made huge advancements in the field of model developments, releasing newer models’ versions at insane speed. In this section, we will see the main models divided by domain.

*   **Language Models**🡪OpenAI's GPTs are advanced language models designed to generate human-like text based on given prompts. They are versatile and can be used for various natural language processing tasks such as text completion, translation, summarization, and coding. This is the field where OpenAI demonstrates outstanding performance thanks to its flagship models family: the GPTs. Since November 2022, when ChatGPT was launched, OpenAI released:
    *   GPT-3.5-turbo, the model behind the first version of ChatGPT
    *   GPT-4 (first multimodal model) and GPT-4-turbo (optimized for chats and assistants)
    *   GPT-4o, the most advanced multimodal model
    *   GPT-4o mini, the smaller version of the GPT-4o which is faster and cheaper.

In addition to those latest models, users can access a set of so called “GPT base models”, like the Babbage-002 or davinci-002 (the GPT-2), that represents the legacy completions APIs (we will cover completions APIs in the next section).

*   **Image model**🡪 OpenAI's image models, like DALL-E, are designed to generate and manipulate images from textual descriptions. DALL-E models can create highly detailed and imaginative visuals, enabling users to produce unique artwork, design concepts, and more. For instance, DALL-E 3 (the latest version of the model) is capable of creating intricate and creative images based on complex prompts, pushing the boundaries of what AI can do in the field of visual arts. These models are particularly useful in creative industries, digital marketing, and any field that benefits from high-quality, custom visuals​.
*   **Text to speech and Speech to text**🡪 In Chapter 1, we already mentioned the Speech-to-text (STT) model developed by OpenAI, Whisper, that can handle and process audio input. In addition to that, OpenAI also developed Text-to-Speech (TTS) models that provide high-quality outputs that are both intelligible and expressive, making them suitable for a wide range of applications from customer service bots to educational tools. Text-to-Speech (TTS) models developed by OpenAI convert written text into spoken language, producing realistic and natural-sounding audio. These models are essential for creating voice assistants, improving accessibility for visually impaired users, and generating automated announcements.
*   **Text to video🡪**with the announcement of SORA, OpenAI revealed its cutting-edge text-to-video model capable of generating realistic and imaginative video scenes from text descriptions. By adapting techniques from DALL-E and incorporating transformers, SORA can create high-fidelity videos up to one minute long. It excels in maintaining 3D consistency, object permanence, and simulating interactions within videos. While still facing challenges like accurately modeling complex physics and ensuring temporal coherence, SORA holds significant potential for creative industries, offering new possibilities in video production and storytelling
*   **Embeddings**🡪Embeddings models from OpenAI transform text into numerical representations called vectors (or embeddings) that capture semantic meaning and are projected in a multi-dimensional vector space.

The mathematical distances between different instances in this space represent their similarity in terms of meaning. As an example, imagine the words queen, woman, king, and man. Ideally, in our multidimensional space, where words are vectors, if the representation is correct, we want to achieve the following:

![Figure 2.10 – Example of vectorial equations among words](media/file25.png)

Figure 2.10 – Example of vectorial equations among words

This means that the distance between woman and man should be equal to the distance between Queen and King.

Embeddings can be extremely useful in intelligent search scenarios. Indeed, by getting the embedding of the user input and the documents the user wants to search, it is possible to compute distance metrics (namely, cosine similarity) between the input and the documents. By doing so, we can retrieve the documents that are closer, in mathematical distance terms, to the user input.

The OpenAI’s embedding models **text-embedding-ada-002** and **text-embedding-3-large** models offer state-of-the-art performance in tasks such as text similarity, text search, and code search by creating dense vector representations of text. These embeddings allow for efficient and effective comparison of large text datasets, improving search accuracy and relevance

*   **Moderation**🡪OpenAI's moderation models are designed to detect and filter out inappropriate, harmful, or unsafe content in text. These models are integral to maintaining safe and respectful online environments by identifying potentially offensive or harmful language. The latest moderation model, **text-moderation-007**, is robust and effective in ensuring content compliance and safety across platforms. It helps developers and companies enforce community guidelines and prevent the spread of harmful content, thereby promoting safer digital interactions.

Overall, OpenAI has reached more and more coverage over the last years, addressing diverse areas of Generative AI with state-of-the-art models.

### Trying OpenAI models in the Playground

To access your OpenAI playground, you need to create an OpenAI account and navigate through [https://platform.openai.com/playground](https://platform.openai.com/playground). This is how the landing page looks like:

![Figure 2.4 – OpenAI Playground at https://platform.openai.com/playground](media/file26.png)

Figure 2.4 – OpenAI Playground at https://platform.openai.com/playground

As you can see from *Figure 2.4*, the Playground offers a UI where the user can start interacting with the model, which you can select on the right-hand side of the UI.

Before diving deeper into the main sections of the playground, let’s first define some jargon you will see in this chapter:

*   **Tokens**: Tokens can be considered as word fragments or segments that are used by the API to process input prompts. Unlike complete words, tokens may contain trailing spaces or even partial sub-words. To better understand the concept of tokens in terms of length, there are some general guidelines to keep in mind. For instance, one token in English is approximately equivalent to four characters, or three-quarters of a word.
*   **Prompt**: In the context of **natural** **language** **processing** (**NLP**) and ML, a prompt refers to a piece of text that is given as input to an AI language model to generate a response or output. The prompt can be a question, a statement, or a sentence, and it is used to provide context and direction to the language model.
*   **Context**: In the field of GPT, context refers to the words and sentences that come before the user’s prompt. This context is used by the language model to generate the most probable next word or phrase, based on the patterns and relationships found in the training data.
*   **Model** **confidence**: Model confidence refers to the level of certainty or probability that an AI model assigns to a particular prediction or output. In the context of NLP, model confidence is often used to indicate how confident the AI model is in the correctness or relevance of its generated response to a given input prompt.
*   **Functions**. Functions is yet another definition for the concept of tools or plug-ins introduced in Chapter 1\. With functions, we provide the model with an extra skill that it can invoke to accomplish user’s task. A function will always have a description in natural language, so that the model knows when to invoke it.

The preceding definitions will be pivotal in understanding how to use Azure OpenAI model families and how to configure their parameters.

In the Playground, there are four main sections to interact with the models:

**Chat**: Here you can test all the chat models available today, including both text-only models (like the GPT-3.5) or multimodal models (like the GPT-4o). You can provide a system message – that is the set of instructions that you provide your model with, all in natural language. You can also compare the output of two different models, given the same user’s question. The following is an example on how to do that:

![Figure 2.5 – An example of comparison between two models.](media/file27.png)

Figure 2.5 – An example of comparison between two models.

For each model, you can also play with some parameters that you can configure. Here is a list:

*   **Temperature** (ranging from 0 to 1): This controls the randomness of the model’s response. A low-level temperature makes your model more deterministic, meaning that it will tend to give the same output to the same question. For example, if I ask my model multiple times *What is OpenAI?* with temperature set as 0, it will always give the same answer. On the other hand, if I do the same with a model with temperature set as 1, it will try to modify its answers each time, in terms of wording and style.
*   **Max tokens:** This controls the length (in terms of tokens) of the model’s response to the user’s prompt.
*   **Stop sequences** (user input): This makes responses end at the desired point, such as the end of a sentence or list.
*   **Top** **probabilities** (ranging from 0 to 1): This controls which tokens the model will consider when generating a response. Setting this to 0.9 will consider the top 90% most likely of all possible tokens. One could ask *Why not set top probabilities as 1 so that all the most likely tokens are chosen?* The answer is that users might still want to maintain variety when the model has low confidence, even in the highest-scoring tokens.
*   **Frequency penalty** (ranging from 0 to 1): This controls the repetition of the same tokens in the generated response. The higher the penalty, the lower the probability of seeing the same tokens more than once in the same response. The penalty reduces the chance proportionally, based on how often a token has appeared in the text so far (this is the key difference from the following parameter).
*   **Presence penalty** (ranging from 0 to 2): This is similar to the previous one but stricter. It reduces the chance of repeating any token that has appeared in the text at all so far. As it is stricter than the frequency penalty, the presence penalty also increases the likelihood of introducing new topics in a response.

Besides trying OpenAI models in the Playground, you can always call the models API in your custom code and embed models into your applications. Indeed, in the right corner of the Playground, you can click on **View** **code** and export the configuration as shown here:

*   **Assistants**. OpenAI Assistants can be seen as a way to develop AI Agents (introduced in Chapter 1) faster and more easily. In fact, Assistants can be defined as entities powered by an LLM, with a set of instructions to follows and a set of tools or plug-ins to use – basically the same definition of AI Agents!

In the case of OpenAI assistants, they come with three pre-built tools:

*   File Search🡪it allows the user to upload custom documents so that the Assistant can navigate through it to accomplish the query. It operates with a RAG-based framework.
*   Function Calling🡪it allows the user to define a set of custom functions that can be invoked by the Assistant to accomplish a given task.
*   Code Interpreter🡪it refers to the capability of the Assistant to run code either against provided documents (for example, in case of spreadsheets or analytical papers that required mathematical computations) or simply to solve complex tasks provided by the user (for example, complex mathematical problems).

In the following picture, you can see an example of an Assistant called “Chat with PDF”, which is specialized in responding upon provided documents (in my case, I uploaded the paper “LLaMA: Open and Efficient Foundation Language Models” by Hugo Touvron et al.

![Figure 2.8 – Example of an OpenAI Assistant.](media/file28.png)

Figure 2.8 – Example of an OpenAI Assistant.

As you can see from the preceding screenshot, the Assistant was able to answer my question retrieving knowledge from the provided document. In fact, my question was pretty vague, since the term “toxicity” can refer to multiple domains; nevertheless, the Assistant knows to watch over the provided documents as primary source of information.

Completions. This section refers to a class of models called “base models”, like the GPT-3, they are the basis from where the so called “assistant models” (or chat models, as we saw previously) are built. For example, the chat model GPT-3.5-turbo (the model behind ChatGPT), is a fine-tuned version of the base model GPT-3.

> Completions (base) models are designed for generating single responses to prompts, making them suitable for tasks like text generation and summarization without maintaining context over multiple interactions. Chat (assistant) models, on the other hand, are optimized for interactive conversations, capable of maintaining context across multiple turns, and are ideal for applications like chatbots and virtual assistants.

Below you can see an example of a typical completion task in the Playground:

![Figure 1: Example of completion task in OpenAI Playground](media/file29.png)

Figure 1: Example of completion task in OpenAI Playground

As you can see, upon my words “Today I went to a grocery store and”, the model completed the sentence with the most likely words.

Today, completion models are rarely used as they are outperformed by chat models, yet they can be further fine-tuned to tailored use cases (we will cover fine-tuning later on in this section).

*   Text to speech. In addition to Whisper, the aforementioned speech-to-text model, OpenAI also released its TTS (text-to-speech) model that can be tested directly in the Playground.

Let’s see an example:

![Figure 2: Example of using OpenAI’s TTS models in the Playground](media/file30.png)

Figure 2: Example of using OpenAI’s TTS models in the Playground

As you can see from the above picture, you can select the voice, model, speed and format of the generated audio.

All the previous models come as pre-built, in the sense that they have already been pre-trained on a huge knowledge base.

However, there are some ways you can make your model more customized and tailored for your use case.

The first method is embedded in the way the model is designed, and it involves providing your model with the context in the **few-learning** **approach** (we will focus on this technique later on in the book). Namely, you could ask the model to generate an article whose template and lexicon recall another one you have already written. For this, you can provide the model with your query of generating an article *and* the former article as a reference or context, so that the model is better prepared for your request.

Here is an example of it:

![Figure 2.11 – An example of a conversation within the OpenAI Playground with the few-shot learning approach](media/file31.png)

Figure 2.11 – An example of a conversation within the OpenAI Playground with the few-shot learning approach

In the previous illustration, I instructed the model to output only the label of the tweet’s sentiment, providing it with three examples on how to do that.

The second method is more sophisticated and is called **fine-tuning**. Fine-tuning is the process of adapting a pre-trained model to a new task.

In fine-tuning, the parameters of the pre-trained model are altered, either by adjusting the existing parameters or by adding new parameters, to better fit the data for the new task. This is done by training the model on a smaller labeled dataset that is specific to the new task. The key idea behind fine-tuning is to leverage the knowledge learned from the pre-trained model and fine-tune it to the new task, rather than training a model from scratch. Have a look at the following figure:

![Figure 2.12 – Model fine-tuning](media/file32.jpg)

Figure 2.12 – Model fine-tuning

In the preceding figure, you can see a schema on how fine-tuning works on OpenAI pre-built models. The idea is that you have available a pre-trained model with general-purpose weights or parameters. Then, you feed your model with custom data, typically in the form of *key-value* prompts and completions as shown here:

```
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
...
```

Once the training is done, you will have a customized model that performs particularly well for a given task, for example, the classification of your company’s documentation.

The nice thing about fine-tuning is that you can make pre-built models tailored to your use cases, without the need to re-train them from scratch, yet leveraging smaller training datasets and hence less training time and computing. At the same time, the model keeps its generative power and accuracy learned via the original training, the one that occurred on the massive dataset.

In this paragraph, we got an overview of the models offered by OpenAI to the general public, from those you can try directly in the Playground (GPT, Codex) to more complex models such as embeddings. We also learned that, besides using models in their pre-built state, you can also customize them via fine-tuning, providing a set of examples to learn from.

In the following sections, we are going to focus on the background of those amazing models, starting from the math behind them and then getting to the great discoveries that made ChatGPT possible.

## ChatGPT: the state of the art

In November 2022, OpenAI announced the web preview of its conversational AI system, ChatGPT, available to the general public. This was the start of huge hype coming from subject matter experts, organizations, and general users – to the point that, after only 5 days, the service reached 1 million users!

Before writing about ChatGPT, I will let it introduce itself, using a snapshot taken a few days after the launch:

![Figure 2.25 – ChatGPT introduces itself in November 2022.](media/file33.png)

Figure 2.25 – ChatGPT introduces itself in November 2022.

As mentioned above, the first release of ChatGPT was built on top of an advanced language model that utilizes a modified version of GPT-3, which has been fine-tuned specifically for dialogue. This fine-tuned version is called GPT-3.5-turbo. The optimization process involved **Reinforcement Learning with Human Feedback** (**RLHF**), a technique that leverages human input to train the model to exhibit desirable conversational behaviors.

We can define RLHF as a machine learning approach where an algorithm learns to perform a task by receiving feedback from a human. The algorithm is trained to make decisions that maximize a reward signal provided by the human, and the human provides additional feedback to improve the algorithm’s performance. This approach is useful when the task is too complex for traditional programming or when the desired outcome is difficult to specify in advance.

The relevant differentiator here is that ChatGPT has been trained with humans in the loop so that it is aligned with its users. By incorporating RLHF, ChatGPT has been designed to better understand and respond to human language in a natural and engaging way.

> The same RLHF mechanism was used for what we can think of as the predecessor of ChatGPT— InstructGPT. In the related paper published by OpenAI’s researchers in January 2022, InstructGPT is introduced as a class of models that is better than GPT-3 at following English instructions.

ChatGPT is still available today as a free application anyone can use; however, since February 2023, OpenAI announced a new paid version called ChatGPT Plus and costs 20$/month, offering subscribers several advantages, including access to the latest models, fastest response time, a great set of plug-ins and the possibility to create your own Assistant in the GPTs playground.

Let’s have a quick tour of ChatGPT user interface at the current date:

![Figure 3: Landing page of ChatGPT at chatgpt.com](media/file34.png)

Figure 3: Landing page of ChatGPT at chatgpt.com

Let’s double click on each section:

1.  You can decide the model to use behind ChatGPT. In my case, I have set the GPT-4o, which is only available for paid subscription. This is the model we are going to use throughout this book.
2.  A set of pre-built prompts are proposed to the user to familiarize themselves with the application.
3.  The text box is the place where users can ask their questions. Note that there is a small clip in the left-hand corner: it indicates the possibility of uploading files the model will be able to navigate through. Those files can be either uploaded locally, or retrieved from cloud storage like Google Drive or OneDrive.
4.  On the left-hand sidebar, you can see the list of previous chats (mine are covered) you had with ChatGPT. This is an extremely useful tool, since in each chat, through the various round of interactions you had with the model, you created a context ChatGPT is aware of. This means that, if you want to continue a previously started conversation, you can open the related chat and start talking with the model without describing again the whole scenario.
5.  A nice recently added feature is the possibility of having shared workspaces with other users, so that you can cooperate with your team members. Note that this is an additional paid feature which is not included in the Plus plan, and it costs extra 5$/month.
6.  Finally, ChatGPT Plus offers the possibility of creating the GPTs, that are personalized assistant you can tailor for specific functions. You can decide to keep your GPT private or to publish it into the GPTs store, where anyone can use and rate it. We are going to cover the GPTs in a dedicated Chapter later on in this book.

Finally, ChatGPT Plus comes with two built-in plugins that the model can invoke autonomously if needed: the web search plug-in (to retrieve up-to-date information) and the code-interpreter plug-in (to process analytical documentation or perform complex mathematical tasks).

Throughout this book, we will leverage ChatGPT Plus to showcase the capabilities of the latest models and features, nevertheless the majority of examples that we will cover can be achieved with the free version of ChatGPT as well (which is currently powered by GPT-3.5-turbo).

The ongoing developments and improvements in ChatGPT’s architecture and training methods promise to push the boundaries of language processing even further.

## Summary

In this chapter, we went through the history of OpenAI, its research fields, and the latest developments, up to ChatGPT. We went deeper into the OpenAI Playground for the test environment and how we can leverage all the OpenAI’s models families.

With this first glance at the OpenAI models, we will also saw how easy it is to test or embed pre-trained models into your applications: the game-changer element here is that you don’t need powerful hardware and hours of time to train your models, since they are already available to you and can also be customized if needed, with a few examples.

In the next chapter, we also begin *Part 2* of this book, where we will see ChatGPT in action within various domains and how to unlock its potential. You will learn how to get the highest value from ChatGPT by properly designing your prompts, how to boost your daily productivity, and how it can be a great project assistant for developers, marketers, and researchers.

## References

*   Radford, A., & Narasimhan, K. (2018). Improving language understanding by generative pre-training.
*   Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, L., & Polosukhin, I. (2017). Attention Is All You Need. ArXiv. [https://doi.org/10.48550/ arXiv.1706.03762](https://doi.org/10.48550/) OpenAI. Fine-Tuning Guide. OpenAI platform documentation. [https://platform.openai.com/docs/guides/fine-tuning](https://platform.openai.com/docs/guides/fine-tuning).

# 3 Understanding Prompt Design

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In the previous chapters, we mentioned the term **prompt** several times while referring to user input in ChatGPT and LLMs models in general.

Since prompts have a massive impact on LLMs performance, prompt engineering is a crucial activity while designing LLM-powered applications. In fact, there are several techniques that can be implemented to not only to refine your LLM’s responses, but also to reduce risks associated with hallucination and biases.

In this chapter, we are going to cover the emerging techniques in the field of prompt engineering, starting from basic approaches up to advanced frameworks. More specifically, we will go through the following topics:

*   Introduction to prompt engineering
*   Zero, one and few shot learning
*   Basic principles of prompt engineering
*   Advanced techniques of prompt engineering
*   Mitigation of risks and hallucinations with prompt engineering
*   Handling prompt injections

By the end of this chapter, you will have the foundations to build functional and solid prompts for your LLM-powered applications, which will also be relevant in the upcoming chapters.

## What is prompt engineering?

Before explaining what prompt engineering is, let’s start with the atomic definition of prompt.

A prompt is a text input that guides the behaviour of an LLM to generate a text output. In the context of LLMs and LLMs-powered applications, we can distinguish two types of prompts:

*   The prompt that the user’s type when interacting with the LLM. For example, a prompt might be “Give me a detailed explanation of a proton”, or “Generate a workout plan to run a marathon”. Below you can see an example of a simple user prompt:

![](media/file36.png)![Figure 1: Example of user’s prompt.](media/file37.png)

Figure 1: Example of user’s prompt.

You will hear referring to this component simply as “prompt”, “query”, or “user’s input”.

*   The prompt that instruct the model to behave in a certain way regardless of the user’s query. This refers to the set of instructions in natural language that we provide the model with, so that it behaves in a certain way when interacting with end users. You can think about that as a sort of “backend” of your LLM, something that will be handled by the application developers rather than the final users.

In the following picture, you can see an example where we ask the model the same above questions, yet this time we add a system message to instruct the model to behave in a certain way:

![](media/file38.png)![](media/file39.png)![Figure 2: Example of a metaprompt.](media/file40.png)

Figure 2: Example of a metaprompt.

We refer to this type of prompt as “metaprompt” or “system message”.

Henceforth, prompt engineering is the process of designing effective prompts that elicit high-quality and relevant outputs from LLMs. Prompt engineering requires creativity, understanding of the LLM, and precision.

![Figure 3: Example of prompts engineering to specialize LLMs.](media/file41.png)

Figure 3: Example of prompts engineering to specialize LLMs.

Over the last years, prompt engineering became a brand new discipline itself, and this is a demonstration of the fact that, interacting with those models require a new set of skills and capabilities that did not exist before. The “art of prompting” has become a top skill when it comes to build Generative AI applications in enterprise scenarios; however, it can also be extremely useful for individual users that use ChatGPT or similar AI assistant in daily tasks, as it improves dramatically the quality and accuracy of results.

Over the next sections, we are going to see some examples of how to build efficient and robust prompts and metaprompts for your LLM applications. Note that all the techniques we are going to cover, can be embedded both in the user prompt and the metaprompt, depending on your specific requirements. To do that, I will leverage the OpenAI Playground, so that I can have greater flexibility to operate at both levels of prompting.

## Zero-, one-, and few-shot learning – typical of transformers models

In the previous chapters, we mentioned how LLMs typically come in a pre-trained format. They have been trained on a huge amount of data and have had their (billions of) parameters configured accordingly.

However, this doesn’t mean that those models can’t learn anymore. In *Chapter 2*, we saw that one way to customize an OpenAI model and make it more capable of addressing specific tasks is by fine-tuning.

> Fine-tuning is the process of adapting a pre-trained model to a new task. In fine-tuning, the parameters of the pre-trained model are altered, either by adjusting the existing parameters or by adding new parameters so that they fit the data for the new task. This is done by training the model on a smaller labeled dataset that is specific to the new task. The key idea behind fine-tuning is to leverage the knowledge learned from the pre-trained model and fine-tune it to the new task, rather than training a model from scratch.

Fine-tuning is a proper training process that requires a training dataset, compute power, and some training time (depending on the amount of data and compute instances).

That is why it is worth testing another method for our model to become more skilled in specific tasks: **shot learning**: The idea is to let the model learn from simple examples rather than the entire dataset. Those examples are samples of the way we would like the model to respond so that the model not only learns the content but also the format, style, and taxonomy to use in its response.

Furthermore, shot learning occurs directly via the prompt (as we will see in the following scenarios), so the whole experience is less time-consuming and easier to perform.

The number of examples provided determines the level of shot learning we are referring to. In other words, we refer to zero-shot if no example is provided, one-shot if one example is provided, and few-shot if more than 2-3 examples are provided.

Let’s focus on each of those scenarios:

*   **Zero-shot learning**. In this type of learning, the model is asked to perform a task for which it has not seen any training examples. The model must rely on prior knowledge or general information about the task to complete the task. For example, a zero-shot learning approach could be that of asking the model to generate a description, as defined in my prompt:

    ![Figure 4.3 – Example of zero-shot learning](media/file42.png)

    Figure 4.3 – Example of zero-shot learning

*   **One-shot learning**: In this type of learning, the model is given a single example of each new task it is asked to perform. The model must use its prior knowledge to generalize from this single example to perform the task. If we consider the preceding example, I could provide my model with a prompt-completion example before asking it to generate a new one:

![Figure 4.4 – Example of one-shot learning](media/file43.png)

Figure 4.4 – Example of one-shot learning

```
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
```

Note that the way I provided an example was similar to the structure used for fine-tuning:

*   **Few-shot learning**: In this type of learning, the model is given a small number of examples (typically between 2 and 5) of each new task it is asked to perform. The model must use its prior knowledge to generalize from these examples to perform the task. Let’s continue with our example and provide the model with further examples:

![Figure 4.5 – Example of few-shot learning with three examples](media/file44.png)

Figure 4.5 – Example of few-shot learning with three examples

The nice thing about few-shot learning is that you can also control model output in terms of how it is presented. You can also provide your model with a template of the way you would like your output to look. For example, consider the following tweet classifier:

![Figure 4.6 – Few-shot learning for a tweets classifier. This is a modified version of the original script from https://learn.microsoft.com/en-us/azure/cognitive-services/openai/how-to/completions](media/file45.png)

Figure 4.6 – Few-shot learning for a tweets classifier. This is a modified version of the original script from https://learn.microsoft.com/en-us/azure/cognitive-services/openai/how-to/completions

Let’s examine the preceding figure. First, I provided ChatGPT with some examples of labeled tweets. Then, I provided the same tweets but in a different data format (list format), as well as the labels in the same format. Finally, in list format, I provided unlabeled tweets so that the model returns a list of labels.

Shot-learning possibilities are limitless– it’s only a matter of testing and a little bit of patience in finding the proper prompt design.

As mentioned previously, it is important to remember that these forms of learning are different from traditional supervised learning, as well as fine-tuning. In few-shot learning, the goal is to enable the model to learn from very few examples, and to generalize from those examples to new tasks.

Now that we’ve learned how to let OpenAI models learn from examples, let’s focus on how to properly define our prompt to make the model’s response as accurate as possible.

## Principles of prompt engineering

Traditionally, in the context of computing and data processing, we often use the expression “garbage in, garbage out”, meaning that the quality of output is determined by the quality of the input. If incorrect or poor-quality data (garbage) is entered into a system, the output will also be flawed or nonsensical (garbage).

When it comes to prompting, the story is similar: if we want accurate and relevant results from our LLMs, we need to provide high-quality input. However, building good prompts is not just about the quality of the response. In fact, we can construct good prompts to:

*   Maximize the relevancy of LLM’s responses
*   Specify the type formatting and style of responses
*   Provide conversational context
*   Reduce inner biases and improve fairness and inclusivity
*   Reduce hallucination

> In the context of large language models (LLMs), "hallucination" refers to the generation of text or responses that are factually incorrect, nonsensical, or not grounded in the training data. This occurs when an LLM produces confident-sounding but erroneous or fabricated information. Hallucinations can arise due to the probabilistic nature of these models, which may predict the next word or phrase based on patterns rather than verified facts.

Let’s see some basic techniques to achieve these results.

### Clear Instructions

The principle of giving clear instructions is to provide the model with enough information and guidance to perform the task correctly and efficiently. Clear instructions should include the following elements:

*   The goal or objective of the task, such as “write a poem” or “summarize an article”.
*   The format or structure of the expected output, such as “use four lines with rhyming words” or “use bullet points with no more than 10 words each”.
*   The constraints or limitations of the task, such as “do not use any profanity” or “do not copy any text from the source”.
*   The context or background of the task, such as “the poem is about autumn” or “the article is from a scientific journal”.

Let’s say, for example, that we want our model to fetch any kind of instructions from text and return to us a tutorial in a bullet list. Plus, if there are no instructions in the provided text, the model should inform us about that.

To try so, let’s leverage the OpenAI Playground under the chat section, so that we can provide both the metaprompt and the prompt.

For this scenario, I’ll set the following metaprompt:

```
You are an AI assistant that helps human by generating tutorials given a text.
You will be provided with a text. If the text contains any kind of istructions on how to proceed with something, generate a tutorial in a bullet list.
Otherwise, inform the user that the text does not contain any instructions.
```

And this will be our user prompt:

```
To prepare the known sauce from Genova, Italy, you can start by toasting the pine nuts to then coarsely
chop them in a kitchen mortar together with basil and garlic. Then, add half of the oil in the kitchen mortar and season with salt and pepper.
Finally, transfer the pesto to a bowl and stir in the grated Parmesan cheese.
```

Let’s see how it works:

![Figure 4: Example of clear instructions in the metaprompt.](media/file46.png)

Figure 4: Example of clear instructions in the metaprompt.

Note that, if we pass the model another text which does not contain any instructions, it will be able to respond as we instructed it:

![Figure 5: Example of chat model following instructions.](media/file47.png)

Figure 5: Example of chat model following instructions.

By giving clear instructions, you can help the model understand what you want it to do and how you want it to do it. This can improve the quality and relevance of the model’s output, and reduce the need for further revisions or corrections.

However, sometimes there are scenarios where clarity is not enough. We might need to infer the way of thinking of our LLM to make it more robust with respect to its task. In next section, we are going to examine one of this techniques, very useful in case of complex tasks to accomplish.

### Split complex tasks into subtasks

Prompt engineering is a technique that involves designing effective inputs for large language models (LLMs) to perform various tasks. Sometimes, the tasks are too complex or ambiguous for a single prompt to handle, and it is better to split them into simpler subtasks that can be solved by different prompts.

Here are some examples of splitting complex tasks into subtasks:

*   Text summarization: A complex task that involves generating a concise and accurate summary of a long text. This task can be split into subtasks such as:
    *   Extracting the main points or keywords from the text.
    *   Rewriting the main points or keywords in a coherent and fluent way.
    *   Trimming the summary to fit a desired length or format.
*   Machine translation: A complex task that involves translating a text from one language to another. This task can be split into subtasks such as:
    *   Detecting the source language of the text.
    *   Converting the text into an intermediate representation that preserves the meaning and structure of the original text.
    *   Generating the text in the target language from the intermediate representation.
*   Poem generation: A creative task that involves producing a poem that follows a certain style, theme, or mood. This task can be split into subtasks such as:
    *   Choosing a poetic form (such as sonnet, haiku, limerick, etc.) and a rhyme scheme (such as ABAB, AABB, ABCB, etc.) for the poem.
    *   Generating a title and a topic for the poem based on the user’s input or preference.
    *   Generating the lines or verses of the poem that match the chosen form, rhyme scheme, and topic.
    *   Refining and polishing the poem to ensure coherence, fluency, and originality.
*   Code generation: A technical task that involves producing a code snippet that performs a specific function or task. This task can be split into subtasks such as:
    *   Choosing a programming language (such as Python, Java, C++, etc.) and a framework or library (such as TensorFlow, PyTorch, React, etc.) for the code.
    *   Generating a function name and a list of parameters and return values for the code based on the user’s input or specification.
    *   Generating the body of the function that implements the logic and functionality of the code.
    *   Adding comments and documentation to explain the code and its usage.
*   Let’s consider the following example. We will provide the model with a short article and ask it to summarize it following these instructions:

```
You are an AI assistant that summarize articles.
To complete this task, do the following subtasks:
Read the provided article context comprehensively and identified the main topic and key points
Generated a paragraph summary of the current article context that captures the essential information and conveys the main idea
Print each step of the process.
```

This is the short article I will provide:

```
Large Language Models (LLMs), a subset of artificial intelligence, have revolutionized the field of natural language processing by demonstrating an unprecedented ability to understand and generate human-like text. These models are trained on vast datasets comprising diverse linguistic inputs, enabling them to produce coherent and contextually relevant responses across a wide range of topics. By leveraging architectures such as transformers, LLMs like GPT-3 and its successors can complete text, answer questions, perform translations, and even engage in complex dialogue. Their applications span from automated customer support and content creation to advanced research and education tools. Despite their incredible capabilities, LLMs also pose challenges, including the potential for biases inherent in training data and the risk of generating misleading or false information. As the development of LLMs continues to advance, ongoing efforts in ethical AI research and deployment strategies are crucial to harness their benefits responsibly and effectively.
```

Let’s see how the model works:

![Figure 6: Example of OpenAI GPT-4o splitting a task into subtasks to generate a summary.](media/file48.png)

Figure 6: Example of OpenAI GPT-4o splitting a task into subtasks to generate a summary.

Splitting complex tasks into easier sub-tasks is a powerful technique, nevertheless it does not address one of the main risks of LLM-generated content, that is, having a wrong output. In next two sections, we are going to see some techniques that are mainly aimed at addressing this risk.

### Ask for justification

LLMs are built in such a way that they predict the next token based on the previous ones without looking back at their generations. This might lead the model to output wrong content to the user, yet in a very convincing way. If the LLM-powered application does not provide specific reference to that response, it might be hard to validate the ground truth behind it.

Henceforth, specifying in the prompt to support the LLM’s answer with some reflections and justification could prompt the model to recover from its actions.

Furthermore, asking for justification might be useful also in case of answers that are right, but simply we don’t know the LLM thought process behind it. For example, let’s say we want our LLM to solve riddles. To do so, we can instruct it as follows:

```
You are an AI assistant specialized in solving riddles.
Given a riddle, solve it the best you can.
Provide a clear justification of your answer and the reasoning behind it.
```

As you can see, I’ve specified in the metaprompt to the LLM to justify its answer and also socializing its reasoning. Let’s test it with the following riddle:

```
What has a face and two hands, but no arms or legs?
```

Output:

![Figure 7: Example of OpenAI’s GPT-4o providing justification after solving a riddle.](media/file49.png)

Figure 7: Example of OpenAI’s GPT-4o providing justification after solving a riddle.

Justifications are a great tool to make your model more reliable and robust, since they “force” it to re-think about its output, as well as providing us with a view of how the reasoning was set to solve the problem.

With a similar approach, we could also intervene at different prompts level to improve our LLM’s performance. For example, we might discover that the model is systematically tackling a mathematical problem in the wrong way, henceforth we might want to suggest the right approach directly at the metaprompt level. Another example might be that of asking the model to generate multiple outputs – along with their justifications – to evaluate different reasoning techniques and prompt the best one in the metaprompt.

In next section, we are going to focus on one of this example, more specifically the possibility of generating multiple outputs and then picking the most likely one.

### Generate many outputs, then use the model to pick the best one

As we saw in the previous section, LLMs are built in such a way that they predict the next token based on the previous ones without looking back at their generations. If this is the case, if one sampled token is the wrong one (in other words, if the model is unlucky), the LLM will keep generating wrong tokens and, henceforth, wrong content. Now, the bad news is that, unlike humans, LLMs cannot recover from errors on their own. It means that, if we ask them, they aknowledge the error, but we need to explicitly prompt them to think about that.

One way to overcome this limitation is that of broadening the space of probabilities of picking the right token. Rather than generating just one response, we can prompt the model to generate multiple response, and then picking the one which is most suitable for the user’s query. This splits the job into two sub-jobs for our LLM:

1.  Generating multiple responses to user’s query;
2.  Comparing those responses and picking the best one, according to some criteria we can specify in the metaprompt.

Let’s see an example, following up with the riddles examined in the previous section.

```
You are an AI assistant specialized in solving riddles.
Given a riddle, you have to generate three answers to the riddle.
For each answer, be specific about the reasoning you made.
Then, among the three answer, select the one which is most plausible given the riddle.
```

In this case, I’ve prompted the model to generate three answers to the riddle, then to give me the most likely, justifying why. Let’s see the result:

![Figure 8: Example of GPT-4o generating 3 plausible answers and picking the most likely one, providing justification.](media/file50.png)

Figure 8: Example of GPT-4o generating 3 plausible answers and picking the most likely one, providing justification.

As aforementioned, forcing the model to tackle a problem with different approaches is a way to collect multiple samples of reasonings, which might serve as further instructions in the metaprompt. For example, if we want the model to always propose us something which is not the most straightforward solution to a problem – in other words, if we want it to “think differently” – we might force it to solve a problem in N ways and then use the most creative reasoning as framework in the metaprompt.

The last element we are going to examine is the overall structure we want to give to our metaprompt. In fact, in previous examples, we saw a sample system message with some statements and instructions. In next section, we will see how the order and “strength” of those statements and instructions is not an invariant.

### Use delimiters

The last principle to be covered is related to the format we want to give to our meta prompt. This helps our LLM to better understand its intents as well as making relations among sections and paragraphs.

To achieve this, we can use delimiters within our prompt. A delimiter can by any sequence of characters or symbols that is clearly mapping a schema rather than a concept. For example, we can consider delimiters the following sequences:

*   >>>>
*   ====
*   ------
*   ####
*   ` ` ` ` `

And so on. Let’s consider, for example, a meta prompt that aims at instructing the model to translate user’s tasks into Python code, providing also an example to do so.

```
You are a Python expert that produces python code as per user's request.
===>START EXAMPLE
---User Query---
Give me a function to print a string of text.
---User Output---
Below you can find the described function:
```def my_print(text):
     #returning the printed text
     return print(text)
```
<===END EXAMPLE
```

Let’s see how it works:

![Figure 9: Sample output of a model using delimiters in the system message.](media/file51.png)

Figure 9: Sample output of a model using delimiters in the system message.

As you can see, it also printed the code in backsticks as showed within the system message.

All the principles examined up to this point are general rules that can make your LLM-powered application more robust. In next sections, we are going to see some advanced techniques for prompt engineering that address the way the model reason and think to the answer, before providing it to the final user.

## Advanced techniques

In previous sections, we covered some basics techniques of prompt engineering. Those techniques should be kept in mind regardless of the type of application your are developing, since are general best practices that improve your LLM performance anyway.

On the other hand, there are some advanced techniques which might be implemented for specific scenarios, that we are going to cover in the upcoming sections.

### Chain of Thoughts

Introduced in the paper “Chain-of-Thought Prompting Elicits Reasoning in Large Language Models” by Wei et al., Chain of thought (CoT) is a technique that enables complex reasoning capabilities through intermediate reasoning steps. It also encourages the model to explain its reasoning, “forcing” it not to be too fact and risking to give the wrong response (as we saw in previous sections).

Let’s say that we want to prompt our LLM to solve first-degree equations. To do so, we are going to provide it with a generic reasoning list as a metaprompt:

```
To solve a generic first-degree equation, follow these steps:
1\. **Identify the Equation:** Start by identifying the equation you want to solve. It should be in the form of "ax + b = c," where 'a' is the coefficient of the variable, 'x' is the variable, 'b' is a constant, and 'c' is another constant.
2\. **Isolate the Variable:** Your goal is to isolate the variable 'x' on one side of the equation. To do this, perform the following steps:

   a. **Add or Subtract Constants:** Add or subtract 'b' from both sides of the equation to move constants to one side.

   b. **Divide by the Coefficient:** Divide both sides by 'a' to isolate 'x'. If 'a' is zero, the equation may not have a unique solution.
3\. **Simplify:** Simplify both sides of the equation as much as possible.
4\. **Solve for 'x':** Once 'x' is isolated on one side, you have the solution. It will be in the form of 'x = value.'
5\. **Check Your Solution:** Plug the found value of 'x' back into the original equation to ensure it satisfies the equation. If it does, you've found the correct solution.
6\. **Express the Solution:** Write down the solution in a clear and concise form.
7\. **Consider Special Cases:** Be aware of special cases where there may be no solution or infinitely many solutions, especially if 'a' equals zero.
Equation:
```

Let’s see how it works:

![Figure 10: Output of the model solving an equation with CoT approach.](media/file52.png)

Figure 10: Output of the model solving an equation with CoT approach.

Note that you can also combine combine it with few-shot prompting to get better results on more complex tasks that require reasoning before responding.

With CoT, we are prompting the model to generate intermediate reasoning steps. This is also a component of another reasoning technique that we are going to examine in next section.

### ReAct

Introduced in the paper “ReAct: Synergizing Reasoning and Acting in Language Models” by Yao et al., ReAct (Reason and Act) is a general paradigm that combines reasoning and acting with large language models. ReAct prompts the language model to generate verbal reasoning traces and actions for a task, and also receives observations from external sources such as web search or databases. This allows the language model to perform dynamic reasoning, and quickly adapt its acting plan based on external information. For example, you can prompt the language model to answer a question by first reasoning about the question, then performing an action to send a query to the web, then receiving an observation from the search results, and then continuing with this thought, action, observation loop until it reaches a conclusion.

The difference between chain of thought and ReAct approaches is that chain of thought prompts the language model to generate intermediate reasoning steps for a task, while ReAct prompts the language model to generate intermediate reasoning steps, actions, and observations for a task.

Note that the “action” phase is generally related to the possibility for our LLM to interact with external tools, such as the web search. However, in the following example we won’t use tools, but rather refer to “action” any task we ask the model to do for us.

This is how the ReAct metaprompt might look like:

```
Answer the following questions as best you can.
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
```

Let’s see how does it work with a simple user’s query:

![Figure 11: Example of ReAct metaprompting](media/file53.png)

Figure 11: Example of ReAct metaprompting

As you can see, in this scenario the model skipped the Action Input and the following observation, since we didn’t provide it with any tool to be used.

This is a great example of how prompting a model to think step by step and explicit each step of the reasoning makes it “wiser” and cautious before answering. It is also a great technique to prevent hallucination.

Overall, prompt engineering is a powerful discipline, still in its emerging phase yet already widely adopted within LLM-powered applications. In next Chapters, we are going to see concrete applications of this techniques.

## Avoiding the risk of hidden bias and taking into account ethical considerations in ChatGPT

ChatGPT has been provided with the Moderator API so that it cannot engage in conversations that might be unsafe. The Moderator API is a classification model performed by a GPT model based on the following classes: violence, self-harm, hate, harassment, and sex. For this, OpenAI uses anonymized data and synthetic data (in zero-shot form) to create synthetic data.

The Moderation API is based on a more sophisticated version of the content filter model available among OpenAI APIs. We discussed this model in *Chapter 1*, where we saw how it is very conservative toward false positives rather than false negatives.

However, there is something we can refer to as **hidden bias**, which derives directly from the knowledge base the model has been trained on. For example, concerning the main chunk of training data of GPT-3, known as the **Common Crawl**, experts believe that it was written mainly by white males from Western countries. If this is the case, we are already facing a hidden bias of the model, which will inevitably mimic a limited and unrepresentative category of human beings.

In their paper, *Languages Models are Few-Shots Learners*, OpenAI’s researchers Tom Brown et al. ([https://arxiv.org/pdf/2005.1416](https://arxiv.org/pdf/2005.1416)) created an experimental setup to investigate racial bias in GPT-3\. The model was prompted with phrases containing racial categories and 800 samples were generated for each category. The sentiment of the generated text was measured using Senti WordNet based on word co-occurrences on a scale ranging from -100 to 100 (with positive scores indicating positive words, and vice versa).

The results showed that the sentiment associated with each racial category varied across different models, with Asian consistently having a high sentiment and Black consistently having a low sentiment. The authors caution that the results reflect the experimental setup and that socio-historical factors may influence the sentiment associated with different demographics. The study highlights the need for a more sophisticated analysis of the relationship between sentiment, entities, and input data:

![Figure 4.14 – Racial sentiment across models](media/file54.png)

Figure 4.14 – Racial sentiment across models

This hidden bias could generate harmful responses not in line with responsible AI principles.

However, it is worth noticing how ChatGPT, as well as all OpenAI models, are subject to continuous improvements. This is also consistent with OpenAI’s AI **alignment** ([https://openai.com/ alignment/](https://openai.com/)), whose research focuses on training AI systems to be helpful, truthful, and safe.

For example, if we ask ChatGPT to formulate guesses based on people’s gender and race, it will not accommodate the exact request, but rather provide us with hypothetical function as well as a huge disclaimer:

![Figure 4.15 – Example of GPT-4o improving over time since it gives an unbiased response](media/file55.png)

Figure 4.15 – Example of GPT-4o improving over time since it gives an unbiased response

Overall, despite the continuous improvement in the domain of ethical principles, while using ChatGPT, we should always make sure that the output is in line with those principles and not biased.

The concepts of bias and ethics within ChatGPT and OpenAI models have a wider collocation within the whole topic of responsible AI, which we are going to focus on in the last chapter of this book.

## Summary

In this chapter, we have dived deeper into the concept of prompt design and engineering since it’s the most powerful way to control the output of ChatGPT and LLMs in general. We learned how to leverage different levels of shot learning to make LLMs more tailored toward our objectives.

We started with an introduction to the concept of prompt engineering and why it is important, to then move towards the basic principles – including clear instructions, asking for justification etc.

Then, we moved towards more advanced techniques, which are meant to shape the reasoning approach of our LLM: few-shot learning, CoT and ReAct.

Prompt engineering is an emerging discipline which is paving the way for a new category of applications, infused with LLMs. In next chapters, we will see those techniques in action building real-world applications using LLMs.

Starting from the next chapter, we will dive deeper into different domains where ChatGPT can boost productivity and have a disruptive impact on the way we work today.

## References

The following are the references for this chapter:

*   [https://arxiv.org/abs/2005.14165](https://arxiv.org/abs/2005.14165)
*   [https://dl.acm.org/doi/10.1145/3442188.3445922](https://dl.acm.org/doi/10.1145/3442188.3445922)
*   [https://openai.com/alignment/](https://openai.com/alignment/)
*   [https://twitter.com/spiantado/status/1599462375887114240?ref](https://twitter.com/spiantado/status/1599462375887114240?ref)
*   ReAct approach. [https://arxiv.org/abs/2210.03629](https://arxiv.org/abs/2210.03629)
*   Chain of Thoughts approach. [https://arxiv.org/abs/2201.11903](https://arxiv.org/abs/2201.11903)
*   What is prompt engineering. [https://www.mckinsey.com/featured-insights/mckinsey-explainers/what-is-prompt-engineering](https://www.mckinsey.com/featured-insights/mckinsey-explainers/what-is-prompt-engineering)
*   Prompt engineering techniques. [https://blog.mrsharm.com/prompt-engineering-guide/](https://blog.mrsharm.com/prompt-engineering-guide/)
*   Prompt engineering principles. [https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/advanced-prompt-engineering?pivots=programming-language-chat-completions](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/advanced-prompt-engineering?pivots=programming-language-chat-completions)

# 4 Boosting Day-to-Day Productivity with ChatGPT

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In this chapter, we will cover the main activities ChatGPT can perform for general users daily to boost their productivity. The chapter will focus on concrete examples of writing assistance, decision- making, information retrieval, and so on, with suggestions and prompts so that you can implement them on your own.

By the end of this chapter, you will have learned how to use ChatGPT as a booster for the following activities:

*   Daily activities such as organizing agendas, meal-prepping, grocery shopping, and so on
*   Generating brand-new text content
*   Improving your writing skills and adapting the same content to different audiences
*   Retrieving documentation and information for research and competitive intelligence

## Technical requirements

For this chapter, you will require a ChatGPT account.

## ChatGPT as a daily assistant

ChatGPT can serve as a valuable daily assistant, helping you manage your tasks and streamline your workflow. It can optimize your daily routine by providing personalized assistance, thus saving you time and enhancing your efficiency.

Let’s start with a general suggestion on how I could make my day more productive:

![Figure 5.1 – An example of ChatGPT generating a productive routine](media/file57.png)

Figure 5.1 – An example of ChatGPT generating a productive routine

Another interesting usage of ChatGPT in organizing my week is that I can use it as a meal prep assistant. Note that here I also asked for a specific table formatting:

![Figure 5.3 – Meal prep for my working week generated by ChatGPT](media/file58.png)

Figure 5.3 – Meal prep for my working week generated by ChatGPT

Alongside meal prepping, ChatGPT can also generate a workout plan according to my requirements:

![Figure 5.4 – Workout plan generated by ChatGPT](media/file59.png)

Figure 5.4 – Workout plan generated by ChatGPT

ChatGPT can also be a loyal and disciplined study partner. It could help you, for example, in summarizing long papers so that you gain a first overview of the discussed topic or help you prepare for exams.

Namely, let’s say you are preparing for mathematics exams using the university book titled *Mathematics for Economics and Business*, by Lorenzo Peccati et al. Before diving deeper into each chapter, you might want to get an overview of the content and main topics that are discussed, as well as whether you need further prerequisites and – mostly importantly if I’m preparing for an exam – how long it will take to study it. You can ask ChatGPT for this:

![Figure 5.5 – ChatGPT providing an overview of a university book](media/file60.png)

Figure 5.5 – ChatGPT providing an overview of a university book

When it comes to questions about specific assets or personal information, we are increasing the risk of ChatGPT’s hallucination. In fact, in the previous example, the model was able to answer since the synopsis of the book was clearly part of the training set, yet the model didn’t have full visibility on the book, as it’s not available on the web for free. If this is the case, it might be a good practice to specify the model to navigate the web to answer these specific questions. Let’s see an example of it:

![Figure 1: Example of ChatGPT invoking the web plug-in.](media/file61.png)

Figure 1: Example of ChatGPT invoking the web plug-in.

As you can see, now ChatGPT invoked the web plug-in and searched over 5 files, also providing the links it navigated through.

You can also ask ChatGPT to ask you some questions about the material you have just studied:

![Figure 5.6 – Example of ChatGPT acting as a professor](media/file62.png)

Figure 5.6 – Example of ChatGPT acting as a professor

> The technique of *Act as*… is a great example of efficient prompting techniques, and it can be listed among the examples described in *Chapter 3*.

Now, let’s look at some more examples of using ChatGPT for more specific tasks, including text generation, writing assistance, and information retrieval.

## Generating text

As a language model, ChatGPT is particularly suited for generating text based on users’ instructions. For example, you could ask ChatGPT to generate emails, drafts, or templates that target a specific audience:

![Figure 5.7 – Example of an email generated by ChatGPT](media/file63.png)

Figure 5.7 – Example of an email generated by ChatGPT

Another example might be asking ChatGPT to create a pitch structure for a presentation you have to prepare (I’ll put here only the first slide as an example):

![Figure 5.8 – Slideshow agenda and structure generated by ChatGPT](media/file64.png)

Figure 5.8 – Slideshow agenda and structure generated by ChatGPT

You can also generate blog posts or articles about trending topics this way. Here is an example:

![Figure 5.9 – Example of a blog post with relevant tags and SEO keywords generated by ChatGPT](media/file65.png)

Figure 5.9 – Example of a blog post with relevant tags and SEO keywords generated by ChatGPT

We can even get ChatGPT to reduce the size of the post to make it fit for a tweet. Here is how we can do this:

![Figure 5.10 – ChatGPT shrinks an article into a Twitter post](media/file66.png)

Figure 5.10 – ChatGPT shrinks an article into a Twitter post

Finally, ChatGPT can also generate video or theatre scripts, including the scenography and the suggested editing. The following figure shows an example of a theatre dialog including scenography and actors:

![Figure 5.11 – Theatre dialog with scenography generated by ChatGPT](media/file67.png)

Figure 5.11 – Theatre dialog with scenography generated by ChatGPT

I only provided one scene out of the four generated by ChatGPT, to keep you in suspense regarding the ending…

Overall, whenever new content needs to be generated from scratch, ChatGPT does a very nice job of providing a first draft, which could act as the starting point for further refinements.

However, ChatGPT can also support pre-existing content by providing writing assistance and translation, as we will see in the next section.

## Improving writing skills and translation

Sometimes, rather than generating new content, you might want to revisit an existing piece of text. It this be for style improvement purposes, audience changes, language translation, and so on.

Let’s look at some examples. Imagine that I drafted an email to invite a customer of mine to a webinar. I wrote two short sentences. Here, I want ChatGPT to improve the form and style of this email since the target audience will be executive level:

![Figure 5.12 – Example of an email revisited by ChatGPT to target an executive audience](media/file68.png)

Figure 5.12 – Example of an email revisited by ChatGPT to target an executive audience

Now, let’s ask the same thing but with a different target audience:

![Figure 5.13 – Example of the same email with a different audience, generated by ChatGPT](media/file69.png)

Figure 5.13 – Example of the same email with a different audience, generated by ChatGPT

ChatGPT can also give you some feedback about your writing style and structure.

Imagine, for example, that you wrote an introduction for an essay titled *The History of Natural Language Processing* and you want some feedback about the writing style and its consistency with the title:

![Figure 5.15 – Example of ChatGPT giving feedback on an introduction for an essay](media/file70.png)

Figure 5.15 – Example of ChatGPT giving feedback on an introduction for an essay

As you can see, not only ChatGPT provided me with feedback and tips for the overall essay, but also it gave me a revised introduction which incorporates all of them.

Let’s also ask ChatGPT to be more specific regarding its suggestion of “Refining Examples”:

![Figure 5.16 – Example of ChatGPT elaborating on something it mentioned](media/file71.png)

Figure 5.16 – Example of ChatGPT elaborating on something it mentioned

I’m also interested in knowing whether my introduction was consistent with the title or whether I’m taking the wrong direction:

![Figure 5.17 – ChatGPT provides feedback about the consistency of the introduction with the title](media/file72.png)

Figure 5.17 – ChatGPT provides feedback about the consistency of the introduction with the title

I was impressed by this last one. ChatGPT was smart enough to see that there was no specific mention of the history of NLP in my introduction. Nevertheless, it sets up the expectation about that topic to be treated later on. This means that ChatGPT also has expertise in terms of how an essay should be structured and it was very precise in applying its judgment, knowing that it was just an introduction.

Let’s now unveil the last ChatGPT skill of the Chapter. In fact, ChatGPT is also an excellent tool for translation. It knows at least 95 languages (if you have doubts about whether your language is supported, you can always ask ChatGPT directly). Here, however, there is a consideration that might arise: what is the added value of ChatGPT for translation when we already have cutting-edge tools such as Google Translate?

To answer this question, we have to consider some key differentiators and how we can leverage ChatGPT’s embedded translations capabilities:

*   ChatGPT can capture the intent. This means that you could also bypass the translation phase since it is something that ChatGPT can do in the backend. For example, if you write a prompt to produce a social media post in French, you could write that prompt in any language you want – ChatGPT will automatically detect it (without the need to specify it in advance) and understand your intent:

    ![Figure 5.18 – Example of ChatGPT generating an output in a language that is different from the input](media/file73.png)

    Figure 5.18 – Example of ChatGPT generating an output in a language that is different from the input

*   ChatGPT can capture the more refined meaning of slang or idioms. This allows for a translation that is not literal so that it can preserve the underlying meaning. Namely, let’s consider the British expression It’s not my cup of tea, which indicates that something is not to one’s liking or preference. Let’s ask both ChatGPT and Google Translate to translate it into Italian:

![Figure 5.19 – Comparison between ChatGPT and Google Translate while translating from English into Italian](media/file74.png)

Figure 5.19 – Comparison between ChatGPT and Google Translate while translating from English into Italian

As you can see, ChatGPT can provide several Italian idioms that are equivalent to the original one, also in their slang format. On the other hand, Google Translate performed a literal translation, leaving behind the real meaning of the idiom.

*   As with any other task, you can always provide context to ChatGPT. So, if you want your translation to have a specific slang or style, you can always specify it in the prompt. Or, even funnier, you can ask ChatGPT to translate your prompt with a sarcastic touch:

![Figure 5.20 – Example of ChatGPT translating a prompt with a sarcastic touch. The original content of the prompt was taken from OpenAI’s Wikipedia page: https://it.wikipedia.org/wiki/OpenAI](media/file75.png)

Figure 5.20 – Example of ChatGPT translating a prompt with a sarcastic touch. The original content of the prompt was taken from OpenAI’s Wikipedia page: https://it.wikipedia.org/wiki/OpenAI

All these scenarios highlight one of the key killing features of ChatGPT and OpenAI models in general. Since they represent the manifestation of what OpenAI defined as **Artificial General Intelligence** (**AGI**), they are not meant to be specialized (that is, constrained) on a single task. On the contrary, they are meant to serve multiple scenarios dynamically so that you can address a wide range of use cases with a single model.

In conclusion, ChatGPT is able not only to generate new text but also to manipulate existing material to tailor it to your needs. It has also proven to be very precise at translating between languages, also keeping the jargon and language-specific expressions intact.

In the next section, we will see how ChatGPT can assist us in retrieving information and competitive intelligence.

## Quick information retrieval and competitive intelligence

Information retrieval and competitive intelligence are yet other fields where ChatGPT is a game-changer. The very first example of how ChatGPT can retrieve information is the most popular way it is used right now: as a search engine. Every time we ask ChatGPT something, it can retrieve information from its knowledge base and reframe it in an original way.

One example involves asking ChatGPT to provide a quick summary or review of a book we might be interested in reading:

![Figure 5.21 – Example of ChatGPT providing a summary and review of a book](media/file76.png)

Figure 5.21 – Example of ChatGPT providing a summary and review of a book

Alternatively, we could ask for some suggestions for a new book we wish to read based on our preferences:

![Figure 5.22 – Example of ChatGPT recommending a list of books, given my preferences](media/file77.png)

Figure 5.22 – Example of ChatGPT recommending a list of books, given my preferences

Furthermore, if we design the prompt with more specific information, ChatGPT can serve as a tool for pointing us toward the right references for our research or studies.

Namely, you might want to quickly retrieve some background references about a topic you want to learn more about – for example, feedforward neural networks. Something you might ask ChatGPT is to point you to some websites or papers where this topic is widely treated:

![Figure 5.23 – Example of ChatGPT listing relevant references](media/file78.png)

Figure 5.23 – Example of ChatGPT listing relevant references

As you can see, ChatGPT was able to provide me with relevant references to start studying the topic. However, it could go even further in terms of competitive intelligence.

Let’s consider I’m writing a book titled Introduction to *Convolutional Neural Networks – an Implementation with Python*. I want to do some research about the potential competitors in the market. The first thing I want to investigate is whether there are already some competitive titles around, so I can ask ChatGPT to generate a list of existing books with the same content:

![Figure 5.24 – Example of ChatGPT providing a list of competitive books](media/file79.png)

Figure 5.24 – Example of ChatGPT providing a list of competitive books

You can also ask for feedback in terms of the saturation of the market you want to publish in:

![Figure 5.25 – ChatGPT advising about how to be competitive in the market](media/file80.png)

Figure 5.25 – ChatGPT advising about how to be competitive in the market

Finally, let’s ask ChatGPT to be more precise about what I should do to be competitive in the market where I will operate:

![Figure 5.26 – Example of how ChatGPT can suggest improvements regarding your book content to make it stand out](media/file81.png)

Figure 5.26 – Example of how ChatGPT can suggest improvements regarding your book content to make it stand out

ChatGPT was pretty good at listing some good tips to make my book unique.

Overall, ChatGPT can be a valuable assistant for information retrieval and competitive intelligence. However, it is important to remember the knowledge base cut-off is 2021: this means that, whenever we need to retrieve real-time information, or while making a competitive market analysis for today, we might not be able to rely on ChatGPT.

Nevertheless, this tool still provides excellent suggestions and best practices that can be applied, regardless of the knowledge base cut-off.

## Summary

All the examples we saw in this chapter were modest representations of what you can achieve with ChatGPT to boost your productivity. These small hacks can greatly assist you with activities that might be repetitive (answering emails with a similar template rather than writing a daily routine) or onerous (such as searching for background documentation or competitive intelligence).

In the next chapter, we are going to dive deeper into three main domains where ChatGPT is changing the game – development, marketing, and research.

# 5 Developing the Future with ChatGPT

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In this chapter, we will discuss how developers can leverage ChatGPT. The chapter focuses on the main use cases ChatGPT addresses in the domain of developers, including code review and optimization, documentation generation, and code generation. The chapter will provide examples and enable you to try the prompts on your own.

After a general introduction about the reasons why developers should leverage ChatGPT as a daily assistant, we will focus on ChatGPT and how it can do the following:

*   Why ChatGPT for developers?
*   Generate, optimize, and debug code
*   Generate code-related documentation and debug your code
*   Explain **machine learning** (**ML**) models to help data scientists and business users with model interpretability
*   Translate different programming languages

By the end of this chapter, you will be able to leverage ChatGPT for coding activities and use it as an assistant for your coding productivity.

## Why ChatGPT for developers?

Personally, I believe that one of the most mind-blowing capabilities of ChatGPT is that of dealing with code. Of any type. We’ve already seen in previous chapters some examples of ChatGPT generating Python code. However, ChatGPT capabilities for developers go way beyond that example. It can be a daily assistant for code generation, explanation, and debugging.

Among the most popular languages, we can certainly mention Python, JavaScript, SQL, and C#. However, ChatGPT covers a wide range of languages, as disclosed by itself:

![Figure 6.1 – ChatGPT lists the programming languages it is able to understand and generate](media/file83.png)

Figure 6.1 – ChatGPT lists the programming languages it is able to understand and generate

Whether you are a backend/frontend developer, a data scientist, or a data engineer, whenever you work with a programming language, ChatGPT can be a game changer, and we will see how in the several examples in the next sections.

From the next section onward, we will dive deeper into concrete examples of what ChatGPT can achieve when working with code. We will see end-to-end use cases covering different domains so that we will get familiar with using ChatGPT as a code assistant.

## Generating, optimizing, and debugging code

The primary capability you should leverage is ChatGPT code generation. How many times have you been looking for a pre-built piece of code to start from? Generating the utils functions, sample datasets, SQL schemas, and so on? ChatGPT is able to generate code based on input in natural language:

![Figure 6.2 – Example of ChatGPT generating a Python function to write into CSV files](media/file84.png)

Figure 6.2 – Example of ChatGPT generating a Python function to write into CSV files

As you can see, not only was ChatGPT able to generate the function, but also it was able to explain what the function does, how to use it, and what to substitute with generic placeholders such as `my_folder`.

Now let’s raise the difficulty bar. If ChatGPT is capable of generating a Python function, could it generate an entire Video Game as well? Let’s try. What I want to do, is providing ChatGPT with an illustration of the type of game I want to develop, and ask it to replicate it with code. The following is an illustration of my desired game (can you guess the name?):

![Figure 1: Illustration of the game Pac-Man.](media/file85.jpg)

Figure 1: Illustration of the game Pac-Man.

Now let’s ask ChatGPT to reproduce it:

![Figure 2: Example of ChatGPT generating a HTML, CSS and JS code.](media/file86.png)

Figure 2: Example of ChatGPT generating a HTML, CSS and JS code.

As disclaimed by ChatGPT, the full game required a lot of code, however, let’s see how the generated code works insofar (to run the code, I used the online tool codepen.io):

![Figure 3: Pac-Man game generated by ChatGPT.](media/img_chapter05_image6.png)

Figure 3: Pac-Man game generated by ChatGPT.

As you can see, the draft product looks already similar to what I’m aiming for! This is an example on how Generative AI can help you overcoming the “curse” of starting from scratch: in fact, starting from a whitepaper can be sometimes blocking, while having a draft product to start from can not only speed up the overall process, but also stimulate creativity and improve the quality of the result.

ChatGPT can also be a great assistant for code optimization. In fact, it might save us some running time or compute power to make optimized scripts starting from our input. This capability might be compared, in the domain of natural language, to the writing assistance feature we saw in Chapter 5 in the Improving writing skills and translation section.

For example, imagine you want to create a list of odd numbers starting from another list. To achieve the result, you wrote the following Python script (for the purpose of this exercise, I will also track the execution time with the timeit and datetime libraries):

```
from timeit import default_timer as timer from datetime import timedelta
start = timer()
elements = list(range(1_000_000)) data = []
for el in elements: if not el % 2:
# if odd number data.append(el)
end = timer() print(timedelta(seconds=end-start))
```

Let’s see how long it takes to run:

![Figure 4: Speed of execution of a Python function.](media/file87.png)

Figure 4: Speed of execution of a Python function.

The execution time was 00.115022 seconds. What happens if we ask ChatGPT to optimize this script?

![Figure 6.6 – ChatGPT generating optimized alternatives to a Python script](media/file88.png)

Figure 6.6 – ChatGPT generating optimized alternatives to a Python script

ChatGPT provided me with two examples to achieve the same results with lower execution time.

Let’s test both of them in a Jupyter Notebook:

```
2023-03-25 11:27:10.270 Uncaught app exception Traceback (most recent call last):
File "C:\Users\vaalt\Anaconda3\lib\site-packages\streamlit\runtime\ scriptrunner\script_runner.py", line 565, in _run_script
exec(code, module. dict )
File "C:\Users\vaalt\OneDrive\Desktop\medium articles\llm.py", line 129, in <module>
user_input = get_text()
File "C:\Users\vaalt\OneDrive\Desktop\medium articles\llm.py", line 50, in get_text
input_text = st.text_input("You: ", st.session_state['input'], key='input', placeholder = 'Your AI assistant here! Ask me anything...', label_visibility = 'hidden')
File "C:\Users\vaalt\Anaconda3\lib\site-packages\streamlit\runtime\ metrics_util.py", line 311, in wrapped_func
result = non_optional_func(*args, **kwargs)
File "C:\Users\vaalt\Anaconda3\lib\site-packages\streamlit\elements\ text_widgets.py", line 174, in text_input
return self._text_input(
```

![Figure 6.7 – Speed of execution of two alternative functions generated by ChatGPT.](media/file89.png)

Figure 6.7 – Speed of execution of two alternative functions generated by ChatGPT.

As you can see, both methods lead to a great reduction in time, respectively of 44.30% and 20.68%.

On top of code generation and optimization, ChatGPT can also be leveraged for error explanation and debugging. Sometimes, errors are difficult to interpret; hence a natural language explanation can be useful to identify the problem and drive you toward the solution.

For example, while running a .py file from my command line, I get the following error:

```
File "C:\Users\vaalt\Anaconda3\lib\site-packages\streamlit\elements\ text_widgets.py", line 266, in _text_input
text_input_proto.value = widget_state.value
TypeError: [] has type list, but expected one of: bytes, Unicode
```

Let’s see whether ChatGPT is able to let me understand the nature of the error. To do so, I simply provide ChatGPT with the text of the error and ask it to give me an explanation.

![Figure 6.8 – ChatGPT explaining a Python error in natural language](media/file90.png)

Figure 6.8 – ChatGPT explaining a Python error in natural language

Finally, let’s imagine I wrote a function in Python that takes a string as input and returns the same string with an underscore after each letter.

In the preceding example, I was expecting to see the g_p_t_ result; however, it only returned t_

with this code:

![Figure 6.9 – Bugged Python function](media/file91.png)

Figure 6.9 – Bugged Python function

Let’s ask ChatGPT to debug this function for us:

![Figure 6.10 – Example of ChatGPT debugging a Python function](media/file92.png)

Figure 6.10 – Example of ChatGPT debugging a Python function

Impressive, isn’t it? Again, ChatGPT provided the correct version of the code, and it helped in the explanation of where the bugs were and why they led to an incorrect result. Let’s see whether it works now:

![Figure 6.11 – Python function after ChatGPT debugging](media/file93.png)

Figure 6.11 – Python function after ChatGPT debugging

Well, it obviously does!

These and many other code-related functionalities could really boost your productivity, shortening the time to perform many tasks.

However, ChatGPT goes beyond pure debugging. Thanks to the incredible language understanding of the GPT model behind, this GenAI tool is able to generate proper documentation alongside the code, as well as explain exactly what a string of code will do, which we will see in the next section.

## Generating documentation and code explainability

Whenever working with new applications or projects, it is always good practice to correlate your code with documentation. It might be in the form of a docstring that you can embed in your functions or classes so that others can invoke them directly in the development environment.

For example, let’s consider the same function developed in the previous section and let’s make it a Python class:

```
class UnderscoreAdder:
    def __init__(self, word):
        self.word = word
    def add_underscores(self):
        new_word = ""
        for i in range(len(self.word)):
            new_word += self.word[i] + "_"
        return new_word
```

We can test it as follows:

![Figure 5: Testing the UnderscoreAdder class.](media/file94.png)

Figure 5: Testing the UnderscoreAdder class.

Now, let’s say I want to be able to retrieve the docstring documentation using the `UnderscoreAdder` ? convention. By doing so with Python packages, functions, and methods, we have full documentation of the capabilities of that specific object, as follows (an example with the pandas Python library):![Figure 6.13 – Example of the pandas library documentation](media/file95.png)

Figure 6.13 – Example of the pandas library documentation

So, let’s now ask ChatGPT to produce the same result for our `UnderscoreAdder` class.

![Figure 6.14 – ChatGPT updating the code with documentation](media/file96.png)

Figure 6.14 – ChatGPT updating the code with documentation

As a result, if we update our class as shown in the preceding code and `UnderscoreAdder`?, we will get the following output:

![Figure 6.15 – The new UnderscoreAdder class documentation](media/file97.png)

Figure 6.15 – The new UnderscoreAdder class documentation

Finally, ChatGPT can also be leveraged to explain what a script, function, class, or other similar things do in natural language. We have already seen many examples of ChatGPT enriching its code-related response with clear explanations. However, we can boost this capability by asking specific questions in terms of code understanding.

For example, let’s ask ChatGPT to explain to us what the following Python script does:

![Figure 6.16 – Example of ChatGPT explaining a Python script](media/file98.png)

Figure 6.16 – Example of ChatGPT explaining a Python script

Code explainability can also be part of the preceding mentioned documentation, or it can be used among developers who might want to better understand complex code from other teams or (as sometimes happens to me) remember what they wrote some time ago.

Thanks to ChatGPT and the capabilities mentioned in this section, developers can easily keep track of the project life cycle in natural language so that it is easier for both new team members and non-technical users to understand the work done so far.

We will see in the next section how code explainability is a pivotal step for ML model interpretability in data science projects.

## Understanding ML model interpretability

Model interpretability refers to the degree of ease with which a human can comprehend the logic behind the ML model’s predictions. Essentially, it is the capability to comprehend how a model arrives at its decisions and which variables are contributing to its forecasts.

Let’s see an example of model interpretability using a deep learning **convolutional neural network** (**CNN**) for image classification. I built my model in Python using Keras. For this purpose, I will download the CIFAR-10 dataset directly from keras.datasets: it consists of 60,000 32x32 color images (so 3-channels images) in 10 classes (airplane, automobile, bird, cat, deer, dog, frog, horse, ship, and truck), with 6,000 images per class. Here, I will share just the body of the model; you can find all the related code in the book’s GitHub repository for data preparation and pre-processing at [https://github.com/PacktPublishing/The-Ultimate-Guide-to-ChatGPT-and-OpenAI/tree/main/Chapter%206%20-%20ChatGPT%20for%20Developers/code](https://github.com/PacktPublishing/The-Ultimate-Guide-to-ChatGPT-and-OpenAI/tree/main/Chapter%206%20-%20ChatGPT%20for%20Developers/code):

```
model=tf.keras.Sequential()
model.add(tf.keras.layers.Conv2D(32,kernel_ size=(3,3),activation='relu',input_shape=
(32,32,1)))
model.add(tf.keras.layers.MaxPooling2D(pool_size=(2,2))) model.add(tf.keras.layers.Flatten()) model.add(tf.keras.layers.Dense(1024,activation='relu')) model.add(tf.keras.layers.Dense(10,activation='softmax'))
```

The preceding code is made of several layers that perform different actions. I might be interested in having an explanation of the structure of the model as well as the purpose of each layer. Let’s ask ChatGPT for some help with that (below you can see an extract of the response):

![Figure 6.17 – Model interpretability with ChatGPT](media/file99.png)

Figure 6.17 – Model interpretability with ChatGPT

As you can see in the preceding figure, ChatGPT was able to give us a clear explanation of the structure and layers of our CNN. It also adds some comments and tips, such as the fact that using the max pooling layer helps reduce the dimensionality of the input.

I can also be supported by ChatGPT in interpreting model results in the validation phase. So, after splitting data into training and test sets and training the model on the training set, I want to see its performance on the test set:

![Figure 6.18 – Evaluation metrics](media/file100.jpg)

Figure 6.18 – Evaluation metrics

Let’s also ask ChatGPT to elaborate on our validation metrics (truncated output):

![Figure 6.19 – Example of ChatGPT explaining evaluation metrics](media/file101.png)

Figure 6.19 – Example of ChatGPT explaining evaluation metrics

Once again, the result was really impressive, and it provided clear guidance on how to set up ML experiments in terms of training and test sets. It explains how important it is for the model to be sufficiently generalized so that it does not overfit and is able to predict accurate results on data that it has never seen before.

There are many reasons why model interpretability is important. A pivotal element is that it reduces the gap between business users and the code behind models. This is key to enabling business users to understand how a model behaves, as well as translate it into code business ideas.

Furthermore, model interpretability enables one of the key principles of responsible and ethical AI, which is the transparency of how a model behind AI systems thinks and behaves. Unlocking model interpretability means detecting potential biases or harmful behaviors a model could have while in production and consequently preventing them from happening.

Overall, ChatGPT can provide valuable support in the context of model interpretability, generating insights at the row level, as we saw in the previous example.

The next and last ChatGPT capability we will explore will be yet another boost for developers’ productivity, especially when various programming languages are being used within the same project.

## Translation among different programming languages

In *Chapter 5*, we saw how ChatGPT has great capabilities for translating between different languages. What is really incredible is that natural language is not its only object of translation. In fact, ChatGPT is capable of translating between different programming languages while keeping the same output as well as the same style (namely, it preserves docstring documentation if present).

There are so many scenarios when this could be a game changer.

For example, you might have to learn a new programming language or statistical tool you’ve never seen before because you need to quickly deliver a project on it. With the help of ChatGPT, you can start programming in your language of preference and then ask it to translate to the desired language, which you will be learning alongside the translation process.

Imagine that the project needs to be delivered in MATLAB (a proprietary numerical computing and programming software developed by MathWorks), yet you’ve always programmed in Python. The project consists of classifying images from the **Modified National Institute of Standards and Technology** (**MNIST**) dataset (the original dataset description and related paper can be found here at [http://yann.lecun.com/exdb/mnist/).](http://yann.lecun.com/exdb/mnist/).) The dataset contains numerous handwritten digits and is frequently utilized to teach various image processing systems.

To start, I wrote the following Python code to initialize a deep-learning model for classification:

```
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
# Load the MNIST dataset
(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_ data()
# Preprocess the data
x_train = x_train.reshape(-1, 28*28) / 255.0 x_test = x_test.reshape(-1, 28*28) / 255.0 y_train = keras.utils.to_categorical(y_train) y_test = keras.utils.to_categorical(y_test)
# Define the model architecture model = keras.Sequential([
layers.Dense(256, activation='relu', input_shape=(28*28,)), layers.Dense(128, activation='relu'),
layers.Dense(10, activation='softmax')
])
# Compile the model
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
# Train the model
history = model.fit(x_train, y_train, validation_split=0.2, epochs=10, batch_size=128)
# Evaluate the model
test_loss, test_acc = model.evaluate(x_test, y_test, verbose=0) print('Test accuracy:', test_acc)
```

Let’s now see what happens if we give the preceding code as context to ChatGPT and ask it to translate it into MATLAB:

![Figure 20 – ChatGPT translates Python code into MATLAB](media/file102.png)

Figure 20 – ChatGPT translates Python code into MATLAB

Let’s also see whether it is capable of translating it into other languages such as JavaScript (note that I didn’t have to repeat the code, since it was already in the chat context):

![Figure 6.21 – ChatGPT translates Python code into JavaScript](media/file103.png)

Figure 6.21 – ChatGPT translates Python code into JavaScript

Code translation could also reduce the skill gap between new technologies and current programming capabilities.

Another key implication of code translation is **application modernization**. Indeed, imagine you want to refresh your application stack, namely migrating to the cloud. You could decide to initiate with a simple lift and shift going toward **Infrastructure-as-a-Service** (**IaaS**) instances (such as Windows or Linux **virtual machines** (**VMs**)). However, in a second phase, you might want to refactor, rearchitect, or ever rebuild your applications.

The following diagram depicts the various options for application modernization:

![Figure 6.22 – Four ways you can migrate your applications to the public cloud](media/file104.png)

Figure 6.22 – Four ways you can migrate your applications to the public cloud

ChatGPT and OpenAI Codex models can help you with the migration. Consider mainframes, for example.

Mainframes are computers that are predominantly employed by large organizations to carry out essential tasks such as bulk data processing for activities such as censuses, consumer and industry statistics, enterprise resource planning, and large-scale transaction processing. The application programming language of the mainframe environment is **Common Business Oriented Language** (**COBOL**). Despite being invented in 1959, COBOL is still in use today and is one of the oldest programming languages in existence.

As technology continues to improve, applications residing in the realm of mainframes have been subject to a continuous process of migration and modernization aimed at enhancing existing legacy mainframe infrastructure in areas such as interface, code, cost, performance, and maintainability.

Of course, this implies translating COBOL to more modern programming languages such as C# or Java. The problem is that COBOL is unknown to most of the new-generation programmers; hence there is a huge skills gap in this context.

Let’s consider a COBOL script that reads an input number, adds 10 to it, and then prints the result.

```
 IDENTIFICATION DIVISION.
       PROGRAM-ID. AddTen.
       DATA DIVISION.
       WORKING-STORAGE SECTION.
       01  INPUT-NUMBER    PIC 9(5).
       01  RESULT-NUMBER   PIC 9(5).
       PROCEDURE DIVISION.
       DISPLAY 'Enter a number: '.
       ACCEPT INPUT-NUMBER.
       COMPUTE RESULT-NUMBER = INPUT-NUMBER + 10.
       DISPLAY 'Result after adding 10: ' RESULT-NUMBER.
       STOP RUN.
```

I then passed the previous COBOL script to ChatGPT so that it can use it as context to formulate its response. Let’s now ask ChatGPT to translate that script into C#:

![Figure 6.23 – Example of ChatGPT translating COBOL to C#.](media/file105.png)

Figure 6.23 – Example of ChatGPT translating COBOL to C#.

Tools such as ChatGPT can help in reducing the skill gap in this and similar scenarios by introducing a layer that knows both the past and the future of programming.

In conclusion, ChatGPT can be an effective tool for application modernization, providing code upgrading in addition to valuable insights and recommendations for enhancing legacy systems. With its advanced language processing capabilities and extensive knowledge base, ChatGPT can help organizations streamline their modernization efforts, making the process faster, more efficient, and more effective.

## Summary

ChatGPT can be a valuable resource for developers looking to enhance their skills and streamline their workflows. We started by seeing how ChatGPT can generate, optimize, and debug your code, but we also covered further capabilities such as generating documentation alongside your code, explaining your ML models, and translating between different programming languages for application modernization.

Whether you’re a seasoned developer or just starting out, ChatGPT offers a powerful tool for learning and growth, reducing the gap between code and natural language.

In the next chapter, we will dive deeper into another domain of application where ChatGPT could be a game changer: marketing.

# 6 Mastering Marketing with ChatGPT

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In this chapter, we will focus on how marketers can leverage ChatGPT, looking at the main use cases of ChatGPT in this domain, and how marketers can leverage it as a valuable assistant.

We will learn how ChatGPT can assist in the following activities:

*   Marketers’ need for ChatGPT
*   New product development and the go-to-market strategy
*   A/B testing for marketing comparison
*   Making more efficient websites and posts with **Search Engine Optimization** (**SEO**)
*   Sentiment analysis of textual data

By the end of this chapter, you will be able to leverage ChatGPT for marketing-related activities and to boost your productivity.

## Technical requirements

You will need an OpenAI account to access ChatGPT.

## Marketers’ need for ChatGPT

Marketing is probably the domain where ChatGPT and OpenAI models’ creative power can be leveraged in their purest form. They can be practical tools to support creative development in terms of new products, marketing campaigns, search engine optimization, and so on. Overall, marketers automate and streamline many aspects of their work, while also improving the quality and effectiveness of their marketing efforts.

Here is an example. One of the most prominent and promising use cases of ChatGPT in marketing is personalized marketing. ChatGPT can be used to analyze customer data and generate personalized marketing messages that resonate with individual customers. For example, a marketing team can use ChatGPT to analyze customer data and develop targeted email campaigns that are tailored to specific customer preferences and behavior. This can increase the likelihood of conversion and lead to greater customer satisfaction. By providing insights into customer sentiment and behavior, generating personalized marketing messages, providing personalized customer support, and generating content, ChatGPT can help marketers deliver exceptional customer experiences and drive business growth.

This is one of many examples of ChatGPT applications in marketing. In the following sections, we will look at concrete examples of end-to-end marketing projects supported by ChatGPT.

## New product development and the go-to-market strategy

The first way you can introduce ChatGPT into your marketing activity might be as an assistant in new product development and **go-to-market** (**GTM**) strategy.

In this section, we will look at a step-by-step guide on how to develop and promote a new product. You already own a running clothing brand called **RunFast** and so far you have only produced shoes, so you want to expand your business with a new product line. We will start by brainstorming ideas to create a GTM strategy. Of course, everything is supported by ChatGPT:

*   Brainstorming ideas: The first thing ChatGPT can support you with is brainstorming and drafting options for your new product line. It will also provide the reasoning behind each suggestion. So, let’s ask what kind of new product line I should focus on:

![Figure 7.1 – Example of new ideas generated by ChatGPT](media/file107.png)

Figure 7.1 – Example of new ideas generated by ChatGPT

Out of the three suggestions, we will pick the second one, because of the positive environmental impact that we can make with it, as well as improving our brand reputation. More specifically, I will start with eco-friendly running socks.

*   Product name: Now that we have our idea fixed in mind, we need to think of a catchy name for it. Again, I will ask ChatGPT for more options so that I can then pick my favorite one:

![Figure 7.2 – A list of potential product names](media/file108.png)

Figure 7.2 – A list of potential product names

GreenStride sounds good enough for me – I’ll go ahead with that one.

*   Generating catchy slogans: On top of the product name, I also want to share the intent behind the name and the mission of the product line, so that my target audience is captured by it. I want to inspire trust and loyalty in my customers and for them to see themselves reflected in the mission behind my new product line.

![Figure 7.3 – A list of slogans for my new product name](media/file109.png)

Figure 7.3 – A list of slogans for my new product name

Great – now I’m satisfied with the product name and slogan that I will use later on to create a unique social media announcement. Before doing that, I want to spend more time on market research for the target audience.

![Figure 7.4 – List of groups of target people to reach with my new product line](media/file110.png)

Figure 7.4 – List of groups of target people to reach with my new product line

It’s important to have in mind different clusters within your audience so that you can differentiate the messages you want to give. In my case, I want to make sure that my product line will address different groups of people, such as competitive runners, casual runners, and fitness enthusiasts.

*   Product variants and sales channels: According to the preceding clusters of potential customers, I could generate product variants so that they are more tailored toward specific audiences:

![Figure 7.5 – Example of variants of the product line](media/file111.png)

Figure 7.5 – Example of variants of the product line

Similarly, I can also ask ChatGPT to suggest different sales channels for each of the preceding groups:

![Figure 7.6 – Suggestions for different sales channels by ChatGPT](media/file112.png)

Figure 7.6 – Suggestions for different sales channels by ChatGPT

*   Standing out from the competition: I want my product line to stand out from the competition and emerge in a very saturated market – I want to make it unique. With this purpose in mind, I asked ChatGPT to include social considerations such as sustainability and inclusivity. Let’s ask ChatGPT for some suggestions in that respect:

![Figure 7.7 – Example of outstanding features generated by ChatGPT](media/file113.png)

Figure 7.7 – Example of outstanding features generated by ChatGPT

As you can see, it was able to generate interesting features that could make my product line unique.

*   Product description: Now it’s time to start building our Go-To-Market plan. First of all, I want to generate a product description for my website, including all the earlier unique differentiators.

    ![Figure 7.8 – Example of description and SEO keywords generated by ChatGPT](media/file114.png)

    Figure 7.8 – Example of description and SEO keywords generated by ChatGPT

*   Fair price: Another key element is determining a fair price for our product. As I differentiated among product variants for different audiences (competitive runners, casual runners, and fitness enthusiasts), I also want to have a price range that takes into account this clustering. Note that, in the following example, ChatGPT is invoking the web search plug-in to retrieve updated information about the current marketplace of running socks in terms of pricing.

![Figure 7.9 – Price ranges for product variants](media/file115.png)

Figure 7.9 – Price ranges for product variants

We are almost there. We have gone through many new product development and go-to-market steps, and in each of them, ChatGPT acted as a great support tool.

As one last thing, we can ask ChatGPT to generate an Instagram post about our new product, including relevant hashtags and SEO keywords. We can then generate the image DALL-E, which comes as an embedded plug-in in ChatGPT Plus.

![Figure 7.10 – Social media post generated by ChatGPT](media/file116.png)

Figure 7.10 – Social media post generated by ChatGPT

And, with the special contribution of DALL-E:

![Figure 1: Example of an illustration generated by ChatGPT powered by DALL-E 3.](media/file117.png)

Figure 1: Example of an illustration generated by ChatGPT powered by DALL-E 3.

Here it is the final result:

![Figure 7.11 – Instagram post entirely generated by ChatGPT and DALL-E](media/file118.png)

Figure 7.11 – Instagram post entirely generated by ChatGPT and DALL-E

Of course, many elements are missing here for complete product development and go-to-market. Yet, with the support of ChatGPT (and the special contribution of DALL-E 3) we managed to brainstorm a new product line and variants, potential customers, catchy slogans, and finally, generated a pretty nice Instagram post to announce the launch of GreenStride!

## A/B testing for marketing comparison

Another interesting field where ChatGPT can assist marketers is A/B testing.

A/B testing in marketing is a method of comparing two different versions of a marketing campaign, advertisement, or website to determine which one performs better. In A/B testing, two variations of the same campaign or element are created, with only one variable changed between the two versions. The goal is to see which version generates more clicks, conversions, or other desired outcomes.

An example of A/B testing might be testing two versions of an email campaign, using different subject lines, or testing two versions of a website landing page, with different call-to-action buttons. By measuring the response rate of each version, marketers can determine which version performs better and make data-driven decisions about which version to use going forward.

A/B testing allows marketers to optimize their campaigns and elements for maximum effectiveness, leading to better results and a higher return on investment.

Since this method involves the process of generating many variations of the same content, the generative power of ChatGPT can definitely assist in that.

Let’s consider the following example. I’m promoting a new product I developed: a new, light and thin climbing harness for speed climbers. I’ve already done some market research and I know my niche audience. I also know that one great channel of communication for that audience is publishing on an online climbing blog, of which most climbing gyms’ members are fellow readers.

My goal is to create an outstanding blog post to share the launch of this new harness, and I want to test two different versions of it in two groups. The blog post I’m about to publish and that I want to be the object of my A/B testing is the following:

![Figure 7.12 – An example of a blog post to launch climbing gear](media/file119.png)

Figure 7.12 – An example of a blog post to launch climbing gear

Here, ChatGPT can help us on two levels:

*   The first level is that of rewording the article, using different keywords or different attention- grabbing slogans. To do so, once this post is provided as context, we can ask ChatGPT to work on the article and slightly change some elements:

![Figure 7.13 – New version of the blog post generated by ChatGPT](media/file120.png)

Figure 7.13 – New version of the blog post generated by ChatGPT

As per my request, ChatGPT was able to regenerate only those elements I asked for (title, subtitle, and closing sentence) so that I can monitor the effectiveness of those elements by monitoring the reaction of the two audience groups.

*   The second level is working on the design of the web page, namely, changing the collocation of the image rather than the position of the buttons. For this purpose, I created a simple web page for the blog post published in the climbing blog (you can find the code in the book’s GitHub repository at [https://github.com/PacktPublishing/The-Ultimate-Guide- to-ChatGPT-and-OpenAI/tree/main/Chapter%207%20-%20ChatGPT%20 for%20Marketers/Code](https://github.com/PacktPublishing/The-Ultimate-Guide-)):

![Figure 7.14 – Sample blog post published on the climbing blog](media/file121.png)

Figure 7.14 – Sample blog post published on the climbing blog

We can directly feed ChatGPT with the HTML code and ask it to change some layout elements, such as the position of the buttons or their wording. For example, rather than Buy Now, a reader might be more gripped by an I want one! button.

So, lets feed ChatGPT with the HTML source code:

![Figure 7.14 – ChatGPT changing HTML code](media/file122.png)

Figure 7.14 – ChatGPT changing HTML code

Let’s see what the output looks like (I also changed the title, subtitle and paragraph with the once generated by ChatGPT):

![Figure 7.15 – New version of the website](media/file123.png)

Figure 7.15 – New version of the website

As you can see, ChatGPT only intervened at the button level, slightly changing their layout, position, color, and wording.

In conclusion, ChatGPT is a valuable tool for A/B testing in marketing. Its ability to quickly generate different versions of the same content can reduce the time to market of new campaigns. By utilizing ChatGPT for A/B testing, you can optimize your marketing strategies and ultimately drive better results for your business.

## Boosting Search Engine Optimization (SEO)

Another promising area for ChatGPT to be a game changer is **Search Engine Optimization** (**SEO**). This is the key element behind ranking in search engines such as Google or Bing and it determines whether your websites will be visible to users who are looking for what you promote.

> SEO is a technique used to enhance the visibility and ranking of a website on **search engine results pages** (**SERPs**). It is done by optimizing the website or web page to increase the amount and quality of organic (unpaid) traffic from search engines. The purpose of SEO is to attract more targeted visitors to the website by optimizing it for specific keywords or phrases.

Imagine you run an e-commerce company called Hat&Gloves, which only sells, as you might have guessed, hats and gloves. You are now creating your e-commerce website and want to optimize its ranking. Let’s ask ChatGPT to list some relevant keywords to embed in our website:

![Figure 7.17 – Example of SEO keywords generated by ChatGPT](media/file124.png)

Figure 7.17 – Example of SEO keywords generated by ChatGPT

As you can see, ChatGPT was able to create a list of keywords of different kinds. Some of them are pretty intuitive, such as **Hats** and **Gloves**. Others are related, with an indirect link. For example, **Gift ideas** is not necessarily related to my e-commerce business, however, it could be very smart to include it in my keywords, so that I can widen my audience.

Another key element of SEO is **search engine intent**. Search engine intent, also known as **user intent**, refers to the underlying purpose or goal of a specific search query made by a user in a search engine. Understanding search engine intent is important because it helps businesses and marketers create more targeted and effective content and marketing strategies that align with the searcher’s needs and expectations.

There are generally four types of search engine intent:

*   **Informational intent**: The user is looking for information on a particular topic or question, such as What is the capital of France? or How to make a pizza at home.
*   **Navigational intent**: The user is looking for a specific website or web page, such as **Facebook login** or **Amazon.com**.
*   **Commercial intent**: The user is looking to buy a product or service, but may not have made a final decision yet. Examples of commercial intent searches include *best laptop under $1000* or *discount shoes online*.
*   **Transactional intent**: The user has a specific goal to complete a transaction, which might refer to physical purchases or subscribing to services. Examples of transactional intent could be *buy iPhone 13* or *sign up for a gym membership*.

By understanding the intent behind specific search queries, businesses and marketers can create more targeted and effective content that meets the needs and expectations of their target audience. This can lead to higher search engine rankings, more traffic, and ultimately, more conversions and revenue.

Now, the question is, will ChatGPT be able to determine the intent of a given request? Before answering, it is worth noticing that the activity of inferring the intent of a given prompt is the core business of **Large Language Models** (**LLMs**), including GPT. So, for sure, ChatGPT is able to capture prompts’ intents.

The added value here is that we want to see whether ChatGPT is able to determine the intent in a precise domain with a precise taxonomy, that is, the one of marketing. That is the reason why prompt design is once again pivotal in guiding ChatGPT in the right direction.

![Figure 7.18 – Example of keywords clustered by user intent by ChatGPT](media/file125.png)

Figure 7.18 – Example of keywords clustered by user intent by ChatGPT

Finally, we could also go further and leverage once more the Act as… hack, which we already mentioned in *Chapter 3*. It would be very interesting indeed to have an assessment of our website and understand whether it is optimized as intended. In marketing, this analysis is called an **SEO audit**. An SEO audit is an evaluation of a website’s SEO performance and potential areas for improvement. An SEO audit is typically conducted by SEO experts, web developers, or marketers, and involves a comprehensive analysis of a website’s technical infrastructure, content, and backlink profile.

During an SEO audit, the auditor will typically use a range of tools and techniques to identify areas of improvement, such as keyword analysis, website speed analysis, website architecture analysis, and content analysis. The auditor will then generate a report outlining the key issues, opportunities for improvement, and recommended actions to address them.

Let’s ask ChatGPT to act as an SEO expert to conduct this audit. As a reference website, we will use our climbing blog referenced above. I will give ChatGPT the code and ask give the following instructions: “act as a SEO specialist and generate a brief SEO audit (max 300 words) on the above HTML code”. This is the response:

![Figure 7.19 – ChatGPT generating an SEO audit on the HTML code of a climbing blog.](media/file126.png)

Figure 7.19 – ChatGPT generating an SEO audit on the HTML code of a climbing blog.

ChatGPT was able to generate a pretty accurate analysis, with relevant comments and suggestions. Overall, ChatGPT has interesting potential for SEO-related activities, and it can be a good tool whether you are building your website from scratch, or you want to improve existing ones.

## Sentiment analysis to improve quality and increase customer satisfaction

Sentiment analysis is a technique used in marketing to analyze and interpret the emotions and opinions expressed by customers toward a brand, product, or service. It involves the use of **natural language processing** (**NLP**) and **machine learning** (**ML**) algorithms to identify and classify the sentiment of textual data such as social media posts, customer reviews, and feedback surveys.

By performing sentiment analysis, marketers can gain insights into customer perceptions of their brand, identify areas for improvement, and make data-driven decisions to optimize their marketing strategies. For example, they can track the sentiment of customer reviews to identify which products or services are receiving positive or negative feedback and adjust their marketing messaging accordingly.

Overall, sentiment analysis is a valuable tool for marketers to understand customer sentiment, gauge customer satisfaction, and develop effective marketing campaigns that resonate with their target audience.

Sentiment analysis has been around for a while, so you might be wondering what ChatGPT could bring as added value. Well, besides the accuracy of the analysis (it being the most powerful model on the market right now), ChatGPT differentiates itself from other sentiment analysis tools since it is **general artificial intelligence** (**AGI**).

This means that when we use ChatGPT for sentiment analysis, we are not using one of its specific APIs for that task: the core idea behind ChatGPT and OpenAI models is that they can assist the user in many general tasks at once, interacting with a task and changing the scope of the analysis according to the user’s request.

So, for sure, ChatGPT is able to capture the sentiment of a given text, such as a Twitter post or a product review. However, ChatGPT can also go further and assist in identifying specific aspects of a product or brand that are positively or negatively impacting the sentiment. For example, if customers consistently mention a particular feature of a product in a negative way, ChatGPT can highlight that feature as an area for improvement. Or, ChatGPT might be asked to generate a response to a particularly delicate review, keeping in mind the sentiment of the review and using it as context for the response. Again, it can generate reports that summarize all the negative and positive elements found in reviews or comments and cluster them into categories.

Let’s consider the following example. A customer has recently purchased a pair of shoes from my e-commerce company, RunFast, and left the following review:

*“I recently bought the RunFast Prodigy shoes and have mixed feelings. They're extremely comfortable with excellent cushioning and support, reducing foot fatigue during my runs. The design is also appealing, and I've received several compliments. However, the durability is disappointing; the outsole wears quickly, and the breathable upper shows signs of wear after a few weeks. Given the high price, I'm hesitant to recommend them despite their comfort and design.”*

Let’s ask ChatGPT to capture the sentiment of this review:

![Figure 7.20 – ChatGPT analyzing a customer review](media/file127.png)

Figure 7.20 – ChatGPT analyzing a customer review

From the preceding figure, we can see how ChatGPT didn’t limit itself to providing a label: it also explained both the positive and negative elements characterizing the review, which has a mixed feeling and hence can be labeled as neutral overall.

Let’s try to go deeper into that and ask some suggestions about improving the product:

![Figure 7.21 – Suggestions on how to improve my product based on customer feedback](media/file128.png)

Figure 7.21 – Suggestions on how to improve my product based on customer feedback

Finally, let’s generate a response to the customer, showing that we, as a company, do care about customers’ feedback and want to improve our products.

![Figure 7.22 – Response generated by ChatGPT](media/file129.png)

Figure 7.22 – Response generated by ChatGPT

The example we saw was a very simple one with just one review. Now imagine we have tons of reviews, as well as diverse sales channels where we receive feedback. Imagine the power of tools such as ChatGPT and OpenAI models, which are able to analyze and integrate all of that information and identify the pluses and minuses of your products, as well as capturing customer trends and shopping habits. Additionally, for customer care and retention, we could also automate review responses using the writing style we prefer. In fact, by tailoring your chatbot’s language and tone to meet the specific needs and expectations of your customers, you can create a more engaging and effective customer experience.

Here are some examples:

*   Empathetic chatbot: A chatbot that uses an empathetic tone and language to interact with customers who may be experiencing a problem or need help with a sensitive issue
*   Professional chatbot: A chatbot that uses a professional tone and language to interact with customers who may be looking for specific information or need help with a technical issue
*   Conversational chatbot: A chatbot that uses a casual and friendly tone to interact with customers who may be looking for a personalized experience or have a more general inquiry
*   Humorous chatbot: A chatbot that uses humor and witty language to interact with customers who may be looking for a light-hearted experience or to diffuse a tense situation
*   Educational chatbot: A chatbot that uses a teaching style of communication to interact with customers who may be looking to learn more about a product or service

In conclusion, ChatGPT can be a powerful tool for businesses to conduct sentiment analysis, improve their quality, and retain their customers. With its advanced natural language processing capabilities, ChatGPT can accurately analyze customer feedback and reviews in real time, providing businesses with valuable insights into customer sentiment and preferences. By using ChatGPT as part of their customer experience strategy, businesses can quickly identify any issues that may be negatively impacting customer satisfaction and take corrective action. Not only can this help businesses improve their quality, but it can also increase customer loyalty and retention.

## Summary

In this chapter, we explored ways in which ChatGPT can be used by marketers to enhance their marketing strategies. We learned that ChatGPT can help in developing new products as well as defining their go-to-market strategy, designing A/B testing, enhancing SEO analysis, and capturing the sentiment of reviews, social media posts, and other customer feedback.

The importance of ChatGPT for marketers lies in its potential to revolutionize the way companies engage with their customers. By leveraging the power of NLP, ML, and big data, ChatGPT allows companies to create more personalized and relevant marketing messages, improve customer support and satisfaction, and ultimately, drive sales and revenue.

As ChatGPT continues to advance and evolve, it is likely that we will see even more involvement in the marketing industry, especially in the way companies engage with their customers. In fact, relying heavily on AI allows companies to gain deeper insights into customer behavior and preferences.

The key takeaway for marketers is to embrace these changes and adapt to the new reality of AI-powered marketing in order to stay ahead of the competition and meet the needs of their customers.

In the next chapter, we will look at the third and last domain in the application of ChatGPT covered in this book – research.

# 7 Research Reinvented with ChatGPT

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In this chapter, we focus on researchers who wish to leverage ChatGPT. The chapter will go through a few main use cases ChatGPT can address, so that you will learn from concrete examples how ChatGPT can be used in research.

By the end of this chapter, you will be familiar with using ChatGPT as a research assistant in many ways, including the following:

*   Researchers’ need for ChatGPT
*   Brainstorming literature for your study
*   Providing support for the design and framework of your experiment
*   Generating and formatting the bibliography to incorporate in your research study

Delivering a pitch or slide deck presentation about your study addressing diverse audiences This chapter will also provide examples and enable you to try the prompts on your own.

## Researchers’ need for ChatGPT

ChatGPT can be an incredibly valuable resource for researchers across a wide range of fields. As a sophisticated language model trained on vast amounts of data, ChatGPT can quickly and accurately process large amounts of information and generate insights that might be difficult or time-consuming to uncover through traditional research methods.

Additionally, ChatGPT can provide researchers with a unique perspective on their field, by analyzing patterns and trends that might not be immediately apparent to human researchers. For example, imagine a researcher studying climate change and wanting to understand the public perception of this issue. They might ask ChatGPT to analyze social media data related to climate change and identify the most common themes and sentiments expressed by people online. ChatGPT could then provide the

researcher with a comprehensive report detailing the most common words, phrases, and emotions associated with this topic, as well as any emerging trends or patterns that might be useful to know.

By working with ChatGPT, researchers can gain access to cutting-edge technology and insights, and stay at the forefront of their field.

Let’s now dive deeper into four use cases where ChatGPT can boost research productivity.

> Most examples proposed in this Chapter are based on up-to-date information; in fact, you will see ChatGPT leveraging the web search plug-in very often. If you are using ChatGPT with the GPT-3.5-turbo (so, the free edition), keep in mind that it doesn’t have the web search plug-in enabled and, henceforth, its knowledge cutoff is limited to the 2021\. This could be a limitation if you are looking for updated information and, in general, for references coming from the web (so that you can double-check the groundedness of the response).

## Brainstorming literature for your study

A literature review is a critical and systematic process of examining existing published research on a specific topic or question. It involves searching, reviewing, and synthesizing relevant published studies and other sources, such as books, conference proceedings, and gray literature. The goal of a literature review is to identify gaps, inconsistencies, and opportunities for further research in a particular field.

The literature review process typically involves the following steps:

1.  **Defining the research question**: The first step in conducting a literature review is to define the research question of the topic of interest. So, let’s say we are carrying on a research on the effects of social media on mental health. Now we are interested in brainstorming some possible research questions to focus our research on, and we can leverage ChatGPT to do so:

![Figure 8.1 – Examples of research questions based on a given topic](media/file131.png)

Figure 8.1 – Examples of research questions based on a given topic

Those are all interesting questions that could be further investigated. Since I’m particularly interested in the first one – “In what ways do social media interactions and online support communities affect the mental well-being of individuals with chronic mental health conditions?” - I will keep that one as a reference for the next steps of our analysis.

1.  **Searching for literature**: Now that we have our research question, the next step is to search for relevant literature using a variety of databases, search engines, and other sources. Researchers can use specific keywords and search terms to help identify relevant studies.

![Figure 8.2 – Literature search with the support of ChatGPT](media/file132.png)

Figure 8.2 – Literature search with the support of ChatGPT

Starting from the suggestions of ChatGPT, we can start diving deeper into those references.

1.  **Screening the literature**: Once relevant literature has been identified, the next step is to screen the studies to determine whether they meet the inclusion criteria for the review. This typically involves reviewing the abstract and, if necessary, the full text of the study.

Let’s say, for example, that we want to go deeper into the **Social Media and Teen Anxiety: Insights from Harvard Graduate School of Education** research paper. Let’s ask ChatGPT to screen it for us:

![Figure 8.3 – Literature screening of a specific paper](media/file133.png)

Figure 8.3 – Literature screening of a specific paper

ChatGPT was able to provide me with an overview of the paper and, considering its research question and main topics of discussion, I think it will be pretty useful for my own study.

1.  **Extracting data**: After the relevant studies have been identified, researchers will need to extract data from each study, such as the study design, sample size, data collection methods, and key findings.

For example, let’s say that we want to gather the following information from the paper Digital Self-Harm: Prevalence, Motivations, and Outcomes by Hinduja and Patchin (2018):

*   Data sources collected in the paper and object of the study
*   data collection method adopted by researchers
*   data sample size
*   main limitations and drawbacks of the analysis
*   the experiment design adopted by researchers

Here is how it goes

![Figure 8.4 – Extracting relevant data and frameworks from a given paper](media/file134.png)

Figure 8.4 – Extracting relevant data and frameworks from a given paper

1.  **Synthesizing the literature**: The final step in the literature review process is to synthesize the findings of the studies and draw conclusions about the current state of knowledge in the field. This may involve identifying common themes, highlighting gaps or inconsistencies in the literature, and identifying opportunities for future research.

Let’s imagine that, besides the papers proposed by ChatGPT, we have collected other titles and papers we want to synthesize. More specifically, I want to understand whether they drive the same conclusions, what are the common trends, and which method might be more reliable than others. For this scenario, we will consider three research papers:

*   *The Effects of Social Media on Mental Health: A Proposed Study*, by Grant Sean Bossard ([https://digitalcommons.bard.edu/cgi/viewcontent. cgi?article=1028&context=senproj_f2020](https://digitalcommons.bard.edu/cgi/viewcontent.))
*   *The Impact of Social Media on Mental Health*, by Vardanush Palyan ([https://www. spotlightonresearch.com/mental-health-research/the-impact-of- social-media-on-mental-health](https://www.))
*   *The Impact of Social Media on Mental Health: a mixed methods research of service providers’ awareness*, by Sarah Nichole Koehler and Bobbie Rose Parrell ([https://scholarworks. lib.csusb.edu/cgi/viewcontent.cgi?article=2131&context=etd](https://scholarworks.))

Here is how the results appear:

![Figure 8.5 – Literature analysis and benchmarking of three research papers](media/file135.png)

Figure 8.5 – Literature analysis and benchmarking of three research papers

Also, in this case, ChatGPT was able to produce a relevant summary and analysis of the three papers provided, including benchmarking among the methods and reliability considerations.

Overall, ChatGPT was able to carry out many activities in the field of literature review, from research question brainstorming to literature synthesis. As always, a **subject-matter expert** (**SME**) is needed in the loop to review the results, however, with this assistance many activities can be done more efficiently.

Another activity that can be supported by ChatGPT is the design of the experiment the researcher wants to carry out. We are going to look at that in the following section.

## Providing support for the design and framework of your experiment

Experiment design is the process of planning and executing a scientific experiment or study to answer a research question. It involves making decisions about the study’s design, the variables to be measured, the sample size, and the procedures for collecting and analyzing data.

ChatGPT can help in experiment design for research by suggesting to you the study framework, such as a randomized controlled trial, quasi-experimental design, or a correlational study, and supporting you alongside the implementation of that design.

Let’s consider the following scenario. We want to investigate the effects of a new educational program on student learning outcomes in mathematics. This new program entails **project-based learning** (**PBL**), meaning that students are asked to work collaboratively on real-world projects, using math concepts and skills to solve problems and create solutions.

For this purpose, we defined our research question as follows:

How does the new PBL program compare to traditional teaching methods in improving student performance?

Here’s how ChatGPT can help:

*   **Determining study design**: ChatGPT can assist in determining the appropriate study design for the research question, such as a randomized controlled trial, quasi-experimental design, or correlational study.

![Figure 8.6 – ChatGPT suggesting the appropriate study design for your experiment](media/file136.png)

Figure 8.6 – ChatGPT suggesting the appropriate study design for your experiment

ChatGPT suggested proceeding with a randomized controlled trial (RCT) and provided a clear explanation of the reason behind it. It seems reasonable to me to proceed with this approach: the next steps will be to identify outcome measures and variables to consider in our experiment.

*   **Identifying outcome measures**: ChatGPT can help you identify some potential outcome measures to determine the results of your test. Let’s ask for some suggestions for our study:

![Figure 8.7 – Learning outcomes for the given research study](media/file137.png)

Figure 8.7 – Learning outcomes for the given research study

It is reasonable for me to pick test scores as the outcome measure.

*   **Identifying variables**: ChatGPT can help the researcher to identify the independent and dependent variables in the study:

![Figure 8.8 – ChatGPT generating variables for the given study](media/file138.png)

Figure 8.8 – ChatGPT generating variables for the given study

Note that ChatGPT was also able to generate the type of variables, called **control variables**, that are specific to the study design we are considering (RCT).

Control variables, also known as covariates, are variables that are held constant or are controlled in a research study in order to isolate the relationship between the independent variable(s) and the dependent variable. These variables are not the primary focus of the study but are included to minimize the effect of confounding variables on the results. By controlling these variables, researchers can reduce the risk of obtaining false positive or false negative results and increase the internal validity of their study.

With the preceding variables, we are ready to set up our experiment. Now we need to select participants, and ChatGPT can assist us with that.

*   **Sampling strategy**: ChatGPT can suggest potential sampling strategies for the study:

![Figure 8.9 – RCT sampling strategy suggestion from ChatGPT](media/file139.png)

Figure 8.9 – RCT sampling strategy suggestion from ChatGPT

Note that, in a real-world scenario, it is always a good practice to ask AI tools to generate more options with explanations behind them, so that you can make a reasoned decision. For this example, let’s go ahead with what ChatGPT suggested to us, which also includes suggestions about the population of interest and sample size.

*   **Data analysis**: ChatGPT can assist the researcher in determining the appropriate statistical tests to analyze the data collected from the study, such as ANOVA, t-tests, or regression analysis.

![Figure 8.10 – ChatGPT suggests a statistical test for a given study](media/file140.png)

Figure 8.10 – ChatGPT suggests a statistical test for a given study

Everything suggested by ChatGPT is coherent and finds confirmation in papers about how to conduct a statistical test. It was also able to identify that we are probably talking about a continuous variable (that is, scores) so that we know that all the information ahead is based on this assumption. In case we want to have discrete scores, we might adjust the prompt by adding this information, and ChatGPT will then suggest a different approach.

The fact that ChatGPT specifies assumptions and explains its reasoning is key to making safe decisions based on its input.

In conclusion, ChatGPT can be a valuable tool for researchers when designing experiments. By utilizing its **natural language processing** (**NLP**) capabilities and vast knowledge base, ChatGPT can help researchers select appropriate study designs, determine sampling techniques, identify variables and learning outcomes, and even suggest statistical tests to analyze the data.

In the next section, we are going to move forward in exploring how ChatGPT can support researchers, focusing on bibliography generation.

## Generating and formatting a bibliography

ChatGPT can support researchers in bibliography generation by providing automated citation and reference tools. These tools can generate accurate citations and references for a wide range of sources, including books, articles, websites, and more. ChatGPT knows various citation styles, such as APA, MLA, Chicago, and Harvard, allowing researchers to select the appropriate style for their work. Additionally, ChatGPT can also suggest relevant sources based on the researcher’s input, helping to streamline the research process and ensure that all necessary sources are included in the bibliography. By utilizing these tools, researchers can save time and ensure that their bibliography is accurate and comprehensive.

Let’s consider the following example. Let’s say we finalized a research paper titled, The Impact of Technology on Workplace Productivity: An Empirical Study. During the research and writing process, we collected the following references to papers, websites, videos, and other sources that we need to include in the bibliography (in order, three research papers, one YouTube video, and one website):

*   *The second machine age: Work, progress, and prosperity in a time of brilliant technologies*. Brynjolfsson, 2014. https://psycnet.apa.org/record/2014-07087-000
*   *The Impact of Technostress on Role Stress and Productivity. Tarafdar*, 2014\. Pages 301-328. https://www.tandfonline.com/doi/abs/10.2753/MIS0742-1222240109
*   *The big debate about the future of work, explained*”. Vox. https://www.youtube.com/ watch?v=TUmyygCMMGA

Obviously, we cannot have the preceding list in our research paper; we need proper formatting for it. To do so, we can provide ChatGPT with the raw list of references and ask it to re-generate it with the specific format, for example, APA style, the official style of the **American Psychological Association** (**APA**), commonly used as reference format style in education, psychology, and social sciences.

Let’s see how ChatGPT works with that:

![Figure 8.11 – A list of references generated in APA format by ChatGPT](media/file141.png)

Figure 8.11 – A list of references generated in APA format by ChatGPT

Note that I specified not to add details in case ChatGPT doesn’t know them. Indeed, I noticed that sometimes ChatGPT was adding the month and day of publication, making some mistakes.

Other interesting assistance ChatGPT can provide is that of suggesting potential reference papers we might want to quote. We’ve already seen in the first paragraph of this chapter how ChatGPT is able to brainstorm relevant literature before the writing process; however, once the paper is done, we might have forgotten to quote relevant literature, or even not be aware of having quoted someone else’s work.

ChatGPT can be a great assistant in brainstorming possible references we might have missed. Let’s consider once more our paper, focused on the research question “In what ways do social media interactions and online support communities affect the mental well-being of individuals with chronic mental health conditions?”. Let’s say that we set the following title and abstracts: which has the following abstract:

**Title***: The Impact of Social Media and Online Support Communities on Mental Well-being in Individuals with Chronic Mental Health Conditions*

**Abstract***: This study explores the effects of social media interactions and online support communities on the mental well-being of individuals with chronic mental health conditions. By examining various online platforms, the research aims to identify both positive and negative impacts. The study utilizes qualitative and quantitative methods, including surveys and interviews, to assess changes in anxiety, depression, and overall life satisfaction. Preliminary findings suggest that while online support can offer valuable social connections and emotional support, excessive use and negative interactions may exacerbate mental health issues.*

Let’s ask ChatGPT to list all the possible references that might be related to this kind of research:

![Figure 8.12 – List of references related to the provided abstract](media/file142.png)

Figure 8.12 – List of references related to the provided abstract

You can repeat this process with other sections of your paper also, to make sure you are not missing any relevant references to include in your bibliography.

Once you have your study ready, you will probably need to present it with an elevator pitch. In the next section, we will see how ChatGPT can also support this task.

## Generating a presentation of the study

The last mile of a research study is often that of being presented to various audiences. This might involve preparing a slide deck, pitch, or webinar where the researcher needs to address different kinds of audiences.

Let’s say, for example, that our study *The Impact of Social Media and Online Support Communities on Mental Well-being in Individuals with Chronic Mental Health Conditions* is meant for a master’s degree thesis discussion. In that case, we can ask ChatGPT to produce a pitch structure that is meant to last 15 minutes and adheres to the scientific method. Let’s see what kind of results are produced (as context, I’m referring to the abstract of the previous paragraph):

![Figure 8.13 – Thesis discussion generated by ChatGPT](media/file143.png)

Figure 8.13 – Thesis discussion generated by ChatGPT

That was impressive! Back in my university days, it would have been useful to have such a tool to assist me in my discussion design.

Starting from this structure, we can also ask ChatGPT to generate a slide deck as a visual for our thesis discussion.

Let’s proceed with this request:

![Figure 8.14 – Slide deck structure based on a discussion pitch](media/file144.png)

Figure 8.14 – Slide deck structure based on a discussion pitch

Finally, let’s imagine that our thesis discussion was outstanding to the point that it might be selected for receiving research funds in order to keep investigating the topic. Now we need an elevator pitch to convince the funding committee. Let’s ask for some support from ChatGPT:

![Figure 8.15 – Elevator pitch for the given thesis](media/file145.png)

Figure 8.15 – Elevator pitch for the given thesis

We can always adjust results and make them more aligned to what we are looking for, however, having structures and frameworks already available can save a lot of time and allows us to focus more on the technical content we want to bring.

Overall, ChatGPT is able to support an end-to-end journey in research, from literature collection and review to the generation of the final pitch of the study, and we’ve demonstrated how it can be a great AI assistant for researchers.

Furthermore, note that in the field of research, some tools that are different from ChatGPT, yet still powered by GPT models, have been developed recently. An example is Humanata.ai, an AI-powered tool that allows you to upload your documents and perform several actions on them, including summarization, instant Q&A, and new paper generation based on uploaded files.

This suggests how GPT-powered tools (including ChatGPT) are paving the way toward several innovations within the research domain.

## Summary

In this chapter, we explored the use of ChatGPT as a valuable tool for researchers. Through literature review, experiment design, bibliography generation and formatting, and presentation generation, ChatGPT can assist the researcher in speeding up those activities with low or zero added value, so that they can focus on relevant activities.

Note that we focused only on a small set of activities where ChatGPT can support researchers. There are many other activities within the domain of research that could benefit from the support of ChatGPT, among which we can mention data collection, study participant recruitment, research networking, public engagement, and many others.

Researchers who incorporate this tool into their work can benefit from its versatility and time-saving features, ultimately leading to more impactful research outcomes.

However, it is important to keep in mind that ChatGPT is only a tool and should be used in conjunction with expert knowledge and judgment. As with any research project, careful consideration of the research question and study design is necessary to ensure the validity and reliability of the results.

With this chapter, we also close Part 2 of this book, which focused on the wide range of scenarios and domains you can leverage ChatGPT for. However, we mainly focused on individual or small team usage, from personal productivity to research assistance. Starting from Part 3, we will elevate the conversation to how large organizations can leverage the same generative AI behind ChatGPT for enterprise-scale projects, using OpenAI model APIs available on the Microsoft Azure cloud.

## References

*   [https://arxiv.org/abs/2312.11914](https://arxiv.org/abs/2312.11914)
*   [https://arxiv.org/abs/2101.07714](https://arxiv.org/abs/2101.07714)
*   [https://arxiv.org/abs/2101.07714](https://arxiv.org/abs/2101.07714)
*   [https://psycnet.apa.org/record/2014-07087-000](https://psycnet.apa.org/record/2014-07087-000)
*   [https://www.tandfonline.com/doi/abs/10.2753/MIS0742-1222240109](https://www.tandfonline.com/doi/abs/10.2753/MIS0742-1222240109)
*   [https://www.youtube.com/watch?v=TUmyygCMMGA](https://www.youtube.com/watch?v=TUmyygCMMGA)

# 9 Exploring GPTs

## Join our book community on Discord

![](media/file0.png)

[https://packt.link/EarlyAccess](https://packt.link/EarlyAccess)

In last Chapters, we saw several examples of how to leverage ChaGPT for various activities, from personal productivity to marketing, from research to software development. For each of these scenarios, we always faced a similar situation: we started with a general purpose model as ChatGPT, to then ask very specific questions or providing specific references to tailor it to our specific need.

However, sometimes this might not be enough if our aim is that of obtaining an extremely specialized model for our own purposes. That’s why we might need to build “purpose-specific ChatGPT” and, luckily enough, OpenAI itself developed a no-code platform to build those customized assistants, that are called GPTs.

In this Chapter, we are going through the GPTs features, capabilities and real-world applications, covering the same use cases we saw in previous Chapters, so that you can see the difference in the output’s quality. Plus, we will also see how to publish your GPT and make it a production application not only for yourself but also for company or for everyone else.

By the end of this Chapter, you will be able to:

*   Understand what a GPT is and what kind of tasks it can achieve
*   Build your own GPT without writing a line of code
*   Publish your GPT and integrate it with external systems

Let’s start from some basic definitions and then jump into the practice.

## Technical requirements

ChatGPT Plus

## What are GPTs?

In November 2023, OpenAI introduced GPTs, specialized versions of ChatGPT designed to enhance productivity and cater to specific tasks and needs. Unlike the general-purpose ChatGPT, these custom versions, called GPTs, allow users to create tailored AI models without any coding knowledge.

> It is important to anticipate a taxonomy consideration that will be relevant throughout this chapter. When we mention the word GPT, there are two main definitions that you found - and will find – in this book:

*   The first one refers to the proper “Generative Pre-trained Transformer” (GPT) model architecture behind OpenAI’s language models. We already mentioned this architecture in Part 1, and we know that this is the framework behind ChatGPT itself.
*   The second one refers, in a more generic way, to the specialized assistants that OpenAI allows users to create in a no-code approach. With GPTs, OpenAI refers to specialized versions of ChatGPT and, in this context, a single GPT refers to one assistant that you created leveraging the GPTs platform (available to all users with ChatGPT Plus).

Over this chapter, whenever you will read GPT or GPTs, keep in mind that we are using the second definition.

The idea of GPTs is like that of AI Agents. In fact, with GPTs, we are building entities powered by LLMs, with specific instructions and provided with both custom knowledge base and a set of tools or plug-ins to interact with the surrounding environment.

Let’s see more in details all these components. First of all, you can have a comprehensive look at all existing GPTs that have been publicly published. To do so, you can navigate through the following page: [https://chatgpt.com/gpts](https://chatgpt.com/gpts).

![Figure 1: Landing page of GPTs.](media/file147.png)

Figure 1: Landing page of GPTs.

As you can see from the preceding picture, this is a proper marketplace of all existing GPTs. You can explore it by category (box 1) or by explaining what you are looking for in the search bar (box 2).

Then, to create your own GPT, you can navigate through the https://chatgpt.com/gpts/editor page and login with your OpenAI account. Alternatively, you can go to ChatGPT and select on the top-left corner of your page “Explore GPTs”, then “Create” (as illustrated in box 3 in the preceding picture).

Once in the editor, you will be asked to configure your GPT, while on the right-hand side you have the possibility to test it in real time.

![Figure 2: Editor page to create your GPT from scratch.](media/file148.png)

Figure 2: Editor page to create your GPT from scratch.

Let’s explore all the components:

*   **Name**: the name you give to your GPT.
*   **Description**: the description of the capabilities of your GPT. This is extremely important especially if you are going to publish your GPT in the marketplace, so that other users can find it easily (as we previously mentioned, GPTs can be searched via natural language in the Search GPTs bar).
*   **Instructions**: this is the metaprompt of your GPT, so that set of instructions in natural language that tailor the assistant to your specific need and that the final user doesn’t see.
*   **Conversation Starters**: this is a set of sample prompts that users could start with to interact with the GPT and gain confidence.
*   **Knowledge**: this refers to the custom documentation we can ground the model on. When we upload documents here, our GPT will be able to navigate through it with the Retrieval Augmented Generation pattern, so that we can provide additional knowledge or even limit our assistant’s responses to the custom knowledge base, depending on our needs.
*   **Capabilities**: these refer to a set of built-in plug-ins we can provide the GPT with, without writing a single line of code. You can see from the above picture that there are three plug-ins out of the box:
    *   Web browsing for searching the web and retrieve up to date information.
    *   DALL-E 3 to generate illustrations.
    *   Code Interpreter and Data Analysis to execute code in a sandboxed Python environment and interact with analytical files (like spreadsheets).
*   Actions: actions can be seen as plug-ins as well, however they differ from capabilities since they are not built-in, but rather specified by the GPT developer. For example, you might generate an action

> In the context of actions, you can be supported by a specialized GPT which is natively integrated in the configuration pane if you click on “Add Actions” and then “Get help from ActionsGPT”.

![](media/file149.png)![Figure 3: Example on how to get support from ActionsGPT](media/file150.png)

Figure 3: Example on how to get support from ActionsGPT

By doing so, you will be prompted to the chat interface of your GPT:

![Figure 4: Landing page of ActionsGPT.](media/file151.png)

Figure 4: Landing page of ActionsGPT.

It’s amazing to see how we are witnessing “GPTs inside GPTs” approaches, don’t you think?

In addition to the standard configuration page, there is another option to build your GPT in a more “conversational way”. In fact, you can switch to the tab “Create” and explain in natural language what you want to achieve with your GPT:

![Figure 5: Creating a GPT starting from a conversation in natural language.](media/file152.png)

Figure 5: Creating a GPT starting from a conversation in natural language.

We will see both methods – standard configuration and conversational configuration – in the practical paragraphs of this chapter.

Now that we know what a GPT is, let’s see how to create one. Over the next sections, we will create 5 different GPTs:

*   The first four will specialize in the four domains we already covered with the general-purpose ChatGPT – personal productivity, code development, marketing and research. The idea is that to compare the overall efficiency and accuracy of a domain-specific GPT versus a general-purpose ChatGPT.
*   The fifth one will cover a new domain, related to artistic creativity. We will leverage the built-in DALL-E plug-in as well as other plug-ins developed by design companies (like Canvas).

Let’s start with boosting our personal productivity with a specialized GPT.

## Personal Assistant

In this scenario, we are going to build a GPT to boost our gym workouts. To do so, we will leverage the built-in plug-ins. Plus, we will add some relevant documents so that the assistant will be grounded on specific knowledge base.

My goal is that of having an assistant that can build for me workout plans according to by fitness ambition, availability, gender and age, preferences and so forth. I also want my assistant to provide me clear explanations on the reason behind its proposals.

To accomplish all of this, I want to make sure that my assistant will:

*   Ask me specific questions it needs to design the best workout for me
*   Provide me with relevant information and sources regarding its responses
*   Consider my feedback, yet it will be able to maintain its idea if it believes it is correct for me.
*   Not accommodate my requests if not reasonable or risky for my health.

Let’s see how to create our GPT, following all the configuration steps mentioned in the previous section:

1.  **Name**: I’ll call my assistant WorkoutGPT.
2.  **Description**: this is the description I set: “Workout Assistant that helps user designing their workout plan according to their needs.”
3.  **Instructions**: here we start with the real core of our GPT. This is the set of instructions I provided the GPT with:

*“You are a workout AI assistant that help users creating their workout plans, depending on their needs.*

*Before generating the plan, make sure to ask the following questions:*

*- Fitness goal and time expectation*

*- Age and gender*

*- Fitness level*

*- Availability for the workout*

*- All other elements that you need in order to define a proper workout plan (e.g. equipment, potential injuries...)*

*If needed, use the provided documents to enrich your responses.*

*If the user suggests you something that it's not plausible for its goal, remain stick to your assumption, explaining the reason behind politely.*

*If the user asks you something that might be risky for his/her health, politely suggest to take it easier and an alternative approach, explaining the reason behind.”*

1.  **Conversation Starters**: here I set three samples of different workouts:
    *   I want to train for a marathon in 6 months.
    *   Generate a 30’ HIIT workout plan without any equipment.
    *   Generate a 45’ weightlifting workout plan with dumbbells only.
2.  **Knowledge**: here I uploaded the standard National Strength and Conditioning Association (NSCA) training load chart, a tool used to help athletes and coaches determine the appropriate training load for different exercises and training sessions. It looks like following:

![Figure 6: NSCA training load chart. Source: https://www.nsca.com/contentassets/61d813865e264c6e852cadfe247eae52/nsca_training_load_chart.pdf](media/file153.png)

Figure 6: NSCA training load chart. Source: https://www.nsca.com/contentassets/61d813865e264c6e852cadfe247eae52/nsca_training_load_chart.pdf

I almost forgot a key step – the illustration! It might seem superficial yet having an icon for your GPT make it more attractive, especially if you are planning to publish it to all users. Luckily enough, we have DALL-E directly integrated in the configuration pane:

![Figure 7: Example on how to set your GPT icon.](media/file154.png)

Figure 7: Example on how to set your GPT icon.

This is how the configuration looks like:

![Figure 8: WorkoutGPT configuration page.](media/file155.png)

Figure 8: WorkoutGPT configuration page.

Great! Let’s now see some sample conversations.

To start with, let’s pick the first conversation starter about the marathon training:

![Figure 9: Example of questions asked from WorkoutGPT to assess user’s overall goals and fitness level.](media/file156.png)

Figure 9: Example of questions asked from WorkoutGPT to assess user’s overall goals and fitness level.

As you can see, our WorkoutGPT immediately asks us the required information to proceed with the plan. Once provided the above information, my assistant generated the following 24-weeks plan (I’ll share here the first 4 weeks):

![Figure 10: Example of workout table for marathon training generated by WorkoutGPT.](media/file157.png)

Figure 10: Example of workout table for marathon training generated by WorkoutGPT.

Plus, it specifies the Friday strength training as follows:

![Figure 11: Example of strength training generated by WorkoutGPT.](media/file158.png)

Figure 11: Example of strength training generated by WorkoutGPT.

Let’s focus on the strength training. I want to better understand how to calibrate the weights. The following illustration shows the first extract of the response:

![Figure 12: Example of WorkoutGPT explaining how to determine the weight to lift in a strength workout.](media/file159.png)

Figure 12: Example of WorkoutGPT explaining how to determine the weight to lift in a strength workout.

In the same response, the Assistant also referenced the NCSA training load chart provided as knowledge base:

![Figure 13: Example of WorkoutGPT retrieving information from custom knowledge base about NSCA training load chart.](media/file160.png)

Figure 13: Example of WorkoutGPT retrieving information from custom knowledge base about NSCA training load chart.

Now I want to challenge my WorkoutGPT, asking for something that might be harmful for me. For example, preparing a marathon with no experience in one month is definitely a terrible idea. Let’s see what my assistant’s thoughts about it are, once I provide it with the list of answers to its starter questions:

![Figure 14: Example of WorkoutGPT gently nudging the user to pivot its goal and expectations given the risks associated with the request.](media/file161.png)

Figure 14: Example of WorkoutGPT gently nudging the user to pivot its goal and expectations given the risks associated with the request.

As you can see, my WorkoutGPT is nudging me to change my approach to the race. While it still provides me with a running workout plan for 3 weeks (here the output is truncated), it is not aimed at running a marathon under 3h and 15m, but rather on building endurance and strength.

Note that, if we had asked the same thing to the general purpose ChatGPT, it would have responded as follows:

![Figure 15: Example of ChatGPT accomplishing user’s task despite the associated risk.](media/file162.png)

Figure 15: Example of ChatGPT accomplishing user’s task despite the associated risk.

Note how ChatGPT, despite being vocal in disclosing its concerns, it's still accommodating my request, providing me with a plan to run a full marathon. This could encourage me – a reckless beginner who thinks that running a marathon is a joke – to dive into this dumb adventure, with serious consequences for me health.

Overall, GPTs allow you to be extremely specific about how your assistant should behave and what it should avoid saying or accommodate.

## Code Assistant

In this section, I want to develop an assistant that is tailored towards data science projects. More specifically, I want my assistant to be able to:

*   Provide clear guidance on how to set up a data science experiment, given the user’s task
*   Generate the Python code needed to run the experiment
*   Leverage the code interpreter capabilities to run and check the code
*   Push the final code to the GitHub repository.

Let’s see how to do that step by step.

1.  Setting the instructions:

*This GPT is a data science assistant that helps users set up and run data science experiments. It provides clear guidance on how to define and organize tasks, generate Python code needed for the experiments, and leverages code interpreter capabilities to run and check the code. The GPT will take the user's input and provide step-by-step instructions to structure the experiment, create the necessary scripts, and execute the code.*

*The GPT will execute the code to see whether it works. Once the final code is accepted by the user, it can be pushed to the GitHub repo as a .ipynb file.*

1.  Setting the (optional) conversation starters:

*How do I set up a classification experiment?*

*Generate Python code for a random forest model.*

*Can you help me preprocess this dataset?*

*Run this code and check for errors.*

1.  Enabling the Code Interpreter & Data Analysis plugin:

    ![Figure 16: Enabling the code Interpreter & Data analysis plugin.](media/file163.png)

    Figure 16: Enabling the code Interpreter & Data analysis plugin.

2.  Creating an Action to communicate with GitHub. To do so, we need to click on “Create new action” and define the required Schema.

> To set the schema of a ChatGPT action using an OpenAPI 3.1.0 specification, you define the structure of the data (requests and responses) that the action will handle. This involves specifying the schema property under content in your API paths. The schema outlines the expected data types, required fields, and possible values for your request and response bodies.

Steps to Define a Schema

1.  Identify the Data Structure: Determine the type of data the action will handle (e.g., JSON objects, arrays).
2.  Define Properties: Under the schema, specify the properties, their types, and any constraints. For example, if the action requires a username and an email, you'd define these under properties.
3.  Set Required Fields: Use the required array to specify which fields must be provided.
4.  Apply to Paths: Place the schema under the appropriate HTTP method in your paths (e.g., POST, GET).

Let’s consider the following example:

```
openapi: 3.1.0
info:
  title: ChatGPT Action API
  version: 1.0.0
paths:
  /perform-action:
    post:
      operationId: performAction
      summary: Perform a specific action with given inputs.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                action:
                  type: string
                  description: The action to perform
                parameters:
                  type: object
                  description: Parameters for the action
                  properties:
                    userId:
                      type: string
                    content:
                      type: string
              required:
                - action
                - parameters
      responses:
        '200':
          description: Successful action response
          content:
            application/json:
              schema:
                type: object
                properties:
                  success:
                    type: boolean
                  message:
                    type: string
```

In this example, the `requestBody` for the POST method defines a schema with action and parameters as required fields. The response is also defined, specifying the structure of the data returned after the action is performed.

This is how the schema looks like:

![Figure 17: Configuration of the GitHub Action schema.](media/file164.png)

Figure 17: Configuration of the GitHub Action schema.

This is the full schema I used:

```
openapi: 3.1.0
info:
  title: GitHub API
  description: API for interacting with GitHub, including pushing code to a repository.
  version: 1.0.0
servers:
  - url: https://api.github.com
    description: GitHub API server
paths:
  /repos/{owner}/{repository}/contents/{path}:
    put:
      operationId: updateFileContents
      summary: Create or update a file in a GitHub repository
      description: Use this endpoint to create a new file or update an existing file in a repository.
      parameters:
        - name: owner
          in: path
          required: true
          description: The owner of the repository.
          schema:
            type: string
        - name: repo
          in: path
          required: true
          description: The name of the repository.
          schema:
            type: string
        - name: path
          in: path
          required: true
          description: The file path in the repository.
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                message:
                  type: string
                  description: Commit message for the file change.
                content:
                  type: string
                  description: The new file content, Base64 encoded.
                sha:
                  type: string
                  description: SHA of the file being replaced, if updating.
                branch:
                  type: string
                  description: The branch where the file should be created or updated.
                committer:
                  type: object
                  properties:
                    name:
                      type: string
                    email:
                      type: string
              required:
                - message
                - content
      responses:
        '200':
          description: Successful file update or creation.
        '201':
          description: Successful file creation.
        '422':
          description: Validation failed or the file already exists.
      security:
        - bearerAuth: []
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: token
  schemas:  # This subsection contains schema definitions (if needed).
    ExampleSchema:  # Example object schema
      type: object
      properties:
        exampleProperty:
          type: string
```

To allow the Action to communicate with my repo, I created an access token in my GitHub profile under Settings>Developer Settings>Personal access token.

You can then test the connection as follows:

![Figure 18: Example of testing the action.](media/file165.png)

Figure 18: Example of testing the action.

Let’s now navigate through the Repo and see whether it worked:

![Figure 19: Example of file uploaded via the GPT Action.](media/file166.png)

Figure 19: Example of file uploaded via the GPT Action.

Great! As you can see, we now have a new file with the pre-defined content.

Let’s now create it and test it:

*   Let’s start with a simple question about how to set up a classification experiment (truncated output):

    ![Figure 20: Example of the DataScience Assistant providing guidance on how to tackle a classification problem.](media/file167.png)

    Figure 20: Example of the DataScience Assistant providing guidance on how to tackle a classification problem.

*   Following these instructions, we are now ready to set up our experiment. We want to tackle the well-known Titanic passengers’ survival prediction. We will upload our dataset (you can find many free versions online, I downloaded mine at [https://github.com/datasciencedojo/datasets/blob/master/titanic.csv](https://github.com/datasciencedojo/datasets/blob/master/titanic.csv)) and leverage a Logistic Regression model.

    > The Titanic survival prediction task is a classic problem in data science and machine learning. The goal is to predict whether a passenger on the Titanic would survive or not, based on various features like age, gender, passenger class, fare, and more. This task is typically used to teach classification techniques, where the model is trained on a labeled dataset and then used to predict outcomes on new data. The challenge involves selecting relevant features, handling missing data, and choosing the appropriate machine learning algorithm to achieve accurate predictions.

*   This is my query:

![Figure 21: Example of DataScience Assistant designing the Titanic survival experiment.](media/file168.png)

Figure 21: Example of DataScience Assistant designing the Titanic survival experiment.

I will now share some screenshots of the model’s response per each step:

1.  Load the data:

    ![Figure 22: Example of DataScience Assistant executing the step 1 of the experiment.](media/file169.png)

    Figure 22: Example of DataScience Assistant executing the step 1 of the experiment.

2.  Explore and clean the data:

![Figure 23: Example of DataScience Assistant exploring and cleaning data.](media/file170.png)

Figure 23: Example of DataScience Assistant exploring and cleaning data.

Note that, when you see the symbol [**>_**], it means that the code interpreter plugin has been triggered. You can click on it to see the executed code:

![Figure 24: Example of DataScience Assistant’s generated code leveraging the code interpreter plugin.](media/file171.png)

Figure 24: Example of DataScience Assistant’s generated code leveraging the code interpreter plugin.

1.  Feature Engineering:

    ![Figure 25: Example of DataScience Assistant doing feature engineering.](media/file172.png)

    Figure 25: Example of DataScience Assistant doing feature engineering.

2.  Data splitting:

    ![Figure 26: Example of DataScience Assistant splitting the dataset into training and test set.](media/file173.png)

    Figure 26: Example of DataScience Assistant splitting the dataset into training and test set.

3.  Train the model:

    ![Figure 27: Example of DataScience Assistant training a Logistic Regression model.](media/file174.png)

    Figure 27: Example of DataScience Assistant training a Logistic Regression model.

4.  Evaluate the model:

![Figure 28: Example of DataScience Assistant evaluating the model’s output.](media/file175.png)

Figure 28: Example of DataScience Assistant evaluating the model’s output.

That’s pretty cool! It is extremely accurate and can save a lot of time. Plus, if you think about data scientists working in large enterprises on many projects, having a similar assistant can also help in following fixed standards among projects, so that the maintenance is aligned across teams.

The last thing that we will ask our GPT is to push the code on our Repo. Let’s see how it works:

![Figure 29 Example of DataScience Assistant leveraging the Action to push the code on GitHub.](media/file176.png)

Figure 29 Example of DataScience Assistant leveraging the Action to push the code on GitHub.

If we click on the link, we can see that the file has been successfully uploaded:

![Figure 30: File uploaded via DataScience Assistant Action.](media/file177.png)

Figure 30: File uploaded via DataScience Assistant Action.

And it worked! Again, this is an example of how GPTs can speed up developers and data scientists’ productivity. By designing data science experiments following a common framework and enabling push workflows without the need of switching to GitHub can save previous time, so that data scientists can focus on the core aspects of their projects.

## Marketing Assistant

As we saw in Chapter 6 – Mastering Marketing with ChatGPT, AI assistant can be extremely valuable when it comes to this domain. In fact, generating text content – like social media posts, blog articles or marketing campaigns – is probably the activity in which these models perform at their best.

> A copywriter is a professional who specializes in writing persuasive and engaging content, often for marketing and advertising purposes. Their work typically includes writing promotional materials such as advertisements, brochures, websites, emails, social media posts, and other forms of content designed to persuade an audience to take a specific action, like making a purchase or subscribing to a service.

In this section, we are going to create a Copywrite assistant which is tailored to this kind of activity. To do so, I named my assistant Copywrite Companion, and I set the following configuration components:

*   Instructions:

*Copywrite Companion is a versatile assistant designed to help users with various writing tasks. It specializes in generating product sheets based on user input, including text and images, crafting email campaigns for newsletters or promotional purposes, creating engaging visuals for new products, and writing tailored social media posts for different platforms. It ensures content is persuasive, engaging, and aligned with the target audience, aiming to increase engagement and promote products or services effectively. The assistant adapts its style to the user's needs and strives for creativity, clarity, and relevance in all outputs. It communicates in a casual, friendly tone, making interactions feel approachable and easygoing.*

*   Conversation starters:

*Can you write a product description for my new product?*

*I need a catchy headline for an article about climbing*

*Give me a few ideas for a blog post about running*

*   Capabilities:

![Figure 31: Copywrite Companion’s enabled plugins.](media/file178.png)

Figure 31: Copywrite Companion’s enabled plugins.

Let’s see it in action:

*   I’ll first ask it to write a product sheet starting from a picture provided (output truncated):

    ![Figure 32: Example of Copywrite Companion generating a product description.](media/file179.png)

    Figure 32: Example of Copywrite Companion generating a product description.

*   As copywriter, we might want to insert this set of information into more structured repository, like an excel file. Let’s ask the assistant to do so, leveraging its code interpreter plugin:

    ![Figure 33: Example of Copywrite Companion converting its previous response into an excel file, leveraging the code interpreter plugin.](media/file180.png)

    Figure 33: Example of Copywrite Companion converting its previous response into an excel file, leveraging the code interpreter plugin.

*   And this is the final result:

![Figure 34: Excel file generated by Copywrite Companion](media/file181.png)

Figure 34: Excel file generated by Copywrite Companion

Let’s now ask the assistant to generate an Instagram post to sponsor our shoes:

![Figure 35: Example of Copywrite Companion generating an Instagram post.](media/file182.png)

Figure 35: Example of Copywrite Companion generating an Instagram post.

As you can see, the assistant also proposed an image description to leverage the DALL-E plugin. Since the description makes sense to me, I’ll go ahead and ask it to generate it:

![Figure 36 Example of Copywrite Companion generating an image leveraging the DALL-E plugin.](media/file183.png)

Figure 36 Example of Copywrite Companion generating an image leveraging the DALL-E plugin.

Finally, I want to have an idea of how big competitor brands – like Nike and Adidas – are doing their marketing activities. To do so, I’ll ask my assistant to gather some evidence from the web:

![Figure 37: Example of Copywrite Companion doing a competitive analysis leveraging the web browsing plugin.](media/file184.png)

Figure 37: Example of Copywrite Companion doing a competitive analysis leveraging the web browsing plugin.

As you can see, our companion correctly leveraged the web browsing plugin to retrieve the required information. Plus, it gave us powerful insight into which leverages the two competitor companies are investing, so that we might think (or ask our companion) about unique differentiators that could let our brand outstand in a competitive market.

We could also do a step ahead and ask for more specific insights about the competition by leveraging the code interpreter and data analysis plugin. Let’s say, for example, that we gathered an excel sheet with the following structure:

![Figure 38: Competitive analysis on an Excel sheet](media/file185.png)

Figure 38: Competitive analysis on an Excel sheet

And now we want to generate some visuals out of it. Let’s ask our Copywriter Companion to do so:

![Figure 39: Example of bar chart and scatter plot generated by Copywrite companion.](media/file186.png)

Figure 39: Example of bar chart and scatter plot generated by Copywrite companion.

![Figure 40: Example of line chart generated by Copywrite companion.](media/file187.png)

Figure 40: Example of line chart generated by Copywrite companion.

Including, as requested, the executive report:

![Figure 41: Example of an Executive report generated by Copywrite companion.](media/file188.png)

Figure 41: Example of an Executive report generated by Copywrite companion.

Overall, tailoring ChatGPT for marketing activities can be extremely useful when it comes to generating new content, designing marketing strategies and doing competitive analysis on the web.

## Research Assistant

In this scenario, we are going focus on research once more, yet this time with a particular focus on papers retrieval. More specifically, we want our assistant to be able to do the following:

*   Retrieving information from custom knowledge base that we provide
*   Integrating the custom documents with papers only from Arxiv, enabling the web plugin for this task
*   Retrieving from a Database (in our case, it will be hosted in Notion) existing ongoing work from other researchers, so that we don’t risk elaborating an essay which is already covered by someone else.

Let’s see all the steps.

1.  Upload custom documents. For this purpose, I’ll use two papers about Image Classification in Machine Learning: “*CIFAKE: image classification and explainable identification of ai-generated synthetic images*” from J. Bird et al., and “*A Comprehensive Study of Vision Transformers in Image Classification Tasks*” from Khalis et al.

You can upload them in the proper section in the configuration pane:

![Figure 42: Uploading custom documents in the configuration pane](media/file189.png)

Figure 42: Uploading custom documents in the configuration pane

1.  Integrating custom documents with web references. To do so, we need to enable the web plugin:

![Figure 43: Enabling the web browsing plugin in the configuration pane](media/file190.png)

Figure 43: Enabling the web browsing plugin in the configuration pane

Plus, we also need to specify the assistant to only navigate through the Arxiv archive. We will see how to specify that in the set of instructions we will create.

1.  Retrieve information from a Notion Database. Here, the idea is that, as researchers, we might come up with ideas that are already being studied and developed by other fellow colleagues. Imagine that we keep track of all the ongoing research in a Notion Database that has the following structure:

![Figure 44: Notion Database structure.](media/file191.png)

Figure 44: Notion Database structure.

To do so, we need to create a GPT actions. To do so, there are two steps to follow:

1.  In your Notion workspace, you need to create a new connection marked as internal, along with a new Internal Integration Secret (or API key) will be created. You can call this connection “chatgpt” or similar.
2.  In your GPT configuration pane, you need to set a new Action schema. Since in our case we need to query a specific Database, the schema will look like the following:
3.  When it comes to authentication, you can click on "Authentication" and choose "API Key". Enter in the information below.
    *   API Key: Use Internal Integration Secret from your newly created connection in Notion
    *   Auth Type: Bearer

![Figure 45: Notion Action Schema.](media/file192.png)

Figure 45: Notion Action Schema.

This is how the whole schema looks like:

```
openapi: 3.1.0
info:
  title: Notion API
  description: API for interacting with Notion's pages, databases, and users.
  version: 1.0.0
servers:
  - url: https://api.notion.com/v1
    description: Main Notion API server
paths:
  /databases/{database_id}/query:
    post:
      operationId: queryDatabase
      summary: Query a database
      parameters:
        - name: database_id
          in: path
          required: true
          schema:
            type: string
        - name: Notion-Version
          in: header
          required: true
          schema:
            type: string
          example: 2022-06-28
          constant: 2022-06-28
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                filter:
                  type: object
                sorts:
                  type: array
                  items:
                    type: object
                start_cursor:
                  type: string
                page_size:
                  type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  object:
                    type: string
                  results:
                    type: array
                    items:
                      type: object
                  next_cursor:
                    type: string
                  has_more:
                    type: boolean
```

Great, now that we have all our ingredients, we need to set our system message and, optionally, our conversation starters. In this scenario, I set the following instructions:

*You are an AI research assistant tasked with helping researchers by leveraging a variety of tools and resources. Your primary responsibilities include retrieving, integrating, and cross-referencing information from custom knowledge bases, academic papers on Arxiv, and ongoing research projects stored in a Notion database. Follow these guidelines to ensure your assistance is accurate, comprehensive, and avoids redundancy:*

1.  *Always answer based on the provided documents*
2.  *If you feel it is needed, extend your search on the web. the ONLY ONE SITE you can navigate through is Arxiv.*
3.  *If asked by the user, check whether the topic is already covered in the Notion DB.*

And the following conversation starters:

1.  *How do VGGNet, ResNet, and Inception differ in accuracy and efficiency for image classification?*
2.  *What challenges do image classification models face, and how do techniques like data augmentation and transfer learning help?*
3.  *How does CNN depth affect image classification, and how do ResNet's residual connections help?*

The final product looks like so:

![Figure 46: Landing page of our ResearchGPT](media/file193.png)

Figure 46: Landing page of our ResearchGPT

Let’s test it:

1.  I’ll first ask a generic question, that will be addressed with the provided papers:

    ![Figure 47: Example of ResearchGPT retrieving knowledge from provided documents](media/file194.png)

    Figure 47: Example of ResearchGPT retrieving knowledge from provided documents

2.  Then, I want to see whether the topic is already covered:

    ![Figure 48: Example of ResearchGPT talking to Notion with the pre-defined action.](media/file195.png)

    Figure 48: Example of ResearchGPT talking to Notion with the pre-defined action.

3.  Finally, I want to integrate the topic to make it unique:

![Figure 49: Example of ResearchGPT using the web browser plugin.](media/file196.png)

Figure 49: Example of ResearchGPT using the web browser plugin.

As you can see, our Assistant could leverage all the tools we provided it with, calling the Notion DB when needed.

## Summary

In this Chapter, we explored how to get customized GPTs to address your specific goals. The possibility of tailoring ChatGPT opens a new landscape of scenarios, where highly specialized AI assistant becomes the everyday companion of professionals. Plus, OpenAI offered a no-code UI to create GPTs, so that all citizen developers can benefit not only from the extended marketplace of available solutions, but also from their own creations.

With the plugins extensibility and the custom knowledge base, custom GPTs can serve you for limitless tasks. Then, with the addition of powerful actions, they can also communicate with the surrounding environment and evolve from “mere” generation to automation.

With this Chapter, we also conclude Part 2 of this book, where we focused on practical applications of ChatGPT. Starting from next Chapter, we are going to cover more in details how large enterprises can leverage OpenAI models and embed them into their business processes. We will cover trending enterprise use cases and see practical implementation on how to embed OpenAI’s LLMs via REST API in your applications.

## References

*   [https://arxiv.org/abs/2312.11914](https://arxiv.org/abs/2312.11914)
*   [https://arxiv.org/abs/2101.07714](https://arxiv.org/abs/2101.07714)
*   [https://arxiv.org/abs/2101.07714](https://arxiv.org/abs/2101.07714)
*   [https://psycnet.apa.org/record/2014-07087-000](https://psycnet.apa.org/record/2014-07087-000)
*   [https://www.tandfonline.com/doi/abs/10.2753/MIS0742-1222240109](https://www.tandfonline.com/doi/abs/10.2753/MIS0742-1222240109)
*   [https://www.youtube.com/watch?v=TUmyygCMMGA](https://www.youtube.com/watch?v=TUmyygCMMGA)
*   [https://arxiv.org/pdf/2312.01232](https://arxiv.org/pdf/2312.01232)
*   [https://arxiv.org/pdf/2303.14126](https://arxiv.org/pdf/2303.14126)