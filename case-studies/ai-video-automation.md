# AI-Driven Video Content Automation Platform

## Overview

Developed an AI-driven software system to automate the generation and publishing of video content to YouTube and Instagram.

The system was designed to reduce manual effort involved in video publishing while providing reliable task scheduling, automated recovery, secure authentication, and subscription management.

## Problem

Publishing video content across multiple social media platforms involved significant manual effort. The system needed to automate the publishing workflow while handling:

* Automated video content generation
* Publishing to multiple platforms
* Secure authentication
* Task scheduling
* Failed task retries
* Recovery after server restarts
* Subscription and payment management

## What I Built

I designed and developed full-stack automation workflows that connected video generation with automated publishing to YouTube and Instagram.

The system integrated:

* OAuth 2.0 for secure authentication
* YouTube Data API v3 for YouTube publishing
* Instagram Graph API for Instagram publishing
* Bull Queue for task scheduling and queue management
* Cashfree for subscription and payment processing
* Webhooks for real-time payment and subscription status management

## Key Engineering Work

### Automated Social Media Publishing

Integrated the YouTube Data API v3 and Instagram Graph API to create secure automated publishing workflows.

### Task Scheduling and Recovery

Implemented Bull Queue for advanced task scheduling with retry logic.

The system also supported automatic recovery after server restarts, improving the reliability of the publishing workflow.

### Payment and Subscription Management

Integrated the Cashfree payment gateway to support recurring subscriptions.

The payment system supported:

* International cards outside India
* UPI within India
* Cards within India

Webhooks were implemented to receive and store real-time payment metadata and efficiently manage subscription and payment statuses.

## Technology

* Node.js
* APIs
* OAuth 2.0
* YouTube Data API v3
* Instagram Graph API
* Bull Queue
* Cashfree
* Webhooks

## Results

The automation system achieved:

* **70% reduction in manual posting time**
* **97% reliability in video publishing workflows**

The solution automated video publishing across YouTube and Instagram while providing task scheduling, retry handling, recovery, and subscription management.

## My Contribution

* Designed and developed full-stack automation systems
* Designed scalable backend workflows
* Implemented robust error handling
* Implemented queue management and task scheduling
* Integrated YouTube and Instagram APIs
* Implemented OAuth 2.0 authentication
* Integrated Cashfree payment gateway
* Implemented webhooks for payment and subscription management
* Built retry and recovery mechanisms for background tasks
