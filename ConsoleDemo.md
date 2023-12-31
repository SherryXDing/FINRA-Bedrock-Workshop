# Bedrock Console Demo 

Perhaps you've opened this lab and immediately thought 
> git, command lines, APIs...huh?
 
and are looking for a less technically involved workshop. Perhaps you have limited time but you're still interested in how large machine learning models work. Thankfully, Amazon Bedrock provides a console experience for you to interact with foundation models (FMs) like Amazon's Titan family or Anthropic's Claude family.

In this notebook, we'll walk you through the steps included in the individual modules for interacting with the models. You'll enter a prompt in the Bedrock playground, observing the model's response and determining the impact the inference parameters have on the the generated text. We'll follow along with the prompts that are included in the modules, pulling out the snippets where we interact with the models, and focus more on working in the console.

## Prerequisites

This notebook assumes you have access to the console in an AWS account for which you have Amazon Bedrock permissions. Please work with your AWS administrator to obtain appropriate access. You won't need any experience in large language models to complete this notebook, but having a general awareness of generative artificial intelligence can be helpful.


## Getting started

First, navigate in the console to the Bedrock Service page, found at this link:

[https://us-east-1.console.aws.amazon.com/bedrock/home](https://us-east-1.console.aws.amazon.com/bedrock/home)

Please note that the link above will send you to the console in the US East 1 (Northern Virginia) region. If you operate in a different region, please be sure to select the appropriate region from the top right corner of your console window, as seen in the picture below.

![](./imgs/console-region-selection.png)

Once you've logged into the console and confirmed you're in the correct region, you can navigate to the playgrounds by finding them in the left hand menu, which can be expanded using the menu button in the top left corner.

![](./imgs/bedrock-menu.png)

There are three playgrounds within the Bedrock console:
- Chat: interactive near-realtime chat with a generative AI, streamed in the browser
- Text: longer form textual analysis and context driven Q&A
- Image: prompt based image generation

In this lab, we'll be working with the Chat and Text playgrounds, beginning with the Chat playground. Select it from the menu.


## Model Selection

Amazon Bedrock supports FMs from several organizations specializing in the development of artificial intelligence, such as Anthropic, Cohere, AI21, and Stability AI, along with Amazon's own Titan family of large language models. For this lab, we'll be using Amazon's Titan models.

1. Choose Amazon from the "Select model category" drop down menu.
2. Choose Titan Text G1 - Express from the "Select model" drop down menu.

This tells the console that you'd like to send your prompts to the `Titan Text G1 - Express` endpoint. Once you've configured your Chat playground and are ready to begin chatting, your window should look like this:

![](imgs/chat-window.png)


## 01_Generation

Start out by asking some questions of the model. 

For example, I asked a question about requirements for selling securities in the United States. When I hit run, I received this output from the model:

![](imgs/series7.png)

You can specify other requests or questions. For example, try prompting the model with:
- Please provide a list of ingredients for making apple pie.
- What is Number Six's famous line in the first episode of The Prisoner?
- Please show me a Python 3 implementation of dimensionality reduction via principal component analysis that does not include the sklearn Python module.

Try a few prompts of your own choosing. Keep in mind though--many of these models have a strong command of syntax and vocabulary, but may not have deep expertise in a given domain. As we'll see later, it can be helpful to provide instructions to the model about what to do when it doesn't know an answer. That way, we avoid [model hallucinations](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence)), as you may already be aware of.

Another way to customize the model's response is by updating the inference parameters associated with the request. These parameters are described in the next section and tweak the model's performance.


## Common inference parameter definitions

### Randomness and Diversity

Foundation models support the following parameters to control randomness and diversity in the response.

##### Temperature
Large language models use probability construct the words in a sequence. For any given next word, there is a probability distribution of options for the next word in the sequence. When you set the temperature closer to zero, the model tends to select the higher-probability words. When you set the temperature further away from zero, the model may select a lower-probability word.

In technical terms, the temperature modulates the probability density function for the next tokens, implementing the temperature sampling technique. This parameter can deepen or flatten the density function curve. A lower value results in a steeper curve with more deterministic responses, and a higher value results in a flatter curve with more random responses.

