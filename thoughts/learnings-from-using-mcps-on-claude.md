---
title: Learnings from using MCPs on Claude
date: 2025-06-26T22:11:00.000Z
description: "My learnings from using MCP server. "
---
**What I Was Trying to Accomplish**

I wanted to build a personal AI advisor that could:



Reference my complete reflection history (188+ entries) for contextual advice

Speak in my voice and tone

Record new journal entries back to Notion

Give advice with inline citations to my past insights



Core MCP Limitations I Discovered

1. API Call Inefficiency for Large Datasets



The Problem: 188 entries = 188+ individual API calls

Token Cost: Each call consumes Claude conversation tokens

Speed: Sequential API calls are inherently slow

Scalability: Gets exponentially worse as your database grows



2. Not Designed for Bulk Data Export



MCP Strength: Real-time, targeted queries ("get my latest reflection")

MCP Weakness: Exhaustive data retrieval ("export everything")

Key Realization: MCPs are for dynamic interaction, not static data preparation



3. Takes Shortcuts on Complex Data



Missing Content: My first export had metadata but no actual reflection content

Incomplete Parsing: May miss nested structures, formatting, or nuanced data

Quality vs Speed Trade-off: MCPs optimize for quick responses, not comprehensive data capture



What MCPs Are Actually Great For

Real-Time Operations



✅ Writing new journal entries back to Notion

✅ Querying recent/specific entries during conversations

✅ Live integrations that enhance chat experiences



Not Great For



❌ One-time bulk exports

❌ Comprehensive data analysis

❌ Building foundational datasets for AI training



The Hybrid Solution I Landed On

Static Export (Direct Notion API): Complete historical context loaded once

+ Live MCP: For recent entries and writing new content

\= Best of Both: Comprehensive context + real-time updates

Key Insight: Context Window vs API Call Trade-offs

For AI advisors specifically, front-loading context is often better than real-time retrieval. It's more efficient to load your entire reflection history once than to make 50+ API calls every conversation.

The "Data Gravity" Problem

Comprehensive AI advisors need comprehensive data. MCPs work against this by making comprehensive data expensive (in tokens/time), leading to the realization that foundational data needs to be "pre-loaded" rather than retrieved on-demand.

Bottom Line

MCPs excel at enhancing conversations with live data, but struggle with the heavy lifting of comprehensive data preparation. Know which job you're trying to solve for.
