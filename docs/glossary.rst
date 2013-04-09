.. Glossary

.. _glossary:

用語集
========

.. glossary::
   :sorted:

   layout
     .. The basic unit of reusable look and feel, a layout consists of a 
     .. :term:`main template` and a :term:`layout class`.

     再利用可能なルックアンドフィールの基本単位。 Layout は
     :term:`main template` と :term:`Layout クラス <layout class>` から
     構成されます。


   layout class
     .. A class registered with a layout that can be used as a place to consolidate
     .. API that is common to all or many templates across a project. 

     プロジェクトを横断して、すべてのあるいは多くのテンプレートに共通の
     API を集約する場所として使うことのできる、レイアウトと共に登録された
     クラス。


   layout instance
     .. An instance of a :term:`layout class`.  For each view, a :term:`layout` is
     .. selected and that layout's :term:`layout class` is instantiated for the
     .. current :pyramid:term:`request` and :pyramid:term:`context` and made
     .. available to templates as the :pyramid:term:`renderer global <renderer
     .. globals>`, ``layout``.

     :term:`Layout クラス <layout class>` のインスタンス。それぞれのビューに
     対しては、1つの :term:`Layout <layout>` が選択されます。また、その
     Layout の :term:`Layout クラス <layout class>` は現在の
     :pyramid:term:`request` と :pyramid:term:`context` に対して
     インスタンス化され、 :pyramid:term:`レンダラーグローバル変数
     <renderer globals>` ``layout`` としてテンプレートの中で利用可能になります。

   main template
     .. Also known as the `o-wrap` or `outer wrapper`, this is a template which
     .. contains HTML that is common to all views that share a particular layout.
     .. View templates are derived from the main template and inject their own 
     .. HTML into the HTML defined by the main template.

     `o-wrap` あるいは `outer wrapper` としても知られています。
     これは特定の Layout を共有するすべてのビューに共通の HTML を含む
     テンプレートです。ビューテンプレートは main テンプレートから継承し、
     main テンプレートによって定義された HTML に自分の HTML を注入します。


   panel
     .. A panel is a reusable component that defines the HTML for small piece of an
     .. entire page.  Panels are callables, like :pyramid:term:`views <view
     .. callable>`, and may either return an HTML string or use a
     .. :pyramid:term:`renderer` for generating HTML to embed in the page.

     Panel は、すべてのページの小さな断片のための HTML を定義する、再利用
     可能なコンポーネントです。パネルは :pyramid:term:`view callable` のような
     callable で、ページに埋め込まれる HTML を生成するために
     HTML 文字列を返すか :pyramid:term:`renderer` を使用することができます。
