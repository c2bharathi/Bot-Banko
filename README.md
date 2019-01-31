# Banko

**This repo is based on a preview version of the Bot Framework and is not a good reference right now. Work is underway to upgrade to the final version and this message will be removed when that work is complete. Use the official samples in the meantime: https://github.com/Microsoft/BotBuilder-Samples**

This is a very simple bot used to demonstrate several [Cognitive Services Luis](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/) capabilities with the [C# Bot Framework V4 SDK](https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0) and [Azure Bot Service](https://azure.microsoft.com/en-us/services/bot-service/). 

This was written in July 2018 whilst the V4 SDK is still 'preview' so may not be accurate in the future.

This sample demonstrates:
* __Luis Intent detection__ 
	* Using `LuisRecognizer` not `LuisRecognizerMiddleware` 
	* Using Luis in middleware means every single message will go via Luis which is not necessary and costly in this scenario because once we have the intent and initial entities we no longer require Luis
* __Luis entity extraction__; getting the entities we have from the initial Luis utterance
* __Entity completion__; using bot dialogs to complete entities that were missing from initial Luis utterance
* __Basic bot dialogs with waterfall steps__

This sample leans heavily on the [ContosoCafe-5-DialogsWithLUISEntities](https://github.com/Microsoft/BotFramework-Samples/tree/contosocafe-v4-dotnet/docs-samples/V4/dotnet/ContosoCafe/ContosoCafe-5-DialogsWithLUISEntities/ContosoCafe) example but simplifies it and focuses on just the Luis aspects.

## The Banko Bot Concept
The bot is built around a very typical banking scenario which has two main capabilities:
* Check balance
* Make a transfer

### Check Balance
Simple intent that displays a made-up balance for the user's account

To invoke the Check Balance feature
* __"Check my balance"__; no entities just the `Balance` intent

### Using Make a Transfer
To make a transfer, the user must provide four different entities. These can be included in the initial utterance; if they are not, the bot will use a dialog to complete them:
* __AccountLabel__; a [simple Luis entity](https://docs.microsoft.com/en-gb/azure/cognitive-services/LUIS/luis-concept-entity-types) to represent the nick name for the account to transfer from i.e. 'Joint', 'Savings', 'Current' or 'Sole' 
* __Money__; a [pre-built Luis Currency entity](https://docs.microsoft.com/en-gb/azure/cognitive-services/LUIS/luis-reference-prebuilt-currency) to represent the amount to be transferred
* __Date__; a [pre-built Luis DatetimeV2 entity](https://docs.microsoft.com/en-gb/azure/cognitive-services/LUIS/luis-reference-prebuilt-datetimev2) to represent the date the transfer should take place
* __Payee__; a [simple Luis entity](https://docs.microsoft.com/en-gb/azure/cognitive-services/LUIS/luis-concept-entity-types) to represent the label for the payment recipient. This will typically be a name or company name (The Luis model has very limited training here, so only 'Martin Kearn', 'Amy Griffiths', 'John Jones' and 'BT' are likely to work as a payee)

The Make a Transfer feature can be invoked using natural language including some, all or none or the required entities. Here are some examples:
* __"I want to make a transfer"__; the `Transfer` intent without any entities.
* __"Transfer from the joint account"__; the `Transfer` intent with the `AccountLabel` entity.
* __"Transfer £20 from the joint account"__; the `Transfer` intent with the `AccountLabel` and `Money` entities.
* __"Transfer £20 from the joint account on saturday"__; the `Transfer` intent with the `AccountLabel`, `Money` and `Date` entities.
* __"Transfer £20 from the joint account to martin kearn on saturday"__; the `Transfer` intent with the `AccountLabel`, `Money`, `Date` and `Payee` entities.
