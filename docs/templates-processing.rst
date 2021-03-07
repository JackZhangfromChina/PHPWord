.. _templates-processing:

模板处理
====================

您可以使用包含的搜索模式 (宏) 创建OOXML文档模板，该模板可以替换为您想要的任何值。只能替换单行值。
宏的定义如下: ``${search-pattern}``。
要加载模板文件，请创建模板处理器的新实例。

.. code-block:: php

    $templateProcessor = new TemplateProcessor('Template.docx');

设单值
""""""""
给模板包含的一个替换

.. code-block:: clean

    Hello ${firstname} ${lastname}!

The following will replace ``${firstname}`` with ``John``, and ``${lastname}`` with ``Doe`` .
The resulting document will now contain ``Hello John Doe!``

.. code-block:: php

    $templateProcessor->setValue('firstname', 'John');
    $templateProcessor->setValue('lastname', 'Doe');

设置一组值
"""""""""
你还可以通过在数组中传递所有值来设置多个值。

.. code-block:: php

    $templateProcessor->setValues(array('firstname' => 'John', 'lastname' => 'Doe'));

setImageValue 设置图像值
"""""""""""""
要寻找的图像模式可能像下面的模式:
    - ``${search-image-pattern}``
    - ``${search-image-pattern:[width]:[height]:[ratio]}``
    - ``${search-image-pattern:[width]x[height]}``
    - ``${search-image-pattern:size=[width]x[height]}``
    - ``${search-image-pattern:width=[width]:height=[height]:ratio=false}``

Where:
    - [width] and [height] can be just numbers or numbers with measure, which supported by Word (cm, mm, in, pt, pc, px, %, em, ex)
    - [ratio] uses only for ``false``, ``-`` or ``f`` to turn off respect aspect ration of image. By default template image size uses as 'container' size.

Example:

.. code-block:: clean

    ${CompanyLogo}
    ${UserLogo:50:50} ${Name} - ${City} - ${Street}

.. code-block:: php

    $templateProcessor = new TemplateProcessor('Template.docx');
    $templateProcessor->setValue('Name', 'John Doe');
    $templateProcessor->setValue(array('City', 'Street'), array('Detroit', '12th Street'));

    $templateProcessor->setImageValue('CompanyLogo', 'path/to/company/logo.png');
    $templateProcessor->setImageValue('UserLogo', array('path' => 'path/to/logo.png', 'width' => 100, 'height' => 100, 'ratio' => false));

克隆块
""""""""""
模板中包含
参考 ``Sample_23_TemplateBlock.php`` 为示例.

.. code-block:: clean

    ${block_name}
    Customer: ${customer_name}
    Address: ${customer_address}
    ${/block_name}

以下内容将复制 ``${block_name}`` 和 ``${/block_name}`` 之间的所有内容3次。

.. code-block:: php

    $templateProcessor->cloneBlock('block_name', 3, true, true);

最后一个参数将重命名块中定义的任何宏，并添加 #1、 #2、 #3...到宏名称。
结果将是

.. code-block:: clean

    Customer: ${customer_name#1}
    Address: ${customer_address#1}
    
    Customer: ${customer_name#2}
    Address: ${customer_address#2}
    
    Customer: ${customer_name#3}
    Address: ${customer_address#3}

也可以传递带有值的数组来替换marcros。
如果传递了带有替换项的数组，则忽略 ``count`` 参数，它是计数数组的大小。

.. code-block:: php

    $replacements = array(
        array('customer_name' => 'Batman', 'customer_address' => 'Gotham City'),
        array('customer_name' => 'Superman', 'customer_address' => 'Metropolis'),
    );
    $templateProcessor->cloneBlock('block_name', 0, true, false, $replacements);

结果将是

.. code-block:: clean

    Customer: Batman
    Address: Gotham City
    
    Customer: Superman
    Address: Metropolis

replaceBlock 替换块
""""""""""""
模板内容包含
.. code-block:: clean

    ${block_name}
    This block content will be replaced
    ${/block_name}

以下内容将 ``${block_name}`` 和 ``${/block_name}`` 之间的所有内容替换为传递的值。

.. code-block:: php

    $templateProcessor->replaceBlock('block_name', 'This is the replacement text.');

deleteBlock 删除块
"""""""""""
和之前类似，但是删除块

.. code-block:: php

    $templateProcessor->deleteBlock('block_name');

cloneRow 克隆行
""""""""
模板文档中克隆表格的一行
参考 ``Sample_07_TemplateCloneRow.php`` 示例.

.. code-block:: clean

    +-----------+----------------+
    | ${userId} | ${userName}    |
    |           |----------------+
    |           | ${userAddress} |
    +-----------+----------------+

.. code-block:: php

    $templateProcessor->cloneRow('userId', 2);

结果将为

.. code-block:: clean

    +-------------+------------------+
    | ${userId#1} | ${userName#1}    |
    |             |------------------+
    |             | ${userAddress#1} |
    +-------------+------------------+
    | ${userId#2} | ${userName#2}    |
    |             |------------------+
    |             | ${userAddress#2} |
    +-------------+------------------+

cloneRowAndSetValues 克隆并赋值
""""""""""""""""""""""""""""""

在由 `$search` 参数标识的表行中查找行，并将其克隆到`$values`中的条目的次数。

.. code-block:: clean

    +-----------+----------------+
    | ${userId} | ${userName}    |
    |           |----------------+
    |           | ${userAddress} |
    +-----------+----------------+

.. code-block:: php

    $values = [
        ['userId' => 1, 'userName' => 'Batman', 'userAddress' => 'Gotham City'],
        ['userId' => 2, 'userName' => 'Superman', 'userAddress' => 'Metropolis'],
    ];
    $templateProcessor->cloneRowAndSetValues('userId', );

结果将是

.. code-block:: clean

    +---+-------------+
    | 1 | Batman      |
    |   |-------------+
    |   | Gotham City |
    +---+-------------+
    | 2 | Superman    |
    |   |-------------+
    |   | Metropolis  |
    +---+-------------+

应用xsl样式表
""""""""""""""""""

应用传递给页眉部分、页脚部分和主要部分的XSL样式表

.. code-block:: php

    $xslDomDocument = new \DOMDocument();
    $xslDomDocument->load('/path/to/my/stylesheet.xsl');
    $templateProcessor->applyXslStyleSheet($xslDomDocument);

设置复杂值
"""""""""""""""

将 ${macro} 删除，并传递复杂类型。
参见 ``Sample_40_TemplateSetComplexValue.php``示例。

.. code-block:: php

    $inline = new TextRun();
    $inline->addText('by a red italic text', array('italic' => true, 'color' => 'red'));
    $templateProcessor->setComplexValue('inline', $inline);

设置复杂块
"""""""""""""""
将 ${macro} 删除，并传递复杂类型。
参见 ``Sample_40_TemplateSetComplexValue.php`` 示例。

.. code-block:: php

    $table = new Table(array('borderSize' => 12, 'borderColor' => 'green', 'width' => 6000, 'unit' => TblWidth::TWIP));
    $table->addRow();
    $table->addCell(150)->addText('Cell A1');
    $table->addCell(150)->addText('Cell A2');
    $table->addCell(150)->addText('Cell A3');
    $table->addRow();
    $table->addCell(150)->addText('Cell B1');
    $table->addCell(150)->addText('Cell B2');
    $table->addCell(150)->addText('Cell B3');
    $templateProcessor->setComplexBlock('table', $table);
