Contingency table in PHP
========================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	en: contingency-table-in-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

A contingency table is generally used to show the relationship between two statistical phenomena. When developing a web application, we will often need to visualize the relationship of a certain phenomenon in the database to a time sequence, typically in the administration.

For example, we have a table of orders that shows individual products and we are interested in how the sales of certain high-volume products are related to time.

A table like the following would be useful for this:

| Date | Apples | Strawberries | Pears |
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

There is no easy way to prepare the data into this form in PHP, and getting it directly into this form directly in SQL is also not elegant, because we have to take into account that there is a dynamic number of columns.

So we need to be smart when designing the output of this data structure.

Serializing data using keys
----------------------------

When building a table, I often use retrieve all records that meet a given condition directly from the database, for example interval data.

Specifically:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

The query retrieves all columns in the order table (`order`), filtering out all records from the beginning of the ages up to ``2019-05-01``, returning sorted from newest to oldest.

With a simple SQL query, we get the data almost instantly. The second nice feature is that database indexes can be used efficiently in compiling the results. However, because we have the data in a plain array, we must further manually serialize it into a data structure that can be converted into a contig table.

Since a contingency table describes the relationship of two or more factors, it makes sense to use a multidimensional key. However, since some data may not exist for all combinations, it is better to serialize the key to a single string and store the data as a flat array.

The data can be assembled in a single loop pass (the `$selection` variable contains the output from the database):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Date year-month
    foreach ($row->items as $product) { // go through products
        $key = $date . '_' . $product->id;
        if (isset($data[$key]) {
            $data[$key]++; // exists, add another product
        } else {
            $data[$key] = 1; // does not exist, create first product
        }
    }
}
```

If we were exploring a simpler data structure, the inner loop would not be needed to traverse the products. In this case, the entire data build could be solved with a single loop.

With this approach, we get a so-called flat array of values that looks like `key: value`, while storing two-dimensional information.

The output is then, for example (in `May 2019`, a product with ID `10` sold `6` units):

``php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Render data to table - templates
-------------------------------

If we have the data available as a flat array, we can render the whole table very easily. To do this, we just need to know the fields of all the products we are interested in and the fields of all the dates for which we want to plot the table.

```php
$products = [ ... ]; // array of products: id => name
$dates = [ ... ]; // by date: date => label

echo '<table>';
foreach ($products as $productId => $productName) {
    echo '<tr>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</table>';
```

Note that when browsing the data, it looks for a specific occurrence by string key folding. This approach allows us to constrain or expand the rendered table arbitrarily depending on what data we are browsing. If the data does not exist, the ternary operator `??` is evaluated and zero is displayed.

We can build arrays of available products and dates as part of the first cycle that prepares the data. At that point, we will be sure that we are only ever plotting data that actually exists. In this case, it is very important that the output from the SQL database is sorted by creation date, otherwise the rows may be shuffled during the final table render.
