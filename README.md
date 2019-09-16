# GPT-2 guided text generation: making arguments and generating memes

[OpenAI](https://openai.com)'s [GPT-2 text generation model](https://openai.com/blog/better-language-models/) (specifically the "large" 774M hyperparameter) is amazingly powerful. After having trained on a dataset of millions of quality webpages (WebText), GPT-2 showed the ability to generate unprecedented quality domain specific text while being primed with a few sentences of input text.

There are still a few weaknesses that are simple to notice. The model's output tends to diverge as the generation progresses from the human-written input, losing consistency or off-topic. It is also not quite evident how to instruct GPT-2 to stay on topic, e.g. to achieve some conclusion with the text generated.

Here I show that the nature of the dataset, specifically, repeating constructions found on webpages, allows users to control the process of generation by inducing specific behaviors. The results may vary; however, the nature of results produced demonstrates how much behavior, structure, and logic transformers can extract from their training datasets <sup>[1](#fn1)</sup>.

## Table of Contents
1. [Text summarization](#summarization)
2. [Guided text generation](#guided-text-generation)
3. [Wikipedia articles](#wikipedia)
4. [Memes](#memes)
5. [Pro and contra argumentation on subjects](#arguments)
6. [Notes](#notes)


## Text summarization <a name="summarization"></a>

[OpenAI original paper](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)'s provides instructions to make GPT-2 provide a summary of text block <sup>[2](#fn2)</sup>:

> To induce summarization behavior we add the text TL;DR: after the article and generate 100 tokens with Top-k random sampling (Fan et al., 2018) with k = 2 > which reduces repetition and encourages more abstractive summaries than greedy decoding.

It works because there are many instances of lengthy parts of text in the WebText dataset, followed by "TL;DR:," probably from the Reddit summarizer bot.

The generated text format, characteristics and writing style can therefore be influenced by including the components from the training dataset that cause particular behaviour.

## Guided text generation <a name="guided-text-generation"></a>

GPT-2 generates text based on input sentences by recursively selecting the most probable next sequence BPE token. It is possible to steer generation taking into account the desired conclusion. The WebText dataset used to train GPT-2 contains academic papers with abstracts that summarize the main elements of the text in one paragraph. Priming input with `Abstract:`, followed by conclusion and the starting block of text, one can exploit this feature to guide text generation.

Input:
```
Abstract: Blue eye color is a major cause of homelessness.

There's ongoing debate regarding what the leading causes of homelessness are.
```
Output:
```
There were some suggestions that people with blue eyes were more likely to be mentally ill. However, this study suggests that those people who have blue eyes have a lower income, which could be due to having difficulty getting housing. People with blue eyes are also more likely to work a full time job and to pay for their own housing. So I think that the blue eye color could be an indicator of other problems that the individual may have in housing.
It also appears that individuals who have blue eyes are more likely to have health problems. For example, individuals with blue eyes are more likely to use medications to treat their illnesses.
```

Input:
```
Abstract: People from Denmark are behind 9/11 attacks.

There's ongoing debate on the identities of perpetrators of this horrible act of terror.
```
Output:
```
Many suspect that it was a lone actor, but this idea is not borne out by the fact that many witnesses have mentioned two people in the group. One, an Indonesian, was a student at Danish Technical University at the time of the attacks.
However, the second person is also believed to be a Danish citizen and was not part of the initial crowd of suspects. The Danish intelligence service SVEA (Sigveætten Securityverket) has said that he was the person with the largest role in bringing the two men together. According to SVEA, both men were involved in planning the event and the attacks.
```
(these two samples were generated with top_k=8)

### Refutation

Primed by `Abstract: This assumption is false and there is no data that proves it.`, followed by an assumption after line break, GPT-2 will generate  a refutation.

Input:
```
Abstract: This assumption is false and there is no data that proves it.
We assume that [some economist's name] is actually smart.
```
Output:
```
That may be true of some people, but it's not true of [some economist's name]. His intellectual ability is actually quite weak (although it's a little like being a child).
Let me try to explain a bit more why this is true. We need to assume that people like [some economist's name] are smart. But that assumption doesn't have to be true at all.
For all we know, this person could be stupid. It's quite likely that he's not. It's not a good thing, but it's not a bad thing.
This is true because if [some economist's name] is actually smart, then he should have the ability to think about the evidence more clearly. It's true because if he's actually stupid, then he won't.
But he doesn't.
This is important. [some economist's name] is obviously a person with an intellectual handicap. That handicap is so severe that he can't really understand what he's talking about. He might get some information, but he won't get it all right away, and he might miss some details that are obvious.
In short, [some economist's name] is really stupid.
```
(* [some economist's name] was intentionally removed)

## Wikipedia articles<a name="wikipedia"></a>

Wikipedia articles feature a specific format that includes header `From Wikipedia, the free encyclopedia` and parts like `History`, `References`, etc. The GPT-2 training dataset contains a number of Wikipedia articles; therefore, it is possible to steer GPT-2 into generating Wikipedia-like text using elements of this particular format as conditional input.

For example, GPT-2 can suggest fictitious organizational goals:

Input:
```
Frente de Libertação Anuridae
From Wikipedia, the free encyclopedia
Frente de Libertação Anuridae is a terrorist organization that engages in direct action in pursuit of conservation of rare frogs in the Amazon rainforest in Brazil.
Goals and objectives:
```
Output:
```
Frente de Libertação Anuridae aims to protect, conserve, and enhance the biodiversity of Amazonia's largest forest, which is home to nearly 90% of the world's biodiversity.
Frente de Libertação Anuridae is working with local non-government organizations, NGOs, and local governments (to be determined) to protect and conserve all known frogs of the species F. anurata in the Amazon Rainforest. This mission is to include all known species and to ensure that no frog remains under threat.
```

as well as make up its imaginary history.

Input:
```
Frente de Libertação Anuridae
From Wikipedia, the free encyclopedia
Frente de Libertação Anuridae is a terrorist organization that engages in direct action in pursuit of conservation of rare frogs in the Amazon rainforest in Brazil.
History:
```
Output:
```
Frente de Libertação Anuridae was established in 1985 and is headquartered in Manaus, Brazil. It was founded to protect the endangered Anuridae frogs in the Amazon Basin and its membership includes representatives of Brazil's government and military.
Formed in 1985 after the death of the organization's founder, Frente de Libertação Anuridae was the first Brazilian-based terrorist organization.
In the early 1990s, Frente de Libertação Anuridae established an office in Sao Paulo, Brazil. In the mid-1990s the group's activities were redirected toward the Brazilian Amazon rainforest.
Activities: In 1998, the Frente de Libertação Anuridae's leader, Joao Carlos Ribeiro de Castro was arrested on charges of kidnapping, murder, and torture. On April 5, 2000, Ribeiro's wife, Marguerite, was also arrested. She later pleaded guilty and was sentenced to 10 years in prison.
In the late 1990s and early 2000s, the Frente de Libertação Anuridae conducted several attacks in the Amazon Basin. The last of these was carried out in October of 2003, with a team of four gunmen (two Frente de Libertação personnel and one unidentified man) who kidnapped five indigenous fishermen on an isolated beach in the state of Piauí. The kidnapping and the murder of the fishermen took place during a time of heavy rainfall in the forest, which was particularly devastating to Anurid frogs.
```

More fictitious stuff:

Input:
```
Indian Space Research Organisation
From Wikipedia, the free encyclopedia
The Indian Space Research Organisation (ISRO, /ˈɪsroʊ/) is the space agency of the Government of India headquartered in the city of Bengaluru.
Controversies:
```
Output:
```
There have been many controversies about the ISRO in the early years of its existence. There are numerous reasons for this, among them are:
• The space agency has a long history of mismanagement, with various mismanaged projects, including the space station, and failures in the manned space program.
• The mission to Mars was abandoned for a time due to the failure to get funding from India's National Space Commission, which had been responsible for it. However, the government was forced to grant the funding in late 2011; this resulted in the ISRO acquiring the Mars rocket, Mars Orbiter Mission (MOM) satellite which was successfully launched in September 2012. The MOM was the third satellite in India's first lunar orbit mission. It is the first spacecraft of the country to orbit the Moon and has become a world landmark.
• There are serious doubts about the effectiveness of various missions launched by ISRO, such as Chandrayaan-1, Chandrayaan-2 and Chandrayaan-3 which had not gone as planned.
```

Input:
```
Indian Space Research Organisation
From Wikipedia, the free encyclopedia
The Indian Space Research Organisation (ISRO, /ˈɪsroʊ/) is the space agency of the Government of India headquartered in the city of Bengaluru.
Activities
Biological Warfare:
```
Output:
```
ISRO is actively involved in researching biological weapons and its use in space. A number of different projects involving the research of Biological Warfare is under way at the Indian Space Research Organization, under the supervision and leadership of Dr G Mohapatra. The main focus of ISRO's research is towards studying the biological and chemical warfare capabilities of various species of animals. It is a major player with a strong presence across the globe in all aspects of space technology - from launching satellites to testing new space vehicles, and from designing and constructing launch vehicles to providing the Indian Space Research Organisation (ISRO) with space-based facilities to study and study various types of space phenomena. The scientific work done by ISRO is in the fields of space biology (both in terms of biological and non-biological warfare) and space physics (the study of the physical and electromagnetic phenomena of space and the interactions between them).
```

## Memes <a name="memes"></a>

Likewise, thanks to [knowyourmeme](https://knowyourmeme.com) articles included in the training corpus, formatting conditional text input as knowyourmene article makes GPT-2 generate meme description, history etc.

Input:
```
Know Your Meme
Memes Categories Images Trending Images Editorials Login

HODL

Summary
HODL is a popular internet meme.
```
Output:
```
The image of a person holding a bottle with no ice inside or a person holding the bottle and a sign saying "no ice" is frequently featured on the Internet. HODL is often posted on Facebook, and its popularity has spread to other websites such as Pinterest.

HODL is a common form of online protest.

Origin

HODL refers to the idea of holding your drink outside of a restaurant or bar during business hours, in defiance of the owners' and patrons' wishes, in an effort to ensure they don't serve alcoholic drinks until the business is empty.

On December 1st, 2008, the popular internet blog "Funny Memes" posted a post titled "HODL – You've Got No Ice" which featured an image of a man holding a bottle with no ice inside, a sign saying "no" in big black letters across the glass, and a caption that read "you've got no ice."

Spread

The meme spread around November 2008, and has made its way onto Facebook , Twitter , Tumblr , Reddit , Pinterest and Google
```
Input:
```
Know Your Meme
Memes Categories Images Trending Images Editorials Login

Cappuccino Dog

Summary
Cappuccino Dog is a popular internet meme.
```
Output:
```
The image shows a coffee-stained dog with its ears and tail pulled up, a cup in its mouth, and several drinks in its left paw.

Origin

A popular internet meme known as "Cappuccino Dog," which first appeared in November 2010 on 4chan's /b/ (random or random) board, has spawned numerous images on various sites across the internet, including Tumblr, Pinterest, Reddit, and 4chan's /jp/ (Japan/Pics) board, but was first posted to 4chan in November 2010 as a meme depicting a coffee-stained dog (with its ears and tail pulled up, a cup in its mouth) and several drinks in its left paw. The image became so popular for its association with coffee that 4chan users began to refer to the meme as "Cappuccino dog" or "Cappuccino Dog," a term which has since become more commonly used.

On November 21st, 2010, an unidentified user on /b/ posted a picture of a coffee-stained dog with a cup in its mouth (shown below). Within three days, the post gathered upwards of 2,500 up votes. The picture of the dog, with its ears and tail pulled up, and with cups in their hands, quickly circulated online, and on November 23rd the image was posted to Reddit as "Cappuccino Dog."

Spread

The image quickly spread around the net, eventually reaching several imageboard sites such as /b/, /jp/, /r9k/, and 8chan's /votek/, with many users using its image to share their own memes. On November 24th, Reddit user r4g4g4g4 posted a thread asking people to submit their own memes. On November 30th, the Facebook page "Merry Cappuccino Dog" was launched. On December 1st, a Facebook meme page for "Cappuccino Dog" was created, featuring many variations of the image.

```

## Pro and contra argumentation on subjects<a name="arguments"></a>

Legal documents are written in a coherent and logical style. Court opinions usually comprise parts such as background, prior history, decision, summary, opinion and begin with a distinctive heading. It is possible to take advantage of their structure to instruct GPT-2 to generate text mimicking the style of legal documents. Moreover, using the particular sentences that appear in court opinions, GPT-2 can be guided to make a pro or contra argument on a statement.

<!-- {suffers from the following problems, fails, is unsound, falls apart in multiple ways,  fundamentally flawed in a few important ways, cannot be accepted,  ignores the realities,  is not entirely valid, has NO basis whatsoever, } { is clearly correct, does have merit,  is convincing because,  is correct so far, represents a fully defensible position on this subject, which is , this argument is supported by substantial evidence: } -->

### Contra:

Input: ([source](https://masterthyself.com/npc-meme-narcissism-collectivism/))
```
ON WRIT OF CERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE SEVENTH CIRCUIT.
Dates argued and decided:                                                                                                                                                                               
February 24, 2004, Argued
May 3, 2004, Decided

I. BACKGROUND
The premise of the NPC meme is the proposition that a large number of people in the world lack an “inner monologue.” This can be construed as either a personal sense of agency, free will, volitional thought, or some other permutation of that idea. If you havenʼt played enough multiplayer video games to know, NPC stands for Non-Player Character. These are characters that you interact with who have a limited series of scripted responses. They donʼt do anything besides what theyʼre literally programed to do. Thatʼs it.
The meme suggests that these large number of people in the world without the inner monologue are actually not “real,” in the sense that they donʼt have the capacity to do anything other than what they are taught. In a separate article, Iʼve called the trait they lack metaconsciousness, or the capacity to observe the workings of oneʼs own mind and act on that information.
As you have either observed or could have guessed, this meme rapidly became political in nature, generally suggesting the left is entirely comprised of NPCs. I will not be getting into the politics, for reasons I have outlined before, but we can use this as a great lens with which to view individualism and collectivism.
At its root, the NPC meme is commentary on the apparent nature of collectivistic groups and their thought patterns. The more a population tends towards collectivism, the greater the internal pressure to maintain homogeneity of thought will be. As a result, tension caused by divergent opinions will increase to the point of being ridiculous.

II. OPINION
In my opinion, this argument suffers from the following problems:
```
Output:
```
1) The meme itself seems too complex and convoluted. This leads to a certain level of confusion which causes it to become more divisive than it really is. It would be nice to just explain the argument and not have the problem, but I find that too difficult. It makes for a better article on some level and makes it easy to read, so that I don't need to keep writing it.

2) It is too easy to misinterpret, especially to the casual reader. This is probably the main reason it is so effective. It tends to appeal to the people who are more familiar with the concept of collectivism, so it seems to have less of an impact when the people who don't know what that term means can see an illustration of this meme on the internet and have a general idea of what it is.

3) It would also make the article too complicated and difficult to read. In order to make the case against NPC, one would have to go through a lot of complicated details, and it is too easy to fall into the trap of getting frustrated over the lack of clear, succinct, and precise information that would help to make an argument.
```

Input:
```
ON WRIT OF CERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE SEVENTH CIRCUIT.
Dates argued and decided:

February 24, 2004, Argued
May 3, 2004, Decided

I. BACKGROUND
The only reason Gendo treated Shinji the way he did was because he was never good with people, and losing Yui made him even worse. He thought that if he tried to talk to Shinji, he would just end up hurting him, so he decided that it would be better to just not talk to him at all.

II. OPINION
In my opinion, this argument stands no merit:
```
Output:
```
Gendo's reason for treating Shinji as "non-person" was purely emotional and was motivated by his concern for Yui. It was not, and could not have been, because Gendo had no real idea what Yui was like. The mere prospect of a conversation and the knowledge that the boy she cared about was suffering a terrible and tragic loss did not cause Gendo to abandon his own feelings and concern about Yui at Yui's expense. To the contrary, Gendo's fear of being alone with Shinji, combined with his fear of losing his "best friend," led him to treat Shinji as non-person, even to the point of refusing to even try. Gendo's decision to disregard his "principles" and his own feelings in this matter was entirely due to the fact that he was not sure that Yui was really okay, and he needed a reason to not take responsibility for her death, as well as an emotional outlet. I therefore do not believe that Gendo's claim that he simply did not have any other choice in this regard has any weight at all.
```

### Pro:

Input:
```
ON WRIT OF CERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE SEVENTH CIRCUIT.
Dates argued and decided:

February 24, 2004, Argued
May 3, 2004, Decided

I. BACKGROUND
The only reason Gendo treated Shinji the way he did was because he was never good with people, and losing Yui made him even worse. He thought that if he tried to talk to Shinji, he would just end up hurting him, so he decided that it would be better to just not talk to him at all.

II. OPINION
In my opinion, this argument does have merit because:
```
Output:
```
a) Gendo was not a "good person"; he had a very high regard for humanity and wanted to protect it from the "other side"; he believed that he was responsible for the fate of Yui (and, consequently, Shinji) and that he must have known that he must be a burden for him to be a parent and, ultimately, a person himself;

b) Gendo had an emotional attachment to Yui that he could not possibly give up;

c) Gendo, if he wanted to protect his son, could not have left him with any choice in the matter;

d) Gendo's attitude towards Shinji at the time of his death is not inconsistent with the idea that he was not a good parent.
```
<!-- ## language_models_are_unsupervised_multitask_learners
Since its' attention context is limited to 1024 tokens, it is reasonable to expect the bigger transformer models such as [Megatron](https://nv-adlr.github.io/MegatronLM) to be better since being able to handle large contexts is essential for generating longer coherent texts.  -->

Input:
```
ON WRIT OF CERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE SEVENTH CIRCUIT.
Dates argued and decided:

February 24, 2004, Argued
May 3, 2004, Decided

I. BACKGROUND
Luxury is part and parcel of life in society.  Marketing of modern luxury goods runs counter to most marketing, and is rich with the use of scarcity as a principle of influence. The essence of luxury is the symbolic desire to belong to a superior class, which everyone will have chosen according to their dreams, because anything that can be a social signifier can become a luxury. In the luxury world, price is something not to be mentioned. As a general rule, the imagined price should be higher than it really is. It’s the opposite in traditional marketing.

II. OPINION
In my opinion, this argument is supported by substantial evidence:
```

Output:
```
1. Luxury is not always about the "luxurious" thing; it can be about anything.

2. This argument is more effective with reference to the "lifestyle" of the wealthy class. The rich can be seen as having achieved more status by becoming rich and living in a way that is "luxurious." (It also makes sense with regard to the upper middle class. Their status is more dependent on "lifestyle"—i.e. what they can afford to afford.)

3. The argument is more effective because the wealthy tend to be the ones who are most willing to be the object of marketing, rather than being the ones who are most willing to buy a "luxury" product. This is not a coincidence.

4. It has a good deal to do with the fact that some of the most famous people have made a lot of money through marketing.

5. A consumer can only make a decision about a product when he has heard about it, even though the product itself has not been publicly sold.
```

## Notes <a name="notes"></a>

<a name="fn1">1</a>: The results shown were produced using publicly released 774M model, no finetuning was involved.

<a name="fn2">2</a>:The paper suggests using Top-k random sampling with top_k=2, i.e. to consider top 2 logits while producing the output. It's a good setting for summarizing task, since it produces deterministic results (there is little of randomness in the sampling process, it always chooses top 2 logits with the highest probability). For purposes demonstrated higher top_k value is more suitable. To avoid lengthy sentences, all samples provided in this document were generated with temperature=1, top_k=16 and output length=256 tokens, unless stated otherwise.

## Written by

l4rz

This repository has no association or relationship with OpenAI.
