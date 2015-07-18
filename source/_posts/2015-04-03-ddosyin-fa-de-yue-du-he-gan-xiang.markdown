---
layout: post
title: "DDoS引发的阅读和感想"
date: 2015-04-03 19:55
comments: true
categories: 
- 随笔
---
又好久没有更新博客了。最近朋友`S`在微信群里问`DDos`是啥。一半比较好奇`S`为啥突然询问`DDos`，一半巩固一下相关互联网常识。我谷歌了`DDos`，看到谷歌新闻卡片中，`Github`遭受`DDos`攻击的新闻。我想，哦哦哦哦，原来`S`同志是看到了这个新闻了。[这里](http://www.netresec.com/?month=2015-03&page=blog&post=china%27s-man-on-the-side-attack-on-github)有该事件的一些`技术`分析？

<!--more-->

# 题外话

根据我阅读[HN](https://news.ycombinator.com/)很多评论的经验来看，大多数意见相左的评论，讨论到最后还是互相不认可对方的观点的。原因无外乎下面这句老话。。。

> We believe what we want to believe. It is all we ever do.  
- flemeth 《[Dragon Age](http://en.wikipedia.org/wiki/Dragon_Age:_Origins)》

# 阅读

好了，回到原来的新闻事件。出于对该次事件一部分舆论反应的好奇，我在[HN](https://news.ycombinator.com/)上搜到相关新闻的[帖子](https://news.ycombinator.com/item?id=9293849)。该帖有300多条评论。总体来说，大多数评论`反革命`情绪比较严重。比如说下面这条评论。

> ...  
> China and Russia are both (quite unique) examples of countries with an unfathomable degree of control over their citizens. It can be hard to grasp occasionally, coming from a western mindset but for the vast majority within said countries, the entire reality they see and what they believe to be true is heavily distorted--in that, it is defined by the vision of the oligarchy and information is carefully controlled to produce a desired set of beliefs. North Korea is an extreme caricature of this pattern.  
> ...  
> - j42

不过比较让人欣慰的是，可以看到有人马上回复道。

> The difference between the U.S. and China/Russia is that the people in China/Russia know the media is controlled by the powers that be.
Here, our press is also "defined by the vision of the oligarchy and information is carefully controlled to produce a desired set of beliefs." We just believe that it's free.
See: [http://en.wikipedia.org/wiki/Manufacturing_Consent](http://en.wikipedia.org/wiki/Manufacturing_Consent)  
> - abalone

然后？然后有人看到上面那条评论就开喷了。

> It is rather stupid to equate the degree of media manipulation in the West vs. China and Russia.  
>  - obstinate

于是，有位有识之士对上面那条评论进行了回复。

> You are right. The degree and sophistication of media manipulation is profoundly greater in the west. While the Chinese block a lot of media, the manipulation is minimal. Most Chinese are very cynical and know exactly what is going on. The west, or at least the US, traps people in a matrix of sorts where they don't even see the manipulation. The narrative is exquisitely framed and guided to leave people with a sense of moral superiority and basic faith in the system despite perceived flaws, as is ironically exhibited by your comment.
edit: some good resources on the history and nature of western media manipulation are the BBC documentary "The Century of Self" and Noam Chomsky's book "Manufacturing Consent".  
> - colordrops

继续往下拉，看到一条有趣的评论，还去`Github`挖了一下一个很久之前的[issue](https://github.com/greatfire/wiki/issues/1)。

> Most people might not know what kind of organization GreatFire really is because too much context is missing. I only discovered recently it's not so simple. There have been a lot of talks about the behavior of GreatFire for quite for a while but most of the talks are in Chinese. There are some in English though, to give everybody a glimpse here is an example: [https://github.com/greatfire/wiki/issues/1](https://github.com/greatfire/wiki/issues/1)
I have an impression is GreatFire tried to weaponize all the users of github. They succeeded.
I don't like the GFW either. But I think I'm very likely to be downvoted because the context of this incident is quite complicated. It's not easy to tell the truth especially when it's against most people's belief.  
> - jjcc

# 感想

由此，我忽然想到另外一个中策。就是在`Github`上建立小号，并且上传维鸡jie密或者shi诺d的文件。这样`Github`也有台阶下，不用被套上限制自由的骂名。`Github`可借机更新`ToU`，关闭一些政z敏感账号。

# 附录

在读帖子的时候，还看到有趣的2个人的评论。先是一个叫`gbog`的人写到。

> You obviously haven't been there. I think Chinese gov have the same level of control over its citizens as France: very erratic, sometime works well, some people try to play with fire, but overall the Chinese are all but lobotomized robots in the hands of a few puppet masters. There's over 500 strikes a year in China, not counting all the ones not big enough to be counted. I have seen streets of pedestrians walking against policemen, who were sweating of fear. Right now the prez is quite appreciated and trusted by the people, so he probably has some level of control, but this is earned by its fight against corruption, and not by some matrix-like brainwashing system.

然后一个叫`westiseast`的人回复道。

> I have been there ;)
You're misinterpreting the nature of control. Yes, there are protests, mostly because the government lets them happen. It helps people let off steam, it gives the government an indication of how people feel, and quite often there are conflicting interests which the Party can rise above (remember, government and the Party are not the same thing). So, often it's a bunch of workers protesting against a company, or a corrupt local official in one department - the Party can let that happen, and choose sides later when they've decided which way the wind is blowing. Policemen are shitting themselves because the Party mostly sides with the security apparatus but today they might let the protest get a bit wild if they want to allow the protesters a bit of leeway, and then those untrained, poorly equipped policemen will be screwed.
When stuff he Party doesn't like happens, they shut it down using methods you (on the whole) cannot do in France, the UK, and the U.S. Try introducing political censorship of material critical to Hollande. Try censoring books and courses in university. Try locking up journalists and writers (on tax evasion charges of course) when they say stuff you disagree with. Try rolling out the tanks when a protest gets out of hand. Etc.
So don't be fooled by the seemingly light hand of the gov - they've intentionally backed off from the Cultural Recolution level of control because they know that most people don't give a damn, and if left alone they will do nothing. How about an experiment - I'll hold up an anti government sign in front of the French parliament, and you do the same in Tiananmen Square and we'll see how much control the Chinese gov has ;)

`gbog`继续回复道。

> You're misinterpreting the nature of control.
I don't think so. I am just taking the other angle, from the people's perspective, and want to debunk the cliche that Chinese people are easy to control. They've had much more revolutions than any other country in their long history. They're all but easy to control. In French we say "like boiling milk", which means they can easily and suddenly get out of control and wash out anything on their way. Just blocking a few topics on social network is certainly not enough. As for things that are allowed or forbidden, it seems more cultural than anything else: In China direct verbal confrontation is very rare, while it is very common in the West, and this holds in families, in companies and also at the country's level. Not very surprisingly, in France insulting the head of state is not forbidden, and even something like a national entertainment. However, in France we have laws telling people if they are allowed to work on Sundays, which seems extremely weird and borderline "totalitarian" to the Chinese, which believe people should be allowed to work whenever they need to or want to.
Also, when talking about China, it needs to be reminded that in fact the core Western values (i.e. Enlightment values) and the core Chinese values (i.e. Confucean values) are very similar, and quite compatible. (See how fast Chinese immigrants adapt to and adopt Western values.) For instance, secularism and religious tolerance, equality of rights and before the law, meritocracy, etc.
I think the world is going very badly these days, and a big chunk of it is in the hands of people whose values are really opposed to the core of modern humanist values, and this chunk is not China. We'd better team up and fight (with ideas, not with guns) what really threatens humanity as a whole. Just my thoughts.

这个时候有趣的事情发生了，这个`westiseast`回复道。

> Hey, I didn't see it was you! Still at Douban?
I know what you mean in terms of "boiling milk" - in that respect I agree. I keep thinking these days of that old saying of China as a sleeping elephant; instead I think the people are the sleeping elephant. I think the government's strategy relies a lot on ignorance and apathy, but if even half of these stories we read as standard on NYTimes etc made it into the public consciousness, there would be huge issues.
About the laws - I guess it's not the actual content of the laws or relatively different values that illustrates control. Eg in your example about working on Sunday's - if you decided to fight one of those laws, you could do it openly and publicly and in principle it would be a fair fight. You might even embarrass the government or a political leader, but here there's so little chance of that - that's the different nature of the Communist Party control. The government/party has taken away avenues to legitimately discuss/debate/fight, so the options are either total apathy or explosive revolution. That's scary!

这位`gbog`先生原来在豆瓣上班。。。。。。这是他的[github](https://github.com/guibog)和[twitter](https://twitter.com/guibog)。




