PHP PDO MYSQ CLASS VS MEMCACHE
===============================
<p>Драйвер для роботи з БД MySQL. Клас є розширенням PDO і включає в себе готові функції для роботи з основниви операціями CRUD. Працює по паттерну singleton. Основне завдання: зменшити кількість помилок при рутинних операціях, та зменшити кількість коду.</p>
<p><!--pagebreak--></p>
<h2>Налаштування з'єднання з БД.</h2>
<p>В конструкторі класу є $_config. Його бажано замінити своїм або підставити свої значення.</p>
<h2>Використання.</h2>
<h3>SELECT</h3>
<pre class="prettyprint">$db = DB::instance();<br />$r = $db-&gt;select("select id,name,is_folder from menu order by id asc")-&gt;all();</pre>
<pre>Результат:<br />Array
(
    [0] =&gt; Array
        (
            [id] =&gt; 1
            [name] =&gt; Про проект
            [title] =&gt; Про проект
        )

    [1] =&gt; Array
        (
            [id] =&gt; 2
            [name] =&gt; Історія
            [title] =&gt; Історія
        )

)<br /><br />$r = $db-&gt;select("select name,is_folder from menu where id=1")-&gt;row();<br />Результат:<br />(
    [name] =&gt; Про проект
    [title] =&gt; Про проект
)</pre>
<pre class="prettyprint"><br />$r = $db-&gt;select("select name from menu where id=1")-&gt;row('name');</pre>
<pre>Результат:<br />Про проект</pre>
<p>Також в метода select є другий паратетр $debug. Якщо передати true | 1 тоді перез здійсненням запиту виведеться дамп sql.</p>
<h3>INSERT</h3>
<p>Було:</p>
<pre class="prettyprint">INSERT INTO `menu` (`id`, `name`, `title`) VALUES (NULL, 'Про проект', 'Про проект');</pre>
<p>Стало:</p>
<pre class="prettyprint">$db-&gt;insert(<br />&nbsp;&nbsp;&nbsp; 'menu',<br />&nbsp;&nbsp;&nbsp; array(<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'name' =&gt; 'Про проект',<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'title'=&gt; 'Про проект',<br />&nbsp;&nbsp;&nbsp; )<br />);<br /><br />Імовірність помилки зменшена. І все екранується автоматично<br /><br />UPDATE<br />UPDATE `menu` SET `title` = 'Історія 1' WHERE `menu`.`id` =2;<br />$db-&gt;update('menu', array('title'=&gt; 'Історія 1'), 'id=2');</pre>
<p>При малій кількості полів різницю не видно. Але. Якщо для прикладу в масив $_POST['menu'] внести дані для вставки з форми, тоді можна розширяти форму полями і не хвилюватись про обробник</p>
<pre class="prettyprint">$db-&gt;update('menu', $_POST['menu'], 'id=2');</pre>
<h3>DELETE</h3>
<pre class="prettyprint">DELETE FROM `menu` WHERE `menu`.`id` = 2<br />$db-&gt;delete('menu', 'id=2');</pre>
<pre class="prettyprint">$db-&gt;delete('menu', 'id=2 limit 1');</pre>
<h2>MEMCACHE</h2>
<pre class="prettyprint">$db = DB::instance();<br />$db-&gt;useCache(1, 3600);<br />$r = $db-&gt;select("select id,name,title from menu order by id asc")-&gt;all();<br />або<br />$db = DB::instance()-&gt;useCache(1, 3600);<br />$r = $db-&gt;select("select id,name,title from menu order by id asc")-&gt;all();<br />Тобто ви можете міняти час життя кешу.
</pre>
