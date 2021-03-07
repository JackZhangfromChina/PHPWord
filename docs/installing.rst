.. _setup:

安装/配置
======================

配置
------------

强制:

- PHP 5.3.3+
- `XML Parser <http://www.php.net/manual/en/xml.installation.php>`__ 扩展
- `Zend\\Escaper <http://framework.zend.com/manual/current/en/modules/zend.escaper.introduction.html>`__ 组件
- Zend\\Stdlib 组件
- `Zend\\Validator <http://framework.zend.com/manual/current/en/modules/zend.validator.html>`__ 组件

可选:

- `Zip <http://php.net/manual/en/book.zip.php>`__ 扩展
- `GD <http://php.net/manual/en/book.image.php>`__ 扩展
- `XMLWriter <http://php.net/manual/en/book.xmlwriter.php>`__ 扩展
- `XSL <http://php.net/manual/en/book.xsl.php>`__ 扩展
- `dompdf <https://github.com/dompdf/dompdf>`__ 库

安装
------------

PHPWord 通过 `Composer <https://getcomposer.org/>`安装 __.
你只需要在你的包中`添加依赖 <https://getcomposer.org/doc/04-schema.md#package-links>`__ 。

Example:

.. code-block:: json

    {
        "require": {
           "phpoffice/phpword": "v0.14.*"
        }
    }

如果您是开发人员，或者您想帮助我们进行测试，请为开发人员获取最新的分支。
注意: 所有贡献必须针对开发者分支机构。

Example:

.. code-block:: json

    {
        "require": {
           "phpoffice/phpword": "dev-develop"
        }
    }

示例
-------------

“samples” 目录中提供了更多示例。
为了便于访问这些示例，请在samples目录中启动 “php-s localhost:8000”，然后浏览到http:// localhost:8000以查看示例。
