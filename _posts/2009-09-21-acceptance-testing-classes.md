---
layout: post
title: Acceptance Testing Classes
date: 2009-09-21 22:20:11.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- process
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '1044436860'
author: Alan Francis
permalink: "/2009/09/21/acceptance-testing-classes/"
---
I have seen a lot of unit tests in my 10 years as an XP practitioner and coach. I have also had a lot of debates about styles of unit testing. The longest running of these debates is about the role of "mock objects" in TDD.

Martin Fowler has characterised the use of mocks as "interaction testing" in that you are verifying that certain methods get called as a result of stimulating the object under test, rather than that the state of the object changed.

I don't like this style of testing for a couple of reasons.

The first reason, which will really have to be an article on it's own, is that it tends to make refactoring a lot harder than it might otherwise be. This is due to a lot of tests that verify specific methods are called in a specific order. The names and order of these methods can change often early in development, as well as later if a large refactoring is performed.

The second is what I wanted to talk about here.

When every collaborator of a class is mocked, it becomes divorced from reality. It is truly being tested in isolation. We verify that when this fake object pokes it in a certain way it responds by poking these other fake objects in a different way. We have no evidence (at this point) to suggest that the object will be poked in the same ways in production.

What we end up with, I think, is a set of acceptance tests for an individual class.

When I mind map this I end up with "waterfall". We are designing and building and acceptance testing a class in isolation, and increasing the cost of change of that class. We lose a holistic view of the system and are discouraged from refactoring, especially across classes. The software becomes less soft.