##### Top-K 
Temperature defines the probability distribution of potential words, and Top K defines the cut off where the model no longer selects the words. For example, if K=50, the model selects from 50 of the most probable words that could be next in a given sequence. This reduces the probability that an unusual word gets selected next in a sequence. In technical terms, Top K is the number of the highest-probability vocabulary tokens to keep for Top-K-filtering - This limits the distribution of probable tokens, so the model chooses one of the highest probability tokens.

##### Top P
Top P defines a cut off based on the sum of probabilities of the potential choices. If you set Top P below 1.0, the model considers the most probable options and ignores less probable ones. Top P is similar to Top K, but instead of capping the number of choices, it caps choices based on the sum of their probabilities. For the example prompt "I hear the hoof beats of ," you may want the model to provide "horses," "zebras" or "unicorns" as the next word. If you set the temperature to its maximum, without capping Top K or Top P, you increase the probability of getting unusual results such as "unicorns." If you set the temperature to 0, you increase the probability of "horses." If you set a high temperature and set Top K or Top P to the maximum, you increase the probability of "horses" or "zebras," and decrease the probability of "unicorns."


### Length

The following parameters control the length of the generated response.

##### Response length
Configures the minimum and maximum number of tokens to use in the generated response.

##### Length penalty
Length penalty optimizes the model to be more concise in its output by penalizing longer responses. Length penalty differs from response length as the response length is a hard cut off for the minimum or maximum response length.

In technical terms, the length penalty penalizes the model exponentially for lengthy responses. 0.0 means no penalty. Set a value less than 0.0 for the model to generate longer sequences, or set a value greater than 0.0 for the model to produce shorter sequences.

### Repetitions

The following parameters help control repetition in the generated response.

##### Repetition penalty (presence penalty) 
Prevents repetitions of the same words (tokens) in responses. 1.0 means no penalty. Greater than 1.0 decreases repetition.


In the Chat playground, you'll see `Default` in the top right of the prompt box, reflecting that the prompt was sent to the Bedrock API using the default inference parameters.

![](imgs/carryforward.png)

Near the `Run` button, you'll see a link that says "Update inference configurations." Pressing that button opens up the window pictured below.

![](imgs/inference-parameters.png)

For example, the length of the response can be controlled using the length parameter. By setting it to `1`, you'll limit the response length to a single token--which will likely be part of the first word in the response. You'll also notice that the inference parameters used for a given inference are captured in the prompt box.

![](imgs/response-length.png)

Setting the length parameter to a significantly higher number (perhaps all the way up to `4096`) will allow for longer answers. Keep in mind that a longer answer is not necessarily a higher quality answer, but each token does create an expense. In other words, as described above, choosing a response length that is both concise and appropriate in its level of detail is paramount for the cost efficiency of your generative applications.

Feel free to play around with the other parameters to see how they may impact the responses provided by the model.


## 02_Summarization

Large language models like Titan can also summarize text. For this example, switch to the Text playground from the left hand menu in the console.

As before, select the Amazon model category and the Titan Text G1 - Express model. Once you've selected the model, your console page should appear as pictured below.

![](imgs/text-playground.png)


### Short Text Summarization
We'll follow along with the prompts from the Summarization module here. For the first, we're going to ask the model to summarize the announcement for Amazon Bedrock, which is copied below:


> Please provide a summary of the following text. Do not add any information that is not mentioned in the text below.
>
>AWS took all of that feedback from customers, and today we are excited to announce Amazon Bedrock, a new service that makes FMs from AI21 Labs, Anthropic Stability AI, and Amazon accessible via an API. 
> 
> Bedrock is the easiest way for customers to build and scale generative AI-based applications using FMs, democratizing access for all builders. Bedrock will offer the ability to access a range of powerful FMs for text and images—including Amazons Titan FMs, which consist of two new LLMs we’re also announcing today—through a scalable, reliable, and secure AWS managed service. 
> 
> With Bedrock’s serverless experience, customers can easily find the right model for what they’re trying to get done, get started quickly, privately customize FMs with their own data, and easily integrate and deploy them into their applications using the AWS tools and capabilities they are familiar with, without having to manage any infrastructure (including integrations with Amazon SageMaker ML features like Experiments to test different models and Pipelines to manage their FMs at scale).

You can copy the text from the cell below.

