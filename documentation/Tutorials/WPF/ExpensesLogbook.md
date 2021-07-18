---
layout: page
title: Expenses Logbook introduction
---

[Contents](Contents) [Previous](SimpleLogging) [Next](Categories1)

## Expenses Logbook introduction

### Introduction

In order to continue the tutorial with a real world example (well, more or less), in the next chapters a simple expenses logbook will be created.

The logbook will have following functions initially:

+ A menu with some menu items you can enable and disable
+ A configuration page where you can define categories for expenses
+ A page where you can add expenses
+ A very simple reporting page
+ A settings page where you can configure the application
+ An about dialog window

Some more features may be added in due course to demonstrate Caliburn.Micro features. For a full blown application, it makes sense to use a database. [SQLite](https://www.sqlitetutorial.net/) is a nice solution because it is fully local. [Dapper](https://dapper-tutorial.net/dapper) would nicely complement this for the data-access layer. You can add this functionality yourself. For this tutorial, data will be stored in .csv files.

For this tutorial, we do not aim for the best or most effective solution, but the primary purpose is to demonstrate how Caliburn.Micro can help to solve problems.

### Starting point

If you have followed the previous chapters, you now have a running Caliburn.Micro tutorial application. The tutorial continues from this point, so please make sure to have this working properly.

The first step will be to create a simple maintenance screen to create a list of categories for expenses.

[Contents](Contents) [Previous](SimpleLogging) [Next](Categories1)
