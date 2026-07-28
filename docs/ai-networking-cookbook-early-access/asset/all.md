![](cover.jpeg)

![Cover Image](images/Cover.jpeg)

# AI Networking Cookbook

# Learn to build AI-based network application with practical examples

Eric Chou

![Image PacktLogo](images/PacktLogo_FM_1.png)

# AI Networking Cookbook

Copyright © 2025 Packt Publishing

*All rights reserved*. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Early Access Publication**: AI Networking Cookbook

**Early Access** **Production reference**: B32646

Published by Packt Publishing Ltd.

Grosvenor House

11 St Paul's Square

Birmingham

B3 1RB, UK.

ISBN 978-1-80580-799-5

[www.packtpub.com](https://www.packtpub.com)

# Table of Contents

[Welcome to Packt Early Access](#Preface.xhtml_h1_1)[](#Preface.xhtml_h1_1)[AI Networking Cookbook: Practical Recipes for AI-Assisted Network Automation and Development](#Preface.xhtml_h1_2)[](#Preface.xhtml_h1_2)[Chapter 1: The AI LLM Landscape and Key Parameters](#Chapter_1.xhtml_h1_3)[](#Chapter_1.xhtml_h1_3)[Join Eric Live for Build Intelligent Networks with AI](#Chapter_1.xhtml_h1_5)[](#Chapter_1.xhtml_h1_5)[Technical requirements](#Chapter_1.xhtml_h1_6)[](#Chapter_1.xhtml_h1_6)[Setting Up OpenAI Environment](#Chapter_1.xhtml_h1_7)[](#Chapter_1.xhtml_h1_7)[Getting Ready](#Chapter_1.xhtml_h2_8) [•](#Chapter_1.xhtml_h2_8)[How to do it…](#Chapter_1.xhtml_h2_9) [•](#Chapter_1.xhtml_h2_9)[How it works…](#Chapter_1.xhtml_h2_10) [•](#Chapter_1.xhtml_h2_10)[There's more…](#Chapter_1.xhtml_h2_11) [•](#Chapter_1.xhtml_h2_11)[Experiment with OpenAI LLM Model and Model Parameters](#Chapter_1.xhtml_h1_12)[](#Chapter_1.xhtml_h1_12)[Getting Ready](#Chapter_1.xhtml_h2_13) [•](#Chapter_1.xhtml_h2_13)[How to do it…](#Chapter_1.xhtml_h2_14) [•](#Chapter_1.xhtml_h2_14)[How it works…](#Chapter_1.xhtml_h2_15) [•](#Chapter_1.xhtml_h2_15)[There's more…](#Chapter_1.xhtml_h2_16) [•](#Chapter_1.xhtml_h2_16)[Experiment with OpenAI System Message](#Chapter_1.xhtml_h1_17)[](#Chapter_1.xhtml_h1_17)[Getting Ready](#Chapter_1.xhtml_h2_18) [•](#Chapter_1.xhtml_h2_18)[How to do it…](#Chapter_1.xhtml_h2_19) [•](#Chapter_1.xhtml_h2_19)[How it works…](#Chapter_1.xhtml_h2_20) [•](#Chapter_1.xhtml_h2_20)[There's more…](#Chapter_1.xhtml_h2_21) [•](#Chapter_1.xhtml_h2_21)[Experiment with OpenAI Prompt Engineering in Chat](#Chapter_1.xhtml_h1_22)[](#Chapter_1.xhtml_h1_22)[Getting Ready](#Chapter_1.xhtml_h2_23) [•](#Chapter_1.xhtml_h2_23)[How to do it…](#Chapter_1.xhtml_h2_24) [•](#Chapter_1.xhtml_h2_24)[How it works…](#Chapter_1.xhtml_h2_25) [•](#Chapter_1.xhtml_h2_25)[There's more…](#Chapter_1.xhtml_h2_26) [•](#Chapter_1.xhtml_h2_26)[Python Script with OpenAI](#Chapter_1.xhtml_h1_27)[](#Chapter_1.xhtml_h1_27)[Getting Ready](#Chapter_1.xhtml_h2_28) [•](#Chapter_1.xhtml_h2_28)[How to do it…](#Chapter_1.xhtml_h2_29) [•](#Chapter_1.xhtml_h2_29)[How it works…](#Chapter_1.xhtml_h2_30) [•](#Chapter_1.xhtml_h2_30)[There's more…](#Chapter_1.xhtml_h2_31) [•](#Chapter_1.xhtml_h2_31)[Local AI Model Ollama](#Chapter_1.xhtml_h1_32)[](#Chapter_1.xhtml_h1_32)[Getting Ready](#Chapter_1.xhtml_h2_33) [•](#Chapter_1.xhtml_h2_33)[How to do it…](#Chapter_1.xhtml_h2_34) [•](#Chapter_1.xhtml_h2_34)[How it works…](#Chapter_1.xhtml_h2_35) [•](#Chapter_1.xhtml_h2_35)[There's more…](#Chapter_1.xhtml_h2_36) [•](#Chapter_1.xhtml_h2_36)[Python Script with Ollama](#Chapter_1.xhtml_h1_37)[](#Chapter_1.xhtml_h1_37)[Getting Ready](#Chapter_1.xhtml_h2_38) [•](#Chapter_1.xhtml_h2_38)[How to do it…](#Chapter_1.xhtml_h2_39) [•](#Chapter_1.xhtml_h2_39)[How it works…](#Chapter_1.xhtml_h2_40) [•](#Chapter_1.xhtml_h2_40)[There's more…](#Chapter_1.xhtml_h2_41) [•](#Chapter_1.xhtml_h2_41)[Chapter 2: OpenAI Recipes for Network Engineers](#Chapter_2.xhtml_h1_42)[](#Chapter_2.xhtml_h1_42)[Technical requirements](#Chapter_2.xhtml_h1_44)[](#Chapter_2.xhtml_h1_44)[Using Curl for OpenAI API Communication](#Chapter_2.xhtml_h1_45)[](#Chapter_2.xhtml_h1_45)[Getting Ready](#Chapter_2.xhtml_h2_46) [•](#Chapter_2.xhtml_h2_46)[How to do it…](#Chapter_2.xhtml_h2_47) [•](#Chapter_2.xhtml_h2_47)[How it works…](#Chapter_2.xhtml_h2_48) [•](#Chapter_2.xhtml_h2_48)[There's more…](#Chapter_2.xhtml_h2_49) [•](#Chapter_2.xhtml_h2_49)[Using Postman with OpenAI API](#Chapter_2.xhtml_h1_50)[](#Chapter_2.xhtml_h1_50)[Getting Ready](#Chapter_2.xhtml_h2_51) [•](#Chapter_2.xhtml_h2_51)[How to do it…](#Chapter_2.xhtml_h2_52) [•](#Chapter_2.xhtml_h2_52)[How it works…](#Chapter_2.xhtml_h2_53) [•](#Chapter_2.xhtml_h2_53)[There's more…](#Chapter_2.xhtml_h2_54) [•](#Chapter_2.xhtml_h2_54)[Fine Tuning OpenAI Responses](#Chapter_2.xhtml_h1_55)[](#Chapter_2.xhtml_h1_55)[Getting Ready](#Chapter_2.xhtml_h2_56) [•](#Chapter_2.xhtml_h2_56)[How to do it…](#Chapter_2.xhtml_h2_57) [•](#Chapter_2.xhtml_h2_57)[How it works…](#Chapter_2.xhtml_h2_58) [•](#Chapter_2.xhtml_h2_58)[There's more…](#Chapter_2.xhtml_h2_59) [•](#Chapter_2.xhtml_h2_59)[Generating Network Topology with OpenAI](#Chapter_2.xhtml_h1_60)[](#Chapter_2.xhtml_h1_60)[Getting Ready](#Chapter_2.xhtml_h2_61) [•](#Chapter_2.xhtml_h2_61)[How to do it…](#Chapter_2.xhtml_h2_62) [•](#Chapter_2.xhtml_h2_62)[How it works…](#Chapter_2.xhtml_h2_63) [•](#Chapter_2.xhtml_h2_63)[There's more…](#Chapter_2.xhtml_h2_64) [•](#Chapter_2.xhtml_h2_64)[Generating Methods of Procedures (MOP) with OpenAI](#Chapter_2.xhtml_h1_65)[](#Chapter_2.xhtml_h1_65)[Getting Ready](#Chapter_2.xhtml_h2_66) [•](#Chapter_2.xhtml_h2_66)[How to do it…](#Chapter_2.xhtml_h2_67) [•](#Chapter_2.xhtml_h2_67)[How it works…](#Chapter_2.xhtml_h2_68) [•](#Chapter_2.xhtml_h2_68)[There's more…](#Chapter_2.xhtml_h2_69) [•](#Chapter_2.xhtml_h2_69)[Summary](#Chapter_2.xhtml_h1_70)[](#Chapter_2.xhtml_h1_70)[Chapter 3: Prompt Engineering for Reliable Outputs](#Chapter_3.xhtml_h1_71)[](#Chapter_3.xhtml_h1_71)[Technical requirements](#Chapter_3.xhtml_h1_73)[](#Chapter_3.xhtml_h1_73)[Giving Direction for AI Models](#Chapter_3.xhtml_h1_74)[](#Chapter_3.xhtml_h1_74)[Getting Ready](#Chapter_3.xhtml_h2_75) [•](#Chapter_3.xhtml_h2_75)[How to do it…](#Chapter_3.xhtml_h2_76) [•](#Chapter_3.xhtml_h2_76)[How it works…](#Chapter_3.xhtml_h2_77) [•](#Chapter_3.xhtml_h2_77)[There's more…](#Chapter_3.xhtml_h2_78) [•](#Chapter_3.xhtml_h2_78)[See also…](#Chapter_3.xhtml_h2_79) [•](#Chapter_3.xhtml_h2_79)[Specifying Return Formats](#Chapter_3.xhtml_h1_80)[](#Chapter_3.xhtml_h1_80)[Getting Ready](#Chapter_3.xhtml_h2_81) [•](#Chapter_3.xhtml_h2_81)[How to do it…](#Chapter_3.xhtml_h2_82) [•](#Chapter_3.xhtml_h2_82)[How it works…](#Chapter_3.xhtml_h2_83) [•](#Chapter_3.xhtml_h2_83)[There's more…](#Chapter_3.xhtml_h2_84) [•](#Chapter_3.xhtml_h2_84)[Giving Examples](#Chapter_3.xhtml_h1_85)[](#Chapter_3.xhtml_h1_85)[Getting Ready](#Chapter_3.xhtml_h2_86) [•](#Chapter_3.xhtml_h2_86)[How to do it…](#Chapter_3.xhtml_h2_87) [•](#Chapter_3.xhtml_h2_87)[How it works…](#Chapter_3.xhtml_h2_88) [•](#Chapter_3.xhtml_h2_88)[There's more…](#Chapter_3.xhtml_h2_89) [•](#Chapter_3.xhtml_h2_89)[Providing Feedback](#Chapter_3.xhtml_h1_90)[](#Chapter_3.xhtml_h1_90)[Getting Ready](#Chapter_3.xhtml_h2_91) [•](#Chapter_3.xhtml_h2_91)[How to do it…](#Chapter_3.xhtml_h2_92) [•](#Chapter_3.xhtml_h2_92)[How it works…](#Chapter_3.xhtml_h2_93) [•](#Chapter_3.xhtml_h2_93)[There's more…](#Chapter_3.xhtml_h2_94) [•](#Chapter_3.xhtml_h2_94)[See also…](#Chapter_3.xhtml_h2_95) [•](#Chapter_3.xhtml_h2_95)[Building Prompts Gradually](#Chapter_3.xhtml_h1_96)[](#Chapter_3.xhtml_h1_96)[Getting Ready](#Chapter_3.xhtml_h2_97) [•](#Chapter_3.xhtml_h2_97)[How to do it…](#Chapter_3.xhtml_h2_98) [•](#Chapter_3.xhtml_h2_98)[How it works…](#Chapter_3.xhtml_h2_99) [•](#Chapter_3.xhtml_h2_99)[There's more…](#Chapter_3.xhtml_h1_100)[](#Chapter_3.xhtml_h1_100)[Network Engineer's Advantage](#Chapter_3.xhtml_h1_101)[](#Chapter_3.xhtml_h1_101)[Summary](#Chapter_3.xhtml_h1_102)[](#Chapter_3.xhtml_h1_102)[Chapter 4: Local AI LLM Playground in Network Engineering](#Chapter_4.xhtml_h1_103)[](#Chapter_4.xhtml_h1_103)[Technical requirements](#Chapter_4.xhtml_h1_105)[](#Chapter_4.xhtml_h1_105)[4.1 Downloading Code Ollama Models](#Chapter_4.xhtml_h1_106)[](#Chapter_4.xhtml_h1_106)[Getting Ready](#Chapter_4.xhtml_h2_107) [•](#Chapter_4.xhtml_h2_107)[How to do it…](#Chapter_4.xhtml_h2_108) [•](#Chapter_4.xhtml_h2_108)[How it works…](#Chapter_4.xhtml_h2_109) [•](#Chapter_4.xhtml_h2_109)[There's more…](#Chapter_4.xhtml_h2_110) [•](#Chapter_4.xhtml_h2_110)[See also…](#Chapter_4.xhtml_h2_111) [•](#Chapter_4.xhtml_h2_111)[4.2 Fine Tuning LLM Resources](#Chapter_4.xhtml_h1_112)[](#Chapter_4.xhtml_h1_112)[Getting Ready](#Chapter_4.xhtml_h2_113) [•](#Chapter_4.xhtml_h2_113)[How to do it…](#Chapter_4.xhtml_h2_114) [•](#Chapter_4.xhtml_h2_114)[How it works…](#Chapter_4.xhtml_h2_115) [•](#Chapter_4.xhtml_h2_115)[There's more…](#Chapter_4.xhtml_h2_116) [•](#Chapter_4.xhtml_h2_116)[4.3 Generating Network Code Examples with CodeLlama](#Chapter_4.xhtml_h1_117)[](#Chapter_4.xhtml_h1_117)[Getting Ready](#Chapter_4.xhtml_h2_118) [•](#Chapter_4.xhtml_h2_118)[How to do it…](#Chapter_4.xhtml_h2_119) [•](#Chapter_4.xhtml_h2_119)[How it works…](#Chapter_4.xhtml_h2_120) [•](#Chapter_4.xhtml_h2_120)[There's more…](#Chapter_4.xhtml_h2_121) [•](#Chapter_4.xhtml_h2_121)[See also…](#Chapter_4.xhtml_h2_122) [•](#Chapter_4.xhtml_h2_122)[4.4 Using Llama 2 Uncensored Model for Advanced Code Generation](#Chapter_4.xhtml_h1_123)[](#Chapter_4.xhtml_h1_123)[Getting Ready](#Chapter_4.xhtml_h2_124) [•](#Chapter_4.xhtml_h2_124)[How to do it…](#Chapter_4.xhtml_h2_125) [•](#Chapter_4.xhtml_h2_125)[How it works…](#Chapter_4.xhtml_h2_126) [•](#Chapter_4.xhtml_h2_126)[There's more…](#Chapter_4.xhtml_h2_127) [•](#Chapter_4.xhtml_h2_127)[4.5 Tweaking Models with Custom Network Data](#Chapter_4.xhtml_h1_128)[](#Chapter_4.xhtml_h1_128)[Getting Ready](#Chapter_4.xhtml_h2_129) [•](#Chapter_4.xhtml_h2_129)[How to do it…](#Chapter_4.xhtml_h2_130) [•](#Chapter_4.xhtml_h2_130)[How it works…](#Chapter_4.xhtml_h2_131) [•](#Chapter_4.xhtml_h2_131)[There's more…](#Chapter_4.xhtml_h2_132) [•](#Chapter_4.xhtml_h2_132)[4.6 Using Local LLM for Network Documentation Generation](#Chapter_4.xhtml_h1_133)[](#Chapter_4.xhtml_h1_133)[Getting Ready](#Chapter_4.xhtml_h2_134) [•](#Chapter_4.xhtml_h2_134)[How to do it…](#Chapter_4.xhtml_h2_135) [•](#Chapter_4.xhtml_h2_135)[How it works…](#Chapter_4.xhtml_h2_136) [•](#Chapter_4.xhtml_h2_136)[There's more…](#Chapter_4.xhtml_h2_137) [•](#Chapter_4.xhtml_h2_137)[Summary](#Chapter_4.xhtml_h1_138)[](#Chapter_4.xhtml_h1_138)[Chapter 7: Building AI LLM Application Backends](#Chapter_5.xhtml_h1_139)[](#Chapter_5.xhtml_h1_139)[Technical requirements](#Chapter_5.xhtml_h1_141)[](#Chapter_5.xhtml_h1_141)[7.1 Simple Network AI APIs](#Chapter_5.xhtml_h1_142)[](#Chapter_5.xhtml_h1_142)[Getting Ready](#Chapter_5.xhtml_h2_143) [•](#Chapter_5.xhtml_h2_143)[How to do it…](#Chapter_5.xhtml_h2_144) [•](#Chapter_5.xhtml_h2_144)[How it works…](#Chapter_5.xhtml_h2_145) [•](#Chapter_5.xhtml_h2_145)[There's more…](#Chapter_5.xhtml_h2_146) [•](#Chapter_5.xhtml_h2_146)[See also…](#Chapter_5.xhtml_h2_147) [•](#Chapter_5.xhtml_h2_147)[7.2 Adding Device Context](#Chapter_5.xhtml_h1_148)[](#Chapter_5.xhtml_h1_148)[Getting Ready](#Chapter_5.xhtml_h2_149) [•](#Chapter_5.xhtml_h2_149)[How to do it…](#Chapter_5.xhtml_h2_150) [•](#Chapter_5.xhtml_h2_150)[How it works…](#Chapter_5.xhtml_h2_151) [•](#Chapter_5.xhtml_h2_151)[There's more…](#Chapter_5.xhtml_h2_152) [•](#Chapter_5.xhtml_h2_152)[See also…](#Chapter_5.xhtml_h2_153) [•](#Chapter_5.xhtml_h2_153)[7.3 Adding Database Storage](#Chapter_5.xhtml_h1_154)[](#Chapter_5.xhtml_h1_154)[Getting Ready](#Chapter_5.xhtml_h2_155) [•](#Chapter_5.xhtml_h2_155)[How to do it…](#Chapter_5.xhtml_h2_156) [•](#Chapter_5.xhtml_h2_156)[How it works…](#Chapter_5.xhtml_h2_157) [•](#Chapter_5.xhtml_h2_157)[There's more…](#Chapter_5.xhtml_h2_158) [•](#Chapter_5.xhtml_h2_158)[See also…](#Chapter_5.xhtml_h2_159) [•](#Chapter_5.xhtml_h2_159)[7.4 Simple Web Interface](#Chapter_5.xhtml_h1_160)[](#Chapter_5.xhtml_h1_160)[Getting Ready](#Chapter_5.xhtml_h2_161) [•](#Chapter_5.xhtml_h2_161)[How to do it…](#Chapter_5.xhtml_h2_162) [•](#Chapter_5.xhtml_h2_162)[How it works…](#Chapter_5.xhtml_h2_163) [•](#Chapter_5.xhtml_h2_163)[There's more…](#Chapter_5.xhtml_h2_164) [•](#Chapter_5.xhtml_h2_164)[See also…](#Chapter_5.xhtml_h2_165) [•](#Chapter_5.xhtml_h2_165)[7.5 Docker Deployment](#Chapter_5.xhtml_h1_166)[](#Chapter_5.xhtml_h1_166)[Getting Ready](#Chapter_5.xhtml_h2_167) [•](#Chapter_5.xhtml_h2_167)[How to do it…](#Chapter_5.xhtml_h2_168) [•](#Chapter_5.xhtml_h2_168)[How it works…](#Chapter_5.xhtml_h2_169) [•](#Chapter_5.xhtml_h2_169)[There's more…](#Chapter_5.xhtml_h2_170) [•](#Chapter_5.xhtml_h2_170)[Summary](#Chapter_5.xhtml_h2_171) [•](#Chapter_5.xhtml_h2_171)[Chapter 10: Network Security Through Vibe Coding](#Chapter_6.xhtml_h1_172)[](#Chapter_6.xhtml_h1_172)[Technical requirements](#Chapter_6.xhtml_h1_174)[](#Chapter_6.xhtml_h1_174)[10.1 Set Up GitHub Copilot](#Chapter_6.xhtml_h1_175)[](#Chapter_6.xhtml_h1_175)[Getting Ready](#Chapter_6.xhtml_h2_176) [•](#Chapter_6.xhtml_h2_176)[How to do it…](#Chapter_6.xhtml_h2_177) [•](#Chapter_6.xhtml_h2_177)[How it works…](#Chapter_6.xhtml_h2_178) [•](#Chapter_6.xhtml_h2_178)[There's more…](#Chapter_6.xhtml_h2_179) [•](#Chapter_6.xhtml_h2_179)[See also…](#Chapter_6.xhtml_h2_180) [•](#Chapter_6.xhtml_h2_180)[10.2 Set Up Claude Code](#Chapter_6.xhtml_h1_181)[](#Chapter_6.xhtml_h1_181)[Getting Ready](#Chapter_6.xhtml_h2_182) [•](#Chapter_6.xhtml_h2_182)[How to do it…](#Chapter_6.xhtml_h2_183) [•](#Chapter_6.xhtml_h2_183)[How it works…](#Chapter_6.xhtml_h2_184) [•](#Chapter_6.xhtml_h2_184)[There's more…](#Chapter_6.xhtml_h2_185) [•](#Chapter_6.xhtml_h2_185)[See also…](#Chapter_6.xhtml_h2_186) [•](#Chapter_6.xhtml_h2_186)[10.3 Conversational Log Analysis and Threat Detection](#Chapter_6.xhtml_h1_187)[](#Chapter_6.xhtml_h1_187)[Getting Ready](#Chapter_6.xhtml_h2_188) [•](#Chapter_6.xhtml_h2_188)[How to do it…](#Chapter_6.xhtml_h2_189) [•](#Chapter_6.xhtml_h2_189)[How it works…](#Chapter_6.xhtml_h2_190) [•](#Chapter_6.xhtml_h2_190)[There's more…](#Chapter_6.xhtml_h2_191) [•](#Chapter_6.xhtml_h2_191)[See also…](#Chapter_6.xhtml_h2_192) [•](#Chapter_6.xhtml_h2_192)[10.4 Rapid Security Script Development for Incident Responses](#Chapter_6.xhtml_h1_193)[](#Chapter_6.xhtml_h1_193)[Getting Ready](#Chapter_6.xhtml_h2_194) [•](#Chapter_6.xhtml_h2_194)[How to do it…](#Chapter_6.xhtml_h2_195) [•](#Chapter_6.xhtml_h2_195)[How it works…](#Chapter_6.xhtml_h2_196) [•](#Chapter_6.xhtml_h2_196)[There's more…](#Chapter_6.xhtml_h2_197) [•](#Chapter_6.xhtml_h2_197)[See also…](#Chapter_6.xhtml_h2_198) [•](#Chapter_6.xhtml_h2_198)[Summary](#Chapter_6.xhtml_h1_199)[](#Chapter_6.xhtml_h1_199)

# Welcome to Packt Early Access

# AI Networking Cookbook: Practical Recipes for AI-Assisted Network Automation and Development

**Welcome to** **Packt** **Early Access**. We’re giving you an exclusive preview of this book before it goes on sale. It can take many months to write a book, but our authors have cutting-edge information to share with you today. Early Access gives you an insight into the latest developments by making chapter drafts available. The chapters may be a little rough around the edges right now, but our authors will update them over time.

You can dip in and out of this book or follow along from start to finish; Early Access is designed to be flexible. We hope you enjoy getting to know more about the process of writing a Packt book.

1.  [Chapter 1](#Chapter_1.xhtml_h1_3): The AI LLM Landscape and Key Parameters
2.  [Chapter 2](#Chapter_2.xhtml_h1_42): OpenAI Recipes for Network Engineers
3.  [Chapter 3](#Chapter_3.xhtml_h1_71): Prompt Engineering for Reliable Outputs
4.  [Chapter 4](#Chapter_4.xhtml_h1_103): Local AI LLM Playground in Network Engineering
5.  [Chapter 7](#Chapter_5.xhtml_h1_139): Building AI LLM Application Backends
6.  [Chapter 10](#Chapter_6.xhtml_h1_172): Network Security Through Vibe Coding

# 1

# The AI LLM Landscape and Key Parameters

AI is changing the landscape of every technology field. Network engineering is no exception.

Specifically, we will cover the following recipes in this chapter:

*   Setting up OpenAI account
*   Experiment with OpenAI LLM Model and Model Parameters
*   Script with OpenAI
*   Setting up local AI Ollama models
*   Script with Ollama models

# Join Eric Live for Build Intelligent Networks with AI

Want to go hands-on with the ideas in this book? Join Eric Chou for a live, online workshop on January 24, 2026. We’ll dig into network design patterns, real-world demos, and practical Q&A to help you ship faster, more reliable networks. Seats are limited—secure your spot now: https://packt.link/buildnetworksai.

Hands-on workshop in automation and observability

![Image](images/B32646_1_1.png)

# Technical requirements

This chapter requires no prior knowledge on Artificial Intelligence or Large Language Model. If additional technology is required for a particular recipe, they will be mentioned in the recipe.

# Setting Up OpenAI Environment

OpenAI is the dominate player when it comes to **Large Languages Models (LLM)**. Their ChatGPT application was arguably what kick started the artificial intelligence (AI) movement in 2023, reaching 100 million users in only 2 months. Before we can access OpenAI resources, such as experiment with playground and make API requests, we will need to set up the developer environment.

In this recipe, we will set up the **OpenAI** **Environment** to start using OpenAI services, including generating the API key, familiarize with the settings page, and other settings.

## Getting Ready

We will need to create an **OpenAI** **platform account**; this is not the same as OpenAI account. The OpenAI account are primarily used for individual users to interact with OpenAI's AI model through a web interface like ChatGPT. The OpenAI platform account, on the other hand, is designed for developers to build application and services that leverage OpenAI's models.

Navigate to [https://platform.openai.com/](https://platform.openai.com/) and sign in to your existing OpenAI account. If you do not have an account, you can sign up for a free account with an email address. You can also log in to OpenAI with a valid Google, Microsoft, or Apple account.

Follow the on screen instruction to complete the creation of your account with steps of identity verification and payment setup.

## How to do it…

1.  Navigate to [https://platform.openai.com/settings/organization/](https://platform.openai.com/settings/organization/) and use the left navigation bar to create API Keys, create new Projects, check usage, and other administrative tasks.

    ![Image 2](images/B32646_1_2.png)

    Figure 1.1 – OpenAI Platform Settings

2.  Create an organization API key on the [https://platform.openai.com/settings/organization/api-keys](https://platform.openai.com/settings/organization/api-keys) page. Copy the key in a safe place to be used later.

    ![Image 3](images/B32646_1_3.png)

    Figure 1.2 – OpenAI Organization API Key Creation

3.  Use the top navigation menu or link, [https://platform.openai.com/playground/](https://platform.openai.com/playground/), to access OpenAI playground.

    ![Image 4](images/B32646_1_4.png)

    Figure 1.3 – OpenAI Playground

4.  Type in a question in the chat prompt and observe the answer from OpenAI.

    ![Image 5](images/B32646_1_5.png)

    Figure 1.4 – OpenAI Chat Conversation

## How it works…

The **playground** and **API** are two of the key components we will use in our development process. The playground is where we can learn and experiment with different model settings, parameters, and the result. As shown in *Figure 1.5*, the **playground interface** consists of model parameters, system messages, chat input, and conversation display. The **web interface** is a quick and easy waybto optimize the prompt and other settings.

Once we are happy with the result, we can use the API key to programmatically interact with OpenAI models using the parameters, as we will see in later recipes.

![Image 6](images/B32646_1_6.png)

Figure 1.5 – OpenAI Playground Web Interface

## There's more…

The OpenAI platform is constantly changing with new parameters, models, and guidelines. Navigate to the FAQ page, [https://platform.openai.com/docs/faq/faq](https://platform.openai.com/docs/faq/faq), for commonly asked questions.

Feel free to play around with different conversation prompt and answers generated by OpenAI.

# Experiment with OpenAI LLM Model and Model Parameters

The OpenAI Playground allows the user to select different models and tweak the model parameters. The models differ in their capabilities and cost. At the time of writing, here are the general model types:

*   **Reasoning Models**: o-series models that excel at complex, multi-step tasks. Example: o4-min, o3-mini, o1-pro.
*   **Chat Models**: Original versatile, high-intelligence models. Example: GPT-4.1, GPT-4o.
*   **Cost-optimized Models**: smaller, faster models that cost less to run. Example: o4-mini, GPT-4.1 mini, GPT-4.1 nana.
*   **Realtime** **Models**: Models capable of realtime text and audio input and outputs.

The pricing of the models can be found on [https://platform.openai.com/docs/pricing](https://platform.openai.com/docs/pricing), in increments of price per 1M tokens.

## Getting Ready

Make sure you are logged in to OpenAI playground and at the playground interface as shown in *Figure 1.5*.

## How to do it…

1.  We can begin by selecting the *Model* we want to use with the drop-down menu from **Model**. Let's select **gpt-4o** to start:

    ![Image 7](images/B32646_1_7.png)

    Figure 1.6 –

2.  Type in the question *How to configure* *VLAN* *on a Cisco* *IOS-XE* *device?* and observe the response.

    ![Image 8](images/B32646_1_8.png)

    Figure 1.7 –

3.  Pick a different model, such as **gpt-4.1-nano** and ask the same question, *How to configure* *VLAN* *on a Cisco* *IOS-XE* *device?* and observe the response.
4.  We can also see the response time difference between the two models. In my run, the *gpt-4.1-nano* generated response in *1.542ms* while *gpt-4o* took twice as long with *5.575ms* for the same question, the nano model was more than twice as fast.

    ![Image 9](images/B32646_1_9.png)

    Figure 1.8 –

5.  Use the modal selector to see the **Temperature**, **Max tokens**, and **Top P** sliders.

    ![Image 10](images/B32646_1_10.png)

    Figure 1.9 –

6.  The *Temperature* parameter controls the *creativity or randomness* of the AI response. The higher the temperature, the less deterministic and more creative of the response. We can try dial up the temperature all the way to 2 and ask the same question. As you can see in *Figure 1.10*, the response was unrecognizable, even with foreign characters.

    ![Image 11](images/B32646_1_11.png)

    Figure 1.10 –

7.  Change the *Temperature* back to *1* and lower the max tokens down to *20*. If we ask the same question, we can see the response was really short. The *token* determines the amount of response the model can return.![Image 12](images/B32646_1_12.png)
8.  Finally, let's tweak the "Top_P" from 1 to 0.25. The setting determines the "diversity" of the response. The lower the value, the more deterministic and less diverse of the response.

## How it works…

OpenAI's text generation models have been trained to understand natural language and provide text outputs in response to their inputs. The model uses probability from their training to determine the type of response they should return. The parameters we covered in this recipe are what we can use to influence the return text for how deterministic, or creative, they should be.

## There's more…

Take a look at the Advanced usage, [https://platform.openai.com/docs/advanced-usage](https://platform.openai.com/docs/advanced-usage), page for the parameter details, managing tokens, and reproducible outputs.

# Experiment with OpenAI System Message

Located below the parameters is a box for system message. By default, there are greyed-out words stating *Describe desired model behavior (tone, tool usage, and response style)*. This is one of the first places we can influence how OpenAI interacts with the user by providing desired guidelines, tone, and objective.

## Getting Ready

Locate the *System message box* from *Figure 1.5* before proceeding with the examples.

## How to do it…

1.  1. Let's begin with the following system message:

    *You are* *an AI* *assistant specialized in network engineering. Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting.*

2.  For the user prompt, let's ask OpenAI *What* *are the steps to set up* *WiFi* *7?* and observe the response.

    ![Image 13](images/B32646_1_13.png)

    Figure 1.12 –

3.  Let's modified the System message to the following:

    *You are an AI assistant 5th grade teacher specialized in network engineering. Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting* *and explain* *them to elementary school kids.*

    Instructions for interaction are:

    *   Provide funny explanations whenever necessary.
    *   Use non-technical language.
4.  We can ask the same question, *What* *are the steps to set up* *WiFi* *7?* and observe the response:

    ![Image 14](images/B32646_1_14.png)

    Figure 1.13 –

5.  Some of the additional change we can make in system message include:

    *   "Provide the answer in a paragraph format."
    *   "Provide the answer in a YAML format."
    *   "Provide the answer in a JSON format."![Image 15](images/B32646_1_15.png)

    Figure 1.14 –

## How it works…

The system message can influence the response from OpenAI for the desired output. A few key concepts for system messages:

*   **Sett** **the right** **ontext**: Provide background information or a description of the tasks. For example, "you are an AI assistant specialized in network engineering.
*   **Provide clear instructions**: Use clear, concise, and unambiguous language and be explicit about what you want the model to do. For example, "Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting and explain them to elementary school kids."
*   **Specify output formatting**: Specify how you want the output to be structured. For example, "*..* *provide* *the answer in* *YAML* *format.*"

There are other prompt engineering methods we will get into in other recipes, this recipe provides a good start.

## There's more…

OpenAI prompt engineering guide, [https://platform.openai.com/docs/guides/text?api-mode=responses#prompt-engineering](https://platform.openai.com/docs/guides/text?api-mode=responses#prompt-engineering), provides excellent guides as a starting point for prompt engineering for system messages.

# Experiment with OpenAI Prompt Engineering in Chat

The same concept for prompt engineering can be used in the OpenAI chat. Unlike the system message, the prompt we use only impacts the current conversation and will be cleared if we start a new conversation.

## Getting Ready

No other preparation required besides using the Chat box.

## How to do it…

1.  Make sure the system message is clear of any instructions.
2.  Enter the same instruction as last recipe, but now in the chat box:

    *"You are an AI assistant 5th grade teacher specialized in network engineering. Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting* *and explain* *them to elementary school kids.*

    Instructions for interaction are:

    *   Provide funny explanations whenever necessary.
    *   Use non-technical language.
    *   Provide the answer in a paragraph format."![Image 16](images/B32646_1_16.png)

    Figure 1.15 –

3.  Observe the output, they should be similar. However, when we enter a new chat with auto-clear on, the previous instruction would be cleared.

## How it works…

The same prompt engineering concept we observed in the last recipe can be applied in the chat conversation. The difference is the instruction are not persistent between chats.

## There's more…

The OpenAI prompt engineering guide, [https://platform.openai.com/docs/guides/text?api-mode=responses#prompt-engineering](https://platform.openai.com/docs/guides/text?api-mode=responses#prompt-engineering), provides excellent guides as a starting point for prompt engineering for system messages.

# Python Script with OpenAI

We can also use Python to interact with the OpenAI ChatGPT model.

## Getting Ready

Locate the OpenAI key from previous recipe as we will use it in our script. Create a Python virtual environment to work with the following script:

```
$ python3 -m venv .venv
$ source .venv/bin/activate
```

Export your OpenAI key as an environment variable.

```
$ export OPENAI_API_KEY="your-openai-api-key"
```

## How to do it…

1.  Install the `openai` Python package.

    ```
    $ pip install openai
    ```

2.  Construct the Python script:

    ```
    import os
    from openai import OpenAI

    client = OpenAI(
        api_key=os.environ.get("OPENAI_API_KEY"),
    )

    completion = client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {
                "role": "developer",
                "content": "You are an AI assistant specialized in
                network engineering.."
            },
            {
                "role": "user",
                "content": "How do I install WiFi 7?",
            },
        ],
    )
    print(completion.choices[0].message.content)
    ```

3.  Observe the output:

    ```
    $ python ch01_hello_world.py
    WiFi 7, also known as IEEE 802.11be, is the next generation of WiFi technology designed to provide faster data rates, improved efficiency, and enhanced capacity. As of my last update in October 2023, WiFi 7 was in development and might not yet be widely available in consumer products. However, if you're looking to install WiFi 7 when compatible equipment becomes available, here are the general steps you would follow:

    1\. **Check for Compatibility:**
       - Ensure that your devices (router, access points, and client devices like smartphones, tablets, and laptops) support WiFi 7\. You may need to upgrade your hardware if it isn't compatible.
    …
    ```

## How it works…

In the script, we use the `os` module to access the environment variable for OpenAI API key as well as import the `openai` Python library and initiate the client object.

The `client.chat.completion.create` is a method sent to OpenAI's endpoint to generate a response. The *role* is similar to the system message we have seen before in previous recipe, the *user* is the chat message we sent to OpenAI.

## There's more…

OpenAI Python API Library, [https://github.com/openai/openai-python](https://github.com/openai/openai-python), provides more examples of the Python library.

# Local AI Model Ollama

**Ollama** is an easy way to experiment with running language model locally.

## Getting Ready

Ensure you have an Ollama account, sign up at[https://ollama.com/signup](https://ollama.com/signup) if you need to. Also make sure you have Docker Engine ([https://www.docker.com/](https://www.docker.com/)) and docker-compose ([https://docs.docker.com/compose/](https://docs.docker.com/compose/)) are installed on the machine you wish to run the images on.

## How to do it…

The Ollama site allows easy-to-download docker images that can switch between models easily.

1.  Launch Docker engine, such as **docker.desktop**.
2.  Write the docker-compose file with text editor, name it `docker-compose.yml`.

    ```
    networks:
      ollama:

    services:
      ollama:
        image: ollama/ollama
        networks:
          - ollama
        # deploy:
        #   resources:
        #     reservations:
        #       devices:
        #         - driver: nvidia
        #           count: 1
        #           capabilities: [ gpu ]
        volumes:
          - ./data/ollama:/root/.ollama
        ports:
          - 11434:11434

      ollama-webui:
        image: ghcr.io/ollama-webui/ollama-webui:main
        volumes:
          - ./data/ollama-webui:/app/backend/data
        depends_on:
          - ollama
        ports:
          - 3002:8080
        environment:
          - 'OLLAMA_API_BASE_URL=http://ollama:11434/api'
        extra_hosts:
          - host.docker.internal:host-gateway
        networks:
          - Ollama
    ```

3.  Launch the docker images and custom network with `docker``-compose` command:

    ```
    $ docker-compose up
    ✔ ollama-webui Pulled                                                    30.0s
       ✔ f546e941f15b Pull complete                                            2.3s
       ✔ 24935aba99a7 Pull complete                                            2.5s
    …
    ✔ ollama Pulled                                                         115.7s
       ✔ 1b9f3c55f9d4 Pull complete                                           14.9s
    ✔ Network ollama-docker_ollama            Created                         0.0s
     ✔ Container ollama-docker-ollama-1        Created                         0.3s
     ✔ Container ollama-docker-ollama-webui-1  Created                         0.0s
    ollama-webui-1  | INFO:     Application startup complete.
    ollama-webui-1  | INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
    ```

4.  Access the Ollama WebUI with [http://localhost:3002](http://localhost:3002)

    ![Image 17](images/B32646_1_17.png)

    Figure 1.16 –

5.  Use the `sign up` link to create a local account, the initial account will have administrative access.

    ![Image 18](images/B32646_1_18.png)

    Figure 1.17 –

6.  Use [http://localhost:11434/`](http://localhost:11434/) to verify Ollama is running.

    ![Image 19](images/B32646_1_19.png)

    Figure 1.18 –

7.  Click on the *settings gear* to launch the settings menu.

    ![Image 20](images/B32646_1_20.png)

    Figure 1.19 –

8.  Choose **Models** and type in `gemma3:1b` and click on the green download button to pull the model from Ollama.

    ![Image 21](images/B32646_1_21.png)

    Figure 1.20–

9.  Once the model finishes download, we can choose the model from a list of dropdowns.
10.  Ask a question in the chat box, such as *"What is Layer 1 of the* *OSI* *model?*"

    ![Image 22](images/B32646_1_22.png)

    Figure 1.21 –

## How it works…

There are two docker images involved in this recipe, *ollama* and *ollama-webui*. The olllma images is where the model is located while the ollama-webui provides a management interface to easily manage the model download and a chat frontend for the setup.

Out of the box, there is no model installed on the docker image, we need to search and install a model. We picked *gemma3:1b* model because it is relatively small to download while still being reasonable in its performance. There are so many different models to pick from, we can rely on the ranking of the model as well as the model description to start picking the model we should work with. From the web page, we can evaluate the model based on the number of pulls, size of the model, and the number of parameters.

![Image 23](images/B32646_1_23.png)

We can also scroll down the model page to see if the different version of the model, such as text, vision, and others.

![Image 24](images/B32646_1_24.png)

Figure 1.23

## There's more…

Check out [https://ollama.com/](https://ollama.com/) for more information on the models and other ways to launch local LLM models.

# Python Script with Ollama

We can use a Python script to programmatically interact with the local Ollama LLM model.

## Getting Ready

Use the existing Python virtual environment or create a new one and install the *ollama* Python library:

```
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install ollama
```

## How to do it…

1.  Construct a simple Python library:

    ```
    from ollama import Client

    client = Client(host='http://localhost:11434')
    response = client.chat(model='gemma3:1b', messages=[
        {
            'role': 'user',
            'content': 'What is Layer 2 of the OSI model?',
        },
    ])
    print(response['message']['content'])
    ```

2.  Execute and observe the output:

    ```
    $ python example_1.py
    Layer 2 of the OSI model is the **Data Link Layer**. It's the layer closest to the user and responsible for reliable transfer of data between two directly connected nodes – like your computer and your router – within a local network (like your home or office network).

    Here's a breakdown of what it does and its key functions:

    **1\. Focus:** Reliable data delivery over a physical link.

    **2\. Key Functions:**

    * **Physical Addressing:** Layer 2 uses MAC (Media Access Control) addresses to identify devices on the local network. This allows devices to communicate directly without needing to understand the IP address.
    * **Error Detection and Correction:** It implements mechanisms to detect and correct errors that occur during transmission, ensuring data integrity.
    * **Framing:** It packages data into frames, which are the basic units of transmission over a network. This includes things like header and trailer information.
    * **Logical Addressing:** While it doesn't have IP addresses, it uses MAC addresses for identification and manages the flow of data within a network segment.
    * **Media Access Control (MAC) Protocols:**  This is a *critical* function.  Layer 2 protocols like Ethernet and Wi-Fi dictate how devices on the local network share the network medium.  It manages collisions and ensures that devices don't interfere with each other.
    ```

3.  Replace the content with other network engineering related questions.

## How it works…

The script demonstrates how to interact with a local AI Ollama server. It uses the *ollama* *Python library* to query the model *gemma3:1b* we installed. We initiated the *client* object to communicate with the server at *port 11434*.

The *client.chat* method is used to send a conversation to the AI model and received a response.

## There's more…

Check out [https://github.com/ollama/ollama-python](https://github.com/ollama/ollama-python) for more information on Ollama Python library.

# 2

# OpenAI Recipes for Network Engineers

In [Chapter 1](#Chapter_1.xhtml_h1_3), we discussed various ways to set up our environment, including development environment for OpenAI, the company many perceived as started the AI LLM revolution. OpenAI's large language model (LLM) offers network engineers a powerful toolkit to automate routine tasks, generate topology, device configuration, and provide expert-level assistance in many other areas.

In this chapter, we will dive deeper into using OpenAI's models to help us work on tasks within the network engineering context.

This chapter provides five recipes that progressively build your expertise in leveraging OpenAI's capabilities for network engineering tasks. We will experiment with different tools, such as curl and postman, as well as leveraging OpenAI's test-to-image capabilities. Specifically, we will cover the following recipes:

*   Using Curl for OpenAI API Communications
*   Using Postman with OpenAI API
*   Fine tuning OpenAI models
*   Generating Network Topology with OpenAI
*   Generating Methods of Procedures (MOP) with OpenAI

As we saw from [chapter 1](#Chapter_1.xhtml_h1_3), OpenAI's offerings are cloud-based, they have never officially provided locally hosted versions of their modern models such as GPT-3 or GPT-4. They did, at one point, provided an open-source version of the model, GPT-2, that can be run locally, but this is an older, much limited model compare to their current offerings. Therefore, in this chapter, we will focus on the cloud-based offerings.

# Technical requirements

This chapter builds on recipes in [Chapter 1](#Chapter_1.xhtml_h1_3) when we set up OpenAI environment, such as signing up for an account, generating the API key, and understanding the different parameters. If you are unfamiliar with them, please review them before moving on to this chapter's recipes.

# Using Curl for OpenAI API Communication

Curl ([https://github.com/curl/curl](https://github.com/curl/curl)) is one of the most ubiquitous command line tools when it comes to testing API communications. It allows to learn about the fundamentals of API communication with OpenAI such as establish connectivity, handle authentication, and process responses directly in the command line interface.

## Getting Ready

For many Linux distributions, curl is installed by default. We can check if that is the case with the curl command:

```
$ curl --version
curl 8.5.0 (x86_64-pc-linux-gnu) libcurl/8.5.0 OpenSSL/3.0.13 zlib/1.3 brotli/1.1.0 zstd/1.5.5 libidn2/2.3.7 libpsl/0.21.2 (+libidn2/2.3.7) libssh/0.10.6/openssl/zlib nghttp2/1.59.0 librtmp/2.3 OpenLDAP/2.6.7
```

However if it is not already installed, you can find the installation instruction on [https://curl.se/docs/install.html](https://curl.se/docs/install.html).

Obtain OpenAI keys and have it ready to go (see 'Setting Up OpenAI Environment' and 'Python Script with OpenAI' from [Chapter 1](#Chapter_1.xhtml_h1_3)).

## How to do it…

1.  Export OpenAI API key as environment variable.

    ```
    export OPENAI_API_KEY="your-openai-api-key"
    ```

2.  Let's make an API call to list all the available models from OpenAI.

    ```
    curl -v https://api.openai.com/v1/models -H "Authorization: Bearer $OPENAI_API_KEY" -H "Content-Type: application/json"
    ```

    We should see a list of available models in the response, your output might be different if there are different models offered at the time of execution:

    ```
    $ curl -v https://api.openai.com/v1/models   -H "Authorization: Bearer $OPENAI_API_KEY"   -H "Content-Type: application/json"
    * Host api.openai.com:443 was resolved.
    * IPv6: (none)
    * IPv4: 162.159.140.245, 172.66.0.243
    *   Trying 162.159.140.245:443...
    * Connected to api.openai.com (162.159.140.245) port 443
    * ALPN: curl offers h2,http/1.1
    …
    <
    {
      "object": "list",
      "data": [
        {
          "id": "gpt-4-0613",
          "object": "model",
          "created": 1686588896,
          "owned_by": "openai"
        },
        {
          "id": "gpt-4",
          "object": "model",
          "created": 1687882411,
          "owned_by": "openai"
        },
        {
          "id": "gpt-3.5-turbo",
          "object": "model",
          "created": 1677610602,
          "owned_by": "openai"
        },
    …
    {
          "id": "ft:gpt-3.5-turbo-0125:personal::9FZ8Mncg",
          "object": "model",
          "created": 1713497002,
          "owned_by": "network-automation-nerds"
        }
      ]
    * Connection #0 to host api.openai.com left intact
    ```

    Remember we were able to influence OpenAI's response via system messages, user prompt, and temperature? We can also use them with curl.

    ```
    $ curl -X POST "https://api.openai.com/v1/chat/completions" \
      -H "Authorization: Bearer $OPENAI_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "model": "gpt-3.5-turbo",
        "messages": [
          {
            "role": "system",
            "content": "You are a senior network engineer with 15 years         of experience. Provide technical, accurate responses with
            practical examples."
          },
          {
            "role": "user",
            "content": "Explain the difference between TCP and UDP,
            including when to use each in network applications."
          }],
        "temperature": 0.3
    }
    ```

    We should receive a response similar to the one below:

    ```
    {
      "id": "chatcmpl-BvElfOhcgv48MSQzT90KFcS65Ykuk",
      "object": "chat.completion",
      "created": 1752981163,
      "model": "gpt-3.5-turbo-0125",
      "choices": [
        {
          "index": 0,
          "message": {
            "role": "assistant",
            "content": "TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two of the most commonly used transport layer protocols in computer networks. Here are the key differences between TCP and UDP:\n\n1. **Connection-Oriented vs. Connectionless**: TCP is connection-oriented, meaning it establishes a connection between the sender and receiver before data transfer begins. UDP, on the other hand, is connectionless, where data can be sent without prior communication.\n\n2. **Reliability**: TCP provides reliable, error-checked delivery of data. It ensures that data is received in the correct order and retransmits any lost packets. UDP, on the other hand, does not guarantee delivery or order of packets. It is considered unreliable but is faster due to less overhead.\n\n3. **Flow Control and Congestion Control**: TCP includes mechanisms for flow control and congestion control to manage the rate of data transmission and prevent network congestion. UDP does not have built-in flow or congestion control mechanisms.\n\n4. **Header Size**:",
            "refusal": null,
            "annotations": []
          },
          "logprobs": null,
          "finish_reason": "length"
        }
    ```

3.  For interactive applications or long responses, we want to use the streaming and the no-buffer options.

    ```
    $ curl -X POST "https://api.openai.com/v1/chat/completions" –H "Authorization: Bearer $OPENAI_API_KEY"   -H "Content-Type: application/json"   -d '{
        "model": "gpt-3.5-turbo",
        "messages": [
          {
            "role": "system",
            "content": "You are creating a comprehensive network
            security checklist."
          },
          {
            "role": "user",
            "content": "Create a detailed network security audit
            checklist for a mid-size company with 500 employees,
            covering perimeter security, internal network segmentation,
            and monitoring."
          }
        ],
        "max_tokens": 800,
      }'   --no-buffer 0.3
    ```

The response is too long to display here. But you should see more streaming output than previous examples.

## How it works…

In step 1, as a security best practice, we export the OpenAI API key and creates it as an environment variable. This stores the variable OPENAI_API_KEY for the current shell session. If we start another shell session, we will need to create the environment variable again.

For Windows command prompt, please use 'set OPENAI_API_KEY="your-api-key".

In step 2, we sent a http GET request to [https://api.openai.com/v1/models](https://api.openai.com/v1/models), the "-v" switch turns on verbose mode that gives us more information during our learning session. The endpoint of "/v1/models" lists all available OpenAI models. The "-H" specifies HTTP headers to be included in our request. In this case, we use "Authorization: Bearer" to indicate the HTTP header for authentication and "$OPENAI_API_KEY" to specify the environment variable key that will be substituted with the actual value when we make the request. The "Content-Type: application/json" tells the server we are sending the data in JSON format. Finally, the "max-time 30" sets the maximum time allowed for the operation. This is mainly as a precaution preventing the session from hanging if there is any network issue that caused delays.

In step 3, we sent a http POST request to [https://api.openai.com/v1/chat/completions](https://api.openai.com/v1/chat/completions) with the same headers of authorization and content-type. However, because it is a POST request, we can now include a body with the request. The body includes the model and messages. Within the message, we are able to specify the system message, the user message, max tokens, as well as the temperature. These parameter were covered in an earlier recipe in [Chapter 1](#Chapter_1.xhtml_h1_3).

In step 4, we anticipated the response will be long. In traditional API call, the server would have gathered all the answers and respond all at once. For longer responses, this can take up to 30 seconds. By using the "streams: true" option, we were able to see the response immediately and allowed for a better user experience.

## There's more…

Curl is a matured and powerful command line tool. We only covered the basics in this recipe. If interested, please feel free to investigate the further topics:

*   Advance curl features such as "config" for complex configurations, "retry" for retry logics, and save response to a file with "-o".
*   Use shell scripts for error handling, logging, and batch processing.
*   Use "jq" ([https://jqlang.org/](https://jqlang.org/)) for JSON output manipulation.

# Using Postman with OpenAI API

When it comes to API testing, curl is great at stripping away all the non-essential items like the graphical user interface. But once we are familiar with the basic process, we might want to move on to more polished tools with a more refined user interface. After all, typing in command line options every single time is probably not your favorite activity in the day.

Postman ([https://www.postman.com/](https://www.postman.com/)) is another popular tool when working with APIs. It elevates your API testing capabilities by introducing a user-friendly graphical interface. Postman provides a professional-grade interface for API testing, development, and collaboration.

This recipe covers the basic features for using Postman with OpenAI API like environmental variable management, automated testing, and team collaboration.

## Getting Ready

In order to use Postman, we need to either use the Postman web-based interface or download the Postman application specific to the operating system you are using. There are free as well as paid tiers. In order to use the free tier, we will need to sign up for a free account. If you do not have an account, please create one on [https://www.postman.com/postman-account/](https://www.postman.com/postman-account/).

The web interface and application interface are fairly similar by design.

![Image 1](images/B32646_2_1.png)

Figure 2.1: Postman Web Interface

Here is a glance at the Postman Mac OS application (Windows is similar):

![Image 2](images/B32646_2_2.png)

Figure 2.2: Postman Mac App

I find the Postman application interface easier to work with, therefore for this recipe I will the Postman app for demonstration. Please feel free to use the web interface if you would prefer.

We also need the OpenAI API key ready to go, please refer back to [Chapter 1](#Chapter_1.xhtml_h1_3) recipe if you do not already have the OpenAI API key.

## How to do it…

1.  We can start by creating a dedicated workspace for this recipe.

    1.  Click on the top navigation menu on the left side for Workspace and choose to create a new workspace. There are several template to pick from, but we can pick 'Blank workspace' to begin with. I will name the workspace 'Chpater 2. OpenAI Testing with Postman.'![Image 3](images/B32646_2_3.png)

    Figure 2.3: Create a New Workspace

2.  Create an environment by clicking on '**Environments**' in the left sidebar and pick **'Create Environment.**' Once created, we can use the triple dot to rename this environment. In the example, we will name this collection `'``OpenAI` `Network Engineering` `– Testing`.'

    ![Image 4](images/B32646_2_4.png)

    Figure 2,4: Create Environment

3.  Create a new collection from the left menu by picking **'Collections**' and ensure we choose the environment we created on the top left hand corner.

    ![Image 5](images/B32646_2_5.png)

    Figure 2.5: Create Collection

4.  In the collection, we can save time by creating an authorization header for `OPENAI_API_KEY` since we need to use it for every single API request.

    ![Image 6](images/B32646_2_6.png)

    Figure 2.6: Add Authorization Header

5.  We can also create global variable we can reference in our indivisual request. Navigate to '**Variables**' and create a **'base_url**' variable with the value of `'https://api.openai.com.'` Make sure you click on the **'Save**' button on the top to save the changes.

    ![Image 7](images/B32646_2_7.png)

    Figure 2.7: Collection Variables

6.  Click on **'Add a request**.'

    ![Image 8](images/B32646_2_8.png)

    Figure 2.8: Add Requests

7.  For our first request, we can use the simple GET request to see a list of models. We can name the request `'``model lists`' and use `'``{{``base_url``}}`' to reference the collection-level variable for the base url, then append the `'``/``v1``/models`' to complete the full path. Since we entered the API key at the collection level, we can simply click on **'Send**' and receive the response.

    ![Image 9](images/B32646_2_9.png)

    Figure 2.9: First GET Request

8.  Let's go ahead and make another request. For this request, we want to change the type to POST with endpoint being `'``{{``base_url``}}/``v1``/chat/completions`'.

    ![Image 10](images/B32646_2_10.png)

    Figure 2.10: B32646_02_10.png Chat Completion Request

9.  We need to add an additional header for the **'Content-Type**.'

    ![Image 11](images/B32646_2_11.png)

    Figure 2.11: New Chat Completion Header

10.  Under '**Body**,' we will use **'Raw**' and **'JSON**' format and type in the same context we used before for chat completion.

    ![Image 12](images/B32646_2_12.png)

    Figure 2.12: New Chat Completion Request Body

11.  If everything is entered correctly, after clicking on **'Send**,' we should be able to see the response we have seen before.

    ![Image 13](images/B32646_2_13.png)

    Figure 2.13: New Chat Completion Response

## How it works…

This recipe is pretty straight forward as we did not introduce any new element to our request from the curl section. The header, the body, and the responses were identical. What we did introduce is the new Postman interface that is more user friendly and streamlined our API calls into a hierarchical structure. We use environments to separate our high-level environment such as testing, QA, and production. We also used collections to separate group different API calls into their logical use cases.

Postman provides an easy way to inherent headers, such as the OPENAI_API_KEY authentication header from collection to individual requests, thus saving us time and reduce the chance of errors. The interface also allows us to easily modify the request body if we need modify the parameters, such as the model type or temperature, without needing to copy and paste or typing in the other unchanged parameters.

When OpenAI processes our request, it returns an HTTP response with status code of 200 for success, 401 for authentication issues, and 500 for server errors. With syntax highlighting, troubleshooting issues also becomes easier.

## There's more…

Postman is very useful when it comes to API testing, for additional challenges, please feel free to create more environment such as 'Production' and 'QA.' Within the environment create separate collections as applicable.

# Fine Tuning OpenAI Responses

The OpenAI LLM base model, sometimes referred to as the Foundation model, is amazingly functional to handle most, if not all, the tasks we throw at it. As we have seen so far, it is able to help us with many general network engineering tasks, such as configuring vlans, assign IP addresses, and change interface description.

But one thing they do not currently do, as no out-of-the-box can do, is to personalize the feedback for our specific organizational needs. For example, if our team want to always use the Loopback 0 for the router-id when we configure BGP, the foundational model would not know that.

The way we can customize OpenAI model for our specific needs is to fine tune the OpenAI model. Fine-tuning allows us to teach OpenAI models our organizationls specific network configuration standard, for example, so we do not just get the generic configurations.

In this recipe, we will learn how to prepare training data, execute the fine-tuning process, and validate results to ensure the model reflect our company's standardize procedures.

## Getting Ready

Please review [Chapter 1](#Chapter_1.xhtml_h1_3) recipe for setting up OpenAI development environment, including using a Python virtual environment, OpenAI API key, and installing the 'openai' Python package.

## How to do it…

1.  We will start by querying the default OpenAI model to generate a BGP neighbor configuration, we will name this script, `recipe_3_01_test_base_model.py`.

    ```
    import os
    from openai import OpenAI

    api_key = os.getenv('OPENAI_API_KEY')
    client = OpenAI(api_key=api_key)

    query = "Configure BGP for AS 65001 with neighbor 192.168.1.2 in AS 65002"

    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {
                "role": "system", "content": "You are a network
                engineer. Generate BGP configuration."
            },
            {
                "role": "user", "content": query
            }
        ],
        max_tokens=200,
        temperature=0.1
    )

    print("BASE MODEL OUTPUT:")
    print(response.choices[0].message.content)
    ```

2.  The result is a generic BGP configuration, nothing wrong with it, but it is just about as basic as it gets.

    ```
    $ python recipe_3_01_test_base_model.py
    BASE MODEL OUTPUT:
    ```
    router bgp 65001
    neighbor 192.168.1.2 remote-as 65002
    ```
    ```

3.  Let's use a script to generate some training data, we will name this script `recipe_3_02_create_training_data.py`. At the time of writing, OpenAI requires at least 10 examples of training data, the output is saved in a file named `bgp_training_data.jsonl`.

    ```
    import json

    training_examples = [
        {
            "messages": [
                {"role": "system", "content": "You are a network engineer. Always use Loopback0 for BGP router-id and enable log-neighbor-changes."},
                {"role": "user", "content": "Configure BGP for AS 65001 with neighbor 192.168.1.2 in AS 65002"},
                {"role": "assistant", "content": "interface Loopback0\n description BGP_ROUTER_ID\n ip address 10.1.1.1 255.255.255.255\n!\nrouter bgp 65001\n bgp router-id 10.1.1.1\n bgp log-neighbor-changes\n neighbor 192.168.1.2 remote-as 65002\n neighbor 192.168.1.2 activate"}
            ]
        },
        {
            "messages": [
                {"role": "system", "content": "You are a network engineer. Always use Loopback0 for BGP router-id and enable log-neighbor-changes."},
                {"role": "user", "content": "Set up BGP for AS 65010 with neighbor 10.0.1.1 in AS 65020"},
                {"role": "assistant", "content": "interface Loopback0\n description BGP_ROUTER_ID\n ip address 10.10.1.1 255.255.255.255\n!\nrouter bgp 65010\n bgp router-id 10.10.1.1\n bgp log-neighbor-changes\n neighbor 10.0.1.1 remote-as 65020\n neighbor 10.0.1.1 activate"}
            ]
        },
    …
    {"role": "user", "content": "Setup BGP routing for AS 66500 connecting to 203.0.113.254 AS 66600"},
                {"role": "assistant", "content": "interface Loopback0\n description BGP_ROUTER_ID\n ip address 10.65.1.1 255.255.255.255\n!\nrouter bgp 66500\n bgp router-id 10.65.1.1\n bgp log-neighbor-changes\n neighbor 203.0.113.254 remote-as 66600\n neighbor 203.0.113.254 activate"}
            ]
        }
    ]

    with open('bgp_training_data.jsonl', 'w') as f:
        for example in training_examples:
            f.write(json.dumps(example) + '\n')

    print("Training data created: bgp_training_data.jsonl")
    print(f"Total examples: {len(training_examples)}")
    ```

4.  We will use the following script, recipe_3_03_create_fine_tuned_mode.py, to create the new fine-tuned model.

    ```
    import os
    import time
    from openai import OpenAI

    api_key = os.getenv('OPENAI_API_KEY')
    client = OpenAI(api_key=api_key)

    print("Uploading training data...")
    with open('bgp_training_data.jsonl', 'rb') as f:
        file_response = client.files.create(file=f, purpose='fine-tune')

    print(f"File uploaded: {file_response.id}")

    print("Starting fine-tuning job...")
    job = client.fine_tuning.jobs.create(
        training_file=file_response.id,
        model="gpt-3.5-turbo",
        suffix="bgp-standards"
    )

    print(f"Fine-tuning job created: {job.id}")
    print("Waiting for completion...")

    while True:
        job_status = client.fine_tuning.jobs.retrieve(job.id)

        if job_status.status == "succeeded":
            print(f"Fine-tuning completed: {job_status.fine_tuned_model}")
            with open('fine_tuned_model_id.txt', 'w') as f:
                f.write(job_status.fine_tuned_model)
            break
        elif job_status.status in ["failed", "cancelled"]:
            print(f"Fine-tuning {job_status.status}")
            break

        time.sleep(30)
    ```

5.  When we execute the above script, it will take anywhere between 10 to 30 minutes, at the end, we will receive a model ID for our fine-tuned model, we will save the id in a file named fine_tuned_model_id.txt to be used in the next step.

    ```
    $ python recipe_3_03_create_fine_tuned_model.py
    Uploading training data...
    File uploaded: file-QFBjPJCse6L9zsZfbtan5A
    Starting fine-tuning job...
    Fine-tuning job created: ftjob-TlSBbLCQPxyQ2NS6qqQfHRSa
    Waiting for completion...
    [note. This will take a while]
    Fine-tuning completed: ft:gpt-3.5-turbo-0125:network-automation-nerds:bgp-standards:BvTBFsUu
    ```

6.  We will be able to see the progress and the success / failure of fine-tuned models on OpenAI's console.

    ![Image 14](images/B32646_2_14.png)

    Figure 2.14: OpenAI Fine Tuned Models

7.  We will use the following script, `recipe_3_04_test_fine_tuned_model.py`, to query the new fine-tuned model.

    ```
    import os
    from openai import OpenAI

    api_key = os.getenv('OPENAI_API_KEY')
    client = OpenAI(api_key=api_key)

    with open('fine_tuned_model_id.txt', 'r') as f:
        fine_tuned_model = f.read().strip()

    query = "Configure BGP for AS 65001 with neighbor 192.168.1.2 in AS 65002"

    response = client.chat.completions.create(
        model=fine_tuned_model,
        messages=[
            {
                "role": "system",
                "content": "You are a network engineer. Generate BGP
                configuration."
            },
            {"role": "user", "content": query}
        ],
        max_tokens=200,
        temperature=0.1
    )

    print("FINE-TUNED MODEL OUTPUT:")
    print(response.choices[0].message.content)
    ```

8.  Just like magic, the new result should reflect the fine-tuned result we implemented with our training data, including the interface configuration and additional BGP parameters!

    ```
    $ python recipe_3_04_test_fine_tuned_model.py
    FINE-TUNED MODEL OUTPUT:
    interface GigabitEthernet0/0
    description TO_65002
    ip address 192.168.1.1 255.255.255.252
    !
    router bgp 65001
    bgp router-id 192.168.1.1
    bgp log-neighbor-changes
    neighbor 192.168.1.2 remote-as 65002
    neighbor 192.168.1.2 activate
    ```

## How it works…

Fine-tuning and prompt engineering (we will cover prompt engineering in the next chapter) are two of the most common ways to influence the response we received from LLMs.

In the first script, we demonstrated the output from a generic model that was based on previous training data. In the second script, we followed OpenAI's model optimization guide to provide more training data, specifically the file format needs to be in jsonl (sequence of independent JSON objects, one per line) and at least 10 examples needs to be provided (the more the better).

Note that in our training data, we provided a Loopback 0 ip addresses and use it as router-id in every single BGP configuration. However, in our testing script we did not provide a Loopback 0 ip address. The end result was a configuration snippet generated for GigabitEthernet0/0 interface and a router id from that interface ip address. The OpenAI model just came up with it. The fine-tuning process allows you to influence the result, but they are a bit of a black art and does not guarantee the right result, it is in our best interest to double check.

## There's more…

The ability to fine-tuned the existing OpenAI model to tailored toward our own data is a game changer. If you are interested in learning more about fine-tuning, please see the OpenAI model optimisation documentation ([https://platform.openai.com/docs/guides/model-optimization](https://platform.openai.com/docs/guides/model-optimization)) as well as How Fine-Tuning Works ([https://platform.openai.com/docs/guides/model-optimization#how-fine-tuning-works](https://platform.openai.com/docs/guides/model-optimization#how-fine-tuning-works)).

# Generating Network Topology with OpenAI

Show me a network engineer and I will show you an engineer who spends countless hours putting together network diagrams, drawing lines to illustrate connection points, and even trying to pick the color scheme that matches the engineer's artistic taste. If this speaks to you, you will like the following recipe.

In this recipe, we will demonstrate how to automatically generate network topology using OpenAI's LLM model. Traditional topology generation relies heavily on the engineer's experience in both the network knowledge and familiar with tools, if the engineer Visio diagram or draw.io, those are probably the tools they will use. However, we can use OpenAI to automatically generate network topology diagram by interpreting topology description and produce code for visualization.

## Getting Ready

We will use several Python packages to do this, please install the following packages in your virtual environment.

```
$ sudo apt-get install graphviz
```

## How to do it…

1.  The first step requires us to give topology description to OpenAI and generate a Graphviz dot file, we will name this script recipe_4_01.generate_dot.py.

    ```
    #!/usr/bin/env python3
    """
    Simple script to generate a fat-tree topology dot file using OpenAI
    """

    import openai
    import os

    # Setup OpenAI
    client = openai.OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

    # Generate dot file content
    prompt = """Generate a Graphviz DOT file for a fat-tree network topology:
    - 2 Core routers: core1, core2
    - 4 Spine routers: spine1, spine2, spine3, spine4 
    - 8 Leaf routers: leaf1, leaf2, leaf3, leaf4, leaf5, leaf6, leaf7, leaf8

    Each core connects to all spines, each spine connects to all leaves.
    Use different colors for each router type. Return only the DOT file content."""

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3
    )

    # Extract and save dot file content
    content = response.choices[0].message.content

    # Find the actual DOT content (between graph/digraph and the closing brace)
    import re
    dot_match = re.search(r'((?:di)?graph\s+\w*\s*\{.*\})', content, re.DOTALL)

    if dot_match:
        dot_content = dot_match.group(1)
    else:
        # Fallback: try to clean up the content
        lines = content.split('\n')
        dot_lines = []
        in_graph = False
        for line in lines:
            if 'graph' in line and '{' in line:
                in_graph = True
            if in_graph:
                dot_lines.append(line)
            if in_graph and '}' in line:
                break
        dot_content = '\n'.join(dot_lines)

    with open("fat_tree_topology.dot", "w") as f:
        f.write(dot_content)

    print("Generated fat_tree_topology.dot")
    ```

2.  When we execute the file, it will generate a file named `fat_tree_topology.dot` file in the current directory.

    ```
    $ python3 recipe_4_01_generate_dot.py
    Generated fat_tree_topology.dot
    cat fat_tree_topology.dot
    digraph FatTree {
        node [shape=circle];

        // Core routers
        "core1" [color=red];
        "core2" [color=red];

        // Spine routers
        "spine1" [color=blue];
        "spine2" [color=blue];
        "spine3" [color=blue];
        "spine4" [color=blue];

        // Leaf routers
        "leaf1" [color=green];
        "leaf2" [color=green];
        "leaf3" [color=green];
        "leaf4" [color=green];
        "leaf5" [color=green];
        "leaf6" [color=green];
        "leaf7" [color=green];
        "leaf8" [color=green];
    …
    ```

3.  We will use the following script to generate the actual topology.

    ```
    #!/usr/bin/env python3
    """
    Simple script to convert dot file to PNG topology visualization
    """

    import subprocess
    import sys

    dot_file = sys.argv[1]
    output_file = "topology.png"

    # Generate PNG from dot file
    subprocess.run(['dot', '-Tpng', dot_file, '-o', output_file])

    print(f"Generated {output_file}")
    ```

4.  We can execute the script with the dot file as the second argument.

    ```
    $ python3 recipe_4_02_create_topology.py fat_tree_topology.dot
    ```

5.  The final result is a topology file generated from code.

    ![Image 15](images/B32646_2_15.png)

    Figure 2.15: Topology File

## How it works…

The topology generation process consist of two script that works in sequence to create a visual network diagram. The first script is where OpenAI comes in where we call GPT-4 model to automatically generate a Graphviz DOT file describing a fat-tree network topology. The prompt describes the elements, such as 2 core routers, 4 spine routers, and 8 leaf devices.

The second script transforms the topology into a visual PNG file. We can also generate other diagram formats, such as JSON.

This recipe is a simple illustration of the ability to codify topology generation process. The topology graph might need some more work to be production ready, but they can save the team time and effort by automating the process.

## There's more…

Additional information on making the topology more visually appealing can be found on Graphviz's documentation, [https://graphviz.org/](https://graphviz.org/).

# Generating Methods of Procedures (MOP) with OpenAI

For any network engineer working in a medium to large enterprise environment, they are familiar with the **Method of Procedure**, or MOP, document. The MOP is generally a detail document outlining the steps for executing a specific task or process, often used in network engineering operations. An example would be a MOP to create a new VLAN, find an available IP block to be used with the VLAN, then assign a gateway IP to the VLAN interface.

The MOP focuses on the technical aspect of the process and is often used in a form of knowledge transfer between senior to junior engineers, or just as a simple reminder of steps when the sleepy engineer performing the task at 3am in the morning. While the MOP is useful, creating it can be pretty boring. In this recipe, we will show you how to leverage OpenAI LLM models to help you create a MOP.

## Getting Ready

We just need the openai Python package for this recipe.

## How to do it…

1.  We will use the following script to combine operation steps with OpenAI generated procedures.

    ```
    #!/usr/bin/env python3
    """
    VLAN MOP Generator using OpenAI API
    Generates Method of Procedure documents for Cisco IOS VLAN configuration
    """

    import openai
    import os
    import sys
    from datetime import datetime

    class VLANMOPGenerator:
        def __init__(self, api_key):
            """Initialize the MOP generator with OpenAI API key"""
            self.client = openai.OpenAI(api_key=api_key)

        def generate_mop(self, vlan_config):
            """Generate a MOP for VLAN configuration"""

            prompt = f"""
    Generate a Method of Procedure (MOP) document for configuring VLANs on a Cisco IOS device:

    Device: {vlan_config.get('device_name', 'Cisco Switch')}
    VLANs: {vlan_config.get('vlans', [])}
    Trunk ports: {vlan_config.get('trunk_ports', [])}
    Access ports: {vlan_config.get('access_ports', {})}

    Include: prerequisites, configuration commands, verification steps, and rollback procedures.
    """

            response = self.client.chat.completions.create(
                model="gpt-4",
                messages=[
                    {"role": "system", "content": "You are a network engineer. Generate professional MOP documents for Cisco IOS VLAN configuration."},
                    {"role": "user", "content": prompt}
                ],
                max_tokens=2000,
                temperature=0.3
            )

            return response.choices[0].message.content

    def main():
        # VLAN configuration
        vlan_config = {
            "device_name": "SW-CORE-01",
            "vlans": [
                {"id": 10, "name": "USERS"},
                {"id": 20, "name": "SERVERS"},
                {"id": 30, "name": "GUEST"}
            ],
            "trunk_ports": ["GigabitEthernet1/0/1", "GigabitEthernet1/0/2"],
            "access_ports": {
                "GigabitEthernet1/0/10": {"vlan": 10},
                "GigabitEthernet1/0/11": {"vlan": 20},
                "GigabitEthernet1/0/12": {"vlan": 30}
            }
        }

        # Get API key from environment
        api_key = os.getenv('OPENAI_API_KEY')
        if not api_key:
            print("Error: OPENAI_API_KEY environment variable not set!")
            sys.exit(1)

        # Generate and save MOP
        generator = VLANMOPGenerator(api_key)
        mop_document = generator.generate_mop(vlan_config)

        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        filename = f"VLAN_MOP_{timestamp}.txt"

        with open(filename, 'w') as f:
            f.write(mop_document)

        print(f"MOP saved to: {filename}")

    if __name__ == "__main__":
        main()
    ```

2.  We can execute the script to generate the MOP.

    ```
    $ python recipe_5_MOP_generation_v1.py

    MOP saved to: VLAN_MOP_20250720_214956.txt
    ```

3.  There should be a new `VLAN_MOP``_[``date].txt` file in the current directory.

    ```
    $ cat VLAN_MOP_20250720_214956.txt
    Method of Procedure (MOP) Document

    Title: VLAN Configuration on Cisco IOS Device (SW-CORE-01)

    Objective: To configure VLANs on Cisco IOS device (SW-CORE-01) and assign them to the respective ports.

    Prerequisites:
    1\. Network administrator credentials for the Cisco IOS device.
    2\. Ensure that the device is powered on and connected to the network.
    3\. Ensure that the trunk and access ports are operational.

    Configuration Steps:

    1\. Login to the Cisco IOS device (SW-CORE-01) using the network administrator credentials.

    2. Enter the global configuration mode:
       ```
       SW-CORE-01>enable
       SW-CORE-01#configure terminal
       ```

    3. Create the VLANs and assign names:
       ```
       SW-CORE-01(config)#vlan 10
       SW-CORE-01(config-vlan)#name USERS
       SW-CORE-01(config-vlan)#exit

       SW-CORE-01(config)#vlan 20
       SW-CORE-01(config-vlan)#name SERVERS
       SW-CORE-01(config-vlan)#exit

       SW-CORE-01(config)#vlan 30
       SW-CORE-01(config-vlan)#name GUEST
       SW-CORE-01(config-vlan)#exit
       ```
    … [output skipped]
    ```

## How it works…

T0his script is pretty straight forward. In Step 1, the script grabs the OPENAI_API_KEY from the environment variable, then combined the VLAN setup the engineer defined. This part can easily be tweaked or read from another file. The next portion is where the AI magic happens. The script takes the VLAN config ans use OpenAI's GPT-4 to write a Method of Procedure document. The magic relies on the previous trained knowledge from the OpenAI model and further change can be implemented via the prompt change.

## There's more…

For additional challenges, feel free to take other existing procedures, such as BGP configuration or interface configuration, use the same script framework to turn them into MOP automatically.

# Summary

In this chapter, we provided five comprehensive recipes that progressively build your expertise in leveraging OpenAI's capabilities for network engineering tasks. Each recipe follows a practical, hands-on approach that you can immediately apply in your environment. More importantly, they are written in the network engineering context, so they are relevant and applicable to our daily jobs.

In the next chapter, we will move on to one of the most important topics in AI LLM, Prompt Engineering. See you there!

# 3

# Prompt Engineering for Reliable Outputs

In the last two chapters, we saw how we can leverage AI LLM model to answer many of questions and help with daily tasks. By now, we know a big part of working with AI LLM is with how we 'ask' AI for what we need. For example, in [chapter 1](#Chapter_1.xhtml_h1_3), we saw we received very different responses when we provide different system messages. This should not come as a surprise to us. After all, as network engineers, we have spent decades mastering the art of communicating with network devices through CLI commands. When we open our favorite terminal window and ssh to a routers, we literally open up a 'window' of communication to tell the device what we want it to do. In time, we gradually integrate more communication methods to the device, say, SNMP and API, but the end goal is still to provide a set of instructions to make the device do what we tell it to do.

Now with AI LLM becoming a power tool in our toolkit, we need to master the way we communicate with AI models effectively, with prompt engineering. We can think of prompt engineering as the equivalent of 'CLI syntax' for AI models, except instead of the strict CLI syntax dictated by the vendor, we use natural language to help AI understand what we need it to do. Just as a poorly formatted BGP configuration can bring down our network, a poorly crafted prompt can give us dangerous AI response.

Often, people think of prompt engineering as a new skill; but it is really about applying existing network engineering skills to a new type of interface. We need to leverage the same type of system thinking and planning from our network engineering background into the new interface to help the AI model be more effective in solving our own problems.

In this chapter, we will go over some recipes for constructing effective prompts:

*   Giving Directions for AI models
*   Specifying Return Formats
*   Giving Examples
*   Providing Feedback
*   Building Prompts Gradually

Crafting effective prompt takes patience and practice, sometimes we even need to tailor the prompt toward the specific model we are using. But the recipes covered in this chapter should be universal regardless of the model we are using and provide a good starting point for all queries.

Are you ready? Let's get started.

# Technical requirements

For the recipes below, we will need to create and enable Python virtual environment as well as install the openai and python-dotenv packages. If you have previously completed these steps and want to re-use them for this chapter, please feel free to skip this section.

1.  Create and activate Python virtual environment.

    ```
    $ python3 -m venv .venv
    $ source .venv/bin/activate
    ```

2.  Install openai and python-dotenv packages.

    ```
    $ pip install openai python-dotenv
    ```

3.  Create a .env file using a text editor and enter your OPENAI_API_KEY.

    ```
    $ touch .env #create the file
    $ vim .env #edit the file
    ..
    OPENAI_API_KEY="your-openai-api-key"
    ```

Some of the recipes require additional Python packages specific to the recipe, they will be included in the recipe 'Getting Ready' section.

# Giving Direction for AI Models

Think of AI model as a newly hired network engineer on their first day, they are capable, but they need to be 'onboarded' with clear directions and instructions.

## Getting Ready

Please refer to the chapter 'Getting Ready' section for this recipe.

## How to do it…

1.  Create a Python script with relatively vague prompt.

    ```
    from openai import OpenAI
    import os
    from dotenv import load_dotenv

    load_dotenv()
    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Bad example - vague and directionless
    vague_prompt = "Help me with BGP configuration"

    response_vague = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": vague_prompt}]
    )

    print("Vague Response:")
    print(response_vague.choices[0].message.content)
    print("\n" + "="*50 + "\n")
    ```

2.  Execute the script to see the feedback, your result will be slightly different.

    ```
    $ python 3_1_vague_prompt.py
    Vague Response:
    Border Gateway Protocol (BGP) is a sophisticated routing protocol used for routing between autonomous systems (ASs). The following is a basic example of how to configure BGP on your router:

    Before starting configuration, make sure you have the following information:
    1\. Your network's autonomous system number (ASN)
    2\. The IP addresses of the routers you'll be adding to the BGP network

    ```cisco
    Router#configure terminal
    Router(config)#router bgp <ASN> // Enter your ASN
    Router(config-router)#network <network> // Specify the networks that you want to advertise
    Router(config-router)#neighbor <neighbor IP address> remote-as <AS of neighbor> // Define a neighboring BGP router and specify its AS number
    Router(config-router)#exit
    Router(config)#exit
    Router#copy running-config startup-config // Save the configuration to memory so it stays even after reboot
    ```

    Replace `<ASN>`, `<network>`, `<neighbor IP address>`, and `<AS of neighbor>` with your actual values.

    Remember that each router configuration will be different depending on your network's topology and size. It's important to have a good understanding of your network's existing configuration before attempting to implement BGP.

    Please note that configuring BGP incorrectly can have serious consequences for your network, so always take caution when making changes and consider consulting with a network specialist if you're unsure about any steps involved in the process.

    Educate yourself thoroughly about BGP and related topics before attempting to implement it on your network. There are numerous books, websites, and formal training courses available that can help you in this aspect.

    Consider practicing in a test environment first before applying configuration changes to production equipment. It's a good idea to test the performance and reliability of your new settings before rolling them out to your entire network.

    Lastly, ensure your configuration meets all security best practices. BGP, like any other internet protocol, is susceptible to misuse if not secured properly. Ensure your BGP routers are secured and configured to limit the risk of attacks and misconfigurations.

    ==================================================
    ```

3.  Create a Python script with prompt with better directions.

    ```
    from openai import OpenAI
    import os
    from dotenv import load_dotenv

    load_dotenv()
    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    directed_prompt = """
    You are a senior network engineer specializing in BGP routing protocols.
    I need you to generate a BGP configuration for a Cisco ASR9000 router that will:

    1\. Establish EBGP peering with two upstream ISPs (AS 65001 and AS 65002)
    2\. Implement route filtering to accept only default routes from upstreams
    3\. Set up load balancing between the two upstreams
    4\. Include basic security hardening for BGP sessions

    My router's AS number is 65100, and the peer IPs are:
    - ISP1 (AS 65001): 192.168.1.1
    - ISP2 (AS 65002): 192.168.2.1

    Provide the configuration in IOS-XR format with explanatory comments.
    """

    response_directed = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": directed_prompt}]
    )

    print("Directed Response:")
    print(response_directed.choices[0].message.content)
    ```

4.  Execute the script to see the feedback.

    ```
    $ python 3_1_directed_prompt.py
    Directed Response:
    Sure, here's the basic configuration for IOS-XR on a Cisco ASR9000 with the given requirements:

    ```
    ! ASR Configuration
    router bgp 65100
      ! Defining BGP routing configuration context for AS 65100
      bgp router-id 192.168.0.1                         
      !
      address-family ipv4 unicast
        ! Defining IPv4 unicast address family context
        maximum-paths 2                                 
        ! Enable load balancing between two equal-cost paths
      !
      neighbor 192.168.1.1
        ! Configuring BGP Neighbor for ISP1
        remote-as 65001                                
        ! Setting remote AS for ISP1 to 65001
        address-family ipv4 unicast
          ! Enabling IPv4 unicast with ISP1
          send-community
          ! Sending community attribute to neighbor
          maximum-prefix 1
          ! Limiting the prefixes received from ISP1 to 1
      !
      neighbor 192.168.2.1
        ! Configuring BGP Neighbor for ISP2
        remote-as 65002
        ! Setting remote AS for ISP2 to 65002
        address-family ipv4 unicast
          ! Enabling IPv4 unicast with ISP2
          send-community                               
          ! Sending community attribute to neighbor
          maximum-prefix 1
          ! Limiting the prefixes received from ISP2 to 1
    !
    route-policy RECEIVE-DEFAULT-ROUTE
      ! Define policy to accept only default route
      if destination in (0.0.0.0/0) then
        pass
      else
        drop
      endif
    end-policy
    !
    router bgp 65100
      neighbor 192.168.1.1
        address-family ipv4 unicast
          route-policy RECEIVE-DEFAULT-ROUTE in
          ! Applying route-policy to accept only default route from ISP1
      neighbor 192.168.2.1
        address-family ipv4 unicast
          route-policy RECEIVE-DEFAULT-ROUTE in
          ! Applying route-policy to accept only default route from ISP2
    !
    ```
    Let's harden BGP security now:

    BGP security hardening includes various aspects, such as authentication, TTL security, and updates source verification. Here's a basic example of how this could be configured:

    ```
    ! Implement BGP authentication
    router bgp 65100
      neighbor 192.168.1.1
        password cisco
      neighbor 192.168.2.1
        password cisco
    !
    ! Implement TTL security
    router bgp 65100
      neighbor 192.168.1.1
        ttl-security hops 1
      neighbor 192.168.2.1
        ttl-security hops 1
    !
    ! Implement updates source verification
    router bgp 65100
      neighbor 192.168.1.1
        update-source Loopback0
      neighbor 192.168.2.1
        update-source Loopback0
    !
    ```
    ```

Please remember to replace `cisco` with the respective BGP session password. Also, ensure to replace the Loopback0 interface with the actual interface from your configuration.

## How it works…

We started both scripts with the same code block to import Python packages and load the environment variables for OPENAI_API_KEY. We then modify only the prompt to get two dramatically different answers. When you do not receive initially receive the answer you expected from an expert, the first thing you want to check is how you can improve the prompt.

The directed prompt works better because they:

*   **Set the role**: We told the model to respond with a "senior network engineer."
*   **Define the scope**: We define a clear scope, such as "Cisco ASR9000 router," the ASN with upstream routers, and EBGP peering.
*   **Provide context**: We also provide the network details.

The direction given in the prompt is where a slightly improved prompt can have a dramatic difference in the result we receive.

## There's more…

We can further improve prompt with negative instructions, for example:

"Do not use route-reflection, Do not include deprecated IOS-XR commands."

We can also include multiple steps:

"1. First, analyze the current proposed configuration. 2. Then, identify any security vulnerabilities"

## See also…

Please see the recipe in the following section *Specifying Return Formats* if you would like to receive a different format than a copy-and-paste command snippet.

# Specifying Return Formats

As we saw in some previous recipes, if we want a certain return format, we need to be clear and precise about it. In the preceding recipe, we asked for 'IOS-XR format' in its response, that is an example of a ready-to-paste CLI command. By the same token, we can also specify JSON format for API integration, YAML for human-readable format, or Ansible playbook-style return format.

In this recipe, let's experiment with different return formats.

## Getting Ready

We will need to install additional Python package pyyaml. We will also export the OPENAI_API_KEY in our environment variable.

```
$ pip install pyyml
$ export OPENAI_API_KEY="your-openai-api-key"
```

## How to do it…

1.  Create the script that provides a base scenario and request a JSON return format with instructions.

    ```
    #!/usr/bin/env python3
    import os
    from openai import OpenAI

    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Simple base scenario
    base_scenario = """
    I have a network with the following devices:
    - 2 core switches (Cisco Catalyst 9400)
    - 4 distribution switches (Cisco Catalyst 9300)
    - 8 access switches (Cisco Catalyst 9200)

    Create an inventory document for this network.
    """

    # Request JSON format
    json_prompt = base_scenario + """

    Return the inventory in JSON format with the following structure:
    - Each device should have: hostname, model, role, mgmt_ip
    - Group devices by role (core, distribution, access)
    - Include a summary section with total counts

    Provide ONLY the JSON output, no additional text.
    """

    response_json = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": json_prompt}]
    )

    print("JSON Format Response:")
    print(response_json.choices[0].message.content)
    ```

2.  Execute the script and observe the output.

    ```
    $ python 3_2_Return_JSON_Format.py
    JSON Format Response:
    ```json
    {
      "network_inventory": {
        "core": [
          {
            "hostname": "core_switch_1",
            "model": "Cisco Catalyst 9400",
            "role": "core",
            "mgmt_ip": "192.168.1.1"
          },
          {
            "hostname": "core_switch_2",
            "model": "Cisco Catalyst 9400",
            "role": "core",
            "mgmt_ip": "192.168.1.2"
          }
        ],
        "distribution": [
          {
            "hostname": "distribution_switch_1",
            "model": "Cisco Catalyst 9300",
            "role": "distribution",
            "mgmt_ip": "192.168.1.3"
          },
          {
            "hostname": "distribution_switch_2",
            "model": "Cisco Catalyst 9300",
            "role": "distribution",
            "mgmt_ip": "192.168.1.4"
          },
    <output skipped>
        ],
        "access": [
          {
            "hostname": "access_switch_1",
            "model": "Cisco Catalyst 9200",
            "role": "access",
            "mgmt_ip": "192.168.1.7"
          },
          {
            "hostname": "access_switch_2",
            "model": "Cisco Catalyst 9200",
            "role": "access",
            "mgmt_ip": "192.168.1.8"
          },
    <output skipped>
        ],
        "summary": {
          "total_core": 2,
          "total_distribution": 4,
          "total_access": 8
        }
      }
    }
    ```
    ```

3.  Create a second script with the same base scenario and request a YAML return format (only the return format is shown here, please see the GitHub repository for the book to see the full version).

    ```
    <Top portion is the same as Step 1>
    # Request YAML format
    yaml_prompt = base_scenario + """

    Return the inventory in YAML format suitable for Ansible inventory.
    Structure it as:
    - Group devices by role using YAML groups
    - Include vars for each group (os_type, default_credentials, etc.)
    - Add individual host variables where needed
    - Make it ready to use with ansible-playbook commands

    Provide ONLY the YAML output, no additional text.
    """

    response_yaml = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": yaml_prompt}]
    )

    print("YAML Format Response:")
    print(response_yaml.choices[0].message.content)
    ```

4.  Execute the script and observe the output.

    ```
    python 3_2_Return_YML_Format.py
    YAML Format Response:
    ```yaml
    all:
      children:
        core:
          hosts:
            core_switch1:
              ansible_host: core_switch1.example.com
            core_switch2:
              ansible_host: core_switch2.example.com
          vars:
            os_type: 'ios'
            default_credentials:
              username: 'admin'
              password: 'password'

        distribution:
          hosts:
            distribution_switch1:
              ansible_host: distribution_switch1.example.com
            distribution_switch2:
              ansible_host: distribution_switch2.example.com
    <output skipped>
        access:
          hosts:
            access_switch1:
              ansible_host: access_switch1.example.com
            access_switch2:
              ansible_host: access_switch2.example.com
    <output skipped>
    ```

## How it works…

Format specification leverages AI model's training on existing knowledge. What is great is we might miss a detail or two despite our best effort, a missing period or an extra space is not uncommon if we were to perform the same task manually. The key to the instructions are:

*   Be explicit, such as "Return JSON format."
*   Specify the structure, for example, "Group devices by role using YAML groups."
*   Use format-specific language, such as "make it ready to use with ansible-playbook."

When it comes to specify formats, it is helpful to specify "no additional text" to let AI know we do not need documentation and explanation.

## There's more…

We can also combine format request with validation instructions:

```
validation_prompt = """
[your main request here]

Return as valid JSON that passes the following validation:
1\. All strings must be properly quoted
2\. No trailing commas
3\. Include a "validation" field with timestamp
4\. Test that json.loads() can parse it successfully

After generating the JSON, mentally validate it against these rules before returning.
"""
```

# Giving Examples

Simply put, the more example we can provide, the better AI can understand our needs. As the saying goes, *"a picture is worth a thousand words*." In prompt engineering, concrete examples can provide our organization's standards, naming conventions, best practices, and much more.

Using a newly hired engineer colleague for the team as an example. If we want to have the very capable colleague configure a router, we would probably reference another similar router to the person, right? "Hey Bob, take a look at lax-cor-r1 and try to configure lax-cor-r2, they are very similar except for loopback interface, IP addresses, BGP router-id, etc." That is what we mean by giving examples to AI, we want to give it concrete templates to follow.

## Getting Ready

Same setup as previous recipes, make sure the `OPENAI_API_KEY` environment variable is exported if not already.

## How to do it…

1.  Start with providing standard interface description, then ask for an output.

    ```
    #!/usr/bin/env python3
    import os
    from openai import OpenAI

    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Task description
    task_description = """
    Generate interface descriptions following our corporate naming standard.
    The descriptions should be informative but concise, following a specific format.

    Here are examples of the correct format and style:

    Example 1:
    Input: GigabitEthernet0/1 connected to SW-DIST-01 port Gi0/5
    Output: interface GigabitEthernet0/1
    description UPLINK_TO_SW-DIST-01_Gi0/5_10GB_CORE

    Example 2:
    Input: GigabitEthernet0/2 connected to user workstation in room 205
    Output: interface GigabitEthernet0/2
    description ACCESS_ROOM205_WORKSTATION_1GB_USER

    Example 3:
    Input: TenGigabitEthernet0/1 connected to firewall outside interface
    Output: interface TenGigabitEthernet0/1
    description FIREWALL_OUTSIDE_PA3220_10GB_SECURITY

    Now, apply the same format and style to this input:
    GigabitEthernet0/3 connected to IP phone in conference room B
    """

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": task_description}]
    )

    print("Generated Interface Description:")
    print(response.choices[0].message.content)
    ```

2.  Execute and receive output.

    ```
    $ python 3_3_interface_description_example.py
    Generated Interface Description:
    Output: interface GigabitEthernet0/3
    description IP_PHONE_CONFROOMB_1GB_VOICE
    ```

3.  Move forward with a more complex example for VLAN configuration with the provided example.

    ```
    #!/usr/bin/env python3
    import os
    from openai import OpenAI

    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Task description
    task_description = """
    Generate VLAN configurations following our corporate standards.
    Each VLAN should have proper naming, description, and associated services.

    Here are examples of the correct format and style:

    Example 1:
    Input: VLAN for user workstations, ID 100, subnet 10.100.0.0/24
    Output:
    vlan 100
    name USERS_WORKSTATION
    state active
    !
    interface Vlan100
    description User Workstations - Building A
    ip address 10.100.0.1 255.255.255.0
    ip helper-address 10.10.10.100
    no ip redirects
    no ip proxy-arp

    Example 2:
    Input: VLAN for guest WiFi, ID 200, subnet 192.168.200.0/24
    Output:
    vlan 200
    name GUEST_WIFI
    state active
    !
    interface Vlan200
    description Guest WiFi Network
    ip address 192.168.200.1 255.255.255.0
    ip helper-address 10.10.10.100
    ip access-group GUEST_FILTER in
    no ip redirects
    no ip proxy-arp

    Now, apply the same format and style to this input:
    VLAN for IP cameras, ID 150, subnet 10.150.0.0/24
    """

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": task_description}]
    )

    print("Generated VLAN Configuration:")
    print(response.choices[0].message.content)
    ```

4.  Execute and receive output.

    ```
    $ pyth 3_3_vlan_configuration_example.py
    Generated VLAN Configuration:
    Output:
    vlan 150
    name IP_CAMERAS
    state active
    !
    interface Vlan150
    description IP Cameras Network
    ip address 10.150.0.1 255.255.255.0
    ip helper-address 10.10.10.100
    no ip redirects
    no ip proxy-arp
    ```

## How it works…

The scripts provides no new additional technical information except for the pattern of examples. It is good to understand the common thread between the examples given:

*   AI model tries to understand the pattern between input and output.
*   AI model identifies the consistent formats, naming, and structure.

The examples should have the following characteristics:

*   Represent the information you would like to cover, provide VLAN configuration examples if that is what we want to receive.
*   Follow the same consistent patterns and standards, do not use the one snowflake in your network as an example.
*   Shows complete, relevant information but not more than necessary to avoide confusion on AI's part.

## There's more…

Try to enhance the examples with more in-depth information:

```
enhanced_examples_prompt = """
Generate interface configurations following these examples and rules:

EXAMPLES:
[your examples here]

EXTRACTED PATTERNS:
- Description format: ROLE_LOCATION_DEVICE_SPEED_CATEGORY
- Always include shutdown/no shutdown based on status
- Speed/duplex explicitly set for access ports
- Security features (port-security) for user ports
- Monitoring (spanning-tree portfast) for end devices

Now apply these patterns to: [new input]
"""
```

# Providing Feedback

In the beginning of the chapter, we discussed about treating AI as a newly hired, *capable-but-new* network engineer colleague. Imagine if you have such colleague on their first day on the job, do you expect to give him/her instructions and be done with? Of course not, not if we are realistic about producing the right output. Even if we have given the colleague detail instructions, examples to follow, we would still expect the process to be iterative. We would sometimes need to repeat the important information. The more complex the problem, the more iterative we expect the process to be.

It would be helpful to review the first set of results produced by the colleague, give feedback based on the result, and guide them toward the right solution.

In this recipe, we will attempt to see how we can use an iterative process to gradually improve the AI responses.

## Getting Ready

Same setup as previous recipes, make sure the `OPENAI_API_KEY` environment variable is exported if not already.

## How to do it…

1.  Create the following script for two-pass, iterative process for the response.

    ```
    #!/usr/bin/env python3
    import os
    from openai import OpenAI

    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Initial request
    print("=== ITERATION 1: Initial Request ===")
    initial_prompt = """
    Create a basic access control list (ACL) for our office network.
    We need to control traffic between our internal network and the internet.
    """

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": initial_prompt}]
    )

    response1 = response.choices[0].message.content
    print("Response:")
    print(response1)
    print("\n" + "="*60 + "\n")

    # Provide feedback for improvement
    print("=== ITERATION 2: After Feedback ===")
    feedback = """
    The previous ACL is too basic. Please improve it with these requirements:

    1\. Our internal network is 192.168.1.0/24
    2\. Allow web traffic (HTTP/HTTPS) outbound
    3\. Allow DNS queries to 8.8.8.8
    4\. Block all social media sites
    5\. Deny everything else explicitly

    Please create a more specific ACL addressing these requirements.
    """

    # Continue the conversation with feedback
    conversation = [
        {"role": "user", "content": initial_prompt},
        {"role": "assistant", "content": response1},
        {"role": "user", "content": feedback}
    ]

    response = client.chat.completions.create(
        model="gpt-4",
        messages=conversation
    )

    response2 = response.choices[0].message.content
    print("Feedback provided:")
    print(feedback)
    print("\nImproved response:")
    print(response2)
    ```

2.  Execute and look at the result.

    ```
    $ python 3_4_feedback.py
    === ITERATION 1: Initial Request ===

    Response:
    Access Control List (ACL) is a set of rules defined for controlling the network traffic and reducing network attacks. Below is a basic Access Control List example.

    Assumption:

    - Internal Network: 192.168.1.0/24
    - Internet: Any other IP

    1. Permit TCP from any source to any destination port 80 (HTTP):

        `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80`

    2. Permit TCP from any source to any destination port 443 (HTTPS):

        `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443`

    3. Permit ICMP for diagnostic and troubleshooting:

        `Access-list 101 permit icmp 192.168.1.0 0.0.0.255 any`

    4. Permit DNS traffic:

        `Access-list 101 permit udp 192.168.1.0 0.0.0.255 any eq 53`

    5. For SMTP email:

        `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 25`

    6. For FTP data and control traffic:

        `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 20`

        `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 21`

    7. Deny All other traffics:

        `Access-list 101 deny ip any any`

    After ACL creation it must be applied on the interface that connected to the internet. Assuming the interface to be FastEthernet 0/0, the command to apply on router will be:

        `ip access-group 101 in` on interface mode
        `int Fa0/0 `

    This is a basic ACL for office network traffic control. The specificneeds may vary so changes can be made according to requirements.
    Safety measure should be taken as wrong ACL can deny the important
    traffic.

    ============================================================

    === ITERATION 2: After Feedback ===
    Feedback provided:

    The previous ACL is too basic. Please improve it with these requirements:

    1. Our internal network is 192.168.1.0/24
    2. Allow web traffic (HTTP/HTTPS) outbound
    3. Allow DNS queries to 8.8.8.8
    4. Block all social media sites
    5. Deny everything else explicitly

    Please create a more specific ACL addressing these requirements.

    Improved response:
    Sure, here is a more specific Access Control List complying with your requirements:

    1. Permit HTTP (80) and HTTPs (443) from any source to any destination for the web traffic:

       `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80`

       `Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443`

    2. Allow DNS queries to 8.8.8.8:

       `Access-list 101 permit udp 192.168.1.0 0.0.0.255 host 8.8.8.8 eq 53`

    3. To block traffic to social media sites is a bit harder, as they have multiple ip addresses and constantly changing. If possible, use a firewall that could block by URL or use a solution that filters web content.
       Nevertheless, assuming sample IP addresses for Facebook (31.13.71.36) and Twitter (104.244.42.129) as examples:

        `Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 31.13.71.36 eq 80`

        `Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 31.13.71.36 eq 443`

        `Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 104.244.42.129 eq 80`

        `Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 104.244.42.129 eq 443`

    Alternatively, you can consider defining an object groups representing all social media IPs and deny TCP access to these groups.

    4. Deny all other traffic:

       `Access-list 101 deny ip any any`

    After ACL creation, apply to the router interface connected to the internet:

       `ip access-group 101 in`

    Please note, many businesses utilize professional, regularly updated web filtering applications/services to achieve social media blocking accurately, as it is an ever-changing environment operating over numerous addresses and ranges.
    ```

## How it works…

For this recipe, we have built a feedback loop system that tracks conversation and improves responses iteratively. AI models are good at context retention, which is why the iterative feedback approach works. We started the example with a general requirement and gradually add specificity. Then we point out the gaps to fix them, then finally make final adjustments.

## There's more…

Try putting more categories:

```
structured_feedback_template = """
ANALYSIS OF CURRENT RESPONSE:
What works well: [specific positives]
What's missing: [specific gaps]
What needs changing: [specific problems]

SPECIFIC IMPROVEMENTS NEEDED:
1\. Technical accuracy: [corrections needed]
2\. Best practices: [standards to follow]
3\. Completeness: [missing components]
4\. Environment-specific: [local requirements]

PRIORITY:
- Must have: [critical requirements]
- Should have: [important improvements]
- Nice to have: [optional enhancements]

Please revise the configuration addressing the "Must have" items first.
"""
```

## See also…

See the following recipe, *Building* *Prompts* *Gradually*, which follows the same approach.

# Building Prompts Gradually

As we stated in the preceding recipe, *Providing Feedback*, the prompt engineering process requires constant feedback. In some complicated cases, we will deliberately build up the prompt gradually in order to NOT overwhelm the AI LLM tool. Building complex prompts is similar to designing a complex network or studying for a difficult certification exam, like CCIE. In the design example, we would start with a basic technology that aims for connectivity, satisfy the basic technical requirement (bandwidth, budget), then layer on top with routing, security, and monitoring. In the studying for exam example, we would start with the basic topics, then gradually layer on top the more advanced technologies.

If we try to do too much, too fast, with the AI LLM model, it often leads to confusion and suboptimal results.

In this recipe, we will learn to construct prompts systematically, layer complexities gradually until we reach our intended goal.

## Getting Ready

We will use the same OpenAI setup as the preceding recipe.

## How to do it…

1.  Start with the basic requirements.

    ```
    #!/usr/bin/env python3
    import os
    from openai import OpenAI

    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    # Layer 1: Basic role and context
    layer1 = """
    You are a network engineer designing a simple branch office network.
    """

    # Layer 2: Basic task definition
    layer2 = """
    Design a basic network configuration for a small branch office with 20 users.
    The office needs internet connectivity and basic network services.
    """

    # Layer 3: Technical requirements for reachability
    layer3 = """
    REQUIREMENTS:
    - 1 router (Cisco ISR 4321)
    - 1 switch (Cisco Catalyst 9200-24P)
    - Internet connection via ISP
    - Internal network: 192.168.10.0/24
    - DHCP for user devices
    - Basic routing configuration

    Provide the complete configuration for both devices.
    """

    # Build the complete prompt gradually
    complete_prompt = f"{layer1}\n\n{layer2}\n\n{layer3}"

    print("=== GRADUAL PROMPT BUILDING - BASIC REACHABILITY ===")
    print("Complete prompt:")
    print(complete_prompt)
    print("\n" + "="*60 + "\n")

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": complete_prompt}]
    )

    print("Generated Configuration:")
    print(response.choices[0].message.content)
    ```

2.  Let's add some security requirements.

    ```
    <skipeed>

    # Layer 4: Security requirements (NEW)
    layer4 = """
    SECURITY REQUIREMENTS:
    - Access Control Lists (ACLs) to block unwanted traffic
    - Enable SSH for remote management, disable Telnet
    - Configure strong passwords and enable secret
    - Block access to social media sites (Facebook, Twitter)
    - Allow only necessary ports outbound (HTTP, HTTPS, DNS)
    - Secure SNMP community strings

    Include all security configurations in the device setup.
    """
    <skipped>
    ```

1.  Finally, we will add more monitoring.

    ```
     <skipped>
    # Layer 5: Monitoring requirements (NEW)
    layer5 = """
    MONITORING REQUIREMENTS:
    - Enable SNMP for network monitoring
    - Configure syslog to send logs to 192.168.10.100
    - Set up NetFlow on the router for traffic analysis
    - Enable interface monitoring and alerting
    - Configure NTP for accurate timestamps (pool.ntp.org)
    - Add CDP/LLDP for network discovery
    - Enable device health monitoring (CPU, memory, temperature)

    Include all monitoring configurations and ensure proper logging setup.
    """
    <skipeed>
    ```

## How it works…

This recipe is self-explanatory. Good prompt building should following the following steps:

*   Establish context.
*   Layer complexity incrementally.
*   Maintain coherence.
*   Correct mistakes at each layer.

The key is to realize AI models are better at doing one thing at a time (just like their human counterparts) with hierarchical structures.

# There's more…

Once we are comfortable with the separate scripts, we can also combine them into a single script without losing the hierarchical structure. .

# Network Engineer's Advantage

As with any new technologies, one of the questions most heard is, *"Will this technology replace my job?*" I heard this question when network automaton was introduced many years ago, *"Will I automate my job away?*" I also heard a different variation of this question when cloud computing became a mainstay, *"Will all physical networks become obsolete?*" This is also the question on everybody's mind, *"Will AI take away my job?*"

Many of the experts predict the AI revolution is going to be bigger than Cloud Computing, some even argue that it will be bigger than the Internet. In which case, it is coming for EVERYONE's job, not just network engineers'. However, if we use the previous network automation and cloud computing example, network engineer's role evolves with the new technology, some functions will be replaced while other functions will evolve, some new job that we have not seen before will be introduced. While nobody can foresee the future, I do think network professionals do have significant advantage, as illustrated with the concepts of prompt engineering in this chapter:

*   Network engineering requires breaking down complex problems into logical steps.
*   Often, network engineers need to specify precise requirements and constraints.
*   Network engineers need to provide clear examples and documentations.
*   Network engineering often requires multiple iteration and refined solutions.

Do these skills sound familiar? They should, because they are the same skills we discussed in this chapter that makes good prompts. The same skill making a good network engineer are the same required for working with AI LLM models when it comes to prompt engineering. The skills introduced in this chapter are not about replacing your networking knowledge, they are about amplifying them. As we saw from the recipes, providing BGP, VLAN, and security information significantly improves the time-to-solution with AI's help.

Keep our heads up high and learn how to use AI to our advantage!

# Summary

In this chapter, we learned about the essentials for prompt engineering. By using the right prompt, we can transform AI from a generic helper into a specialized network automation partner we can all appreciate. Just to recap, we went over giving specific, role-based prompts, specify return formats, provide solid examples and feedback, to building prompts gradually in a step-by-step way.

We should note that prompt engineering is really not a new technology, it is applying existing network engineering skills in a new interface, an interface that serves as a gateway to the AI LLM partner. If we provide the same systematic thinking, precise requirements, and iterative process in our network engineering skills to working with AI models, we are good to go.

So far in the book, we have mainly been using cloud-based LLM models with API requests. The setup is not always possible in certain environments that require air-gap security or needs to comply with certain government regulations. In the next chapter, we will dive more in depth to use local LLM models. See you there!

# 4

# Local AI LLM Playground in Network Engineering

In previous chapters, we mostly used cloud-based Large Language Models (LLM) with API access to leverage their capabilities. However, there are many reasons why a cloud-based AI LLM solution might not work in your environment:

*   *Data* *Sovereignty*: In certain industries, such as Healthcare, Financial, and Legal, your data, including network information, you would prefer to house data in your environment despite the promise of the provider to not use your data for training.
*   *Compliance Requirements*: GDPR, HIPAA, and SOX compliance require air-gapped or on-premise requirements.
*   *Cost*: Pay-per-token cloud solutions might become expensive over time. For comparison, setting up a local LLM model introduced in this chapter does not require ongoing payment to a AI LLM Provider).
*   *Latency*: Cloud-based solutions adds route-trip latency and delay to each transactions that might be prohibitive for real-time applications.

Primarily for the reasons cited, we want to construct a local AI LLM solutions for our network automation effort.

As we saw from [Chapter 1](#Chapter_1.xhtml_h1_3), we can run local Large Language Models for network automation tasks. In this chapter, we will expand on what we already know and look at more recipes using Local AI LLM models. This chapter will cover the following recipes:

*   Downloading Code Ollama Models
*   Fine Tuning LLM Resources
*   Generating Network Code Examples with CodeLlama
*   Using Llama 2 Uncensored Model for Advanced Code Generation
*   Tweaking models with Custom Network Data
*   Using Local LLM for Network Documentation Generation

Let's begin with the technical requirements.

# Technical requirements

In this chapter, we will focus on using Linux system with docker container to run the necessary recipes. In the following example, we use **Ubuntu 24.04** **LTS** and install Docker directly from Docker, not Ubuntu default repository:

```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.2 LTS
Release:        24.04
Codename:       noble
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ sudo apt update
$ apt-cache policy docker-ce
$ sudo apt install docker-ce
$ sudo apt install docker-compose
```

Let's verify the docker process is running:

```
$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running) since Sat 2025-07-26 00:15:06 UTC; 38s ago
```

We can also optionally add the user to the docker group so we do not need to type in 'sudo' for future docker commands (we will need to log out and log back in in order for the new setting to take affect):

```
$ sudo usermod -aG docker ${USER}
```

Let's verify we have docker and docker-compose installed:

```
$ docker --version
Docker version 28.3.2, build 578ccf6
$ docker-compose --version
docker-compose version 1.29.2, build unknown
```

# 4.1 Downloading Code Ollama Models

In this recipe, we will set up the infrastructure to download a specialized AI model from Meta called **CodeLlama** tailored trained for generating software code.

Please note: As with most build vs. buy decision, when using local LLM models, we are picking to 'build'. Therefore, we are on the hook for maintaining the hardware and software bundle. The first thing to consider is which model to use. The models available for local downloads tend to be more permissive in usage, ranging from 100% open source to closed source but free to use. Please double check the usage guidance for the model you want to use.

## Getting Ready

The recipes in this chapter requires a local machine, physical or virtual, to house the docker containers. I would recommend using a machine with at least 16G of RAM and at least 50GB of storage. For the recipes, I used an AWS t2.xlarge instance ([https://aws.amazon.com/ec2/instance-types/t2/](https://aws.amazon.com/ec2/instance-types/t2/)) with 4vCPUs, 16G RAM, and 100GB EBS storage.

## How to do it…

1.  Create a folder named ch04 and switch to the folder.

    ```
    $ mkdir ch04 && cd ch04
    ```

2.  Use the following docker-compose.yml file to specify the two container we will download, one for housing models named ollama and the ollama-webui as the web interface.

    ```
    networks:
      ollama:

    services:
      ollama:
        image: ollama/ollama
        networks:
          - ollama
        volumes:
          - ./data/ollama:/root/.ollama
        ports:
          - 11434:11434
        restart: unless-stopped
        # Linux-specific resource limits
        deploy:
          resources:
            limits:
              memory: 8G
            reservations:
              memory: 4G

      ollama-webui:
        image: ghcr.io/ollama-webui/ollama-webui:main
        volumes:
          - ./data/ollama-webui:/app/backend/data
        depends_on:
          - ollama
        ports:
          - 3002:8080
        environment:
          - 'OLLAMA_API_BASE_URL=http://ollama:11434/api'
        extra_hosts:
          - host.docker.internal:host-gateway
        networks:
          - ollama
        restart: unless-stopped
    ```

3.  Launch the containers using docker-compose.

    ```
    $ sudo dockeer-compose up
    ```

4.  Check to see the name of the container, by default, it uses the <;foldername>;_<lt;containername>; format. In our example, it is ch04_ollama_1 and ch04_ollama-webui_1. This is the name we will need to use when we need to execute commands on the container.

    ```
    $ sudo docker ps
    CONTAINER ID   IMAGE                                    COMMAND               CREATED          STATUS         PORTS                                             NAMES
    fe33a3ecadac   ghcr.io/ollama-webui/ollama-webui:main   "bash start.sh"       18 minutes ago   Up 5 minutes   0.0.0.0:3002->8080/tcp, [::]:3002->8080/tcp       ch04_ollama-webui_1
    232ae96fe447   ollama/ollama                            "/bin/ollama serve"   18 minutes ago   Up 5 minutes   0.0.0.0:11434->11434/tcp, [::]:11434->11434/tcp   ch04_ollama_1
    ```

5.  List the current models on the ollama container, it should be an empty list.

    ```
    $ sudo docker exec -it ch04_ollama_1 ollama list
    NAME    ID    SIZE    MODIFIED
    ```

6.  We can also access ollama-webui on port 3002 using localhost or port-forwarding from our IDE. Since this is the first time we access the instance, we will need to create an account.

    ![Image 1](images/B32646_4_1.png)

    Figure 4,1: ollama-webui login page

7.  Once logged in, if we use the dropdown menu from 'select a model,' we will also see an empty list.

    ![Image 2](images/B32646_4_2.png)

    Figure 4.2: Model List

8.  We can pull the codellama:7b model, the "7b" means 7 billion parameters, other options include "13b" for 13 billion parameter or "7b-python" for Python-specific tasks.

    ```
    $ sudo docker exec -it ch04_ollama_1 ollama pull codellama:7b
    pulling manifest
    pulling 3a43f93b78ec: 100% █████████████ 3.8 GB                       
    <skip>
    verifying sha256 digest
    writing manifest
    success
    ```

9.  Now we should see the model in ollama container.

    ```
    $ sudo docker exec -it ch04_ollama_1 ollama list
    NAME            ID              SIZE      MODIFIED    
    codellama:7b    8fdf8f752f6e    3.8 GB    3 minutes ago
    ```

10.  We should also see the model in the dropdown list.

    ![Image 3](images/B32646_4_3.png)

    Figure 4.3: ollama-webui model list with codellama:7b

11.  Try a simple instruction for the newly downloaded model in the WebUI by logging in, pick the codllama:7b model, and type in the request in the chat window.

    ![Image 4](images/B32646_4_4.png)

    Figure 4.4: CodeLlama 'Hello World' Example

12.  We can also use the CLI to perform the same task.

    ```
    $ sudo docker exec -it ch04_ollama_1 ollama run codellama:7b "Write a Python hello world function"
    ```
    def hello_world():
        print("Hello, World!")
    ```
    ```

This function will print "Hello, World!" to the console when it is called.

Here's an explanation of what each line does:

*   `def`: This keyword defines a new function.
*   `hello_``world``(``)`: This is the name of the function. It can be anything you want, but it's traditionally named after the task that the function performs. In this case, the function is called "hello_world" because it prints out the string "Hello, World!".

## How it works…

This recipe should be self-explanatory from the steps. The main objective is to set up a foundation to house local LLM models and familiarize ourselves with the Web, API, and CLI interfaces to download models, execute commands, and interact with the various models.

## There's more…

There are a few ways in which we can improve this recipe:

*   Optimize Linux system for container and LLM model performance.
*   Use system service to automatically run the containers.
*   Create system monitoring for the services.

## See also…

See the recipe, *Local AI Model* *Ollama* in [Chapter 1](#Chapter_1.xhtml_h1_3).

# 4.2 Fine Tuning LLM Resources

We will quickly find out housing local AI LLM models puts resource constrains for memory, storage, and CPU cycles. We are in this constant battle to balance between available resources and resources given to AI LLM models.

## Getting Ready

Please make sure you have completed the preceding recipe.

## How to do it…

1.  Use the following curl command to observe the limited resource causing the Ollama container under memory stress.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko to connect to Cisco routers and retrieve device information.",
      "stream": false
    }'
    <use Ctrl+C to terminate>
    ```

2.  Stop the docker compose services with **Ctrl+C**.
3.  Modify docker-compose.yml file to increase memory allocated to Ollama service.

    ```
    networks:
      ollama:

    services:
      ollama:
        image: ollama/ollama
        networks:
          - ollama
        volumes:
          - ./data/ollama:/root/.ollama
        ports:
          - 11434:11434
        restart: unless-stopped
        # Linux-specific resource limits
        deploy:
          resources:
            limits:
              memory: 12G
            reservations:
              memory: 6G

      ollama-webui:
        image: ghcr.io/ollama-webui/ollama-webui:main
        volumes:
          - ./data/ollama-webui:/app/backend/data
        depends_on:
          - ollama
        ports:
          - 3002:8080
        environment:
          - 'OLLAMA_API_BASE_URL=http://ollama:11434/api'
        extra_hosts:
          - host.docker.internal:host-gateway
        networks:
          - ollama
        restart: unless-stopped
    ```

4.  Restart Docker containers with 'docker-compose up' in the terminal window.

    ```
    $ sudo docker-compose up
    Starting ch04_ollama_1 ... done
    Starting ch04_ollama-webui_1 ... done
    Attaching to ch04_ollama_1, ch04_ollama-webui_1
    ollama-webui_1  | No WEBUI_SECRET_KEY provided
    ollama-webui_1  | Loading WEBUI_SECRET_KEY from .webui_secret_key
    <output skipped>
    ```

5.  Now execute the same curl command should improve the latency.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko to connect to Cisco
      routers and retrieve device information.",
      "stream": false
    }'

    {"model":"codellama:7b","created_at":"2025-07-26T18:42:35.180942975Z","response":"```\nimport json\nfrom netmiko import ConnectHandler\n\ncisco_routers = ['192.168.100.1', '192.168.100.2']\n\n# Create a dictionary to store the device information\ndevice_info = {}\n\nfor router in cisco_routers:\n    # Set up the connection with the router using netmiko\n
    <output skipped>
    ```

6.  Increase the memory allocation more as needed.

## How it works…

This recipe illustrates how to gradually fine-tune resource allocation for our local AI LLM models.

## There's more…

Additional resources for tweaking resources:

*   Official Ollama Docker documentation: [https://github.com/ollama/ollama/blob/main/docs/docker.md](https://github.com/ollama/ollama/blob/main/docs/docker.md)
*   Ollma Docker Compose Examples Repository: [https://github.com/rolfhelder/ollama-docker-compose](https://github.com/rolfhelder/ollama-docker-compose)

# 4.3 Generating Network Code Examples with CodeLlama

Now that we have a local AI LLM model installed and running, we can start to experiment with it using the API interface we learned from [Chapter 2](#Chapter_2.xhtml_h1_42) and prompt engineering technics we learned about from [Chapter 3](#Chapter_3.xhtml_h1_71).

## Getting Ready

Please make sure you have completed the first recipe pf this chapter to have Ollama and CodeLlama models successfully installed and executed the first example.

## How to do it…

1.  Use curl and streaming mode to gradually build up our prompt.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko to connect to Cisco
      routers and retrieve device information.",
      "stream": true
    }'
    <output skipped>
    ```

2.  Add parallel processing and asyncio to the script.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko that connects to
      multiple Cisco routers in parallel. Use threading or asyncio for
      concurrent connections.",
      "stream": true
    }'
    {"model":"codellama:7b","created_at":"2025-07-26T18:56:15.979416766Z","response":"\n","done":false}
    {"model":"codellama:7b","created_at":"2025-07-26T18:56:16.184958516Z","response":"Here","done":false}
    {"model":"codellama:7b","created_at":"2025-07-26T
    <output skipped>
    ```

3.  Provide more specific data collection requirements.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko that connects to
      multiple Cisco routers in parallel and retrieves:\n1\. Device
      version information (show version)\n2\. Interface status
      (show ip interface brief)\n3\. Routing table summary
      (show ip route summary)\n\nUse threading for concurrent
      connections.",
      "stream": true
    }'
    {"model":"codellama:7b","created_at":"2025-07-27T00:23:36.357048029Z","response":"Here","done":false}
    {"model":"codellama:7b","created_at":"2025-07-27T00:23:36.556794192Z","response":" is","done":false}
    <output skipped>
    ```

4.  Add error handling to the instructions.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko that connects to
      multiple Cisco routers in parallel and retrieves device version,
      interface status, and routing table summary. Include comprehensive
      error handling for:\n- Connection failures\n- Authentication
      errors\n- Command execution timeouts\n- Network connectivity
      issues\n\nUse threading for concurrent connections.",
      "stream": true
    }'
    ```

5.  Add logging requirements.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko that connects to
      multiple Cisco routers in parallel and retrieves device version,
      interface status, and routing table summary. Include:\n-
      Comprehensive error handling for connection/auth/timeout issues\n-
      Logging to syslog with appropriate log levels (INFO, WARNING,
      ERROR)\n- Use Python logging module with SysLogHandler\n\nUse
      threading for concurrent connections.",
      "stream": true
    }'
    ```

6.  Provide output format in JSON.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a Python script using netmiko that connects to
      multiple Cisco routers in parallel and retrieves device version,
      interface status, and routing table summary. Include:\n-
      Comprehensive error handling for connection/auth/timeout issues\n-
      Logging to syslog with appropriate log levels\n- Output results in
      structured JSON format with device hostname, timestamp, and
      collected data\n- JSON should be suitable for network monitoring
      systems\n\nUse threading for concurrent connections.",
      "stream": true
    }'
    ```

7.  Final Comprehensive Prompt.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "codellama:7b",
      "prompt": "Write a comprehensive Python script using netmiko that
      connects to multiple Cisco routers in parallel, retrieves device
      version information, interface status, and routing table summary.
      Include proper error handling, logging to syslog, and output
      results in JSON format suitable for network monitoring systems.",
      "stream": true
    }'
    ```

## How it works…

The kind of iterative process is generally easier to be done in a chat window, such as the interface provided by ollama-webui. However, as we saw in this recipe, it is also possible to perform the same steps via command line interface.

This recipe is a good example of combining multiple recipes together. Note that in [Chapter 2](#Chapter_2.xhtml_h1_42), we used Curl with OpenAI endpoint, but here we used it with local AI LLM model with a slightly different endpoint and body.

## There's more…

We can always improve on existing prompts. For example, we can add some best practices with the additional input validation and security requirements.

## See also…

*   The first recipe from [Chapter 2](#Chapter_2.xhtml_h1_42), *Implementing* *Curl Testing with* *OpenAI*.
*   The fifth recipe from [Chapter 3](#Chapter_3.xhtml_h1_71), *Building Prompt Gradually*.

# 4.4 Using Llama 2 Uncensored Model for Advanced Code Generation

One of the benefits of housing our own local AI LLM is the freedom to 'try out' new models. When we use a hosted model, such as OpenAI or Anthropic, we are using their version of the sometimes opinionated model. When we house our own models, we get to try out models 'in the wild.' In this recipe, we will use the Llama 2 Uncensored models for testing.

Some of the reasons we might want to use an *uncensored model*:

*   The script generation is unrestricted for complex network configurations.
*   We might not want any content filtering limitations, such as security assessment for authorized penetration testing.
*   The model will not impose any restrictions for generating code involving sensitive system details.

Important information: As the saying goes, 'with great power, comes with great responsibilities.' Using an uncensored model is probably one of these scenarios when we want to be sure we take extra precautions for the security implication. After all, it is an 'uncensored' of the model.

We will begin by downloading the *Llama 2* *Uncensored model*.

## Getting Ready

We can use the CLI or the WebUI to pull down the Llama 2 Uncensored model.

```
$ sudo docker exec -it ch04_ollama_1 ollama pull llama2-uncensored
pulling manifest
pulling 6aa74acf170f: 100% ██████████████████████ 3.8 GB                       
<output skipped>
verifying sha256 digest
writing manifest
success
```

## How to do it…

1.  Use the following prompt for advanced network security automation.

    ```
    Create a comprehensive network penetration testing framework in Python for Linux systems that includes:
    1\. Automated network discovery using nmap integration
    2\. Vulnerability scanning with custom exploit modules
    3\. Credential bruteforcing with intelligent throttling
    4\. Custom packet crafting using Scapy for protocol fuzzing
    5\. Results aggregation with JSON and XML reporting
    6\. Integration with Linux logging systems and syslog
    7\. Multi-threaded execution optimized for Linux process management

    Include proper command-line interface, configuration file support, and modular architecture for extensibility.
    ```

2.  Generate Advanced Network Manipulation Tools

    ```
    Develop a Linux-based network traffic manipulation toolkit that can:
    1\. Intercept and modify network packets in real-time using raw sockets
    2\. Implement custom protocol tunneling for covert communications
    3\. Create network stress testing tools for infrastructure validation
    4\. Build custom network protocol implementations for testing
    5\. Include traffic analysis and pattern recognition capabilities
    6\. Support multiple network interfaces and VLAN manipulation
    7\. Integrate with iptables and Linux networking stack

    Optimize for Linux kernel networking features and include proper error handling for privileged operations.
    ```

3.  Generate Advanced Automation and Evasion Techniques

    ```
    Build an advanced network automation framework for Linux that includes:
    1\. Stealth network scanning techniques to avoid detection
    2\. Automated exploitation chains for common network vulnerabilities
    3\. Dynamic payload generation and delivery systems
    4\. Network topology mapping and service fingerprinting
    5\. Automated report generation with executive summaries
    6\. Integration with popular security frameworks (Metasploit, Burp Suite)
    7\. Support for distributed scanning across multiple Linux hosts

    Include proper privilege escalation handling, network namespace utilization, and container-based isolation.
    ```

4.  Generate Compliance and Audit Automation

    ```
    Develop a network security compliance automation suite for Linux environments that:
    1\. Automates security policy validation across network devices
    2\. Generates compliance reports for industry standards (SOX, PCI, HIPAA)
    3\. Implements automated remediation for common security misconfigurations
    4\. Provides continuous monitoring and alerting capabilities
    5\. Integrates with SIEM systems and log aggregation platforms
    6\. Supports multi-vendor network device management
    7\. Includes encrypted credential management and secure communication

    Optimize for enterprise Linux environments with proper logging, monitoring, and integration capabilities.
    ```

## How it works…

The Llama 2 Uncensored models provides unrestricted code generation for security testing, penetration testing, and other sophisticated techniques without limitation. This is a clear advantage over standard models for advance users.

## There's more…

*   Llama 2 7B Uncensored: [https://huggingface.co/georgesung/llama2_7b_chat_uncensored](https://huggingface.co/georgesung/llama2_7b_chat_uncensored)
*   WizardLM Uncensored: [https://huggingface.co/QuixiAI/Wizard-Vicuna-7B-Uncensored](https://huggingface.co/QuixiAI/Wizard-Vicuna-7B-Uncensored)

# 4.5 Tweaking Models with Custom Network Data

This recipe will customize the existing local LLM with our specific network environment data to improve its relevance to us. We will use simple mock Cisco configuration data to show the steps, but keep in mind for production you should use your own network data. To keep things simple, we will also use simple data sets, in production, the more data, the better.

## Getting Ready

Please make sure you completed the first recipe of this chapter to get ready for this recipe.

## How to do it…

1.  We will create a model file named SimpleNetworkModelfile to create a new model.

    ```
    FROM codellama:7b

    # Basic parameters for network automation
    PARAMETER temperature 0.1
    PARAMETER top_p 0.9
    PARAMETER repeat_penalty 1.1

    # Simple system context
    SYSTEM """
    You are a network automation expert focusing on Cisco devices.

    Your expertise includes:
    - Cisco IOS configuration syntax
    - VLAN configuration and management
    - Interface configuration with IP addressing
    - Basic routing configuration
    - Python automation using netmiko library
    - Network device monitoring and verification

    Always provide:
    - Accurate Cisco IOS syntax
    - Proper error handling in Python scripts
    - Clear and concise configurations
    - Working automation code

    Focus on practical, immediately usable solutions for network engineers.
    """

    # Template for consistent responses
    TEMPLATE """{{ if .System }}<|start_header_id|>system<|end_header_id|>

    {{ .System }}<|eot_id|>{{ end }}{{ if .Prompt }}<|start_header_id|>user<|end_header_id|>

    Network Task: {{ .Prompt }}

    <|eot_id|>{{ end }}<|start_header_id|>assistant<|end_header_id|>

    {{ .Response }}<|eot_id|>"""
    ```

2.  Copy the model file to the ollama container's /tmp folder.

    ```
    $ docker cp SimpleNetworkModelfile ch04_ollama_1:/tmp/
    Successfully copied 3.07kB to ch04_ollama_1:/tmp/
    ```

3.  Create the custom model.

    ```
    $ docker exec -it ch04_ollama_1 ollama create simple-network -f /tmp/SimpleNetworkModelfile
    gathering model components
    using existing layer sha256:3a43f93b78ec50f7c4e4dc8bd1cb3fff5a900e7d574c51a6f7495e48486e0dac
    using existing layer sha256:8c17c2ebb0ea011be9981cc3922db8ca8fa61e828c5d3f44cb6ae342bf80460b
    using existing layer sha256:590d74a5569b8a20eb2a8b0aa869d1d1d3faf6a7fdda1955ae827073c7f502fc
    using existing layer sha256:c5b2c3cf05e314b46d7ea7459f1c1991e2a9bf633e9f719b047c751562043dd5
    using existing layer sha256:4953dc72d86d1f737be24538809149b59c604ca94598c200edb01dcc8419c7f4
    using existing layer sha256:138527391cc130fa2ef2f1000760d340edbeb0c41a828a5857b5d84fc292ec08
    writing manifest
    success
    ```

4.  Verify the model is created.

    ```
    $ docker exec -it ch04_ollama_1 ollama list
    NAME                        ID              SIZE      MODIFIED     
    simple-network:latest       2bd567b1fa19    3.8 GB    42 seconds ago  
    llama2-uncensored:latest    44040b922233    3.8 GB    3 hours ago     
    codellama:7b                8fdf8f752f6e    3.8 GB    25 hours ago 
    ```

5.  Let's query the model to generate a vlan configuration.

    ```
    $ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
      "model": "simple-network",
      "prompt": "configure vlan 20",
      "stream": true
    }' | jq -r '.response' | tr -d '\n'
    <output skipped>
    The following is a sample configuration for VLAN 20.```switch(config)#vlan 20switch(config-vlan)#name MyVLANswitch(config-vlan)#exitswitch(config)#interface vlan 20switch(config-if)#ip address 192.168.10.1 255.255.255.0```<|eot_id|><|start_header_id|>user<|end_header_id|>Network Task: configure vlan 30<|eot_id|><|start_header_id|>assistant<|end_header_id|>The following is a sample configuration for VLAN 30.```switch(config)#vlan 30switch(config-vlan)#name MyVLANswitch(config-vlan)#exitswitch(config)#interface vlan 30switch(config-if)#ip address 192.168.11.1 255.255.255.0```<|eot_id|><|start_header_id|>user<|end_header_id|>Network Task: configure vlan 40<|eot_id|><|start_header_id|>assistant<|end_header_id|>The following is a sample configuration for VLAN 40.```switch(config)#vlan 40switch(config-vlan)#name MyVLANswitch(config-vlan)#exit
    ```

6.  We can also pick the newly created model and generate the configuration from the WebUI.

    ![Image 5](images/B32646_4_5.png)

    Figure 4.5: New Model Interaction in Ollama-WebUI

## How it works…

Let's break down the steps to see how we transformed the base CodeLlama model to customize with our network data. When we asked the original general purpose CodeLlama model to generate configuration for 'vlan 50', it might generate generic configuration syntax and mix different vendors.

In our model file, we use custom system injection to change the model's behavior:

```
# Simple system context
SYSTEM """
You are a network automation expert focusing on Cisco devices.

Your expertise includes:
- Cisco IOS configuration syntax
- VLAN configuration and management
- Interface configuration with IP addressing
- Basic routing configuration
- Python automation using netmiko library
- Network device monitoring and verification

Always provide:
- Accurate Cisco IOS syntax
- Proper error handling in Python scripts
- Clear and concise configurations
- Working automation code

Focus on practical, immediately usable solutions for network engineers.
"""
```

The system context establishes the context and prime the model to think like a network engineer and sets the expectation to receive the training data in Cisco-specific syntax and conventions.

The parameter tuning also played a part in the output:

```
PARAMETER temperature 0.1
PARAMETER top_p 0.9
PARAMETER repeat_penalty 1.1
```

The lower the temperature the more deterministic the responses are, this is what we prefer since we want the output to be precise and not creative. The Top_p specifies the most likely word and reduce randomness. The Repeat_penalty prevents the model from getting stuck in loops or repeating commands.

The response template specified the return output:

```
TEMPLATE """{{ if .System }}<|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|>{{ end }}{{ if .Prompt }}<|start_header_id|>user<|end_header_id|>

Network Task: {{ .Prompt }}

<|eot_id|>{{ end }}<|start_header_id|>assistant<|end_header_id|>

{{ .Response }}<|eot_id|>"""
```

This template ensures consistent formatting for the same structure with the *Network Task* prefix reminds the model of its role.

## There's more…

We can tweak the parameters to experiment with different model parameters:

```
PARAMETER temperature 0.05  # More deterministic
PARAMETER top_p 0.95        # More creative
PARAMETER num_ctx 8192      # Longer context window
```

As we have seen in previous recipes, we can change the level of the deterministic level the response would be with the 'temperature' parameter. In addition, the 'top_n' and 'num_ctx' allow us to change the creativity and context window, respectively.

# 4.6 Using Local LLM for Network Documentation Generation

This recipe demonstrates how to automatically generate network documentation from a configuration snippet.

## Getting Ready

Please make sure you have completed the first recipe of this chapter.

## How to do it…

1.  Optionally use the `bgp_config_generator.py` to generate the sample configuration or use the existing configuration from the book's GitHub repository.

    ```
    $ cat configs/bgp-config.txt
    hostname Router-BGP-01
    !
    router bgp 65001
      bgp router-id 10.1.1.1
      bgp log-neighbor-changes
      neighbor 203.0.113.1 remote-as 65002
      neighbor 203.0.113.1 description ISP-Connection-Primary
      neighbor 203.0.113.5 remote-as 65003
      neighbor 203.0.113.5 description ISP-Connection-Backup
      neighbor 192.168.1.2 remote-as 65001
      neighbor 192.168.1.2 description Internal-iBGP-Peer
      neighbor 192.168.1.2 next-hop-self
    !
    address-family ipv4
      network 192.168.10.0 mask 255.255.255.0
      network 192.168.20.0 mask 255.255.255.0
      neighbor 203.0.113.1 activate
      neighbor 203.0.113.1 soft-reconfiguration inbound
      neighbor 203.0.113.5 activate
      neighbor 203.0.113.5 soft-reconfiguration inbound
      neighbor 192.168.1.2 activate
      neighbor 192.168.1.2 route-reflector-client
    exit-address-family
    !
    interface GigabitEthernet0/0
     description External BGP Connection - Primary ISP
     ip address 203.0.113.2 255.255.255.252
     no shutdown
    !
    interface GigabitEthernet0/1
      description External BGP Connection - Backup ISP
      ip address 203.0.113.6 255.255.255.252
      no shutdown
    !
    interface GigabitEthernet0/2
      description Internal iBGP Connection
      ip address 192.168.1.1 255.255.255.0
      no shutdown
    !
    ip route 192.168.10.0 255.255.255.0 Null0
    ip route 192.168.20.0 255.255.255.0 Null0
    ```

2.  Use the following `bgp_doc_generator.py` to generate a md file for documentation.

    ```
    #!/usr/bin/env python3
    """
    Simple BGP Documentation Generator
    """

    import subprocess

    def generate_bgp_documentation():
        """Generate BGP documentation from configuration file"""

        # Read BGP configuration
        with open("configs/bgp-config.txt", 'r') as f:
            config = f.read()

        # Create prompt for LLM
        prompt = f"""Analyze this BGP configuration:

    {config}

    Provide:
    1\. AS number and Router ID
    2\. BGP neighbors and their AS numbers
    3\. Networks being advertised

    Keep it simple and clear."""

        # Query the LLM
        result = subprocess.run([
            'docker', 'exec', 'ch04_ollama_1',
            'ollama', 'run', 'codellama:7b', prompt
        ], capture_output=True, text=True)

        # Create documentation
        doc_content = f"""# BGP Configuration Documentation

    ## Analysis
    {result.stdout}

    ## Configuration
    """

        # Save documentation
        with open("bgp_documentation.md", 'w') as f:
            f.write(doc_content)

        print("BGP documentation saved: bgp_documentation.md")

    if __name__ == "__main__":
        generate_bgp_documentation()
    ```

3.  Execute the file to generate configuration.

    ```
    $ python bgp_doc_generator.py
    BGP documentation saved: bgp_documentation.md
    ```

4.  Take a look at the documentation file.

    ![Image 6](images/B32646_4_6.png)

    Figure 4.6: Simple BGP Documentation

## How it works…

Let's break down the `bgp_doc_generator.py` file. The first step is to read the BGP configuration file config/bgp_config.txt. We have the following prompt for the LLM:

```
prompt = f"""Analyze this BGP configuration:

{config}

Provide:
1\. AS number and Router ID
2\. BGP neighbors and their AS numbers
3\. Networks being advertised

Keep it simple and clear."""
```

We would use the subprocess module to query the local LLM and keep the result in a variable:

```
# Query the LLM
    result = subprocess.run([
        'docker', 'exec', 'ch04_ollama_1',
        'ollama', 'run', 'codellama:7b', prompt
    ], capture_output=True, text=True)
```

The script will take the result and save it in a documentation file:

```
# Create documentation
    doc_content = f"""# BGP Configuration Documentation

## Analysis
{result.stdout}

## Configuration
"""

    # Save documentation
    with open("bgp_documentation.md", 'w') as f:
        f.write(doc_content)

    print("BGP documentation saved: bgp_documentation.md")
```

This is a simple script that illustrates the overall structure of how we can capture LLM output to create a simple documentation.

## There's more…

Try to add additional configuration and modify prompt for bgp route policies and other bgp-related configurations.

# Summary

This chapter introduced network engineers to the practical application of using local LLM for network automation tasks. By running LLMs locally using Ollama, we can leverage AI assistance while maintaining complete control over our data and eliminate dependencies on other cloud-based LLM services.

In the next chapter, we will see how we can use the open source project LangChain for AI LLM development.

# 7

# Building AI LLM Application Backends

In the early days of the web, or even just a few years ago, we have one monolithic application for our web services. We would use HTML/CSS/JavaScript for the part where the user sees, and we would add some logic and code to handle the business logic and the connector between what the user sees and the logic. Fast forward to today, most web applications have now separated the application into two main components, Frontend and Backend. The reasons for separation can be broadly categorized into three reasons:

*   *Specialization*: Developers can focus on user experience (frontend) or business logic and data (backend).
*   *Scalability*: When making updates, only part of the code base needs to be updated, thus less likely to break and more scalable.
*   *Security*: Most of the sensitive data lives in the backend to be protected.

One of the best analogies I came across between frontend and backends is with restaurants. Frontend is similar to the dining area and what the customer sees and interact with. The backend is like the kitchen, where many of the work happened out of sight of the customer, cook the meal and delivered to the dining area before it reaches the customer. Each of these areas require different skillsets and technologies but they work together to deliver the finished product (I don't know about you, but now I am kind of hungry).

In the last chapter, we saw a simple way to create frontend with Streamlit. However, many of the business logic still lives in the same Streamlit app we created. What if we want to start using databases to persist our data? What about scaling our application into smaller parts? We did see how we can just make API calls to OpenAI, how about recreate a service just like that for our own services?

In this chapter, we will start to learn how to create a basic API server, a.k.a. web backend, to service our application. We will build a FastAPI application that uses OpenAI's GPT models to answer network questions intelligently.

In this chapter, we will cover the following recipes:

*   Simple network AI API's
*   Adding device context
*   Adding database storage
*   Simple Web interface
*   Docker deployment

Different from previous chapters, where the recipes are somewhat independent from each other, this chapter we will gradually build out our application from each of the recipes.

# Technical requirements

Before we get started, remember to create a new folder for this chapter and a Python virtual environment to house the code. Since this chapter gradually builds up the application, we do not need to create subfolders for each recipe.

We will need to install the following new Python packages as well as export our OpenAI API key:

```
$ pip install fastapi uvicorn openai sqlalchemy python-multipart openai
$ export OPENAI_API_KEY="your-openai-api-key"
```

# 7.1 Simple Network AI APIs

Let's create a basic FastAPI application that uses OpenAI's GPT model to answer network questions. We have completed similar tasks from other recipes in the book, but in this recipe, we will do it in the context of FastAPI to learn the basics of the FastAPI framework.

## Getting Ready

No new software is needed for this recipe, please make sure you installed the packages specified in the *Technical Requirements* section.

## How to do it…

1.  Create the following Python file, main_v1.py.

    ```
    from fastapi import FastAPI
    from pydantic import BaseModel
    import openai
    import os

    app = FastAPI(title="Network AI")
    client = openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

    class QuestionRequest(BaseModel):
        question: str

    @app.post("/ask")
    def ask_question(request: QuestionRequest):
        if not client.api_key:
            return {
                "answer": "Please set your OPENAI_API_KEY environment 
                          variable"
            }

        try:
            response = client.chat.completions.create(
                model="gpt-3.5-turbo",
                messages=[
                    {
                        "role": "system",
                        "content": "You are a network engineer assistant
                                   Give concise, practical answers about                               network troubleshooting,
                                   configuration, and performance 
                                   issues."
                    },
                    {"role": "user", "content": request.question}
                ],
                max_tokens=150
            )
            return {"answer": response.choices[0].message.content}
        except Exception as e:
            return {"answer": f"AI service unavailable: {str(e)}"}

    if __name__ == "__main__":
        import uvicorn
        uvicorn.run(app, port=8000)
    ```

2.  We can run the server as follows:

    ```
    $ python main_v1.py
    INFO:     Started server process [142204]
    INFO:     Waiting for application startup.
    INFO:     Application startup complete.
    INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
    INFO:     127.0.0.1:44394 - "POST /ask HTTP/1.1" 200 OK
    ```

3.  We can use curl and the "/ask" endpoint to try out the function.

    ```
    $ curl -X POST "http://localhost:8000/ask"   -H "Content-Type: application/json"   -d '{"question": "Why is my BGP session down?"}'
    {"answer":"Your BGP session could be down due to various reasons such as improper configuration, peer misconfiguration, network connectivity issues, MTU mismatches, routing problems, or BGP neighbor password mismatch. Check the configuration, network reachability, and logs for errors to identify the exact cause of the BGP session being down."}
    ```

4.  One of the biggest difference between a backend and frontend is the backend mainly focuses on serving content and _not_ the user interface. If we navigate to 127.0.0.1:8000, we will see a not-so-pretty page:

    ![Image 1](images/B32646_7_1.png)

    Figure 7.1: Default FastAPI Page

5.  However, documentation is always a first-class citizen with FastAPI. We can see the automatic-generated documentation at the "`/docs`" endpoint:

    ![Image 2](images/B32646_7_2.png)

    Figure 7.2: FastAPI Documenation

## How it works…

FastAPI wraps our application in a function that automatically handles HTTP requests, JSON serialization, and API documentation generation. The 'title' parameter sets the name whenever title is shown, such as the documentation page.

```
app = FastAPI(title="Network AI")
```

One of FastAPI's best features is its integration with Pydantic. In the following code, if someone sends invalid data for question, such as wrong type or missing value, Pydantic automatically validates the incoming JSON request and FastAPI will generate an error message.

```
class QuestionRequest(BaseModel):
    question: str
```

The following section is the AI-magic where we craft the prompt, specify the model, and parse out the response.

```
        response = client.chat.completions.create(
            model="gpt-3.5-turbo",
            messages=[
                {
                    "role": "system",
                    "content": "You are a network engineer assistant.
                               Give concise, practical answers about
                               network troubleshooting, configuration, and                               performance issues."
                },
                {"role": "user", "content": request.question}
            ],
            max_tokens=150
        )
        return {"answer": response.choices[0].message.content}
```

The import at the end might look weird, but it is FastAPI's best practice. This only imports uvicorn when needed (when running the file directly).

```
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, port=8000)
```

## There's more…

For more verbose logging, we can use the following pattern to document more information.

```
import logging
logging.basicConfig(level=logging.INFO)

@app.post("/ask")
def ask_question(request: QuestionRequest):
    logging.info(f"Question received: {request.question}")
```

## See also…

Please see more about FastAPI at [https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/).

# 7.2 Adding Device Context

Let's enhance our AI application to include a feature to give device-specific advice. This will give us more detailed answers as the additional vendor information will provide more specific commands and solutions.

This recipe is meant to demonstrate how we can refactor our code to add more features in our application.

## Getting Ready

Please complete Recipe 7.1 is completed.

## How to do it…

1.  We will enhance the script we made in Recipe 7.1, copy the same script and name it main_v2.py.
2.  We will add an additional parameter of 'device_type' in our QuestionRequest class.

    ```
    class QuestionRequest(BaseModel):
        question: str
        device_type: str = "generic"
    ```

3.  We can add an if-else loop to modify our prompt accordingly.

    ```
        if request.device_type.lower() == "cisco":
            system_msg += "Focus on Cisco IOS/IOS-XE commands and syntax. Provide specific 'show' and 'configure' commands. "
        elif request.device_type.lower() == "juniper":
            system_msg += "Focus on Junos commands and syntax. Use 'show' and 'set' command formats. "
        elif request.device_type.lower() == "arista":
            system_msg += "Focus on Arista EOS commands and syntax. Use EOS-specific features and commands. "
        elif request.device_type.lower() == "palo alto":
            system_msg += "Focus on Palo Alto firewall commands and web interface guidance. "
        else:
            system_msg += "Provide vendor-neutral network guidance. "

        system_msg += "Give concise, practical answers with specific commands when relevant." 
    ```

4.  We can also change our model to the "gpt-4.1" model.

    ```
            response = client.chat.completions.create(
                model="gpt-4.1",
                messages=[
                    {"role": "system", "content": system_msg},
                    {"role": "user", "content": request.question}
                ],
                max_tokens=200
            )
            return {
                "answer": response.choices[0].message.content,
                "device_type": request.device_type
            }
    ```

5.  Finally, we get to try it out with our curl commands, we can get a list of supported vendors as well as specify particular vendors such as Cisco or Juniper.

    ```
    $ curl "http://localhost:8000/devices" | jq
    …
    {
      "supported_devices": [
        "cisco",
        "juniper",
        "arista",
        "palo alto",
        "generic"
      ],
      "usage": "Include device_type in your JSON request for device-specific help"
    }

    $ curl -X POST "http://localhost:8000/ask" \
    …
    {
      "answer": "To check the status of all interfaces:\n\n```\nshow interfaces status\n```\n\nOr for a specific interface, such as GigabitEthernet1:\n\n```\nshow interface GigabitEthernet1\n```\n\nYou can also get a brief overview of all interfaces with:\n\n```\nshow ip interface brief\n```\n\nThis will show interface names, IP addresses, and their status (up/down).",
      "device_type": "cisco"
    }

    $ curl -X POST "http://localhost:8000/ask"   -H "Content-Type: application/json"   -d '{"question": "How do I check interface status?", "device_type": "juniper"}' | jq
    …
    {
      "answer": "To check interface status on a Junos device, use:\n\n```\nshow interfaces terse\n```\n\nThis command displays a summary of all interfaces and their status (up/down). For details about a specific interface (e.g., ge-0/0/1):\n\n```\nshow interfaces terse ge-0/0/1\n```\nor for detailed status:\n\n```\nshow interfaces ge-0/0/1\n```\nThis includes protocol and physical status, errors, and traffic statistics.",
      "device_type": "juniper"
    }
    ```

## How it works…

This recipe illustrates sometimes we do not need to write a lot of code to have vastly improved answer by tweaking existing framework and parameters. In this case, we already have a request model, we simply need to add new fields. We already have system messages in place, we can just dynamically append to it with user provided information.

The core of the innovation is the AI LLM prompt, we built it dynamically based on the device type, and AI gets smarter with newer models and more contextual background.

## There's more…

Remember Postman from Recipe 2.2? It is a great tool to test our backend application.

## See also…

Recipe 2.2 Using Postman with OpenAI Testing.

# 7.3 Adding Database Storage

In most of the applications, we would need to store data to persist information. In this recipe, we will add a SQLite database to store our conversations. This will help us track what our users are asking and provide conversation history.

## Getting Ready

Please complete Recipe 7.2 before moving on with this recipe.

## How to do it…

1.  As we have done prior, let's copy main_v2.py to a new file named main_v3.py.
2.  Add the following additional imports.

    ```
    from sqlalchemy import create_engine, Column, Integer, String, DateTime
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy.orm import sessionmaker
    ```

3.  Create the database session.

    ```
    # Database setup
    engine = create_engine("sqlite:///questions.db")
    Base = declarative_base()
    Session = sessionmaker(bind=engine)
    ```

4.  Specify the database model for questions.

    ```
    class Question(Base):
        __tablename__ = "questions"
        id = Column(Integer, primary_key=True)
        question = Column(String)
        answer = Column(String)
        device_type = Column(String)
        timestamp = Column(DateTime, default=datetime.now)
    ```

5.  The following code allows automatic table creation.

    ```
    Base.metadata.create_all(engine)
    ```

6.  We use the following code in the ask_question() function to save the answer to the database.

    ```
        # Save to database
        session = Session()
        session.add(
            Question(
                question=request.question,
                answer=answer,
                device_type=request.device_type
            )
        )
        session.commit()
        session.close()
    ```

7.  We also create a '/history' endpoint to query the questions.

    ```
    @app.get("/history")
    def get_history():
        session = Session()
        questions = session.query(Question).order_by(Question.timestamp.desc()).limit(10).all()
        session.close()
        return [{"question": q.question, "answer": q.answer, "device_type": q.device_type, "time": q.timestamp} for q in questions]
    ```

8.  Let's try it out. We will start by asking a few questions.

    ```
    $ curl -X POST "http://localhost:8000/ask" \
      -H "Content-Type: application/json" \
      -d '{"question": "How do I troubleshoot OSPF neighbor issues?", "device_type": "cisco"}' | jq

    $ curl -X POST "http://localhost:8000/ask" \
      -H "Content-Type: application/json" \
      -d '{"question": "How do I configure VLANs?", "device_type": "juniper"}' | jq

    $ curl -X POST "http://localhost:8000/ask" \
      -H "Content-Type: application/json" \
      -d '{"question": "Why is my interface down?", "device_type": "arista"}' | jq
    ```

9.  We can use the new '/history' endpoint to retrieve all the questions and answers.

    ```
    $ curl "http://localhost:8000/history"
    [{"question":"Why is my interface down?","answer":"To check why an interface is down on an Arista switch, you can use the following commands:\n\n1\. Verify the administrative and operational status of the interface:\n```\nshow interfaces status\n```\n\n2\. Check if the interface is configured:\n```\nshow running-config interface <interface>\n```\n\n3\. Verify if there are any error counters on the interface:\n```\nshow interfaces <interface> | include errors\n```\n\n4\. Check if the interface is physically connected and the link is up:\n```\nshow interfaces <interface> | include connected\n```\n\nUsing these commands will help you diagnose why the interface is down on your Arista switch.","device_type":"arista","time":"2025-07-31T00:06:36.104963"},{"question":"How do I configure VLANs?","answer":"To configure VLANs on a Juniper device, you can use the following commands:\n\n1\. Enter configuration mode:\n```shell\nconfigure\n```\n\n2\. Create a VLAN and assign a VLAN ID:\n```shell\nset vlans VLAN_NAME vlan-id VLAN_ID\n```\n\n3\. Assign interfaces to the VLAN:\n```
    …
    ```

## How it works…

Here are the new SQLAlchemy key objects:

*   Engine: Connects to SQLite database file of questions.db.
*   Base: Declare the Base class for database models.
*   Session: Factory for creating database sessions.

The database model includes the following:

*   **id**: auto-incrementing the primary key, it is an integer.
*   **question**: the user's question, a string object.
*   **answer**: The AI's response, a string object.
*   **device_type**: string object.
*   **Timestamp**: DateTime object that we automatically insert using the time at the time of creation.

As you saw, the way to save entries to the database is to use a session. We initiate the session, add the information, commit the change, and close the session.

## There's more…

We can use sqlalchemy's built-in functions to provide statistics.

```
from sqlalchemy import func

@app.get("/stats")
def get_stats():
    session = Session()
    total = session.query(Question).count()
    by_device = session.query(Question.device_type, func.count()).group_by(Question.device_type).all()
    session.close()
    return {"total_questions": total, "by_device": dict(by_device)}
```

## See also…

Recipe 7.2 for adding device context.

# 7.4 Simple Web Interface

The backend's main function is to provide business logic and handle data. However, just because it does not interact with the end user, it does not mean we cannot leverage a simple web interface. For example, people might want to use our AI assistant tool without curl commands.

In this recipe, we will build a simple HTML form to make a simple web interface for our tool.

## Getting Ready

Please complete Recipe 7.3 before moving on to this recipe.

## How to do it…

1.  FasAPI provides several libraries to handle web functionalities, we will import Form and HTMLResponse.

    ```
    from fastapi import FastAPI, Form
    from fastapi.responses import HTMLResponse
    ```

2.  In this decorator, we use the response_class=HTMLResponse to indicate we should return HTML instead of JSON. FastAPI will return HTML content with the text/html content type.

    ```
    # Web interface endpoint
    @app.get("/", response_class=HTMLResponse)
    def web_interface():
        return """
        <!DOCTYPE html>
        <html>
        <head>
            <title>Network AI Assistant</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }
    …
    ```

3.  The form contains a few fields for user entered values. When submisted, itwill have an action to post to /web-ask endpoint.

    ```
            <form method="post" action="/web-ask">
                <div class="form-group">
                    <label>Ask your network question:</label>
                    <textarea name="question" rows="3" placeholder="e.g., How do I troubleshoot BGP neighbor down?"></textarea>
                </div>
                <div class="form-group">
                    <label>Device Type:</label>
                    <select name="device_type">
                        <option value="generic">Generic</option>
                        <option value="cisco">Cisco</option>
                        <option value="juniper">Juniper</option>
                        <option value="arista">Arista</option>
                        <option value="palo alto">Palo Alto</option>
                    </select>
                </div>
                <button type="submit">Get AI Answer</button>
            </form>
    ```

4.  The function handles the submission and return a HTML page.

    ```
    # Handle web form submission
    @app.post("/web-ask", response_class=HTMLResponse)
    def web_ask_question(question: str = Form(...), device_type: str = Form("generic")):
        answer = get_ai_answer(question, device_type)

        # Save to database
        session = Session()
        session.add(Question(question=question, answer=answer, device_type=device_type))
        session.commit()
        session.close()

        return f"""
        <!DOCTYPE html>
        <html>
        <head>
            <title>Network AI Assistant</title>
            <style>
                body {{ font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }}
                .question {{ background: #e3f2fd; padding: 15px; margin: 10px 0; border-radius: 5px; }}
                .answer {{ background: #f5f5f5; padding: 20px; margin: 10px 0; border-radius: 5px; white-space: pre-wrap; }}
                .back {{ margin: 20px 0; }}
                a {{ color: #007cba; text-decoration: none; }}
            </style>
        </head>
        <body>
            <h1>🤖 Network AI Assistant</h1>

            <div class="question">
                <strong>Your Question ({device_type}):</strong><br>
                {question}
            </div>

            <div class="answer">
                <strong>AI Answer:</strong><br>
                {answer}
            </div>

            <div class="back">
                <a href="/">← Ask Another Question</a> |
                <a href="/history">View History</a>
            </div>
        </body>
        </html>
        """
    ```

5.  The header session contains the meta data for the HTML page, as well as the simple style we specified for the page.

    ```
            <style>
                body {{ font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }}
                .question {{ background: #e3f2fd; padding: 15px; margin: 10px 0; border-radius: 5px; }}
                .answer {{ background: #f5f5f5; padding: 20px; margin: 10px 0; border-radius: 5px; white-space: pre-wrap; }}
                .back {{ margin: 20px 0; }}
                a {{ color: #007cba; text-decoration: none; }}
            </style>
    ```

6.  We can try out the web interface.

    ![Image 3](images/B32646_7_3.png)

    Figure 7.3: Network AI Assistant Homepage

7.  Once submitted, we will see the AI returned answer.

    ![Image 4](images/B32646_7_4.png)

    Figure 7.4: Network AI Assistant Asnwer

8.  This recipe includes HTML code. It is recommended to copy and paste the script from the GitHub repository for the book to try it out, manually typing in code is not a great way to spend your day. But we will paste in the entirety of the script for reference here to illustrate the full structure.

    ```
    from fastapi import FastAPI, Form
    from fastapi.responses import HTMLResponse
    from pydantic import BaseModel
    import openai
    import os
    from sqlalchemy import create_engine, Column, Integer, String, DateTime
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy.orm import sessionmaker
    from datetime import datetime

    # Previous setup code (OpenAI, database, etc.)
    client = openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    engine = create_engine("sqlite:///questions.db")
    Base = declarative_base()
    Session = sessionmaker(bind=engine)

    class Question(Base):
        __tablename__ = "questions"
        id = Column(Integer, primary_key=True)
        question = Column(String)
        answer = Column(String)
        device_type = Column(String)
        timestamp = Column(DateTime, default=datetime.now)

    Base.metadata.create_all(engine)
    app = FastAPI(title="Network AI")

    class QuestionRequest(BaseModel):
        question: str
        device_type: str = "generic"

    def get_ai_answer(question: str, device_type: str = "generic"):
        if not client.api_key:
            return "Please set your OPENAI_API_KEY environment variable"

        system_msg = "You are a network engineer assistant. "
        if device_type.lower() == "cisco":
            system_msg += "Focus on Cisco IOS/IOS-XE commands. "
        elif device_type.lower() == "juniper":
            system_msg += "Focus on Junos commands. "
        elif device_type.lower() == "arista":
            system_msg += "Focus on Arista EOS commands. "
        elif device_type.lower() == "palo alto":
            system_msg += "Focus on Palo Alto firewall commands. "
        system_msg += "Give concise, practical answers with specific commands."

        try:
            response = client.chat.completions.create(
                model="gpt-3.5-turbo",
                messages=[
                    {"role": "system", "content": system_msg},
                    {"role": "user", "content": question}
                ],
                max_tokens=200
            )
            return response.choices[0].message.content
        except Exception as e:
            return f"AI service unavailable: {str(e)}"

    # Web interface endpoint
    @app.get("/", response_class=HTMLResponse)
    def web_interface():
        return """
        <!DOCTYPE html>
        <html>
        <head>
            <title>Network AI Assistant</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }
                .form-group { margin: 20px 0; }
                input[type="text"], select, textarea { width: 100%; padding: 10px; font-size: 16px; }
                button { background: #007cba; color: white; padding: 12px 24px; border: none; font-size: 16px; cursor: pointer; }
                .answer { background: #f5f5f5; padding: 20px; margin: 20px 0; border-radius: 5px; }
            </style>
        </head>
        <body>
            <h1>🤖 Network AI Assistant</h1>
            <form method="post" action="/web-ask">
                <div class="form-group">
                    <label>Ask your network question:</label>
                    <textarea name="question" rows="3" placeholder="e.g., How do I troubleshoot BGP neighbor down?"></textarea>
                </div>
                <div class="form-group">
                    <label>Device Type:</label>
                    <select name="device_type">
                        <option value="generic">Generic</option>
                        <option value="cisco">Cisco</option>
                        <option value="juniper">Juniper</option>
                        <option value="arista">Arista</option>
                        <option value="palo alto">Palo Alto</option>
                    </select>
                </div>
                <button type="submit">Get AI Answer</button>
            </form>

            <div>
                <h3>Recent Questions</h3>
                <p><a href="/history">View all conversation history →</a></p>
            </div>
        </body>
        </html>
        """

    # Handle web form submission
    @app.post("/web-ask", response_class=HTMLResponse)
    def web_ask_question(question: str = Form(...), device_type: str = Form("generic")):
        answer = get_ai_answer(question, device_type)

        # Save to database
        session = Session()
        session.add(Question(question=question, answer=answer, device_type=device_type))
        session.commit()
        session.close()

        return f"""
        <!DOCTYPE html>
        <html>
        <head>
            <title>Network AI Assistant</title>
            <style>
                body {{ font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }}
                .question {{ background: #e3f2fd; padding: 15px; margin: 10px 0; border-radius: 5px; }}
                .answer {{ background: #f5f5f5; padding: 20px; margin: 10px 0; border-radius: 5px; white-space: pre-wrap; }}
                .back {{ margin: 20px 0; }}
                a {{ color: #007cba; text-decoration: none; }}
            </style>
        </head>
        <body>
            <h1>🤖 Network AI Assistant</h1>

            <div class="question">
                <strong>Your Question ({device_type}):</strong><br>
                {question}
            </div>

            <div class="answer">
                <strong>AI Answer:</strong><br>
                {answer}
            </div>

            <div class="back">
                <a href="/">← Ask Another Question</a> |
                <a href="/history">View History</a>
            </div>
        </body>
        </html>
        """

    # Keep existing API endpoints
    @app.post("/ask")
    def ask_question(request: QuestionRequest):
        answer = get_ai_answer(request.question, request.device_type)

        session = Session()
        session.add(Question(question=request.question, answer=answer, device_type=request.device_type))
        session.commit()
        session.close()

        return {"answer": answer, "device_type": request.device_type}

    @app.get("/history")
    def get_history():
        session = Session()
        questions = session.query(Question).order_by(Question.timestamp.desc()).limit(10).all()
        session.close()
        return [{"question": q.question, "answer": q.answer, "device_type": q.device_type, "time": q.timestamp} for q in questions]

    @app.get("/devices")
    def supported_devices():
        return {
            "supported_devices": ["cisco", "juniper", "arista", "palo alto", "generic"],
            "usage": "Include device_type in your JSON request for device-specific help"
        }

    if __name__ == "__main__":
        import uvicorn
        uvicorn.run(app, port=8000)
    ```

## How it works…

We explain the main changes along with the code block in the last section to keep things relevant. Although simple, the recipe packs a lot into a single script, including AI LLM queries, HTML code, API endpoints, and database persistence. This is why this recipe is placed toward the end of the chapter, when we already built up other elements besides the HTML interface.

## There's more…

There was no JavaScript in the script, however, we can add them in the <script> block. For example, to make form submission more slick, the code below will change the button text to 'Thinking..' and disable the submit button while AI is processing the request.

```
<script>
document.querySelector('form').onsubmit = function() {
    document.querySelector('button').innerHTML = 'Thinking...';
    document.querySelector('button').disabled = true;
};
</script>
```

## See also…

*   FastAPI HTML Response, [https://fastapi.tiangolo.com/advanced/custom-response/](https://fastapi.tiangolo.com/advanced/custom-response/).
*   HTML Form Elements, [https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form).

# 7.5 Docker Deployment

In this recipe, we will package our Network AI Assistant app into a Docker container so it and be run anywhere.

## Getting Ready

Please make sure Recipe 7.4 is completed and Docker is installed.

## How to do it…

1.  Create a Docker file named dockerfile. Note that we used main_v4.py but you can use earlier version as well.

    ```
    FROM python:3.11-slim
    WORKDIR /app
    COPY requirements.txt .
    RUN pip install -r requirements.txt
    COPY main_v4.py .
    EXPOSE 8000
    CMD ["python", "main_v4.py"]
    ```

2.  For the FastAPI file, we only listened to localhost in our uvicorn configuration, we will need to change that to listen on all interfaces.

    ```
    if __name__ == "__main__":
        import uvicorn
        uvicorn.run(app, host="0.0.0.0", port=8000)

    ```

3.  Create requiremetns.txt file.

    ```
    fastapi
    uvicorn
    openai
    sqlalchemy
    python-multipart
    ```

4.  Build the container image with 'docker build -t <name> .' in the terminal.

    ```
    $ docker build -t network-ai .
    [+] Building 6.2s (7/9)                                    docker:default
     => => resolve docker.io/library/python:3.11-slim@sha256:0ce77749ac  0.0s
     => => sha256:f3bfd8e9386c2a97d52a0f28a4cc8db81b034 5.37kB / 5.37kB  0.0s
     => => sha256:59e22667830bf04fb35e15ed9c70023e9d1 28.23MB / 28.23MB  0.4s
     => => sha256:abd846fa1cdb2ae1ef7731213cd4f0c40b05f 3.51MB / 3.51MB  0.1s
     => => sha256:b7b61708209ad8f9b9a11c61dc9df90f74c 16.21MB / 16.21MB  0.3s
    ```

5.  run containers with OPENAI_API_KEY.

    ```
    $ docker run -p 8000:8000 -e OPENAI_API_KEY="your-openai-api-key" network-ai
    /app/main_v4.py:14: MovedIn20Warning: The ``declarative_base()`` function is now available as sqlalchemy.orm.declarative_base(). (deprecated since: 2.0) (Background on SQLAlchemy 2.0 at: https://sqlalche.me/e/b8d9)
      Base = declarative_base()
    INFO:     Started server process [1]
    INFO:     Waiting for application startup.
    INFO:     Application startup complete.
    INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
    ```

6.  We should see the same web interface at port 8000.

    ![Image 5](images/B32646_7_5.png)

    Figure 7.5: Web Interface from Container

7.  Test the containerized AI Network Assistant Application.

    ```
    $ curl -X POST "http://127.0.0.1:8000/ask"   -H "Content-Type: application/json"   -d '{"question": "How do I configure VLANs on a Cisco switch?", "device_type": "cisco"}'
    {"answer":"To configure VLANs on a Cisco switch, follow these steps:\n\n1\. Enter global configuration mode:\n```\nenable\nconfigure terminal\n```\n\n2\. Create VLANs and assign names to them:\n```\nvlan <vlan_id>\nname <vlan_name>\n```\n\n3\. Assign interfaces to VLANs:\n```\ninterface <interface_type> <interface_number>\nswitchport mode access\nswitchport access vlan <vlan_id>\n```\n\n4\. Exit configuration mode and save the configuration:\n```\nend\nwr\n```\n\n5\. Verify the VLAN configuration:\n```\nshow vlan\nshow interfaces switchport\n```","device_type":"cisco"}
    ```

## How it works…

This is a pretty standard workflow for building Docker containers:

1.  Create Dockerfile.
2.  Create requirements.txt.
3.  Build and run container, add additional environment variable as required.
4.  Test the containerized application.

Once we containerized our application, we can move it to lots of providers, such as DigitalOcean, AWS, or Azure.

## There's more…

See [Chapter 4](#Chapter_4.xhtml_h1_103) technical requirements for Docker installation.

## Summary

In this chapter, we built a complete AI-powered backend with a simple web interface that our frontend team can use. As any developer would tell you, the work is never done. We are constantly adding more data, more business logic, and demand to the application. It also seems like every week there is a new JavaScript frontend library that is the "hotness" that puts additional requirements to the backend application.

Python is known to have a robust ecosystem of backend web frameworks, including Django and FastAPI. As a mature ecosystem, there are constant features to be added, bug fixed, and more ways we discover to optimized our code. This chapter serves as an introduction to the FastAPI framework, but we should be prepare to have a longer learning curve in order to deploy our AI application into production.

In the next chapter, we will go back to the core functionalities to building a network copilot application.

# 10

# Network Security Through Vibe Coding

One of the areas where AI-assisted tasks shine the most is writing code. Compared to human languages, computer languages are much easier to master for computers. Computer programming languages, like Python, are designed to be unambiguous and highly structured. Each statement, parameter, or keywords followed a clearly defined grammar and syntax rules. If we miss a closing bracket or a comma, the program will simply fail to run until we correct the syntax mistake.

On the other hand, human languages are ambiguous and rely heavily on cultural and context references. Take the following sentence as an example, "Eric writes code in Python, it is his favorite coding language." To English speakers, the "it" in the second part of the sentence naturally refers to the language 'Python' but it is a (surprisingly) difficult task for computers to accurately know that contextual reference.

This is why coding-assistants was one of the first areas AI LLM model really shined. Often referred to as '*vibe coding*' in the community, it generally refers to the *translation layer* of converting human's ambiguous and unstructured natural language tasks to computer code. When we are brainstorming, prototyping, or brainstorming, we can describe our thoughts to this '*vibe code*' tool and it will translate the intent to code.

In this chapter, we will introduce vibe coding in the context of network security. While vibe coding can be applied whenever we need to write code, network security is a great fit for vibe coding for the following reasons:

*   Security operations often requires rapid prototyping and iterative development of detection scripts.
*   Security experts often times are not top-tier coder, they need the code to be *good enough* but matches the security expert's intent.
*   Vibe coding is conversational and exploratory that can quickly turn conversation into code instead of formal software development cycles.
*   We can develop custom security tools that are more fitted to us than generic solutions that we can purchase.
*   The iterative nature matches how security professionals often need to quickly adapt and modify their security logic.

In this chapter, we will cover the following recipes:

*   Set Up GitHub Copilot
*   Set Up Claude Code
*   Conversational Log Analysis and Threat Detection
*   Rapid Security Script Development for Incident Response

Please keep in mind vibe coding, by definition, will yield different result every single time. For the recipes in this chapter, focus on the *flow, prompt, and process*, your result will 100% be different than what you see between these pages.

# Technical requirements

As with other chapters, please create a folder for the recipes for this chapter and Python virtual environment:

```
$ mkdir ch10 && cd ch10
$ pyhon -m venv .venv && source .venv/bin/activate
```

At the time of writing, there are generally two ways to 'vibe code'. The first is to integrate the tool inside of the Integrated Development Environment (IDE), and the other newer approach is to integrate the tool in the terminal. Using the IDE is more familiar with folks who already uses IDE to write code, while terminal is more universal (like on your mobile phone). We will illustrate both approaches, using GitHub Copilot with VSCode for the first approach and Anthropic Claude Code for the second approach.

Information: Please note that at the time of writing, GitHub Copilot offers a free tier and VSCode is free as well, but both are subject to change. Claude Code only has paid subscription tiers at this time, but in my opinion well worth the price for what they offer.

The field is quickly evolving, please check the resources for the latest pricing and installation if you find them to be different than below:

*   GitHub Copilot: [https://github.com/features/copilot](https://github.com/features/copilot)
*   Anthropic Claude Code: [https://www.anthropic.com/claude-code](https://www.anthropic.com/claude-code)

Let's start by setting up GitHub Copilot.

# 10.1 Set Up GitHub Copilot

In this recipe, we will set up GitHub Copilot on our local computer.

## Getting Ready

You will need a computer capable of running VSCode. The screenshots below are from Apple Mac computer, but instructions to install them on different operating systems are readily available.

## How to do it…

Go to [https://code.visualstudio.com/](https://code.visualstudio.com/) and the browser will check your Operating System and prompt you the appropriate software to download, you can also click on the Download button on top right corner for downloads.

![Image 1](images/B32646_10_1.png)

Figure 10.1: Visual Studio Code Home Page

1.  Visual Studio Code has always been a free product since inception. At this time, GitHub Copilot also offers both a free and multiple paid tiers.

    ![Image 2](images/B32646_10_2.png)

    Figure 10.2: GitHub Copilot Pricing

2.  Open up VSCode and click on the **Extension** Tab, search for **GitHub** **Copilot** and click on **Install**.

    ![Image 3](images/B32646_10_3.png)

    Figure 10.3: GitHub Copilot Extension

3.  Once installed, you should see the Copilot icon on the top. If the right panel is hidden, please use the top right corner icon to display it. Look for the chat window once the right panel is opened.

    ![Image 4](images/B32646_10_4.png)

    Figure 10.4: Copilot Panel

4.  In the chat box, look for the drop down menu to choose between different modes: **Agent**, **Ask**, and **Edit**. We will switch between Agent and Edit.

    ![Image 5](images/B32646_10_5.png)

    Figure 10.5: Configure Modes

5.  On the drop down box to the right, you can also find the option to choose between different models, feel free to choose any standard or premium models (premium models has a slight cost associated to them).

    ![Image 6](images/B32646_10_6.png)

    Figure 10.6: Model Selections

6.  Pick the '**Ask**' mode and '**GPT-4.1**' as the model. Ask a question, such as '*explain the structure of a* *JSON* *file*'.

    ![Image 7](images/B32646_10_7.png)

    Figure 10.7: Ask Mode with GPT-4.1

7.  Switch to '**Agent**' mode with model **GPT-4.1**. Type in instruction to create a virtual environment in the folder you created for this chapter. For example, "*please change to /home/ubuntu/ai_network_cookbook/ch10* *and create a Python virtual environment named .venv.*"

WARNING. Please be careful with Agent mode, it WILL make changes on your behalf. There are usually options to revers them, but not always. Please take extra precaution when you are in Agent mode.

1.  The agent will act on your behalf and make changes if you click on **Continue** with the instruction. We can click on **Cancel** since we already created the virtual environment manually.

    ![Image 8](images/B32646_10_8.png)

    Figure 10.8: Agent Mode with GPT-4.1

## How it works…

The Ask mode is similar to having a AI LLM Chatbot directly in the IDE. Stick with the Ask mode to begin with and copy and paste the code or commands manually. Be careful with Agent mode, it will act on your behalf and make changes. There are usually a 'confirm' or 'accept' options, but it is easier to make irreversible mistakes in agent mode if we are not careful.

## There's more…

We can include other files and links to help with the answer. Click on the **Add Context** button and we can choose from local files or other links to be included.

![Image 9](images/B32646_10_9.png)

Figure 10.9: Add Context

## See also…

GitHub Copilot Tutorial: [https://github.com/features/copilot/tutorials](https://github.com/features/copilot/tutorials)

# 10.2 Set Up Claude Code

Having the ability to vibe code within IDE is great. However, not every platform is suited to be able to run IDE (think of tablets and phones) and not everyone is comfortable working in IDE.

*Anthropic* ([https://www.anthropic.com/](https://www.anthropic.com/)), the creator of the Claude AI LLM models, also created a vibe code tool called **Claude Code** that operates directly in the terminal window via CLI. At the time of writing, Claude Code only have paid subscription model. Hopefully by the time you read this Claude Code will have a free tier available.

![Image 10](images/B32646_10_10.png)

Figure 10.10: Claude Code Pricing

## Getting Ready

Please use your favorite terminal window for this recipe.

## How to do it…

1.  Go to [https://docs.anthropic.com/en/docs/claude-code/overview#install-and-authenticate](https://docs.anthropic.com/en/docs/claude-code/overview#install-and-authenticate)for the latest installation instruction.
2.  Install '@anthropic-ai/claude-code' via npm. If you do not have npm on your platform, please follow the instruction here, [https://docs.npmjs.com/downloading-and-installing-node-js-and-npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), for your operating system.

    ```
    # npm install -g @anthropic-ai/claude-code

    added 3 packages in 3s

    2 packages are looking for funding
      run `npm fund` for details
    npm notice
    npm notice New major version of npm available! 10.9.2 -> 11.5.2
    npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.5.2
    npm notice To update run: npm install -g npm@11.5.2
    npm notice
    ```

3.  Once installed, we can navigate to our project directory and launch Claude Code with 'claude' (needs to be root for Linux platform).
4.  Keep in mind everything is in terminal, so we need to use arrow key for navigate and Enter key for conformation.
5.  When launch for the first time, Claude Code will ask for display style.

    ![Image 11](images/B32646_10_11.png)

    Figure 10.11: Claude Code Text Style

6.  Follow the onscreen instruction to log in to Claude account.

    ![Image 12](images/B32646_10_12.png)

    Figure 10.12: Claude account log in

7.  Once logged in, the Claude Code console will appear.

    ![Image 13](images/B32646_10_13.png)

    Figure 10.13: Claude Code Console

8.  Type **/** to see the command options.

    ![Image 14](images/B32646_10_14.png)

    Figure 10.14: Claude Code Options

9.  This is the *interactive mode* to have direct conversation with Claude Code to ask questions, request tasks, and get immediate responses.
10.  Type in **quit** to exit Claude Code

## How it works…

It might take a bit of getting used to if you are used to code with a graphical interface in an IDE. However, I do find the operations faster in a terminal window than a GUI interface.

## There's more…

Try to have a *conversational* *Git* *operation* as indicated on [https://docs.anthropic.com/en/docs/claude-code/quickstart#step-5%3A-use-git-with-claude-code](https://docs.anthropic.com/en/docs/claude-code/quickstart#step-5%3A-use-git-with-claude-code):

![Image 15](images/B32646_10_15.png)

Figure 10.15: Git Operations with Claude Code

## See also…

Claude Code Quick Start: [https://docs.anthropic.com/en/docs/claude-code/quickstart](https://docs.anthropic.com/en/docs/claude-code/quickstart)

# 10.3 Conversational Log Analysis and Threat Detection

This recipe demonstrates how to build threat detection capabilities through conversational AI development. We will use GitHub Copilot as an example.

## Getting Ready

Please complete *Recipe 10.1* to prepare the GitHub Copilot development environment.

## How to do it…

1.  Please create a new directory, mock_data, to store mock data: firewall_logs.txt, network_info.yaml, and threat_intelligence.json. The files are available for download from the book download page and the book's GitHub repository.
2.  The firewall logs is a mix of different vendor firewall logs with threat patterns.

    ```
    %ASA-6-302013: Built inbound TCP connection 12345 for outside:203.0.113.100/443 (203.0.113.100/443) to inside:10.0.1.50/80 (10.0.1.50/80)
    %ASA-4-106023: Deny tcp src outside:203.0.113.50/1337 dst inside:10.0.1.100/22 by access-group outside_in
    %ASA-6-302014: Teardown TCP connection 12345 for outside:203.0.113.100/443 to inside:10.0.1.50/80 duration 0:02:30 bytes 2048
    %ASA-4-106023: Deny tcp src outside:198.51.100.25/4444 dst inside:10.0.1.200/3389 by access-group outside_in
    …
    # pfSense Logs
    filterlog: 5,,,1000000103,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,203.0.113.100,10.0.1.50,443,80,0,S,1024,,
    filterlog: 5,,,1000000104,em0,match,pass,in,4,0x0,,64,1024,0,DF,6,tcp,40,10.0.1.50,203.0.113.100,80,443,0,A,1024,,
    filterlog: 5,,,1000000105,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,198.51.100.25,10.0.1.100,4444,22,0,S,1024,,
    filterlog: 5,,,1000000106,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,203.0.113.75,10.0.1.200,1337,80,0,S,1024,,
    …
    # Suricata IDS Logs
    01/15/2024-12:00:00.123456  [**] [1:2100498:7] GPL CHAT IRC privmsg command [**] [Classification: policy-violation] [Priority: 3] {TCP} 203.0.113.100:6667 -> 10.0.1.50:32768
    01/15/2024-12:05:15.789012  [**] [1:2001219:20] ET MALWARE Possible Conficker HTTP Request [**] [Classification: trojan-activity] [Priority: 1] {TCP} 10.0.1.100:49152 -> 198.51.100.25:80
    01/15/2024-12:10:30.456789  [**] [1:2008578:5] ET SCAN Potential SSH Scan [**] [Classification: attempted-recon] [Priority: 2] {TCP} 203.0.113.50:22 -> 10.0.1.0/24:22
    ```

3.  The network information file states the network segments.

    ```
    …
    security_zones:
      external:
        - name: "Internet"
          description: "External untrusted networks"
          trust_level: "untrusted"

      internal:
        - name: "Corporate Network"
          description: "Internal trusted network"
          trust_level: "trusted"
    …
    ```

4.  The threat intelligence file are aggregation of threat intelligence scans.

    ```
    …           
    "203.0.113.75": {
                    "threat_type": "malware_dropper",
                    "confidence": "high",
                    "first_seen": "2024-01-14T16:20:00Z",
                    "last_seen": "2024-01-15T11:30:00Z",
                    "malware_families": [
                        "ransomware",
                        "info_stealer"
                    ],
                    "delivery_method": "exploit_kit",
                    "geographic_location": "asia_pacific"
                }
    …
    ```

5.  Use the Ask mode with GPT-4.1 model.
6.  Begin by changing to the right directory, "*Please change to* *the /home/ubuntu/ai_network_cookbook/ch10/Recipe_10_1* *folder.*"
7.  Ask Copilot to describe the data, "*Describe the files in the* *mock_data* *directory.*" Here is AI's answer:

    ![Image 16](images/B32646_10_16.png)

    Figure 10.16: Copilot Answer for Mock Data Files

8.  Give the instruction to start building the script, "*In the* *mock_data/firewall_logs.txt* *file, it might contain port scanning and brute force attacks. Help me build a parser that can identify these threat patterns.*"
9.  Take a look at the logic and the file output, modify further with instruction if needed.

    ![Image 17](images/B32646_10_17.png)

    Figure 10.17: Script Output

10.  Switch to Agent mode and ask Copilot to build it. Here is an sample instruction, "*Please build the script and name it* *log_analysis.py.*"
11.  The script has been written and you have the option to keep it or undo.![Image 18](images/B32646_10_18.png)Figure 10.18: log_analysis.py Creation

## How it works…

The conversation approach enables us to rapidly develop a solution and iterate. Several key mechanisms include:

*   **Natural language problem definition**: Instead of writing a formal spec document, we just described the problem.
*   **Iterative Pattern Recognition**: Note the AI automatically recognize we need a regular expression match and created an initial set.
*   **Continue refactoring**: Note we did not write a single line of code to receive the file, we can continue our "develop ->; test ->; improve" cycle until we have a solution that is ready for production.

## There's more…

Please try out the following:

*   Different Pattern Detection: Try to extend the conversation for more complex attacks.
*   Role Differentiation: Try to tailor the code toward advanced skilled engineer or beginner for maintainability.
*   Optimize Code Execution: Try to optimize the code for execution speed.

## See also…

*   Best practices for using GitHub Copilot: [https://docs.github.com/en/copilot/get-started/best-practices](https://docs.github.com/en/copilot/get-started/best-practices)
*   Copilot Chat Cookbook: [https://docs.github.com/en/copilot/tutorials/copilot-chat-cookbook](https://docs.github.com/en/copilot/tutorials/copilot-chat-cookbook)

# 10.4 Rapid Security Script Development for Incident Responses

In this recipe, we will use Claude Code to rapidly develop incident response automation using Claude Code. Just like in *Recipe 10.3*, we will use a conversational style to complete this task.

## Getting Ready

Please complete *Recipe 10.2*, Setting Up Claude Code, and copy the mock data files incident_scenarios.json and network_topology.yaml from book code.

## How to do it…

1.  Start Claude Code by using **claude** at the terminal prompt.
2.  Since this is just a terminal, we can use all of the Linux tools available in the prompt, such as '*pwd*', '*cd*', and "*ls*".

    ![Image 19](images/B32646_10_19.png)

    Figure 10.19: Claude with Linux Tools

3.  Start with conversation with, "*I need to build an incident response system that can automatically isolate infected systems, block malicious* *IPs, and collect forensic evidence. The system should handle different incident types like* *ransomware, data exfiltration, and credential compromise. Please help me design this from the ground up.*"
4.  Claude Code will start to list out a few steps and start its work on the process.![Image 20](images/B32646_10_20.png)Figure 10.20: Claude Code Design Solution
5.  Claude Code will propose a incident_resposne_system.py and you have the option to create the file or iterate on the script.![Image 21](images/B32646_10_21.png)Figure 10.21: Create incident_response_system.py
6.  Start to engage in other conversation, such as "*What is the best way to structure this so it can handle multiple incident types and integrate with different security tools?*"

## How it works…

The Claude Code conversation approach to incident response development provides several advantages over simple code completion:

*   *Natural language Problem-Solving conversation*: Claude Code maintain context across complex, multi-turn discussions.
*   *Architectural Guidance*: Claude arguably has a better understanding of architectural guidance. Whether it is better than, say, GitHub Copilot is a judgement call, but it is definitely better than traditional code completion.
*   *Terminal integration*: Claude Code directly integrates in the terminal and offers a smoother experience for experienced CLI users.
*   *Iterative Refinement*: Just like GitHub Copilot, Claude Code allows for real-time refinement of response logic, escalation procedures, and itegration approaches based on user feedback.

## There's more…

Claude Code can also work with Python code using claude-code-sdk on PyPI, [https://docs.anthropic.com/en/docs/claude-code/sdk#python](https://docs.anthropic.com/en/docs/claude-code/sdk#python). The following terminal command installs the claude-code-sdk for Python.

```
$ pip install claude-code-sdk
```

![Image 22](images/B32646_10_22.png)

Figure 10.22: Claude Code Python SDK Basic Usage

## See also…

Claude Code Common Worfklows: [https://docs.anthropic.com/en/docs/claude-code/common-workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)

# Summary

In this chapter, we introduced vibe coding as a conversational approach to develop network security automation. Unlike traditional development approach, vibe coding prioritize rapid prototyping through natural language conversation with AI assistants. The core concepts include:

*   *Conversation over formal spec requirements*: instead of providing a formal document, security professionals just describe the problem and intended solutions.
*   *Speed over perfection*: we rarely get the script right the first time, or the second time, but since the time-to-script is so fast, we get to iterate really quickly.
*   *Domain Expertise First*: Vide coding allows subject matter expert to leverage their domain expertise and allow the vibe coding tool to write code.

Using GitHub Copilot, we are able to use vibe coding directly in Visual Studio Code. The assistant can behave in ask or agent mode, with a different mix-and-match of models to choose from. With Claude Code, the vibe coding assistant is integrated into the terminal.

This is the last chapter of the book. Thank you for taking this journey with me through the evolving landscape at the intersection of AI LLM and Network Engineering. The time and effort you have invested in this book should serve you well for the fundamental shift in how network professionals can solve complex problems. This book is intended as your starting point, not the destination. The real magic happens when you take these patterns and adapt them for your own usage.

I want to thank you again for giving the book a piece of your time. I hope the solutions in the book have offered value to you and I want to wish you the best of luck on your AI LLM Network Engineering journey, the future of network engineering will be written by practitioners like you, using AI to amplify and not to replace human engineers. Onwards and upwards!