``` 
Please provide a summary of the following text. Do not add any information that is not mentioned in the text below.

AWS took all of that feedback from customers, and today we are excited to announce Amazon Bedrock, a new service that makes FMs from AI21 Labs, Anthropic Stability AI, and Amazon accessible via an API.
Bedrock is the easiest way for customers to build and scale generative AI-based applications using FMs, democratizing access for all builders. Bedrock will offer the ability to access a range of powerful FMs for text and images—including Amazons Titan FMs, which consist of two new LLMs we’re also announcing today—through a scalable, reliable, and secure AWS managed service. With Bedrock’s serverless experience, customers can easily find the right model for what they’re trying to get done, get started quickly, privately customize FMs with their own data, and easily integrate and deploy them into their applications using the AWS tools and capabilities they are familiar with, without having to manage any infrastructure (including integrations with Amazon SageMaker ML features like Experiments to test different models and Pipelines to manage their FMs at scale). 
```

Without changing the inference parameters, you can see that the model does a fairly decent job at summarizing the announcement, with its summary in green text immediately below the announcement text.

![](imgs/short-summary.png)

Feel free to tweak the parameters and re-run the prompt to see how it impacts the response. For example, increasing the temperature will change the specific details included in the summary. 

In the background, your console session is interacting with the `bedrock` and `bedrock-runtime` application programming interfaces (APIs). You can see the request being sent to the API by clicking the "View API request" button in the lower right corner of the Text playground window. The a truncated version of the previous example's API request is captured below:

```
{
  "modelId": "amazon.titan-text-express-v1",
  "contentType": "application/json",
  "accept": "*/*",
  "body": "{\"inputText\":\"Please provide a summary of the following text. Do not add any information...without managing any infrastructure.\",\"textGenerationConfig\":{\"maxTokenCount\":512,\"stopSequences\":[],\"temperature\":0,\"topP\":0.9}}"
}
```

As you can see, the console has formatted your prompt along with your selected inference parameters and is prepared to send it to the `bedrock-runtime` API. If you choose to complete the remaining modules, you'll create these API requests manually. For now, just know that the code within the Console is creating and sending these requests on your behalf under the hood.


### Long Text Summarization
Large language models are also capable of summarizing longer text. In some cases, the text may be sufficiently long that it must be split up across multiple "chunks" that are then provided to the model. Within the console, we'll simply provide the entirety of the text we want summarized. For this example, we'll use Andy Jassy's 2022 Letter to Amazon Shareholders, the text of which can be found in this repository at [02_Summarization/letters/2022-letter.txt](02_Summarization/letters/2022-letter.txt).

Before providing the text to the model, we'll need to provide it with instructions what to do. We'll add the following to the top of our prompt:

> Please provide a summary for the following text. Use only the information below to create the summary:

Now, open the letter mentioned above and copy all of the text onto your clipboard. Paste it beneath your prompt, and hit run.

![](imgs/jeff-bezos.png)

Wait...Jeff who? Wasn't his [second letter to the shareholders](http://media.corporate-ir.net/media_files/irol/97/97664/reports/Shareholderletter98.pdf) written back in 1998?

As we can see, the model made a mistake here. Andy's letter to the shareholders doesn't mention his or Jeff's names--how did it come to the conclusion that Jeff was still CEO? Chances are, the data upon which the model was trained maintains such a strong association between the concepts of `Amazon`, `Jeff Bezos`, and `CEO` that his position in that role was assumed to still be accurate. 

Let's see if through prompt engineering we can correct that error by changing the prompt we provide the model. Change the prompt ahead of the text to say
> Please use the following text and what you already know about Amazon to create a summary:

You should notice that upon execution, the model now correctly ascribes the letter to the current Amazon CEO, Andy Jassy.

![](imgs/andy-jassy.png)

Manipulating the prompts given to the model is one of the most important methods you'll use to improve the responses that are generated.


## 03_Question Answering

As mentioned before, large language models are trained to emulate a strong understanding of natural language. They are _not_ trained to be experts in every particular field. Sometimes, you'll want to use a model to answer specific questions about a niche field. In order to accomplish this, we provide additional information to the model that it can use to address our prompts.

This method relies on a technique called retrieval augmented generation (RAG), which combines the benefits of retrieval-based generation and attention-based generation. RAG uses a retrieval model to select relevant documents from a dataset and then uses an attention-based model to generate a summary or paraphrase of the selected content.

For example, let's prompt the model to help us change a tire on our Audi A8. 

![](imgs/audi-spare.png)

