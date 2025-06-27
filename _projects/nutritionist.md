---
layout: post
title: "Personal Nutritionist"
status: "Live & Secure"
timeline: "June 2025 - Present"
demo_url: "https://my-nutritionist.pages.dev"
tech_stack: ["HTML/CSS", "JavaScript", "Firebase Auth", "Firestore", "OpenAI API", "Cloudflare Pages"]
description: "A web-based nutritionist chatbot that provides personalized nutrition advice using AI"
---

## Overview

A web-based nutritionist chatbot that provides personalized nutrition advice using AI. Users can create health profiles with their medical information, dietary preferences, and fitness goals, then chat with an AI nutritionist that tailors advice based on their specific needs.

The application features user authentication, cross-device profile syncing, and secure API key management - making it both powerful and production-ready.

## Key Features

- AI-powered nutrition advice tailored to individual health profiles
- User authentication with email/password via Firebase Auth
- Health profile management with real-time cross-device syncing
- Secure API key management using environment variables
- Responsive design that works on desktop and mobile
- Multi-layered security to prevent unauthorized API usage

## What I Built

- A conversational AI interface using OpenAI's GPT-3.5-turbo model
- User authentication system with Firebase Auth
- Real-time database integration with Firestore for profile syncing
- Secure serverless functions for API key management
- Responsive chat interface with typing indicators and message history
- Health profile forms with data validation and persistence

## What I Learned

- How to integrate multiple APIs (OpenAI, Firebase) in a single application
- Building secure authentication flows and user management
- Implementing real-time data synchronization across devices
- Managing sensitive API keys using environment variables and serverless functions
- Deploying applications with Cloudflare Pages and automated CI/CD
- Security best practices for preventing unauthorized API access
- Using Git for version control and issue tracking

## Challenges & Solutions

**Challenge:** Discovered unauthorized users were consuming my OpenAI API, leading to unexpected usage and potential costs.

**Solution:** Implemented comprehensive security measures including authentication requirements, UI disabling when not logged in, server-side validation, and multi-layered API protection. Also secured my OpenAI account with 2FA and replaced compromised API keys.

**Challenge:** Users needed their health profiles available across different devices and browsers.

**Solution:** Integrated Firebase Firestore for real-time data syncing, with localStorage as a backup for offline functionality.

**Challenge:** Managing sensitive API keys securely without exposing them in the codebase.

**Solution:** Used Cloudflare Pages environment variables and created a secure serverless endpoint to fetch keys at runtime.

## Technical Architecture

- **Frontend:** Vanilla JavaScript with modular class-based architecture
- **Authentication:** Firebase Auth with email/password providers
- **Database:** Firestore for user profiles and settings
- **AI Integration:** OpenAI GPT-3.5-turbo via REST API
- **Hosting:** Cloudflare Pages with automated deployments from GitHub
- **Security:** Environment variables, serverless functions, and multi-layer validation

## Current Status

The application is live and fully functional with enterprise-level security. Users can sign up, create detailed health profiles, and receive personalized nutrition advice. The system handles cross-device synchronization seamlessly and protects against unauthorized access.

All security vulnerabilities have been addressed, and the application now serves as a solid foundation for future enhancements like meal planning features, progress tracking, and integration with fitness apps.

## Reflections

This project taught me that building production-ready applications involves much more than just making features work. Security, user experience, data persistence, and deployment considerations are equally important.

The security incident was actually a valuable learning experience - it showed me the importance of thinking like an attacker and implementing defense-in-depth strategies. Going through the process of investigating unauthorized usage, securing API keys, and implementing comprehensive authentication made me a much better developer.

Most importantly, I learned that complex applications can be built incrementally. Starting with a simple chat interface and gradually adding authentication, database integration, and security features made the project manageable and allowed me to learn each technology thoroughly.