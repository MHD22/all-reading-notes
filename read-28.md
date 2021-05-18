##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 28 - RecyclerView

<hr>

## Create dynamic lists with RecyclerView:

RecyclerView makes it easy to efficiently display large sets of data. You supply the data and define how each item looks, and the RecyclerView library dynamically creates the elements when they're needed.

As the name implies, RecyclerView recycles those individual elements. When an item scrolls off the screen, RecyclerView doesn't destroy its view. Instead, RecyclerView reuses the view for new items that have scrolled onscreen. This reuse vastly improves performance, improving your app's responsiveness and reducing power consumption.

## Key classes

* Several different classes work together to build your dynamic list.

`RecyclerView` is the ViewGroup that contains the views corresponding to your data. It's a view itself, so you add `RecyclerView` into your layout the way you would add any other UI element.

Each individual element in the list is defined by a view holder object. When the view holder is created, it doesn't have any data associated with it. After the view holder is created, the `RecyclerView` binds it to its data. You define the view holder by extending `RecyclerView.ViewHolder`.

The RecyclerView requests those views, and binds the views to their data, by calling methods in the adapter. You define the adapter by extending `RecyclerView.Adapter.`

The layout manager arranges the individual elements in your list. You can use one of the layout managers provided by the RecyclerView library, or you can define your own. Layout managers are all based on the library's `LayoutManager` abstract class.


### Steps for implementing your RecyclerView

If you're going to use RecyclerView, there are a few things you need to do. 

* First of all, decide what the list or grid is going to look like. Ordinarily you'll be able to use one of the RecyclerView library's standard layout managers.

* Design how each element in the list is going to look and behave. Based on this design, extend the ViewHolder class. Your version of ViewHolder provides all the functionality for your list items. Your view holder is a wrapper around a View, and that view is managed by RecyclerView.

* Define the Adapter that associates your data with the ViewHolder views.

* ***Plan your layout:***
  * The RecyclerView library provides three layout managers, which handle the most common layout situations:
    * `LinearLayoutManager`
    * `GridLayoutManager`
    * `StaggeredGridLayoutManager` 

* ***Implementing your adapter and view holder:***
  * When you define your adapter, you need to override three key methods:
    * `onCreateViewHolder()`
    * `onBindViewHolder()`
    * `getItemCount()`


<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://developer.android.com/guide/topics/ui/layout/recyclerview#java)
<!-- ###### [resource2](https://developer.android.com/training/data-storage/shared-preferences) -->