---
title: Understand data alteration concepts in LUIS - Azure | Microsoft Docs
description: Learn how data can be changed before predictions in Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb

ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 03/15/2018
ms.author: v-geberr;
---

# Data alterations
LUIS provides ways to manipulate the utterance before or during the prediction. 

## Correct spelling errors in utterance
LUIS uses [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) to correct spelling errors in the utterance. LUIS needs the key associated with that service. Create the key, then add the key as a querystring parameter at the [endpoint](https://aka.ms/luis-endpoint-apis). 

You can also correct spelling errors in the **Test** panel by [entering the key](train-test.md#view-bing-spell-check-corrections-in-test-panel). The key is kept as a session variable in the browser for the Test panel. Add the key to the Test panel in each browser session you want spelling corrected. 

Usage of the key in the test panel and at the endpoint count toward the [key usage](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quota. LUIS implements Bing Spell Check limits for text length. 

The endpoint requires two params for spelling corrections to work:

|Param|Value|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|[Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) subscription key|

When [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detects an error, the original utterance, and the corrected utterance are returned along with predictions from the endpoint.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## Change time zone of prebuilt datetimeV2 entity
When a LUIS app uses the prebuilt datetimeV2 entity, a datetime value can be returned in the prediction response. The timezone of the request is used to determine the correct datetime to return. If the request is coming from a bot or another centralized application before getting to LUIS, correct the timezone LUIS uses. 

The timezone is corrected by adding the user's timezone to the [endpoint](https://aka.ms/luis-endpoint-apis) using the `timezoneOffset` param. The value of `timezoneOffset` should be the positive or negative number, in minutes, to alter the time. It is not the UTC timezone. 

|Param|Value|
|--|--|
|`timezoneOffset`|positive or negative number, in minutes|



## Next steps

> [!div class="nextstepaction"]
> [Correct spelling mistakes with this tutorial](luis-tutorial-bing-spellcheck.md)

[LUIS]:luis-reference-regions.md