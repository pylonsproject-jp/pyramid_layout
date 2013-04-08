.. About Layouts, Panels

==========================
Layout と Panel について
==========================

.. If you have a large project with lots of views and templates,
.. you most likely have a lot of repetition. The header is the same,
.. the footer is the same. A lot of CSS/JS is pulled in, etc.

多数のビューやテンプレートを含む大規模なプロジェクトであれば、
おそらく多くの重複が含まれていることでしょう。ヘッダーは同じ、
フッターも同じ、沢山の CSS / JS が読み込まれている、などなど。


.. Lots of template systems have ways to share templating between
.. templates. But how do you get the data into the master template? You
.. can put it in the view and pass it in, but then it is hard to know what
.. parts belong to the view versus the main template. Then there's
.. testing, overriding, cases where you have multiple main templates.

多くのテンプレートシステムには、テンプレート間でテンプレートの一部を共有する
方法があります。しかし、マスターテンプレートにデータを渡すにはどうすれば
いいでしょうか。それをビューの中に置いてテンプレートにデータを渡すことが
できますが、どの部分がビューに属し、どの部分がメインテンプレートに
属するかを知るのは困難です。そして、複数のメインテンプレートを持って
いる場合におけるテストやオーバーライドのケースがあります。


.. Wouldn't it be nice to have a formal concept called "Layout" that
.. gained many of the benefits of Pyramid machinery like views?

ビューのような Pyramid 機構の利点の多くを持つ "Layout" と呼ばれる
形式概念を持つのは良いことだと思いませんか?


.. This section introduces the concepts of :term:`layout` and "panel".

このセクションは、 :term:`Layout <layout>` と "Panel" の概念について紹介します。


.. About Layouts

Layout とは
==============

.. Most projects have a global look-and-feel and each view plugs into it.
.. In ZPT-based systems, this is usually done with a main template that
.. uses METAL to wrap each view's template.

ほとんどのプロジェクトはグローバルなルックアンドフィールを持ち、
それぞれのビューはそれに差し込まれます。 ZPT ベースのシステムでは、
これは通常それぞれのビューのテンプレートをラップするための METAL を使用する
メインテンプレートによって行われます。


.. Having the template, though, isn't enough. The template usually has
.. logic in it and needs data. Usually each view had to pass in
.. that data. Later, Pyramid's
.. :ref:`renderer globals <pyramid:beforerender_event>`
.. provided an elegant
.. facility for always having certain data available in all renderings.

しかし、テンプレートがあるだけでは十分ではありません。テンプレートは、
通常その中にロジックを持っており、データを必要とします。通常、それぞれの
ビューがデータを渡さなければならないでしょう。後に Pyramid の
:ref:`レンダラーグローバル変数 <pyramid:beforerender_event>` は、
あるデータをすべてのレンダリングにおいて常に利用可能にするための
エレガントな仕組みを提供しました。


.. In Pyramid Layout, these ideas are brought together and given a name:
.. :term:`layout`. A layout is a combination of templating and logic to which a
.. view template can point. With Pyramid Layout, :term:`layout` becomes a
.. first-class citizen with helper config machinery and defined plug points.

Pyramid Layout では、これらのアイデアは一緒にされて名前が与えられます:
:term:`Layout <layout>` です。 Layout は、ビューテンプレートが参照することの
できるテンプレートとロジックの組み合わせです。 Pyramid Layout によって、
:term:`Layout <layout>` は、ヘルパーの config 機構と定義されたプラグポイントを
持つ、一級市民になりました。


.. In more complex projects, different parts of the same site need different
.. layouts. Pyramid Layout provides a way for managing the use of different
.. layouts in different places in your application.

より複雑なプロジェクトでは、同じサイトの異なる部分は異なるレイアウトを
必要とします。 Pyramid Layout は、アプリケーションの異なる場所での
異なるレイアウトの使用を管理するための方法を提供します。


.. About Panels

Panel とは
============

.. In your project you might have a number of layouts and certainly many
.. view templates. Reuse is probably needed for little boxes on the
.. screen. Or, if you are using someone else's layout, you might want to
.. change one small part without forking the entire template.

あなたのプロジェクトには多くのレイアウトがあり、そして多数のビュー
テンプレートがあるかもしれません。おそらく画面上の小さなボックスに対して
再利用が必要とされるでしょう。あるいは、誰か他の人のレイアウトを使用
しているなら、すべてのテンプレートをフォークせずに小さな一部分だけを
変更したいと思うかもしれません。


.. In ZPT, macros provide this functionality. That is, re-usable snippets of
.. templating with a marginal amount of overidability. Like main templates,
.. though, they also have logic and data that need to be schlepped into the
.. template.

ZPT ではマクロがこの機能を提供します。すなわち、最低限のオーバーライド
可能性を持つ再利用可能なテンプレートのスニペットです。しかし、マクロも
メインテンプレートと同じくロジックを持ち、データがテンプレートに渡される
必要があります。


.. Pyramid Layout addresses these re-usable snippets with panels. A :term:`panel`
.. is a box on the screen driven by templating and logic. You make panels,
.. register them, and you can then use them in your view templates or :term:`main
.. templates <main template>`.

Pyramid Layout は、 Panel によってこのような再利用可能なスニペットに
取り組みます。 :term:`Panel <panel>` はテンプレートとロジックによって駆動される
画面上のボックスです。 Panel を作って登録しておけば、ビューテンプレートや
:term:`main template` の中でそれらを使うことができます。


.. Moreover, making and using them is a very Pythonic, Pyramid-like process. For
.. example, you call your :term:`panel` as a normal Python callable and can pass
.. it arguments.  Registration of panels, like :term:`layouts <layout>`, is very
.. similar to registration of views in Pyramid.

さらに、それらを作ったり使用したりすることは、とても Pythonic で
Pyramid 流のプロセスです。例えば :term:`Panel <panel>` を通常の Python callable
として呼び出して、それに引数を渡すことができます。 Panel の登録は、
:term:`Layout <layout>` 同様、 Pyramid におけるビューの登録に非常に似ています。
