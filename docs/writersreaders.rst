.. _writersreaders:

Writers & readers
=================

OOXML
-----

OOXML 文档包 由下文件构成。

- \_rels/

   - .rels

- docProps/

   - app.xml
   - core.xml
   - custom.xml

- word/

   - rels/

      - document.rels.xml

   - media/
   - theme/

      - theme1.xml

   - document.xml
   - fontTable.xml
   - numbering.xml
   - settings.xml
   - styles.xml
   - webSettings.xml

- [Content\_Types].xml

OpenDocument
------------

Package
~~~~~~~

OpenDocument 文档包 由下文件构成。

- META-INF/

   - manifest.xml

- Pictures/
- content.xml
- meta.xml
- styles.xml

content.xml
~~~~~~~~~~~

``content.xml`` 的结构如下所示。

- office:document-content

   - office:font-facedecls
   - office:automatic-styles
   - office:body

      - office:text

         - draw:\*
         - office:forms
         - table:table
         - text:list
         - text:numbered-paragraph
         - text:p
         - text:table-of-contents
         - text:section

      - office:chart
      - office:image
      - office:drawing

styles.xml
~~~~~~~~~~

``styles.xml`` 的结构如下所示。

- office:document-styles

   - office:styles
   - office:automatic-styles
   - office:master-styles

      - office:master-page

RTF
---

待完成。

HTML
----

待完成。

PDF
---

待完成。
