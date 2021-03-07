.. _recipes:

Recipes 小记
=======

创建左浮动图像
-----------------------

使用相对于水平边距和垂直线的绝对定位。

.. code-block:: php

    $imageStyle = array(
        'width' => 40,
        'height' => 40,
        'wrappingStyle' => 'square',
        'positioning' => 'absolute',
        'posHorizontalRel' => 'margin',
        'posVerticalRel' => 'line',
    );
    $textrun->addImage('resources/_earth.jpg', $imageStyle);
    $textrun->addText($lipsumText);

自动下载生成的文件
----------------------------------------

使用 ``php://output`` 作为文件名.

.. code-block:: php

    $phpWord = new \PhpOffice\PhpWord\PhpWord();
    $section = $phpWord->createSection();
    $section->addText('Hello World!');
    $file = 'HelloWorld.docx';
    header("Content-Description: File Transfer");
    header('Content-Disposition: attachment; filename="' . $file . '"');
    header('Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document');
    header('Content-Transfer-Encoding: binary');
    header('Cache-Control: must-revalidate, post-check=0, pre-check=0');
    header('Expires: 0');
    $xmlWriter = \PhpOffice\PhpWord\IOFactory::createWriter($phpWord, 'Word2007');
    $xmlWriter->save("php://output");

创建编号标题
------------------------


定义编号样式和标题样式，并将这两种样式 (带有 ``pStyle``和``numStyle``) 匹配，如下所示。

.. code-block:: php

    $phpWord->addNumberingStyle(
        'hNum',
        array('type' => 'multilevel', 'levels' => array(
            array('pStyle' => 'Heading1', 'format' => 'decimal', 'text' => '%1'),
            array('pStyle' => 'Heading2', 'format' => 'decimal', 'text' => '%1.%2'),
            array('pStyle' => 'Heading3', 'format' => 'decimal', 'text' => '%1.%2.%3'),
            )
        )
    );
    $phpWord->addTitleStyle(1, array('size' => 16), array('numStyle' => 'hNum', 'numLevel' => 0));
    $phpWord->addTitleStyle(2, array('size' => 14), array('numStyle' => 'hNum', 'numLevel' => 1));
    $phpWord->addTitleStyle(3, array('size' => 12), array('numStyle' => 'hNum', 'numLevel' => 2));

    $section->addTitle('Heading 1', 1);
    $section->addTitle('Heading 2', 2);
    $section->addTitle('Heading 3', 3);

在标题中添加链接
-------------------------

将 'HeadingN' 段落样式应用于TextRun或Link。示例代码:

.. code-block:: php

    $phpWord = new \PhpOffice\PhpWord\PhpWord();
    $phpWord->addTitleStyle(1, array('size' => 16, 'bold' => true));
    $phpWord->addTitleStyle(2, array('size' => 14, 'bold' => true));
    $phpWord->addFontStyle('Link', array('color' => '0000FF', 'underline' => 'single'));

    $section = $phpWord->addSection();

    // Textrun
    $textrun = $section->addTextRun('Heading1');
    $textrun->addText('The ');
    $textrun->addLink('https://github.com/PHPOffice/PHPWord', 'PHPWord', 'Link');

    // Link
    $section->addLink('https://github.com/', 'GitHub', 'Link', 'Heading2');

删除MS Word标题栏中的 [Compatibility Mode] 文本
----------------------------------------------

使用 ``Metadata\Compatibility\setOoxmlVersion(n)`` 方法 其中 ``n`` 为 Office版本 (14 = Office 2010，15 = Office 2013)。

.. code-block:: php

    $phpWord->getCompatibility()->setOoxmlVersion(15);
