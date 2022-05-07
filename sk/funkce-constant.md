Funkcia PHP constant()
======================

> id: cfd5a942-e9ab-45a7-9727-9ec5049b69d4
> slug:
> 	cs: funkce-constant
> 	sk: funkcia-php-constant
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '068b23f308df6afd99cfa773c22b56a7'

Dostupnosť vo verziách: `PHP 4.0.4`

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `string` | *not* | Názov konštanty. |


Vrátené hodnoty
----------------


- int <p>
Metóda <b>filter()</b> musí vrátiť jednu z týchto možností
tri hodnoty po dokončení.
</p><table>

<thead>
<tr>
<th>Vrátená hodnota</th>
<th>Význam</th>
</tr>

</thead>

<tbody class="tbody">
<tr>
<td><b>PSFS_PASS_ON</b></td>
<td>
Filter úspešne spracovaný s údajmi dostupnými v
<code class="parameter">out</code> <em>vedľajšia brigáda</em>.
</td>
</tr>

<tr>
<td><b>PSFS_FEED_ME</b></td>
<td>
Filter bol úspešne spracovaný, ale neboli k dispozícii žiadne údaje.
návrat. Je potrebné získať viac údajov z prúdu alebo z predchádzajúceho filtra.
</td>
</tr>

<tr>
<td><b>PSFS_ERR_FATAL</b> (predvolené)</td>
<td>
Vo filtri došlo k neopraviteľnej chybe a nie je možné pokračovať.
</td>
</tr>

/
    public function filter($in, $out, &$consumed, $closing)
    {
    }

    /**
- bool
/
    verejná funkcia onCreate()
    {
    }

    /**
- reťazec
- zmiešaná hodnota konštanty alebo &null; ak konštanta nie je
definované.

Ďalšie zdroje
------------


- https://www.php.net/manual/en/php-user-filter.filter.php
- https://www.php.net/manual/en/php-user-filter.oncreate.php
- https://www.php.net/manual/en/php-user-filter.onclose.php
/
    verejná funkcia onClose()
    {
    }

}

/**
Inštancie Directory sa vytvárajú volaním funkcie dir(), nie operátorom new.
/
trieda Directory {

    /**
- https://www.php.net/manual/en/directory.close.php
/
    verejná funkcia close ( $dir_handle ) {}

    /**
Rukoväť adresára prevíjania.
Rovnaké ako rewinddir(), len dir_handle je predvolene $this.
- https://www.php.net/manual/en/directory.rewind.php
/
    verejná funkcia rewind ( $dir_handle ) {}

    /**
Čítanie záznamu z adresára handle.
Rovnaké ako readdir(), len dir_handle je predvolene $this.
- https://www.php.net/manual/en/directory.read.php
/
    public function read ( $dir_handle) { }

}

/**
Vracia hodnotu konštanty
