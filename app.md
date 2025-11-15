# Role
You are a professional React developer with 15 years of experience.

# Task
I want you to build an application consisting of one page

## Articles Page
This page consists of:
- Header - **must** be on the top of the page. On the left part of the header should be a logo, on the right part of the header should be a **button** to log out.
- Sidebar - **must** be located on the left side of the page. It consists of three controls:
  1. *Category Filter* is a **tree** with selectable items (**without** checkboxes). Each item, including tree group items, must be selectable. When any tree item is selected, the URL param called *category* must be updated with an ID of the tree item.
  2. *Countries Filter* is a filterable **list** of items. This **list** must be filterable by text which is typed into the input above the list. Each list item must be selectable, and selection must be done by the checkbox selection. When list item checked or unchecked, the URL param called **countries** (which is an array of strings) must be updated with selected or unselected *Countries Filter Item* ID.
  3. *Impact filter* is a *select* component - Negative, Neutral, and Positive. I want it to look like three rectangles:
    1. The negative option. with a red border and "Negative" text inside.
    2. For neutral option with a grey border and "Neutral" text inside.
    3. For positive option with a blue border and "Positive" text inside.
    When the *Impact Filter* item is selected, I want the URL param called **impact**, which is a string, to be updated with the selected option.
- Article's table - **must** be located on the right side of the page, under the header and to the right of the sidebar. It should contain a table element. This table must have the following columns:
  - ID
  - Title
  - Author
  - Date
  Data for articles table **must** be loaded on apge init, and every time when any **filter** in the sidebar is updated.


