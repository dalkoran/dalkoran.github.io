---
layout: post
title: "Validation Revisited"
date: 2008-05-03 16:10
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


This will be the first of a few posts that attempt to explain how I'm going about building my first prototype WPF application. Everything I've built thus far in WPF has really just been code fragments to try out a small subset of the WPF architecture. The goal of this is to create for myself a WPF application framework designed for building typical line of business (LO![](http://blog.spencen.com/emoticons/cool.png) applications.
 

I'm going to start with the Validation framework that I've blogged about [previously](http://blog.spencen.com/2007/10/15/validation-engine.aspx) and also show how similar concepts applied to the validation can be used to reduce the amount of XAML I have to write in other areas.
 

### Goals

 <table cellspacing="0" cellpadding="2" width="659" border="1"> <tbody> <tr> <td valign="top" width="326">**Goal**</td> <td valign="top" width="331">**Translation**</td></tr> <tr> <td valign="top" width="326">Provide responsive, non-intrusive and comprehensive validation of data.</td> <td valign="top" width="331">Immediate feedback where performance permits.<br>No message boxes!<br>Async processing option.<br>Property level, control level, class level validation.</td></tr> <tr> <td valign="top" width="326">Reduce the amount of business logic duplicated in the presentation layer by harnessing a rich meta-data model exposed by the business layer.</td> <td valign="top" width="331">Define meta-data on business logic preferably using attributes.<br>I consider end-user field names to be business meta-data.</td></tr></tbody></table> 

### Features

 

1.  Validation logic is layer independent, it doesn't rely on base business classes, or any presentation framework elements. (Why do the WPF Validation classes like ValidationResult and ValidationRule live in the System.Windows namespace branch in PresentationFramework.dll?)  Validation Rules can be defined using declarative markup (attributes) or "registered" against an entity at runtime. There is no mandatory requirement for a class being able to participate in the validation. That is to say - no required base business entity, no mandatory interfaces etc.  Provides various extensibility points for easily defining new rules with all the same features.  Validation results allow for rich information to be provided about the rule violation and the context in which it occurred. For example, the results are tied back to the property, instance or control responsible. This happens regardless of whether the rule violation was as a direct result of a control value changing, or whether it was due to a business rule running on either client or server. 

### Validation Rules

 

![WorkingArea](/images/WorkingArea_5.png) The basis for this ValidationEngine is the abstract ValidationRule class. Derived classes are responsible for validating a provided value. If the value does not violate the rule then a standard "passed" result is returned. Otherwise the rule is responsible for constructing a message that describes why the value didn't pass. 
 

The message can be the default text defined against the class or one that is defined specific to the validation instance. Either method has the opportunity to use standard tokens that are defined by the rule, or derived from the context. For example a ValidationRule applied to a class property will have the DisplayName of the property made available to it for construction a standard or overridden message, e.g. "The {property} is a mandatory field and requires a value to be entered."
 

Similar to the System.Windows.Control.ValidationRule class the&nbsp; abstract Validate method is where the actual validation occurs. The signature for Validate is the same making it easy to create a generic wrapper to convert any existing ValidationRule for use in this framework.
 

The ValueProvider property is a reference to an object (implementing IValidationValueProvider) that is responsible for providing a value to the ValidationRule when executed. The simple scenario here is a provider that uses a PropertyDescriptor to get a value of the property to which the ValidationRule is registered when performing validation against an instance.
 

Priority is used to define the default order in which rule violations should be ranked.
 

Severity is used to determine whether the rule violation is indicative of an error, a warning, a hint etc. This is also used to determine whether the violation will trigger IsValid on the Validator.
 

### Applying Rules via Attributes

 

![IAttributeRuleProvider](/images/IAttributeRuleProvider_3.png) The easiest method of assigning ValidationRules is to decorate the business entities with attributes (a very common approach). This is made very simple by having an IAttributeRuleProvider interface. Any attribute class that implements this interface is responsible for generating one or more ValidationRules via its GetRules method that will be applied against the entity itself or an entity property (depending on where the attribute was placed).
 

For example, say you create a new Attribute that allows for string properties to be further defined by nominating them as URLs, e-mail addresses, phone numbers etc. By having your Attribute implement IAttributeRuleProvider you could ensure that a ValidationRule is automatically assigned by the AttributeRuleProvider. In this example it may be that it simple returns a RegEx ValidationRule which the regular expression specific to the string type.
 

### Performing the Validation

 

![IValidator](/images/IValidator_3.png) The Validation of ValidationRules is performed by an IValidator. There are currently two types of IValidators, inheriting from a common abstract Validator class. The EntityValidator which can validate rules against a class instance and uses the INotifyPropertyChanged interface to optionally keep executing the rules. The other is a FormValidator which extends the base implementation with knowledge about UI elements. For instance it allows validation to be performed against an unbound control, and has the capability of tying business properties back to any control that they may be bound to for error notification.
 

IValidators can be nested. In this scenario when they are asked to Validate they will also call Validate on each of their child IValidators. Also there IsValid property will only become true if all child IValidators also have their IsValid property being true. A common usage here might be to have a FormValidator that is responsible for validation form specific rules which has child IValidators for each business entity enable for updating.
 

A ValidationValueProvider is responsible for providing the Validator with both a value to test and a context for that value. The value is passed to the ValidationRule for validation and the context is attached to any violation result.
 

A Validator is created that is responsible for keeping track of the current validity of all objects that are registered with it. The Validator gets its rules from one or more rule providers that implement IValidationRuleProvider.
 

### Validating in the Business Layer

 

This is easy, there are two simple options. 
 

First choice is to create an EntityValidator instance and call the RegisterEntity method on it to register your business class that has been decorated with IAttributeRuleProvider attributes. Then call Validate and check use the IsValid and Ful
lResults properties to determine success/failure. Using this method it is possible for the business class to have no knowledge of the validation logic.
 

Second choice is to create a base business entity class that has this functionality built it (or extend your existing base business entity class). This could then provide either direct access to an instance EntityValidator, or simply expose the properties you require as wrappers around the internal EntityValidator.
 

### Visualising Validation Results in the Presentation Layer

 

**ErrorTemplate**
 

This is in a state of flux. Currently I'm using the System.Windows.Controls.Validation class to highlight errors using its ErrorTemplate attached property. I started using IDataErrorInfo but it seemed to be more of a hindarance than anything. The only real benefit was its integration with the Validation properties via DataErrorValidationRule, but initial results seem that I can achieve this a different way.
 

**ValidationPanel**
 

A very simple custom control that derives from ItemsControl. Its ItemsSource is linked to the FullResults property of the forms/presenters top level IValidator. How the errors are displayed is really just dependent upon the styling applied to the control. However, I've added some very minimal functionality to start with. For example, clicking on a Validation Result whose context can be linked to a bound control will cause that control to receive focus.
 

### Binding Exceptions

 

In my framework Binding exceptions are caught by the FormValidator. This means that if data is entered into a control that is not possible for the Binding to coerce into the bound source the FormValidator will become IsValid=False even though the business entity Validator to which the control is bound may remain IsValid=True. This felt like the obvious choice - the value never gets to the underlying business entity - yet the form still needs to notify of the error and prevent a Save etc. since what appears on screen (the bound control retains the un-bindable value) is not reflective of the model.
 

### What's left to be done?

 

Lots.
 

### So where is the code?

 

Its coming soon...


