---
layout: post
title: SurveyJS for those standard forms
author: Kristian Lange
email: lange.kristian@gmail.com
---

Doing user input forms, e.g. for questionaires or surveys in the browser yourself often means handling a lot of &lt;input&gt;, &lt;textarea&gt;, CSS etc. Those can be quite cumbersome. One wants to concentrate on the actual survey and does not want to count radiobuttons or the pixels of the border of input fields. Therefore usually one uses a library that helps with this process. So far there where mostly [jQuery UI](https://jqueryui.com/) and [Bootstrap](http://getbootstrap.com/). [Alpaca](http://www.alpacajs.org/) also does the job but I found it a bit difficult to use (although I only tried for one afternoon). Now there is a new option: [SurveyJS](http://surveyjs.org/) (or survey.js - both spellings seem to be used).

With [SurveyJS](http://surveyjs.org) one can do these standard forms like for [demographics](http://surveyjs.org/examples/jquery/questiontype-text/), [multiple-choice](http://surveyjs.org/examples/jquery/questiontype-checkbox/), or [dropdowns](http://surveyjs.org/examples/jquery/questiontype-dropdown/). It also enables more complex forms like this [matrixdropdown](http://surveyjs.org/examples/jquery/questiontype-matrixdropdown/) or this [rating](http://surveyjs.org/examples/jquery/questiontype-rating/).

But what makes SurveyJS really useful is that one can easily combine all those different elements to a single survey. E.g. have a look at this [feedback questionaire](http://surveyjs.org/examples/jquery/real-productfeedback/) - it even has sub-pages.

The forms in SurveyJS are defined with JavaScript objects - no need to define a &lt;input&gt; yourself, e.g. the following defines a rating selection:

```javascript
window.survey = new Survey.Model({
    questions: [{
        type: "rating",
        name: "satisfaction",
        title: "How satisfied are you with the Product?",
        mininumRateDescription: "Not Satisfied",
        maximumRateDescription: "Completely satisfied"
    }]
});
```

And (of course) it's simple to combine SurveyJS with JATOS. I'd recommend to use their jQuery version. And to submit the survey's result back to your JATOS server you have to change the `survey.onComplete` function to something similar to this: 

```javascript
survey.onComplete.add(function (result) {
    $("#demographics").hide(); // Hides the 'Thank you for completing ...' message
    jatos.submitResultData(JSON.stringify(result.data), jatos.startNextComponent);
});
```

To have a quick look or a good starting point in JATOS download and import the [SurveyJS example study](http://www.jatos.org/Example-Studies.html#demographic-and-survey-questions-using-surveyjs-library).

 
