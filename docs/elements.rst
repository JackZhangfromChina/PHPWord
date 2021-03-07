.. _elements:

Elements 元素
========

以下是每个容器中的元素可用性矩阵。 
列显示容器，而行列出元素。

+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| Num   | Element         | Section   | Header   | Footer   | Cell    | Text Run   | Footnote   |
+=======+=================+===========+==========+==========+=========+============+============+
| 1     | Text            | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 2     | Text Run        | v         | v        | v        | v       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 3     | Link            | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 4     | Title           | v         | ?        | ?        | ?       | ?          | ?          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 5     | Preserve Text   | ?         | v        | v        | v\*     | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 6     | Text Break      | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 7     | Page Break      | v         | -        | -        | -       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 8     | List            | v         | v        | v        | v       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 9     | Table           | v         | v        | v        | v       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 10    | Image           | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 11    | Watermark       | -         | v        | -        | -       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 12    | OLEObject       | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 13    | TOC             | v         | -        | -        | -       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 14    | Footnote        | v         | -        | -        | v\*\*   | v\*\*      | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 15    | Endnote         | v         | -        | -        | v\*\*   | v\*\*      | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 16    | CheckBox        | v         | v        | v        | v       | v          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 17    | TextBox         | v         | v        | v        | v       | -          | -          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 18    | Field           | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 19    | Line            | v         | v        | v        | v       | v          | v          |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+
| 20    | Chart           | v         |          |          | v       |            |            |
+-------+-----------------+-----------+----------+----------+---------+------------+------------+

Legend:

- ``v``. 可用.
- ``v*``. 只在 header/footer 中可用.
- ``v**``. 只在节中可用。
- ``-``. 不可用.
- ``?``. 应该可用.

Texts 文本
-----

文本可以使用 ``addText`` 和 ``addTextRun`` 被添加。
``addText`` 用于创建仅包含相同样式的文本的简单段落。
``addTextRun`` 用于创建包含不同样式的文本的复杂段落 (一些粗体，其他
斜体等) 或其他元素，例如图像或链接。语法如下:

.. code-block:: php

    $section->addText($text, [$fontStyle], [$paragraphStyle]);
    $textrun = $section->addTextRun([$paragraphStyle]);

- ``$text``. Text to be displayed in the document.
- ``$fontStyle``. See :ref:`font-style`.
- ``$paragraphStyle``. See :ref:`paragraph-style`.

有关可用样式选项，请参阅:ref:`font-style` and :ref:`paragraph-style`.

如果要对添加的文本启用跟踪更改，可以为特定用户在给定时间将其标记 INSERTED 或 DELETED :

.. code-block:: php

    $text = $section->addText('Hello World!');
    $text->setChanged(\PhpOffice\PhpWord\Element\ChangedElement::TYPE_INSERTED, 'Fred', (new \DateTime()));

Titles 标题
~~~~~~

如果要构建文档或构建目录，则需要标题或标题。
要向文档添加标题，请使用 ``addtitlestyle``和`` addtitle``方法。
如果 `depth` 为0，将插入标题，否则插入标题1，标题2，...

.. code-block:: php

    $phpWord->addTitleStyle($depth, [$fontStyle], [$paragraphStyle]);
    $section->addTitle($text, [$depth]);

- ``depth``.
- ``$fontStyle``. 参见 :ref:`font-style`.
- ``$paragraphStyle``. 参见 :ref:`paragraph-style`.
- ``$text``. 文档中要显示的文本。 可以使`string` 或一个 `\PhpOffice\PhpWord\Element\TextRun`

有必要在文档中添加标题样式，否则标题将不会被检测为真实标题。

Links 链接
~~~~~

您可以使用函数addLink向文档添加超链接:

.. code-block:: php

    $section->addLink($linkSrc, [$linkName], [$fontStyle], [$paragraphStyle]);

- ``$linkSrc``. The URL of the link.
- ``$linkName``. Placeholder of the URL that appears in the document.
- ``$fontStyle``. See :ref:`font-style`.
- ``$paragraphStyle``. See :ref:`paragraph-style`.

Preserve texts 
~~~~~~~~~~~~~~

The ``addPreserveText`` method is used to add a page number or page count to headers or footers.

.. code-block:: php

    $footer->addPreserveText('Page {PAGE} of {NUMPAGES}.');

Breaks 分隔
------

