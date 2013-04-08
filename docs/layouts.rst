.. Using Pyramid Layout

Pyramid Layout を使う
=====================

.. To get started with Pyramid Layout, include :mod:`pyramid_layout` in your 
.. application's config:

Pyramid Layout を使い始めるには、アプリケーションの config に
:mod:`pyramid_layout` を含めてください:


::

    config = Configurator(...)
    config.include('pyramid_layout')


.. Alternately, instead of using the
.. :ref:`the Configurator's <pyramid:configuration_narr>`
.. include method, you can
.. activate Pyramid Layout by changing your application’s .ini file, 
.. using the following line:

あるいは、 :ref:`Configurator <pyramid:configuration_narr>` の include
メソッドを使用する代わりに、次の行によりアプリケーションの .ini
ファイルを変更することによって Pyramid Layout を有効にすることもできます:


::

    pyramid.includes = pyramid_layout


.. Including :mod:`pyramid_layout` in your application adds two new directives
.. to your :pyramid:term:`configurator`: :meth:`add_layout
.. <pyramid_layout.config.add_layout>` and :meth:`add_panel
.. <pyramid_layout.config.add_panel>`.  These directives work very much like
.. :meth:`add_view <pyramid:pyramid.config.Configurator.add_view>`, but add
.. registrations for layouts and panels.  Including :mod:`pyramid_layout` will
.. also add an attribute, ``layout_manager``, to the request object of each
.. request, which is an instance of :class:`LayoutManager
.. <pyramid_layout.layout.LayoutManager>`.

アプリケーションに :mod:`pyramid_layout` をインクルードすることで、
:meth:`add_layout <pyramid_layout.config.add_layout>` と
:meth:`add_panel <pyramid_layout.config.add_panel>` という2つの新しい
ディレクティブが :pyramid:term:`configurator` に追加されます。これらの
ディレクティブは、 :meth:`add_view
<pyramid:pyramid.config.Configurator.add_view>` ととてもよく似た働きを
しますが、 Layout と Panel のためのレジストリを追加します。
:mod:`pyramid_layout` をインクルードすることで、さらに各リクエストの
リクエストオブジェクトに属性 (``layout_manager``) が追加されます。
それは :class:`LayoutManager <pyramid_layout.layout.LayoutManager>` の
インスタンスです。


.. Finally, three renderer globals are added which will be available to all
.. templates: ``layout``, ``main_template``, and ``panel``.  ``layout`` is an
.. instance of the :term:`layout class` of the current layout.  ``main_template``
.. is the template object that provides the :term:`main template` (aka, o-wrap)
.. for the view.  ``panel``, a shortcut for :meth:`LayoutManager.render_panel
.. <pyramid_layout.layout.LayoutManager.render_panel>`,  is a callable used to
.. render :term:`panels <panel>` in your templates.

最後に、すべてのテンプレートで利用可能な 3 つのレンダラーグローバル変数が
追加されます: ``layout``, ``main_template``, ``panel`` です。
``layout`` は現在のレイアウトの :term:`Layout クラス <layout class>` のインスタンスです。
``main_template`` は、ビューに :term:`main template` (別名 o-wrap) を
提供するテンプレートオブジェクトです。
``panel`` (:meth:`LayoutManager.render_panel
<pyramid_layout.layout.LayoutManager.render_panel>` のショートカット) は、
テンプレートの中で :term:`Panel <panel>` をレンダリングするために使う
callable です。


.. Using Layouts

Layout を使う
-------------

.. A :term:`layout` consists of a :term:`layout class` and :term:`main template`.
.. The layout class will be instantiated on a per request basis with the context
.. and request as arguments.  The layout class can be omitted, in which case a
.. default layout class will be used, which only assigns `context` and `request`
.. to the layout instance.  Generally, though, you will provide your own layout
.. class which can serve as a place to provide API that will be available to your
.. templates.  A simple layout class might look like:

:term:`Layout <layout>` は :term:`Layout クラス <layout class>` と
:term:`main template` から構成されます。 Layout クラスは、リクエスト毎に
引数としてコンテキストとリクエストを伴ってインスタンス化されます。 Layout
クラスは省略可能で、その場合はデフォルトの Layout クラスが使用されます。
それは単に layout インスタンスに `context` と `request` を代入します。
しかし、テンプレートの中で利用可能な API を提供する場所として使える
自分自身の Layout クラスを提供するのが一般的でしょう。
単純な Layout クラスはこんな風になります:


::

    class MyLayout(object):
        page_title = 'Hooray! My App!'

        def __init__(self, context, request):
            self.context = context
            self.request = request
            self.home_url = request.application_url

        def is_user_admin(self):
            return has_permission(self.request, 'manage')


.. A :term:`layout instance` will be available in templates as the
.. renderer global, ``layout``. For example, if you are using Mako or ZPT
.. for templating, you can put something like this in a template:

:term:`Layout インスタンス <layout instance>` はレンダラーグローバル変数
``layout`` としてテンプレートの中で利用可能です。例えば、テンプレートとして
Mako または ZPT を使用していれば、テンプレートの中でこのように書くことが
できます:


