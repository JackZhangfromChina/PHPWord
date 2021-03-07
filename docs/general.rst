.. _general:

常见用法
=============

基本示例
-------------

下面是PHPWord库的基本示例。 更多的示例在 `samples
目录下 <https://github.com/PHPOffice/PHPWord/tree/master/samples/>`__。

.. code-block:: php

    <?php
    require_once 'bootstrap.php';

    // Creating the new document...
    $phpWord = new \PhpOffice\PhpWord\PhpWord();

    /* Note: any element you append to a document must reside inside of a Section. */

    // Adding an empty Section to the document...
    $section = $phpWord->addSection();
    // Adding Text element to the Section having font styled by default...
    $section->addText(
        '"Learn from yesterday, live for today, hope for tomorrow. '
            . 'The important thing is not to stop questioning." '
            . '(Albert Einstein)'
    );

    /*
     * Note: it's possible to customize font style of the Text element you add in three ways:
     * - inline;
     * - using named font style (new font style object will be implicitly created);
     * - using explicitly created font style object.
     */

    // Adding Text element with font customized inline...
    $section->addText(
        '"Great achievement is usually born of great sacrifice, '
            . 'and is never the result of selfishness." '
            . '(Napoleon Hill)',
        array('name' => 'Tahoma', 'size' => 10)
    );

    // Adding Text element with font customized using named font style...
    $fontStyleName = 'oneUserDefinedStyle';
    $phpWord->addFontStyle(
        $fontStyleName,
        array('name' => 'Tahoma', 'size' => 10, 'color' => '1B2232', 'bold' => true)
    );
    $section->addText(
        '"The greatest accomplishment is not in never falling, '
            . 'but in rising again after you fall." '
            . '(Vince Lombardi)',
        $fontStyleName
    );

    // Adding Text element with font customized using explicitly created font style object...
    $fontStyle = new \PhpOffice\PhpWord\Style\Font();
    $fontStyle->setBold(true);
    $fontStyle->setName('Tahoma');
    $fontStyle->setSize(13);
    $myTextElement = $section->addText('"Believe you can and you\'re halfway there." (Theodor Roosevelt)');
    $myTextElement->setFontStyle($fontStyle);

    // Saving the document as OOXML file...
    $objWriter = \PhpOffice\PhpWord\IOFactory::createWriter($phpWord, 'Word2007');
    $objWriter->save('helloWorld.docx');

    // Saving the document as ODF file...
    $objWriter = \PhpOffice\PhpWord\IOFactory::createWriter($phpWord, 'ODText');
    $objWriter->save('helloWorld.odt');

    // Saving the document as HTML file...
    $objWriter = \PhpOffice\PhpWord\IOFactory::createWriter($phpWord, 'HTML');
    $objWriter->save('helloWorld.html');

    /* Note: we skip RTF, because it's not XML-based and requires a different example. */
    /* Note: we skip PDF, because "HTML-to-PDF" approach is used to create PDF documents. */

PHPWord 设置
----------------

``PhpOffice\PhpWord\Settings`` 类提供了一些选项，这些选项将
影响PHPWord的行为。以下是选项。

XML Writer compatibility
~~~~~~~~~~~~~~~~~~~~~~~~

此选项设置
`XMLWriter::setIndent <http://www.php.net/manual/en/function.xmlwriter-set-indent.php>`__
和
`XMLWriter::setIndentString <http://www.php.net/manual/en/function.xmlwriter-set-indent-string.php>`__.
默认值是 ``true`` (兼容), 是
`为了
OpenOffice <https://github.com/PHPOffice/PHPWord/issues/103>`__ 来
正确呈现OOXML文档。 在开发过程中，你可以设置为 ``false``
使生成的XML文件更易于阅读。

.. code-block:: php

    \PhpOffice\PhpWord\Settings::setCompatibility(false);

Zip 类
~~~~~~~~~

默认的, PHPWord 使用 `Zip extension <http://php.net/manual/en/book.zip.php>`__
处理压缩的压缩档案和其中的文件。如果你不能
Zip扩展安装在您的服务器上，您可以使用纯PHP库
替代, `PclZip <http://www.phpconcept.net/pclzip/>`__, 其已经内嵌在 PHPWord中。

.. code-block:: php

    \PhpOffice\PhpWord\Settings::setZipClass(\PhpOffice\PhpWord\Settings::PCLZIP);

输出转义
~~~~~~~~~~~~~~~

编写某些格式的文档，尤其是基于XML的文档，需要正确的输出转义。
没有它，当你在文档中添加特殊字符，如 & 符号、引号和其他字符时，您的文档可能会损坏。
转义可以通过两种方式执行: 软件开发人员在库之外，通过内置机制在库内部。
默认情况下，为了与v0.13.0之前的版本向后兼容，禁用了内置机制。
要将其打开，请在PHPWord配置文件中将 `outputescape ingenabled` 选项设置为`true`，或在运行时使用以下指令:

.. code-block:: php

    \PhpOffice\PhpWord\Settings::setOutputEscapingEnabled(true);

默认字体
~~~~~~~~~~~~

默认情况下，每个文本都显示在Arial 10字号中。您可以更改
使用以下两个函数默认字体:

.. code-block:: php

    $phpWord->setDefaultFontName('Times New Roman');
    $phpWord->setDefaultFontSize(12);

文档设置
-----------------
可以使用``$phpWord->getSettings()`` 获取生成的文档的设置

放大设置
~~~~~~~~~~~~~~~~~~~~~
默认缩放值为100%。这可以更改为另一个百分比

.. code-block:: php

    $phpWord->getSettings()->setZoom(75);

或者 预定义的值 ``fullPage``, ``bestFit``, ``textFit``

.. code-block:: php

    $phpWord->getSettings()->setZoom(Zoom::BEST_FIT);