Text breaks 文本分隔
~~~~~~~~~~~

文本分隔是空的新行。要添加文本分隔，请使用以下语法。所有参数都是可选的。

.. code-block:: php

    $section->addTextBreak([$breakCount], [$fontStyle], [$paragraphStyle]);

- ``$breakCount``. How many lines.
- ``$fontStyle``. See :ref:`font-style`.
- ``$paragraphStyle``. See :ref:`paragraph-style`.

Page breaks 
~~~~~~~~~~~

有两种方法可以使用 ``addPageBreak`` 插入分页符或使用段落的 ``pageBreakBefore`` 样式。

.. code-block:: php

    $section->addPageBreak();

Lists 列表
-----

可以使用 ``addListItem`` 和`` addListItemRun`` 方法添加列表。
``AddListItem`` 用于创建仅包含纯文本的列表。
``AddListItemRun`` 用于创建包含文本的复杂列表项
具有不同的风格 (一些粗体、其他斜体等) 或其他元素，例如
图像或链接。语法如下:

基本用法:

.. code-block:: php

    $section->addListItem($text, [$depth], [$fontStyle], [$listStyle], [$paragraphStyle]);
    $listItemRun = $section->addListItemRun([$depth], [$listStyle], [$paragraphStyle])

Parameters:

- ``$text``. Text that appears in the document.
- ``$depth``. Depth of list item.
- ``$fontStyle``. See :ref:`font-style`.
- ``$listStyle``. List style of the current element TYPE\_NUMBER,
  TYPE\_ALPHANUM, TYPE\_BULLET\_FILLED, etc. See list of constants in PHPWord\\Style\\ListItem.
- ``$paragraphStyle``. See :ref:`paragraph-style`.

参考 ``Sample_09_Tables.php`` 获得更多示例.

高级用法:

您还可以通过使用编号样式的名称更改 ``$listStyle`` 参数来创建自己的编号样式。

.. code-block:: php

    $phpWord->addNumberingStyle(
        'multilevel',
        array(
            'type' => 'multilevel',
            'levels' => array(
                array('format' => 'decimal', 'text' => '%1.', 'left' => 360, 'hanging' => 360, 'tabPos' => 360),
                array('format' => 'upperLetter', 'text' => '%2.', 'left' => 720, 'hanging' => 360, 'tabPos' => 720),
            )
        )
    );
    $section->addListItem('List Item I', 0, null, 'multilevel');
    $section->addListItem('List Item I.a', 1, null, 'multilevel');
    $section->addListItem('List Item I.b', 1, null, 'multilevel');
    $section->addListItem('List Item II', 0, null, 'multilevel');

可用样式参考 :ref:`numbering-level-style`.

Tables 表格
------

要添加表格，行和单元格 使用 ``addTable``, ``addRow``, and ``addCell`` 方法:

.. code-block:: php

    $table = $section->addTable([$tableStyle]);
    $table->addRow([$height], [$rowStyle]);
    $cell = $table->addCell($width, [$cellStyle]);

可以通过 ``addTableStyle`` 定义表格样式:

.. code-block:: php

    $tableStyle = array(
        'borderColor' => '006699',
        'borderSize'  => 6,
        'cellMargin'  => 50
    );
    $firstRowStyle = array('bgColor' => '66BBFF');
    $phpWord->addTableStyle('myTable', $tableStyle, $firstRowStyle);
    $table = $section->addTable('myTable');

可用样式参考 :ref:`table-style`.

Cell span 
~~~~~~~~~

您可以使用 ``gridSpan`` 跨多列的单元格，也可以使用 ``vMerge`` 跨多行。

.. code-block:: php

    $cell = $table->addCell(200);
    $cell->getStyle()->setGridSpan(5);

See ``Sample_09_Tables.php`` for more code sample.

Images 图片
------

要添加图像，请对节、页眉、页脚、textrun或表格单元格使用 ``addImage``方法。

.. code-block:: php

    $section->addImage($src, [$style]);

- ``$src``. 本地图像的字符串路径、远程图像的URL或图像数据 (作为字符串)。警告: 不要在此传递用户生成的字符串，因为这将允许攻击者通过传递文件路径或url而不是图像数据来读取任意文件或执行服务器端请求伪造。
- ``$style``. See :ref:`image-style`.

示例:

