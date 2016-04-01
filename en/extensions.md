# Extending OXSEARCH #

OXSEARCH can be extended by additional modules and themes. However, you should never edit existing OXSEARCH files!

### Postprocessing with  Modifiers ###

sometimes the necessity arises for ElasticSearch fields to contain different values from the corresponding database fields or to add completely new fields that are only relevant for particular search requests. This postprocessing is handled by the
`marmOxsearchModifiers`  class. The methods of this class are applied to the individual records between the first aggregation and the transfer to the index. The mapping is achieved by a predefined name scheeme:

    public function [data typ]_[FiELD]($value, $set) {}

 * _data type_ is the type of the record. OXSEARCH supports the following types:
    * _product_ is a (parent) article.
    * _category_ describes a product category.
    * _variant_ is a product variant.
   _data type_ must be written in lower case.
 * _FELD_ is the name of the record field which needs modification. It must be written in upper case.
 * The parameters:
    * `$value` is the current value des Feldes.
    * `$set` is a copy of the record.

OXSEARCH comes with three modifiers:

 * `product_OXPRICE` and `product_OXVARMINPRICE` calculates the gross price into the price fields if the shop operates in net mode.
 * `product_SUGGEST` creates a field for auto suggestions.

For further modifications, extend the class into a new module and implement the modification methods with the same name scheme..

__Note: Modifications can only be applied to existing fields. This also applies to fields that are used by OXSEARCH or any other extension! Simply set the corresponding database field to `TINYINT(1) NULL`.__
