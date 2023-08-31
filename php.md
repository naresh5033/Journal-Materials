# php

- is a general purpose geared towards web development,  and its a oop.
- can be written alongside with the html. and its a interpreted Server side language.
- php is best choice for free lancers, since its easy to set up, use, deploy and create projects quickly.
- is a server side language, need webserver (apache) to run the code.
- <?php echo "hello"; ?> and the short hand for this is <?= "hello"; ?>

- print_r("hello"); - for array and vals
- var_dump("hello"); - types and length.
- var_export("hello"); - o/p string - 'hello'
- vars must be prefixed with $name= "jon"; 
- for the str concat its . period instead of + in js ex $name .'is' . $age; lly we can use double quotes or string literals just like js
- for the constant - define(NAME, 'jon'); echo NAME;
- array is just as normal as the js.. there is also associate array we can make ex - $hex = ['red' => 'apple']; kvp..
- json_encode() - to convert the array(associative) in to obj.. lly json_decode();
- it does have switch statements , while loop, do while. foreach($items as $item){}..
  - if we have associated array then foreach($items as $key => $value){}
- to use a global var use global kw - global $x;
- when defining the function function add(x, y){return x + y;}
- we can also write a arrow fn ex $add = fn(x, y) =>  x + y;
- some array methods are array_push(), array_unshift(), array_pop(), array_shift(), unset($arr[2])..lly set() for add particular idx, to split array_chunk($arr, 2) 
  - array_merge(), array_combine(); // merge 2 arr and makes kvp, array_keys($arr3) and array_flipped()..will flip the k & v.
  - array_map().. array_filter($arr, fn($arr) => {$arr<= 10}); // array_reduce($arr, fn($carry, $arr) => $carry + $arr);

- **string methods** - ex htmlspecialchars($string2); 
- it hsa printf(%s, 'name'); lly there are other specifiers as well ex %f, 
- **super globals** are available in all scopes. ex $_GET, $_POST, $_ENV, lly cookies, files, session, request,

- **sanitizing i/ps** the form i/ps ex filter_sanitize_special_chars(), filter_input()... lly there is filter_var() for sanitizing anything.
- **cookie** to set the cookie, setcookie('name', 'jon', time() + 84600 * 30); // and to check the if(isset($_COOKIE['name'])){echo '$_COOKIE['name'];}

### sessions

- to start the session - session_start(); and to reset the session - session_destroy() and redirects to home - header('Location: /');

- **file handling** - is the ability to read and write files and php has builtin fns - ex - fopen(), fread(), fclose(), filesize(), file_exists()
- file uploading via form <form  enctype="multipart/form-data"> // while uploading the file ve to add this enctype w/o this we can't upload the file..
  - .explode('.', $name) // will split the file name with . period.
- **exception** - has try / catch block..

### oop 

- all the oop concept applies here as well. except a wierd syntax for the ctor 
- ex - the constructor is a method that runs when an object is created. ex to create a constructor - public function __constructor(){}
- ve path separator ex - parent::__constructor($name); this-> name = $name;

 **include and required** if we wanna add any layout ex - add the header to html file we can use the include or require kw.