::

    <title>${layout.page_title}</title>


.. For Jinja2:

Jinja2 では:


::

    <title>{{layout.page_title}}</title>


.. All :term:`layouts <layout>` must have an associated template which is the
.. :term:`main template` for the layout and will be present as ``main_template``
.. in renderer globals.

すべての :term:`Layout <layout>` には、その Layout の :term:`main template` である
関連付けられたテンプレートがあり、それはレンダラーグローバル変数の中に
``main_template`` として存在します。


.. To register a layout, use the :meth:`add_layout
.. <pyramid_layout.config.add_layout>` method of the configurator:

layout を登録するには、 configurator の :meth:`add_layout
<pyramid_layout.config.add_layout>` メソッドを使用してください:


::

    config.add_layout('myproject.layout.MyLayout', 
                      'myproject.layout:templates/default_layout.pt')


.. The above registered layout will be the default layout.  Layouts can also be 
.. named:

上記の登録された Layout は、デフォルト Layout になるでしょう。
Layout は名前を付けることもできます:


::

    config.add_layout('myproject.layout.MyLayout', 
                      'myproject.layout:templates/admin_layout.pt',
                      name='admin')


.. Now that you have a layout, time to use it on a particular view. Layouts can
.. be defined declaratively, next to your renderer, in the view configuration:

これで Layout を作ったので、今度は特定のビューでそれを使用する番です。
Layout は、ビュー設定でレンダラーと並べて宣言的に定義できます:


::

    @view_config(..., layout='admin')
    def myview(context, request):
        ...


.. In Pyramid < 1.4, to use a named layout, call
.. :meth:`LayoutManager.use_layout
.. <pyramid_layout.layout.LayoutManager.use_layout>` method in your view:

Pyramid < 1.4 では、名前付き Layout を使用するために、ビューの中で
:meth:`LayoutManager.use_layout
<pyramid_layout.layout.LayoutManager.use_layout>` メソッドを呼んでください:


::

    def myview(context, request):
        request.layout_manager.use_layout('admin')
        ...


.. If you are using :pyramid:term:`traversal` you may find that in most cases it
.. is unnecessary to name your layouts.  Use of the `context` argument to the
.. layout configuration can allow you to use a particular layout whenever the
.. :pyramid:term:`context` is of a particular type:

:pyramid:term:`traversal` を使用していれば、ほとんどの場合 Layout に
名前を付ける必要がないことに気づくかもしれません。 Layout 設定に
`context` 引数を使用することで、 :pyramid:term:`context` が特定の型で
ある場合は常に特定の Layout を使うようにできます:


::

    from ..models.wiki import WikiPage

    config.add_layout('myproject.layout.MyLayout', 
                      'myproject.layout:templates/wiki_layout.pt',
                      context=WikiPage)


.. Similarly, the `containment` argument allows you to use a particular layout for
.. an entire branch of your :pyramid:term:`resource tree`:

同様に、 `containment` 引数は、特定の Layout を :pyramid:term:`resource
tree` の全てのブランチで使用できるようにします:


::

    from ..models.admin import AdminFolder

    config.add_layout('myproject.layout.MyLayout', 
                      'myproject.layout:templates/admin_layout.pt',
                      containment=AdminFolder)


.. The decorator :func:`layout_config <pyramid_layout.layout.layout_config>` can
.. be used in conjuction with :meth:`Configurator.scan
.. <pyramid:pyramid.config.Configurator.scan>` to register layouts declaratively:

デコレータ :func:`layout_config <pyramid_layout.layout.layout_config>` は、
Layout を宣言的に登録するために :meth:`Configurator.scan
<pyramid:pyramid.config.Configurator.scan>` と組み合わせて使えます:


::

    from pyramid_layout.layout import layout_config

    @layout_config(template='templates/default_layout.pt')
    @layout_config(name='admin', template='templates/admin_layout.pt')
    class MyLayout(object):
        ...


.. Layouts can also be registered for specific context types and
.. containments. See the :ref:`api docs <apidocs>` for more info.

Layout を特定のコンテキスト型や containment に対して登録することも
できます。詳しくは :ref:`api docs <apidocs>` を参照してください。


.. Using Panels

Panel を使う
------------

.. A :term:`panel` is similar to a view but is responsible for rendering only a
.. part of a page.  A panel is a callable which can accept arbitrary arguments
.. (the first two are always ``context`` and ``request``) and either returns an
.. html string or uses a Pyramid renderer to render the html to insert in the
.. page.

:term:`Panel <panel>` はビューに似ていますが、ページの一部だけをレンダリング
することに責任を持ちます。 Panel は任意の引数 (最初の2つは常に
``context`` と ``request`` です) を受け取る callable で、
ページに挿入するために html 文字列を返すか、または Pyramid レンダラーを
使用して html をレンダリングします。


