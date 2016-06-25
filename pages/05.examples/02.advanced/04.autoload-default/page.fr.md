---
title: Default Autoloader
menu: Default Autoloader
template: jaxon
cache_enable: false
description: This example shows how to optimize Jaxon requests processing with autoloading.
---

In this example, the Jaxon classes are not registered when processing a request.
However, the Jaxon library is smart enough to detect that the required class is missing, and to load only the necessary file.

<div class="row">
    <div class="col-sm-12">
        <h5>Comment ça marche</h5>

<p>The Jaxon class in the file ./classes/namespace/app/Test/Test.php</p>
<pre><code class="language-php">
namespace App\Test;

use Jaxon\Response\Response;

class Test
{
    public function sayHello($isCaps)
    {
        if ($isCaps)
            $text = 'HELLO WORLD!';
        else
            $text = 'Hello World!';
        $xResponse = new Response();
        $xResponse->assign('div1', 'innerHTML', $text);
        $xResponse->toastr->success("div1 text is now $text");
        return $xResponse;
    }

    public function setColor($sColor)
    {
        $xResponse = new Response();
        $xResponse->assign('div1', 'style.color', $sColor);
        $xResponse->toastr->success("div1 color is now $sColor");
        return $xResponse;
    }

    public function showDialog()
    {
        $xResponse = new Response();
        $buttons = array(array('title' => 'Close', 'class' => 'btn', 'click' => 'close'));
        $options = array('maxWidth' => 400);
        $xResponse->pgw->modal("Modal Dialog", "This modal dialog is powered by PgwModal!!", $buttons, $options);
        return $xResponse;
    }
}
</code></pre>

<p>The Jaxon class in the file ./classes/namespace/ext/Test/Test.php</p>
<pre><code class="language-php">
namespace Ext\Test;

use Jaxon\Response\Response;

class Test
{
    public function sayHello($isCaps)
    {
        if ($isCaps)
            $text = 'HELLO WORLD!';
        else
            $text = 'Hello World!';
        $xResponse = new Response();
        $xResponse->assign('div2', 'innerHTML', $text);
        $xResponse->toastr->success("div2 text is now $text");
        return $xResponse;
    }

    public function setColor($sColor)
    {
        $xResponse = new Response();
        $xResponse->assign('div2', 'style.color', $sColor);
        $xResponse->toastr->success("div2 color is now $sColor");
        return $xResponse;
    }

    public function showDialog()
    {
        $xResponse = new Response();
        $buttons = array(array('title' => 'Close', 'class' => 'btn', 'click' => 'close'));
        $width = 300;
        $xResponse->bootstrap->modal("Modal Dialog", "This modal dialog is powered by Twitter Bootstrap!!", $buttons, $width);
        return $xResponse;
    }
}
</code></pre>

<p>The javascript event bindings</p>
<pre><code class="language-php">
// Select
&lt;select id="colorselect" onchange="App.Test.Test.setColor(jaxon.$('colorselect').value); return false;"&gt;&lt;/select&gt;

// Buttons
&lt;button onclick="App.Test.Test.sayHello(0); return false;"&gt;Click Me&lt;/button&gt;
&lt;button onclick="App.Test.Test.sayHello(1); return false;"&gt;CLICK ME&lt;/button&gt;

// Select
&lt;select id="colorselect" onchange="Ext.Test.Test.setColor(jaxon.$('colorselect').value); return false;"&gt;&lt;/select&gt;

// Buttons
&lt;button onclick="Ext.Test.Test.sayHello(0); return false;"&gt;Click Me&lt;/button&gt;
&lt;button onclick="Ext.Test.Test.sayHello(1); return false;"&gt;CLICK ME&lt;/button&gt;

&lt;button onclick="App.Test.Test.showDialog(); return false;"&gt;Show PgwModal Dialog&lt;/button&gt;
&lt;button onclick="Ext.Test.Test.showDialog(); return false;"&gt;Show Twitter Bootstrap Dialog&lt;/button&gt;
</code></pre>

<p>The PHP object registrations</p>
<pre><code class="language-php">
$jaxon = Jaxon::getInstance();

$jaxon->setOption('core.debug.on', false);
$jaxon->setOption('core.prefix.class', '');

// Add class dirs with namespaces
$jaxon->addClassDir(__DIR__ . '/classes/namespace/app', 'App');
$jaxon->addClassDir(__DIR__ . '/classes/namespace/ext', 'Ext');

// Check if there is a request.
if($jaxon->canProcessRequest())
{
    // When processing a request, the required class will be autoloaded
    $jaxon->processRequest();
}
else
{
    // The Jaxon objects are registered only when the page is loaded
    $jaxon->registerClasses();
}
</code></pre>
    </div>
</div>
