.. _containers:

Containers 容器
==========

容器是可以放置元素 (文本、列表、表、等等)。
有3个主要集装箱，即节、页眉和页脚。
有3个元素也可以充当容器，即textruns，表格单元格和脚注。

Sections 节
--------

word中的每个可见元素都放置在一个部分内。创建一节，使用以下代码:

.. code-block:: php

    $section = $phpWord->addSection($sectionStyle);

``$sectionStyle``是一个可选的关联数组，它设置在节上。示例:

.. code-block:: php

    $sectionStyle = array(
        'orientation' => 'landscape',
        'marginTop' => 600,
        'colsNum' => 2,
    );

Page number 页码
~~~~~~~~~~~

您可以使用  ``pageNumberingStart``  更改节页码
部分的样式。

.. code-block:: php

    // Method 1
    $section = $phpWord->addSection(array('pageNumberingStart' => 1));

    // Method 2
    $section = $phpWord->addSection();
    $section->getStyle()->setPageNumberingStart(1);

Multicolumn 多列
~~~~~~~~~~~

您可以通过以下方式将节布局更改为多列 (如在报纸中)
使用节的 ``breaktype`` 和 ``colsnum`` 样式。

.. code-block:: php

    // Method 1
    $section = $phpWord->addSection(array('breakType' => 'continuous', 'colsNum' => 2));

    // Method 2
    $section = $phpWord->addSection();
    $section->getStyle()->setBreakType('continuous');
    $section->getStyle()->setColsNum(2);

Line numbering 行号
~~~~~~~~~~~~~~


您可以使用 ``lineNumbering``的样式 将行号应用于节。

.. code-block:: php

    // Method 1
    $section = $phpWord->addSection(array('lineNumbering' => array()));

    // Method 2
    $section = $phpWord->addSection();
    $section->getStyle()->setLineNumbering(array());

以下是行号样式的属性。

-  ``start`` Line numbering starting value
-  ``increment`` Line number increments
-  ``distance`` Distance between text and line numbering in *twip*
-  ``restart`` Line numbering restart setting
   continuous\|newPage\|newSection

Headers 页眉
-------

每个部分都可以有自己的标题引用。要创建标题，请使用
``Addheader`` 方法:

.. code-block:: php

    $header = $section->addHeader();

确保将结果保存在本地对象中。您可以使用所有元素
可用于页脚。有关详细信息，请参见 ``页脚`` 部分。
此外，只有在标题引用内部，您才能添加水印
或者背景图片。参见 ``水印`` 部分。

您可以传递一个可选参数来指定页眉/页脚的应用位置，它可以是

-  ``Footer::AUTO`` default, all pages except if overridden by first or even
-  ``Footer::FIRST`` each first page of the section
-  ``Footer::EVEN`` each even page of the section. Will only be applied if the evenAndOddHeaders is set to true in phpWord->settings

要更改evenandoddheader，请使用 ``getSettings`` 方法返回Settings对象，然后调用 ``setEvenAndOddHeaders`` 方法:

.. code-block:: php

    $phpWord->getSettings()->setEvenAndOddHeaders(true);

Footers 页脚
-------

每个部分都可以有自己的页脚参考。要创建页脚，请使用``addFooter`` 方法:

.. code-block:: php

    $footer = $section->addFooter();

确保将结果保存在本地对象中以将元素添加到
页脚。您可以将以下元素添加到页脚:

-  Texts ``addText`` and ``createTextrun``
-  Text breaks
-  Images
-  Tables
-  Preserve text

有关每个元素的详细信息，请参见 ``Elements`` 部分。

Other containers 其他容器
----------------

Textruns、表格单元格和脚注是也可以充当
集装箱。有关以下内容的详细信息，请参阅相应的 “元素” 部分
每个元素。
