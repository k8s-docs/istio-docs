tabs and lists
================

{{< tabset category-name=“test” >}}

{{< tab name=“One” category-value=“one” >}} 1. One paragraph in a list
in a tab {{< /tab >}}

{{< tab name=“Two” category-value=“two” >}} 1. Three

1. separate

2. bullets in a list in a tab

   This last bullet with two paragraphs {{< /tab >}}

{{< tab name=“Three” category-value=“three” >}} 1. Simple text in a list
in a tab

::

   A paragraph

   .. warning::


   Warning in a list in a tab


   And another

1. Second bullet

2. Third bullet {{< /tab >}}

{{< tab name=“Four” category-value=“four” >}} 1. Simple text with
*markdown* in a list in a tab

::

   .. warning::


   Warning in a list in a tab


{{< /tab >}}

{{< tab name=“Five” category-value=“five” >}} 1. Simple text in a list
in a tab

::

   {{< text plain >}}
   Text block in a list in a tab


{{< /tab >}}

{{< tab name=“Six” category-value=“six” >}} 1. Simple text with
*markdown* in a list in a tab

::

   .. warning::


   Warning with _markdown_ in a list in a tab


1. Second bullet {{< /tab >}}

{{< tab name=“Seven” category-value=“seven” >}} 1. Simple text with
*markdown* in a list in a tab

::

   .. code:: sh


   $ NoIndent:
       FourIndent:
           - EightIndent
       FourIndentAgain:
           - EightIndentAgain


1. Second bullet {{< /tab >}}

{{< /tabset >}}
