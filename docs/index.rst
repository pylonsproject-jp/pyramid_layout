.. Pyramid Layout: Composable UX for Pyramid

=============================================
Pyramid Layout: Pyramid のための構成可能な UX
=============================================

.. Making an attractive, efficient user-experience (UX) is hard. Pyramid Layout
.. provides a layout-based approach to building your global look-and-feel
.. then re-using it across your site. You can then manage your global UX
.. :term:`layout` as a unit, just like models, views, static resources, and
.. other parts of Pyramid.

魅力的で効率的なユーザーエクスペリエンス (UX) を作ることは困難です。
Pyramid Layout は、全体的なルックアンドフィールを構築し、それをあなたの
サイトで横断的に再利用するための Layout ベースのアプローチを提供します。
それにより、モデルやビュー、静的リソースまたは Pyramid の他の部分と
同じように、グローバルな UX :term:`Layout <layout>` を1つの単位として
管理することができます。


.. If you are OCD, and want the same ways to organize and override your UX
.. that you get in your Python code, this :term:`layout` approach is your
.. cup of tea.

あなたが OCD (Obsessive-Compulsive Disorder; 強迫性障害 = 細かいことが
気になる人) で、 UX を構造化したりオーバーライドしたりするために Python
コード中と同じ方法を望むなら、この :term:`Layout <layout>` アプローチを気に入る
でしょう。


.. Approach

アプローチ
==========

.. - Make one (or more) :term:`layout` objects of template and template logic

- 1つ (あるいは複数の) テンプレートとテンプレートロジックの :term:`Layout <layout>`
  オブジェクトを作ります。


.. - Do useful things with this unit of layout: registration,
..   dynamic association with a view, pluggability via Pyramid overrides,
..   testing in isolation

- この Layout の単位で何か有用なことをする: 登録、ビューとの動的な関連、
  Pyramid オーバーライドによるプラグ可能性、単独でのテスト


.. - Layouts can share lightweight units called :term:`panels <panel>` which are
..   objects of template and code, sharing the same useful things

- Layout は :term:`Panel <panel>` と呼ばれる軽量の単位を共有することができます。
  それはテンプレートとコードのオブジェクトで、同じ有用なものを共有しています。


.. - Use of any of the common Pyramid templating engines (Chameleon ZPT, Mako,
..   Jinja2) is tested and supported with examples.

- 一般的な Pyramid テンプレートエンジン (Chameleon ZPT, Mako, Jinja2)
  の使用はテスト済みで、例と共にサポートされます。


Contents
========

.. toctree::
    :maxdepth: 2

    about
    layouts
    demo
    api
    glossary

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`

