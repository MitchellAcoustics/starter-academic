---
title: Questions on predictive soundscape modelling
date: 2022-04-25T17:32:56.530Z
draft: false
featured: true
tags:
  - soundscape
  - machinelearning
  - ssid
categories: []
projects:
  - ssid
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
A few months ago, I gave a talk on using regression models to predict soundscape perceptions (see [the video here](https://www.youtube.com/watch?v=bUN9NWhpFow)). A fellow researcher was kind enough to reach out with a few very good questions about our work, which I answered in near-excessive detail. I thought it might be useful to share their questions and my answers with everyone! I hope you enjoy this look at the future of soundscape design. 



*Do you think there is reasonable validity in lab-based soundscape evaluation?*

Depending on the quality of the lab presentation, yes. Virtual Reality systems, either with ambisonic playback or with head-tracked binaural, along with a head mounted display appear to achieve at least acceptable levels of ecological validity ([see work from my colleague Chunyang Xu](https://www.mdpi.com/2624-599X/3/1/3/htm "https\://www.mdpi.com/2624-599X/3/1/3/htm")). This should also be considered with how useful lab-based evaluation can be for research. There’s so much we can do experimentally in a lab that is impossible in situ. This includes physiological/neurological measurements, testing the addition of new sounds in a complex scene, investigating the influence of the direction a sound comes from, and mixing and matching visual and auditory stimuli (what happens when the sound scene so clearly doesn’t match the visual environment?). But of course, this is balanced by things which are only possible in situ and not in a lab – sampling the population which actually uses a space, incorporating the participants’ purpose for being there and their relationship to the space, etc. So it depends a lot on what your research question is – for true ecological validity, for engagement with the space, for monumental and historical spaces, in situ is probably still best. For things which can’t be done in situ or a to have more controlled experimental setup – in the lab.

 

What I think is clear (to me at least) is that over-the-internet evaluations are almost certainly not valid for perceptual assessments. It’s very difficult to know exactly what the respondent was exposed to. The lack of control in playback loudness and quality, the static audio rather than headtracked, and usually the lack of a head mounted display all limit what we can do with it. We’ve had some success using online surveys for sound source identification, but we actually ended up throwing out a whole load of perceptual attribute data we collected this way during COVID because we felt it wasn’t valid. 

 

*Your example of retirees having different attitudes to the rest of the population is a good example.* 

Interestingly, multi-level modelling was [the analysis we used to find that particular result](https://www.sciencedirect.com/science/article/pii/S0272494421001134 "https\://www.sciencedirect.com/science/article/pii/S0272494421001134")!

 

*This brings into question the truth of the quality of a soundscape given these differing attitudes and opinions; if you are engineering a soundscape for quality, how do you maximise soundscape quality given this difference? How representative is the survey population per group (retirees/students/workers etc), and does this influence the generalisability of your model?*

While there are significant effects of these sorts of demographic questions, like that of retirees, they are still relatively small, accounting for about 1.4% of the variance for Pleasantness ratings and 3.9% for Eventfulness ratings. That shouldn’t be ignored, but the sort of predictive modelling we’re talking about isn’t yet at the point where we can approach that level of accuracy. I suspect that the inherent randomness and degree of variance for any individual’s response is such that we may never get there, for these sorts of models. Particularly for in situ surveys, we’re dealing with quite noisy data where the noise is inherent due to every individual’s randomness in their response.

 

But this gets to my next point, which hopefully I’ll be publishing on soon – we need to think of soundscape assessments in statistical and probabilistic terms. For real-world assessments, I don’t think it’s at all useful and is in fact probably irresponsible, to talk about predicting the response of any individual, or a single predicted response to a single input. Our modelling should ideally incorporate the inherent randomness of people’s responses, such that any single input (i.e. a recording or sound sample) gives a range of predicted responses, since that is how it works in reality. Even with perfect information – i.e. we know exactly the sound you’re exposed to and your noise sensitivity and we know your demographic information and what you had for breakfast this morning – there would still be some random range of responses you could give. This range would likely have a normal shape and have tendencies and we could predict how that distribution would move, but it’s pointless to try to say there is some ground truth of what you will perceive for a given input – human perception is a chaotic system. When we scale this up to locations and populations of people, we should be saying things like “70% of people would likely find this space vibrant”. 

 

Now, how we maximise soundscape quality is a different question to predicting assessments. To me, this is a designer’s task in a lot of cases. Which is the ideal soundscape, a vibrant one, or a calm one? It depends on the context, and there’s no single number or descriptor that says ‘this is the best soundscape’. The designers (including acoustic and soundscape experts) need to decide on their desired type of soundscape, in consultation with the users and other stakeholders. Then we can help predict what changes will lead to this desired soundscape outcome.

 

*I wanted to ask how deep you went into your nonlinear modelling strategies, and did they highlight anything that is not represented by your multilevel linear model?* 

The non-linear work was done by a Masters student I supervised and was mostly exploratory. We tried several non-linear methods and saw some differences among them, but none of them achieved the performance of the MLM I presented. At the moment I’m not completely sure why this is, but I suspect it’s down to the care that was put into the structure of the MLM. It’s the culmination of three years of PhD work, compared to less than a year afforded to the Masters work. The nonlinear work was meant as a sanity check to make sure there wasn’t some obvious advantage to non-linear models that would blow the linear one out of the water. There are some other reasons behind this approach, but mostly I came into the work from an engineering consulting perspective and one of the requirements I placed on my model was to be transparent and interpretable so that engineers could dig through it and understand what was causing e.g. a soundscape to be chaotic. This immediately excluded things like neural networks and to some extent SVMs and random forests from my personal research. The hope was for some other researchers to explore a non-linear approach (specifically neural networks) from the start but unfortunately this fell through. 

 

I’m hoping to 1. Incorporate nonlinear transformations of input features into future linear models, and 2. Further investigate non-linear models. 

 

*However, I wonder if they mask the insufficiency of linear regressions in modelling piecewise and nonlinear features that appear in soundscape evaluation.* 

I’m not convinced this is insurmountable in a linear regression. For me the next step to identifying this is to look at the wide array of features we could include and calculated non-linear correlations (personally, I’m looking at mutual information and conditional mutual information). This can help us identify whether there is any relationship, linear, piecewise, or otherwise, between the input feature and the output evaluation. Once that has been narrowed down and identified, we can do a non linear transformation before feeding into the linear model or just define separate regimes for the linear model (e.g. one set of coefficients for below 65 dBA, and one set for above). I think this is very likely, since several pieces of research have seemed to identify some threshold (somewhere around 65-70 dBA) above which the sound level is in fact dominant in a classical environmental acoustics/annoyance manner. I think we’ll need to define different approaches for the different regimes.

 

*Did you have to perform any further preprocessing of your data before feeding your nonlinear models?*

For this specific work, I’d say no. We have a data cleaning process, all features are standardized before feature selection, and we used either a train/test split or cross validation for feature selection and model building, but not any true pre-processing. As above, this will probably change as we try to make sure we haven’t missed any nonlinearities.

 

*Have you explored methods for feature generation that are not manually engineered i.e. not traditional psychoacoustic indices?*

Yes and no. For yes, I’ve been exploring a new metric for characterising temporal behaviour (based on the [1/f behaviour explored here](https://asa.scitation.org/doi/full/10.1121/1.4927033?casa_token=160IcPBZfF8AAAAA%3A3ChYe_kJm6PAMsSxHbzj4S6RtHua6emRAVyAkGXdiwB1pUyg5gI4Lhux9XxbiCNwpSukGkupwmrF "https\://asa.scitation.org/doi/full/10.1121/1.4927033?casa_token=160IcPBZfF8AAAAA%3A3ChYe_kJm6PAMsSxHbzj4S6RtHua6emRAVyAkGXdiwB1pUyg5gI4Lhux9XxbiCNwpSukGkupwmrF")), but it is derived from the time series of the psychoacoustic indices themselves. For no, again, this was a limitation I imposed from the start to my particular work. I wanted to make use of the vast psychoacoustic research which has not made its way into environmental acoustics before, and I wanted to make sure the metrics used were understandable by the engineers who would likely use the models. If we instead used for example MFCCs, it’s very difficult to interpret what is actually influencing the outcome of the model. Great, you have a super accurate ANN based on MFCCs, now what? How do you identify what is causing the problem in your soundscape? How do you design a mitigation strategy if you can’t even identify what the problem is? How do you identify what is working well to enhance or apply elsewhere if you’re working with a black box? 

 

That’s a bit facetious, but I do think there is serious value in a transparent and interpretable model with understandable inputs. That said, I’m not entirely opposed to techniques like that and am using them for some other work on sound source recognition.

 

*Do you plan to test the generalisability of your model to soundscapes not in your dataset?*

The specific model I presented, no. It is not generalisable to other locations due to the way the MLM was set up. The next step is to replace that categorical variable which identifies the locationID with some combination of characteristics which *describe* the location – architectural typology, visual openness, greenness, etc. With this, it could then be applied to other locations. So *a model* trained on this dataset will eventually be generalisable and will be tested on other locations, just not the one we currently have.

 

*I wanted to ask if you had an intuition for the validity of the different psychoacoustic metrics that you ended up with in your model for eventfulness?* 

It’s like you’ve read my mind! Here’s a section I wrote a few months ago on the use of psychoacoustic metrics in soundscape for my thesis:

 

“From the experience of the previous studies which are highly focussed on the existing environmental acoustic and psychoacoustic metrics, one (of many) potential limitations has been revealed. For the most part, these metrics were designed to characterise various negative qualities of the sound. Certainly, they therefore have a valid negative correlation with positive assessments of the sound, but the simple fact is that they were conceived of and implemented in an attempt to quantify some sonic characteristic that was assumed by the researchers to contribute to a negative perception. Hence why in Zwicker’s empirical formula for Psychoacoustic Annoyance, all of the constituent parts have positive coefficients.

 

While this would not theoretically hinder a formula for describing positive aspects of the sound, it creates a sort of conceptual barrier. If all of these metrics are designed to capture negative aspects of the sound, then it is insufficient to use them to create a formula to describe a positive sound, since that formula would only represent the ‘absence of negativity’, not necessarily positivity.”

 

*I understand that eventfulness might be an attribute that is very local to the specific soundscape experience being surveyed at the time; if this is the case, what is the intuition for fluctuation strength being a factor in eventfulness?* 

On the contrary, the feature reduction of our model showed that the context (at least due to the location) is not important for Eventfulness, while it is highly important for Pleasantness. Therefore eventfulness is less local to the specific soundscape experience and is much more dependent on the acoustic features, regardless of the rest of the context. 

 

*I see LAeq is still the strongest influence in your model of eventfulness, given this, I am interested to know why loudness isn't in this list of predictors.* 

This is primarily down to the feature selection process we used, which prioritised model performance. Given the slate of other features which were initially considered, it’s conceivable that LAeq was providing information which Loudness didn’t, when taken in context with the other psychoacoustical metrics and therefore Loudness was removed. We wouldn’t expect both features to be included in the final model anyway and, in fact, we would have manually removed one if necessary to prevent collinearity. Since LAeq and Loudness encode similar (but not identical) information, they are highly correlated but will behave somewhat differently within the model and the feature selection. That said, this process is somewhat brittle, meaning with a slightly different sample, Loudness may have won out in place of LAeq. The overall structure and behaviour of the model is robust, but given two highly correlated metrics, it’s a bit of a toss up which one was ultimately selected. I would discourage anyone from making declarative statements like ‘LAeq is the important one, ditch Loudness’ on the basis of this model.

 

*On the same note, do you think annotations of the number of discrete acoustic events that occured during a recording might be an effective indicator of eventfulness (assuming you could compute the number of discrete events, either computationally or manually); I guess I am asking if you think eventfulness is purely based on the number of events that occured or if eventfulness is motivated by many other factors.*

I absolutely think that’s a great point. Something like the number of LAmax events above a certain value (which I think is used in sleep disturbance?) would probably be a good candidate metric to include. I’ll put it on the list! Alternatively, if a system for counting the discrete sound events could be made, maybe using environmental sound recognition, that would also be great.

 

That said, I think it would again be one of many features in the final model. I don’t think there’s any single metric which can describe the complex perceptual attributes we’re aiming for. Again, I’ll just quote a section I wrote a bit ago:

 

“Contrary to the hopes expressed by Aletta et al. (2014), that ”ideally there should be one acous- tic indicator per dimension”, the evidence from subsequent investigations and modelling at- tempts (Lionello et al., 2020) indicates this to be unlikely. There appears to be no reason we should think the perceptual dimensions should be reduced to a single acoustic indicator. The dimensions of soundscape represent complex perceptual concepts which we should expect to be composed of a multi-factor interaction between the input features. This necessary complexity highlights the need for a more sophisticated machine learning approach in order to handle and interpret the interactions between the many input features which contribute to the formation of a soundscape perception. (Aletta et al., 2016)”

 

Personally, I think the next big step will be to integrate a sound source recognition model along into a psychoacoustic and contextual model of soundscape to really tease out how the perceptual response is dependent on the acoustic characteristics, combined with the meaning assigned to the sound source, and mediated by the context and personal factors. 

 

*How co-compatible do you think pleasantness and vibrancy are with valence and arousal?*

For my purposes, pleasantness and *eventfulness* are just the specific labels for the concept of soundscape valence and arousal, which is how Axelsson presented it originally. So technically they are exactly compatible, since pleasantness is just soundscape valence and eventfulness is just soundscape arousal. I think Axelsson’s work, Simone Torresin’s work on the indoor soundscape circumplex, and our own upcoming work on translating the perceptual attributes, all agree enough to demonstrate that the framework of the soundscape circumplex is solid enough to build on, although there are still some particularities to work out, particularly with the precise labels used for the various attributes.

 

*Have you performed any dimensionality reduction in the process of evaluating the data, to see which dimensions in the data are co-linear and orthogonal?* 

Not formally yet (at least not beyond some simple exploratory correlation analyses), but that’ll be coming…

 

*Do you plan to have participants evaluate your lockdown recordings in-the-lab, to get an idea of the validity of either your model or how participants might rate your soundscapes in a lab setting?*

I probably won’t end up doing that work, but yeah I think someone from our lab is planning on it. We were definitely planning on doing a round of validation work comparing in situ vs lab responses with our recordings, it’s just a matter of lab time, workload, and competing with other good ideas.

 

*Finally, I wanted to ask if you were planning to publish any of the audio and higher resolution 360 still images from your database?* 

*I am interested in using the recordings in my own research (with the necessary citations, correspondence and invitation to collaborate of course).*

Great to hear! Yes, we are working on this! We released the data used for the lockdown modelling study for open access on [Zenodo as the ‘International Soundscape Database’](https://zenodo.org/record/5654747 "https\://zenodo.org/record/5654747"). But this just includes the survey results and the output of the psychoacoustic analysis at this early stage. I’m now working on organising the binaural recordings to make them openly available as part of a future version of the database.