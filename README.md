# DBTools
## This is free and unencumbered software released into the public domain.
### Secondary License: 0-BSD (if Unlicense not accepted)
```php
<?php
// include all libs
require_once "inc/lib/all-libs.php";
// include tables BEFORE NewConnect!
require_once "inc/db/tables/...";
// if user is null (in this case), value "root" will be used (default: root)
DB::NewConnect("dbname", null, "password");
// only after NewConnect you are able to use tables

// what if I don't want to use mysql???
// then use NewDsnConnect
DB::NewDsnConnect("sqlite:dir/mytest.db");
?>
```

## Table Template
```php
<?php
class DBTbl_Test extends DBTBase {
  static $DBIndex = 0, $Name = "test", $Key = "id";
}
?>
```
## Warning
### Some functions may require `SQL::Esc`, or `SQL::BEsc`, or `SQL::TEsc`
```php
DBTbl_Test::Update($aWhere);
// $aWhere needs protection!
DBTbl_Test::Update("id='".SQL::Esc("32")."'", "values are protected");
```
Other examples:
```php
<?php
  // SQLi protect 1 variable!
  $a = "'";
  SQL::Esc($a);
  // HOWEVER, some people may not want $a to be changed!
  // Applies only to SQL::Esc, since it is main function
  $d = SQL::Esc($a, 1); // prevents changing $a's value
  
  // SQLi protect 2 variables!
  $a = "'"; $b = 3;
  SQL::BEsc($a, $b);
  
  // SQLi protect 3 variables!
  $a = "'"; $b = 3; $c = 4;
  SQL::TEsc($a, $b, $c);
?>
```
## Usage of `DBTbl_Test`
### Relevant functions:
```php
<?php
// Upload to blob?
// Note: it is advised that you first INSERT, then use this!
// If you leave blobfield empty, it will assign for you automatically!
DBTbl_Test::UploadBlob($_FILES["filefield"], "", "id=32");
// Or if there's multiple blob fields in table:
DBTbl_Test::UploadBlob($_FILES["filefield"], "blob_field2", "id=32");
// Upload blob string???
DBTbl_Test::UploadBlobString("long data...", "", "id=32");
// Or if there's multiple blob fields in table:
DBTbl_Test::UploadBlobString("long data...", "blob_field2", "id=32");

// Let's say table "test" has columns in this order (must be correct order!)
// field1, field2, field3)
DBTbl_Test::Insert("a", "b", "c");
// If someone would prefer to skip a field, they would put "null"
DBTbl_Test::Insert("a", null, "c");
// Yes, yes, there is a way to make it less complicated, like this:
DBTbl_Test::Insert2(array(
  "field1" => "a",
  "field3" => "c"
));
// Same applies for: Update, Update2, Delete, Delete2
DBTbl_Test::Update("field1='a'", 'd', null, null);
// Update2
DBTbl_Test::Update2(array(
  "field1" => "a",
  "field3" => "c"
), "field1='a'");
// Delete by $Key
DBTbl_Test::Delete(11);
// Delete by "WHERE"
DBTbl_Test::Delete2("field1='a'");
// Delete all data?
DBTbl_Test::DeleteAll();

// Other functions?
// RowCount, HasRows, Fetch, FetchAll, FetchObject, FetchAllObjects, GetByKValue

// Fetch by (eg: id)?
$Data = DBTbl_Test::GetByKValue(32);
if (false !== $Data) echo "It works!"; 

// Fetch all objects
$All = DBTbl_Test::FetchAllObjects();
// FetchAll
$All = DBTbl_Test::FetchAll(PDO::FETCH_ASSOC);
// ...


?>
```
