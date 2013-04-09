.. Demo App With Pyramid Layout

Pyramid Layout を使ったデモアプリ
=================================

.. Let's see Pyramid Layout in action with the demo application provided
.. in ``demo``.

``demo`` の中で提供されるデモアプリケーションを使って Pyramid Layout が
動いているところを見てみましょう。


.. Installation

インストール
------------

.. Normal Pyramid stuff:

通常の Pyramid のやり方と同じです:


.. #. Make a virtualenv

.. #. ``env/bin/python demo/setup.py develop``

.. #. ``env/bin/pserve demo/development.ini``

.. #. Open ``http://0.0.0.0:6543/`` in a browser

.. #. Click on the ``Home Mako``, ``Home Chameleon``, and
..    ``Home Jinja2`` links in the header to see views for that use each.


#. virtualenv を作る

#. ``env/bin/python demo/setup.py develop``

#. ``env/bin/pserve demo/development.ini``

#. ブラウザで ``http://0.0.0.0:6543/`` を開く

#. ヘッダーの ``Home Mako``, ``Home Chameleon``, ``Home Jinja2`` リンクを
   クリックして、それぞれで使われるビューを見る


.. Now let's look at some of the code.

それではいくつかコードを見ていきましょう。


.. Registration

登録
------------

.. Pyramid Layout defines configuration directives and decorators you can
.. use in your project. We need those loaded into our code. The demo does
.. this in the ``etc/development.ini`` file:

Pyramid Layout は、プロジェクトの中で使用できる設定ディレクティブと
デコレータを定義しています。それらをコードの中にロードする必要があります。
デモでは ``etc/development.ini`` ファイルでこれを行っています:


.. literalinclude:: ../demo/development.ini
    :lines: 9-12
    :language: ini



.. The ``development.ini`` entry point starts in ``demo/__init__.py``:

``development.ini`` エントリーポイントは ``demo/__init__.py`` の中で
開始します:


.. literalinclude:: ../demo/demo/__init__.py
    :language: python
    :linenos:


.. This is all Configurator action. We register a route for each view. We
.. then scan our ``demo/layouts.py``, ``demo/panels.py``, and
.. ``demo/views.py`` for registrations.

これはすべて Configurator アクションです。ここでは各ビューの route を
登録しています。そして、登録のために ``demo/layouts.py``, ``demo/panels.py``,
``demo/views.py`` を scan します。


Layout
------

.. Let's start with the big picture: the global look-and-feel via a :term:`layout`:

全体像から始めましょう: :term:`Layout <layout>` によるグローバルな
ルックアンドフィールです:


.. literalinclude:: ../demo/demo/layouts.py
    :language: python
    :linenos:


.. The ``@layout_config`` decorator comes from Pyramid Layout and allows
.. us to define and register a :term:`layout`. In this case we've stacked 3
.. decorators, thus making 3 layouts, one for each template language.

``@layout_config`` デコレータは Pyramid Layout から来たもので、
:term:`Layout <layout>` を定義および登録することができます。この場合、
3つのデコレータを積み重ねています。それぞれのテンプレート言語に対して
1つずつ、3つの Layout を作っています。


.. note::

    .. The first ``@layout_config`` doesn't have a ``name`` and is thus
    .. the layout that you will get if your view doesn't specifically
    .. choose which layout it wants.

    最初の ``@layout_config`` は ``name`` を持っていません。そのため
    それはビューがどの Layout が欲しいか特に指定しなかった場合に
    得られる Layout になります。


.. Lines 21-24 illustrates the concept of keeping templates and the template
.. logic close together. All views need to show the ``project_title``.
.. It's part of the global look-and-feel :term:`main template`. So we put this
.. logic on the *layout*, in one place as part of the global contract,
.. rather than having each view supply that data/logic.

21-24 行目は、テンプレートとテンプレートロジックをお互いの近くに置いておく
ことの概念を例示しています。すべてのビューで ``project_title`` を表示する
必要があります。それはグローバルなルックアンドフィールの
:term:`main template` の一部です。そのため、それぞれのビューがデータ/ロジック
を持つのではなく、グローバルな契約の一部としてこのロジックを *layout* の中の
1つの場所に置いています。


.. Let's next look at where this is used in the template for one of the
.. 3 layouts. In this case, the Mako template at
.. ``demo/templates/layouts/layout.mako``:

次に、これが3つの Layout のうちの1つに対するテンプレートの中で
使われている場所を見ましょう。この場合は
``demo/templates/layouts/layout.mako`` の Mako テンプレートです:


.. code-block:: mako

    <title>${layout.project_title}, from Pylons Project</title>


.. Here we see an important concept and some important magic: the template
.. has a top-level variable ``layout`` available. This is an instance of
.. your :term:`layout class`.

ここで、重要な概念と、ある重要なマジックを見ることができます: テンプレート
にはトップレベルの変数 ``layout`` があります。これは :term:`Layout クラス
<layout class>` のインスタンスです。


.. For the ZPT crowd, if you look at the master template in
.. ``demo/templates/layouts/layout.pt``, you might notice something weird
.. at the top: there's no ``metal:define-macro``. Since Chameleon allows a
.. template to be a top-level macro, Pyramid Layout automatically binds
.. the entire template to the macro named ``main_template``.

