#### 1 A. Provide a query to check how many total PO and CER from existing PR
```
SELECT 
    pr_id,
    COUNT(DISTINCT table_po.id) AS total_po,
    COUNT(DISTINCT table_cer.id) AS total_cer
FROM 
    table_pr
LEFT JOIN 
    table_po ON table_pr.id = table_po.pr_id
LEFT JOIN 
    table_cer ON table_pr.id = table_cer.pr_id
GROUP BY 
    pr_id;

```
#### 1 B. Provide a query to check if total items between PO and CER are tally.
```
SELECT 
    pr_id,
    SUM(po_quantity) AS total_po_quantity,
    SUM(cer_quantity) AS total_cer_quantity
FROM (
    SELECT 
        pr_id,
        SUM(CAST(quantity AS INTEGER)) AS po_quantity,
        0 AS cer_quantity
    FROM 
        table_po
    GROUP BY 
        pr_id
    UNION ALL
    SELECT 
        pr_id,
        0 AS po_quantity,
        SUM(CAST(quantity AS INTEGER)) AS cer_quantity
    FROM 
        table_cer
    GROUP BY 
        pr_id
) AS combined
GROUP BY 
    pr_id;

```

#### 1 C. Provide a query to check if item PO and CER are the same. (same sequence and product_id)
```
SELECT 
    po.pr_id,
    po.sequence AS po_sequence,
    po.product_id AS po_product_id,
    cer.sequence AS cer_sequence,
    cer.product_id AS cer_product_id
FROM 
    table_po_line AS po
FULL OUTER JOIN 
    table_cer_line AS cer ON po.pr_id = cer.pr_id AND po.sequence = cer.sequence AND po.product_id = cer.product_id;
```

#### 2.A What will the above code snippet output ?
```
true
false
1
1
undefined
```

#### 2.B Write a simple function (less than 160 characters) that returns a boolean indicating whether or not a string is a palindrome.
```
const isPalindrome = str => str.split('').reverse().join('') === str;
```

#### 2.C List out the different ways an HTML element can be accessed in a JavaScript code.

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Accessing HTML Elements</title>
</head>
<body>
  <div id="myDiv">Hello, World!</div>
  <p class="myClass">This is a paragraph.</p>
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>

  <script>
    const myDiv = document.getElementById('myDiv');
    console.log(myDiv.textContent);
    const elementsWithClass = document.getElementsByClassName('myClass');
    console.log(elementsWithClass[0].textContent);
    const listItems = document.getElementsByTagName('li');
    console.log(listItems[0].textContent);
    const firstListItem = document.querySelector('li');
    console.log(firstListItem.textContent);
    const allListItems = document.querySelectorAll('li');
    allListItems.forEach(item => console.log(item.textContent));
    const parentOfDiv = myDiv.parentNode;
    console.log(parentOfDiv.tagName);

    const nextSiblingOfDiv = myDiv.nextElementSibling;
    console.log(nextSiblingOfDiv.textContent);

    const previousSiblingOfDiv = myDiv.previousElementSibling;
    console.log(previousSiblingOfDiv.textContent);
  </script>
</body>
</html>

```

#### 3. A Write a method which will remove any given character from a String.
```
function removeChar($s, $char) {
    return str_replace($char, '', $s);
}
$result = removeChar("hello", "l");
echo $result; 
```

#### 3. B Write code to check if a String is palindrome or not.
```
function isPalindrome($s) {
    $s = strtolower($s); 
    return $s === strrev($s);
}
$result = isPalindrome("racecar");
echo $result ? "True" : "False"; 
```

#### 3. C Write a function to find the first non repeated character of a given String.
```
function firstNonRepeatedChar($s) {
    $charCount = array_count_values(str_split($s));
    foreach ($charCount as $char => $count) {
        if ($count === 1) {
            return $char;
        }
    }
    return null; 
}
$result = firstNonRepeatedChar("statistics");
echo $result;  
```

#### 3. D Write a function to find the second highest number in an integer array.
```
function secondHighest($arr) {
    if (count($arr) < 2) {
        return null; 
    }
    $maxNum = max($arr[0], $arr[1]);
    $secondMax = min($arr[0], $arr[1]);
    foreach ($arr as $num) {
        if ($num > $maxNum) {
            $secondMax = $maxNum;
            $maxNum = $num;
        } elseif ($num > $secondMax && $num != $maxNum) {
            $secondMax = $num;
        }
    }

    return $secondMax;
}
$result = secondHighest([1, 3, 5, 2, 4]);
echo $result;  
```

#### 4. Inherit Person objects support people with last name or with middle name such as: “James Bond”, “Robert Downey Junior”, “Zendaya Maree Stoermer Coleman”.
```
class Person(object):
    def __init__(self, name):
        self.name = name

    def get_full_name(self):
        return ' '.join(part.capitalize() for part in self.name.split())

test = Person("robert downey junior")
print(test.get_full_name())
```

#### 5. Inherit GoFood object to add convenience fee for Rp.3,000 (fixed amount) if transport fee is lower than Rp.10,000.
```
class GoFood(object):
    def __init__(self):
        self.orders = []

    def add_order(self, product, qty, price):
        self.orders.append({'product': product, 'qty': qty, 'price': price})

    def get_transport_fee(self, location):
        if location == 'pelita':
            return 5000
        return 10000

    def get_total_order(self, location, discount_voucher):
        total_order = sum([x['qty'] * x['price'] for x in self.orders])
        transport_fee = self.get_transport_fee(location)
        
        return total_order + transport_fee - discount_voucher

class GoFoodWithConvenienceFee(GoFood):
    def get_transport_fee(self, location):
        base_fee = super().get_transport_fee(location)
        if base_fee < 10000:
            return base_fee + 3000
        return base_fee

test = GoFoodWithConvenienceFee()
test.add_order('mie ayam', 2, 15000)
test.add_order('teh o beng', 2, 5000)
print(test.get_total_order('pelita', 10000))

```

