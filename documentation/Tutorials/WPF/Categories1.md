---
layout: page
title: Category Model and ViewModel
---

[Contents](Contents) [Previous](ExpensesLogbook) [Next](CategoryEditorHookup)

## Create a categories user control

In the setup chapter, we created an empty Shell with a placeholder for user controls. In this chapter we will create the first user control and show how to hook it up in the application.

The Categories function allows to define categories for expenses and in the next chapter expenses will be assigned a category.

### Create a model

The first step is to create a simple model and put it into the Models folder. This is a bit of a simplified solution architecture, but let's keep it that way for tutorial purposes and avoid a lot of confusion. This is justified because it is a small application.

So, create a ``CategoryModel`` class that looks like this:

```csharp
namespace Caliburn.Micro.Tutorial.Wpf.Models
  {
  public class CategoryModel
    {
    public string CategoryName { get; set; }
    public string CategoryDescription { get; set; }
    }
  }
```

It has two auto properties and that is all we need. Optionally, you can add an Id property, which you need for storage in a database.

### Create a ViewModel

Tor the next step you can either start creating a View or a ViewModel. Probably you will switch between these two a number of times. For readability in this case a ViewModel is created. Make sure to use the ``ViewModels`` folder. The initial setup for the ``CategoryViewModel`` class looks like this:

```csharp
using Caliburn.Micro.Tutorial.Wpf.Models;

namespace Caliburn.Micro.Tutorial.Wpf.ViewModels
  {
  public class CategoryViewModel : Screen
    {
    private BindableCollection<CategoryModel> _categoryList = new BindableCollection<CategoryModel>();
    private CategoryModel _selectedCategoryModel;
    private string _categoryName;
    private string _categoryDescription;

    public BindableCollection<CategoryModel> CategoryList
      {
      get
        {
        return _categoryList;
        }
      set
        {
        _categoryList = value;
        }
      }

    public CategoryModel SelectedCategory
      {
      get
        {
        return _selectedCategoryModel;
        }

      set
        {
        _selectedCategoryModel = value;
        NotifyOfPropertyChange(() => SelectedCategory);
        }
      }

    public string CategoryName
      {
      get => _categoryName; set
        {
        _categoryName = value;
        NotifyOfPropertyChange(() => CategoryName);
        }
      }

    public string CategoryDescription
      {
      get => _categoryDescription; set
        {
        _categoryDescription = value;
        NotifyOfPropertyChange(()=>CategoryDescription);
        }
      }

    protected override void OnViewLoaded(object view)
      {
      base.OnViewLoaded(view);
      CategoryList.Add(new CategoryModel {CategoryName="Meals",CategoryDescription="Lunched and diners"});
      CategoryList.Add(new CategoryModel { CategoryName = "Representation", CategoryDescription = "Gifts for our customers" });
      }

    public void Edit()
      {
      CategoryName=SelectedCategory.CategoryName;
      CategoryDescription=SelectedCategory.CategoryDescription;
      }

    public void Delete()
      {
      CategoryList.Remove(SelectedCategory);
      Clear();
      }

    public void Save()
      {
      CategoryModel newCategory= new CategoryModel();
      newCategory.CategoryName = CategoryName;
      newCategory.CategoryDescription = CategoryDescription;
      if (SelectedCategory!=null)
        {
        // remove the existing category, needed to update the view
        CategoryList.Remove(SelectedCategory);
        }
      CategoryList.Add(newCategory);
      Clear();
      }

    public void Clear()
      {
      CategoryName= string.Empty;
      CategoryDescription=string.Empty;
      SelectedCategory=null;
      }
    }
  }
```

This is a fairly long code fragment. Now each part of it will be discussed separately. A large part of the ViewModel is dovoted to represent the data you need in the View.

The first thing you can see is that the ViewModel inherits from ``Screen`` which is a kind of conductor in Caliburn.Micro. As the name says, it will allow the use of a single user control or window.

``BindableCollection<CategoryModel> CategoryList``
CategoryList is a List containing all categories. It is used in the DatGrid we will define in the View.

``BindableCollection<T>`` is a class derived from ``ObservableCollection<T>`` but has some improvements in event processing at the UI thread. Therefore it is preferred to use a BindableCollection above ``ObservableCollection``.

There is one more thing to discuss here:

``private BindableCollection<CategoryModel> _categoryList = new BindableCollection<CategoryModel>();``

You see the ``CategoryList`` is instantiated here. This avoids null value exceptions. It is good to know you can do this while creating the property.

``CategoryModel SelectedCategory`` represents the selected item. In the View you must bind it. In non-MVVM solutions you might use an eventhandler to handle selecting an item from a list. Using Caliburn.Micro, we will not do it, but instead we obtain the value and use it in the ViewModel. This separates it nicely from the UI. For example, you now can create a test case where you pick a value, assign it to ``SelectedCategory`` and then use this to drive a test. No need to have a working UI.

You can edit the data in the DataGrid in the View directly, but here a different technique is used. The idea is that you select an item you want to edit (or create a new one) and then save the changes or discard them. You also can create new values. To support this, we need a copy for each field in the ``CategoryModel``.

```csharp
public string CategoryName
      {
      get => _categoryName; set
        {
        _categoryName = value;
        NotifyOfPropertyChange(() => CategoryName);
        }
      }
```

The example shows a full property is used. In the setter we use an additional line of code:

``NotifyOfPropertyChange(() => CategoryName);``

This is the Caliburn.Micro way of implementing change notifications. Its use is not restricted to using it in a setter. Any place where you need to tell the UI that a property is changed, just call this method.

Alternatively you can use ``NotifyOfPropertyChange("SelectedCategory");`` which may look more familiar and may be a way to help migrating an existing project to Caliburn.Micro. The advantage of the lambda expression is that the compiler will detect typos for you.

```csharp
protected override void OnViewLoaded(object view)
      {
      base.OnViewLoaded(view);
      CategoryList.Add(new CategoryModel {CategoryName="Meals",CategoryDescription="Lunched and diners"});
      CategoryList.Add(new CategoryModel { CategoryName = "Representation", CategoryDescription = "Gifts for our customers" });
      }
```

``OnViewLoaded`` is an event that will be fired once loading the view is completed. Here we use the synchronous version. There also is an asynchronous version. You will see that one shortly.

In many cases you can initialize the ViewModel in the constructor as well. This is not always working, therefore I prefer to wait till the View is created.

The last part of the code for the ViewModel is a bunch of methods, each representing the actions the ViewModel can execute. In this case Edit, Delete, Save and Clear.

For the moment, we are done with the ViewModel, but you may notice that this code is not yet failsafe. If the ``SelectedCategory`` is null the application will crash. It also will allow to store an empty ``CategoryName``. We will address these issues later.

[Contents](Contents) [Previous](ExpensesLogbook) [Next](CategoryEditorHookup)