ZPT の人々にとっては、 ``demo/templates/layouts/layout.pt`` にある
マスターテンプレートを見ると先頭に不思議なものがあることに気がつくかも
しれません: ``metal:define-macro`` はありません。 Chameleon は
テンプレートがトップレベルのマクロになることを許すので、 Pyramid Layout
は自動的に全テンプレートを ``main_template`` という名前のマクロに結び付けます。


.. How does your view know to use a :term:`layout`? Let's take a look.

ビューはどのようにして :term:`Layout <layout>` を使用する方法を知るのでしょうか。
見てみましょう。


.. Connecting Views to a Layout

ビューを Layout に接続する
----------------------------

.. Our demo app has a very simple set of views:

デモアプリには、いくつかの非常に単純なビューがあります:


.. literalinclude:: ../demo/demo/views.py
    :language: python
    :linenos:


.. We again have one callable with 3 stacked decorators. The decorators
.. are all normal Pyramid ``@view_config`` stuff.

再び3つの積み重なったデコレータと1つのcallableがあります。
デコレータはすべて通常の Pyramid ``@view_config`` のものです。


.. The second one points at a Chameleon template in
.. ``demo/templates/home.pt``:

2つ目のデコレータは ``demo/templates/home.pt`` にある Chameleon テンプレート
を指しています:


.. literalinclude:: ../demo/demo/templates/home.pt
    :language: html


.. The first line is the one that opts the template into the :term:`layout`. In
.. ``home.jinja2`` that line looks like:

最初の行はテンプレートを :term:`Layout <layout>` に opt into するものです。
``home.jinja2`` の中では、この行はこんな風になります:


.. code-block:: jinja

  {% extends main_template %}


.. For both of these, ``main_template`` is inserted by Pyramid Layout,
.. via a Pyramid renderer global, into the template's global namespace.
.. After that, it's normal semantics for that template language.

これら両方について、 ``main_template`` が Pyramid Layout によって
Pyramid レンダラーグローバル変数を通してテンプレートのグローバルな名前空間に
挿入されます。その後は、そのテンプレート言語用の通常のセマンティクスに
なります。


.. Back to ``views.py``. The view function grabs the ``Layout Manager``,
.. which Pyramid Layout conveniently stashes on the request. The
.. ``LayoutManager``'s primary job is getting/setting the current :term:`layout`.
.. Which, of course, we do in this function.

``views.py`` に戻りましょう。このビュー関数は ``Layout Manager`` を
取得しています。 Pyramid Layout は利便性のためにそれを request に格納
しています。 ``LayoutManager`` の主な役割は、現在の :term:`Layout <layout>` の
値の get/set です。もちろん、それはこの関数の中で行っていることです。


.. Our function then grabs the :term:`layout instance` and manipulates some state
.. that is needed in the global look and feel. This, of course, could have been
.. done in our ``AppLayout`` class, but in some cases, different views have
.. different values for the headings.

その後、その関数は :term:`Layout インスタンス <layout instance>` を取得して
グローバルなルックアンドフィールに必要な何らかの状態を操作します。もちろん
これは ``AppLayout`` クラスでもできたでしょうが、場合によっては別のビューが
見出しに対して異なる値を持っていることがあります。


.. Re-Usable Snippets with Panels

Panel による再利用可能なスニペット
------------------------------------

.. Our :term:`main template` has something interesting in it:

:term:`main template` は、その中に興味深いものを持っています:


.. literalinclude:: ../demo/demo/templates/layouts/layout.mako
    :lines: 27-49
    :emphasize-lines: 3,12
    :language: mako


.. Here we break our global layout into reusable parts via :term:`panels <panel>`.
.. Where do these come from? ``@panel_config`` decorators, as shown in
.. ``panels.py``. For example, this:

ここで、グローバルな Layout を :term:`Panel <panel>` によって再利用可能な部分に分解
しています。これらはどこから来たのでしょうか。 ``@panel_config``
デコレータです。それは ``panels.py`` の中で見ることができます。
例えばこれは:


.. code-block:: mako

  ${panel('navbar')}


.. ...comes from this:

...ここから来ています:


.. literalinclude:: ../demo/demo/panels.py
    :language: python
    :lines: 4-25


.. The ``@panel_config`` registered a panel under the name ``navbar``,
.. which our template could then use **or override**.

``@panel_config`` は、 ``navbar`` という名前で Panel を登録しました。
その後、テンプレートはそれを使用、 **またはオーバーライド** することが
できます。


.. The ``home.mako`` view template has a more interesting panel:

``home.mako`` ビューテンプレートにはさらに面白い Panel があります:


.. code-block:: mako

  ${panel('hero', title='Mako')}


.. ...which calls:

...これは以下を呼び出します:


.. literalinclude:: ../demo/demo/panels.py
    :language: python
    :lines: 28-33
    :linenos:


.. This shows that a :term:`panel` can be parameterized and used in different
.. places in different ways.

これは、 :term:`Panel <panel>` をパラメータ化して、他の場所で異なる方法で利用
できるということを示しています。
