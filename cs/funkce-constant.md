PHP funkce constant()
=====================

> id: cfd5a942-e9ab-45a7-9727-9ec5049b69d4
> slug:
> 	cs: funkce-constant
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.4`

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `string` | *není* | The constant name. |


Návratové hodnoty
----------------


- int <p>
The <b>filter()</b> method must return one of
three values upon completion.
</p><table>

<thead>
<tr>
<th>Return Value</th>
<th>Meaning</th>
</tr>

</thead>

<tbody class="tbody">
<tr>
<td><b>PSFS_PASS_ON</b></td>
<td>
Filter processed successfully with data available in the
<code class="parameter">out</code> <em>bucket brigade</em>.
</td>
</tr>

<tr>
<td><b>PSFS_FEED_ME</b></td>
<td>
Filter processed successfully, however no data was available to
return. More data is required from the stream or prior filter.
</td>
</tr>

<tr>
<td><b>PSFS_ERR_FATAL</b> (default)</td>
<td>
The filter experienced an unrecoverable error and cannot continue.
</td>
</tr>

/
    public function filter($in, $out, &$consumed, $closing)
    {
    }

    /**
- bool
/
    public function onCreate()
    {
    }

    /**
- string
- mixed the value of the constant, or &null; if the constant is not
defined.

Další zdroje
------------


- https://www.php.net/manual/en/php-user-filter.filter.php
- https://www.php.net/manual/en/php-user-filter.oncreate.php
- https://www.php.net/manual/en/php-user-filter.onclose.php
/
    public function onClose()
    {
    }

}

/**
Instances of Directory are created by calling the dir() function, not by the new operator.
/
class Directory  {

    /**
- https://www.php.net/manual/en/directory.close.php
/
    public function close ( $dir_handle ) {}

    /**
Rewind directory handle.
Same as rewinddir(), only dir_handle defaults to $this.
- https://www.php.net/manual/en/directory.rewind.php
/
    public function rewind ( $dir_handle ) {}

    /**
Read entry from directory handle.
Same as readdir(), only dir_handle defaults to $this.
- https://www.php.net/manual/en/directory.read.php
/
    public function read ( $dir_handle) { }

}

/**
Returns the value of a constant
