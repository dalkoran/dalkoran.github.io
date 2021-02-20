---title: "More Thoughts on Validation"
date: 2007-10-28 14:07
author: spencen
comments: true
categories: [Development]
tags: []
---
I've been thinking a little more about what I want out of a Validation Engine - here are some more random thoughts...
 

Validation Scopes
 

1.  **Business Rules**. These are defined within the application model - normally within the classes used to represent the business objects. Examples include - date of birth that must be historic, cheque number must be unique, expiry date must be greater than start date.  **Form Rules**. These are defined within the form definition - for WPF applications this could be via XAML. They represent ad-hoc rules that are specific to a form. In some cases this will be adding rules to data represented by business objects (e.g. via data-binding) which may already have a set of Business Rules. In other cases they will be applied to un-bound data. An example of an un-bound form rule is a check-box that must be checked by the user to verify they have read the instructions on the screen before being allowed to proceed.  **Data Rules**. These are defined by the restrictions that occur because business data is mapped to some underlying data model. Examples include maximum string lengths, numeric precision (entering 4.000000001 might exceed storage capabilities), non-nullable fields. 

Validation Timeframe
 

1.  **Synchronous**. The validation is immediately performed by the rule yielding either a pass or fail result. Examples of validation that can be synchronous include expiry date being greater than start date.  **Asynchronous**. The validation cannot be immediately determined by the client and requires some potentially long running component. In this scenario the validation rule may yield a "to be determined" result. This will almost certainly be the case if the validation requires access to data not stored on the client and needs to make a complex query direct to the database - or more likely a fairly simple query that is marshaled to the application logic running on the server - for example via a web service. Examples of validation that can be asynchronous include validating that a cheque number is unique. 

Validation Roll-up
 

1.  Know whether a rule has passed or failed. Note that a rule may affect multiple data elements.  Know whether a data element (i.e. property) is valid. Note that a data element may be affected by multiple rules.  Know whether a data object (i.e. business object instance) is valid. Requires that all its data elements are valid.  Know whether a form is valid. Requires that all its data objects are valid. 

Validation Triggers
 

1.  **OnBinding**. Validation occurs during binding updates.  **Forced**. Validation is programmatically forced - either for a single rule, a single business object instance or an entire form.  **Suppressed**. Each rule may be temporarily suppressed. Alternatively all rules may be suppressed via a single method call. 

So far I've checked out Paul Stovell's excellent <a href="http://www.codeproject.com/WPF/wpfvalidation.asp" target="_blank">post on codeproject</a>. Also the <a href="http://www.bennedik.de/2007/03/enterprise-wpf-validation.html" target="_blank">post that this spawned by Martin Bennedik</a> which also discusses the Patterns and Practices team's <a href="http://www.codeplex.com/entlib" target="_blank">Validation Application Block</a> (VA![](http://blog.spencen.com/emoticons/cool.png). <a href="http://blogs.msdn.com/tomholl/archive/2007/01/09/validators-supplied-in-the-vab.aspx" target="_blank">Here's a list</a> of the VAB meta-data rules.
 

I've also been reading up on IdeaBlade's DevForce toolset which has a <a href="http://www.ideablade.com/PDF/VerificationWhitepaper.pdf" target="_blank">good whitepaper</a> on what they refer to as "verification". 


