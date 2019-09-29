# Basic Syntax


image loaded from the web
![htttps://www.google.com](https://images.emojiterra.com/mozilla/512px/1f517.png)   

image loaded from img/logo.png![pic](../../../a_blog_imgs/logo.png)  
 
 image loaded from the web with a link(uses HTML)
[<img src="http://www.google.com.au/images/nav_logo7.png">](http://google.com/)  


image loaded from img/logo.png with a link(uses HTML) 
[<img src="../a_blog_imgs/logo.png">](http://google.com/)  


if you are not seeing thisimage , well , simply put: if your blog resides in following 
directory:
```
docs/
 |
 |----x/
 |	|
 |	|----y/
 |	|  |
 |	|  |
 |	|  |---z/
 |	|  |  |
 |	|  |  |
 |	|  |  |---myblog.md
 |	|  |  |
```
 then you should create a folder `img` in the `z/` folder and add pics there and access it using
 `.../img/imagename.png` . so your final structure will be:  
 
```
docs/
 |
 |----x/
 |	|
 |	|----y/
 |	|  |
 |	|  |
 |	|  |---z/
 |	|  |  |
 |	|  |  |
 |	|  |  |---img/
 |	|  |  |  |-----imagename.png
 |	|  |  |---myblog.md
 |	|  |  |
```

like this : ![../img/logo.png](../img/logoxxx.png)

## Body copy

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras arcu libero,
mollis sed massa vel, *ornare viverra ex*. Mauris a ullamcorper lacus. Nullam
urna elit, malesuada eget finibus ut, ullamcorper ac tortor. Vestibulum sodales
pulvinar nisl, pharetra aliquet est. Quisque volutpat erat ac nisi accumsan
tempor.

**Sed suscipit**, orci non pretium pretium, quam mi gravida metus, vel
venenatis justo est condimentum diam. Maecenas non ornare justo. Nam a ipsum
eros. [Nulla aliquam](#) orci sit amet nisl posuere malesuada. Proin aliquet
nulla velit, quis ultricies orci feugiat et. `Ut tincidunt sollicitudin`
tincidunt. Aenean ullamcorper sit amet nulla at interdum.

## Headings

### The 3rd level

#### The 4th level

##### The 5th level

###### The 6th level

## Headings <small>with secondary text</small>

### The 3rd level <small>with secondary text</small>

#### The 4th level <small>with secondary text</small>

##### The 5th level <small>with secondary text</small>

###### The 6th level <small>with secondary text</small>

## Blockquotes

> Morbi eget dapibus felis. Vivamus venenatis porttitor tortor sit amet rutrum.
  Pellentesque aliquet quam enim, eu volutpat urna rutrum a. Nam vehicula nunc
  mauris, a ultricies libero efficitur sed. *Class aptent* taciti sociosqu ad
  litora torquent per conubia nostra, per inceptos himenaeos. Sed molestie
  imperdiet consectetur.

### Blockquote nesting

> **Sed aliquet**, neque at rutrum mollis, neque nisi tincidunt nibh, vitae
  faucibus lacus nunc at lacus. Nunc scelerisque, quam id cursus sodales, lorem
  [libero fermentum](#) urna, ut efficitur elit ligula et nunc.

> > Mauris dictum mi lacus, sit amet pellentesque urna vehicula fringilla.
    Ut sit amet placerat ante. Proin sed elementum nulla. Nunc vitae sem odio.
    Suspendisse ac eros arcu. Vivamus orci erat, volutpat a tempor et, rutrum.
    eu odio.

> > > `Suspendisse rutrum facilisis risus`, eu posuere neque commodo a.
      Interdum et malesuada fames ac ante ipsum primis in faucibus. Sed nec leo
      bibendum, sodales mauris ut, tincidunt massa.

### Other content blocks

> Vestibulum vitae orci quis ante viverra ultricies ut eget turpis. Sed eu
  lectus dapibus, eleifend nulla varius, lobortis turpis. In ac hendrerit nisl,
  sit amet laoreet nibh.
  ``` js hl_lines="8"
  var _extends = function(target) {
    for (var i = 1; i < arguments.length; i++) {
      var source = arguments[i];
      for (var key in source) {
        target[key] = source[key];
      }
    }
    return target;
  };
  ```

  > > Praesent at `:::js return target`, sodales nibh vel, tempor felis. Fusce
      vel lacinia lacus. Suspendisse rhoncus nunc non nisi iaculis ultrices.
      Donec consectetur mauris non neque imperdiet, eget volutpat libero.

## Lists

### Unordered lists

* Sed sagittis eleifend rutrum. Donec vitae suscipit est. Nullam tempus tellus
  non sem sollicitudin, quis rutrum leo facilisis. Nulla tempor lobortis orci,
  at elementum urna sodales vitae. In in vehicula nulla, quis ornare libero.

    * Duis mollis est eget nibh volutpat, fermentum aliquet dui mollis.
    * Nam vulputate tincidunt fringilla.
    * Nullam dignissim ultrices urna non auctor.

* Aliquam metus eros, pretium sed nulla venenatis, faucibus auctor ex. Proin ut
  eros sed sapien ullamcorper consequat. Nunc ligula ante, fringilla at aliquam
  ac, aliquet sed mauris.

* Nulla et rhoncus turpis. Mauris ultricies elementum leo. Duis efficitur
  accumsan nibh eu mattis. Vivamus tempus velit eros, porttitor placerat nibh
  lacinia sed. Aenean in finibus diam.

### Ordered lists

1. Integer vehicula feugiat magna, a mollis tellus. Nam mollis ex ante, quis
  elementum eros tempor rutrum. Aenean efficitur lobortis lacinia. Nulla
  consectetur feugiat sodales.

2. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur
  ridiculus mus. Aliquam ornare feugiat quam et egestas. Nunc id erat et quam
  pellentesque lacinia eu vel odio.

    1. Vivamus venenatis porttitor tortor sit amet rutrum. Pellentesque aliquet
      quam enim, eu volutpat urna rutrum a. Nam vehicula nunc mauris, a
      ultricies libero efficitur sed.

        1. Mauris dictum mi lacus
        2. Ut sit amet placerat ante
        3. Suspendisse ac eros arcu

    2. Morbi eget dapibus felis. Vivamus venenatis porttitor tortor sit amet
      rutrum. Pellentesque aliquet quam enim, eu volutpat urna rutrum a. Sed
      aliquet, neque at rutrum mollis, neque nisi tincidunt nibh.

    3. Pellentesque eget `:::js var _extends` ornare tellus, ut gravida mi.
    ``` js hl_lines="1"
    var _extends = function(target) {
      for (var i = 1; i < arguments.length; i++) {
        var source = arguments[i];
        for (var key in source) {
          target[key] = source[key];
        }
      }
      return target;
    };
    ```

3. Vivamus id mi enim. Integer id turpis sapien. Ut condimentum lobortis
  sagittis. Aliquam purus tellus, faucibus eget urna at, iaculis venenatis
  nulla. Vivamus a pharetra leo.

### Definition lists

Lorem ipsum dolor sit amet

:   Sed sagittis eleifend rutrum. Donec vitae suscipit est. Nullam tempus
    tellus non sem sollicitudin, quis rutrum leo facilisis. Nulla tempor
    lobortis orci, at elementum urna sodales vitae. In in vehicula nulla.

    Duis mollis est eget nibh volutpat, fermentum aliquet dui mollis.
    Nam vulputate tincidunt fringilla.
    Nullam dignissim ultrices urna non auctor.

Cras arcu libero

:   Aliquam metus eros, pretium sed nulla venenatis, faucibus auctor ex. Proin
    ut eros sed sapien ullamcorper consequat. Nunc ligula ante, fringilla at
    aliquam ac, aliquet sed mauris.

## Code blocks

### Inline

Morbi eget `dapibus felis`. Vivamus *`venenatis porttitor`* tortor sit amet
rutrum. Class aptent taciti sociosqu ad litora torquent per conubia nostra,
per inceptos himenaeos. [`Pellentesque aliquet quam enim`](#), eu volutpat urna
rutrum a.

Nam vehicula nunc `:::js return target` mauris, a ultricies libero efficitur
sed. Sed molestie imperdiet consectetur. Vivamus a pharetra leo. Pellentesque
eget ornare tellus, ut gravida mi. Fusce vel lacinia lacus.

### Listing

    #!js hl_lines="8"
    var _extends = function(target) {
      for (var i = 1; i < arguments.length; i++) {
        var source = arguments[i];
        for (var key in source) {
          target[key] = source[key];
        }
      }
      return target;
    };

## Horizontal rules

Aenean in finibus diam. Duis mollis est eget nibh volutpat, fermentum aliquet
dui mollis. Nam vulputate tincidunt fringilla. Nullam dignissim ultrices urna
non auctor.

***

Integer vehicula feugiat magna, a mollis tellus. Nam mollis ex ante, quis
elementum eros tempor rutrum. Aenean efficitur lobortis lacinia. Nulla
consectetur feugiat sodales.

## Data tables

| Sollicitudo / Pellentesi | consectetur | adipiscing | elit    | arcu | sed |
| ------------------------ | ----------- | ---------- | ------- | ---- | --- |
| Vivamus a pharetra       | yes         | yes        | yes     | yes  | yes |
| Ornare viverra ex        | yes         | yes        | yes     | yes  | yes |
| Mauris a ullamcorper     | yes         | yes        | partial | yes  | yes |
| Nullam urna elit         | yes         | yes        | yes     | yes  | yes |
| Malesuada eget finibus   | yes         | yes        | yes     | yes  | yes |
| Ullamcorper              | yes         | yes        | yes     | yes  | yes |
| Vestibulum sodales       | yes         | -          | yes     | -    | yes |
| Pulvinar nisl            | yes         | yes        | yes     | -    | -   |
| Pharetra aliquet est     | yes         | yes        | yes     | yes  | yes |
| Sed suscipit             | yes         | yes        | yes     | yes  | yes |
| Orci non pretium         | yes         | partial    | -       | -    | -   |

Sed sagittis eleifend rutrum. Donec vitae suscipit est. Nullam tempus tellus
non sem sollicitudin, quis rutrum leo facilisis. Nulla tempor lobortis orci,
at elementum urna sodales vitae. In in vehicula nulla, quis ornare libero.

| Left       | Center   | Right   |
| :--------- | :------: | ------: |
| Lorem      | *dolor*  | `amet`  |
| [ipsum](#) | **sit**  |         |

Vestibulum vitae orci quis ante viverra ultricies ut eget turpis. Sed eu
lectus dapibus, eleifend nulla varius, lobortis turpis. In ac hendrerit nisl,
sit amet laoreet nibh.

<table>
  <colgroup>
    <col width="30%">
    <col width="70%">
  </colgroup>
  <thead>
    <tr class="header">
      <th>Table</th>
      <th>with colgroups (Pandoc)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Lorem</td>
      <td>ipsum dolor sit amet.</td>
    </tr>
    <tr>
      <td>Sed sagittis</td>
      <td>eleifend rutrum. Donec vitae suscipit est.</td>
    </tr>
  </tbody>
</table>


# Admonition

[Admonition][1] is an extension included in the standard Markdown library that
makes it possible to add block-styled side content to your documentation, for
example summaries, notes, hints or warnings.

  [1]: https://python-markdown.github.io/extensions/admonition/

## Installation

Add the following lines to your `mkdocs.yml`:

``` yaml
markdown_extensions:
  - admonition
```

## Usage

Admonition blocks follow a simple syntax: every block is started with `!!!`,
followed by a single keyword which is used as the [type qualifier][2] of the
block. The content of the block then follows on the next line, indented by
four spaces.

Example:

``` markdown
!!! note
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! note

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

  [2]: #types

### Changing the title

By default, the block title will equal the type qualifier in titlecase. However,
it can easily be changed by adding a quoted string after the type qualifier.

Example:

``` markdown
!!! note "Phasellus posuere in sem ut cursus"
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! note "Phasellus posuere in sem ut cursus"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

### Removing the title

Similar to setting a [custom title][3], the icon and title can be omitted by
providing an empty string after the type qualifier:

Example:

``` markdown
!!! note ""
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! note ""

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

  [3]: #changing-the-title

### Embedded code blocks

Blocks can contain all kinds of text content, including headlines, lists,
paragraphs and other blocks – except code blocks, because the parser from the
standard Markdown library does not account for those.

However, the [PyMdown Extensions][4] package adds an extension called
[SuperFences][5], which makes it possible to nest code blocks within other
blocks, respectively Admonition blocks.

  [4]: https://facelessuser.github.io/pymdown-extensions
  [5]: https://facelessuser.github.io/pymdown-extensions/extensions/superfences/

Example:

!!! note

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

    ``` mysql
    SELECT
      Employees.EmployeeID,
      Employees.Name,
      Employees.Salary,
      Manager.Name AS Manager
    FROM
      Employees
    LEFT JOIN
      Employees AS Manager
    ON
      Employees.ManagerID = Manager.EmployeeID
    WHERE
      Employees.EmployeeID = '087652';
    ```

    Nunc eu odio eleifend, blandit leo a, volutpat sapien. Phasellus posuere in
    sem ut cursus. Nullam sit amet tincidunt ipsum, sit amet elementum turpis.
    Etiam ipsum quam, mattis in purus vitae, lacinia fermentum enim.

### Collapsible blocks

The [Details][6] extension which is also part of the [PyMdown Extensions][4]
package adds support for rendering collapsible Admonition blocks. This is
useful for FAQs or content that is of secondary nature.

Example:

``` markdown
??? note "Phasellus posuere in sem ut cursus"
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

??? note "Phasellus posuere in sem ut cursus"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

By adding a `+` sign directly after the start marker, blocks can be rendered
open by default.

  [6]: https://facelessuser.github.io/pymdown-extensions/extensions/details/

## Types

Admonition supports user-defined type qualifiers which may influence the style
of the inserted block. Following is a list of type qualifiers provided by the
Material theme, whereas the default type, and thus fallback for unknown type
qualifiers, is `note`.

### Note

Example:

``` markdown
!!! note
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! note

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `note`
* `seealso`

### Abstract

Example:

``` markdown
!!! abstract
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! abstract

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `abstract`
* `summary`
* `tldr`

### Info

Example:

``` markdown
!!! info
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! info

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `info`
* `todo`

### Tip

Example:

``` markdown
!!! tip
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! tip

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `tip`
* `hint`
* `important`

### Success

Example:

``` markdown
!!! success
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! success

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `success`
* `check`
* `done`

### Question

Example:

``` markdown
!!! question
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! question

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `question`
* `help`
* `faq`

### Warning

Example:

``` markdown
!!! warning
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! warning

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `warning`
* `caution`
* `attention`

### Failure

Example:

``` markdown
!!! failure
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! failure

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `failure`
* `fail`
* `missing`

### Danger

Example:

``` markdown
!!! danger
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! danger

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `danger`
* `error`

### Bug

Example:

``` markdown
!!! bug
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! bug

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `bug`

### Example

Example:

``` markdown
!!! example
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! example

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `example`
* `snippet`

### Quote

Example:

``` markdown
!!! quote
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
```

Result:

!!! quote

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Qualifiers:

* `quote`
* `cite`
