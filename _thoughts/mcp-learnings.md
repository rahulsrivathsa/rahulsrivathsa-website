---
layout: post
title: "Initial explorations with MCPs and Claude"
date: 2025-06-27
description: "What I learned from trying to use Notion MCP via Claude"
---

# Key Learnings About MCPs: When They Work (And When They Don't)

I tried building a personal AI advisor using MCPs to access my 188 Notion journal entries. Here's what I learned about their limitations:

## The Problem: MCPs Don't Scale for Large Datasets

**What I needed:** All 188 journal entries for comprehensive advice  
**What happened:** 188+ individual API calls, eating tokens and going very slowly  
**Reality check:** MCPs optimize for real-time queries, not bulk data retrieval

## Where MCPs Struggle

❌ **Bulk exports** - Takes shortcuts, misses important data  
❌ **Comprehensive data analysis** - Too expensive in tokens/time  
❌ **One-time data preparation** - Not designed for this use case

## Where MCPs Excel

✅ **Real-time operations** - Writing new entries back to Notion  
✅ **Targeted queries** - "Get my latest reflection"  
✅ **Live integrations** - Enhancing ongoing conversations

## The Solution: Hybrid Architecture

**Static export** (direct Notion API) for historical context  
**+ Live MCP** for recent entries and writing new content  
**= Best of both worlds**

## Key Insight

For AI advisors, **front-loading context beats real-time retrieval**. It's more efficient to load your entire dataset once than make 50+ API calls every conversation.

## Bottom Line

MCPs are great for enhancing conversations with live data, but they're not built for the heavy lifting of comprehensive data preparation. Know which job you're solving for.