.. code-block:: php

    $section = $phpWord->addSection();
    $section->addImage(
        'mars.jpg',
        array(
            'width'         => 100,
            'height'        => 100,
            'marginTop'     => -1,
            'marginLeft'    => -1,
            'wrappingStyle' => 'behind'
        )
    );
    $footer = $section->addFooter();
    $footer->addImage('http://example.com/image.php');
    $textrun = $section->addTextRun();
    $textrun->addImage('http://php.net/logo.jpg');
    $source = file_get_contents('/path/to/my/images/earth.jpg');
    $textrun->addImage($source);

Watermarks 水印
~~~~~~~~~~


要添加水印 (或页面背景图像)，您的节需要
标题参考。创建标题后，您可以使用
添加水印的 ``addWatermark`` 方法。

.. code-block:: php

    $section = $phpWord->addSection();
    $header = $section->addHeader();
    $header->addWatermark('resources/_earth.jpg', array('marginTop' => 200, 'marginLeft' => 55));

Objects 对象
-------

您可以添加OLE嵌入，例如Excel电子表格或PowerPoint
使用 ``addOLEObject`` 方法对文档进行演示。

.. code-block:: php

    $section->addOLEObject($src, [$style]);

Table of contents 目录
-----------------

要添加目录 (TOC)，可以使用 ``addTOC`` 方法。
只有添加了至少一个标题 (请参阅 "Titles")，才能生成TOC。

.. code-block:: php

    $section->addTOC([$fontStyle], [$tocStyle], [$minDepth], [$maxDepth]);

- ``$fontStyle``. See font style section.
- ``$tocStyle``. See available options below.
- ``$minDepth``. Minimum depth of header to be shown. Default 1.
- ``$maxDepth``. Maximum depth of header to be shown. Default 9.

Options for ``$tocStyle``:

- ``tabLeader``. Fill type between the title text and the page number. Use the defined constants in ``\PhpOffice\PhpWord\Style\TOC``.
- ``tabPos``. The position of the tab where the page number appears in *twip*.
- ``indent``. The indent factor of the titles in *twip*.

Footnotes & endnotes 脚注和尾注
--------------------

您可以使用 ``addFootnote`` 创建脚注，并使用
文本或textruns中的 ``addEndnote``，但建议使用textrun
获得更好的布局。您可以脚注和尾注上 使用 ``addText`` 、 ``addLink``，
``addTextBreak``、  ``addImage``、 ``addOLEObject``。

在 textrun:

.. code-block:: php

    $textrun = $section->addTextRun();
    $textrun->addText('Lead text.');
    $footnote = $textrun->addFootnote();
    $footnote->addText('Footnote text can have ');
    $footnote->addLink('http://test.com', 'links');
    $footnote->addText('.');
    $footnote->addTextBreak();
    $footnote->addText('And text break.');
    $textrun->addText('Trailing text.');
    $endnote = $textrun->addEndnote();
    $endnote->addText('Endnote put at the end');

在 text:

.. code-block:: php

    $section->addText('Lead text.');
    $footnote = $section->addFootnote();
    $footnote->addText('Footnote text.');


默认情况下，脚注参考编号将显示为十进制编号。
从1开始。该数字使用``FooterReference`` 样式，您可以
使用 ``addFontStyle`` 方法重新定义。此样式的默认值为
``array('superScript' => true)``;

脚注编号可以通过在该节设置脚注属性来控制。

.. code-block:: php

    $fp = new PhpWord\SimpleType\FootnoteProperties();
    //sets the position of the footnote (pageBottom (default), beneathText, sectEnd, docEnd)
    $fp->setPos(FootnoteProperties::POSITION_DOC_END);
    //set the number format to use (decimal (default), upperRoman, upperLetter, ...)
    $fp->setNumFmt(FootnoteProperties::NUMBER_FORMAT_LOWER_ROMAN);
    //force starting at other than 1
    $fp->setNumStart(2);
    //when to restart counting (continuous (default), eachSect, eachPage)
    $fp->setNumRestart(FootnoteProperties::RESTART_NUMBER_EACH_PAGE);
    //And finaly, set it on the Section
    $section->setFootnoteProperties($properties);

Checkboxes 复选框
----------

可以通过 ``addCheckBox`` 将复选框元素添加到节或表格单元格中。

.. code-block:: php

    $section->addCheckBox($name, $text, [$fontStyle], [$paragraphStyle])