While these instructions seem correct, the Audi A8 doesn't have a spare tire. Instead, it has a tire repair kit with which a driver can patch their tire. For most cars, the instructions provided are appropriate. In this particular case, however, the model is guessing incorrectly based on what it believe generally to be true about vehicle maintenance. 

Let's ask it to take a guess about something that doesn't exist--the Amazon Tirana:

![](imgs/tirana.png)

The model assumed that the imaginary Tirana is actually a bicycle, and provided general instructions about repairing a flat tire on a bicycle. The Tirana doesn't exist, so we want to make sure the model won't provide an incorrect answer if it doesn't know. To achieve that, we simply specify as much in the prompt:

![](imgs/tirana-notsure.png)

Now, Titan admits that it doesn't know how to repair a flat tire on the Tirana, but still provides general steps (that in this case, happens to be almost correct). Let's take it one step further and revisit our stranded Audi. If we happen to have the owner's manual, we can provide it as context to the model so that it can be used by the model to provide a better answer. I've copied a (fictional) portion of the owner's manual below:

> Tires and tire pressure:
>
> Tires are made of black rubber and are mounted on the wheels of your vehicle. They provide the necessary grip for driving, cornering, and braking. Two important factors to consider are tire pressure and tire wear, as they can affect the performance and handling of your car.
>
> Where to find recommended tire pressure:
>
> You can find the recommended tire pressure specifications on the inflation label located on the driver's side B-pillar of your vehicle. Alternatively, you can refer to your vehicle's manual for this information. The recommended tire pressure may vary depending on the speed and the number of occupants or maximum load in the vehicle.
>
> Reinflating the tires:
>
> When checking tire pressure, it is important to do so when the tires are cold. This means allowing the vehicle to sit for at least three hours to ensure the tires are at the same temperature as the ambient temperature.
>
>To reinflate the tires:
>
>    Check the recommended tire pressure for your vehicle.
>    Follow the instructions provided on the air pump and inflate the tire(s) to the correct pressure.
>    In the center display of your vehicle, open the "Car status" app.
>    Navigate to the "Tire pressure" tab.
>    Press the "Calibrate pressure" option and confirm the action.
>    Drive the car for a few minutes at a speed above 30 km/h to calibrate the tire pressure.
>
> Note: In some cases, it may be necessary to drive for more than 15 minutes to clear any warning symbols or messages related to tire pressure. If the warnings persist, allow the tires to cool down and repeat the above steps.
>
>Flat Tire:
>
>If you encounter a flat tire while driving, you can temporarily seal the puncture and reinflate the tire using a tire mobility kit. This kit is typically stored under the lining of the luggage area in your vehicle.
>
>Instructions for using the tire mobility kit:
>
>    Open the tailgate or trunk of your vehicle.
>    Lift up the lining of the luggage area to access the tire mobility kit.
>    Follow the instructions provided with the tire mobility kit to seal the puncture in the tire.
>    After using the kit, make sure to securely put it back in its original location.
>    Contact Audi or an appropriate service for assistance with disposing of and replacing the used sealant bottle.
>
>Please note that the tire mobility kit is a temporary solution and is designed to allow you to drive for a maximum of 10 minutes or 8 km (whichever comes first) at a maximum speed of 80 km/h. It is advisable to replace the punctured tire or have it repaired by a professional as soon as possible.

You can find the content above at [03_QuestionAnswering/data/audi.txt](03_QuestionAnswering/data/audi.txt). We'll change our prompt to reference the content from the manual as context with which it can address your prompt, pulling out the specific information you need. Start by beginning your prompt with:

>I'm going to provide an excerpt from the Audi owner's manual. Please use only this information to tell me how to repair a flat tire on my Audi A8.

Beneath this, copy the text from the owner's manual and run the prompt. Now, you should see the steps extracted from the information you provided. 

![](imgs/audi-correct.png)

By providing context that the model needs to address the prompt allows you to build from the model's general understanding of natural language and apply it to specific domains that are relevant to your use cases.


## Wrapping it all up

In this lab session, we learned about interacting with Amazon Bedrock via the AWS Console. You saw the impact that inference parameters had on the responses provided by a model to a given a prompt. We leveraged RAG to find the specific and accurate answer to questions based large volumes of information we know might be relevant. 

Because Amazon Bedrock executes your prompts against models on your behalf, there's nothing for you to clean up in this lab. Simply log out of your console session and follow whatever steps are necessary to invalidate any temporary credentials used for the lab.