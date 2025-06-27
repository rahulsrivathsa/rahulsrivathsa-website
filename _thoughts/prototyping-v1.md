---
layout: post
title: "Prototyping with ChatGPT and Claude"
date: 2025-06-27
description: "An approach to building v1s using ChatGPT Custom GPTs and Claude Projects before full apps"
---

# Prototype First: Build AI Tools in Existing Platforms Before Going Custom

Instead of building custom apps, I prototyped two AI tools using existing platforms:

**Claude Projects** → Personal advisor (accesses my Notion journals)  
**ChatGPT Custom GPT** → Personal nutritionist (static health data)

## Why This Worked

### Claude Projects for Personal Advisor
- **Perfect workflow fit:** My journals were already in Notion, MCP writes back seamlessly to create new content
- **Chat interface:** Natural for asking advice
- **Rapid iteration:** Adjust instructions, test immediately

### ChatGPT Custom GPT for Nutritionist  
- **Simple needs:** Just upload health data and nutrition goals
- **Quick setup:** Files + instructions = done

## What I Discovered
**Initial assumption:** "I need semantic search and vector databases"  
**Reality:** File upload + smart instructions solved 80% of my needs. The remaining 20% became much more apparent with a live version that I could use.   
**Key insight:** For applications that heavily rely on a chatbot interface, start with a CustomGPT / Claude Project. 

## The Strategic Question

After 2-4 weeks of daily use: **Do the remaining limitations justify custom development?**

## Bottom Line

In both these cases, I was able to start testing the idea quickly and observing where the limitations were. This helped determine a more robust feature set once I actually sat down to build the app. With the nutritionist for example, I decided to turn it into a web app so other friends and family could use it, I could add in weight tracking, and the ability to save insights that then were emailed back to me as a weekly report. 

**Prototype in existing AI platforms first.** Let real usage drive technical decisions, not theoretical requirements. You'll save months of development and build something you actually use.

The fastest way to build great AI tools is to start with tools that already exist.