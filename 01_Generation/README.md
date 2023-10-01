# Lab 1 - Text Generation

## Overview

In this lab, you will learn how to generate text using LLMs on Amazon Bedrock. We will demonstrate the use of LLMs using the Bedrock API as well as how to utilize the LangChain framework that integrates with Bedrock. 

We will first generate text using a zero-shot prompt. The zero-shot prompt provides instruction to generate text content without providing a detailed context. We will explore zero-shot email generation using two approaches: Bedrock API (BoTo3) and Bedrock integration with LangChain. Then we will show how to improve the quality of the generated text by providing additional context in the prompt.  

## Audience

Architects and developer who want to learn how to use Amazon Bedrock LLMs to generate text. 
Some of the business use cases for text generation include:

- Generating product descriptions based on product features and benefits for marketing teams
- Generation of media articles and marketing campaigns
- Email and reports generation

## Workshop Notebooks

We will generate an email response to a customer where the customer had provided negative feedback on service received from a customer support engineer. The text generation workshop includes the following three notebooks. 

1. [Contextual Text Generation using LangChain](./02_contextual_generation.ipynb) - We provide additional context in the prompt which includes the original email from the customer that we would like the model to generate a response for. The example includes a custom prompt template in LangChain, so that variable values can be substitued in the prompt at runtime.  

## Architecture

![Bedrock](./images/bedrock.jpg)
![Bedrock](./images/bedrock_langchain.jpg)
