---
layout: post
title: Jekyll - Code Blocks
key: 20160616
tags:
  - Jekyll
  - English
---

## Code Spans

Use `<html>` tags for this.

Here is a literal `` ` `` backtick.
And here is ``  `some`  `` text (note the two spaces so that one is left
in the output!).

<!--more-->

**markdown:**

    Here is a literal `` ` `` backtick.
    And here is ``  `some`  `` text (note the two spaces so that one is left
    in the output!).

## Standard Code Blocks

    Here comes some code

    This text belongs to the same code block.

^
    This one is separate.

**markdown:**

```
    Here comes some code

    This text belongs to the same code block.

^
    This one is separate.
```

---

```
(() => console.log('hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world!'))();
```

**markdown:**

    ```
    (() => console.log('hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world!'))();
    ```

---

```javascript
(() => console.log('hello, world!'))();
```

**markdown:**

    ```javascript
    (() => console.log('hello, world!'))();
    ```

---

```none
(() => console.log('hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world!'))();
```

## Highlighting Code Snippets

{% highlight javascript %}
(() => console.log('hello, world!'))();
{% endhighlight %}

**markdown:**

```
{%- raw -%}
{% highlight javascript %}
(() => console.log('hello, world!'))();
{% endhighlight %}
{% endraw %}
```

### Line Numbers

{% highlight javascript linenos %}
var hello = 'hello';
var world = 'world';
var space = ' ';
(() => console.log(hello + space + world + space + hello + space + world + space + hello + space + world + space + hello + space + world))();
{% endhighlight %}


{% highlight java linenos %}
/**
  * Decor的意思是：装饰，布置。
  * View树的根节点。
  * 事件分发的启点，ViewRootImpl最先调用dispatchPointerEvent（实现在父类View里面）。
  * 事件调用在DecorView里面形成了一个环。（先通过Window交由Activity分发，Activity再调用DecorView中的真正事件分发方法）
  */
public class DecorView extends FrameLayout  {
    private PhoneWindow mWindow;

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        // DecorView直接覆盖ViewGroup的事件分发实现，其实这只是饶了个圈，
        // 正真的事件分发会由Activity回调到superDispatchTouchEvent（ViewGroup的事件分发处理）。
        // 调用Window的WindowCallbackWrapper对象继续分发。
        final Window.Callback cb = mWindow.getCallback();
        return cb != null && !mWindow.isDestroyed() && mFeatureId < 0
                ? cb.dispatchTouchEvent(ev) : super.dispatchTouchEvent(ev);
    }

    public boolean superDispatchTouchEvent(MotionEvent event) {
        // 调用父类ViewGroup进行事件分发处理。
        return super.dispatchTouchEvent(event);
    }
}
{% endhighlight %}

**markdown:**

```
{%- raw -%}
{% highlight javascript linenos %}
var hello = 'hello';
var world = 'world';
var space = ' ';
(() => console.log(hello + space + world + space + hello + space + world + space + hello + space + world + space + hello + space + world))();
{% endhighlight %}
{% endraw %}
```

---

{% highlight javascript %}
(() => console.log('hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world!'))();
{% endhighlight %}

{% highlight none %}
(() => console.log('hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world! hello, world!'))();
{% endhighlight %}

## Fenced Code Blocks

~~~
Here comes some code.
~~~

**markdown:**

    ~~~
    Here comes some code.
    ~~~

---

~~~~~~~~~~~~
~~~~~~~
code with tildes
~~~~~~~~
~~~~~~~~~~~~~~~~~~

**markdown:**

    ~~~~~~~~~~~~
    ~~~~~~~
    code with tildes
    ~~~~~~~~
    ~~~~~~~~~~~~~~~~~~

## Language of Code Blocks

~~~
def what?
  42
end
~~~
{: .language-ruby}

**markdown:**

    ~~~
    def what?
      42
    end
    ~~~
    {: .language-ruby}

---

~~~ ruby
def what?
  42
end
~~~

**markdown:**

    ~~~ ruby
    def what?
      42
    end
    ~~~