- ``$name``. Name of the check box.
- ``$text``. Text to be displayed in the document.
- ``$fontStyle``. See :ref:`font-style`.
- ``$paragraphStyle``. See :ref:`paragraph-style`.

Textboxes 文本框
---------

待完成

Fields 域
------

目前支持以下字段:

- PAGE
- NUMPAGES
- DATE
- XE
- INDEX

.. code-block:: php

    $section->addField($fieldType, [$properties], [$options], [$fieldText])

参考 ``\PhpOffice\PhpWord\Element\Field`` for list of properties and options available for each field type.
Options which are not specifically defined can be added. Those must start with a ``\``.

For instance for the INDEX field, you can do the following (See `Index Field for list of available options <https://support.office.com/en-us/article/Field-codes-Index-field-adafcf4a-cb30-43f6-85c7-743da1635d9e?ui=en-US&rs=en-US&ad=US>`_ ):

.. code-block:: php

    //the $fieldText can be either a simple string
    $fieldText = 'The index value';

    //or a 'TextRun', to be able to format the text you want in the index
    $fieldText = new TextRun();
    $fieldText->addText('My ');
    $fieldText->addText('bold index', ['bold' => true]);
    $fieldText->addText(' entry');
    $section->addField('XE', array(), array(), $fieldText);

    //this actually adds the index
    $section->addField('INDEX', array(), array('\\e "	" \\h "A" \\c "3"'), 'right click to update index');

Line 行
----

可以使用 ``addLine``将行元素添加到节中。

.. code-block:: php

    $lineStyle = array('weight' => 1, 'width' => 100, 'height' => 0, 'color' => 635552);
    $section->addLine($lineStyle);

可用的行样式属性:

- ``weight``. Line width in *twip*.
- ``color``. Defines the color of stroke.
- ``dash``. Line types: dash, rounddot, squaredot, dashdot, longdash, longdashdot, longdashdotdot.
- ``beginArrow``. Start type of arrow: block, open, classic, diamond, oval.
- ``endArrow``. End type of arrow: block, open, classic, diamond, oval.
- ``width``. Line-object width in *pt*.
- ``height``. Line-object height in *pt*.
- ``flip``. Flip the line element: true, false.

Chart 图表
-----

可以通过下方代码添加图表

.. code-block:: php

    $categories = array('A', 'B', 'C', 'D', 'E');
    $series = array(1, 3, 2, 5, 4);
    $chart = $section->addChart('line', $categories, $series, $style);

可见样式选项参考 :ref:`chart-style`.

查看 Sample_32_Chart.php 获得更多选项和样式。

Comments 注释
--------

可以使用 ``addComment`` 将注释添加到文档中。
注释可以包含格式化文本。添加注释后，可以将其链接到具有 ``setCommentStart`` 的任何元素。

.. code-block:: php

    // first create a comment
    $comment= new \PhpOffice\PhpWord\Element\Comment('Authors name', new \DateTime(), 'my_initials');
    $comment->addText('Test', array('bold' => true));

    // add it to the document
    $phpWord->addComment($comment);

    $textrun = $section->addTextRun();
    $textrun->addText('This ');
    $text = $textrun->addText('is');
    // link the comment to the text you just created
    $text->setCommentStart($comment);

如果没有使用 ``setCommentEnd`` 为注释设置结束，则注释将在其启动的元素的末尾自动结束。

Track Changes 跟踪变化
-------------

可以在文本元素上设置轨道更改。有两种方法可以设置元素的更改信息。
通过调用 `setChangeInfo()`或使用 `setTrackChange()` 在元素上设置 `TrackChange` 实例。

.. code-block:: php

    $phpWord = new \PhpOffice\PhpWord\PhpWord();

    // New portrait section
    $section = $phpWord->addSection();
    $textRun = $section->addTextRun();

    $text = $textRun->addText('Hello World! Time to ');

    $text = $textRun->addText('wake ', array('bold' => true));
    $text->setChangeInfo(TrackChange::INSERTED, 'Fred', time() - 1800);

    $text = $textRun->addText('up');
    $text->setTrackChange(new TrackChange(TrackChange::INSERTED, 'Fred'));

    $text = $textRun->addText('go to sleep');
    $text->setChangeInfo(TrackChange::DELETED, 'Barney', new \DateTime('@' . (time() - 3600)));