.. note::

    .. You can mix-and-match template languages in a project. Some panels
    .. can be implemented in Jinja2, some in Mako, some in ZPT. All can
    .. work in layouts implemented in any template language supported by
    .. Pyramid Layout.

    プロジェクトの中でテンプレート言語を混在して使うことができます。
    ある Panel は Jinja2 で実装し、ある Panel は Mako で、ある Panel は
    ZPT で、ということができます。
    Pyramid Layout がサポートする任意のテンプレート言語で実装された
    Layout は、すべて動作します。


.. A :term:`panel` can be configured using the method, ``add_panel`` of the 
.. ``Configurator`` instance:

:term:`Panel <panel>` は ``Configurator`` インスタンスの ``add_panel`` メソッド
を使用して設定することができます:


::

    config.add_panel('myproject.layout.siblings_panel', 'siblings',
                     renderer='myproject.layout:templates/siblings.pt')


.. Because :term:`panels <panel>` can be called with arguments, they can be
.. parameterized when used in different ways. The panel callable might look
.. something like:

:term:`Panel <panel>` を引数付きで呼ぶことができるので、それらは
異なる方法で使用された時にパラメータ化することができます。
Panel callable は次のようなものです:


::

    def siblings_panel(context, request, n_siblings=5):
        return [sibling for sibling in context.__parent__.values()
                if sibling is not context][:n_siblings]


.. And could be called from a template like this:

そして、テンプレートからこのように呼ぶことができます:


::

    ${panel('siblings', 8)}  <!-- Show 8 siblings -->


.. If using :meth:`Configurator.scan <pyramid:pyramid.config.Configurator.scan>`,
.. you can also register the panel declaratively:

:meth:`Configurator.scan <pyramid:pyramid.config.Configurator.scan>` を
使用する場合、 Panel を宣言的に登録することもできます:


::

    from pyramid_layout.panel import panel_config

    @panel_config('siblings', renderer='templates/siblings.pt')
    def siblings_panel(context, request, n_siblings=5):
        return [sibling for sibling in context.__parent__.values()
                if sibling is not context][:n_siblings]


.. :term:`Panels <panel>` can be registered to match only specific context types.
.. See the :ref:`api docs <apidocs>` for more info.

:term:`Panel <panel>` は特定のコンテキスト型とだけマッチするように登録すること
ができます。詳細については :ref:`api docs <apidocs>` を参照してください。


.. Using the Main Template

メインテンプレートを使う
------------------------

.. The precise syntax for hooking into the :term:`main template` from a view 
.. template varies depending on the templating language you're using.

ビューテンプレートから :term:`main template` に繋げるための正確な文法は、
使用しているテンプレート言語に依存して変わります。


ZPT
~~~

.. If you are a ZPT user, connecting your view template to the :term:`layout` and
.. its :term:`main template` is pretty easy. Just make this the outermost element
.. in your view template:

あなたが ZPT ユーザなら、ビューテンプレートを :term:`Layout <layout>` と
その :term:`main template` に接続することはかなり容易です。ビューテンプレート
中で最も外側の要素を単にこのようにしてください:


.. code-block:: xml

  <metal:block use-macro="main_template">
  ...
  </metal:block>


.. You'll note that we're taking advantage of a feature in Chameleon that allows
.. us to `use a template instance as a macro
.. <http://chameleon.repoze.org/docs/latest/reference.html#id46>`_ without having
.. to explicitly define a macro in the :term:`main template`.

:term:`main template` に明示的にマクロを定義する必要なしに
`テンプレートインスタンスをマクロとして使用
<http://chameleon.repoze.org/docs/latest/reference.html#id46>`_ することが
できるという Chameleon の特徴を利用していることに気づくでしょう。


.. After that, it's about what you'd expect. The :term:`main template` has to
.. define at least one slot. The view template has to fill at least one slot.

その後は、あなたの予想した通りです。 :term:`main template` は少なくとも
1つのスロットを定義しなければなりません。ビューテンプレートは少なくとも
1つのスロットを満たさなければなりません。


Mako
~~~~

.. In Mako, to use the :term:`main template` from your :term:`layout`, use this as
.. the first line in your view template:

Mako で :term:`Layout <layout>` から :term:`main template` を使用するには、
ビューテンプレートの中の最初の行としてこれを使用してください:


.. code-block:: xml

  <%inherit file="${context['main_template'].uri}"/>


.. In your :term:`main template`, insert this line at the point where you'd like
.. for the view template to be inserted:

:term:`main template` では、ビューテンプレートが挿入してほしいポイント
にこの行を挿入してください:


.. code-block:: xml

  ${next.body()}


Jinja2
~~~~~~

.. For Jinja2, to use the :term:`main template` for your :term:`layout`, use this
.. as the first line in your view template:

Jinja2 については、 :term:`main template` を :term:`Layout <layout>` に対して
使用するために、ビューテンプレートの中の最初の行としてこれを使用してください:


.. code-block:: xml

  {% extends main_template %}


.. From there, blocks defined in your :term:`main template` can be overridden by
.. blocks defined in your view template, per normal usage.

それにより、通常の使い方と同様に :term:`main template` で定義されたブロックを
ビューテンプレートで定義されたブロックによってオーバーライドすることができます。

