---
layout: page
title: Expenses Logbook introduction
---

[Contents](Contents) [Previous](CategoryView) [Next](Menu)

## Conditional Execution

### Introduction

In WPF applications you can disable a button using ``IsEnabled="false"`` in the XAML code. You can bind this to a property in the code behind, or define the conditions in the code behind. In Caliburn.Micro the ViewModel can determine conditions that must be met in order to execute a command. For example, The ``Edit()`` command is tied to the Button named ``Edit`` in the view. It would e nice if we can disable it if the ``SelectedCategory`` is ``null``.

### The Caliburn.Micro solution

In Caliburn.Micro this can be solved using a method or a property, which must obey the naming convention, where the PropertyName starts with **Can**, followed by the command name. For the ``Edit()`` command this may look like:

```csharp
   public bool CanEdit 
     { 
     get
       {
       return SelectedCategory!=null;
       }
     }
```

If you include this code in the ``CategoryViewModel`` you will see that  the Edit button is disabled.

If you change ``SelectedCategory`` in the UI, this should evaluate ``CanEdit`` again. Adding a notification to the ``SelectedCategoryProperty`` will do the job:

```csharp
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
    NotifyOfPropertyChange(()=>CanEdit);
    }
  }
```

The advantage of this way of working is that you can enforce this from the ViewModel. The disadvantage may become clear when create similar enabling properties for other situations, e.g. the Delete command. There is a lot of duplicate code, just because you need different names.

### Completing the conditions

Using the same principle, execution conditions are added for the Delete and Save button. The ``CanSave`` property checks if you actually created a ``CategoryName`` to avoid the possibility to save empty Categories. Note you need to name sure ``CategoryName`` is not null as well.

The code for the complete ``CategoryViewModel`` look like this, with the last updates added:

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
        NotifyOfPropertyChange(() => CanEdit);
        NotifyOfPropertyChange(() => CanDelete);
        }
      }

    public string CategoryName
      {
      get => _categoryName; set
        {
        _categoryName = value;
        NotifyOfPropertyChange(() => CategoryName);
        NotifyOfPropertyChange(() => CanSave);
        }
      }

    public string CategoryDescription
      {
      get => _categoryDescription; set
        {
        _categoryDescription = value;
        NotifyOfPropertyChange(() => CategoryDescription);
        }
      }

    protected override void OnViewLoaded(object view)
      {
      base.OnViewLoaded(view);
      CategoryList.Add(new CategoryModel { CategoryName = "Meals", CategoryDescription = "Lunched and diners" });
      CategoryList.Add(new CategoryModel { CategoryName = "Representation", CategoryDescription = "Gifts for our customers" });
      }


    public bool CanEdit
      {
      get
        {
        return SelectedCategory != null;
        }
      }

    public void Edit()
      {
      CategoryName = SelectedCategory.CategoryName;
      CategoryDescription = SelectedCategory.CategoryDescription;
      }

    public bool CanDelete
      {
      get
        {
        return SelectedCategory != null;
        }
      }

    public void Delete()
      {
      CategoryList.Remove(SelectedCategory);
      Clear();
      }

    public bool CanSave
      {
      get
        {
        return CategoryName?.Length > 2;
        }
      }

    public void Save()
      {
      CategoryModel newCategory = new CategoryModel();
      newCategory.CategoryName = CategoryName;
      newCategory.CategoryDescription = CategoryDescription;
      if (SelectedCategory != null)
        {
        // remove the existing category, needed to update the view
        CategoryList.Remove(SelectedCategory);
        }
      CategoryList.Add(newCategory);
      Clear();
      }

    public void Clear()
      {
      CategoryName = string.Empty;
      CategoryDescription = string.Empty;
      SelectedCategory = null;
      }
    }
  }
```

This concludes the functions to setup Categories. You should be able to compile and run the app now.

[Contents](Contents) [Previous](CategoryView) [Next](Menu)