镜像页边距
~~~~~~~~~~~~~~~~~~~~~~~~~~
使用镜像边距为双面文档 (如书籍或杂志) 设置面向页面。

.. code-block:: php

    $phpWord->getSettings()->setMirrorMargins(true);

拼写和语法检查
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

默认情况下，打开word文档后会显示拼写和语法错误。
对于大型文档，这会减慢文档的打开速度。您可以通过以下方式隐藏拼写和/或语法错误:

.. code-block:: php

    $phpWord->getSettings()->setHideGrammaticalErrors(true);
    $phpWord->getSettings()->setHideSpellingErrors(true);

您还可以指定拼写和语法检查的状态，将拼写或语法标记为脏将在打开文档时强制重新检查。

.. code-block:: php

    $proofState = new ProofState();
    $proofState->setGrammar(ProofState::CLEAN);
    $proofState->setSpelling(ProofState::DIRTY);

    $phpWord->getSettings()->setProofState(proofState);

跟踪修订
~~~~~~~~~~~~~~~
可以使用 ``setTrackRevisions`` 激活跟踪修订, 您可以进一步指定

-  不使用移动语法，而是移动的项目将在一个地方被视为删除，并在另一个地方被添加
-  不跟踪格式修订

.. code-block:: php

    $phpWord->getSettings()->setTrackRevisions(true);
    $phpWord->getSettings()->setDoNotTrackMoves(true);
    $phpWord->getSettings()->setDoNotTrackFormatting(true);

十进制符号
~~~~~~~~~~~~~~
表示十进制数字的默认符号是英语中的 ``.``。例如，在法语中，您可能需要将其更改为 ``，``。

.. code-block:: php

    $phpWord->getSettings()->setDecimalSymbol(',');

文档语言
~~~~~~~~~~~~~~~~~
可以通过以下方式更改文档的默认语言。

.. code-block:: php

    $phpWord->getSettings()->setThemeFontLang(new Language(Language::FR_BE));

``Language`` 有3个参数，一个用于拉丁语，一个用于东亚语言，一个用于复杂 (双向) 语言。
几个语言代码在 ``PhpOffice\PhpWord\ComplexType\Language`` 类里提供了但是也可使用任何有效的 code/ID 。

如果您要生成RTF文档，则需要对语言进行不同的设置。

.. code-block:: php

    $lang = new Language();
    $lang->setLangId(Language::EN_GB_ID);
    $phpWord->getSettings()->setThemeFontLang($lang);

文件信息
--------------------

您可以设置文档信息，如标题、创建者和公司
名称。使用以下功能:

.. code-block:: php

    $properties = $phpWord->getDocInfo();
    $properties->setCreator('My name');
    $properties->setCompany('My factory');
    $properties->setTitle('My title');
    $properties->setDescription('My description');
    $properties->setCategory('My category');
    $properties->setLastModifiedBy('My name');
    $properties->setCreated(mktime(0, 0, 0, 3, 12, 2014));
    $properties->setModified(mktime(0, 0, 0, 3, 14, 2014));
    $properties->setSubject('My subject');
    $properties->setKeywords('my, key, word');

测量单位
-----------------

Open Office XML中的基本长度单位是twip。Twip的意思是 “二十
一分之一 一英寸点 ”，即1 twip = 1/1440英寸。

您可以使用PHPWord帮助函数来转换英寸、厘米或 点twip。

.. code-block:: php

    // Paragraph with 6 points space after
    $phpWord->addParagraphStyle('My Style', array(
        'spaceAfter' => \PhpOffice\PhpWord\Shared\Converter::pointToTwip(6))
    );

    $section = $phpWord->addSection();
    $sectionStyle = $section->getStyle();
    // half inch left margin
    $sectionStyle->setMarginLeft(\PhpOffice\PhpWord\Shared\Converter::inchToTwip(.5));
    // 2 cm right margin
    $sectionStyle->setMarginRight(\PhpOffice\PhpWord\Shared\Converter::cmToTwip(2));

文件保护
-------------------

文档 (或部分文档) 可以通过密码保护。

.. code-block:: php

    $documentProtection = $phpWord->getSettings()->getDocumentProtection();
    $documentProtection->setEditing(DocProtect::READ_ONLY);
    $documentProtection->setPassword('myPassword');

自动重新计算打开时的域
----------------------------------------

要强制更新文档中存在的字段，请将updateFields设置为true

.. code-block:: php

    $phpWord->getSettings()->setUpdateFields(true);

连字
-----------
连字符描述了用连字符破译单词的过程。有几个选项可以控制连字符。

自动连字符
~~~~~~~~~~~~~~~~

自动断字文本套装 `autoHyphenation` 转 `true `。

.. code-block:: php

    $phpWord->getSettings()->setAutoHyphenation(true);

连续连字符限制
~~~~~~~~~~~~~~~~~~~~~~~~

以连字符结尾的文本的最大连续行数可以由 ``连续连字符限制`` 选项控制。
如果选项未设置或提供的值为 ``0``，则没有限制。

.. code-block:: php

    $phpWord->getSettings()->setConsecutiveHyphenLimit(2);

连字符区
~~~~~~~~~~~~~~~~

连字符区域 (在 *twip* 中) 是应用连字符之前允许的空格量。
连字符区越小，连字符越多。或者换句话说，连字符区越宽，连字符就越少。

.. code-block:: php

    $phpWord->getSettings()->setHyphenationZone(\PhpOffice\PhpWord\Shared\Converter::cmToTwip(1));

连字符帽
~~~~~~~~~~~~~~

为了控制所有大写字母中的单词是否应使用 `donothyphenatecap` 选项连字符。

.. code-block:: php

    $phpWord->getSettings()->setDoNotHyphenateCaps(true);
